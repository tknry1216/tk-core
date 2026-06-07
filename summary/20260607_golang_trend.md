# Go言語トレンドサマリー (2026年6月)

## 1. Go 最新リリース状況

### Go 1.26 (2026年2月リリース)

Go 1.26 は Go 1.25 の6ヶ月後にリリースされた最新メジャーバージョン。ツールチェーン、ランタイム、標準ライブラリ全般にわたる改善が含まれる。

#### 言語仕様の変更

- **`new()` 関数の拡張**: `new` の引数に式を指定して初期値を設定可能に。JSONやProtocol Bufferのオプショナルなポインタフィールドの扱いが簡潔になった。
- **自己参照ジェネリクス**: ジェネリック型が自身の型パラメータリスト内で自分自身を参照可能に。複雑なデータ構造やインターフェースの実装が容易になった。

#### パフォーマンス改善

- **Green Tea GC がデフォルト化**: 実験的だった Green Tea ガベージコレクタがデフォルトで有効に。小さなオブジェクトのマーキング・スキャンがメモリローカリティとCPUスケーラビリティの向上により高速化。
- **cgo オーバーヘッド約30%削減**: cgo呼び出しのベースラインランタイムオーバーヘッドが大幅に削減。
- **スタックアロケーション改善**: コンパイラがスライスのバッキングストアをスタック上に確保できるケースが増加。
- **JPEG エンコーダ/デコーダ刷新**: より高速かつ高精度な新実装に置き換え。
- **`ReadAll` 最適化**: 中間メモリ割り当てが削減され、約2倍高速化、メモリ使用量も約半分に。

#### 開発ツール

- **`go fix` の刷新**: コードベースを最新のイディオムやコアライブラリAPIに自動更新する「モダナイザー」として再実装。

#### 新規パッケージ・実験的機能

- **`crypto/hpke`**: RFC 9180 に準拠した Hybrid Public Key Encryption (HPKE) の実装。ポスト量子ハイブリッドKEMもサポート。
- **`simd/archsimd`（実験的）**: アーキテクチャ固有のSIMD演算へのアクセスを提供。amd64で128/256/512ビットベクトル型をサポート。
- **ゴルーチンリーク検出（実験的）**: `runtime/pprof` パッケージにリークしたゴルーチンを報告する新プロファイルタイプを追加。

#### セキュリティ

- 64ビットプラットフォームでヒープベースアドレスの起動時ランダム化を実施し、cgo使用時のメモリアドレス予測攻撃を困難に。

### Go 1.25 (2025年8月リリース)

- Go 1.24 からのツール、ランタイム、コンパイラ、リンカ、標準ライブラリ全般にわたる改善。
- macOS 12 Monterey 以降が必須に（Go 1.24 が macOS 11 をサポートする最後のバージョン）。

### Go 1.24 (2025年2月リリース)

- **ジェネリック型エイリアス**: 型エイリアスのパラメータ化を完全サポート。
- **ツール依存管理**: `go.mod` の `tool` ディレクティブで実行可能依存を追跡（`tools.go` ハック不要に）。
- **WebAssembly**: `go:wasmexport` ディレクティブ追加。WASI Preview 1 でリアクタ/ライブラリとしてのビルドをサポート。
- **ポスト量子暗号**: X25519MLKEM768 鍵交換メカニズムがデフォルト有効。
- **GC改善**: インクリメンタルポーズタイムが平均15〜25%改善。

---

## 2. エコシステム・コミュニティ動向

### 普及状況

- **TIOBE Index**: 2025年4月時点で **7位**（Go史上最高順位）。
- **プロフェッショナル開発者数**: 主要言語として約 **220万人**、副次言語を含めると **500万人以上**（JetBrains調べ、5年前の2倍）。
- **成長率**: 2024年に **Python、TypeScript に次ぐ第3位** の成長速度。
- **新規採用予定**: ソフトウェア開発者の **11%** が今後12ヶ月以内にGoの採用を計画。

### 人気Webフレームワーク（2026年）

| フレームワーク | シェア | 特徴 |
|---|---|---|
| **Gin** | 48% | GitHub 88,000+ stars。最速クラスの性能と開発者フレンドリーなAPI |
| **Gorilla** | 17% | 成熟したツールキット群 |
| **Echo** | 16% | ミニマリストなルーティング、効率的なメモリ管理。マイクロサービス向き |
| **Fiber** | 11% | fasthttp ベースの高パフォーマンス。ただし汎用ミドルウェアとの互換性に制約 |
| **Chi** | - | 標準ライブラリの自然な拡張。`net/http` ミドルウェアがそのまま利用可能 |
| **Encore.go** | - | 分散システム向けモダンフレームワーク。インフラ自動化を内蔵 |

