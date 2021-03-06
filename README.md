# Camlup

![Camlup](http://www.afghanistan-today.org/media/slideshows/camel_slide2.jpg)

http://www.afghanistan-today.org/article/?id=814

CamlupはOCamlへのトランスレータ言語です。

Camlupを使う事でC言語ライクな文法でOCamlのプログラムを記述出来ます。

## ビルド&インストール

### Mac OSX

あらかじめ、OCamlをインストールしておきます。

#### ビルド

ocamlとUnix環境のmakeがある環境で以下のコマンドを実行します。

    $ make

と入力します。

#### インストール

    $ make install

と実行すると、/usr/local/binにnmlcがコピーされインストールが完了します。

### Windows

1. OCamlをインストールします。
2. nmlc.exeを以下のURLからダウンロードして、パスを通します。

https://github.com/hsk/camlup/blob/master/nmlc.exe?raw=true

※新しいWindowsの場合は認証が必要です。
問題は無いはずですが、ウィルスチェックを行ってから実行してください。
プログラムはソースを読み込みmlファイルを出力し、ocamlを実行するのみです。

## Hello World

```
open Printf
printf("hello world!\n")
```

hello.nml

```
$ ./nmlc -run examples/hello.nml
hello world!
```

## 特徴

- 美しいカリー化された関数の定義、呼び出し
- ネイティブなコンパイル
- 高速コンパイル
- 高速な処理系のビルド


以下のように関数f5を定義して、呼び出す事が出来ます:

```
f5 = {
 | a b c d e => a + b + c + d + e
}

printf("%d\n" f5(1 2 3 4 5))
```

この関数をOCamlで書くと以下のようになります:
```
let f5 = (
  fun a b c d e -> a + b + c + d + e
)
printf "%d\n" (f5 1 2 3 4 5)
```

タプルを使った関数

```
f = {
 | a,b => a + b
}
printf("%d\n")(f(1,2))
printf("%d\n" f(1,2))
```

タプルを２つ受け取るカリー化関数

```
f2 = {
  | a,b c,d => a*b + c*d
}
printf("%d\n")(f2(1,2)(3,4))
printf("%d\n" f2(1,2 3,4))
```

## 実装状況

- [ ] リテラル
    - [x] 文字列
    - [x] 整数
    - [x] 浮動小数点数
    - [x] 文字列のエスケープシーケンス
    - [ ] 文字
- [x] 変数
- [x] if else 式
- [x] match 式
    - [x] int,string,list,variant,tuple
    - [x] ネストしたマッチ
    - [x] match式のas
- [x] for,while式
- [x] パーシャルファンクション
- [x] 配列
    - [x] 値設定
    - [x] リテラル？コンストラクタ？
    - [x] 値取得
- [x] レコード
- [ ] 代数データ型
    - [x] 大まかな機能
    - [ ] 型パラメータ
    - [ ] mutableなフィールド
    - [ ] 値の設定
- [x] ブロック
- [x] 名前付き引数
- [ ] オブジェクト指向
    - [x] とりあえず動かす
    - [x] private
    - [x] mutable
- [ ] モジュール
    - [x] とりあえず動かす
    - [x] Map等が使えるようにする
    - [ ] シグニチャ
- [ ] 行番号の埋め込み
    - [x] 式
    - [ ] 型
    - [ ] 宣言
    - [ ] より詳細に
- [ ] その他
    - [x] バージョン管理する
    - [x] リリースバージョンを作成する
    - [ ] 全般的なテスト
    - [ ] ドキュメント
        - [ ] windowsについて
        - [ ] dexpとrmlについて
    - [ ] チュートリアル
    - [ ] makeとmake install を分ける

## サンプル

シンプルなサンプル

- [examples/hello.nml](examples/hello.nml)
- [examples/fib.nml](examples/fib.nml)

詳細な動作

[examples/test.nml](examples/test.nml)とその変換結果[examples/test.ml](examples/test.ml) を参考にしてください。

# ドキュメント

- [チュートリアル](docs/tutorial)
- [言語仕様](docs/spec)
- [Camlup ドキュメント](docs)

# License

MIT Licence
