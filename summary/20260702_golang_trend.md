# Go言語トレンドサマリー

**更新日時:** 2026年7月2日

---

## 最新リリース状況

### Go 1.27 RC（2026年8月リリース予定）
- 2026年6月18日に RC1 がリリース済み。安定版は2026年8月頃にリリース予定。

### Go 1.26（2026年2月リリース）
- **Green Tea GC がデフォルト化**: Go 1.25 で実験的に導入された新しいガベージコレクタが標準で有効に。小オブジェクトのマーキング・スキャンの局所性と CPU スケーラビリティを改善し、GC ヘビーなプログラムで 10〜40% の GC オーバーヘッド削減を実現。
- **cgo 呼び出しが約 30% 高速化**: ベースラインの cgo 呼び出しオーバーヘッドが大幅に削減。
- **`new(expression)` 構文**: `new(42)` で `*int` を生成可能に。ポインタ生成のヘルパー関数が不要に。
- **実験的 SIMD 組み込み関数**: `simd/archsimd` パッケージで amd64 向け 128/256/512 ビットベクタ演算をサポート。
- **自己参照型ジェネリクス**: ジェネリック型が自身の型パラメータリストで自身を参照可能に（F-bounded polymorphism）。
- **`errors.AsType`**: `errors.As` のアウトポインタパターンを置き換えるジェネリクス活用 API。
- **`crypto/hpke` パッケージ**: RFC 9180 準拠の Hybrid Public Key Encryption を標準ライブラリに追加。
- **`runtime/secret` パッケージ（実験的）**: 秘密情報の安全な破棄が可能に。
- **`go fix` の刷新**: Go の「モダナイザー」の集約先として、コードベースを最新のイディオム・API に更新するためのフィクサーを多数同梱。
- **実験的ゴルーチンリークプロファイル**: リークしたゴルーチンの検出を支援。

### Go 1.25（2025年8月リリース）
- **実験的 Green Tea GC**: `GOEXPERIMENT=greenteagc` で有効化。
- **実験的 `encoding/json/v2`**: 大幅に高速化された JSON デコード・アンマーシャリング。ストリーミングエンコード/デコード、プリティプリント、大文字小文字区別マッチング、重複キー拒否など多数の改善。
- **コンテナ対応 GOMAXPROCS**: Linux 上の cgroup CPU 帯域制限に基づき自動チューニング。
- **Flight Recorder トレーシング**: 継続的で低オーバーヘッドな可観測性を提供。

### Go 1.24（2025年2月リリース）
- **ジェネリック型エイリアスの正式サポート**: 実験的フラグなしで型エイリアスの型パラメータ化が可能に。
- **Swiss Tables マップ実装**: 新しい組み込みマップ実装で平均 2〜3% の CPU オーバーヘッド削減。
- **`go.mod` の `tool` ディレクティブ**: `tools.go` のワークアラウンドが不要に。
- **FIPS 140-3 準拠**: Go 暗号モジュールを通じた透過的な FIPS 準拠メカニズム。
- **ポスト量子暗号**: X25519MLKEM768 鍵交換がデフォルト有効に。
- **`testing/synctest` パッケージ（実験的）**: フェイククロックを使った並行コードのテスト支援。

---

## 言語機能の進化

### ジェネリクスの成熟
- **Go 1.18（2022年）** で導入されたジェネリクスが着実に進化。
- **Go 1.25** で「コア型（core types）」の概念が言語仕様から完全に削除され、仕様が簡素化。
- **Go 1.26** で自己参照型ジェネリクスが解禁され、F-bounded polymorphism が実現。ビルダーパターンや数学的型の表現力が大幅に向上。
- **ジェネリックメソッド（提案 #75526）が承認**: Robert Griesemer による提案が承認され、実装に移行中。メソッドが独自の型パラメータを宣言可能になる。Go のジェネリクスにおける長年の制限が解消される見込み。

### イテレータ / Range-over-func
- **Go 1.23（2024年8月）** で `for-range` ループがイテレータ関数をサポート。
- `iter.Seq[V]` / `iter.Seq2[K, V]` 型が標準ライブラリに追加。
- `iter.Pull` / `iter.Pull2` でプッシュイテレータをプルイテレータに変換可能。
- `maps.Keys`、`slices.Sorted` など標準ライブラリがイテレータベースの API を提供。
- コンパイラによるインライン最適化で、オーバーヘッドは無視できるレベル。

