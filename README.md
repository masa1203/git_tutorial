# Udemyもう怖くないGit

Githubはネット上のレポジトリホスティングサービス

## Gitの初期設定

```sh
$ git config --global user.name "masa1203"
$ git config --global user.email mail address
$ git config --global core.editor ""

# 確認方法
$ git config user.name
$ git config user.email
$ git config --list
```

## Gitの仕組み

- Gitはどうやって管理しているのか
    - Gitはスナップショットを記録する
    - 差分の計算だとブランチのマージに時間がかかる

- コミットをたどることで以前の状態に戻せる

## 操作の流れを掴む

ローカルは3つのエリアに分かれている
ワークツリー⇒ステージ⇒リポジトリ

- リポジトリ
    - スナップショットを記録 git commit
- ステージ
    - コミットする変更を準備 git add
- ワークツリー
    - ﾌｧｲﾙを変更する作業場

## Gitのデータの持ち方(コミットまでの流れ)

リポジトリに圧縮ファイル、ツリー、コミットファイルを作成することでをデータを保管している

コミットが親コミットを持つことで変更履歴をたどることができる

Gitの本質はデータを圧縮してスナップショットで保存している

Gitのコマンドはそのデータに対して色々な操作をしている

## リポジトリの作成

```
$ git init
$ ls .git/
config  description  HEAD  hooks  info  objects  refs
```

## Gitリポジトリのコピーを作成

```sh
$ git clone ~~
.gitとファイル内容をコピーすることができる
```

## 変更をステージに追加

ステージは何のためにあるのか？
⇒一部の変更だけをコミットしたいため。

```
$ git add <ファイル名>
$ git add <ディレクトリ名>
$ git add .
```

## 変更を記録する(コミット)

```sh
$ git commit
$ git commit -m "メッセージ"
$ git commit -v # 変更内容をエディター上で確認できる
```

コミットメッセージは変更内容と要点んを理由を一行で完結にかく。

## 変更状況を確認する

git status

## 変更差分を確認する

```sh
git diff
git diff ファイル名

# git add した後の変更分
git diff --staged
```
## 変更履歴を確認する

```sh
$ git log

# 1行で表示する
$ git log --oneline

# ファイルの変更差分を表示する
$ git log -p index.html

# 表示するコミット数を制限する
$ git log -n <コミット数>
```

## ファイルの削除を記録する

``` BASH
# ファイルごと削除
# ワークツリーからもリポジトリからも変更履歴を削除する
$ git rm <ファイル名>
$ git rm -r <ディレクトリ名>

# ワークツリーの変更は残しつつ、コミットしたファイルを削除したい
$ git rm --cached <ファイル名>
```

## ファイルの移動を記録する

ワークツリー上のファイル名を変更したとき、既にステージ上にあるデータについても名前を変更させたい

``` BASH
$ git mv <旧ファイル> <新ファイル>

# 以下のコマンドと同じ
# 旧ファイルを消して新しいファイルが追加されたことと同じ
$ mv <旧ファイル> <新ファイル>
$ git rm <旧ファイル>
$ git add <新ファイル>
```

## Githubにプッシュする

- リモートレポジトリを新規追加する

pushする際に毎回URLを打つのは大変なのでoriginという名前で置いておく。

``` BASH
# リモートレポジトリの追加
$ git remote add origin https://~~~

# 送信すうｒ
$ git push <リモート名> <ブランチ名>
$ git push -u origin master
```

`-u`を付けると今後はgit pushだけで`git push origin master`ができるようになる。

## コマンドにエイリアスを付ける

コマンドの入力を短縮して入力をラクにしたい。

--globalオプションはローカル環境全体に反映されるので
ローカルのみに反映したい場合は--localを使用する。

``` BASH
$ git config --global alias.ci commit
$ git config --global alias.st status
$ git config --global alias.br branch
$ git config --global alias.co checkout
```

# バージョン管理したくないファイルを扱う

パスワードなどの機密情報を載せたファイルは除く。
キャッシュファイルなどものぞく。

`.gitignore`ファイルにファイル名を追記する。

``` .gitignore
# #でコメントアウト

# 指定したファイルを指定
index.html

# ルートディレクトリを指定
/root.html

# ディレクトリ以下を除外
dir/

# /以外の文字列にマッチ「*」
/*/*.css  # 1つしたのディレクトリにある.cssファイル
```

