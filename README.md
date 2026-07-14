# yharuto-presentation-skill-for-slidev Skill


`yharuto-presentation-skill-for-slidev` Skillで定義されているパステルカラーのデザインシステムを、Slidev（Vite/Vue製のWebベース・プレゼンテーションツール）でも利用できるようにしたAgent Skillです。
Marpは1つの自己完結したMarkdownファイル + インライン`<style>`で完結しますが、Slidevはプロジェクト構成であり、`<style>`タグはそのスライド1枚にしかスコープされません。そのためこのSkillでは、デザインをグローバルCSS（`styles/index.css`）として切り出し、Slidev側の`.slidev-layout`クラス構造に合わせて移植しています。

実際にSlidevプロジェクトを立ち上げてブラウザ上で描画を確認済みです（ヘッドマターとスライド1のフロントマターの統合、`h2`直後要素のマージン適用における`!important`の必要性など、実機検証で見つかった点を反映済み）。

## How to use
npx skills

```bash
npx skills add yharuto0917/yharuto-presentation-skill-slidev yharuto-presentation-skill-for-slidev
```

gh skill install

```bash
gh skill install yharuto0917/yharuto-presentation-skill-slidev yharuto-presentation-skill-for-slidev
```
```
```
```

## 📁 ディレクトリ構成

```text
yharuto-presentation-skill-for-slidev/
├── README.md                  # 本ドキュメント
└── skills/
    └── yharuto-presentation-skill-for-slidev/
        ├── SKILL.md                # スキル本体（セットアップ手順とデザイン移植の設計判断）
        └── assets/
            ├── index.css           # 移植済みデザインシステム本体。<project>/styles/index.css にコピーして使用
            └── global-bottom.vue   # ページ番号フッター（Marpのpaginate: trueに相当、任意）
```

## デザインの特徴

- パステルカラーのアクセントボーダー（上部ピンク・下部ミント、タイトルスライドはグラデーション）
- Noto Sans JP + Fira Code フォント（Slidevの`fonts:`ヘッドマターでGoogle Fontsを自動読込）
- 本文スライドはh2見出しを上部固定バー風にスタイリング（タイトルスライドのみh1使用）
- シンタックスハイライト付きコードブロックにもピンクのアクセントボーダー

## 使い方

Claude Codeから`~/.claude/skills/yharuto-presentation-skill-for-slidev`（シンボリックリンク経由で`~/.agents/skills/yharuto-presentation-skill-for-slidev`を参照）としてSkillを呼び出すと、`skills/yharuto-presentation-skill-for-slidev/SKILL.md`の手順に従ってSlidevデッキにこのデザインを適用します。