---

## エコシステム・ツーリング

### Web フレームワーク（2025年 JetBrains 調査）
| フレームワーク | 採用率 | 特徴 |
|---|---|---|
| **Gin** | 48% | 最も人気。httprouter ベースで高速 |
| **Gorilla** | 17% | 下降傾向（gorilla/mux は2023年にアーカイブ） |
| **Echo** | 16% | ミニマリスト設計、標準 context.Context 使用 |
| **Chi** | 12% | 標準ライブラリの自然な拡張 |
| **Fiber** | 11% | Express.js インスパイアの高速フレームワーク |

- **標準ライブラリ `net/http`** も Go 1.22 以降のルーティング強化により、サードパーティルータとの競争力が向上。
- **Encore**: バックエンドフレームワークとして注目度が上昇中。

### CLI・TUI ツール
- **Cobra**: Go の `go` コマンド自体にも使われる、支配的な CLI ツールキット。
- **Bubble Tea**: Elm Architecture ベースの強力な TUI フレームワーク。Charmbracelet エコシステムの中核。
- **Crush（by Charmbracelet）**: Bubble Tea で構築されたターミナルベースの AI コーディングアシスタント。

### データベース・ORM
- **Ent**: 複雑なデータリレーション管理で大きな牽引力を獲得。

### ロギング
- **log/slog**（Go 1.21+ 標準ライブラリ）: 構造化ロギングの自然な選択肢として定着。
- **logrus**: 安定しているがメンテナンスモードに移行。Go 1.21 未満をサポートするプロジェクト向け。

### 可観測性
- **OpenTelemetry Go SDK**: slog と組み合わせたシステム可視性と分散トレーシングが主流に。

### リンター・コード解析
- **golangci-lint**: CI/CD およびローカル開発の標準的なオールインワンリンターランナー。100 以上のリンターを並列実行。

### ワークフロー
- **Temporal**: スケーラブルなバックエンドアーキテクチャにおけるワークフローオーケストレーションツールとして定着。

---

## AI / ML エコシステム

### Go × AI の現在地
Go は ML のトレーニングよりも**推論・デプロイ**に特に適している。シングルバイナリデプロイにより、本番環境での依存関係管理が不要。Assembled 社の事例では、Go ベースの LLM インフラが Python の 3-5 倍少ないメモリで月間数百万の LLM リクエストを処理。

### LLM / AI エージェントフレームワーク
- **Genkit for Go（Google）**: 2025年9月に v1.0 安定版リリース。型安全な AI フロー、統一モデルインターフェース（Google AI、Vertex AI、OpenAI、Anthropic、Ollama 対応）、マルチモーダル、構造化出力、ツール呼び出し、RAG、エージェントワークフローをサポート。
- **Google ADK-Go**: AI エージェント開発キット。v2.0.0（2026年6月）。マルチエージェント構成、Agent2Agent（A2A）プロトコル、30 以上の DB 対応 MCP Toolbox、OpenTelemetry 統合、YAML エージェント定義。GitHub 8.3k stars。
- **Eino（ByteDance/CloudWeGo）**: Go AI フレームワークで最多の GitHub 12.1k stars。ChatModelAgent、DeepAgent（問題分解 + サブエージェント委任）、グラフ/ワークフロー構成、ヒューマンインザループ、ストリーム処理をサポート。
- **LangChainGo**: LangChain の Go 実装。GitHub 9.5k stars、1,900 以上の依存プロジェクト。2025年9月に CVE-2025-9556（プロンプトインジェクションによるファイル読み取り）が発見・修正。
- **any-llm-go（Mozilla.ai）**: 統一 LLM インターフェース。Anthropic、DeepSeek、Gemini、Groq、Ollama、OpenAI など 8 プロバイダ対応。
- **Anyi**: 自律型 AI エージェントフレームワーク。DAG ベースワークフロー定義、逐次/並列/条件分岐ステップ実行。RPA シナリオに最適。

