# Go言語トレンドサマリー 2026年6月

**更新日時:** 2026年6月6日

---

## 1. Go 最新バージョン情報

### Go 1.26（2026年2月10日リリース）

Go 1.26 は半年ごとのリリースサイクルに基づき 2026年2月にリリースされた。GC・コンパイラ・言語仕様の各方面で大きな改善が含まれる。

| バージョン | リリース日 | 主な内容 |
|-----------|-----------|---------|
| Go 1.26.0 | 2026/02/10 | Green Tea GC デフォルト化、自己参照ジェネリクス、`new()` 式オペランド、SIMD 実験的サポート、`crypto/hpke` |
| Go 1.26.1 | 2026/03/05 | セキュリティ修正（crypto/x509, html/template, net/url, os） |
| Go 1.26.2 | 2026/04/07 | 10件のセキュリティ修正（XSS、ワイルドカード証明書バイパス、tar DoS、TLS デッドロック等） |
| Go 1.26.3 | 2026/05/07 | セキュリティ修正（net/http, syscall, net/mail 等） |
| Go 1.26.4 | 2026/06/02 | セキュリティ修正（crypto/x509, mime, net/textproto） |

#### Go 1.26 の主な新機能

- **Green Tea ガベージコレクタ**: Go 1.25 で実験的導入されていた新GCがデフォルト有効に。実プログラムで **10〜40%** のGCオーバーヘッド削減。Intel Ice Lake / AMD Zen4 以降ではSIMD/ベクタ命令活用で追加の約10%向上
- **`new()` 式オペランド**: `p := new(42)` のように初期値を指定可能に（`*int` のポインタが生成される）
- **自己参照ジェネリクス**: ジェネリック型が自身の型パラメータリスト内で自身を参照可能に（F-bounded polymorphism）
- **`errors.AsType`**: エラー型アサーションの out-pointer パターンを置き換える新API
- **`crypto/hpke`**: Hybrid Public Key Encryption（RFC 9180）をサポート。ポスト量子ハイブリッド KEM 対応
- **実験的 `simd/archsimd`**: amd64 向けアーキテクチャ固有 SIMD 命令（128/256/512ビットベクタ型）
- **実験的 `runtime/secret`**: メモリ上の秘密情報を安全に消去するパッケージ
- **cgo オーバーヘッド約30%削減**
- **`go fix` 刷新**: Go analysis framework ベースで全面再構築。数十の「modernizer」アナライザを内蔵し、古いコードを自動的に最新のイディオムに更新

### Go 1.27（2026年8月リリース予定）

Go 1.27 は2026年8月リリース予定で、5月20日にリリースフリーズ済み。

#### Go 1.27 の注目新機能

- **ジェネリックメソッド（Generic Methods）**: Go 1.18 でジェネリクスが導入されて以来、最大のジェネリクス関連の進化。メソッド宣言で独自の型パラメータを宣言可能に。Robert Griesemer 氏の提案が2026年3月に承認（issue #77273）
  ```go
  func (r Receiver) Method[T any](arg T) { }
  ```
  制約: ジェネリックな具象メソッドはインターフェースメソッドを実装できない
- **構造体リテラルの拡張**: キーに任意の有効なフィールドセレクタを使用可能に
- **型推論の改善**: ジェネリック関数の型推論が、関数型変数への代入を含む全コンテキストに一般化
- **小規模アロケーション最適化**: 80バイト未満のアロケーションコストを最大30%削減

