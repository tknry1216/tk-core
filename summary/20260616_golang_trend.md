# Go (Golang) トレンドサマリー 2026年6月

## 1. 最新リリース状況

### Go 1.26（2026年2月10日リリース、最新パッチ: 1.26.4）

- **Green Tea GC がデフォルト化**: Go 1.25 で実験的だった新ガベージコレクタが正式にデフォルト有効化。メモリブロック中心のアーキテクチャにより、GC オーバーヘッドが **10〜40% 削減**。tile38 ベンチマークで 35% 削減を達成
- **`new` 関数の拡張**: `new` の引数に式を指定し初期値を設定可能に（従来はゼロ値初期化のみ）
- **自己参照ジェネリクス型**: ジェネリクス型が自身の型パラメータリスト内で自分自身を参照可能に（例: `type Adder[A Adder[A]] interface { Add(A) A }`）。2017年から未解決だった課題が解決
- **cgo オーバーヘッド約30%削減**: プロセッサの「syscall」状態を廃止し、goroutine ステータスの直接チェックに簡素化
- **実験的 SIMD パッケージ**: `GOEXPERIMENT=simd` で `simd/archsimd` パッケージが利用可能。128/256/512-bit ベクトル型対応。GC でも AVX-512 活用で追加 ~10% の性能向上
- **`crypto/hpke` パッケージ**: RFC 9180 準拠のハイブリッド公開鍵暗号（HPKE）を新規追加、ポスト量子ハイブリッド KEM 対応
- **`errors.AsType` 追加**: ジェネリクスを活用したエラー型アサーションで、従来の out-pointer パターンを不要に
- **`go fix` 完全刷新**: Go analysis フレームワーク上で再構築。24以上の「modernizer」アナライザが安全なコードリライトを自動実行。`//go:fix inline` ディレクティブもサポート
- **小オブジェクトアロケーション最大30%高速化**: サイズ特化型アロケーションルーチンを生成
- **ヒープベースアドレスランダム化**: 64-bit プラットフォームでの cgo メモリ悪用に対するハードニング

### Go 1.25（2025年8月12日リリース）

- **コンテナ対応 GOMAXPROCS**: Linux の cgroup CPU 帯域幅制限を自動認識。Kubernetes Pod の CPU リミットを尊重し、動的に適応（~30秒ごとに再チェック）
- **実験的 `encoding/json/v2`**: `GOEXPERIMENT=jsonv2` で有効化。最大 **10倍高速** なアンマーシャリング、多くの構造体でゼロヒープアロケーション。json/v2 ワーキンググループが 2025年11月に設立
- **Green Tea GC（実験的）**: `GOEXPERIMENT=greenteagc` で有効化。Go 1.26 でデフォルト化
- **`testing/synctest` パッケージ安定化**: Go 1.24 での実験的導入から標準ライブラリに昇格。偽クロック付き「バブル」内で決定論的な並行テストが可能に
- **Flight Recorder API**: `runtime/trace.FlightRecorder` による軽量なランタイムトレースキャプチャ
- **DWARF 5 デバッグ情報**: デバッグデータのストレージフットプリント削減、大規模バイナリのリンク時間短縮
- **`go doc -http`**: ローカルドキュメントサーバの起動（オフライン開発対応）
- **`go.mod` に `ignore` ディレクティブ追加**: go コマンドが無視するディレクトリを指定可能に
- **Core Types 概念の廃止**: 言語仕様からジェネリクスの「core types」を除去し、より明確な記述に置き換え

### Go 1.24（2025年2月11日リリース）

- **ジェネリクス型エイリアス**: 型エイリアスが型パラメータを持てるように
- **Swiss Tables ベースの新 map 実装**: マイクロベンチマークで最大 **60% 高速化**。全体で CPU オーバーヘッドが平均 **2〜3%** 削減。Datadog 実績: マップメモリ使用量 **~70% 削減**（726 MiB → 217 MiB）、フリート全体で 200 TiB の RAM 削減
- **Spin Bit Mutex**: 高競合シナリオで **最大 70% 高速化**
- **`crypto/mlkem` パッケージ**: ML-KEM-768/ML-KEM-1024 のポスト量子暗号対応
- **`os.Root` 型**: ファイルシステムアクセスをディレクトリ内に安全に制限
- **`go.mod` の `tool` ディレクティブ**: 実行可能な依存関係をモジュールで追跡可能に（`tools.go` ワークアラウンド不要に）
- **`go:wasmexport` ディレクティブ**: Go 関数を WebAssembly ホストにエクスポート可能に
- **`runtime.AddCleanup`**: `runtime.SetFinalizer` より柔軟で効率的な新ファイナライゼーション機構