### AI SDK
- **OpenAI Go SDK**（公式）: GitHub 3.3k stars、v3.41.0（2026年6月）。Responses API、Chat Completions、ストリーミング、ツール呼び出し、構造化出力、マルチターン対話をサポート。
- **Anthropic Go SDK**（公式）: Go 1.23+ 対応。Claude API の完全アクセス。
- **GoAI SDK**: 25 以上の LLM プロバイダに対応する統一 API。

### ML フレームワーク・ライブラリ
- **Ollama**: Go で構築された軽量 LLM ランタイム。Llama、Mistral、Gemma、DeepSeek などをローカルで実行・管理。Go AI エコシステムの代表的プロジェクト。
- **GoMLX**: Go エコシステムで最もアクティブに開発されている ML フレームワーク。OpenXLA バインディングで CPU/GPU（NVIDIA CUDA）/TPU をサポート。PyTorch/JAX に相当。GitHub 1.5k stars、v0.27.3（2026年4月）。HuggingFace モデルインポート、ONNX 変換、WASM コンパイル対応。2026年中頃にダイナミックシェイプサポートを予定。
- **Gorgonia**: ディープニューラルネットワークの構築・訓練。NLP・音声認識に強み。
- **Gonum ML**: 包括的な数値計算ライブラリ（Go の NumPy に相当）。
- **Bifrost**: 最速の LLM ゲートウェイ。LiteLLM の 50 倍高速で、適応的ロードバランシングと 1,000 以上のモデルをサポート。

---

## クラウドネイティブ

### CNCF エコシステム
- **15.6 百万人の開発者** がクラウドネイティブ技術を使用（CNCF + SlashData 調査、2025年11月）。
- CNCF は **230 以上のプロジェクト**、**300,000 以上のコントリビュータ**（190カ国、11,500 以上の組織）をホスト。
- 2025年春の CNCF Sandbox バッチでは **9 プロジェクト中 8 つが Go で記述** — Go がクラウドネイティブの支配的言語であることを証明。

### Kubernetes
- **Kubernetes v1.36「Haru」**（2026年4月）: 70 の機能強化（18 stable、25 beta、25 alpha）。ユーザー名前空間 GA、きめ細かな kubelet API 認証 GA、外部 ServiceAccount トークン署名 GA、CEL による Mutating Admission Policies。
- **client-go v0.36.2**（2026年6月）: 61,635 以上の Go パッケージがインポート。

### サービスメッシュ
- **Istio v1.29**（2026年）: Ambient モード（サイドカー不要）が安定版に昇格。
- **Cilium**: 5,000 以上の本番デプロイ。eBPF ベースで Istio サイドカーより 40-60% 低レイテンシ。
- **Knative**: 2025年10月に CNCF を卒業。Kubernetes 上のサーバーレス技術の成熟を示す。

### 可観測性
- **OpenTelemetry**: CNCF 第 2 位のプロジェクト。コミット 39% 増、コントリビュータ 1,301→1,756（+35%）。Go Auto-Instrumentation（eBPF ベース）が 2026年2月にベータリリース。

### Go の位置づけ
- Go はバックエンド開発・クラウドインフラ・CLI ツール開発の分野で事実上の標準言語としての地位を強化。Docker、Kubernetes、Terraform、Prometheus、Helm、Istio、CoreDNS など主要 CNCF プロジェクトの基盤。

---

## トレンドプロジェクト（GitHub）

### GitHub スター数 Top 20（Go プロジェクト、2026年7月時点）
| 順位 | プロジェクト | Stars | 説明 |
|---|---|---|---|
| 1 | awesome-go | 176k | Go ライブラリ・ツールのキュレーションリスト |
| 2 | Ollama | 175k | ローカル LLM ランタイム（月間 5,200 万 DL） |
| 3 | golang/go | 135k | Go 言語本体 |
| 4 | Kubernetes | 123k | コンテナオーケストレーション |
| 5 | frp | 108k | リバースプロキシ |
| 6 | Gin | 89k | HTTP Web フレームワーク |
| 7 | ragflow | 84k | RAG エンジン |
| 8 | fzf | 81k | コマンドラインファジーファインダ |
| 9 | lazygit | 80k | ターミナル Git UI |
| 10 | Hugo | 76k | 高速静的サイトジェネレータ |

