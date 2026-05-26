# Go言語トレンドサマリー

**更新日時:** 2026年5月26日

---

## Go 1.26 リリースと主要新機能（2026年2月）

Go 1.26が2026年2月にリリースされた。ツールチェーン・ランタイム・ライブラリ全般にわたる大幅な改善が含まれている。

### 言語仕様の変更

- **`new` 関数の拡張**: 組み込み関数 `new` のオペランドに式（Expression）を指定可能になり、基本データ型や関数の戻り値から直接ポインタを生成できるようになった
- **ジェネリクス型の自己参照**: ジェネリクス型が自身の型パラメータリスト内で自己参照できるようになり、複雑なデータ構造やインターフェースの実装が簡素化された

### ランタイム・パフォーマンス

- **Green Tea GC のデフォルト有効化**: 実験的だった Green Tea ガベージコレクタがデフォルトで有効化。SIMDを活用したループ処理の並列化により、AIインフラをはじめとする超高負荷なデータ処理が大幅に最適化された
- **cgo オーバーヘッド 30% 削減**: GoとC言語の境界をまたぐ cgo 呼び出しのベースラインオーバーヘッドが約30%削減
- **スライスのスタック割り当て最適化**: コンパイラがスライスのバッキングストアをスタック上に割り当てられるケースが拡大し、パフォーマンスが向上
- **ヒープベースアドレスのランダム化**: 64ビットプラットフォームでランタイム起動時にヒープベースアドレスをランダム化するセキュリティ強化

### ツーリング

- **Go Fix の再構築**: `go fix` コマンドがGo分析フレームワークベースに完全リライトされ、新開発の「Modernizer フレームワーク」により、挙動の正確性を維持したまま最新の言語機能を取り入れたコードへ自動変換が可能に
- **Goroutine リークプロファイラ（実験的）**: `runtime/pprof` パッケージに `goroutineleak` プロファイルタイプが追加され、リークしたgoroutineの検出が可能に

### セキュリティ

- **`runtime/secret` パッケージ（実験的）**: 秘密情報の安全な破棄をランタイムが保証する仕組みが導入

### 新しい標準ライブラリパッケージ

- `crypto/hpke` - Hybrid Public Key Encryption
- `crypto/mlkem/mlkemtest` - ML-KEM テスト支援
- `testing/cryptotest` - 暗号テスト支援

