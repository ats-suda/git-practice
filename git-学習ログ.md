# Git / GitHub 学習ログ（Claudeとのやり取りまとめ）

Gitをまったくの初心者から学んだときのQ&Aを、質問と要点の形でまとめたもの。
セットで、手順をまとめた資料「[Git と GitHub のはじめかた](./git-github-はじめかた.md)」もある。

---

## 1. Git と GitHub って何？ 用語がわからない

- **Git** … ファイルの変更履歴を何度もセーブできる道具（ゲームのセーブ機能）。
- **GitHub** … そのセーブデータをネット上に置く倉庫＋みんなで共同作業する場所。
- 主な用語：リポジトリ（入れ物フォルダ）、コミット（1回セーブ）、プッシュ（アップロード）、プル（最新を取り込む）、クローン（まるごとコピー）、ブランチ（作業の枝）、マージ（合体）、プルリクエスト（取り込みのお願い）。
- 基本の流れ：`clone → 編集 → commit → push`、更新の受け取りは `pull`。

## 2. インストール〜最初のコマンドの手順書がほしい（Windows）

- 手順書を作成 → 「はじめかた」資料に集約。
- 大枠：Gitインストール → 名前/メール設定 → `git init` → `git add` → `git commit` → `git push`。

## 3. GitHub Desktop の導入手順は？

- コマンドが苦手なら、ボタン操作でcommit/push/pullできる無料アプリ。
- desktop.github.com からインストール → GitHubアカウントと連携 → リポジトリ作成 → Summary記入 → Commit → Publish。

## 4. 作ったファイルの公開設定はどうなってる？

- ファイル単位ではなく、**リポジトリ全体の Public / Private 設定**で決まる。
- 作成前はローカルにあるだけで非公開。push して初めてネットに出る。
- New repository画面は初期状態が **Public**（＝世界に公開）のことが多いので注意。
- あとから変更：Settings → 一番下 Danger Zone → Change repository visibility。

## 5. CAD（SOLIDWORKSなど）のファイルは管理できる？

- 技術的には管理できる（バージョン保存・復元は可能）。
- でも**バイナリ**なので、①差分が見えない ②マージできない ③リポジトリが重くなる、と相性は△。
- 対策：**Git LFS**（大きなバイナリ用）、チームなら **SOLIDWORKS PDM**（チェックアウト方式）。

## 6. Gコードや STEP 形式ならいける？

- **Gコード（.nc/.gcode）** … 中身がテキスト。Gitと相性◎（差分もマージもOK）。
- **STEP（.step/.stp）** … 中身はテキストだが機械的。保存には向くが差分は読みにくく、編集を続ける元データには不向き（書き出し後の形式なので設計履歴が失われる）。
- 目安：**テキストほどGit向き、バイナリほど相性が悪い**というグラデーション。

## 7. Word / Excel / PowerPoint（.docx等）はバイナリじゃないの？

- 正体は「**XML（テキスト）を ZIP で固めたもの**」。中身はテキストだが、圧縮されているのでGitには**バイナリのように見える**。
- 保存・復元はできるが、行単位の差分・マージは基本できない（△）。
- 昔の .doc/.xls/.ppt は本物のバイナリ。

## 8. パワポで図の位置・サイズを変えたのも管理できる？

- 「バージョンとして保存・復元」はできる。
- ただし「どう動いたか」を分かりやすく見るのは苦手（座標がXMLに数字で入っているだけ）。
- 見た目の変化を追うなら、PowerPointの「比較」機能や OneDrive/SharePoint のバージョン履歴が向く。

## 9. MATLAB 関係（.m / .mat / .slx など）の相性は？

- MATLABはGit連携が標準装備で、主役の **.m はテキスト＝相性◎**。
- **△になるもの**：.slx（Simulink）、.mlx（ライブスクリプト）、.mat（データ）、.fig、.mlapp など（ZIP/バイナリ）。
- 純正の**比較ツール／3-wayマージ**でその弱点を補える。R2025a以降はライブスクリプトを**テキストの.m形式**でも保存でき、差分が読める。

## 10. MATLAB Agentic Toolkit / MCP Server を使えば △ は解決する？

