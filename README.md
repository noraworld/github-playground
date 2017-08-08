# Git / GitHub の練習 & 個人的なメモ
このリポジトリは、個人的に Git と GitHub の練習をするために作成しました。  
Git の使い方や GitHubの仕様についてわかったことや、コミットルールなどの個人的なメモをここにまとめます。

## Gitコマンドチップ

### ローカルには残し、GitHub 上からは削除したい場合
`$ git rm --cached [ファイル名]` または `$ git rm -r --cached [ディレクトリ名]`

ただし、歴史は残るので注意

### 過去に違うユーザ名やメールアドレスでプッシュしたものをすべて統一する場合 (歴史改竄)
`$ git filter-branch -f --env-filter "GIT_AUTHOR_NAME='ユーザ名'; GIT_AUTHOR_EMAIL='メールアドレス'; GIT_COMMITTER_NAME='ユーザ名'; GIT_COMMITTER_EMAIL='メールアドレス';" HEAD`

`$ git push --force -u origin master`

歴史が変わってしまうので注意  
参考: [Git の Commit Author と Commiter を変更する](http://qiita.com/sea_mountain/items/d70216a5bc16a88ed932)

### 直前のコミットの変更点を上書きする
`$ git commit --amend`  
`$ git push --force -u origin master`

歴史が変わってしまうので注意  
コミットメッセージを書きかえたいときや新たなコミットとするまでもない変更をしたときに使う

### 直前のコミットを取り消す
`$ git reset --hard HEAD^`

歴史が変わってしまうので注意

### 直前のコミットに戻る (svn revert に相当)
`$ git checkout [ファイル名]`  

参考: [Git の変更を元に戻す](https://www.atlassian.com/ja/git/tutorial/undoing-changes)

### Git と Subversion の対応表
[Git, git-svn, svnコマンド対応表](http://qiita.com/sugarshin/items/0394e9ad867951dd74fe)

## Issueを立てるときの注意点
Issueは一度立てると削除することはできないし、タイトルも必ず変更履歴が残って削除することができない。正直この仕様は良くない。

タイトル入力のときに間違ってリターンキー押しただけの単純なミスでも永久に間違いが残り続けるので、Issueを取り扱うときは十二分に注意すること！！

## メモ
* ファイルを間違えてコミットしたあとに .gitignore を追加してもそのファイルは GitHub 上では表示されたまま
* .gitignore に dir/ や file.txt を書くと、全ての階層における同名のディレクトリやファイルが適用される
    * 特定のディレクトリやファイルだけ指定したいときは、git 管理下からの相対パスで表現する
* ディレクトリだけ追加して、中に何もファイルが存在しないとコミットできない
* ディレクトリの中にディレクトリが1つしか存在しない場合は、dir1/dir2 のように表示される
* rm や git rm でファイルを削除しても歴史は残る
* 空のコミットをしたいときは、`git commit --allow-empty -m "Commit message"`
    * ただし、ファイルの追加や変更があったときにaddしてしまうと--allow-emptyでも変更が反映されてしまうので注意
    * 逆を言うと、addしなければファイルの追加や変更があっても--allow-emptyで変更を反映させずに空コミットをすることができる

## メールアドレスの有用性について
GitHub に登録しているメールアドレスを git config で登録しておかないと、リポジトリの Latest commit の部分の commiter の名前がリンクにならずにアイコンも表示されない。

さらに、適当なメールアドレスの設定でプッシュして、後から正しいメールアドレスでプッシュすると2人の contributors が存在することになって、メールアドレスを変更する前の contributor が example123 という適当な名前になってしまう。

また、メールアドレスが正しく設定されていないと、organization にコミットしたときに、自分のページに "Repositories contributed to" が表示されない。

結論: **メールアドレスは正しく設定しましょう！**

## コミットメッセージの個人的な絵文字ルール
* :tada: `:tada:` → Initial commit
* :sparkles: `:sparkles:` → 新しいファイルや機能の追加
* :pencil2: `:pencil2:` → 機能の修正
* :art: `:art:` → コードのフォーマットの修正
* :fire: `:fire:` → ファイルや機能の削除
* ~~:white_check_mark: `:white_check_mark:` → 意図した通りにコミットされているか確認 (このリポジトリのみ)~~

:warning: このリポジトリはGit/GitHubの使い方や仕様を確認することがほとんどであるため、`:white_check_mark` を使うとほとんどがこの絵文字になってしまうので46コミットめでやめました。

参考: [Emoji cheat sheet fot GitHub](http://www.emoji-cheat-sheet.com)

## READMEから画像を参照するとき
![参照できるかチェック](https://github.com/secondnoraworld/github-playground/blob/3578784ee1fcfbab8dd3242652042c7121330709/octocat.png)

該当する画像を削除してしまうと当然参照することができなくなる。しかしその画像を削除する前のコミットの画像を参照すれば削除してしまった画像でもREADMEから参照することができる。

## git alias を設定する際の注意点
エイリアスを使用すると Git のサブコマンドを自由に設定することができるが、**既存のサブコマンドを上書きすることはできない**。たとえば、以下のように `push` サブコマンドを上書きすることはできない。

```bash
$ git config --global alias.push '!echo "Pushing into branch $(git rev-parse --abbrev-ref HEAD)..." && git push -u origin $(git rev-parse --abbrev-ref HEAD)'
```

この状態で `git push` すると、通常通りの `git push` が実行されてしまうので注意！
