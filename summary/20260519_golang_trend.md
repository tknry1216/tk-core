# Go言語トレンドサマリー

**更新日時:** 2026年5月19日

---

## Go 1.26 リリースと主要な新機能

Go 1.26 は 2026年2月10日にリリースされた。その後、セキュリティ修正を含むパッチリリースが継続的に提供されている。

| バージョン | リリース日 | 主な内容 |
|-----------|-----------|---------|
| Go 1.26 | 2026年2月10日 | メジャーリリース |
| Go 1.26.1 | 2026年3月5日 | crypto/x509, html/template, net/url, os パッケージのセキュリティ修正 |
| Go 1.26.2 | 2026年4月7日 | archive/tar, crypto/tls, crypto/x509, html/template, os パッケージのセキュリティ修正 |
| Go 1.26.3 | 2026年5月7日 | html/template, net, net/http, net/http/httputil, net/mail, syscall パッケージのセキュリティ修正 |

### 言語仕様の変更

- **`new` 関数の拡張**: 組み込みの `new` 関数のオペランドに式を指定できるようになり、変数の初期値を直接設定可能に
- **ジェネリクスの自己参照**: ジェネリック型が自身の型パラメータリスト内で自分自身を参照できるようになり、複雑なデータ構造やインターフェースの実装が簡素化

### Green Tea ガベージコレクタ（デフォルト有効化）

Go 1.25 で実験的に導入された Green Tea GC が Go 1.26 でデフォルト有効化された。

- **GC オーバーヘッドの 10〜40% 削減**: GC を多用するプログラムにおいて、マーキングとスキャンの性能が大幅に向上
- **スパン単位スキャン**: 個別オブジェクトのマーキングから、8 KiB スパン単位の連続メモリスキャンへ移行。ランダムなポインタ追跡を逐次スキャンに変換
- **SIMD 活用**: Intel Ice Lake / AMD Zen 4+ アーキテクチャでベクトル命令を使用した並列スキャンにより、さらに約 10% の性能向上
- **無効化オプション**: `GOEXPERIMENT=nogreenteagc` をビルド時に指定することで無効化可能

### その他のランタイム・パフォーマンス改善

- **cgo オーバーヘッドの約 30% 削減**: cgo 呼び出しのベースラインオーバーヘッドが大幅に改善
- **スライスのスタック割り当て拡大**: コンパイラがスライスのバッキングストアをスタック上に割り当てできるケースが増加
- **ゴルーチンリークプロファイル**: GC を利用してゴルーチンリークを検出する新しいプロファイルが導入。並行性プリミティブでブロックされているゴルーチンが、実行可能なゴルーチンから到達不能な場合にリークとして検出
- **`go fix` コマンドの刷新**: ゼロから再構築され、コードベースを現行の Go イディオムや API に自動更新する「モダナイザー」として機能

### 実験的機能

- **`runtime/secret` パッケージ**: `GOEXPERIMENT=runtimesecret` 有効時に `secret.Do(f)` 内で使われたレジスタ・スタックを返却前に消去し、秘密情報の安全な破棄をランタイムが保証

---

## Go 1.27 開発状況

Go 1.27 は 2026年8月のリリースが見込まれている。

- Green Tea GC のオプトアウト設定（`GOEXPERIMENT=nogreenteagc`）の削除が予定
- ゴルーチンリークプロファイルのデフォルト有効化
- `go/types` パッケージにおいて、GODEBUG 設定や go.mod の言語バージョンに関係なく、型エイリアスに対して常に `Alias` 型を生成するよう変更

---

## AI エージェント開発: Google ADK Go 1.0

2026年3月31日、Google が **Agent Development Kit (ADK) for Go 1.0** をリリース。Go で AI エージェントを本番環境で構築・管理するためのフレームワーク。

### 主な機能

- **マルチエージェントシステム**: SequentialAgent、ParallelAgent、LoopAgent による複雑なエージェントワークフローの構築
- **OpenTelemetry 統合**: モデル呼び出しやツール実行ループの構造化トレースとスパンを自動生成
- **プラグインシステム**: Retry and Reflect プラグインなど、エージェントの主要な指示を変更せずに横断的関心事を注入
- **Agent2Agent (A2A) プロトコル**: Go、Java、Python エージェント間のシームレスな通信をサポート

