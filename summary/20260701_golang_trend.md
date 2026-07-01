# Go言語トレンドサマリー

**更新日時:** 2026年7月1日

---

## 1. Go 最新リリース状況

### Go 1.26（2026年2月リリース）

Go 1.26 は2026年最大のリリースであり、言語仕様の拡張・ランタイムの大幅改善・標準ライブラリの強化が行われた。2026年6月時点で Go 1.26.4 が最新パッチリリースとなっている。

#### 言語仕様の変更

- **`new` 関数の拡張**: `new` のオペランドに式を指定して初期値を設定可能になった（例: `p := new(int(42))`）
- **ジェネリクスの自己参照型パラメータ**: ジェネリック型が自身の型パラメータリスト内で自分自身を参照できるようになり、複雑なデータ構造やインターフェースの実装が簡素化された

#### Green Tea ガベージコレクタ（デフォルト有効化）

Go 1.26 最大の目玉機能。Go 1.25 で実験的に導入された Green Tea GC がデフォルトで有効化された。

- **10〜40% の GC オーバーヘッド削減**を実現
- 従来のオブジェクト単位の並行マーク＆スイープから、**メモリブロック単位のアーキテクチャ**に刷新
- 8KiB スパン単位で小オブジェクト（< 512 bytes）を逐次スキャンし、キャッシュ局所性を大幅改善
- **SIMD アクセラレーション**対応（Intel Ice Lake / AMD Zen 4+ で追加 ~10% の性能向上）
- アップグレードは自動的（コード変更・設定変更不要）

#### パフォーマンス改善

- **cgo オーバーヘッドが約 30% 削減**
- スライスのバッキングストアをより多くの状況でスタックに割り当て可能に
- AMD64 プラットフォームでの追加最適化

#### セキュリティ・暗号化の強化

- **`crypto/hpke` パッケージ新設**: RFC 9180 準拠の Hybrid Public Key Encryption（HPKE）を標準ライブラリで提供
- **ポスト量子暗号がデフォルト有効化**: TLS で `SecP256r1MLKEM768` / `SecP384r1MLKEM1024` 鍵交換が自動有効化
- 3つの新しい暗号関連パッケージが標準ライブラリに追加（サードパーティ依存なし）

### Go 1.25（2025年8月リリース）

- `go build -asan` の強化
- `go mod ignore` ディレクティブの追加
- `go doc -http` サーバーの新設
- コンテナ対応 GOMAXPROCS
- Green Tea GC の実験的導入（`GOEXPERIMENT=greenteagc`）

### Go 1.24（2025年2月リリース）

- **ジェネリック型エイリアスの完全サポート**
- **`tool` ディレクティブ**: `go.mod` で実行可能な依存関係を追跡可能に（`tools.go` ワークアラウンド不要）
- **`go:wasmexport`**: WebAssembly ホストへの関数エクスポートが可能に
- **TLS Encrypted Client Hello (ECH)** サポート
- **ポスト量子鍵交換 `X25519MLKEM768`** のデフォルト有効化

---

## 2. Go 開発者動向・統計

### Go Developer Survey 2025 結果

2025年9月に実施された公式開発者調査（回答数: 7,070、分析対象: 5,379）の主要結果:

| 指標 | 数値 |
|------|------|
| 満足度 | **91%**（63% が「非常に満足」）— 7年連続 |
| プロフェッショナル開発者の割合 | 87% |
| 主要業務で Go を使用 | 82% |
| 6年以上の開発経験 | 75% |
| AI ツールを毎日使用 | 53% |
| AI ツール未使用/ほぼ未使用 | 29% |
| Go 採用を計画中（全開発者の） | 11% |

### 市場規模・人気

- JetBrains によると、Go をプライマリ言語とするプロ開発者は **約220万人**（5年前の2倍）
- セカンダリ利用者を含めると **500万人以上**
- TIOBE Index で **第7位**（2025年4月時点、Go 史上最高順位）
- 最大の不満: 「Go のベストプラクティス/イディオムの遵守」（33%）

---

## 3. エコシステム・フレームワーク動向

### Web フレームワーク

| フレームワーク | GitHub Stars | 特徴 |
|--------------|-------------|------|
| **Gin** | 75,000+ | 最も人気。高速、構造体タグによるバリデーション、豊富なミドルウェア |
| **Echo** | - | マイクロサービス向け。最小限のルーティング、効率的なメモリ管理 |
| **Fiber** | - | Express.js インスパイア。Fasthttp ベース、ゼロメモリアロケーション |
| **Chi** | - | 最小限アプローチ。標準ライブラリに近い設計のルーティング強化 |
| **Encore.go** | - | 分散システム向け。インフラ自動化・オブザーバビリティ・サービスディスカバリ内蔵 |

### 標準ライブラリの利用状況

