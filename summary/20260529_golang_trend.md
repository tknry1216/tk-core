# Go言語トレンドサマリー (2026年5月)

## 1. 最新リリース: Go 1.26 (2026年2月)

Go 1.25の6ヶ月後にリリースされたGo 1.26では、言語仕様・ランタイム・標準ライブラリに多くの改善が含まれている。

### 言語仕様の変更

- **`new()` 関数の拡張**: `new` の引数に式を指定して初期値を設定可能に。`encoding/json` やProtocol Buffersなどでポインタによるオプショナル値を扱う際に便利
- **自己参照ジェネリクス**: ジェネリック型が自身の型パラメータリスト内で自分自身を参照可能に
- **`go fix` ツールの刷新**: コードをモダンなGoの機能に自動移行するアルゴリズムスイートを搭載

### Green Tea GC (ガベージコレクタ)

Go 1.25で実験的に導入された **Green Tea GC** がデフォルトで有効化。

- 小さいオブジェクトのマーキング・スキャン性能が向上（局所性とCPUスケーラビリティの改善）
- **実プログラムでGCオーバーヘッドが10〜40%削減**
- contiguous memory blocks (spans) をスキャンする設計で、Webサービスに多い小オブジェクト割り当てに最適化
- `GOEXPERIMENT=nogreenteagc` で無効化可能

### ランタイム・パフォーマンス改善

- **cgo呼び出しのオーバーヘッドが約30%削減**
- 64bitプラットフォームでヒープベースアドレスのランダム化（セキュリティ強化）
- コンパイラがスライスのバッキングストアをスタック上に割り当て可能なケースが増加
- goroutineリークを報告する新しいプロファイルタイプ（`runtime/pprof`、実験的）

### 標準ライブラリの改善

- JPEGエンコーダ/デコーダが高速・高精度な実装に置き換え
- `ReadAll` の中間メモリ割り当てが削減
- **`crypto/hpke`パッケージ**: RFC 9180準拠のHybrid Public Key Encryption。ポスト量子ハイブリッドKEMにも対応

### 過去のリリースからの注目機能 (Go 1.24〜1.25)

| バージョン | 主な機能 |
|---|---|
| Go 1.24 | ジェネリック型エイリアスの完全サポート、`go.mod` の `tool` ディレクティブ、GCポーズ15-25%削減、`go:wasmexport` ディレクティブ、TLS ECHサポート、ポスト量子鍵交換 (X25519MLKEM768) |
| Go 1.25 | Green Tea GC (実験的)、各種パフォーマンス改善 |

---

## 2. エコシステム・フレームワーク動向

### 主要Webフレームワーク

| フレームワーク | 特徴 | GitHub Stars |
|---|---|---|
| **Gin** | 最も人気のフレームワーク。高速・高い拡張性・開発者フレンドリーなAPI | 88,000+ |
| **Echo** | REST API特化。標準の `context.Context` を使用し、エラーを返す設計 | - |
| **Chi** | 標準ライブラリの延長線上の設計。`net/http` ミドルウェア互換 | - |
| **Fiber** | Express.jsインスパイア。高速・低メモリ使用量 | - |

### 主要ライブラリ

- **ORM / DB**: GORM、SQLC（SQLからの型安全コード生成）、pgx
- **設定管理**: Viper（JSON/YAML/TOML等マルチフォーマット対応）
- **マイクロサービス**: Go kit（サービスディスカバリ、ロードバランシング、フォールトトレランス）

---

## 3. コミュニティ・採用動向

### 成長指標

- **プロ開発者数**: 220万人（5年前の2倍） — JetBrains調べ
- **TIOBE Index**: 7位（Go史上最高位、2025年4月時点）
- **GitHub**: 成長速度3位（Python、TypeScriptに次ぐ）

### 主要ユースケース

- **クラウドネイティブ / DevOps**: Kubernetes、Terraform等のインフラツールの言語として不動の地位
- **AI/MLツーリング**: モデルデプロイや高性能推論環境でPythonの補完的ポジションを確立
- **IoT**: 並行接続の効率的処理と低リソース消費がIoTデバイスに最適

---

## 4. AIエージェント領域での躍進

2026年、Go言語はAIエージェント開発の主要言語の一つとなっている。

### MCP (Model Context Protocol) & A2A (Agent2Agent)

- **MCP**: エージェントとツール/データソース間の接続プロトコル（Anthropic提唱）
- **A2A**: エージェント間の通信プロトコル（Google提唱）。Linux Foundation傘下のAgentic AI Foundationが管理、v1.2に到達
  - 150以上の組織が本番運用
  - Microsoft、AWS、Salesforce、SAP、ServiceNowが採用
  - GitHub Stars 22,000超

### SDK対応

- Google **Agent Development Kit (ADK)** がv1.0 GAに到達（Go含む4言語対応: Python, Go, Java, TypeScript）
- A2A SDKもGo含む5言語で本番対応

### Goが選ばれる理由

- 高いコンカレンシー性能がエージェント間通信に適している
- バイナリデプロイの容易さがプロダクション環境での運用に有利
- TensorFlowのGoバインディング等でモデル統合にも対応

---

## 5. 開発者の課題・関心

2025年のGo Developer Surveyによると:

- **ベストプラクティス** の識別・適用に関するサポート要望が多い
- **標準ライブラリの活用** を深めたいという声
- **AI開発ツール** の利用は広まっているが、生成コードの品質への満足度は中程度
- よりモダンな言語機能の追加を期待する声

---

## Sources

- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.26 is released](https://go.dev/blog/go1.26)
- [Go 1.26 unleashes performance-boosting Green Tea GC - InfoWorld](https://www.infoworld.com/article/4131097/go-1-26-unleashes-performance-boosting-green-tea-gc.html)
- [Go 1.24 Release Notes](https://go.dev/doc/go1.24)
- [Using go fix to modernize Go code](https://go.dev/blog/gofix)
- [Popular Go Web Frameworks - JetBrains GoLand Blog](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)
- [Best Go Backend Frameworks in 2026 - Encore](https://encore.dev/articles/best-go-backend-frameworks)
- [Golang in 2026: Usage, Trends, and Popularity - ZenRows](https://www.zenrows.com/blog/golang-popularity)
- [Why Go Is Becoming a Language for AI Tooling in 2026](https://dasroot.net/posts/2026/02/why-go-becoming-language-ai-tooling-2026/)
- [Results from the 2025 Go Developer Survey](https://go.dev/blog/survey2025)
- [The Go Ecosystem in 2025 - JetBrains GoLand Blog](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/)
- [A2A Protocol](https://a2a-protocol.org/latest/)
- [MCP vs A2A: The Complete Guide to AI Agent Protocols in 2026](https://dev.to/pockit_tools/mcp-vs-a2a-the-complete-guide-to-ai-agent-protocols-in-2026-30li)
- [Google ADK 1.0 and A2A Protocol](https://explore.n1n.ai/blog/google-adk-1-0-a2a-protocol-multi-agent-standard-2026-05-04)