**参考リンク:**
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.27 Release Notes (WIP)](https://tip.golang.org/doc/go1.27)
- [Go Approves Generic Methods -- Machine Herald](https://machineherald.io/article/2026-03/12-go-approves-generic-methods-after-years-of-resistance-targeting-go-127/)
- [Green Tea GC -- Go Blog](https://go.dev/blog/greenteagc)

---

## 2. エコシステム・ツールの動向

### Web フレームワーク

JetBrains の Go Developer Survey によると、フレームワーク採用率は以下のとおり。

| フレームワーク | 採用率 | 特徴 |
|--------------|--------|------|
| **Gin** | 48% | 最も成熟。ミドルウェアエコシステムとドキュメントが充実 |
| **Gorilla** | 17% | 軽量でモジュラーなツールキット |
| **Echo** | 16% | Gin より洗練された API。バリデーション・テンプレート内蔵 |
| **Fiber** | 11% | FastHTTP ベースで最高の生スループット（約130k req/sec）。Node.js 開発者に人気 |
| **Chi** | - | 軽量・stdlib `net/http` 準拠。最小限の抽象化を好む開発者向け |

パフォーマンス面では Fiber が単純 JSON エンドポイントで Gin/Echo の約1.6倍のスループットを示すが、実際のDB・外部APIコールを含むワークロードでは差は縮小する。

### gRPC / Connect-RPC

- **grpc-go**: 最新 v1.81.1（2025年5月）。TLS 1.3 標準化、per-call メトリクスのカスタムラベル対応、バッファプーリングによるメモリ使用量削減
- **connect-go (connectrpc)**: v1.20.0。gRPC・gRPC-Web・Connect 独自プロトコルの3つをサポート。REST ライクな API で `curl` によるテストが可能。Buf 社が開発
- **gRPConf 2026**: 2026年9月3日開催予定

### ORM / データベースライブラリ

| ライブラリ | アプローチ | CRUD ベンチマーク | 適用場面 |
|-----------|----------|------------------|---------|
| **sqlc** | SQL-first コード生成 | **15.1ms**（最速） | マイクロサービス、パフォーマンス重視 |
| **Ent** | スキーマファースト、グラフベース | 良好 | 大規模コードベース、複雑なデータモデル |
| **Bun** | SQL-first + ORM 機能 | 良好 | パフォーマンスと明示的SQLの両立 |
| **GORM** | コードファースト Active Record | 82.6ms | 高速プロトタイピング、CRUD 中心アプリ |
| **sqlx** | 薄い SQL ラッパー | 良好 | database/sql 上の最小限抽象化 |

sqlc がマイクロサービスアーキテクチャで勢いを増している一方、GORM は開発者体験の良さから依然として最もポピュラー。

### テストツール

- **stretchr/testify**: v1.8.3 で安定。破壊的変更は受け付けない方針
- **go-openapi/testify v2**: v2.4.0（2026年5月）。stretchr/testify のフォークで活発に開発中。ジェネリクスによる型安全アサーション、ゴルーチンリーク検出内蔵、外部依存ゼロ
- **GoMock**: v2.0.0 で成熟。Testify との統合も良好
- テーブル駆動テストが Go のイディオマティックな主流パターンとして定着

### ビルド・モジュール

- `go fix` が Go 1.26 で刷新され、コードの自動モダナイズツールとして機能
- `go mod init` で生成される go directive が Go 1.25.0 を指定するように変更（後方互換性の促進）
- GoLand 2026.1 が Go 1.26 サポートを含めリリース（2026年3月）

**参考リンク:**
- [Gin vs Echo vs Fiber 2026 -- Encore](https://encore.dev/articles/gin-vs-echo-vs-fiber)
- [Popular Go Web Frameworks -- JetBrains (April 2026)](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)
- [sqlc vs GORM vs sqlx 比較 2026 -- Reintech](https://reintech.io/blog/sqlc-vs-gorm-vs-sqlx-go-database-libraries-compared-2026)
- [go-openapi/testify v2 -- GitHub](https://github.com/go-openapi/testify)

---

## 3. Go と AI / LLM / MCP

2026年、Go は AI ツーリングおよびプロダクションレベルの AI アプリケーション開発で存在感を急速に拡大している。

### MCP（Model Context Protocol）SDK

| ライブラリ | バージョン | GitHub Stars | 備考 |
|-----------|----------|-------------|------|
| **modelcontextprotocol/go-sdk**（公式） | v1.6.1 | 4,700 | Google と共同メンテナンス。MCP spec 2025-11-25 対応 |
| **mark3labs/mcp-go** | v0.54.1 | 8,800 | コミュニティ SDK。公式 SDK の誕生に影響を与えた |
| **metoro-io/mcp-golang** | - | - | 少ないコード量で MCP サーバーを構築 |
| **viant/mcp** | - | - | 軽量 JSON-RPC ベース実装 |

- **gopls（Go 言語サーバー）** に実験的な MCP サーバー機能が内蔵され、AI アシスタントに Go 開発ツールを直接公開可能に

### AI エージェントフレームワーク

| フレームワーク | バージョン | GitHub Stars | 開発元 |
|--------------|----------|-------------|--------|
| **Eino** | v0.9.4 | 11,700 | ByteDance（CloudWeGo）。Doubao・TikTok で実運用 |
| **LangChainGo** | v0.1.14 | 9,400 | コミュニティ。OpenAI/Gemini/Ollama 等を統合 |
| **Google ADK Go** | v1.4.0 | 8,100 | Google。YAML ベースエージェント定義、OpenTelemetry 統合、A2A プロトコル対応 |
| **Firebase Genkit for Go** | - | - | Google。Go の AI アプリ開発フレームワーク |

### LLM プロバイダ SDK

| SDK | バージョン | 備考 |
|-----|----------|------|
| **openai/openai-go**（公式） | v3.39.0 | Responses API、Chat Completions、ストリーミング、ツール呼び出し |
| **anthropics/anthropic-sdk-go**（公式） | v1.47.0 | Messages API、バッチ処理、エージェントフレームワーク（Beta） |
| **sashabaranov/go-openai** | - | コミュニティ製。公式より歴史が長い |
| **mozilla-ai/any-llm-go** | - | 8+プロバイダの統一インターフェース |

### Go 公式ブログの動き

Go チームが [Building LLM-powered applications in Go](https://go.dev/blog/llmpowered) を公式ブログで公開し、Go での AI アプリケーション開発を公式に推進している。

### 総括

Python がリサーチ・データサイエンスで支配的な地位を維持する一方、Go は **AI エージェントのデプロイ、MCP サーバー構築、高並行 AI API サービス** の領域で選択肢として定着しつつある。

**参考リンク:**
- [Official MCP Go SDK](https://github.com/modelcontextprotocol/go-sdk)
- [mcp-go (mark3labs)](https://github.com/mark3labs/mcp-go)
- [Google ADK Go](https://github.com/google/adk-go)
- [Eino (CloudWeGo)](https://github.com/cloudwego/eino)
- [LangChainGo](https://github.com/tmc/langchaingo)
- [Building LLM-powered applications in Go](https://go.dev/blog/llmpowered)
- [Why Go Is Becoming a Language for AI Tooling in 2026](https://dasroot.net/posts/2026/02/why-go-becoming-language-ai-tooling-2026/)

---

## 4. コミュニティ動向

### 主要カンファレンス

| イベント | 日程 | 場所 |
|---------|------|------|
| **GopherCon Singapore** | 2026/05/21 | シンガポール |
| **GopherCon Europe** | 2026/06/15-18 | ベルリン（Festsaal Kreuzberg） |
| **GopherCon US** | 2026/08/03-06 | シアトル（Seattle Convention Center） |
| **Go Conference 2026** | 2026/09/11 | 東京（中野セントラルパーク カンファレンス） |
| **gRPConf 2026** | 2026/09/03 | - |
| **GopherCon Africa** | 2026 | ケニア |
| **GopherCon Israel** | 2026 | イスラエル |

### 日本国内のコミュニティ活動

- **Go Conference mini in Sendai 2026**: 2026年2月21日に仙台で開催。テーマ「Go Forward Together」
- **月例イベント多数**: Asakusa.go、Kyoto.go、Women Who Go Tokyo、Yokohama Go Reading Group、Miyazaki.go、Go Connect、Biwako.go 等が定期的に活動
- **golang.tokyo**: Go 採用企業のコミュニティ。トークイベント・ハンズオン開催
- **Gophers Japan**: Go Conference 公式オーガナイザー

### 採用・市場動向

#### 開発者サーベイ（Go Developer Survey 2025、2026年1月公開）
- **回答者**: 7,070名（分析対象 5,379名）
- **満足度**: 91% が Go での開発に満足。約2/3が「非常に満足」
- **AI ツール利用**: 53% が AI コーディングツールを毎日使用
- **開発者の要望**: ジェネリクス改善、エラーハンドリング構文改善、パターンマッチング、Result 型

#### 採用統計
- **推定ユーザー数**: 410万人が過去1年に Go を使用。180万人がプライマリ言語として使用（JetBrains 推計）
- **TIOBE Index**: Go が **7位** に到達（Go 史上最高）
- **GitHub Octoverse**: 2024年に最も成長が速い言語の **3位**（Python、TypeScript に次ぐ）
- **API トラフィック**: Go が全 API コールの **12%** を占める（前年 8.4% から上昇、Cloudflare Radar）

#### 日本の求人市場
- **平均提示年収ランキング 1位**: Go が **723万円** で3年連続1位（paiza 2025年調査）
  - 2位 TypeScript: 714万円、3位 Ruby: 689万円
- **求人数**: Indeed Japan で 5,241件、マイナビエンジニアで 324件
- **需給ギャップ**: Go は「穴場言語」（需要に対して供給が不足している言語）として Kotlin、Swift と並び上位

**参考リンク:**
- [GopherCon 2026](https://www.gophercon.com/)
- [GopherCon Europe 2026](https://www.gophercon.eu/)
- [Go Conference 2026](https://gocon.jp/)
- [Go Developer Survey 2025](https://go.dev/blog/survey2025)
- [paiza プログラミング言語別年収ランキング](https://prtimes.jp/main/html/rd/p/000000233.000012063.html)

---

## 5. クラウド・インフラストラクチャ

### Kubernetes / CNCF

- **Go は CNCF プロジェクトの主要言語**: 58の CNCF プロジェクトが Go を使用（最多）
- **Kubernetes v1.36** が2026年にリリース。オートスケーリング改善、セキュリティポリシー強化、ネットワーキング拡張
- **OpenTelemetry（Go）**: コミット数 39% 増加、コントリビュータ数 1,301→1,756 に拡大
- **注目の Go ベース CNCF プロジェクト**: Cilium（eBPF ネットワーキング）、Dapr（分散アプリケーションランタイム）、Flux（GitOps CD）、Istio / Linkerd（サービスメッシュ）
- KubeCon + CloudNativeCon Europe 2026（アムステルダム）にて、Rust vs Go のコンテナネットワークスタック比較が発表される等、Go のクラウドインフラにおけるポジションは堅固

### WebAssembly / WASI

- **WASIp3 提案（issue #77141）**: Go に `wasip3/wasm` ポートを追加する正式提案が提出。WASIp3 はイディオマティックな goroutine 並行性をサポートする初の WASI マイルストーン
- **watgo（2026年4月）**: WAT パース・検証・WASM バイナリエンコードを行う純 Go ツールキット
- WebAssembly が実用的成熟段階に到達。サーバーサイド WASM が現実的なデプロイターゲットに

### サーバーレス（AWS Lambda）

- Go は Lambda でのコールドスタート **100ms 未満**（JVM ベースは1〜2秒）で最高のパフォーマンス
- **Go 1.x マネージドランタイムは非推奨**: `provided.al2023` カスタムランタイムへの移行が必須
- **aws-lambda-go-api-proxy アーカイブ化（2025年5月）**: AWS は Lambda Web Adapter（LWA）を推奨
- Profile-Guided Optimization（PGO）で Lambda 実行時間を最大 **15%** 短縮
- Node.js 比で **85% のコスト削減** 事例が報告

### Docker / コンテナ

- Docker 採用率が IT プロフェッショナルの **92%** に到達（2024年の80%から急上昇）
- Go 静的バイナリ（`CGO_ENABLED=0`）は **scratch** イメージで 10MB 未満のコンテナを実現
- **distroless**（`gcr.io/distroless/static-debian13`）は約2MiBで CA 証明書・タイムゾーンデータ込み
- マルチステージビルド + `docker scout` / `trivy` による脆弱性スキャンが標準プラクティスに

**参考リンク:**
- [CNCF Project Velocity 2025](https://www.cncf.io/blog/2026/02/09/what-cncf-project-velocity-in-2025-reveals-about-cloud-natives-future/)
- [WASIp3 Proposal for Go (issue #77141)](https://github.com/golang/go/issues/77141)
- [Go with AWS Lambda Guide 2026](https://reintech.io/blog/go-aws-lambda-serverless-functions-guide-2026)
- [Go and Kubernetes CLI Tools 2026](https://dasroot.net/posts/2026/02/go-kubernetes-cli-tools-2026/)
- [Building Minimal Go Containers](https://dasroot.net/posts/2026/02/building-minimal-go-containers-high-throughput-apis/)

---

## 6. 総括：2026年6月の Go を取り巻く主要テーマ

1. **パフォーマンスの飛躍**: Green Tea GC のデフォルト化（10〜40% GC 削減）、cgo 30% 高速化、SIMD 実験的サポートにより、Go のランタイムパフォーマンスは過去最高水準に
2. **ジェネリクスの成熟**: Go 1.26 の自己参照ジェネリクスに加え、Go 1.27 でジェネリックメソッドが実現。Go 1.18 以来最大のジェネリクス進化
3. **AI/LLM エコシステムの急成長**: 公式 MCP SDK、Google ADK Go、ByteDance Eino 等の登場により、Go は AI エージェント開発の有力言語に
4. **クラウドインフラの盤石な基盤**: CNCF 58プロジェクトで Go が最多言語。Kubernetes、Docker、サービスメッシュの中核
5. **日本市場での高い評価**: 平均提示年収3年連続1位（723万円）、活発なコミュニティ活動、需給ギャップによる高い市場価値
6. **WebAssembly の新展開**: WASIp3 提案により goroutine 並行性を活かした WASM 開発が視野に

---

*このサマリーは 2026年6月6日時点の Web 調査に基づいて作成されました。*
