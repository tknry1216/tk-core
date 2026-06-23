# Go言語トレンドレポート 2025-2026

> 調査日: 2026-06-23

## 目次

1. [最新バージョンの動向](#1-最新バージョンの動向)
2. [エコシステム・注目ライブラリ](#2-エコシステム注目ライブラリ)
3. [パフォーマンス改善とランタイムの進化](#3-パフォーマンス改善とランタイムの進化)
4. [コミュニティ動向と採用状況](#4-コミュニティ動向と採用状況)
5. [AI/ML・クラウドネイティブ・WebAssembly](#5-aimlクラウドネイティブwebassembly)
6. [開発ツール・エコシステムの進化](#6-開発ツールエコシステムの進化)

---

## 1. 最新バージョンの動向

### Go 1.24（2025年2月リリース）

- **ジェネリック型エイリアス**: 型エイリアスがジェネリクスに完全対応。定義型と同様にパラメータ化が可能になった
- **ツール依存管理**: `go.mod`に`tool`ディレクティブが追加され、実行可能な依存ツールを追跡可能に。従来の`tools.go`でブランクインポートするワークアラウンドが不要に
- **WebAssemblyエクスポート**: `go:wasmexport`コンパイラディレクティブの追加により、GoプログラムからWebAssemblyホストへの関数エクスポートが可能に
- **暗号化の強化**: `crypto/mlkem`パッケージ（ポスト量子暗号）の追加
- **プラットフォーム**: macOS 11 Big Surをサポートする最後のリリース

### Go 1.25（2025年8月リリース）

- **実験的ガベージコレクタ（Green Tea GC）**: 新しいGCが実験的に導入。低レイテンシ・低オーバーヘッドを実現
- **実験的 encoding/json/v2**: `GOEXPERIMENT=jsonv2`で有効化。従来のJSON処理の課題を解消する新実装
- **Core Types概念の廃止**: Go 1.18で導入された「Core Types」を完全に撤廃し、ジェネリクスの型制約がより柔軟に
- **DWARF 5デバッグ情報**: コンパイラ・リンカがDWARF 5形式のデバッグ情報を生成。バイナリサイズ削減とリンク時間の短縮
- **WaitGroup.Go メソッド**: ゴルーチンの作成とカウントを一体化する便利メソッドの追加
- **FMA命令対応**: `GOAMD64=v3`以上でfused multiply-add命令を使用し、浮動小数点演算の高速化・高精度化

### Go 1.26（2026年2月リリース）

- **`new`関数の拡張**: `new`関数のオペランドに式を指定可能に。変数の初期値を直接設定できるようになった
- **自己参照ジェネリクス**: ジェネリック型が自身の型パラメータリストで自己参照可能に。`Adder[A Adder[A]]`のような代数的インターフェースが記述可能
- **Green Tea GCのデフォルト化**: 実験的だったGreen Tea GCがデフォルトで有効に。実環境で10〜40%のGCオーバーヘッド削減
- **実験的SIMDパッケージ**: `GOEXPERIMENT=simd`で`simd/archsimd`パッケージが利用可能。amd64で128/256/512ビットベクトル型をサポート
- **HPKE暗号化**: `crypto/hpke`パッケージの追加（RFC 9180準拠）。ポスト量子ハイブリッドKEMのサポート
- **cgoオーバーヘッド約30%削減**: cgo呼び出しのベースラインオーバーヘッドが大幅に改善
- **スタックアロケーション改善**: スライスのバッキングストアをスタック上に配置できるケースが拡大

---

## 2. エコシステム・注目ライブラリ

### Webフレームワーク

| フレームワーク | シェア | 特徴 |
|---|---|---|
| **Gin** | 48% | 高速・シンプル。REST API/マイクロサービスの主力 |
| **net/http（標準ライブラリ）** | 32% | Go 1.22でルーティング強化。外部依存なし |
| **Gorilla** | 17% | 成熟したミドルウェアエコシステム |
| **Echo** | 16% | ミニマリスト設計。低レイテンシのマイクロサービス向け |
| **Fiber** | 11% | Express.jsライクなAPI。高スループット |

### ロギング

- **log/slog**（Go 1.21+標準）: 新規プロジェクトのデファクトスタンダード。構造化ログを標準ライブラリで実現
- **zap / zerolog**: ハイパフォーマンス用途で引き続き人気
- **logrus**: レガシープロジェクトで利用継続

### テスト

- **Godog**: BDDテストフレームワーク。採用率が約4%に成長（2025年時点）
- **testify**: アサーション・モックの定番ライブラリ
- **標準testing**: テーブルドリブンテストのパターンが引き続き主流

### ORM・データベース

- **GORM**: 最も広く使われるORM
- **sqlc**: SQLからGoコードを生成。型安全なデータアクセス
- **Ent**: Facebookが開発。グラフベースのORM

### CLI

- **Cobra**: CLIアプリケーションの業界標準
- **Bubble Tea**: TUI（ターミナルUI）フレームワーク。Elm Architectureベース

### マイクロサービス

- **Go kit**: 分散システム構築の定番。サービスディスカバリ・ロードバランシング・耐障害性
- **Encore**: Go向け次世代バックエンドフレームワーク。インフラ自動化

---

## 3. パフォーマンス改善とランタイムの進化

### Green Tea ガベージコレクタ

- Go 1.25で実験導入、Go 1.26でデフォルト化
- 小規模オブジェクトのマーキング・スキャンのローカリティとCPUスケーラビリティを改善
- **実環境で10〜40%のGCオーバーヘッド削減**
- Intel Ice Lake / AMD Zen 4以降ではベクトル命令を活用し、さらに約10%の追加改善
- ポーズタイムの大幅な短縮

### コンパイラ・リンカの改善

- DWARF 5形式のデバッグ情報生成（Go 1.25）: 大規模バイナリのリンク時間短縮
- スライスのスタックアロケーション拡大（Go 1.26）: ヒープ割り当て削減によるパフォーマンス向上
- FMA命令の活用（Go 1.25）: 浮動小数点演算の高速化

### ランタイムの進化

- **コンテナ対応の自動化**: CPU制限に基づく`GOMAXPROCS`の自動チューニング。クラウド・エッジ環境での効率向上
- **Flight Recorder**: 低オーバーヘッドの継続的トレーシング。オブザーバビリティの強化
- **cgoオーバーヘッド30%削減**（Go 1.26）: C言語連携のパフォーマンスボトルネック解消
- **SIMDサポート**（Go 1.26実験的）: amd64での128/256/512ビットベクトル演算

---

## 4. コミュニティ動向と採用状況

### 開発者数・人気

- **プロフェッショナル開発者220万人**がGoを主要言語として使用（5年前の2倍）
- セカンダリ言語を含めると**500万人超**
- **TIOBE Index 7位**（2025年4月時点）: Go史上最高位
- 2024年に**最も成長した言語の3位**（Python、TypeScriptに次ぐ）
- 世界の開発者の**13.5%**がGoを使用（プロフェッショナルでは14.4%）
- JetBrains Language Promise Indexで**4位**（TypeScript、Rust、Pythonに次ぐ）
- **11%の開発者**が今後12ヶ月以内のGo採用を計画

### 開発者満足度（2025年調査）

- Goに対する満足度は**90%以上**を継続維持
- 「非常に満足」が**62%**
- AIツールへの満足度は**55%**（「やや満足」42%、「非常に満足」13%）
- AIツールの課題: 非機能的コード生成（53%）、品質の低いコード（30%）

### 企業利用

- Go主要利用組織の**40%以上がテクノロジーセクター**
- **金融サービス13%**が次点
- Uber、Amazon、HelloFreshなどがバックエンドインフラでGoを利用
- エンタープライズユーザーの**75%が3ヶ月以内に生産性を発揮**
- **93%が1年以内に生産性を達成**
- Dockerfileのリポジトリ使用が**前年比120%成長**（190万リポジトリ、2025年）

### 今後の言語改善への要望

1. ジェネリクスのさらなる改善（より柔軟な型パラメータ）
2. エラーハンドリング構文の改善
3. パターンマッチング
4. Result型のサポート強化

---

## 5. AI/ML・クラウドネイティブ・WebAssembly

### AI/ML統合

Goは「AIモデルのトレーニング」ではなく「AIサービスのプロダクション展開」で強みを発揮する方向で成長。

#### 主要フレームワーク・ライブラリ

| ライブラリ/フレームワーク | 概要 |
|---|---|
| **LangChainGo** | LangChainのGo実装。チェーン・エージェント・ツール・エンベディングのコンポーザブルなコンポーネント |
| **GoMLX** | Go向けML加速フレームワーク。「PyTorch/JAX/TensorFlow for Go」。WebAssemblyでブラウザ実行も可能 |
| **Hugot** | GoMLXの上位レイヤー。パイプライン構築を簡素化 |
| **Ollama** | Go製のローカルLLM実行ツール。シンプルなAPIでローカルAI利用を実現 |
| **Google ADK** | GoogleのAIエージェント開発キット |
| **Firebase Genkit** | Google Firebaseの生成AI開発フレームワーク |
| **Eino** | ByteDance発のAIエージェントフレームワーク |

#### 2026年のGoにおけるAIエージェント開発

- スケーラブルでコンカレントなAIエージェントの構築にGoが最適解として台頭
- 主要7フレームワーク: Google ADK、Firebase Genkit、LangChainGo、Eino、Jetify AI SDK、Anyi、Agent SDK Go
- GoMLX + Hugotの組み合わせにより、単一言語でMLパイプライン全体をカバー可能

### クラウドネイティブ

- **Go = クラウドネイティブインフラの共通言語**: Docker、Kubernetes、Terraform、Prometheus、etcdなど主要ツールの大半がGo製
- Dockerfileリポジトリの**120%成長**がGoのクラウドネイティブ基盤としての存在感を裏付け
- コンテナ環境でのランタイム自動チューニング（GOMAXPROCS）が実用レベルに
- マイクロサービスアーキテクチャの実装言語として安定した地位

### WebAssembly

- Go 1.24の`go:wasmexport`ディレクティブにより、GoからWasmホストへの関数エクスポートが正式サポート
- GoMLXがWebAssemblyをサポートし、**MLモデルのブラウザ実行**が可能に
- エッジコンピューティングでのWASMエージェント実行が実用段階へ
- ブラウザ、Webアプリ、エッジデバイスでのGo実行が現実的に

---

## 6. 開発ツール・エコシステムの進化

### リンター・静的解析

- **golangci-lint**: オールインワンリンターランナーとして業界標準に。並列実行・キャッシュによる高速動作
- CI/CDパイプラインでの利用が標準的プラクティスに

### LSP・エディタサポート

- **gopls（Go Language Server）**: 継続的な改善。ジェネリクス対応の強化
- VS Code + Go拡張が最も利用されるIDE環境
- JetBrains GoLandが商用IDEとして高い支持

### ジェネリクスの活用状況

- Go 1.18（2022年）での導入以降、ライブラリでの型安全な抽象化が着実に浸透
- Go 1.25でCore Types廃止、Go 1.26で自己参照ジェネリクスが追加され、表現力が大幅向上
- スライスユーティリティ（`slices`パッケージ）、マップユーティリティ（`maps`パッケージ）が標準に

### モジュール・ビルドシステム

- `go.mod`の`tool`ディレクティブ（Go 1.24）: ツール依存管理の標準化
- Go Modulesの成熟: パッケージ管理の安定

---

## まとめ

Go言語は2025-2026年にかけて以下の方向で大きな進化を遂げている:

1. **ランタイムの根本的改善**: Green Tea GCのデフォルト化により、GCオーバーヘッドが10-40%削減。SIMD実験的サポートも開始
2. **ジェネリクスの成熟**: Core Types廃止、自己参照ジェネリクス追加により、型システムの表現力が飛躍的に向上
3. **AIエージェント開発の台頭**: LangChainGo、GoMLX、Ollamaを中心に、Goでのプロダクションレベルのai開発が現実的に
4. **クラウドネイティブの支配的地位を維持**: コンテナ・Kubernetes関連エコシステムの成長とともに、Goの重要性は増加
5. **安定した開発者コミュニティの成長**: TIOBE 7位到達、220万人のプロフェッショナル開発者、90%以上の満足度

---

## 出典

- [Go 1.24 Release Notes](https://go.dev/doc/go1.24)
- [Go 1.25 Release Notes](https://go.dev/doc/go1.25)
- [Go 1.25 is released](https://go.dev/blog/go1.25)
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.26 is released](https://go.dev/blog/go1.26)
- [Go 1.26 Introduces Two Language Changes, New Performance Improvements - Phoronix](https://www.phoronix.com/news/Go-1.26-Released)
- [Results from the 2025 Go Developer Survey](https://go.dev/blog/survey2025)
- [The Go Ecosystem in 2025 - JetBrains GoLand Blog](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/)
- [Golang in 2026: Usage, Trends, and Popularity - ZenRows](https://www.zenrows.com/blog/golang-popularity)
- [Popular Go Web Frameworks - JetBrains](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)
- [AI and Go in 2026 - Applied Go](https://appliedgo.net/spotlight/ai-and-go/)
- [The Go Revolution: Golang in AI Agent Development - Mule AI](https://muleai.io/blog/2026-02-28-golang-ai-agent-frameworks-2026/)
- [Top 7 Best Golang AI Agent Frameworks - Relia Software](https://reliasoftware.com/blog/golang-ai-agent-frameworks)
- [GoMLX - GitHub](https://github.com/gomlx/gomlx)
- [Go Wiki: AI](https://go.dev/wiki/AI)
- [Go developers meh on AI coding tools - InfoWorld](https://www.infoworld.com/article/4121617/go-developers-meh-on-ai-coding-tools-survey.html)
- [State of Go 2026 - The Dev Newsletter](https://devnewsletter.com/p/state-of-go-2026/)
- [Go Programming Language 2026: Cloud-Native Infrastructure](https://www.programming-helper.com/tech/go-programming-language-2026-cloud-native-microservices)
- [Best Go Backend Frameworks in 2026 - Encore](https://encore.dev/articles/best-go-backend-frameworks)