JetBrains State of Developer Ecosystem 2025 によると、Go 開発者の **32%** が `net/http` を使用しており、その人気は大きく変わっていない。

---

## 4. AI・機械学習インフラにおける Go の台頭

### Go が AI インフラ言語として確立

2026年、Go は AI システムの**インフラ層**で存在感を急速に拡大している。Python が研究・実験で支配的な一方、Go はモデルデプロイメント・高性能推論環境で採用が進んでいる。

#### 主要な動き

- **Ollama**: Go で書かれたオープンソース LLM ローカル実行ツール。コンシューマーハードウェアでシンプルな API により LLM を実行可能
- **GoMLX**: Go 向けアクセラレーテッド機械学習フレームワーク
- **AI ゲートウェイ**: Go ベースの AI ゲートウェイが認証・レート制限を含めても Python/Node.js より高いスループット密度を達成

#### Go AI エージェントフレームワーク

| フレームワーク | 特徴 |
|--------------|------|
| **Google ADK (Go)** | GitHub Stars 7,900+。マルチエージェント連携、MCP 統合、A2A プロトコル対応 |
| **LangChain Go** | 柔軟なライブラリベースのアプローチ |
| **Eino** | Go ネイティブ設計 |
| **Genkit** | ラピッドプロトタイピング向け |

### MCP・A2A プロトコルと Go

- **MCP（Model Context Protocol）**: 累計ダウンロード **9,700万回**。Anthropic・OpenAI・Google・Microsoft が採用。エージェント→ツール層の事実上の標準
- **A2A（Agent2Agent）プロトコル**: GitHub Stars **22,000+**。Go を含む5言語（Python, JS, Java, Go, .NET）でプロダクションレディ SDK を提供
- Go はエージェント間通信プロトコルにおける**主要対応言語**の一つとして、アジェンティック AI システムの構築で選ばれるケースが増加

---

## 5. クラウドネイティブ・インフラにおける Go の地位

Go はクラウドネイティブインフラ・DevOps ツーリング・マイクロサービスアーキテクチャの分野で引き続き**支配的な言語**としての地位を維持している。

### 主な活用領域

- **コンテナオーケストレーション**: Kubernetes、Docker など主要ツールの実装言語
- **IaC（Infrastructure as Code）**: Terraform の実装言語
- **オブザーバビリティ**: システム監視・API 標準化・長期保守性への注力が進む
- **IoT**: 複雑化する IoT エコシステムにおけるスケーラブルで信頼性の高いデバイス通信システムの構築

---

## 6. 2026年後半の注目ポイント

1. **Green Tea GC の実運用フィードバック蓄積**: デフォルト有効化後の大規模本番環境での効果測定が進む
2. **ポスト量子暗号の普及**: Go 標準ライブラリでのデフォルトサポートにより、量子コンピュータ時代への備えが加速
3. **AI エージェント開発における Go の拡大**: MCP/A2A プロトコル対応と高い並行処理性能により、プロダクショングレードの AI システム構築での採用増
4. **Go 1.27 への期待**: さらなる言語仕様の改善とランタイム最適化が見込まれる
5. **AI ツールとの統合深化**: 開発者の過半数が AI ツールを日常利用する中、Go 開発ワークフローとの統合がさらに進展

---

## 参考リンク

- [Go 1.26 is released - The Go Programming Language](https://go.dev/blog/go1.26)
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [The Green Tea Garbage Collector - The Go Programming Language](https://go.dev/blog/greenteagc)
- [Go 1.24 Release Notes](https://go.dev/doc/go1.24)
- [Results from the 2025 Go Developer Survey](https://go.dev/blog/survey2025)
- [Golang in 2026: Usage, Trends, and Popularity - ZenRows](https://www.zenrows.com/blog/golang-popularity)
- [Go Programming Language 2026: Cloud-Native Infrastructure - Programming Helper](https://www.programming-helper.com/tech/go-programming-language-2026-cloud-native-microservices)
- [AI and Go in 2026 - Applied Go](https://appliedgo.net/spotlight/ai-and-go/)
- [Why Go Is Becoming a Language for AI Tooling in 2026 - dasroot.net](https://dasroot.net/posts/2026/02/why-go-becoming-language-ai-tooling-2026/)
- [The Go Revolution: Golang in AI Agent Development 2026 - Mule AI](https://muleai.io/blog/2026-02-28-golang-ai-agent-frameworks-2026/)
- [Popular Go Web Frameworks - JetBrains Blog](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)
- [The Go Ecosystem in 2025 - JetBrains Blog](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/)
- [Go 1.26 New Crypto Packages: Post-Quantum Era - Medium](https://medium.com/techtrends-digest/go-1-26-new-crypto-packages-security-built-for-the-post-quantum-era-bfff794fd09a)

---

*このサマリーは自動生成されました。*