- **しない**。これらはAIエージェントをMATLABにつなぎ、コードを書く/実行/テスト/解析する道具（主に .m 側）。
- △を実際に埋めるのは、それとは別の **MATLAB純正の差分・マージ連携**（Comparison Tool + mlDiff/mlMerge）をGitに設定すること。
- エージェントは「変更を説明」「形式変換」などの補助として上乗せで効く。

## 11. 今の名前・メールの確認と上書き方法

- 確認：`git config --global user.name` / `... user.email`（何も出なければ未設定）。一覧は `git config --global --list`。
- 上書き：同じ設定コマンドをもう一度打つだけ（値は1つなので自動で置き換わる）。
- 範囲は2種類：`--global`（PC全体）と、フォルダ内で `--global` を外して打つ**リポジトリごと**の設定（こちらが優先）。

## 12. `git status` の応答の意味

- `On branch master`：今いるブランチ名。`No commits yet`：まだ未コミット。
- `Untracked files:`：Gitがまだ管理していないファイル。`git add` で追跡開始できる、というヒント付き。
- エラーではなく、順調に進んでいるサイン。

## 13. `git add` で出た `LF will be replaced by CRLF` の警告

- **エラーではなく注意書き**。改行をWindows式(CRLF)にそろえるというお知らせで、ファイルは壊れない。
- 気になれば一度だけ `git config --global core.autocrlf true`。

## 14. コミット成功メッセージの意味

- `[master (root-commit) e1c0cc0] ...`：初回コミット成功。`root-commit`は一番最初の印、`e1c0cc0`はコミットID。
- `1 file changed, 1 insertion(+)`：1ファイルで1行追加。`create mode ... memo.txt`：新規登録。

## 15. `git log` の読み方

- `commit <長いID>`：フルのコミットID（短縮版が先頭7文字）。
- `(HEAD -> master)`：現在地の矢印がそのコミットを指している。
- `Author` / `Date`：誰が・いつ。下にコミットメッセージ。
- 1行表示は `git log --oneline`。抜けるときは `q`。

## 16. New repository 画面には何を入れる？

- Owner/Repository nameはそのまま、Descriptionは任意（空でOK）。
- **visibility**（Public/Private）を選ぶ。
- ⚠️ すでにローカルでcommit済みなら、**README / .gitignore / license は追加しない**（追加すると初回pushがぶつかる）。

## 17. 作成後のコマンドをコピペし忘れた

- 何も壊れていない。リポジトリのページを再度開けば同じコマンドが出る。
- または `git remote add origin <URL>` → `git branch -M main` → `git push -u origin main` を打てばOK。
- `remote origin already exists` が出たら `git remote set-url origin <URL>`。

## 18. `git push` 成功メッセージの意味

- `* [new branch] main -> main`：GitHub側にmainができ、内容が入った。
- `branch 'main' set up to track 'origin/main'.`：ローカルとGitHubのmainがひもづき、次回から `git push` だけでOK（`-u`のおかげ）。

## 19. `git add .` の「.」も必要？

- 必要。`.` は「今いるフォルダ（ここ）」を表す記号で、`git add .` は「変更ぜんぶを含める」の意味。
- `git add memo.txt` は特定ファイルだけ。`add` とドットの**間に半角スペース**が必要。

## 20〜21. 日常サイクルを実施 → status / log の結果

- `nothing to commit, working tree clean`：全部保存済みの良い状態（編集したのに出たら上書き保存し忘れの可能性）。
- `Your branch is up to date with 'origin/main'`：ローカルとGitHubが同期済み。
- `git log --oneline` にコミットが積み上がっていれば成功。
- メッセージは「更新」「2回目」より「memoに◯◯を追記」など**中身がわかる一言**が良い。

## 22. 前のバージョンに戻すには？（正式名称は「コミット」）

- 戻す先はコミットID（`git log --oneline`で確認）。
- **方法1（安全・推奨）**：`git restore --source=<ID> ファイル名` → 確認して add/commit/push。
- **方法1.5**：`git show <ID>:ファイル名` で中身を覗くだけ。
- **方法2**：`git revert <ID>` … 打ち消しコミットを追加（push済みでも安全）。
- ⚠️ `git reset --hard` は履歴が消えるので当面使わない。

