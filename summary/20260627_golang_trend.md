# Go言語トレンドサマリー

**更新日時:** 2026年6月27日
**対象:** Go言語（Golang）の最新動向・エコシステム・コミュニティ

---

## 1. Go 最新バージョン動向

### Go 1.26（2026年2月リリース）

Go 1.25 から6ヶ月後にリリースされた最新メジャーバージョン。言語仕様の改善、パフォーマンスの大幅な向上が含まれる。

#### 言語仕様の変更

- **`new()` 関数の拡張**: `new` の引数に式（expression）を指定可能になり、変数の初期値を直接指定できるようになった
- **ジェネリクスの自己参照制約の解除**: ジェネリック型が自身の型パラメータリスト内で自身を参照する制約が撤廃され、自己参照型制約の記述が可能に

#### パフォーマンス改善

- **Green Tea GC（ガベージコレクタ）**: Go 1.25 で実験的導入されていた Green Tea GC がデフォルトに昇格。小オブジェクト（< 512バイト）のマーキング・スキャンを8KiBスパン単位で処理し、ランダムなポインタ追跡からシーケンシャルスキャンへ変換。実環境で **10〜40% の GC オーバーヘッド削減** を実現
- **cgo オーバーヘッド削減**: cgo の基本オーバーヘッドが約 **30%** 削減
- **スタック上スライス割り当ての改善**: コンパイラがより多くのケースでスライスのバッキングストアをスタック上に割り当て可能に

#### 標準ライブラリの追加

- **`crypto/hpke`**: RFC 9180 に基づく Hybrid Public Key Encryption（HPKE）の実装。ポスト量子ハイブリッド KEM のサポートを含む
- **`simd/archsimd`（実験的）**: アーキテクチャ固有の SIMD 操作を提供。現在 amd64 で 128/256/512 ビットベクトル型をサポート

#### ツールチェイン改善

- **`go fix` コマンドの刷新**: Go のモダナイザーとして再設計。数十のフィクサーを搭載し、コードベースを最新のイディオム・APIに自動更新

### Go 1.25（2025年8月リリース）

- **`testing/synctest` パッケージ**: 並行コードのテスト支援。仮想化された時間でゴルーチンを隔離的に実行し、テストの確実性を向上
- **DWARF バージョン 5 対応**: コンパイラ・リンカがデバッグ情報を DWARF v5 で生成。大規模バイナリのデバッグ情報サイズとリンク時間を削減
- **コンテナ対応 GOMAXPROCS**: コンテナ環境での CPU リソース認識が改善
- **Green Tea GC（実験的導入）**

### Go 1.24（2025年2月リリース）

- **`go.mod` の `tool` ディレクティブ**: 実行可能な依存関係をモジュールで追跡可能に。従来の `tools.go` によるブランクインポートの回避策が不要に
- **`runtime.AddCleanup`**: `runtime.SetFinalizer` より柔軟で効率的なファイナライゼーション機構
- **`weak` パッケージ**: 弱参照（weak pointer）のサポート。メモリ効率の高いキャッシュや弱マップの構築に有用

---

## 2. エコシステム・フレームワーク動向

### Web フレームワーク

| フレームワーク | GitHub Stars | 特徴 |
|---------------|-------------|------|
| **Gin** | 88,000+ | 最も人気。高速・拡張性が高く、開発者フレンドリーな API |
| **Echo** | - | REST API に特化。`context.Context` を使用し、`panic` ではなくエラーを返す設計 |
| **Fiber** | - | Express.js にインスパイア。高速・省メモリで独自のルーティングエンジン |
| **Chi** | - | 標準ライブラリの延長のような設計。`net/http` ミドルウェアとの完全互換 |

### データベース関連

- **GORM**: Go 向け ORM の定番。Active Record パターンベース
- **SQLC**: SQL クエリから型安全な Go コードを自動生成
- **pgx**: PostgreSQL 向け高性能ピュア Go ドライバ

### 標準ライブラリの位置づけ

Go の標準ライブラリは非常に充実しており、多くのプロダクションサービスが `net/http` のみで運用されている。フレームワークに依存しない開発スタイルも一般的。

---

## 3. AI・LLM エコシステム

### Go が AI ツーリングの言語として台頭

2026年、Go は AI ツーリング・MLOps の分野で存在感を急速に拡大している。高い並行処理性能、低レイテンシ、効率的なリソース管理が評価されている。

### 主要ライブラリ・フレームワーク

