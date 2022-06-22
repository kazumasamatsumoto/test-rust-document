# ドキュメントの内容

Rust のドキュメントを作って、Github Pages に公開する方法

## 手順１

mdbook というツールをインストールします。

```terminal
cargo install mdbook
```

バージョンを確認しましょう。

```terminal
mdbook --version
```

## 手順２

Github のリポジトリを準備します。

## 手順３

mdbook を作っていきます

```terminal
mdbook init
```

## 手順５

プロジェクトのルート階層から.github というディレクトリを作ります。

```terminal
mkdir .github
```

## 手順 6

.github のディレクトリに移動します。

```terminal
cd .github
```

## 手順 7

actions と workflows のディレクトリを作成します

```terminal
mkdir actions
mkdir workflows
```

## 手順 8

actions に移動します

```terminal
cd actions
```

## 手順 9

build ディレクトリを作ります

```terminal
mkdir build
```

## 手順 10

build ディレクトリに移動します

```terminal
cd build
```

## 手順 11

Dockerfile を作成します

```terminal
touch Dockerfile
```

## 手順 12

Dockerfile の中身を書きます(保存を忘れずに)

```Dockerfile
FROM rust:latest

RUN cargo install mdbook --no-default-features --features output --vers "^0.3.5"

CMD ["mdbook", "build"]
```

## 手順 13

.github まで戻ります

```terminal
cd ../../
```

## 手順 14

workflows に移動します

```terminal
cd workflows
```

## 手順 15

main.yml ファイルを作成します

```terminal
touch main.yml
```

## 手順 16

main.yml の中身を記入します（コピペでもいいですが、変えれるところがあればどうぞ）

```yml
name: CI/CD
on:
  push:
    branches:
      - main
    paths:
      - "src/**"
      - "book.toml"

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup mdBook
        uses: peaceiris/actions-mdbook@v1
        with:
          mdbook-version: "0.4.8"
          # mdbook-version: 'latest'

      - run: mdbook build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./book
```

## 手順 17

リポジトリを設定します。（ここは github のリポジトリを作った時のコマンドを参照するようにしてください）

## 手順 18

add commit push を実施します

```terminal
git add .
git commit -m "first-commit"
git push
```

## 手順 19

Github のアクションのページで動いているか確認します

## 手順 20

Github Pages を有効化にします。
ブランチは`branch: gh-pages`に設定してください

## 手順 21

うまく行っていることを願っています。
おしまい。

その時に rust-jp のスラックで質問並びに有志の方が丁寧に回答していただいてくれています
こちらも参考にしてください。

[参考ページ](https://rust-jp.slack.com/archives/C0DJCNRPC/p1655874493959109)
