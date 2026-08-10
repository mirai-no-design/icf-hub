# 公開とDOI取得の手順

3リポジトリ（icf-hub / icf-registry / icf-exchange-spec）をGitHubで公開し、ZenodoでDOIを取得するまでの手順。

## 1. GitHub Organizationの作成

1. GitHubにログインし、右上「＋」→「New organization」→ Freeプラン
2. 組織名：`mirai-no-design`（会社として発信するため、個人アカウントではなくOrganizationを推奨）
3. 組織のプロフィールに会社HP（mirai-no-design.co.jp）を設定

## 2. リポジトリの作成とプッシュ

3つのリポジトリを「Public」で作成し、それぞれのフォルダの中身をプッシュする。

```bash
# 例：icf-registry の場合（icf-hub / icf-exchange-spec も同様）
cd icf-registry
git init
git add .
git commit -m "ICF Registry v0.1.0 (Draft): initial public release"
git branch -M main
git remote add origin https://github.com/mirai-no-design/icf-registry.git
git push -u origin main
```

各リポジトリのSettingsで、Issues を有効にしておく（コメント募集の受け口）。

## 3. Zenodo連携（DOIの自動発行）

ZenodoはCERNが運営する無料の研究成果リポジトリで、GitHubのリリースにDOIを自動発行できる。

1. https://zenodo.org にアクセスし、「Log in」→「Log in with GitHub」でログイン（GitHubアカウントの認可を求められる）
2. 右上のアカウントメニュー →「GitHub」ページを開く
3. リポジトリ一覧が表示されるので、`mirai-no-design/icf-hub`・`icf-registry`・`icf-exchange-spec` の3つのスイッチを **ON** にする
   - 一覧に出ない場合は「Sync now」を押す。Organizationのリポジトリには管理者権限が必要
4. これで準備完了。**以後、GitHubでリリースを作るたびに、Zenodoが自動でアーカイブしてDOIを発行する**

各リポジトリに置いてある `.zenodo.json` が、Zenodo上のタイトル・著者・ライセンス等のメタデータになる（公開前に著者名・所属を実名に更新すること）。

## 4. リリースの作成（＝DOI発行の瞬間）

1. GitHubのリポジトリページ →「Releases」→「Create a new release」
2. Tag: `v0.1.0`、Title: `v0.1.0 (Draft)`、説明にはこの版の要約を書く
3. 「Publish release」を押す
4. 数分後、Zenodoの「Upload」一覧に自動でエントリが作られ、DOIが発行される

## 5. DOIの種類と使い分け（重要）

Zenodoは2種類のDOIを発行する。

| 種類 | 例 | 用途 |
|---|---|---|
| **Concept DOI**（全バージョン共通） | 10.5281/zenodo.1234567 | 「このプロジェクトを引用する」とき。READMEやHPにはこちらを載せる |
| Version DOI（各リリース固有） | 10.5281/zenodo.1234568 | 「v0.1.0のこの記述を引用する」とき。論文で特定版を引くときはこちら |

発行されたら、各READMEの冒頭にDOIバッジを追加する（ZenodoのページにバッジのMarkdownが表示される）。

```markdown
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)
```

## 6. 公知化（防衛的公開）としての意味

- DOI発行時点で、公開日時が第三者（CERN）により証明される
- 以後、この内容と同一のアイデアを他者が特許で囲い込むことはできなくなる（先行技術化）
- 逆に、公開した内容は自社でも特許化できなくなる。**リリース前に、特許化したい要素が含まれていないか最終確認すること**

## 7. HPからのリンク

みらいのでざいんHPに「オープン仕様（ICF HUB）」ページを1枚作り、次を掲載する。

- 3層の説明（README冒頭の要約でよい）
- GitHubの3リポジトリへのリンク
- Concept DOI（引用方法）
- 「コメント・接続のご相談はGitHub Issuesまたはお問い合わせへ」

## 8. 更新の運用

- 仕様の変更はPull Request → 議論 → mainへマージ → 節目でリリース（新しいVersion DOIが発行される）
- READMEの表・CITATION.cffの `version` と `date-released` をリリース時に更新する
- 大きな設計変更はIHRとして提案してから本文に反映する（IHR-000参照）
