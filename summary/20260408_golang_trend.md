# Go言語トレンドサマリー 2025-2026

**更新日時:** 2026年4月8日

---

## Go リリース動向

### Go 1.23（2024年8月リリース）
- **イテレータと Range-over-Functions**: `for-range` ループで `func(func(K) bool)` 形式のイテレータ関数が使用可能に。`iter` パッケージの新設と `slices` / `maps` パッケージへのイテレータ対応追加
- **ジェネリック型エイリアス**: プレビューサポートとして導入
- **Timer / Ticker の改善**: 未停止の Timer/Ticker がガベージコレクション対象に。タイマーチャネルがアンバッファード（容量0）に変更され、`Reset` / `Stop` 後の古い値の受信が防止
- **PGO（Profile-Guided Optimization）の改善**: ビルド時間短縮と 386/amd64 でのパフォーマンス向上
- **ツーリング強化**: `go env -changed` で変更済み設定のみ表示、`go mod tidy -diff` でファイル変更なしに差分確認可能

**参考:** [Go 1.23 Release Notes](https://go.dev/doc/go1.23)

### Go 1.24（2025年2月リリース）
- **ジェネリック型エイリアス正式サポート**: 型エイリアスに型パラメータを付与可能に
- **Swiss Tables ベースの新 map 実装**: CPU オーバーヘッドが平均 2-3% 削減、小オブジェクトのメモリアロケーション効率化、ランタイム内部 mutex の改善
- **`go.mod` の tool ディレクティブ**: 実行可能な依存関係をモジュール内で追跡可能に（`tools.go` のワークアラウンドが不要に）
- **FIPS 140-3 準拠メカニズム**: Go Cryptographic Module による FIPS 140-3 承認アルゴリズムの透過的実装
- **TLS の ECH（Encrypted Client Hello）対応**: ポスト量子鍵交換 `X25519MLKEM768` がデフォルトで有効化
- **`testing/synctest` パッケージ（実験的）**: 並行コードのテスト支援。`synctest.Run` でゴルーチンを隔離された「バブル」内で実行し、フェイククロックで時間操作
- **`testing.B.Loop` メソッド**: `b.N` ベースのループに代わる、より高速でエラーの少ないベンチマーク反復手法
- **`os.Root` 型**: 特定ディレクトリ内でのファイルシステム操作を安全に制限
- **`go build` / `go install` に `-json` フラグ追加**: 構造化 JSON 出力が可能に

**参考:** [Go 1.24 Release Notes](https://go.dev/doc/go1.24), [Go 1.24 Interactive Tour](https://antonz.org/go-1-24/)

### Go 1.25（2025年8月リリース）
- **コンテナ対応 GOMAXPROCS**: Linux 上で cgroup の CPU 帯域幅制限を考慮し、`GOMAXPROCS` が自動調整。全 OS でランタイムが定期的に論理 CPU 数と cgroup 制限の変化を反映
- **実験的 JSON v2 実装**: `GOEXPERIMENT=jsonv2` で有効化可能な新しい JSON 実装
- **DWARF 5 デバッグ情報**: デバッグ情報のサイズ削減とリンク時間の短縮（特に大規模バイナリ）
- **コンパイラ最適化**: スライスのバッキングストアがより多くの状況でスタック上にアロケート可能に
- **`go build -asan` 強化**: AddressSanitizer のサポート改善
- **`go mod ignore` ディレクティブ**: モジュール管理の柔軟性向上
- **`go doc -http` サーバー**: ローカルドキュメントサーバーの新設
- **実験的 GC・FlightRecorder**: ガベージコレクションとランタイム診断の実験的機能

**参考:** [Go 1.25 Release Notes](https://go.dev/doc/go1.25), [Go 1.25: The Container-Native Release](https://dev.to/klaus82/go-125-the-container-native-release-5dfd)

### Go 1.26（2026年2月リリース）
- **`new()` に初期値式サポート**: `new` 組み込み関数が初期値の式を受け取れるように
- **自己参照ジェネリック型**: ジェネリック型が自身の型パラメータリスト内で自己参照可能に。複雑なデータ構造の定義が簡素化
- **Green Tea GC（デフォルト化）**: Go 1.25 で実験的だった新 GC が正式にデフォルトに。GC オーバーヘッドが 10-40% 削減
- **cgo オーバーヘッド約 30% 削減**: C 言語連携のパフォーマンス改善
- **`go fix` の刷新**: Go 分析フレームワークを使用した「modernizer」アナライザーが数十個追加。新しい言語機能を活用する安全なリファクタリングを自動提案
- **実験的 SIMD パッケージ**: `simd/archsimd`（`GOEXPERIMENT=simd`）で amd64 向け 128/256/512 ビットベクター型の SIMD 操作が可能に
- **新暗号パッケージ**: `crypto/hpke`（RFC 9180 HPKE）、`crypto/mlkem/mlkemtest`、`testing/cryptotest`
- **`runtime/secret` パッケージ（実験的）**: 秘密情報の安全な破棄が可能に
- **Go 1.26.1（2026年3月）**: 暗号化・HTML テンプレート・URL パッケージのセキュリティ修正

**参考:** [Go 1.26 Release Notes](https://go.dev/doc/go1.26), [Go 1.26 - Phoronix](https://www.phoronix.com/news/Go-1.26-Released)

---

## エコシステム・ライブラリ動向

### Web フレームワーク・ルーター
| ライブラリ | 採用率 | 特徴 |
|-----------|--------|------|
| **Gin** | 48% | 最も広く使用される HTTP フレームワーク（~80k GitHub stars） |
| **Echo** | 16% | エンタープライズ向け、強力なミドルウェア・セキュリティ |
| **Fiber** | 11% | Express.js ライクな API、fasthttp ベース、高パフォーマンス |
| **Chi** | - | 軽量ルーター、net/http 互換、標準ライブラリ志向 |
| **net/http（標準）** | - | Go 1.22+ でメソッドベースルーティング・パスパラメータ対応。標準のみで十分なケースが増加 |

### ORM・データベース
| ライブラリ | 特徴 |
|-----------|------|
| **GORM** | フル機能 ORM、最大のコミュニティ |
| **Ent** | Facebook 発、グラフベースの型安全 ORM |
| **sqlc** | SQL からの型安全な Go コード生成 |
| **Bun** | 軽量・高性能な SQL ファースト ORM |

### CLI ツール
| ライブラリ | 特徴 |
|-----------|------|
| **Cobra** | CLI アプリの標準的フレームワーク |
| **Bubble Tea** | Charm 製、リッチな TUI フレームワーク |
| **Lip Gloss** | Charm 製、ターミナル向けスタイリング |

### 構造化ロギング（`slog`）
Go 1.21 で導入された `log/slog` パッケージが標準ロギングとして定着。サードパーティ（Zap、Zerolog）と比較して十分な性能を持ち、標準ライブラリである安心感から採用が加速。

**参考:** [Structured Logging with slog - Go Blog](https://go.dev/blog/slog), [Logging in Go with Slog - Better Stack](https://betterstack.com/community/guides/logging/logging-in-go/), [Comparing the best Go ORMs (2026) - Encore](https://encore.cloud/resources/go-orms)

---

## クラウドネイティブ・インフラストラクチャ

### Kubernetes エコシステム
Go は Kubernetes エコシステムの主要言語として不動の地位を確立。以下のツールが Go で構築されている:
- **Kubernetes** 本体、**Docker**、**containerd**
- **Istio**（サービスメッシュ）
- **Prometheus**、**Grafana Agent**（モニタリング）
- **Terraform**、**Pulumi**（IaC）
- **ArgoCD**、**Flux**（GitOps）

### AI インフラとの融合
- **llm-d**（2025年5月発表）: Red Hat・NVIDIA との共同開発による Kubernetes ネイティブの分散推論フレームワーク。ハードウェア非依存・ベンダーニュートラルな設計
- **kServe**: Kubernetes 上でのモデルサービング
- Go による AI/ML ワークロードのオーケストレーション基盤が拡大

---

## Go と AI/ML

Python が AI/ML の主要言語であり続ける中、Go は独自のポジションを確立:

### LLM アプリケーションフレームワーク
- **LangChainGo** (`github.com/tmc/langchaingo`): LangChain の Go ポート。チェーン・エージェント・ツール・エンベディングをサポートし、`llms` パッケージで複数の LLM プロバイダと統合
- **GenKit**（Google）: 複数のモデルプロバイダ対応の統一 API。マルチモーダルコンテンツ・ツール呼び出し・エージェンティックワークフローを提供

### モデルサービング・推論
- **Ollama**: Go ベースのローカル LLM サービング（llama.cpp バックエンド）。オープンソース LLM をローカル実行する最も人気のツールの一つ

### ML ライブラリ
- **GoMLX**: Go 版の PyTorch/JAX/TensorFlow を目指すプロジェクト
- **Hugot**: Hugging Face Transformers を Go アプリケーションに統合するライブラリ

### Go の AI における役割
Go は主にモデルの学習ではなく、AI インフラ（サービング・オーケストレーション・バックエンド）で活用。並行処理・低メモリフットプリント・高速コンパイルが本番 AI サービングシステムに最適

**参考:** [AI and Go in 2026 - Applied Go](https://appliedgo.net/spotlight/ai-and-go/), [Go in the AI/ML Landscape - Medium](https://medium.com/@vladimirvivien/go-in-the-ai-ml-landscape-a-practical-guide-d36d44f360d2)

### セキュリティ・オブザーバビリティ
- **OpenTelemetry** が計装の標準として定着。Go SDK が主要言語 SDK の一つとして成熟
- **Kyverno**、**OPA Gatekeeper**、**Falco** などの Go 製セキュリティツールの普及
- **Jaeger**、**Fluent Bit**、**Grafana** によるオブザーバビリティパイプラインの確立
- eBPF ベースの Continuous Profiling エージェントが OpenTelemetry プロジェクトに寄贈

**参考:** [Go Tools Born from the Cloud-Native Movement](https://dasroot.net/posts/2026/03/go-tools-cloud-native-movement/), [OpenTelemetry Go Goals](https://opentelemetry.io/blog/2025/go-goals/)

---

## Go 開発者サーベイ 2025 結果

2025年9月に実施、5,379名の Go 開発者が回答。

### 満足度
- Go に満足している開発者: **91%**

### 主な用途
- コマンドラインツール: **74%**
- API / RPC サービス: **73%**

### AI ツール利用状況
- 日常的に AI 開発ツールを利用: **53%**
- AI ツール未使用または月数回程度: **29%**
- AI ツールに満足: **55%**（うち「とても満足」は 13%、「やや満足」は 42%）
- AI ツールの最大の不満: 非機能的なコードの生成（**53%**）、動作するコードでも品質が低い（**30%**）

### よく使われる AI コーディングアシスタント
| ツール | 利用率 |
|-------|--------|
| ChatGPT | 45% |
| GitHub Copilot | 31% |
| Claude Code | 25% |
| Claude | 23% |
| Gemini | 20% |

### 開発者の不満 TOP 3
1. Go のベストプラクティス / イディオムへの準拠確保（**33%**）
2. 他言語にある重要な機能が Go にない（**28%**）
3. 信頼できる Go モジュール・パッケージの発見（**26%**）

**参考:** [Results from the 2025 Go Developer Survey](https://go.dev/blog/survey2025), [Go developers meh on AI coding tools - InfoWorld](https://www.infoworld.com/article/4121617/go-developers-meh-on-ai-coding-tools-survey.html)

---

## 採用企業・市場動向

### Go を採用する主要企業
Google、Uber（~2,100 Go サービス、~5,000万行の Go コード）、Dropbox、Netflix、Cloudflare、Docker、HashiCorp、CrowdStrike、American Express、Salesforce、Datadog など多数のグローバル企業が Go を主要技術スタックとして採用。

### Microsoft が TypeScript コンパイラを Go に移植
**TypeScript 7 / Project Corsa** として、Microsoft が TypeScript コンパイラを Go に移植。ビルド時間 **10倍高速化**、メモリ使用量の劇的削減を達成。Go が選ばれた理由は、TypeScript 内部構造との構造的互換性、GC、並行処理の優位性。2026年初頭時点でエディタ・CLI での日常利用に十分安定。Go のパフォーマンス特性を示す最も注目度の高い事例の一つ。

### 市場ポジション
- **TIOBE Index 7位**（2025年4月）: Go 史上最高位
- **2024年に3番目に急成長した言語**（Python、TypeScript に次ぐ）
- **9,214社以上**が Go を使用（2026年時点）
- **開発者の11%** が今後12ヶ月以内に Go 採用を計画
- バックエンド開発・クラウドインフラ・CLI ツール開発の分野で事実上の標準言語として定着
- マイクロサービスアーキテクチャにおける第一選択言語の地位を維持
- DevOps / SRE / Platform Engineering 分野での圧倒的なシェア

### コミュニティ活動（日本）
2026年に入り、東京・京都・横浜・滋賀など全国各地でコミュニティイベント（Go Junction、layerx.go、Go Connect 等）が活発に開催。

**参考:** [17 Major Companies That Use Golang in 2025 - Netguru](https://www.netguru.com/blog/companies-that-use-golang), [Golang in 2026: Usage, Trends, and Popularity - ZenRows](https://www.zenrows.com/blog/golang-popularity), [The Go Ecosystem in 2025 - JetBrains GoLand Blog](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/)

---

## まとめ

Go は 2025-2026 年にかけて以下の方向で進化を続けている:

1. **言語の成熟**: ジェネリクスの型エイリアス・自己参照対応、イテレータ標準化、構造化ロギング（slog）の定着
2. **ランタイムの大幅最適化**: Green Tea GC（10-40% オーバーヘッド削減）、Swiss Tables マップ、コンテナ対応 GOMAXPROCS、cgo 30% 高速化、実験的 SIMD
3. **セキュリティ強化**: FIPS 140-3 準拠、ポスト量子暗号、ECH サポート、HPKE、`runtime/secret`
4. **クラウドネイティブ・AI インフラの中核**: Kubernetes エコシステムの基盤言語としての地位を強化。LLM サービング・オーケストレーション分野への拡大
5. **開発者体験の向上**: `testing/synctest`、`os.Root`、tool ディレクティブ、JSON v2 実験、`go fix` modernizer
6. **大規模採用の加速**: Microsoft TypeScript コンパイラの Go 移植、TIOBE 7位到達、91% の開発者満足度

---

*このサマリーは Web 上の公開情報を基に自動生成されました。*