Sources: [Go 1.26 Release Notes](https://go.dev/doc/go1.26) | [Go 1.25 Release Notes](https://go.dev/doc/go1.25) | [Go 1.24 Release Notes](https://go.dev/doc/go1.24) | [Green Tea GC Blog](https://go.dev/blog/greenteagc) | [JSON v2 Experimental Blog](https://go.dev/blog/jsonv2-exp) | [Datadog Swiss Tables](https://www.datadoghq.com/blog/engineering/go-swiss-tables/)

---

## 2. 言語機能の進化とプロポーザル

### ジェネリクスの成熟

| バージョン | ジェネリクスのマイルストーン |
|---|---|
| Go 1.18（2022年3月） | ジェネリクス導入（型パラメータ、制約、core types 概念） |
| Go 1.21（2023年8月） | stdlib に `slices`、`maps`、`cmp.Ordered` 追加。型推論の大幅拡張 |
| Go 1.22（2024年2月） | range-over-func イテレータ（実験的） |
| Go 1.23（2024年8月） | **range-over-func 安定化**。`iter` パッケージ追加（`iter.Seq`、`iter.Seq2`） |
| Go 1.24（2025年2月） | **ジェネリクス型エイリアス**安定化 |
| Go 1.25（2025年8月） | 言語仕様から **Core Types 概念を除去**、ジェネリクス仕様を簡素化 |
| Go 1.26（2026年2月） | **自己参照ジェネリクス型**。`errors.AsType` でジェネリクス活用のエラー処理 |
| Go 1.27（2026年8月予定） | **ジェネリックメソッド**（承認済み、実装中） |

### ジェネリックメソッドの承認（2026年3月）

- Go 共同設計者 Robert Griesemer のプロポーザル（[#77273](https://github.com/golang/go/issues/77273)）により、**具象型のメソッドへの型パラメータ**が正式に承認
- Go 1.18 以来 FAQ で明示的に否定されていた立場を覆す歴史的な決定
- 制約: ジェネリックメソッドは**インタフェースの実装には使用不可**（動的ディスパッチの複雑性のため）
- 完全に後方互換性あり

### エラーハンドリング構文 — 公式終了宣言（2025年6月3日）

- Go チームが [go.dev/blog/error-syntax](https://go.dev/blog/error-syntax) で**エラーハンドリング構文の変更を恒久的に停止**すると宣言
- 7年間で3つの主要プロポーザル（check/handle、try()、`?` 演算子）がすべて否決
- Google Cloud Next 2025 で調査したすべての Go ユーザーが構文変更に反対
- **`if err != nil` が Go のエラーハンドリングの標準パターンとして確定**
- 今後のエラーハンドリング関連プロポーザルは調査なしでクローズ

### その他の注目プロポーザル

- **Sum Types / Union Types**: 2026年のランタイム・コンパイラグループ会議で「union type, generic methods, maybe tensor」を探索・検討中。プロポーザル [#76920](https://github.com/golang/go/issues/76920) として活発に議論
- **Range-over-func イテレータ**: Go 1.23 で安定化済み。カスタムコレクション型（順序付きマップ、ツリー等）が `range` キーワードとシームレスに統合可能

Sources: [Go Generics in 2026 (DEV)](https://dev.to/gabrielanhaia/go-generics-in-2026-what-finally-works-and-what-still-doesnt-3i31) | [Generic Methods Approved (The Register)](https://theregister.com/2026/03/02/generic_methods_go) | [Error Handling Syntax Decision](https://go.dev/blog/error-syntax) | [Range Over Function Types](https://go.dev/blog/range-functions)

---

## 3. エコシステム・ツーリングのトレンド

### Web フレームワーク

| フレームワーク | GitHub Stars | 採用率 (2025) | 特徴 |
|---|---|---|---|
| **Gin** | ~88,700 | 48% | 最大エコシステム、成熟、net/http ベース |
| **Fiber** | ~39,800 | 11%（急成長） | Express 風 API、fasthttp ベース、最高速スループット |
| **go-zero** | ~33,100 | — | マイクロサービスフレームワーク、中国テックエコシステムで人気 |
| **Echo** | ~32,400 | 16% | クリーン API、低メモリ、マイクロサービス向け |
| **Beego** | ~32,400 | — | フル MVC、Django/Rails 的な全部入り |
| **Chi** | ~22,400 | 12%（増加中） | 軽量、net/http 完全互換、ミニマリスト向け |
| **GoFr** | ~21,300 | — | マイクロサービス向け新興、オブザーバビリティ内蔵 |
| **Encore.go** | ~12,000 | — | インフラ自動化内蔵、分散システム向け |
| **Hertz** | ~7,300 | — | ByteDance (CloudWeGo) 製、大規模マイクロサービス向け |

**注目の動向**:
- Go 1.22 の **net/http.ServeMux 強化**（メソッドベースルーティング `GET /users/{id}` 追加）がフレームワーク依存を軽減
- **Gorilla/mux の衰退**: 36%（2020）→ 17%（2025）。2022-2023 のプロジェクトアーカイブ化と stdlib 強化が要因
- **Fiber の fasthttp トレードオフ**: 生のスピードは優位だが net/http エコシステム非互換が実運用の制約に

### ORM・データベースライブラリ

| ライブラリ | GitHub Stars | アプローチ |
|---|---|---|
| **GORM** | ~39,800 | Active Record ORM（チェーン API、マイグレーション、フック） |
| **sqlc** | ~17,800 | SQL-first コード生成（SQL を書いて型安全な Go を得る） |
| **Ent** (Meta) | ~17,100 | スキーマ as コード、コンパイル時型安全 |
| **sqlx** | ~16,000 | database/sql の薄いラッパー |
| **Bun** | ~7,000+ | 軽量 SQL-first クエリビルダ |

### 開発ツーリング

- **golangci-lint v2.0**（2025年3月）: 設定体系を刷新。`linters.default` で `"all"` / `"standard"` / `"none"` / `"fast"` を指定。`golangci-lint migrate` で v1 → v2 自動変換
- **gopls v0.20.0**: 永続的パッケージインデックスがデフォルト有効化。`source.splitPackage` コードアクションでパッケージ分割支援。modernize アナライザ群（23+）で最新言語機能への自動リライト
- **`go fix`（Go 1.26 完全刷新）**: 24+ の modernizer アナライザで古いパターンを新しい言語/stdlib 機能に自動更新
- **GoLand 2026.2 EAP**: Go Performance Optimization ツールウィンドウ（プロファイリング、エスケープ分析、構造体最適化）

### 注目のオープンソースプロジェクト（Go 製、GitHub Stars 上位）

| プロジェクト | Stars | 概要 |
|---|---|---|
| **Ollama** | ~174,200 | ローカル LLM 推論ツール。Go 製で 2 番目に多い Star 数 |
| **PocketBase** | ~59,100 | 単一ファイルのリアルタイムバックエンド |
| **Bubble Tea** | ~43,100 | Elm アーキテクチャの TUI フレームワーク |
| **Cobra** | ~44,100 | CLI アプリ構築のデファクトスタンダード |
| **Milvus** | ~44,800 | AI 検索向けベクトルデータベース |
| **LocalAI** | ~46,900 | ローカル AI エンジン |

Sources: [go-web-framework-stars (GitHub)](https://github.com/mingrammer/go-web-framework-stars) | [JetBrains Go Ecosystem 2025](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/) | [JetBrains Popular Go Frameworks 2026](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/) | [golangci-lint v2](https://ldez.github.io/blog/2025/03/23/golangci-lint-v2/) | [GitHub Ranking Top 100 Go](https://github.com/EvanLi/Github-Ranking/blob/master/Top100/Go.md)

---

## 4. パフォーマンス・ランタイム改善

### Green Tea GC（Go 1.26 デフォルト）

- 従来のオブジェクト中心の並行マーク＆スイープから、**メモリブロック中心のアーキテクチャ**に移行
- 小オブジェクト（≤ 512 bytes）を 8 KiB span 単位で処理し、ランダムポインタ追跡を**シーケンシャルスキャン**に変換
- L1/L2 キャッシュミスが**約50%削減**。メモリストールが 35%+ のマーク時間を消費していた問題を大幅改善
- AVX-512 対応（Intel Ice Lake / AMD Zen 4 以降）: span が高密度（12.5%+ オブジェクトがマーク済み）の場合に SIMD スキャンに切り替え
- **注意**: DoltHub のテストでは、GC 負荷が低いワークロードでは効果が限定的。アロケーション多用ワークロードに最も効果大

### パフォーマンス数値まとめ

| バージョン | 改善項目 | 改善幅 |
|---|---|---|
| Go 1.24 | Swiss Tables マップ（マイクロベンチ） | 最大 60% 高速化 |
| Go 1.24 | Swiss Tables マップ（実アプリ） | CPU ~1.5% 削減 |
| Go 1.24 | Datadog マップメモリ | **~70% 削減**（726→217 MiB） |
| Go 1.24 | Spin Bit Mutex（高競合） | 最大 70% 高速化 |
| Go 1.24 | 全体ランタイム CPU | 2-3% 削減（平均） |
| Go 1.25 | コンテナ対応 GOMAXPROCS | カーネルスロットリング解消 |
| Go 1.25/1.26 | Green Tea GC | 10-40% GC オーバーヘッド削減 |
| Go 1.26 | Green Tea + SIMD（最新 amd64） | 追加 ~10% GC 改善 |
| Go 1.26 | 小オブジェクトアロケーション | 最大 ~30% 高速化 |
| Go 1.26 | cgo オーバーヘッド | ~30% 削減 |

### PGO（Profile-Guided Optimization）

- Go 1.22+ で **2〜14%** のパフォーマンス改善（代表的ベンチマーク）
- **Uber 実績**: PGO の継続的最適化フレームワークにより、トップサービス群で **24,000 CPU コア削減**
- **Datadog ガイダンス**: 本番環境で最大 **14% CPU 削減**
- 最適化手法: ホットコールサイトのアグレッシブインライン化、インタフェースメソッド呼び出しの脱仮想化、基本ブロック・関数の並び替え

Sources: [Green Tea GC Blog](https://go.dev/blog/greenteagc) | [GitHub Issue #73581](https://github.com/golang/go/issues/73581) | [Uber PGO Blog](https://www.uber.com/en-CA/blog/automating-efficiency-of-go-programs-with-pgo/) | [Datadog PGO Guide](https://docs.datadoghq.com/profiler/guide/save-cpu-in-production-with-go-pgo/) | [Go Swiss Tables Blog](https://go.dev/blog/swisstable)

---

## 5. コミュニティ・採用動向

### 2025 Go Developer Survey（2025年9月実施、回答者 5,379人）

- **満足度 91%**: Go への総合満足度は引き続き高水準（約2/3が「非常に満足」）
- **AI ツール利用**: 53% が日常的に AI 開発ツールを使用、29% は未使用または稀に使用
- **AI ツール満足度**: 55% が満足（「やや満足」42%、「非常に満足」13%）
  - **課題**: 53% が「非機能的なコード生成」を最大の問題と回答、30% が「品質の低いコード」を指摘
- **使用 AI ツール**: ChatGPT（45%）、GitHub Copilot（31%）、Claude Code（25%）、Claude（23%）、Gemini（20%）
- **クラウドデプロイ先**: AWS（46%）、自社サーバ（44%）、GCP（26%）
- **開発者の要望**: ベストプラクティスの特定・適用、標準ライブラリの活用、ジェネリクス改善、パターンマッチング、ホットリロード、テストカバレッジ詳細

### 開発者数・採用規模

| 指標 | 値 | ソース |
|---|---|---|
| グローバル Go 開発者数 | **5.8M** | ZenRows |
| プライマリ言語ユーザー | 2.2M | JetBrains |
| Go 採用予定（全開発者中） | 11% | JetBrains |
| Go 使用確認企業数 | 9,214 社 | Landbase |
| Stack Overflow YoY 成長 | +2 ポイント（2025） | SO Dev Survey |
| GitHub 成長ランク（2024） | **3位**（Python、TypeScript に次ぐ） | GitHub Octoverse |
| JetBrains Language Promise Index | **4位**（TS、Rust、Python に次ぐ） | JetBrains |
| Go 開発者平均年収（US） | **$159,225/年** | Ruby On Remote |
| 給与プレミアム | 中央値比 **+19%** | 複数ソース |
| リモート対応 Go 求人 | 63% | ByteSizeGo |

### TIOBE Index

- 2025年4月: **過去最高の #7** に到達
- 2026年1月: #16 に下落（「Go は安定しすぎて退屈になった」との分析）
- 2026年6月: **#8 前後**に回復
- TIOBE の検索エンジンベース方法論は Go のクラウドネイティブ領域での実際の産業利用を反映しきれていないとの指摘あり

### 主要採用企業

Google、Uber（コアマイクロサービス）、ByteDance（マイクロサービスの **70% が Go**）、Netflix、Cloudflare、Docker、Dropbox、HashiCorp、American Express、PayPal、CrowdStrike、Shopify、Datadog、Apple、Salesforce

### カンファレンス（2026年）

- **GopherCon Europe 2026**: 6月15-18日、ベルリン
- **GopherCon US 2026**: 8月3-6日、シアトル
- **GoLab 2026**: 11月1-3日、ボローニャ

Sources: [2025 Go Developer Survey](https://go.dev/blog/survey2025) | [JetBrains Go Ecosystem 2025](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/) | [Stack Overflow Developer Survey 2025](https://survey.stackoverflow.co/2025) | [GitHub Octoverse 2024](https://github.blog/news-insights/octoverse/octoverse-2024/) | [GopherCon 2026](https://www.gophercon.com/)

---

## 6. AI/ML・LLM 統合

### LLM クライアントライブラリ

| ライブラリ | 説明 |
|---|---|
| **anthropic-sdk-go** (v1.50.2) | Anthropic 公式 Go SDK。Messages API、ストリーミング、ツール使用、Bedrock/Vertex 統合 |
| **openai-go** | OpenAI 公式 Go ライブラリ |
| **sashabaranov/go-openai** (v1.41.2) | コミュニティ製 OpenAI クライアント。最も広く使用 |
| **Google GenAI Go SDK** | Gemini モデル公式 SDK |
| **Ollama Go SDK** | ローカル LLM 推論。366+ プロジェクトがインポート |

### マルチプロバイダフレームワーク

- **LangChainGo** (9.4K stars): Python LangChain の Go 移植。チェーン、エージェント、ツール、エンベディング、ベクトルストア対応
- **Firebase GenKit for Go** (2026年5月公開): Google のオープンソース LLM アプリ構築フレームワーク。テキスト生成、構造化出力、ツールコーリング、エージェンティックワークフロー
- **any-llm-go（Mozilla AI）**: OpenAI / Claude / Mistral / Llamafile を単一インタフェースで利用。Go チャネルでストリーミング応答
- **GoLLM / Go-LLMs**: 軽量マルチプロバイダ LLM ラッパー

### AI エージェントフレームワーク（2026年最速成長カテゴリ）

- **Eino** (CloudWeGo/ByteDance, 11.8K stars): Go 最大の LLM アプリフレームワーク。LangChain/Google ADK にインスパイア。マルチエージェント協調、human-in-the-loop 対応
- **Google ADK / Genkit**: 階層的マルチエージェントシステム構築。Agent2Agent (A2A) プロトコル対応
- **AixGo**: プロダクショングレードの AI エージェントフレームワーク
- **MCP Go SDK** (公式、2026年5月22日公開): Model Context Protocol の公式 Go SDK。stdio、SSE、WebSocket、gRPC トランスポート対応。Google との協力で維持

### ML ライブラリ

- **GoMLX** (v0.27.3): Go の PyTorch/JAX 相当。自動微分、ML レイヤーライブラリ、XLA/CoreML/Pure Go/WASM バックエンド。HuggingFace 統合
- **Hugot**: HuggingFace モデルの Go 統合（GoMLX と連携）
- **Gorgonia / Gonum**: 数値計算・科学計算

### ベクトルデータベース（Go 製）

- **Milvus** (~44.8K stars): AI 検索向けベクトル DB
- **chromem-go**: 組み込み型ベクトル DB。1,000 ドキュメントを 0.3ms でクエリ
- **Weaviate**: RAG パイプラインで人気

### Go の AI/ML ポジション

- Python が AI/ML 開発を支配する中、Go は**推論・デプロイ・スケーラブルなインフラ基盤**として差別化
- 2026年は Go の「AI エージェントの年」: ネイティブ並行性、強い型付け、REST/RPC サポートがプロダクショングレードの AI エージェントシステムに最適
- **CGO を WASM で代替する動き**: wazero を使って CGO 依存を WebAssembly モジュールに置き換え、クロスコンパイルとデプロイのポータビリティを向上

Sources: [anthropic-sdk-go (GitHub)](https://github.com/anthropics/anthropic-sdk-go) | [AI and Go in 2026 (Applied Go)](https://appliedgo.net/spotlight/ai-and-go/) | [Eino (GitHub)](https://github.com/cloudwego/eino) | [MCP Go SDK (GitHub)](https://github.com/modelcontextprotocol/go-sdk) | [GoMLX (GitHub)](https://github.com/gomlx/gomlx) | [Top 7 Golang AI Agent Frameworks (Relia)](https://reliasoftware.com/blog/golang-ai-agent-frameworks)

---

## 7. WebAssembly・クラウドネイティブ

### WebAssembly

| Go バージョン | WASM マイルストーン |
|---|---|
| Go 1.11 | 初期 Wasm サポート（`js/wasm`） |
| Go 1.21 | **WASI Preview 1** サポート（`GOOS=wasip1 GOARCH=wasm`） |
| Go 1.24 | **`go:wasmexport`** ディレクティブ + WASI リアクタビルド |

- **TinyGo 0.41**（2026年4月）: 150+ コミットの大型リリース。リフレクションサポート大幅改善、WASI P1/P2 両対応、Go 1.26 サポート
- **WASI Preview 2** が 2025 年に安定化、2026 年の本番 Wasm デプロイメントの基盤に
- **WASI P3**: ネイティブ非同期サポート。Spin v3.5 で初の RC（2025年11月）
- **wazero**: 純粋 Go 製のゼロ依存 WebAssembly ランタイム。インタプリタ・コンパイラ両モード対応
- **Wasm コールドスタート**: **1〜5ミリ秒**（従来コンテナの約 100 倍高速）。エッジ/サーバレスに最適
- **GoMLX + WASM**: Pure Go バックエンドでブラウザ内 ML モデル実行が可能

### クラウドネイティブ

- Go は **クラウドネイティブ開発の事実上の標準言語**を維持
- **Kubernetes v1.36** が 2026 年時点で開発中
- **Gateway API**: Envoy Gateway、Istio、Cilium、Kong の実装でトラフィック管理の標準に
- **Istio**: 2025年 CNCF Graduated。ambient メッシュモード、Gateway API Inference Extension（AI 推論ルーティング）
- **llm-d** (CNCF Sandbox、2026年3月): Kubernetes ネイティブの分散 LLM 推論フレームワーク。Red Hat、Google Cloud、IBM、NVIDIA 等が創設
- **GitOps**: Argo/Flux が基盤的地位を確立
- **FinOps**: コスト可視性・ガバナンスが Kubernetes ワークフローにシームレスに統合
- **Gartner 予測**: 2026年までに新規デジタルワークロードの 95% がクラウドネイティブプラットフォーム上で開発

### 主要 Go 製クラウドネイティブプロジェクト

Kubernetes、Docker、Prometheus、Istio、Cilium、Envoy（コントロールプレーン）、Terraform、Consul、Vault、etcd、containerd、Argo、Flux、Falco、Vitess

Sources: [TinyGo 0.41 Blog](https://tinygo.org/blog/2026/tinygo-0-41-the-big-release/) | [wazero.io](https://wazero.io/) | [CNCF llm-d](https://www.cncf.io/blog/2026/03/24/welcome-llm-d-to-the-cncf-evolving-kubernetes-into-sota-ai-infrastructure/) | [Fairwinds K8s Playbook 2026](https://www.fairwinds.com/blog/2026-kubernetes-playbook-ai-self-healing-clusters-growth) | [Go WASI Support](https://go.dev/blog/wasi)

---

## まとめ：2026年の Go の全体像

| カテゴリ | 状況 |
|---|---|
| 言語進化 | Green Tea GC デフォルト化、ジェネリックメソッド承認、自己参照型追加、エラー構文変更を公式終了 |
| パフォーマンス | GC 10-40% 削減、cgo 30% 高速化、Swiss Tables で map 60% 高速化、SIMD 実験的サポート |
| エコシステム | json/v2 実験進行中、golangci-lint v2、`go fix` 完全刷新、Ollama 174K stars |
| AI/ML | エージェントフレームワーク急成長、MCP 公式 SDK、GoMLX v0.27、LangChainGo 9.4K stars |
| コミュニティ | 満足度 91%、5.8M 開発者、AI ツール利用率 53%、年収 $159K（US平均） |
| クラウドネイティブ | 事実上の標準言語を維持、K8s v1.36 開発中、llm-d で AI 推論基盤化 |
| 今後の注目 | Go 1.27 でジェネリックメソッド実装、Sum Types 探索中、json/v2 正式化、WASI P3 |

---

*調査日: 2026年6月16日*
*調査手法: 複数の Web ソースからの横断的調査・クロスバリデーション（50+ ソース参照）*
