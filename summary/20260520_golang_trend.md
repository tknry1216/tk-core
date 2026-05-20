# Go言語トレンドサマリー

**更新日時:** 2026年5月20日

---

## Go 1.26 リリース概要（2026年2月）

Go 1.26 は2026年2月10日にリリースされた。最新のパッチバージョンは Go 1.26.3（2026年5月7日）で、セキュリティ修正およびバグ修正が含まれる。次期メジャーバージョンの Go 1.27 は2026年8月リリース予定。

---

## 言語仕様の変更

### `new` 関数の拡張
- 組み込みの `new` 関数のオペランドに式を指定し、変数の初期値を設定できるようになった
- 従来: `p := new(int)` → `*p` は `0`
- Go 1.26: `p := new(42)` → `*p` は `42` のように初期値を直接指定可能

### ジェネリクスの自己参照型パラメータ
- ジェネリクス型が自身の型パラメータリスト内で自分自身を参照できるようになった
- 複雑なデータ構造やインターフェースの実装が簡素化される

---

## ランタイムの改善

### Green Tea GC のデフォルト有効化
- 以前は実験的機能だった Green Tea ガベージコレクタがデフォルトで有効化
- GC のレイテンシとスループットの両面で改善

### cgo パフォーマンス向上
- cgo 呼び出しのベースラインオーバーヘッドが約30%削減

### セキュリティ強化
- 64ビットプラットフォームにおいて、ランタイムが起動時にヒープベースアドレスをランダム化
- cgo 使用時のメモリアドレス予測攻撃に対する防御を強化

### スタックアロケーションの改善
- コンパイラがスライスのバッキングストアをスタック上に配置できるケースが増加し、パフォーマンスが向上

---

## 標準ライブラリの追加・改善

### 新パッケージ
- **`crypto/hpke`**: Hybrid Public Key Encryption のサポート
- **`crypto/mlkem/mlkemtest`**: ポスト量子暗号 ML-KEM のテストユーティリティ
- **`testing/cryptotest`**: 暗号テスト用ユーティリティ

### 実験的パッケージ
- **`simd/archsimd`**: SIMD（Single Instruction, Multiple Data）操作のサポート
- **`runtime/secret`**: 秘密情報（鍵・トークン等）の安全な一時保持と確実な消去
- **`runtime/pprof` goroutineleak プロファイル**: リークしたゴルーチンを検出する新しいプロファイルタイプ

### パフォーマンス改善
- JPEG エンコーダ・デコーダが新しい高速かつ高精度な実装に置き換え
- `ReadAll` の中間メモリ割り当てが削減され、約2倍の高速化と約半分のメモリ使用量を実現

---

## ツールチェーンの改善

### `go fix` の刷新
- 新しい `go fix` 実装により、コードをよりモダンなGoの機能を使うよう自動改善
- コードの近代化を安全に行うためのアルゴリズムスイートを搭載

---

## エコシステム・フレームワーク動向

### Webフレームワーク
| フレームワーク | 特徴 |
|---|---|
| **Gin** | 高パフォーマンス（Martiniの最大40倍）、豊富なミドルウェアエコシステム、構造体タグによるバリデーション |
| **Echo** | REST API に特化、`context.Context` の活用、エラーハンドリングが明示的 |
| **Chi** | 標準ライブラリ `net/http` の自然な拡張、`net/http` ミドルウェアがそのまま利用可能 |
| **Fiber** | 最高レベルのパフォーマンス、Express.js に近い API デザイン |
| **Encore.go** | 分散システム向け、インフラ自動化・オブザーバビリティ・サービスディスカバリを組み込み |

### ロギング・オブザーバビリティ
- **slog**: 構造化ログのデファクトスタンダードとして定着。高性能 JSON 出力、モダンなログ集約システムとのシームレスな統合
- **OpenTelemetry**: 分散トレーシング・メトリクスの標準的なソリューション

### ORM・データ管理
- **Ent**: 複雑なデータリレーションの管理に強い、型安全な ORM

### 時系列データベース
- **VictoriaMetrics v1.90**: 高性能・低コストの時系列データベース。大規模マイクロサービスアーキテクチャで採用拡大

---

## AI ツーリングにおけるGoの台頭

### 背景
- Python がAI研究側を支配する一方、Go はモデルのデプロイと高性能推論環境で存在感を拡大
- TensorFlow の Go バインディングや新興の Go ML ライブラリが充実

### GoがAIツーリングに選ばれる理由
- **高い並行処理性能**: ゴルーチンによる効率的な並行処理で、推論リクエストの並列処理に適する
- **シングルバイナリデプロイ**: コンテナ化・エッジデプロイが容易
- **低レイテンシ**: GC の改善により推論サービングの安定したレスポンスタイムを実現
- **クラウドネイティブとの親和性**: Kubernetes・Docker との高い親和性

---

## グラフィックスエコシステムの成長

- Go 1.26 と共に、58万行以上のピュアGoコード（シェーダコンパイラからGUIツールキットまで）による GPU エコシステムが確立
- Go が独立したグラフィックスプラットフォームとして成立しつつある

---

## コミュニティ・市場動向

### 開発者数・人気
- 過去1年間で推定 **410万人** が Go を使用、うち **180万人** がプライマリ言語として利用
- Stack Overflow Developer Survey で全開発者の **13.5%**、プロフェッショナル開発者の **14.4%** が Go を支持

### 企業採用
- Go を主要に使用する組織の **40%以上** がテクノロジーセクター（Google, Datadog, Dropbox, HashiCorp）
- 金融サービス **13%**（American Express, Monzo）
- 小売・物流（Uber, Amazon, HelloFresh）がスケーラブルなバックエンドインフラで採用

### 主要ユースケース
- **クラウドインフラ・DevOps**: Kubernetes, Terraform, Docker など主要ツールの実装言語
- **マイクロサービス**: gRPC による高性能サービス間通信
- **IoT**: 制約のあるハードウェア上での並行接続処理・データストリーム管理
- **CLI ツール**: シングルバイナリ配布の容易さから広く採用

---

## 参考リンク
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.26 is released](https://go.dev/blog/go1.26)
- [Go Release History](https://go.dev/doc/devel/release)
- [Using go fix to modernize Go code](https://go.dev/blog/gofix)
- [The Future of Golang (Go) in 2026 - Ksolves](https://www.ksolves.com/blog/golang/trends-shaping-the-next-generation)
- [Golang in 2026: Usage, Trends, and Popularity - ZenRows](https://www.zenrows.com/blog/golang-popularity)
- [Why Go Is Becoming a Language for AI Tooling in 2026](https://dasroot.net/posts/2026/02/why-go-becoming-language-ai-tooling-2026/)
- [Top Libraries for Go Developers in 2026](https://dasroot.net/posts/2026/02/top-libraries-go-developers-2026/)
- [Popular Go Web Frameworks - JetBrains GoLand Blog](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)
- [Best Go Backend Frameworks in 2026 - Encore](https://encore.dev/articles/best-go-backend-frameworks)
- [GoLand 2026.1 Is Released](https://blog.jetbrains.com/go/2026/03/26/goland-2026-1-is-released/)