## 23. `echo "..." > memo.txt` は上書き？ 追記したい

- `>`（矢印1本）は**上書き**、`>>`（矢印2本）は**末尾に追記**。
- 日記のように書き足すなら `>>`、または素直にエディタで書いて上書き保存。
- `>` `>>` はGitではなくターミナル共通の書き方。

## 24. 差分（diff）画面の見方・色分け

- **赤（−）＝削除された行**、**緑（＋）＝追加された行**、色なし＝変更なし。
- `@@ -1 +1,2 @@`：変更場所の見出し（−が変更前、＋が変更後）。`+2 -1`：2行追加・1行削除の要約。
- テキストファイルをGitで管理する最大のメリット。バイナリ（.docx/.slx等）では差分が見えない。

## 25. このやり取り自体もGitHubに残したい

- やることは同じ：残したい内容を**ファイルにして git-practice フォルダに置き、`add → commit → push`**。
- このログ（.md）と「はじめかた」資料の2つを入れておくと、学習の記録＋手引きの両方が残る。

## 26. push前の個人情報チェック（IDは入れて大丈夫？）

- Publicにpushすると中身は世界に公開されるので、**push前に名前・メール・秘密情報が入っていないか確認**するクセをつける。`grep -rn "探したい文字" .` で検索できる。
- **コミットID（96c1a98 など）は入れてOK** … ただの識別番号で、pushすれば履歴として公開されるもの。秘密情報ではない。
- 作者欄に本物のメールを出したくなければ `git config --global user.email "ユーザー名@users.noreply.github.com"`（GitHubの匿名用メール）に変える。
- ※この学習ログと「はじめかた」資料には、実名・本物のメール・ユーザー名は入っていないことを確認済み。

## 27. AI（Cowork）と Claude Code の違い

- **Cowork** … 隔離された作業環境で動く。資料作り・ファイル整理が得意。GitHubへのpushは代われない（認証情報を持たず、ターミナルに入力もできない）。ファイルは「フォルダを接続」して直接置ける。
- **Claude Code** … 自分のPCの本物のターミナルで動く。自分のGit設定・GitHubログインを使えるので commit〜push まで自動でできる。
- 流れの基本：「AIが作る → 自分のリポジトリに置く →（自分の権限で）push → 公開前に中身を確認」。
- → 詳細は別ファイル「AIとGitを使うときのメモ.md」。

## 28. Gitはフォルダを常時監視していない／中断と再開

- Gitは `git status` などの**コマンドを打った瞬間だけ**動く。履歴は `.git` フォルダに**ファイルとして保存**され、Git Bashはただの入力窓。
- **Git Bashを閉じてもPCをシャットダウンしても、コミット済みのものは何も失われない**（全部ディスクに残る）。
- 再開は「Git Bashを開く → `cd ~/Desktop/git-practice` → 作業 → add/commit/push」だけ。プロンプト末尾に `(main)` が出ていれば正しい場所。
- `git init` / 名前・メール設定 / `remote add` は一度やれば保存済みなので繰り返し不要。

## 29. トップページの説明文＝README.md

- GitHubは、リポジトリ**直下の `README.md`** の中身をトップページに自動表示する。説明が出ないのはREADMEが無いだけ。
- 作り方：直下に `README.md` を作る → Markdownで書く → `add / commit / push`。
- ファイル名は必ず `README.md`。既にローカルでコミット済みのものをpushするなら、新規作成画面の「Add README」はオフにして自分で作る（ぶつからないため）。

---

### 資料をリポジトリに入れる手順

1. `git-学習ログ.md` と `git-github-はじめかた.md` を `~/Desktop/git-practice` フォルダに保存する。
2. Git Bash で:

```bash
cd ~/Desktop/git-practice
git status          # 2つの .md が Untracked で出る
git add .
git commit -m "学習ログとはじめかた資料を追加"
git push
```

3. GitHubの `git-practice` ページを再読み込みして、2ファイルが表示されれば成功。