**参考リンク:**
- [ADK Go 1.0 Arrives! - Google Developers Blog](https://developers.googleblog.com/adk-go-10-arrives/)
- [GitHub - google/adk-go](https://github.com/google/adk-go)

---

## 開発者動向・エコシステム

### 開発者満足度（2025 Go Developer Survey）

2025年9月に実施された Go Developer Survey（回答者 5,379 名）の結果が2026年1月に公開された。

- **91% が Go に満足**: シンプルな言語仕様と充実した標準ライブラリ・ツールチェーンが高く評価
- **AI ツールの利用拡大**: 多くの Go 開発者が AI 開発ツールを使用しているが、品質面での満足度は中程度
- **改善要望**: ベストプラクティスの明確化、高品質なサードパーティモジュールの発見しやすさ、`go` コマンドのヘルプシステムの改善

### 市場規模と採用状況

- CNCF の推計によると、世界で約 **580 万人** の開発者が Go を使用
- Kubernetes、Docker、Terraform 等の DevOps スタック拡大に伴い、Go の需要も継続的に拡大
- バックエンド開発・クラウドインフラ・CLI ツール開発の分野で事実上の標準言語としての地位を確立

### Web フレームワーク利用状況

| フレームワーク | シェア |
|--------------|-------|
| Gin | 48% |
| Gorilla | 17% |
| Echo | 16% |
| Fiber | 11% |

### 注目される適用領域の拡大

- **IoT / エッジコンピューティング**: 低メモリフットプリントと高速実行、並行処理モデルを活かしたセンサーネットワーク・スマートホーム・産業 IoT への展開
- **AI インフラストラクチャ**: TensorFlow Go バインディングや LangChainGo を活用した大規模データ処理。モデル訓練ではなく AI バックエンドシステムとしての役割
- **注目ライブラリ**: OpenTelemetry（自動計測/eBPF）、chi / ConnectRPC（HTTP/RPC API）、Bun / sqlc（SQL ファースト・型安全なデータアクセス）、Testcontainers（統合テスト）

---

## 国内コミュニティ活動（2026年4〜5月）

2026年4〜5月も全国各地で Go 関連の勉強会・イベントが活発に開催されている。

### 4月の主なイベント

- **4/5** [オンライン] Gemini CLI と Go による MCP サーバー構築
- **4/10** [宮崎] Miyazaki.go 勉強会 #3
- **4/17** [東京・渋谷] Ebitengine ぷちConf #4
- **4/19** [オンライン] Women Who Go Tokyo 読書会「Go で作るセキュリティ分析 LLM エージェント」#6

### 5月の主なイベント

- **5/4** [オンライン] Women Who Go Tokyo 読書会「Go で作るセキュリティ分析 LLM エージェント」#8
- **5/13** [仙台] 第52回 Go ハンズオン コードラボ
- **5/15** [宮崎] Miyazaki.go 勉強会 #4
- **5/16** [オンライン] 第79回横浜 Go 読書会
- **5/20** [東京・新宿] Go Connect #13

---

## 参考リンク

- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.26 is released - The Go Blog](https://go.dev/blog/go1.26)
- [The Green Tea Garbage Collector - The Go Blog](https://go.dev/blog/greenteagc)
- [Go 1.26 unleashes performance-boosting Green Tea GC | InfoWorld](https://www.infoworld.com/article/4131097/go-1-26-unleashes-performance-boosting-green-tea-gc.html)
- [Go Release History](https://go.dev/doc/devel/release)
- [Results from the 2025 Go Developer Survey](https://go.dev/blog/survey2025)
- [Golang in 2026: Usage, Trends, and Popularity - ZenRows](https://www.zenrows.com/blog/golang-popularity)
- [Goバージョン一覧まとめ（2026年5月13日更新）](https://r1999.com/version/golang-version/)
- [2026年5月のGoイベント一覧 - golang.jp](https://blog.golang.jp/2026/04/20265go.html)
- [Popular Go Web Frameworks - JetBrains GoLand Blog](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)

---

*このサマリーは自動生成されました。*
