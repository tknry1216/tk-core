# Go言語トレンドサマリー

**更新日時:** 2026年5月14日

---

## バージョンリリース

### Go 1.24（2025年2月リリース）
- **ジェネリック型エイリアス**: 型パラメータ付きの型エイリアスが正式サポート
- **Swiss Table ベースの map 実装**: ハッシュマップの内部実装が刷新され、パフォーマンスが大幅に向上
- **弱参照ポインタ（weak pointers）**: `weak.Pointer` 型の導入
- **`runtime.AddCleanup`**: ファイナライザの改善版として新 API を追加
- **`os.Root`**: サンドボックス化されたファイルシステムアクセスを実現
- **`go.mod` の tool ディレクティブ**: ツール依存関係を `go.mod` で直接管理可能に
- **実験的 `testing/synctest` パッケージ**: 並行処理テストの支援
- **FIPS 140-3 準拠サポート**: 暗号化モジュールの標準準拠
- **ポスト量子 TLS 鍵交換（X25519MLKEM768）**: 量子コンピュータ耐性のある鍵交換アルゴリズムを導入
- **`go build` / `go install` に `-json` フラグ追加**: 構造化 JSON 出力が可能に
- **GC インクリメンタルポーズ時間 15-25% 改善**

### Go 1.25（2025年8月リリース）
- **コンテナ対応 `GOMAXPROCS`**: cgroup の CPU リミットを自動的に尊重
- **実験的 `encoding/json/v2`**: `MarshalWrite` / `UnmarshalRead` によるストリーミング対応の新 JSON パッケージ
- **ジェネリクスから「コア型（core types）」概念を削除**: 明示的な型セットルールに置き換え、仕様を簡素化
- **`testing/synctest` の正式化**
- **`go build -asan` の強化**: アドレスサニタイザの機能拡充
- **`go mod ignore` ディレクティブの追加**
- **`go doc -http` サーバーの新設**
- **実験的 GC・FlightRecorder の追加**

### Go 1.26（2026年2月リリース）
- **`new(T)` で初期値式を受け付け可能に**: `new(int, 42)` のような記述が可能
- **自己参照ジェネリック型パラメータ**: より柔軟なジェネリクス定義が可能に
- **Green Tea GC がデフォルトで有効化**: 1.25 で実験的に導入された新 GC が正式採用
- **cgo オーバーヘッド約 30% 削減**
- **`go fix` のリライト**: モダナイザアナライザによるコード自動更新
- **実験的 SIMD パッケージ（`simd/archsimd`）**: 高性能な数値計算への足がかり
- **`runtime/secret` パッケージ（実験的）**: 秘密情報の安全なメモリ消去
- **ゴルーチンリークプロファイリング**: セキュリティ・可観測性の強化
- **Go 1.26.1（2026年3月）**: 暗号化・HTML テンプレート・URL パッケージ等のセキュリティ修正

---

## ジェネリクスの進化

Go 1.18 で導入されたジェネリクスは、着実に進化を続けている。

| バージョン | 変更点 |
|-----------|--------|
| Go 1.24 | ジェネリック型エイリアスのサポート |
| Go 1.25 | 「コア型」概念の削除、型セットルールへの簡素化 |
| Go 1.26 | 自己参照ジェネリック型パラメータ |
| 2026年3月 | **ジェネリックメソッドが承認**（Robert Griesemer 提案） |

特に 2026年3月に承認された**ジェネリックメソッド**は、長年の FAQ で否定的な立場が示されていた機能であり、Go のジェネリクスにとって画期的な変更となる。現在実装に向けて作業が進行中。

---

## パフォーマンス改善

- **Swiss Table map**（1.24）: map 操作全般で計測可能な高速化を実現
- **GC インクリメンタルポーズ**（1.24）: 15-25% の改善
- **Green Tea GC**（1.25 実験 → 1.26 デフォルト）: 新世代の GC アルゴリズム
- **cgo オーバーヘッド削減**（1.26）: 約 30% の改善で Go-C 間の呼び出しが高速化
- **スライスバッキングストアのスタック割り当て拡大**（1.26）: ヒープ割り当ての削減

---

## エコシステム・コミュニティ動向

### 言語の人気と採用

- **TIOBE ランキング 7位**（2025年4月時点、過去最高）
- 2024年に Python・TypeScript に次いで **3番目に成長が速い言語**
- JetBrains の 2025年調査で **開発者の 11% が Go の採用を予定**

### Web フレームワーク

| フレームワーク | 特徴 |
|--------------|------|
| `net/http`（標準ライブラリ） | Go 開発者の 32% が直接使用 |
| Gin | 最も人気のあるサードパーティフレームワーク |
| Echo | 高速・ミニマリスト |
| Fiber | Express.js ライクな API |

### 開発ツール

- **`golangci-lint`**: 100 以上のリンターを統合した標準的なリンターランナー
- **イテレータ**（Go 1.23 で導入）: ジェネリクスと組み合わせた idiomatic な map/filter パターンが普及
- **`encoding/json/v2`**: 長年の API 課題を解決する新 JSON パッケージ

### AI/ML 統合

- **LangChain Go**: Go での LLM アプリケーション開発
- **kServe**: スケーラブルな AI バックエンドサービス
- AI 駆動の開発ツール・バックエンドサービスにおける Go の採用が拡大中

### コミュニティイベント（日本国内）

Go Junction、layerx.go、Go Connect 等のコミュニティイベントが東京・京都・横浜・滋賀など全国各地で活発に開催されている。

---

## 注目すべき動向

1. **ジェネリックメソッドの承認**: Go のジェネリクスの大きなマイルストーン
2. **SIMD 実験パッケージ**: 高性能数値計算への関心を示す
3. **`runtime/secret` とゴルーチンリークプロファイリング**: セキュリティと可観測性への継続的な投資
4. **ポスト量子暗号**: 量子コンピュータ時代に向けた先進的な対応
5. **Green Tea GC の正式採用**: パフォーマンスとメモリ管理の大幅な改善

---

## 参考リンク

- [Go 1.24 Release Notes](https://go.dev/doc/go1.24)
- [Go 1.24 Released (Go Blog)](https://go.dev/blog/go1.24)
- [Go 1.25 Released (Go Blog)](https://go.dev/blog/go1.25)
- [Go 1.25 interactive tour](https://antonz.org/go-1-25/)
- [What is New in Go 1.25 (freeCodeCamp)](https://www.freecodecamp.org/news/what-is-new-in-go/)
- [Go 1.26 Release Notes](https://go.dev/doc/go1.26)
- [Go 1.26 Released (Go Blog)](https://go.dev/blog/go1.26)
- [Go 1.26 Features overview](https://saraikin.com/posts/go-1-26-features/)
- [Go Ecosystem in 2025 (JetBrains)](https://blog.jetbrains.com/go/2025/11/10/go-language-trends-ecosystem-2025/)
- [Golang Popularity in 2026 (ZenRows)](https://www.zenrows.com/blog/golang-popularity)
- [Generic Methods Approved for Go](https://www.theregister.com/2026/03/02/generic_methods_go/)
- [Popular Go Web Frameworks (JetBrains)](https://blog.jetbrains.com/go/2026/04/28/popular-golang-web-frameworks/)

---

*このサマリーは自動生成されました。*
