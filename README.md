# K_Z_U — Portfolio

つくった道具の記録。**https://k-z-u.github.io/**

macOS アプリ、デスクトップ環境、MCP サーバー、Minecraft サーバーの運用。
24 件を Featured 5 件 + Archive 19 件で並べた 1 ページのサイトです。

---

## つくり

- **ビルド不要の静的サイト。** HTML 1 枚と JPEG 29 枚だけ。npm も CDN も使っていません
- **依存はフォントのみ** — Google Fonts（Poppins / Zen Kaku Gothic New）。
  読み込めない環境ではシステムフォントに落ちます
- **ルーティングは JS。** 一覧と詳細を同じページで切り替えます（URL は変わりません）
- **差し色はプロジェクトごと。** `PROJECTS` の `accent` がその項目の見出し・ボタン・
  次の作品カードなどに `--ac` として流れ込む。色は各 KV から採った

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

## ページ構成

1. **Hero** — 濃紺グラデーション＋緑・藍の光。巨大な `K_Z_U`
2. **Contents** — 濃紺パネルに Featured 5 件の番号付きリスト（アンカー）
3. **Index** — 全 24 件の横一覧表。名前・概要・技術構成・年が 1 行でわかり、クリックで詳細へ
4. **Work** — Featured 5 件。各プロジェクトは **KV 全面タイトルカード** →
   リード文・facts・スクショカード。`詳しく見る` で詳細ビューへ
5. **Archive** — 残り 19 件のサムネカードグリッド
6. **About** — `Hello!` + スキルタイル + Now / Next + 連絡先
7. **Footer** — 濃紺。連絡先

## プロジェクトを足す・直す

`index.html` の `PROJECTS` 配列 1 か所だけ触れば済みます。

```js
{ title: "MineSSH", year: "2026.07", role: "設計・実装", stack: "Tauri · Rust · TypeScript",
  img: "assets/minessh.jpg",
  kv: "assets/minessh-kv.jpg",                                // キービジュアル（任意）。タイトルカードと詳細の最上部に使う
  accent: "#4E9455",                                          // この項目の差し色
  lede: "Minecraft サーバーに SSH で安全にデプロイする macOS アプリ。転送前の検証と、失敗時の自動ロールバックを備える。",   // 一覧と詳細の説明文
  caption: "デプロイ前の差分確認",                            // スクショカードの注釈
  body: "…",                                                  // 詳細ビューの本文
  facts: [["形式", "…"], ["中身", "…"]] }                     // facts テーブル
```

- 配列の**上から `FEATURED_COUNT` 件**（現在 5）が Featured、残りが Archive です。
  順番を入れ替えるだけで昇格・降格できます
- スクリーンショットは長辺 1800px 程度に落としてから `assets/` へ。macOS なら:
  ```bash
  sips -s format jpeg -s formatOptions 82 -Z 1800 元画像.png --out assets/名前.jpg
  ```
- **KV があるプロジェクト**（`kv` フィールドあり）は、Featured のタイトルカードと
  詳細ビュー最上部に KV を出し、`img` のスクリーンショットは Preview として下に添える。
  KV 自体は元サイズ（1672 / 1536px）から上げずに変換する:
  ```bash
  sips -s format jpeg -s formatOptions 82 KV画像.png --out assets/名前-kv.jpg
  ```
- **Featured の図版は cover で切り抜かれます**（アーチ型のため）。
  スクリーンショット全体は詳細ページ側で原寸比率のまま出ます

## 意図的にそうしてあるところ

- **reveal アニメは `.js` クラスで囲ってある。**
  JS が落ちても本文が `opacity: 0` のまま消えません
- **タブが裏に回ると IntersectionObserver が止まる**ので、`visibilitychange` で
  もう一度流しています。これがないと「戻ってきたら本文が真っ白」が起きます
- **明るい KV の上でも白文字が読める**よう、タイトルカードの下側に
  濃いグラデーションと文字影を重ねてあります

## 公開

`main` に push すると GitHub Pages がそのまま配信します（ビルドなし）。

## クレジット

デザインは Tuoi Bong の Behance ポートフォリオ
（2D Game Artist PORTFOLIO - Bong, 2024）を参考に、開発者向けの静的 HTML として再構成したもの。
掲載しているのは自作のプロジェクトのみで、既製品（ComfyUI、Irodori-TTS 本体など）は含みません。
