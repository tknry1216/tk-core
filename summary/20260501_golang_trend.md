# Go言語トレンドサマリー（2026年5月）

## 1. Go言語の現在の位置づけ

- TIOBE Indexで **7位**（2024年の13位から上昇）を維持し、バックエンド開発・クラウドインフラ・CLIツール領域で事実上の標準言語となっている
- 公式Go開発者調査（2024 H2）では **93%** の開発者がGoに満足と回答
- **70%** のGo開発者がAIアシスタントを日常的に利用

## 2. 最新バージョン: Go 1.26（2026年2月リリース）

### 言語仕様の変更

- **`new` 関数の拡張**: `new` 関数のオペランドに式を指定可能になり、初期値付きの変数作成が可能に
- **ジェネリクスの自己参照型**: ジェネリック型が自身の型パラメータリスト内で自分自身を参照可能になり、複雑なデータ構造の定義が簡潔に

### ランタイム改善

- **Green Tea GC（ガベージコレクタ）がデフォルト有効化**: Go 1.25で実験的に導入されたGreen Tea GCが正式にデフォルトに
  - GCオーバーヘッドを実際のプログラムで **10〜40%削減**
  - 個別オブジェクトのマーキングから連続メモリスパンのスキャンへ移行し、局所性とCPUスケーラビリティを改善
  - Intel Ice Lake / AMD Zen 4以降では **SIMDベクタ命令** を活用し、追加で約10%の性能向上
  - ペーシングの最適化、ライトバリアオーバーヘッドの低減、ヒープサイジングの精度向上
  - `GOEXPERIMENT=nogreenteagc` でオプトアウト可能
- **cgoオーバーヘッド約30%削減**
- **スライスのバッキングストアのスタック割り当て最適化**: より多くの状況でスライスのバッキングストアがスタック上に割り当てられるように

### 標準ライブラリの追加

- `crypto/hpke` — Hybrid Public Key Encryption
- `crypto/mlkem/mlkemtest` — ML-KEM（ポスト量子暗号）テストユーティリティ
- `testing/cryptotest` — 暗号テストヘルパー

### Go 1.24からの主要変更（振り返り）

- **ジェネリック型エイリアス** の完全サポート
- **`go.mod` の `tool` ディレクティブ**: 実行可能な依存関係を `tools.go` ファイルなしで追跡可能に
- **`go:wasmexport`** コンパイラディレクティブの追加（WebAssembly関数エクスポート）
- **ポスト量子鍵交換 X25519MLKEM768** がデフォルト有効
- **TLS Encrypted Client Hello (ECH)** のサーバーサイドサポート

## 3. AI / MCP エコシステム

### MCP（Model Context Protocol）SDK

- **公式Go MCP SDK**（[modelcontextprotocol/go-sdk](https://github.com/modelcontextprotocol/go-sdk)）がGoogleとの共同メンテナンスで提供
- GoのMCPサーバーは**シングルバイナリ**、**ミリ秒起動**、**goroutineによるネイティブ並行処理**が強み
- goplsがMCPサポートを実装し、IDE統合が進展
- 主要なSDK:
  - **公式SDK**（modelcontextprotocol/go-sdk）— 仕様準拠
  - **mcp-go**（mark3labs/mcp-go）— HTTPトランスポート対応
  - **mcp-golang**（metoro-io/mcp-golang）— コミュニティ版

### AIエージェントフレームワーク

2026年の主要Goエージェントフレームワーク:

| フレームワーク | 特徴 |
|---|---|
| **LangChainGo** | Python版LangChainのGo移植。10以上のLLMプロバイダ統合、最大のコミュニティ |
| **Google ADK (Agent Development Kit)** | Googleのエージェント開発キット。30以上のDB接続をMCP Toolbox経由でサポート |
| **Eino** (ByteDance/CloudWeGo) | ByteDanceによるLLMアプリケーションフレームワーク |
| **Firebase Genkit** | Googleのマルチモーダル対応フレームワーク |
| **Jetify AI SDK** | Go向け軽量AIツールキット |
| **Agent SDK Go** | プロダクション向けAIエージェント構築フレームワーク |

## 4. Webフレームワーク動向

2026年のGoWebフレームワーク利用率:

| フレームワーク | シェア | 特徴 |
|---|---|---|
| **Gin** | 48% | 高速、ミニマル、最大のエコシステム |
| **Gorilla** | 17% | モジュラー設計、柔軟なルーティング |
| **Echo** | 16% | 高パフォーマンス、ミドルウェア充実 |
| **Fiber** | 11% | Express.jsライク、超高速 |
| その他 | 8% | Chi、標準ライブラリなど |

- Go 1.22以降、標準ライブラリの `ServeMux` がメソッド別ハンドラやパスパラメータに対応し、小〜中規模ではサードパーティフレームワーク不要に

## 5. その他の注目トレンド

### グラフィックス / GUI エコシステム

- Go初のプロフェッショナルグラフィックスエコシステムが登場
  - **580K以上** のPure Goコード
  - **5つのGPUバックエンド**、**4つのシェーダーターゲット**
  - GPU高速化2Dグラフィックス、22ウィジェットのGUIツールキット

### IoT分野での拡大

- Goの低メモリフットプリントと高速実行がIoTデバイスに適合
- goroutineによる並行処理がセンサーネットワーク・スマートホームシステムのデータストリーム処理に最適

### コミュニティ活動（日本）

- Women Who Go Tokyo、Golang Cafeなど定期的な勉強会が全国各地で開催
- 日本でのGoコミュニティが活発に成長中

## 参考リンク

- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.26 is released - The Go Blog](https://go.dev/blog/go1.26)
- [The Green Tea Garbage Collector - The Go Blog](https://go.dev/blog/greenteagc)
- [The Go Ecosystem in 2025 - JetBrains Blog](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/)
- [Popular Go Web Frameworks - JetBrains Blog](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)
- [Official Go MCP SDK - GitHub](https://github.com/modelcontextprotocol/go-sdk)
- [Go MCP Support in gopls](https://go.dev/gopls/features/mcp)
- [Google ADK for Go - Google Developers Blog](https://developers.googleblog.com/en/announcing-the-agent-development-kit-for-go-build-powerful-ai-agents-with-your-favorite-languages/)
- [LangChainGo - GitHub](https://github.com/tmc/langchaingo)
- [Top 7 Golang AI Agent Frameworks 2026](https://reliasoftware.com/blog/golang-ai-agent-frameworks)
- [The Future of Golang in 2026](https://www.ksolves.com/blog/golang/trends-shaping-the-next-generation)
- [Golang in 2026: Usage, Trends, and Popularity](https://www.zenrows.com/blog/golang-popularity)