### ツーリング

- **golangci-lint**: CI/CDとローカル開発の両方で標準的なオールインワンリンターランナーとして定着。

---

## 3. AI・エージェント開発における Go の台頭

2026年は「GoによるAIエージェント開発元年」と言われるほど、AI関連のGoエコシステムが急速に拡大している。

### 主要AIエージェントフレームワーク

| フレームワーク | 提供元 | 特徴 |
|---|---|---|
| **Google ADK for Go** | Google | Go ネイティブなエージェント開発キット。30以上のDBをMCP Toolbox経由でサポート |
| **Firebase Genkit** | Google | マルチモーダルAIアプリケーション構築 |
| **LangChainGo** | コミュニティ | LangChain の Go 実装 |
| **Eino** | コミュニティ | 軽量AIエージェントフレームワーク |
| **Jetify AI SDK** | Jetify | プロダクション向けAI SDK |

### MCP (Model Context Protocol) エコシステム

- **公式Go SDK**: Anthropicが公式のGo MCP SDKをリリースし、GoでのMCPサーバー構築が容易に。
- **gopls MCP対応**: Go言語サーバー（gopls）に実験的MCPサーバー機能が内蔵され、AIアシスタントにgoplsの機能をMCPツールとして公開可能に。
- **Encore MCP Server**: サービスアーキテクチャ、DBスキーマ、分散トレースなどをAIエージェントに構造化データとして公開。

### Go が AI ツーリングで選ばれる理由

- **ゴルーチンとチャネル**: リアルタイムストリーミング、ツール並列化、マルチエージェント協調に最適。
- **強い型付け**: プロダクショングレードのAIソリューションに適した安全性。
- **REST/RPC サポート**: モデルデプロイメントや推論環境との統合が容易。
- **軽量バイナリ**: コンテナ化・デプロイが効率的。

---

## 4. AI ツール利用状況

- Go開発者の **70%以上** がAIアシスタント、エージェント、またはAI搭載コードエディタを定期的に利用。
- 情報検索や反復的なコードブロックの生成に活用されているが、品質面の懸念から満足度は中程度。
- TensorFlow の Go バインディングなど、モデルデプロイメント・高性能推論環境でのGoの役割が強化中。

---

## 5. Go が支える主要クラウドインフラ

Go はクラウドネイティブの中核言語としての地位を維持：

- **Kubernetes** — コンテナオーケストレーション
- **Docker** — コンテナランタイム
- **Terraform** — Infrastructure as Code
- **Prometheus** — モニタリング
- **Istio** — サービスメッシュ

---

## Sources

- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.26 is released](https://go.dev/blog/go1.26)
- [Go 1.26 Introduces Two Language Changes](https://www.phoronix.com/news/Go-1.26-Released)
- [Go 1.26: What's New and Why It Matters](https://travis.media/blog/go-1-26-whats-new/)
- [Go 1.24 Release Notes](https://go.dev/doc/go1.24)
- [Go 1.25 is released](https://hackernoon.com/go-125-is-released-the-go-programming-language)
- [Using go fix to modernize Go code](https://go.dev/blog/gofix)
- [The Go Ecosystem in 2025 - JetBrains](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/)
- [Results from the 2025 Go Developer Survey](https://go.dev/blog/survey2025)
- [Popular Go Web Frameworks - JetBrains](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)
- [Best Go Backend Frameworks in 2026 - Encore](https://encore.dev/articles/best-go-backend-frameworks)
- [Why Go Is Becoming a Language for AI Tooling in 2026](https://dasroot.net/posts/2026/02/why-go-becoming-language-ai-tooling-2026/)
- [Top 7 Best Golang AI Agent Frameworks in 2026](https://reliasoftware.com/blog/golang-ai-agent-frameworks)
- [Gopls: Model Context Protocol support](https://go.dev/gopls/features/mcp)
- [Google ADK for Go](https://developers.googleblog.com/announcing-the-agent-development-kit-for-go-build-powerful-ai-agents-with-your-favorite-languages/)
- [Golang in 2026: Usage, Trends, and Popularity](https://www.zenrows.com/blog/golang-popularity)
- [AI and Go in 2026 - Applied Go](https://appliedgo.net/spotlight/ai-and-go/)