**参考リンク:**
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.26 is released - The Go Programming Language](https://go.dev/blog/go1.26)
- [Go 1.26: What's New and Why It Matters](https://travis.media/blog/go-1-26-whats-new/)
- [Go 1.26 unleashes performance-boosting Green Tea GC | InfoWorld](https://www.infoworld.com/article/4131097/go-1-26-unleashes-performance-boosting-green-tea-gc.html)

---

## セキュリティパッチ（Go 1.26.1 〜 1.26.3）

### Go 1.26.1（2026年3月）

暗号化・HTMLテンプレート・URLパッケージ等に対する5件のセキュリティ修正。

### Go 1.26.2（2026年4月7日）

go コマンド、コンパイラ、`archive/tar`、`crypto/tls`、`crypto/x509`、`html/template`、`os` パッケージに対するセキュリティ修正。CVE-2026-32283、CVE-2026-32282、CVE-2026-27144、CVE-2026-27140 等の脆弱性に対応。

### Go 1.26.3（2026年5月）

- HTMLテンプレートの属性内URLエスケープ不備によるXSS脆弱性（CVE-2026-27142）の修正
- HTTP/2トランスポートにおける不正な `SETTINGS_MAX_FRAME_SIZE` による無限ループ（DoS）の修正
- `consumePhrase` での二次的文字列連結によるメールアドレスパース時のDoS脆弱性の修正

**参考リンク:**
- [Release History - The Go Programming Language](https://go.dev/doc/devel/release)
- [Go 1.26.2 Released: Security Fixes](https://dev.to/ratneshmaurya/go-1262-released-security-fixes-regression-patches-and-an-upgrade-playbook-1ebo)
- [Go 1.26.3 and Go 1.25.10 are released](https://groups.google.com/g/golang-announce/c/qcCIEXso47M)

---

## Go 1.27 開発状況（2026年8月リリース予定）

Go 1.27のリリースフリーズは2026年5月20日に開始済み。2026年8月リリース予定。

### 予定されている主な変更点

- **Type Alias の改善**: `go/types` パッケージが GODEBUG 設定や go.mod の言語バージョンに関わらず、常に型エイリアスに対して `Alias` 型を生成するように変更
- **GC opt-out の廃止**: Green Tea GC のオプトアウト設定が削除予定
- **Goroutine リークプロファイラのデフォルト有効化**: 実験的だった goroutine リークプロファイルがデフォルトで有効化予定
- **新しい標準ライブラリパッケージ**:
  - `uuid` - UUID の生成とパース
  - `crypto/mldsa` - ポスト量子暗号 ML-DSA 署名スキーム（FIPS 204）の実装
- **プラットフォームサポート**: macOS 13 Ventura 以降が必須に（macOS 12 のサポート終了）

**参考リンク:**
- [Go 1.27 Release Notes (tip)](https://tip.golang.org/doc/go1.27)
- [Tree status for Go 1.27 development](https://groups.google.com/g/Golang-dev/c/BfBYry81mIc)
- [doc: write release notes for Go 1.27](https://github.com/golang/go/issues/78779)

---

## Google Agent Development Kit for Go 1.0（2026年3月31日）

GoogleがGoogle I/O 2026に先立ち、Agent Development Kit (ADK) for Go 1.0をリリース。AIエージェントを本番環境で安全に構築・管理するためのフレームワーク。

### 主な機能

- **マルチエージェントアーキテクチャ**: `SequentialAgent`、`ParallelAgent`、`LoopAgent` によるステップ実行・並行実行・反復実行が可能
- **OpenTelemetry 統合**: モデル呼び出しとツール実行ループごとに構造化トレース・スパンを生成し、複雑なエージェントロジックのデバッグを支援
- **プラグインシステム**: ロギング、セキュリティフィルタ、自己修正などの横断的関心事をエージェントの主要命令を変更せずに注入可能
- **Agent2Agent（A2A）プロトコル**: Go、Java、Python エージェント間のシームレスな通信をサポート
- **クラウドネイティブ対応**: Google Cloud Run 等のクラウド環境への強力なデプロイサポート

**参考リンク:**
- [ADK Go 1.0 Arrives! - Google Developers Blog](https://developers.googleblog.com/adk-go-10-arrives/)
- [GitHub - google/adk-go](https://github.com/google/adk-go)
- [Agent Development Kit (ADK) - Go](https://google.github.io/adk-docs/get-started/go/)

---

## 開発ツールの動向

### GoLand 2026.1（2026年3月26日リリース）

JetBrains GoLand 2026.1がリリース。

### GoLand 2026.2 EAP（2026年5月11日開始）

GoLand 2026.2 Early Access Program が開始。パフォーマンスインサイト、メモリ最適化、プロジェクトオンボーディングの改善にフォーカス。

- **パフォーマンス分析ツール**: プロファイリング、エスケープ分析、構造体最適化を統合した「Go Performance Optimization」ツールウィンドウを新設
- **プロファイリング強化**: テストと通常の実行構成の両方でプロファイリングが可能に。pprof ベースのプロファイラがIDE に直接統合
- **Mutex プロファイラ**: goroutine 間のロック競合を可視化し、共有データへのアクセス時にgoroutine が互いにブロックする箇所を特定
- **エスケープ分析**: 値がスタックからヒープに不必要にエスケープするケースをエディタ内で直接ハイライト
- **構造体メモリ最適化**: 構造体のレイアウト改善によるメモリ節約を支援

**参考リンク:**
- [GoLand 2026.1 Is Released | The GoLand Blog](https://blog.jetbrains.com/go/2026/03/26/goland-2026-1-is-released/)
- [The GoLand 2026.2 Early Access Program Has Started](https://blog.jetbrains.com/go/2026/05/11/the-goland-2026-2-early-access-program-has-started/)

---

## コミュニティ・イベント

### Go Conference 2026（2026年9月11日）

- **開催日**: 2026年9月11日（金）
- **会場**: 中野セントラルパークカンファレンス
- **テーマ**: "Go Far, Go Together"
- **プロポーザル募集**: 2026年5月15日（金）より開始

### 2026年5月の Go コミュニティイベント

- 5/4（月）Women Who Go Tokyo 読書会「Goで作るセキュリティ分析LLMエージェント」#8（オンライン）
- 5/13（水）第52回 Go ハンズオン（仙台）
- 5/15（金）Miyazaki.go 勉強会 #4（宮崎）
- 5/16（土）第79回 横浜Go読書会（オンライン）
- 5/20（水）Go Connect #13（東京・新宿）

### 海外カンファレンス

GopherCon をはじめ、世界各地で Go 関連カンファレンスが開催予定。

**参考リンク:**
- [Go Conference 2026](https://gocon.jp/2026/)
- [Go Conference 2026のまとめ - Findy Conference](https://conference.findy-code.io/events/gocon-2026/244)
- [2026年5月のGoイベント一覧](https://blog.golang.jp/2026/04/20265go.html)
- [Golang conferences 2026 / 2027](https://dev.events/golang)

---

## 市場動向

Go言語はバックエンド開発・クラウドインフラ・CLIツール開発に加え、AIエージェント開発の分野でも存在感を拡大している。Google ADK for Go 1.0のリリースにより、Go言語でのAIエージェント構築が本格的に可能になった。

求人市場では引き続き高い需要があり、平均年収は350万〜1,500万円の幅で推移。特にWebサーバー・アプリケーション開発分野でPHPなどからGoへの移行が進んでおり、Go経験者を求める求人が増加傾向にある。メルカリ、ぐるなびなどの大手企業での採用実績も豊富。

---

*このサマリーは2026年5月26日時点の情報に基づいて作成されました。*
