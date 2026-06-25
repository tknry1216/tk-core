# Go (Golang) トレンドサマリー 2025-2026

> 調査日: 2026-06-25

---

## 目次

1. [Go言語バージョンアップデート](#1-go言語バージョンアップデート)
2. [パフォーマンス改善](#2-パフォーマンス改善)
3. [人気フレームワーク・ライブラリ](#3-人気フレームワークライブラリ)
4. [AI/LLMエコシステム](#4-aillmエコシステム)
5. [クラウドネイティブ・インフラストラクチャ](#5-クラウドネイティブインフラストラクチャ)
6. [ツーリング改善](#6-ツーリング改善)
7. [コミュニティ・エコシステム動向](#7-コミュニティエコシステム動向)

---

## 1. Go言語バージョンアップデート

### Go 1.23 (2024年8月リリース)

- **range-over-func**: `iter`パッケージの導入により、関数型に対する`range`ループが可能に。ゼロアロケーションのイテレータパターンを実現
- **PGOビルド高速化**: Profile-Guided Optimizationのビルドオーバーヘッドが100%超から一桁台に大幅削減
- **タイマー再実装**: `time.Timer`/`time.Ticker`がヒープベースの単一タイマーに刷新され、メモリ消費量・パフォーマンスが改善
- **コンパイラ最適化**: PGO駆動のホットブロックアライメント(386/amd64)で追加1-1.5%の性能向上

### Go 1.24 (2025年2月リリース)

- **ジェネリック型エイリアス**: 完全サポート
- **`tool`ディレクティブ**: `go.mod`にツール依存関係を直接記述可能に。従来の`tools.go`ワークアラウンドが不要に
- **Swiss Tableマップ**: マップ操作が最大60%高速化。Datadogではマップメモリが726MiBから217MiBに約70%削減
- **`testing.B.Loop`**: 従来の`for i := 0; i < b.N; i++`パターンを置き換えるシンプルなベンチマークループ
- **`T.Context()` / `B.Context()`**: テスト・ベンチマークでのコンテキスト管理改善
- **`testing/synctest`(実験的)**: 並行コードテスト用の隔離された「バブル」環境
- **`GOCACHEPROG`**: 外部キャッシュプログラムのサポート（リモート/分散キャッシュ戦略に活用可能）
- **cgoアノテーション**: `#cgo noescape` / `#cgo nocallback`でcgo呼び出しのパフォーマンス改善
- **`GOAUTH`**: プライベートモジュール取得時の認証サポート

### Go 1.25 (2025年8月リリース)

- **コンテナ対応GOMAXPROCS**: Linux上でcgroup CPU帯域制限を自動検出。コンテナ環境でのGo実行が大幅改善。~30秒ごとに動的再チェック
- **`testing/synctest` GA化**: 実験的ステータスから正式APIに昇格
- **`encoding/json/v2`(実験的)**: `GOEXPERIMENT=jsonv2`で利用可能
- **Green Tea GC(実験的)**: `GOEXPERIMENT=greenteagc`でGCオーバーヘッドを最大40%削減
- **DWARFv5**: デバッグセクションサイズとリンク時間を削減

### Go 1.26 (2026年2月リリース)

- **Green Tea GCデフォルト化**: GCオーバーヘッドを最大40%削減するガベージコレクタが標準に
- **`new(expr)`構文**: 初期値式を持つ`new`関数のサポート
- **自己参照ジェネリック型**: ジェネリック型パラメータの自己参照が可能に
- **`go fix`リライト**: コードを自動的にモダナイズするサブコマンドの完全書き直し
- **cgoオーバーヘッド約30%削減**
- **`crypto/hpke`パッケージ**: 新しい暗号化プリミティブの追加
- **ゴルーチンリークプロファイル(実験的)**: `GOEXPERIMENT=goroutineleakprofile`でブロックされたゴルーチンを検出
- **Flame graphデフォルト表示**: `go tool pprof`のWebUIでフレームグラフがデフォルトビューに

### エラーハンドリング構文の議論終結

- 2025年6月、Goチームはエラーハンドリングの構文的な言語変更の追求を正式に終了し、関連するすべてのプロポーザルをクローズすることを発表

---

## 2. パフォーマンス改善

### Swiss Tableマップ (Go 1.24)

- マイクロベンチマークで挿入が約31.6%、ルックアップが約21.4%高速化
- Datadogの実例: マップメモリ726MiB→217MiB（約70%削減）、フリート全体で200TiBのRAM削減
- アプリケーション全体で約1.5%のCPU使用量改善

### Green Tea GC (Go 1.25実験的 → Go 1.26デフォルト)

- 大半のワークロードでGC CPU時間が約10%削減
- 高ファンアウトデータ構造（tile38ベンチマーク等）で約35%のGCオーバーヘッド削減
- Go 1.24のGCレイテンシ: 15ms→7ms（約53%改善）、メモリ使用量20%削減

### PGO (Profile-Guided Optimization)の実績

- Uber: フリート全体で24,000 CPUコアの節約を報告
- go-json: iTLBミス30%削減、パフォーマンス4%改善
- Go 1.23でビルドオーバーヘッドが一桁台に低下し、実用的な採用が加速

### コンテナ対応ランタイム (Go 1.25)

- GOMAXPROCS自動調整によりKubernetes環境でのCPUスロットリングとテールレイテンシを大幅改善
- cgroup v1/v2両対応

---

## 3. 人気フレームワーク・ライブラリ

### Webフレームワーク

| フレームワーク | GitHub Stars | 採用率 | 特徴 |
|---|---|---|---|
| **Gin** | ~88,000+ | 48% | デファクトスタンダード。最大のサードパーティミドルウェアエコシステム |
| **Fiber** | ~35,000+ | 11%(急成長) | v3リリース。net/httpネイティブサポート追加。スループットGinの約60%増 |
| **Echo** | ~30,000+ | 16% | 最も多くの組み込みミドルウェア。クリーンなAPI設計 |
| **Chi** | ~18,000+ | 成長中 | v5。外部依存ゼロ、stdlib純粋主義 |
| **Hertz** | ~6,700 | 新興 | ByteDance製。大規模マイクロサービス向け |
| **Encore** | - | 新興 | 統合開発プラットフォーム。インフラ管理・API文書自動生成 |

**注目**: Go 1.22で`net/http.ServeMux`にメソッドベースルーティングとワイルドカードパスパラメータが追加され、シンプルなアプリケーションでは外部ルーターが不要に。

### ORM・データベースライブラリ

| ライブラリ | GitHub Stars | アプローチ |
|---|---|---|
| **GORM** | ~39,800 | コードファースト、Active Recordパターン |
| **sqlc** | ~17,900 | SQLファースト、型安全なGoコード自動生成 |
| **ent** (Meta) | ~17,100 | コードファースト、グラフベースエンティティフレームワーク |
| **sqlx** | 安定 | `database/sql`拡張。軽量な中間層 |

### CLIフレームワーク

- **Cobra**: CLI開発のデファクトスタンダード。GitHub CLI (`gh`)、`kubectl`プラグイン等で使用
- **Bubble Tea** (Charmbracelet) v1.3: TUI開発のデファクトスタンダード。Elm Architecture準拠

### ロギング

- **log/slog** (Go 1.21+ stdlib): 新規プロジェクトの推奨デフォルト
- **zap** (Uber): 高パフォーマンス構造化ロギングの定番
- **zerolog**: 高パフォーマンス代替
- **logrus**: レガシー

### その他注目ライブラリ

- **Templ + HTMX**: Go + サーバーサイドレンダリング + 動的HTML。SPA代替として台頭
- **ConnectRPC**: gRPC/gRPC-Web/Connectプロトコル対応のスリムなRPCライブラリ
- **AWS SDK for Go v2**: v1は2025年7月31日にサポート終了。v2への移行が必須に

---

## 4. AI/LLMエコシステム

### 主要AIインフラプロジェクト（Go製）

| プロジェクト | GitHub Stars | 概要 |
|---|---|---|
| **Ollama** | 175,000+ | ローカルLLMランナー。Go最大のAIプロジェクト |
| **LocalAI** | 47,100+ | セルフホスト型OpenAI互換推論エンジン |
| **Weaviate** | 16,400+ | クラウドネイティブベクトルデータベース |
| **Milvus** | 30,000+ | ベクトルデータベース (Go + C++) |

### LLM統合ライブラリ

| ライブラリ | GitHub Stars | 概要 |
|---|---|---|
| **LangChainGo** | 9,500+ | LangChainのGoポート。10+プロバイダー統合 |
| **Eino** (ByteDance) | 12,000+ | Go向けLLMアプリケーションフレームワーク。10,000+ req/s最適化 |
| **GoAI SDK** | 151+ | Vercel AI SDK着想。25+プロバイダー統合。Go generics活用 |

### 公式プロバイダーSDK

- **OpenAI Go SDK** (`openai/openai-go`): ~2,800 stars。Go 1.22+必須
- **Anthropic Go SDK** (`anthropics/anthropic-sdk-go`): ~1,100 stars。Go 1.24+必須。v1.52.0
- **Firebase Genkit Go**: 1.0 GA。Google AI、Vertex AI、OpenAI、Anthropic、Ollama統合

### AIエージェントフレームワーク

- **Google ADK for Go** (`google/adk-go`): 8,200 stars。2025年11月リリース。マルチエージェントオーケストレーション、A2Aプロトコルサポート
- **Eino ADK**: ChatModelAgent、DeepAgentパターン。Human-in-the-loop対応
- **go-llm**: LLMベースエージェント構築フレームワーク

### MCP (Model Context Protocol) 実装

- **公式Go SDK** (`modelcontextprotocol/go-sdk`): 4,700 stars。Anthropic + Google共同メンテナンス
- **mcp-go** (`mark3labs/mcp-go`): 8,800 stars。コミュニティ実装の先駆者
- **golang.org/x/tools/internal/mcp**: Go公式ツールチーム内部パッケージ（将来のstdlibサポートの可能性）

### Go vs Python in AI

- **Pythonが引き続き優位**: AI/ML本番システムの70%以上がPythonベース
- **Goの強み**: AIインフラ層（推論サービング、エージェントオーケストレーション、APIレイヤー）
- **実用的な住み分け**: Python=モデル訓練・データサイエンス、Go=本番AIインフラ・推論サービング・エージェントオーケストレーション
- **2026年が転換点**: Go公式ブログでLLMアプリケーション構築記事を公開。主要プロバイダーの公式SDK出揃い

---

## 5. クラウドネイティブ・インフラストラクチャ

### コンテナエコシステム

#### Docker Engine v29 (2025年11月)

- containerdイメージストアがデフォルト化（レイジープル、P2P配布対応）
- **Goモジュールパス変更**: `github.com/docker/docker` → `github.com/moby/moby` へ移行が必須に
- nftablesファイアウォールバックエンド（実験的）

#### containerd 2.x

- 2.0でサンドボックスサービス、ユーザー名前空間、コンテナチェックポイント導入
- 最新: v2.3.2 (2026年6月)。Go 1.26.4でビルド

#### runc

- v1.4.0: OCI runtime-spec v1.3対応、cgroups v1非推奨化
- v1.5.0 (2026年6月): Go 1.25+必須。libpathrsデフォルト化。バイナリサイズ~16MB→~14MB

#### Podman 6.0 (2026年)

- **Goモジュールパス変更**: `github.com/containers/podman/v5` → `go.podman.io/podman/v6`
- cgroups v1、iptables、CNI、slirp4netnsの廃止
- AMD GPUサポート追加

### Infrastructure as Code

| ツール | 最新バージョン | 主要マイルストーン |
|---|---|---|
| **OpenTofu** | v1.12.2 | 2025年4月CNCF受理。状態暗号化、OCI対応、エフェメラルリソース |
| **Terraform** | v1.12.x | Plugin Framework GA。BSLライセンス |
| **Pulumi** | CLI v3.246+ | Go Provider SDK v1.0.0 GA。Go generics対応 |
| **Crossplane** | v2.3 | v2.0大規模リライト。2025年11月CNCF Graduation |
| **Terragrunt** | v1.x | v1.0リリース (2026年3月)。Stacks GA |
| **CDKTF** | アーカイブ | 2025年12月非推奨化。Go bindingsの更新停止 |

### Kubernetes エコシステム

- client-go: 引き続きKubernetes APIアクセスの公式Goクライアント
- OCI Runtime Spec v1.3.0 (2025年11月): FreeBSDサポート追加

---

## 6. ツーリング改善

### ビルドツール

- **`go build -json`** (Go 1.24): 構造化JSONビルド出力
- **`go run`/`go tool`キャッシュ** (Go 1.24): 繰り返し実行の高速化
- **`GOCACHEPROG`** (Go 1.24): リモート/分散キャッシュ戦略のサポート
- **`tool`ディレクティブ** (Go 1.24): `go.mod`でのツール依存管理

### リンター・静的解析

- **golangci-lint v2.0** (2025年3月): 設定構造の刷新、100+バンドルリンター、`golangci-lint fmt`コマンド追加。最新v2.12.2
- **Staticcheck 2026.1 (v0.7.0)** (2026年2月): Go 1.25/1.26対応。`new(expr)`サポート
- **govulncheck v1.1.4** (2025年1月): SARIF/VEX出力、15%高速化、SBOM対応

### テスティング

- **`testing/synctest`**: Go 1.24で実験的導入→Go 1.25でGA。並行コードテストの革新
- **`testing.B.Loop`** (Go 1.24): ベンチマークの新パターン
- **testify v1.11.1**: 引き続きGo最も広く使用されるアサーション/モックライブラリ
- **GoMock v0.6.0** (Uber fork): アーカイブモード追加
- **Ginkgo v2.3.0**: 大規模スイートのパフォーマンス30%改善

### デバッガー

- **Delve v1.27.0** (2026年6月): Go 1.27互換。フレームポインタアンワインド、ジェネリックメソッドサポート
- 2025-2026で10リリース。Windows ARM64、loong64、RISCV64アーキテクチャサポート追加
- DAPプロトコルの成熟（データブレークポイント、ヒット条件付きブレークポイント等）

### プロファイリング

- **Flame graphデフォルトビュー** (Go 1.26): `go tool pprof`のWebUIで標準に
- **ゴルーチンリークプロファイル** (Go 1.26実験的): GC到達可能性分析でリークしたゴルーチンを検出
- **Pyroscope 2.0** (2026年4月, Grafana Labs): 連続プロファイリングデータベースの再アーキテクチャ
- **Parca**: eBPFベースの常時オンプロファイラ。19回/秒のサンプリング、ほぼゼロオーバーヘッド

---

## 7. コミュニティ・エコシステム動向

### 開発者サーベイ 2025 (2025年9月実施)

- **満足度**: 91%が満足（62%が「非常に満足」）
- **AI利用**: 70%が週に数回以上AIツールをGo開発に使用。ただし満足度は55%にとどまる
- **エージェンティックAI**: プライマリモードとして使用しているのは17%のみ
- **AIをGoソフトウェアに組み込み**: 78%が組み込んでいない
- **エディタ**: VS Code 37%、GoLand 28%
- **主な不満**: イディオマティックなコードパターン(33%)、言語機能の不足(28%)、信頼できるモジュール発見(26%)

### 採用動向

- **TIOBE Index**: 2025年4月に#7（過去最高）到達。2026年1月に#16に下落（安定性ゆえの検索量低下が原因と分析）
- **Stack Overflow 2025**: C#・シェルスクリプトを抜いて7位に上昇
- **GitHub Octoverse 2024**: 3番目に成長が速い言語（Python、TypeScriptに次ぐ）
- **開発者人口**: 約410万人がプロフェッショナルにGoを使用。180-220万人が主要言語として使用（5年前から倍増）
- **将来の採用意向**: 11%の開発者が今後12ヶ月以内にGoの採用を計画

### 主要企業の採用状況

- **Technology**: Google、Apple、Salesforce、Datadog、Dropbox、HashiCorp
- **金融**: American Express、Monzo
- **運輸/物流**: Uber（約5,000万行のGoコード、約2,100のGoサービス）、Amazon、HelloFresh
- **ストリーミング**: Netflix

### 年収データ (USA, 2026年)

- 平均年収: $109,905 - $139,821
- 75パーセンタイル: $134,500
- 上位10% (トップアーナー): $150,500
- Glassdoor中央値（総報酬）: $171,000

### カンファレンス (2026年)

- **GopherCon US 2026**: Seattle, 8月3-6日
- **GopherCon Europe 2026**: 6月15-18日
- **GopherCon UK 2026**: London, 8月11-13日
- **GopherCon India 2026**: Hyderabad
- **GopherCon Israel 2026**: Tel Aviv

### GitHub人気プロジェクト

| プロジェクト | Stars | カテゴリ |
|---|---|---|
| Ollama | 175,000+ | AI/LLM |
| dive | 54,200+ | コンテナ |
| etcd | 51,900+ | 分散KVストア |
| lazydocker | 51,400+ | コンテナUI |
| go-ethereum | 51,200+ | ブロックチェーン |
| alist | 49,700+ | ファイル管理 |
| Terraform | 48,800+ | IaC |
| LocalAI | 47,100+ | AI/LLM |

### フレームワーク採用率の推移 (JetBrains 2025)

| フレームワーク | 2020年 | 2025年 | 傾向 |
|---|---|---|---|
| Gin | 41% | 48% | 上昇 |
| Gorilla | - | 17% | 安定 |
| Echo | - | 16% | 安定 |
| Chi | - | ~12% | 微増 |
| Fiber | 導入年 | 11% | 急成長 |
| Beego | - | 4% | 下降 |

---

## まとめ: 2025-2026の主要トレンド

1. **Green Tea GC**: Go 1.26でデフォルト化。GCオーバーヘッド最大40%削減はGoのパフォーマンス特性を大きく変える
2. **コンテナネイティブ化**: Go 1.25のGOMAXPROCS自動調整により、Kubernetes環境でのGo実行が劇的に改善
3. **AI/LLMエコシステムの急成長**: Ollama(175k stars)を筆頭に、Go製AIインフラが急拡大。公式SDK出揃い、2026年が転換点
4. **MCP (Model Context Protocol)**: 公式Go SDKリリース。AIエージェントのツール連携標準プロトコルとしてGoエコシステムに浸透
5. **Fiber v3**: net/httpネイティブサポート追加で最大の懸念が解消。Ginに迫る勢い
6. **golangci-lint v2**: メジャーバージョンアップ。設定構造の刷新、100+リンター統合
7. **`tool`ディレクティブ**: Go 1.24で追加。開発ツール管理の長年の課題を解決
8. **エラーハンドリング議論の終結**: Goチームが構文変更の追求を正式に終了
9. **コンテナツールのGoモジュールパス移行**: Docker/Moby、Podman共に大規模なimport path変更
10. **encoding/json/v2**: Go 1.25で実験的導入。標準ライブラリの大型進化

---

## 参考リンク

- [Go公式ブログ](https://go.dev/blog/)
- [Go 1.24 Release Notes](https://go.dev/doc/go1.24)
- [Go 1.25 Release Notes](https://go.dev/doc/go1.25)
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go Developer Survey 2025 Results](https://go.dev/blog/survey2025)
- [JetBrains Go Ecosystem 2025](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/)
- [State of Go 2026](https://devnewsletter.com/p/state-of-go-2026/)
- [Building LLM-powered applications in Go](https://go.dev/blog/llmpowered)
- [Swiss Tables in Go](https://go.dev/blog/swisstable)
- [The Green Tea Garbage Collector](https://go.dev/blog/greenteagc)
- [Container-aware GOMAXPROCS](https://go.dev/blog/container-aware-gomaxprocs)