### 2026年7月の急上昇プロジェクト
- **DeepSeek-Reasonix**（+9,973 stars/月）- DeepSeek ネイティブの AI コーディングエージェント
- **Crush（Charmbracelet）** - Bubble Tea ベースのターミナル AI コーディングアシスタント
- **Bifrost**（6.2k stars）- 最速 LLM ゲートウェイ、1,000+ モデル対応
- **CyberStrikeAI**（4.9k stars）- AI ネイティブセキュリティテストプラットフォーム
- **agentsview**（3.5k stars）- コーディングエージェントのセッション分析ツール

### 注目カテゴリ
- **セルフホスト**: Memos（61k stars）、PocketBase（59k stars）、CasaOS（36k stars）
- **開発ツール**: dive（54k stars）、lazydocker（52k stars）、GoReleaser（15k stars）
- **セキュリティ**: Trivy（37k stars）、gitleaks（28k stars）、gosec（8.9k stars）

---

## パフォーマンス改善のハイライト

| バージョン | 改善内容 |
|---|---|
| Go 1.24 | Swiss Tables マップで最大 60% 高速化（マイクロベンチマーク）、平均 2-3% CPU 削減。GC インクリメンタルポーズ 15-25% 改善。Datadog 実績: Swiss Tables 移行で特定サービスの RAM 200 TiB 削減 |
| Go 1.25 | コンテナ対応 GOMAXPROCS。Green Tea GC（実験的）で GC オーバーヘッド 10-40% 削減、L1/L2 キャッシュミス 50% 削減。スタック上スライス割り当て拡大。DWARF v5 でバイナリサイズ・リンク時間短縮。FMA 命令（GOAMD64=v3+）で浮動小数点演算高速化 |
| Go 1.26 | Green Tea GC デフォルト化。小オブジェクト割り当て（<512B）最大 30% 高速化。cgo 30% 高速化。新世代 AMD64 CPU で追加 ~10% GC 改善。Wasm ヒープメモリ管理効率化 |
| PGO | Profile-Guided Optimization で 2-14% パフォーマンス向上。Uber 実績: go-json の iTLB ミス 30% 削減、インライン最適化で 4% 性能向上 |

---

## 開発者ツーリングの進化

### gopls（Go Language Server）
- **gopls v0.20.0（2025年7月）**: 実験的な **Model Context Protocol（MCP）サーバー** を導入。AI コーディングエージェントがファイル全体を読み込まずに効率的にコンテキストを取得可能に。`go_diagnostics`、`go_references`、`go_rename_symbol`、`go_vulncheck` など12種類のツールを公開。
- **gopls v0.21.0（2025年12月）**: ドキュメントリンクへの定義ジャンプ、Hover でのナビゲーション追加。

### go vet の強化
- **Go 1.24**: `tests` アナライザ（テスト・ベンチマーク・ファザーの宣言ミス検出）、`printf` アナライザ更新、`buildtag` / `copylock` アナライザ改善。
- **Go 1.25**: `waitgroup` アナライザ（`WaitGroup.Add()` の配置ミス検出）、`hostport` アナライザ（`host:port` 文字列の構築ミス検出）を追加。

### Go テレメトリ
- Go 1.23 で導入されたオプトイン方式のテレメトリが定着。有効化すると週次で `telemetry.go.dev` にデータを送信。
- Microsoft が Go 1.25 から Microsoft ビルドの Go でテレメトリ収集を開始（`MS_GOTOOLCHAIN_TELEMETRY_ENABLED=0` で無効化可能）。

---

## コミュニティ・開発者動向

### Go Developer Survey 2025 の結果（回答者 5,379 名）
- **満足度**: 91% が Go での開発に満足（2019年以来安定）。
- **主な用途**: CLI ツール（74%）、API/RPC サービス（73%）。
- **AI ツール利用**: 78% が AI ツールを使用、53% が毎日利用。ただし満足度は 55% にとどまる。
  - 53% が「AI が生成するコードが動作しない」、30% が「動作しても品質が低い」と報告。
  - AI コードが Go のイディオマティックなパターンを外す傾向が課題。
