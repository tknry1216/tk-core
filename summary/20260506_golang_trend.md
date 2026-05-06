# Go言語トレンドサマリー (2026年5月)

## 1. Go言語の現在の位置づけ

- **TIOBE Index**: 2025年4月時点で第7位（Go史上最高順位）
- **開発者数**: 220万人のプロフェッショナル開発者がGoを主要言語として使用（5年前の2倍）
- **開発者の13.5%** が世界的にGoを選択
- **年収中央値**: $75,361（米国シニアクラスは最大$500,000）

## 2. 最新バージョンの主要機能

### Go 1.24（2025年2月リリース）

- **ジェネリック型エイリアス** の完全サポート
- **ツールディレクティブ**: `go.mod` に `tool` ディレクティブを追加し、実行可能な依存関係を追跡（`tools.go` ワークアラウンドが不要に）
- **Swiss Tables ベースの新しいmap実装**: より効率的なメモリ割り当て
- **`go:wasmexport`**: WebAssemblyホストへの関数エクスポート用コンパイラディレクティブ
- ランタイム内部mutexの新実装

### Go 1.25（2025年9月リリース）

- **GOAMD64=v3モード**: fused multiply-add命令による高速・高精度浮動小数点演算
- **DWARF version 5**: デバッグ情報サイズとリンク時間の削減
- **実験的JSONv2実装**（`GOEXPERIMENT=jsonv2`で有効化）
- **`WaitGroup.Go`メソッド**: goroutine作成とカウントをより便利に

### Go 1.26（2026年2月リリース）

- **Green Tea GC**: 実験的だったガベージコレクタがデフォルトで有効化
- **`new`関数の拡張**: オペランドに式を指定して初期値を設定可能
- **自己参照ジェネリック型**: 型パラメータリスト内で自身を参照可能に
- **cgoオーバーヘッド約30%削減**
- スライスのバッキングストアをスタック上に割り当てるケースが増加
- **実験的パッケージ**: `simd/archsimd`（SIMD演算）、`runtime/secret`（秘密情報の安全な消去）

## 3. 人気フレームワーク・ライブラリ

### Webフレームワーク

| フレームワーク | 特徴 | 利用率 |
|---|---|---|
| **Gin** | 高速・ゼロアロケーションルーティング | 48%（2025年） |
| **Echo** | マイクロサービス向け、カスタマイズ性高 | - |
| **Fiber** | Node.js開発者に馴染みやすい、リアルタイム向け | - |
| **Chi** | 標準net/httpベース、http.Handler互換 | - |
| **Hertz** | CloudWeGo製、クラウドネイティブ最適化 | 急成長中 |

### ロギング

- **log/slog**（標準ライブラリ、Go 1.21+）: 新規プロジェクトの第一選択
- **zap**: ハイパフォーマンスロギング
- **zerolog**: 軽量・高速

### リンター

- **golangci-lint**: 100以上のリンターを並列実行するオールインワンツール（gosec, govet, revive, errcheck等を含む）

## 4. AI/ML・LLMエコシステム

Go言語のAI/ML領域での存在感が急速に拡大している。

### 主要フレームワーク・ツール

| ツール | 用途 |
|---|---|
| **LangChainGo** | LangChainのGo実装。チェーン・エージェント・ツール統合 |
| **Google GenKit** | マルチモデルプロバイダ対応の統合API |
| **Jetify AI SDK** | Go-firstなマルチLLMプロバイダSDK |
| **gollm** | OpenAI/Anthropic/Groq/Ollama統合API |
| **MCP Go SDK** | Model Context ProtocolのGo公式SDK |
| **Ollama** | ローカルLLM実行（Go製） |
| **LocalAI** | OpenAI API互換のローカルLLMサーバー（Go製） |

### 2026年注目のAIエージェントフレームワーク

1. Google ADK
2. Firebase Genkit
3. LangChainGo
4. Eino
5. Jetify AI SDK
6. Anyi
7. Agent SDK Go

## 5. クラウドネイティブ・インフラ

Goはクラウドネイティブのデファクト言語としての地位を確立：

- **Kubernetes** - コンテナオーケストレーション
- **Docker** - コンテナプラットフォーム
- **Terraform** - Infrastructure as Code
- **100万人以上** の開発者がクラウド環境でGoを使用

## 6. 開発者トレンド（2025年サーベイより）

### AI開発ツールの利用状況

- **53%** が毎日AI開発ツールを使用
- **29%** はほとんど/全く使用しない
- 満足度は中程度（品質面の懸念あり）

### 主要ユースケース

- API/Webサービス開発
- CLIツール
- クラウドインフラ・DevOps
- マイクロサービス
- データパイプライン
- AI/MLモデルのデプロイ・推論

## 7. 今後の展望

- **Green Tea GC** の成熟によるさらなるパフォーマンス改善
- **AI/MLエコシステム** の継続的拡大（特にLLMサービング）
- **WebAssembly** サポートの強化
- **SIMD対応** による数値計算パフォーマンスの向上
- クラウドネイティブ・エッジコンピューティングでの採用拡大

---

## 参考ソース

- [Go 1.26 is released - The Go Programming Language](https://go.dev/blog/go1.26)
- [Go 1.25 Release Notes](https://go.dev/doc/go1.25)
- [Go 1.24 Release Notes](https://go.dev/doc/go1.24)
- [Results from the 2025 Go Developer Survey](https://go.dev/blog/survey2025)
- [The Go Ecosystem in 2025 - JetBrains GoLand Blog](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/)
- [Golang in 2026: Usage, Trends, and Popularity - ZenRows](https://www.zenrows.com/blog/golang-popularity)
- [AI and Go in 2026 - Applied Go](https://appliedgo.net/spotlight/ai-and-go/)
- [Top 7 Best Golang AI Agent Frameworks 2026](https://reliasoftware.com/blog/golang-ai-agent-frameworks)
- [Popular Go Web Frameworks - JetBrains GoLand Blog](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)
