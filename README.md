# K_Z_U — Portfolio

つくった道具の記録。**https://k-z-u.github.io/**

macOS アプリ、デスクトップ環境、MCP サーバー、Minecraft サーバーの運用。
18 件を Featured 5 件 + Archive 13 件で並べた 1 ページのサイトです。

---

## つくり

- **ビルド不要の静的サイト。** HTML 1 枚と JPEG 23 枚だけ。npm も CDN も使っていません
- **依存はフォントのみ** — Google Fonts（Instrument Serif / Schibsted Grotesk / Zen Kaku Gothic New / JetBrains Mono）。
  読み込めない環境ではシステムフォントに落ちます
- **ルーティングは JS。** 一覧と詳細を同じページで切り替えます（URL は変わりません）

```
index.html          ページ全部（HTML / CSS / データ / スクリプト）
assets/*.jpg        各プロジェクトの画像。*-kv.jpg はキービジュアル、他はスクリーンショット（JPEG 82）
おしゃしん/         変換前の元画像。.gitignore で除外
```

## ローカルで見る

`index.html` を直接開いても動きますが、`file://` だと画像まわりで
ブラウザの制限に当たることがあるのでサーバー経由を推奨します。

```bash
python3 -m http.server 4173
```

## プロジェクトを足す・直す

`index.html` の `PROJECTS` 配列 1 か所だけ触れば済みます。

```js
{ title: "MineSSH", year: "2026.07", role: "設計・実装", stack: "Tauri · Rust · TypeScript",
  img: "assets/minessh.jpg",
  kv: "assets/minessh-kv.jpg",                                // キービジュアル（任意）。詳細の最上部と一覧に使う
  lede: "Minecraft サーバーの更新を、壊さずに終わらせる。",   // 一覧と詳細の見出し下
  caption: "デプロイ前の差分確認",                            // 図版の傍注（fig.01 — …）
  body: "…",                                                  // 詳細ページの本文
  facts: [["形式", "…"], ["中身", "…"]] }                     // 詳細ページの表
```

- 配列の**上から `FEATURED_COUNT` 件**（現在 5）が Featured、残りが Archive です。
  順番を入れ替えるだけで昇格・降格できます
- スクリーンショットは長辺 1800px 程度に落としてから `assets/` へ。macOS なら:
  ```bash
  sips -s format jpeg -s formatOptions 82 -Z 1800 元画像.png --out assets/名前.jpg
  ```
- **KV があるプロジェクト**（`kv` フィールドあり）は、詳細ページ最上部に KV を出し、
  `img` のスクリーンショットはその下の Preview セクションに出る。KV 自体は
  元サイズ（1672 / 1536px）から上げずに変換する:
  ```bash
  sips -s format jpeg -s formatOptions 82 KV画像.png --out assets/名前-kv.jpg
  ```
- **Featured の図版は cover で切り抜かれます**（アーチ型のため）。
  スクリーンショット全体は詳細ページ側で原寸比率のまま出ます

## 意図的にそうしてあるところ

- **reveal アニメーションは `.js` クラスで囲ってある。**
  JS が落ちても本文が `opacity: 0` のまま消えません
- **タブが裏に回ると IntersectionObserver が止まる**ので、`visibilitychange` で
  もう一度流しています。これがないと「戻ってきたら本文が真っ白」が起きます
- **図版のアーチは 190px 止まり。** 999px の半円だと UI スクリーンショットの
  上端を食ってしまうため、弧を残したまま浅くしています
- 右上のボタンは WebAudio のアンビエント音。既定は off です

## 公開

`main` に push すると GitHub Pages がそのまま配信します（ビルドなし）。

## クレジット

デザインは Claude Design の "Marginalia" をベースに、静的 HTML として実装し直したもの。
掲載しているのは自作のプロジェクトのみで、既製品（ComfyUI、Irodori-TTS 本体など）は含みません。