- **AI アシスタント利用率**: ChatGPT（45%）、GitHub Copilot（31%）、Claude Code（25%）、Claude（23%）、Gemini（20%）。
- **開発環境**: VS Code（37%）、GoLand（28%）、Zed（4%）、Cursor（4%）。
- **課題**: ベストプラクティスの把握（33%）、他言語にある機能の不足（28%）、信頼できるモジュールの発見（26%）。

### 市場ポジション
- **TIOBE Index**: Go が 7位（2025年4月）に到達。過去最高順位。
- **開発者シェア**: 世界の開発者の 13.5% が Go を選好。プロフェッショナル開発者では 14.4%。
- **採用意向**: 全ソフトウェア開発者の 11% が今後12ヶ月以内に Go の採用を計画。
- **73% の新規 Go プロジェクトがジェネリクスを広範に使用**（2022年の 12% から急増）。

### カンファレンス（2026年）
- **GopherCon US 2026**（シアトル、8月3-6日）: ワークショップ、コミュニティ主導のプログラミング。
- **GopherCon UK 2026**（ロンドン、8月11-13日）: AI トラックが充実（プロダクション AI システム構築、ローカル LLM デプロイ、RAG パイプライン）。
- **GopherCon Europe 2026**（ベルリン）: ベストプラクティス、ケーススタディ、パフォーマンス最適化。

### Go のグラフィックスエコシステム
- 2026年に Go 初のプロフェッショナルグラフィックスエコシステムが登場。580,000 行以上の純粋な Go コード、完全な GPU コンピューティングスタック、GUI ツールキットを含む。

### WebAssembly の進展
- **Go 1.24**: `go:wasmexport` ディレクティブで Go 関数を WebAssembly ホストにエクスポート可能に。WASI Preview 1 リアクタ/ライブラリサポート。
- **Go 1.26**: Wasm 2.0 標準命令を無条件使用。ヒープメモリ管理の効率化（16 MiB 未満のヒープでメモリ使用量を大幅削減）。
- Wasm 3.0 仕様が2025年9月に確定（ネイティブ GC サポート）。WASI 0.2 と Component Model が安定化。

### リリースサイクル
Go は **6ヶ月のリリースサイクル**（開発期間約4ヶ月 + リリースフリーズ約3ヶ月）を維持。メジャーリリースは毎年 **2月** と **8月** に提供。常に最新2つのメジャーバージョンがサポート対象。

---

**参考リンク:**
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.25 Release Notes](https://go.dev/doc/go1.25)
- [Go 1.24 Release Notes](https://go.dev/doc/go1.24)
- [Go 1.23 Release Notes](https://go.dev/doc/go1.23)
- [Results from the 2025 Go Developer Survey](https://go.dev/blog/survey2025)
- [Go Blog - Generic Methods Approved](https://www.theregister.com/2026/03/02/generic_methods_go/)
- [Go Blog - Green Tea GC](https://go.dev/blog/greenteagc)
- [Go Blog - Swiss Tables](https://go.dev/blog/swisstable)
- [Go Blog - Building LLM-powered applications in Go](https://go.dev/blog/llmpowered)
- [JetBrains - Go Language Trends & Ecosystem 2025](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/)
- [JetBrains - Popular Go Web Frameworks 2026](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)
- [CNCF Annual Report 2025](https://www.cncf.io/reports/cncf-annual-report-2025/)
- [Eino - Go AI Framework (ByteDance)](https://github.com/cloudwego/eino)
- [Google ADK-Go](https://github.com/google/adk-go)
- [Genkit for Go](https://github.com/firebase/genkit)
- [GoMLX on GitHub](https://github.com/gomlx/gomlx)
- [awesome-golang-ai on GitHub](https://github.com/promacanthus/awesome-golang-ai)
- [State of Go 2026](https://devnewsletter.com/p/state-of-go-2026/)
- [Golang in 2026: Usage, Trends, and Popularity](https://www.zenrows.com/blog/golang-popularity)

---

*このサマリーは自動生成されました。*
