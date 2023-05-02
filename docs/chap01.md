# 初めてのGo言語｜1章

## 1.3 go　コマンド

### 1.3.1 go run

Go 言語はコンパイル言語だが、バイナリファイルが一時ディレクトリに置かれ、実行後に削除されている.

```shell
$ go run hello.go
```

### 1.3.2 go build

再利用できるようにバイナリファイルを作りたい場合

```shell
$ go build hello.go
```

### 1.3.3 go mod

モジュールの初期化

```shell
# プロジェクト名
$ mkdir hello-world
$ cd hello-world
$ go mod init hello-world
```

-> 9章 モジュールとパッケージ で説明

```shell
# 必要なライブラリの DL や不要なライブラリの削除
$ go mod tidy

# go.mod ファイルがあるディレクトリを解析してコンパイルしてビルドしてくれる
# go.mod ファイルに宣言されたモジュール名に対応するコマンド `hello-world` が作成される
$ go build

$ ./hello-world
皆さん、こんにちは！
```

### 1.3.4 go install

リポジトリで共有されたツールをインストール e.g. hey

```shell
$ go install github.com/rakyll/hey@latest
```

### 1.3.5 コードのフォーマット

goimports は go fmt の機能強化版

```shell
# -l : フォーマットが正しくないファイルをコンソールに表示
# -w : ファイルを直接書き換える
$ goimports -l -w .
```

### セミコロン挿入規則

コンパイラが改行前の最後のトークンによって `;` が自動的に挿入する

## 1.4 lint と vet

Go の開発者は

- [Effective Go](https://go.dev/doc/effective_go)
- [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

を読むべき

```shell
# メソッドに渡す引数の数を間違えたり
# 使われない変数に値を代入したり
# といったエラーを検知
$ go vet
```

```shell
# スタイルガイドに即しているかの linter
$ staticcheck
```

複数のツールを同時に実行できる

```shell
$ golangci-lint run
```

## 1.6 Makefile

Go の開発者たちは make を採用しているらしい

自分的な Makefile
```shell
.DEFAULT_GOAL := run

IMAGE_NAME=hoge-repo
PORT=8080

tidy:
	go mod tidy

fmt: tidy
	goimports -local https://github.com/fuga-org/hoge-repo -w .

lint: fmt
	staticcheck

vet: lint
	go vet

build: vet
	docker build -t ${IMAGE_NAME} .
	docker image prune -f

run: build
	docker run --env-file .env -p ${PORT}:${PORT} --rm ${IMAGE_NAME}
```
