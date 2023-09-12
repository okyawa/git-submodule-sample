# git moduleの検証

## 概要

- Gitサブモジュールは、Gitリポジトリ内に別のGitリポジトリを組み込むための仕組み
  - `git submodule` は、外部の `git` リポジトリを、自分の `git` リポジトリのサブディレクトリとして登録し、特定の `commit` を参照する仕組み
  - `submodule` として追加したディレクトリは、ただの外部リポジトリへ参照を記録した箱
  - `submodule` の外側は、その箱のどのコミットを参照しているのかを記録
  - `submodule` の内側は、基本的には、自分が操作したときにしか更新されない

## git submodule チートシート

### サブモジュールを追加

- `git submodule add` で追加し `git commit` で保存
  - `.gitmodules` ファイルが更新される

```sh
git submodule add <サブモジュールのリポジトリURL>
git commit -a
```

#### サブモジュール名を変更して追加したい場合

- 最後にパス指定を加える

```sh
git submodule add <サブモジュールのリポジトリURL> <ディレクトリ名>
git commit -a
```

### サブモジュールを含んだGitレポジトリを git clone

- `git clone` に `--recursive` オプションをつける

```sh
git clone https://example.com/repo-with-sub-modules --recursive
```

### サブモジュールの初期化

- サブモジュールを追加した後、初期化が必要

```sh
git submodule init
```

### 他の人がサブモジュール追加したものを反映 / git cloneした後からサブモジュールのファイルを取得

- `git submodule update --init` で、サブモジュールの更新と初期化を同時に行う
- 下記のコマンドを実行すると、 `.git/config` に `.gitmodules` の内容が自動的に書き込まれる
```sh
git submodule update --init
```

### サブモジュールのファイルを更新

```sh
git submodule update --remote
```

#### サブモジュール内のコードを更新

- サブモジュールのディレクトリに入り、 `git pull` で最新の履歴を反映

```sh
cd <サブモジュールのディレクトリ>
git pull
```

### サブモジュールがどのバージョンを指しているか確認

- サブモジュールの最新情報を取得
  - `git submodule foreach git fetch`
- サブモジュールのステータスを表示
  - `git submodule status`
  - ハッシュタグ・モジュール名・タグorブランチが表示される
```sh
git submodule foreach git fetch
git submodule status
```

### サブモジュールのブランチを切り替える

- サブモジュール内に入って別のブランチに切り替え、親側でコミットする
- 下記は、サブモジュール名が `sub-module` の場合の例
```sh
cd sub-module
git checkout main
cd ../
git commit -a
```

### サブモジュールを削除

- `git submodule deinit サブモジュール名` で、サブモジュールの登録解除
- 親のgit管理からサブモジュールを削除
  - `git rm サブモジュール名`
- 最後に、親のgit管理でコミットする
- 下記は、削除したいサブモジュール名が `sub-module` の場合の例
```sh
git submodule deinit sub-module
git rm sub-module
git commit -a
```
