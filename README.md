# Git / GitHub の練習 & 個人的なメモ
このリポジトリは、個人的に Git と GitHub の練習をするために作成しました。  
Git の使い方や GitHub の仕様についてわかったことや、コミットルールなどの個人的なメモをここにまとめます。

## Gitコマンドチップ
### ローカルには残し、GitHub 上からは削除したい場合
`$ git rm --cached [ファイル名]` または `$ git rm -r --cached [ディレクトリ名]`  
ただし、歴史は残るので注意
### 過去に違うユーザ名やメールアドレスでプッシュしたものをすべて統一する場合 (歴史改竄)
`$ git filter-branch -f --env-filter "GIT_AUTHOR_NAME='ユーザ名'; GIT_AUTHOR_EMAIL='メールアドレス'; GIT_COMMITTER_NAME='ユーザ名'; GIT_COMMITTER_EMAIL='メールアドレス';" HEAD`  
`$ git push --force -u origin master`  
歴史が変わってしまうので注意  
参考: <a href="http://qiita.com/sea_mountain/items/d70216a5bc16a88ed932" target="_blank">Git の Commit Author と Commiter を変更する</a>
### 直前のコミットの変更点を上書きする
`$ git commit --amend`
`$ git push --force -u origin master`  
歴史が変わってしまうので注意  
コミットメッセージを書きかえたいときや新たなコミットとするまでもない変更をしたときに使う

## メモ
* ファイルを間違えてコミットしたあとに .gitignore を追加してもそのファイルは GitHub 上では表示されたまま
* .gitignore に dir/ や file.txt を書くと、全ての階層における同名のディレクトリやファイルが適用される
    * 特定のディレクトリやファイルだけ指定したいときは、git 管理下からの相対パスで表現する
* ディレクトリだけ追加して、中に何もファイルが存在しないとコミットできない
* ディレクトリの中にディレクトリが1つしか存在しない場合は、dir1/dir2 のように表示される
* rm や git rm でファイルを削除しても歴史は残る

## メールアドレスの有用性について
GitHub に登録しているメールアドレスを git config で登録しておかないと、リポジトリの Latest commit の部分の commiter の名前がリンクにならずにアイコンも表示されない。

さらに、適当なメールアドレスの設定でプッシュして、後から正しいメールアドレスでプッシュすると2人の contributors が存在することになって、メールアドレスを変更する前の contributor が example123 という適当な名前になってしまう。

また、メールアドレスが正しく設定されていないと、organization にコミットしたときに、自分のページに "Repositories contributed to" が表示されない。

結論: **メールアドレスは正しく設定しましょう！**

## コミットメッセージの個人的な絵文字ルール
* :tada: `:tada:` → Initial commit
* :sparkles: `:sparkles:` → 新規ファイルの追加
* :pencil2: `:pencil2:` → ファイルの修正
* :fire: `:fire:` → ファイルの削除
* :white_check_mark: `:white_check_mark:` → 意図した通りにコミットされているか確認 (このリポジトリのみ)