## ワークツリーのファイルの変更を元に戻したい

``` BASH
# ワークツリーの状態をステージの状態と同じにする⇒変更を取り消している
$ git checkout -- ファイル名
$ git checkout -- <ディレクトリ名>

# 全変更を取り消す
$ git checkout -- .
```

## ステージした変更を取り消す

add した後に取り消したいとき。

``` BASH
$ git reset HEAD <ファイル名>
$ git reset HEAD <ディレクトリ>

# 全変更を取り消す
$ git reset HEAD .
```

リポジトリ内の最新のコミットをステージに反映してaddした内容を取り消している。

HEADは最新のコミットのこと。


## 直前のコミットをやり直したい

``` BASH
$ git commit --amend
```

リモートレポジトリにPushしたコミットはやり直してはダメ。
PUSHした後にコミットは修正してはならない。

現状のステージ内容を直前のコミットに追加する

## リモートリポジトリを確認

``` BASH
$ git remote

# 対応するURL
＄git remote -v
```

## リモートレポジトリを追加

リモートリポジトリは複数登録することができる

``` BASH
$ git remote add <リモート名> <リモートURL>

$ git remote add tutorial https://github.com/masa1203/git_remote_bak.git

$ git push -u bak master
```

## リモートから情報を取得する

### fetch

remotes/リモート/ブランチ　に反映される

``` BASH
$ git fetch <リモート名>

$ git fetch origin

$ git branch -a
* master
  remotes/bak/master
  remotes/origin/master

# ワークツリーの内容を切り替える
$ git checkout remotes/origin/master

# 変更をワークツリーに反映する
$ git merge origin/master
```

### pull

リモートから情報を取得してマージまでを一度にやりたいとき
ワークツリーの内容まで反映する。

``` BASH
$ git pull <リモート名> <ブランチ名>
$ git pull origin master

# 上記コマンドは省略可能
$ git pull

# git pull は下記コマンドと同じこと
$ git fetch origin master
$ git merge origin/master
```

## fetchとpullの使い分け

基本的にはfetchしてからmergeした方が安全
pullはカレントブランチに変更内容がマージされてしまう。

git pull origin hogeしてhogeブランチの内容を取ってきたのに
カレントブランチがmasterなのでmasterにhogeブランチの内容が反映されてしまう。

## リモートの情報を詳しくしる

``` BASH
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/masa1203/git_tutorial.git
  Push  URL: https://github.com/masa1203/git_tutorial.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

## リモートを変更・削除
リモート名を変更したい

``` BASH
# 変更する
$ git remote rename <旧リモート名> <新リモート名>
$ git remote rename tutorial new_tutorial

# 削除する
$ git remote rm <リモート名>
$ git remote rm new_tutorial
```

## ブランチとマージ

- ブランチ
    - 並行して複数機能を開発していくための仕組み。ブランチとはコミットを指すポインタ。
    - ブランチの作成や切り替え、マージが他のバージョン管理ツールより高速。
- マージ
    - 他の人が変更した内容を取得する

ブランチを分岐して開発していくと他の開発メンバーへの影響を与えないし、受けない

### ブランチの仕組み

ローカルリポジトリ: 圧縮ファイル + ツリー + コミット

コミットはスナップショット。それが時系列順に連なる。parentでたどることができる。

branchはコミットをさしたポインタと理解する。
HEADは今のブランチを指している。現在作業中のブランチへのポインタ。

### ブランチを作成する

``` BASH
$ git branch <ブランチ名>

# ブランチをつくるだけ。HEADは移動しない
$ git branch feature

$ git branch
  feature
* master

$ git log --oneline --decorate
b421a3c (HEAD -> master, origin/master, feature) Update home.html
b919f5a Create home.html
470429b git commit --amendを追記
76610a8 .gitignore ファイルを追加
5fcc3de git diffを追記
51fa15c git statusコマンドを追記
e69552d Initial commit

# ブランチの作成や切り替え
$ git checkout <既存ブランチ名>
$ git checkout feature

# ブランチを新規作成して切り替える
$ git checkout -b <新ブランチ名>
```

## 変更をマージする

他の人の変更内容を取り込む作業のこと。

``` BASH
$ git merge <ブランチ名>
$ git merge <リモート名/ブランチ名>