| ライブラリ | 概要 |
|-----------|------|
| **LangChainGo** | LangChain の Go 実装。チェーン・エージェント・ツール・エンベディングなどのコンポーザブルなコンポーネントで LLM アプリを構築 |
| **GenKit** | Google の統一 AI API。マルチモーダル・ツールコール・エージェントワークフローをサポート |
| **any-llm-go** | Mozilla.ai 提供。単一の Go API で多様な LLM に統一アクセス |
| **GoMLX** | Go 向け加速化 ML フレームワーク |
| **gollm** | LLM プロバイダ統一インターフェース。柔軟なプロンプト管理と共通タスク関数 |

### ローカル LLM ソリューション

- **Ollama**: Go で書かれたオープンソースツール。コンシューマハードウェアでローカル LLM を実行。シンプルな API でクラウド非依存の AI を実現
- **LocalAI**: Go ベースの OpenAI API 互換ドロップインリプレースメント。GPU なしでコンシューマハードウェア上で LLM・画像生成・音声処理を実行

### Go の AI 領域での強み

- 数千の同時リクエストを処理する AI API の構築に最適
- LLM レスポンスのリアルタイムストリーミング
- 低いコンピュートコストでのプロダクション運用
- AI エンジニアリング・MLOps のインフラ言語としての地位を確立

---

## 4. エージェントプロトコル（MCP / A2A）

### Model Context Protocol（MCP）

LLM にコンテキストを提供する方法を標準化するオープンプロトコル。Go 向け公式 SDK が提供されており、MCP サーバー・クライアントの実装が容易。

### Agent2Agent Protocol（A2A）

- Google が 2025年4月に策定し、現在は Linux Foundation が管理
- 2026年4月時点で **150以上の本番組織**、**22,000以上の GitHub Stars** を達成
- Go を含む5言語（Python, TypeScript, Go, Java, .NET）で公式 SDK を提供
- **v1.2**（2026年3月）が現在の安定版
- AWS, Cisco, Google, IBM Research, Microsoft, Salesforce, SAP, ServiceNow がTSCメンバー

### MCP vs A2A の関係

- **MCP**: エージェント ↔ ツール 間の接続プロトコル
- **A2A**: エージェント ↔ エージェント 間の通信プロトコル
- 両者は補完関係にあり、Go のエコシステムでは両方のプロトコルをサポートするライブラリが充実

---

## 5. コミュニティ・開発者動向

### Go Developer Survey 2025 の結果

- **満足度**: 回答者の **91%** が Go に満足。うち **62%** が「非常に満足」
- **回答者数**: 5,379名の Go 開発者が参加（2025年9月実施）
- **世界の Go 開発者数**: 推定 **470万〜580万人**

### AI ツール利用状況

- Go 開発者の大半が AI パワード開発ツールを利用
- 主に利用されているのは **ChatGPT**, **GitHub Copilot**, **Claude**
- AI ツールへの満足度は **55%** が満足（うち「非常に満足」は 13%、「やや満足」は 42%）
- 品質面での懸念が残る

### 開発者が求めていること

- ベストプラクティスの特定と適用の支援
- 標準ライブラリの最大限の活用
- 言語・ビルトインツールのモダン機能の拡充

### Go の市場ポジション

- **クラウドネイティブインフラ**: Docker, Kubernetes 等のコンテナエコシステムが Go で構築されており、Dockerfile の利用は前年比 **120%** 成長
- **バックエンド開発**: マイクロサービス・API サーバーの実装言語として定着
- **DevOps ツーリング**: CLI ツール・インフラ管理ツールの開発言語として事実上の標準
- **IoT**: 軽量バイナリと低リソース消費を活かした組み込み・IoT 領域での採用拡大

---

## 参考リンク

- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.25 Release Notes](https://go.dev/doc/go1.25)
- [Go 1.24 Release Notes](https://go.dev/doc/go1.24)
- [Go 1.26 unleashes performance-boosting Green Tea GC - InfoWorld](https://www.infoworld.com/article/4131097/go-1-26-unleashes-performance-boosting-green-tea-gc.html)
- [Go 1.26 interactive tour](https://antonz.org/go-1-26/)
- [Results from the 2025 Go Developer Survey](https://go.dev/blog/survey2025)
- [Why Go Is Becoming a Language for AI Tooling in 2026](https://dasroot.net/posts/2026/02/why-go-becoming-language-ai-tooling-2026/)
- [Building LLM-powered applications in Go](https://go.dev/blog/llmpowered)
- [Popular Go Web Frameworks - JetBrains Blog](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)
- [Golang in 2026: Usage, Trends, and Popularity - ZenRows](https://www.zenrows.com/blog/golang-popularity)
- [AI and Go in 2026 - Applied Go](https://appliedgo.net/spotlight/ai-and-go/)
- [A2A Protocol](https://a2a-protocol.org/latest/)
- [Go Wiki: AI](https://go.dev/wiki/AI)

---

*このサマリーは自動生成されました。*