# 作業中のブランチにorigin/masterの内容をマージする
$ git merge origin/master
```

マージには3種類ある

- Fast Forward: 早送りになるマージ
    - ブランチが枝分かれしてなかったときはブランチのポインタを前に進めるだけ

- Auto Merge: 基本的なマージ
    - 枝分かれして開発していた場合、マージコミットという新しいコミットを作る
    - 親を二つもっている

- Conflict
    - 同じファイルの同じ行に対して異なる編集を行ったとき
    - ブランチをどちらを優先したらよいのかわからないから起きる

## コンフリクトが起きないようにするには？

運用ルールを決める。

- 複数人で同じファイルを変更しない
- pullやmergeする前に変更中の状態をなくしておく(commitやstachをしておく)
- pullするときは、pullするブランチに移動してからpullする
- コンフリクトしても慌てない

## ブランチ名の変更と削除

``` BASH
$ git branch -m <ブランチ名>

# 自分が作業しているブランチの名前を変更する
$ git branch -m new_branch

# 削除する
# masterにマージされていない変更が残っている場合は削除しない
$ git branch -d <ブランチ名>
$ git branch -d feature

# 強制削除する
$ git branch -D <ブランチ名>
```

## ブランチを分けた開発

masterブランチをリリース用ブランチに、開発はトピックブランチを作成して進めるのが基本
masterからブランチを切って、開発が進んでコミットできたらmasterにマージする。

## リモートブランチとは何か

リモートブランチとはリモートのブランチへのポインタ

``` BASH
$ git branch -a
* master
  remotes/origin/feature
  remotes/origin/master

# mergeするときはremotes/はいらない
$ git merge origin/master
```

## プルリクエスト

自分の変更したコードをリポジトリに取り込んでもらえるように依頼する機能。
レビューを挟んで更新されるようになる。

- ローカルのワークツリーのmasterブランチを最新に更新する
- ブランチを作成する
- ファイルを変更して
- 変更をコミット
- Githubへプッシュして
- プルリクエストを送る
- コードレビュー
- プルリクエストをマージ
- ブランチを削除


## Github Flowの流れ

1. masterブランチからブランチを作成
2. ファイルを変更しコミット
3. 同名のブランチをGithubへプッシュ
4. プルリクエストを送る
5. コードレビューし、masterブランチにマージ
6. masterブランチをデプロイ

- masterブランチは常にデプロイできる状態に保
- 新開発はmasterブランチから新しいブランチを作成してスタート
- 作成した新しいブランチ上で作業しコミットする
- 定期的にPushする
- masterにマージするためにプルリクエストを使う
- 必ずレビューを受ける
- masterブランチにマージしたらすぐにデプロイする
    - テストとデプロイ作業は自動化

開発フローをシンプルにしたい。

## リベースとはなんなのか

変更を統合する際に、履歴をきれいに整えるために使うのがリベース
他のブランチの変更履歴を取り込むことができる

``` BASH
$ git rebase <ブランチ名>
```

``` BASH
# masterブランチで何か作業する
$ git checkout master
$ git pull
$ git status
$ type nul > master2.html  # ファイルをさくせい　
$ git add .
$ git commit -m "master2を作成"

# featureブランチを作成してブランチを移動
$ git checkout -b feature
$ type nul > feature2.html  # ファイルを作成
$ git add .
$ git commit -m "feature2を作成"

# featureブランチからmasterブランチの変更を履歴と一緒に取得する
$ git rebase master
Successfully rebased and updated refs/heads/feature.

$ git log --oneline

84abb71 (HEAD -> feature) feature2を新規作成
# ここでmasterブランチでのコミットが取得できていることがわかる
24dd0c7 (master) master2.htmlを新規作成
59cfb11 (origin/master) Merge pull request #2 from masa1203/github_flow
bc9ace2 (origin/github_flow) Github flowを追記
948fa9f Merge pull request #1 from masa1203/pull_request
c8b6002 (origin/pull_request) pull request を追記
0debebb Merge branch 'feature'
17817ac conflictを追記
33f4c9b コンフリクトを追記
e368d1e Merge branch 'feature'
4e8f542 featureを追記
98b76b8 git merge を追記
0985498 master.htmlを新規作成
149d1fe (origin/feature) feature.htmlを新規追加
b421a3c Update home.html
b919f5a Create home.html
470429b git commit --amendを追記

# masterブランチからfeatureブランチへFast Forwordする
$ git checkout master
$ git merge feature

$ git push origin master

```

GithubにPushしたコミットはリベースしない。
git push -f は絶対にNG。

## リベースとマージはどちらを使うのか

- merge
    - コンフリクトの解決が比較的簡単
    - マージコミットが沢山あると履歴が複雑化する
    - 作業の履歴を残したい場合

- rebase
    - 履歴を綺麗に保つことができる
    - コンフリクトの解決が若干面倒(コミットそれぞれに解消が必要)

pushしていないローカルの変更にはリベースを使う。
Pushしたあとはマージを使う。
コンフリクトしそうならマージを使う。

## Pullの設定をrebaseに変更する

- pullにはマージ型とリベース型がある。

- Pullのマージ型
    - マージするのでマージコミットが残る。

``` BASH
$ git pull <リモート名> <ブランチ名>
$ git pull origin master
```

- Pullのリベース型
    - マージコミットが残らない
    - Githubの内容を取得したいだけの時は--rebaseを使用する

``` BASH
$ git pull --rebase <リモート名> <ブランチ名>
$ git pull --rebase origin master
```

- プルをリベース型に設定する
``` BASH
# 全てのpullをリベース型にする
$ git config --global pull.rebase true

# masterブランチがgit pullするときだけリベース型にする
$ git config branch.master.rebase true
```

## リベースで履歴を書き換える

コミットを綺麗に整えてからPushしたいときは履歴を書き換えよう
GithubにPushしていないコミットのみ

複数のコミットをやり直したい

``` BASH
$ git rebase -i <コミットID>

# コミットエディターが立ち上がって変更
$ git rebase -i HEAD~3
git logとは逆順に表示される

$ git rebase -i HEAD~3

squashでひとつ前のコミットと一緒にまとめられる

$ git rebase --continue
```

## タグ

コミットを参照しやすくするためにわかりやすい名前を付けるのがタグ。
よくリリースポイントに使う。

``` BASH
# タグの一覧
$ git tag

# パターンを指定してタグを表示
$ git tag -l "202006"
```

### タグの追加

基本的に注釈付きタグを使用する。

- 注釈付き版 annotated
    - 名前とコメント、署名を付けられる
    - -aオプションで注釈付きタグ
    - -mオプションを付けるとエディタを立ち上げずにメッセージを入力できる

``` BASH
$ git tag -a [タグ名] -m "[メッセージ]"
$ git tag -a 20170520_01 -m "version 01"
```

- 軽量版 lightweight
    - -aオプションが無い場合は軽量版タグを作成する
    - 名前だけ付けられる

``` BASH
$ git tag [タグ名]
$ git tag 20170520_01

# 後からタグ付けする
$ git tag [タグ名][コミット名]
$ git tag 20170520_01 8as93sa
```

### タグのデータを表示する

``` BASH
$ git show [タグ名]
$ git show 20170520_01

$ git tag -a 20200630 -m "version 20200630"

$ git show 20200630
tag 20200630
Tagger: masa1203 <mail_address@gmail.com>
Date:   Tue Jun 30 18:33:04 2020 +0900

version 20200630

commit d29cba3daf1bdf79a70f58c3f554dc31bc9a6824 (HEAD -> master, tag: 20200630, origin/master)
Author: masa1203 <mail address>
Date:   Tue Jun 30 18:18:06 2020 +0900

    third.htmlを追加

diff --git a/third.html b/third.html
new file mode 100644
index 0000000..e69de29
```

### タグをリモートレポジトリに送信する

git push コマンドで別途追加する必要がある

``` BASH
$ git push [リモート名][タグ名]
$ git push origin 20200630_01

# タグを一斉に送信する
$ git push origin --tags
```

### スタッシュ

作業が途中でコミットしたくないけど急遽別のブランチで作業しないといけない。
そういうときに作業を一時避難する。

``` BASH
# stashは隠すという意味
$ git stash
$ git stash save

# 避難した作業の一覧
$ git stash list
stash@{0}: WIP on master: d29cba3 third.htmlを追加
# stash@{1}: これが増えていく

# 最新の避難した作業の復元
$ git stash apply
# 最新のステージの状況も復元する
$ git stash apply --index

# 特定の作業を復元する
$ git stash apply [スタッシュ名]
$ git stash apply stash@{1}

# 最新の避難した作業を削除
$ git stash drop

# 特定の作業を削除する
$ git stash drop [スタッシュ名]
$ git stash drop stash@{2}

# 全作業を削除する
$ git stash clear
```

