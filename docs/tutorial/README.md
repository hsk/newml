# チュートリアル

  TODO:

  - [ ] 全体をとりあえず書く
    - [x] 1 はじめに
    - [x] 2 インストール
    - [x] 3 Hello world
    - [x] 4 算術式とカリー化、改行、セミコロン
    - [x] 5 コメント
    - [x] 6 open
    - [x] 7 変数
    - [x] 8 ブロック
    - [ ] 9 プリミティブ int float string
    - [ ] 10 bool if else
    - [ ] 11 関数
    - [x] 12 参照
    - [ ] 13 ループ
    - [ ] 14 再帰関数
    - [ ] 15 tuple
    - [ ] 16 パターンマッチ
    - [ ] 17 クロージャ
    - [x] 18 パーシャルファンクション
    - [ ] 19 リスト
    - [ ] 20 レコード
    - [ ] 21 代数データ型
    - [ ] 22 モジュール
    - [ ] 23 クラス
    - [ ] 24 多相ヴァリアント
    - [x] 25 キーワード引数
  - [ ] 問題を洗い出す
    - [ ] コメントは分ける

  - [ ] 修正する
  - [ ] 仕様変更に対応する

## 1. はじめに

  newmlはOCamlのトランスレータ言語です。
  OCamlの多くのチュートリアルはOCamlTopのREPLで実行するのですが、ここではファイルに保存してコンパイルして実行するスタイルで説明します。

## 2. インストール

  ocamlがインストールされた状態で、git cloneしてmake make installでインストールします。
  TODO:トップのインストールをコピって来る。

    $ make
    $ make install

## 3. Hello world

  `hello.nml`というファイル名で以下の内容を保存します。

    // hello.nml
    print_string("Hello world!\n")

  `hello.nml`ファイルのあるパスに移動し、以下のコマンドを実行します。

    $ nmlc -run hello.nml


  すると、`hello.ml`ファイルが生成され、ocamlコマンドで実行されます。

    $ nmlc -run hello.nml
    Hello world!
    $

  newmlでは多くのC言語系統の言語と同じように関数呼び出しに()が必須です。


    // hello02.nml
    print_string "Hello world!\n"

  OCamlでは以上のようなプログラムが実行出来ますが、newmlでは、２つの式 `print_string` と `"Hello world!\n"`のように認識されます。そのため、何も起こりません。

    $ nmlc -run hello02.nml
    $

  TODO: 保存されたmlについて書く

## 4. 算術式とカリー化、改行、セミコロン

  `arith.ml`と言うファイルを作成し以下の内容を記述して保存します。

    Printf.printf("%d\n")(1)
    Printf.printf("%d %d %d %d\n")(1)(2)(3)(4)
    Printf.printf("%d %d %d %d\n" 1 2 3 4)
    Printf.printf("%d\n" 1+1)
    Printf.printf("%d\n" 1+2*3)
    Printf.printf("%d\n"; -1-2+10/5)
    Printf.printf("%d\n"
    -1)

  実行すると

    $ nmlc -run arith.ml
    1
    1 2 3 4
    1 2 3 4
    2
    7
    -1
    -1
    $

  と表示されます。Printf.printfを使うとC言語のprintfと同じような出力フォーマットで出力する事が出来ます。
  C言語系統の言語と違って、引数の区切り文字`,`が無い事に注意してください。
  newml(OCaml)では、printf関数はカリー化された関数です。したがって、

    Printf.printf("%d\n")(1)

  のように記述する必要があります。これはこれでなじみのある分かりやすい記述です。
  しかし以下のように、４つの引数があった場合を考えるととても括弧が増えてしまいます。

    Printf.printf("%d %d %d %d\n")(1)(2)(3)(4)

  そこで、newmlではこの括弧を省略する事が出来ます。

    Printf.printf("%d %d %d %d\n" 1 2 3 4)

  いくつも括弧を記述する事はなくスッキリしました。

    Printf.printf("%d\n" 1+1)

  足し算等の式を書く事も出来ます。

    Printf.printf("%d\n" 1+2*3)

  かけ算と足し算ではかけ算が優先されます。

    Printf.printf("%d\n" -1-2+10/5) // error

  マイナスが手前に来た場合、`"%d\n"`と `-1`が接続してしまいます。
  このような場合、`;`を間に記述する事で前の式との接続を切断する事が出来ます。

    Printf.printf("%d\n"; -1-2+10/5)

  newmlでは改行も、`;`と同じ意味を持つので

    Printf.printf("%d\n"
        -1)

  のように記述しても接続しないように出来ます。

## 5. コメント

  コメントは`/*`から始まり`*/`で終わるブロックコメントと
  `//` から改行までの１ラインコメントがあります。
  ブロックコメントはネストする事が出来ます。

    /* コメント */
    /* /*nested comment*/ */

  １行コメント配下のように書きます。

    // 1 line comment

## 6. open

  openを使うと、モジュールが読み込まれ、モジュール名を指定しなくても使う事が出来るようになります。

    // open.nml
    open Printf
    Printf.printf("%d\n" 10)
    printf("%d\n" 10)

## 7. 変数

  変数は 変数名 : 型 = 値 で宣言して初期化出来ます。

    variable : int = 2

  型推論があるので、(OCamlが型推論するだけですが)a=1のように記述する事が出来ます。

    a = 1
    b = 2

  使用しない値は`_ = 値`とすることで捨てる事が出来ます。

    _ = 3

## 8. ブロック

  newmlでは`{}`で括って複数の式を連続して記述出来ます。

    block : unit = {
      printf("test 1\n")
      printf("test 2\n")
    }

  型推論があるので、以下のように書く事も可能です。

    block = {
      printf("test 1\n")
      printf("test 2\n")
    }

  ブロックの値は最後に書いた式の値です。
  ブロック内では、改行しない場合も、`;`で区切る事無く連続して記述する事が可能です。

    block = {
      printf("test 1\n") printf("test 2\n")
    }

  例えば、以下のプログラムは、`a=1`で`b=1+2=3`のマイナス値計算結果`-3`が表示されます。

    _ = {
      a = { a=1 b=a+2; -b }
      printf("a=%d\n" a)
    }

  マイナスのような演算子を結合させたくない場合は`;`を使うか以下のように改行してください。

    _ = {
      a = {
        a=1
        b=a+2
        -b
      }
      printf("a=%d\n" a)
    }

## 9. プリミティブ int float string

  整数のための演算子はC言語と同じように使う事が出来ます。
  OCamlのキーワード等を等を考えると制限がある事に注意してください。

  - `+` 加算
  - `-` 減算
  - `*` 乗算
  - `/` 商
  - `%` 剰余
  - `<<` 左シフト
  - `>>` 右シフト
  - `<` `>` `<=` `>=` 比較演算子
  - `==` `===` `!=` `!==` 
  - `^` XOR
  - `|` OR
  - `&` AND
  - `||` OR
  - `&&` AND

  `==`はOCamlの`=`で`===`がOCamlの`==` `!=`が `<>` です。
  排他的論理和が`^`である事以外はCと同じです。

  TODO:優先順位を確認し直す。

  浮動小数点数の演算は、OCamlの`+.` `-.` `*.` `/.` `**` 演算子が使えます。

    // floats.nml
    open Printf
    floats = {
      n = 1.234
      m = n +. 10.5
      printf("%f\n" m)
      m = n -. 1.1
      printf("%f\n" m)
      m = n *. 0.1
      printf("%f\n" m)

      m = n /. 0.1
      printf("%f\n" m)

      m = -. n
      printf("%f\n" m)

      m = n ** 2.0
      printf("%f\n" m)

    }

  文字列は、""で括って記述します。現状手抜きで"から"までが文字列でそのままOCamlに丸投げしています。

    "abc"

  そのため、文字列内に"を記述する事が出来ません。  TODO:文字列をちゃんとする。

  文字列は`~`を使って結合します。OCamlの`^`ではない点に注意してください。

    // string.nml
    open Printf
    strings = {
      s = "abc"
      printf("%s\n" s)
      printf("%s\n" "abc" ~ "def")
    }

## 10. bool if else

  newmlでif elseはif文ではなく式です。三項演算子は無いので、if else式を変わりに使ってください。

    // if_else.nml
    if_else = {

      if (a < 10) printf("b1\n")
      if (a < 10) { printf("b2\n") }
      if (a < 10) { printf("b2\n") printf("b3\n") }
      if (a > 10) printf("a\n") else printf("b3\n")

      printf(if (a < 1) "a\n" else "b1\n")
    }

## 11. 関数

  関数は`関数名(引数リスト)=式`の形式で記述します。

    functions() = {

      printf("test\n")
    } functions()

  この形式で記述した関数は再帰する事が出来ません。再帰したい場合は、後述するdefを使って再帰可能にします。

    curry_function = {

      f0:()=>int= {|() => 0 }
      printf("%d\n")(f0())
      f0():int= { 0 }
      printf("%d\n")(f0())
      f0():int= 0 
      printf("%d\n")(f0())
      f0()= 0 

      printf("%d\n")(f0())
      printf("%d\n"; f0())
      printf("%d\n" f0())

      f1:(int)=>int={|a => a }
      printf("%d\n" f1(1))
      f1(a:int):int={ a }
      printf("%d\n" f1(1))
      f1(a:int):int= a
      printf("%d\n" f1(1))
      f1(a:int)= a

      printf("%d\n")(f1(1))
      printf("%d\n"; f1(1))
      printf("%d\n" f1(1))

      f2:int(int)(int)={| a b => a + b }
      printf("%d\n" f2(1 2))
      f2(a:int):(int)=>int={| b => a + b }
      printf("%d\n" f2(1 2))
      f2(a:int)(b:int):int={a + b}
      printf("%d\n" f2(1 2))
      f2(a:int; b:int):int= a + b
      printf("%d\n" f2(1 2))
      f2(a:int b:int)= a + b

      printf("%d\n")(f2(1)(2))
      printf("%d\n"; f2(1; 2))
      printf("%d\n" f2(1 2))

      f3:int(int int int)={| a b c => a + b + c }
      printf("%d\n" f3(1 2 3))
      f3(a:int):int(int int)={| b c => a + b + c}
      printf("%d\n" f3(1 2 3))
      f3(a:int)(b:int):int(int)={| c => a + b + c}
      printf("%d\n" f3(1 2 3))
      f3(a:int)(b:int)(c:int):int={a + b + c}
      printf("%d\n" f3(1 2 3))
      f3(a:int; b:int; c:int):int= a + b + c
      printf("%d\n" f3(1 2 3))
      f3(a:int b:int c:int)= a + b + c

      printf("%d\n")(f3(1)(2)(3))
      printf("%d\n"; f3(1; 2; 3))
      printf("%d\n" f3(1 2 3))

      printf(
        "%d %d %d %d\n"
      )(
        f0()
      )(
        f1(1)
      )(
        f2(1)(2)
      )(
        f3(1)(2)(3)
      )

      printf(
        "%d %d %d %d\n";
        f0();
        f1(1);
        f2(1; 2);
        f3(1; 2; 3)
      )

      printf(
        "%d %d %d %d\n"
        f0()
        f1(1)
        f2(1 2)
        f3(1 2 3)
      )

      printf("%d\n")(f3(-1)(-2)(-3))
      printf("%d\n"; f3(-1; -2; -3))
      printf("%d\n" f3(-1; -2; -3))

      printf("%d+%d=%d\n")(a)(b)(a+b)
      printf("%d+%d=%d\n"; a; b; a + b)
      printf("%d+%d=%d\n" a b a + b)
    }

## 12. 参照

  newmlでは変数を書き換えたい場合には参照を使います。

    a = & 1

  `&`で参照を取得します。

    *a := 2

  `=`演算子は変数あるいは関数の定義につかっているため、代入は`:=`を使って代入出来ます。 
  現在一般的に使われているプログラミング言語では変わっている仕様ですが、
  数学的に見れば、=なのに値が違う方がおかしいと考えればこれで良いのかなと思います。
  どっちでも良いと言えばどっちでも良い所ですが、newmlは関数型言語なので、書き換え可能な変数は
  若干使いにくくなっています。

  参照の中身を見るには`*`を使います。

    printf("%d\n"; *a)
  
  C言語のポインタだと思えば分かりやすいでしょう。

  １つ値を増やす場合は、前置`++`演算子を使います。

    ++a

  後置演算子は近年の関数型言語では廃止する事が多くなっています。そのため使えません。 

  参照は#=演算子を使って初期化する事も可能です。

    b #= 1

  この式の意味は、以下の式と同じです。

    b = & 1

  しかしながら、沢山記述が増えた場合を考えると一文字でも短く書きたくなるのと、出来れば、
  書き換え不能なimmutableな変数と書き換え可能なmutableな変数の記述量を一緒にするためこのような記述が可能になる事を検討しています。

    b #int=1 // 動きません
    b #int=2 // 動きません

  書き忘れましたが、値を減らすには `--`前置演算子を用いる事で値を減らす事が出来ます。

    --b

  以下、サンプルプログラムです。

    // reference.nml
    open Printf

    reference() = {

      a = & 1
      *a := 2
      printf("%d\n"; *a)
      ++a
      printf("%d\n"; *a)

      b #= 1
      ++b

      printf("++ %d\n"; *b)
      --b
      printf("-- %d\n"; *b)
    } reference()

## 13. 配列とループ

  newmlでも配列を使ったり、ループをする事が可能です。

  配列は`[|`と `]`で括って初期化出来ます。とじ括弧に|は存在しません。

    a = [| 1; 2; 3]

  細かい違いですが、括弧の開始の`[`と`|`の間にスペースを含める事も出来ます。

    a = [ | 1; 2; 3]

  配列内部を見るには以下のように、[]を使ってC言語のようにアクセスします。
  OCamlはa.(0)のような記述ですので違う点です。

    printf("%d%d%d\n" a[0] a[1] a[2])

  配列に値を設定する場合は、


    a[0] <- 1

  のように<-で設定します。TODO:(要確認。出来てないかも)

  OCamlのArrayモジュールをそのまま呼び出す事が出来ます。

  for文は以下のように記述します。Scalaに似せてあります。

    for (i <- 0 to 3) {
      printf("%d\n" i)
    }

  downtoを使う事で、値が小さくする事も出来ます。これはOCamlの機能です。

    for (i <- 10 downto 1)
      printf("%d\n" i)

  Scalaのbyや、BasicのstepはOCamlにはないので、頑張れば作れますが今の所作者がforを使っていないので必要がなく作成していません。`{}`で囲う必要はありませんが、forの後ろの`()`は必要です。


  whileループは以下のように記述して使います。

    i = & 0
    while (*i < 3) {
      ++i
      printf("while %d\n"; *i)
    }

  この章のサンプルはarray.nmlです。

    // array.nml
    array_and_loop = {
      a = [| 1; 2; 3]
      printf("%d%d%d\n" a[0] a[1] a[2])

      for (i <- 0 to 3) {
        printf("%d\n" i)
      }

      for (i <- 10 downto 1) {
        printf("%d\n" i)
      }

      i = & 0
      while (*i < 3) {
        ++i
        printf("while %d\n"; *i)
      }
    }

## 14. 再帰関数

  再帰関数を使うにはdefを関数の手前に付けます。何故このような至要になっているかというと、
  関数の上書きが出来るようにするためです。

    recursive_function() = {

      /* (let rec) */
      def fib:(int)=>int = {| n =>
        if (n == 0) 0
        else if (n == 1) 1
        else fib(n-2) + fib(n -1)

      }

      /*
      def fib(n:int):int = {
        if (n == 0) 0
        else (if (n == 1) 1
        else fib(n-2) + fib(n-1))
      }
      */

      printf("fib 10 %d\n" fib(10))
    } recursive_function()

## 15. tuple

  多値を使う事も出来ます。

    tuple() = {

      addt:(int,int)=>int = {| a,b =>
        a + b
      }
      printf("%d\n" addt(1, 2))

      /*
      addt(int:a,int:b):int= a + b
      */

      /*
      f3={
        |(a,b,c) => a*b*c
      }
      */
      /*
      f3:(int,int,int)=>int={
        |(a,b,c) => a*b*c
      }
      f3:int((int,int,int))={
        |(a,b,c) => a*b*c
      }
      printf("%d" f3(1,2,3))
      */


      /*
      printf("%d\n" addt(1,2))

      ff2:(int,int)=>(int,int)=>int={
        |a,b c,d => a*b + c*d
      }

      f2:int((int,int))((int,int))={
        |a,b c,d => a*b + c*d
      }
      */
      /*
      f2:int((int,int);(int,int))={
        |a,b c,d => a*b + c*d
      }
      */
      /*
      ff:int=>int,int=>int={
        |a c,d => a + c*d
      }

      ff:int=>((int,int)=>int)={
        |a c,d => a + c*d
      }

      ff:((int,int)=>int)(int)={
        |a c,d => a + c*d
      }

      ff:int((int,int))(int)={
        |a c,d => a + c*d
      }
      */
      /*
      ff:int(int int,int)={
        |a c,d => a + c*d
      }
      */

      f2:(int,int)=>(int,int)=>int={
        |a,b c,d => a*b + c*d
      }
      /*
      f2(a:int,b:int)(c:int,d:int):int={
        a*b + c*d
      }
      */
      printf("%d\n")(f2(1,2)(3,4))
      printf("%d\n" f2(1,2 3,4))
    } tuple()

## 16. パターンマッチ

  パターンマッチを以下のように記述する事が可能です。
  パターンマッチはswitch文を強力にした物です。
  パターンマッチを使うとフィボナッチ関数を以下のように記述出来ます。

    def fib(n) = {
      n match {
        | 0 => 0
        | 1 => 1
        | n => fib(n-2) + fib(n-1)
      }
    }
    printf("fib 10=%d" fib(10))

  newmlのパターンマッチは=>移行の式の連続を実行しますが、次のパターンの処理は実行しません。

    whens = {

      def fib:(int)=>int={
        | n when n == 0 => 0
        | n when n == 1 => 1
        | n => fib(n-2) + fib(n-1)
      }
      printf("fib 11 %d\n" fib(11))
    }

## 17. クロージャ

    closure() = {

      block(sp)(f)={
        printf("{\n")
        f(sp ~ "  ")
        printf("%s}\n" sp)
      }
      p = printf

      p("def %s() " "a") block(""){|sp=>
        p("%sdef %s() " sp "b") block(sp){|sp=>
          p("%sprogram2()\n" sp)
          p("%sprogram2()\n" sp)
        }
        p("%sprogram()\n" sp)
      }
      
      block(sp)(f)={
        printf("{\n")
        f(sp ~ "  ")
        printf("%s}\n" sp)
      }
      p = printf

      p("def %s() " "a") block(""){|sp=>
        p("%sdef %s() " sp "b") block(sp){|sp=>
          p("%sprogram2()\n" sp)
          p("%sprogram2()\n" sp)
        }
        p("%sprogram()\n" sp)
      }
    } closure()

## 18. パーシャルファンクション

  newmlでは１つあるいは複数の値を受け取り、パターンマッチする関数を記述出来ます。

    def fib:(int)=>int={
      | 0 => 0
      | 1 => 1
      | n => fib(n-2) + fib(n-1)
    }

  OCamlのfunctionですが、より強力で、カリー化したパターンマッチを実現しています。

    def f = {
      | 0 0 => 0
      | 0 1 => 1
      | a b => a * b
    }
    printf("%d\n" f(0 1))

  これは、OCamlのfunとfunctionを１つの構文で実現しようとした結果の副作用とも言える機能ですが強力で便利です。

  また、tupleを使ったパターンマッチも記述可能です。

    llor:(int,int)=>int={
      | 0,0 => a=1 b=2 a | b
      | a,b => a | b
    }

  パターンマッチのパターンの開始は|です。そのため、１行で書いた場合、|演算子が左の式と結合してしまう可能性があります。

    f = {|0=>0|n=>n*n} // 構文エラー

  この場合は、以下のようにセミコロンを記述する事で|の前の式と分離する事が出来ます。

    f = {|0=>0;|n=>n*n} // OK

  この章のサンプルは以下のソースで確認出来ます。

    // parcial_function.nml
    parcial_function() = {

      def fib:(int)=>int={
        | 0 => 0
        | 1 => 1
        | n => fib(n-2) + fib(n-1)
      }
      
      printf("fib 10 = %d\n" fib(10))

      /* tuple parcual function */

      llor:(int,int)=>int={
        | 0,0 => a=1 b=2 a | b
        | a,b => a | b
      }

      printf("llor %d\n" llor(1,2))
    } parcial_function()

## 19. リスト

    list() = {

      iter({|x => printf("%d\n")(x)})([1; 2; 3; 4])
      iter({|x => printf("%d\n"; x)})([1; 2; 3; 4])
      iter({|x => printf("%d\n" x)})([1; 2; 3; 4])
      iter({|x => printf("%d\n" x)}; [1; 2; 3; 4])
      /* iter({|x = printf("%d\n" x)} [1; 2; 3; 4])*/
      iter{|x => printf("%d\n" x)}([1; 2; 3; 4])
      iter{|x => printf("%d\n" x)}([1 2 3 4])

      [1 2 3 4] |> iter {
       | x => printf("%d\n" x)
      }

      [1 2 3 4] |> map {
        | x => x * 10
      } |> iter {
        | x => printf("%d\n" x)
      }

      /*
      def ps(xs)= {
        | [] => () ;
        | x::xs => printf("%d\n" x) ps(xs)
      }
      ps(xs)
      */
    } list()

    list_type = {

      ls = [1 2 3]

      ls |> iter {
        |l => printf("%d," l)
      } printf("\n")

      ls:list[int] = [1 2 3]
      
      ls |> iter {
        |l => printf("%d," l)
      } printf("\n")

      ls:list[(int,int)] = [1,2 3,4]
      
      ls |> iter {
        |(l,r) =>
          printf("(%d,%d);" l r)
      } printf("\n")

      def f={
        | [] => ()
        | x::xs =>
          printf("%d,\n" x)
          f(xs)
      }

      f([1 2 3])

      printf("1+20=%d\n" 1 +
        20)
    }

## 20. レコード

    a type {x:int y:int}

    record() = {

      a :a= {:x=1 y=2}

      printf("%d\n" a.x)

      printf("%d\n"; {:x=1+2*3 y={a=1 a}}.x)

      aa = {|{:x}=>printf("%d\n" x)}

      aa({:x=1 y=2})

    } record()

## 21. 代数データ型

    e type | EUnit | EInt(int) | EAdd(e, e)

    variant() = {

      def eval:e=>int={
        | EUnit => 0
        | EInt(i) => i
        | EAdd(a, b) => eval(a) + eval(b)
      }

      printf("1+2=%d\n" eval(EAdd(EInt(1), EInt(2))))
    } variant()

## 22. モジュール

    A module {
      a = 1234
      inc(k) = k + 1
    }

    _ = {
      
      printf("A.a = %d A.inc(10) = %d\n" A.a A.inc(10))
    }

## 23. クラス

    ab class {
      // private member
      a = 123
      // public method
      def c = a

      /*
      // private method
      - d = 10
      */
    }
    _ = {

      printf("ab.a = %d\n" new ab->c)
    }

    abc class(b:int c:int) {
      // public method
      def c = b
    }

    _ = {

      printf("ab.a = %d\n" new abc(10 20)->c)
      printf("ab.a = %d\n" 2*3+1)
    }

    fib class(x:int) {
      def apply =
        x match {
          | 0 => 0
          | 1 => 1
          | n => new fib(x-2)->apply +
                 new fib(x-1)->apply
        }
    }

    _ = {

      printf("A.a = %d\n" new fib(10)->apply)
      printf("%d %d\n" 1+2-3;-1)

    }

## 24. 多相ヴァリアント

  多相ヴァリアントを使うと、以下のように、型を定義せずに代数データ型を使う事が出来ます。
  多相ヴァリアントはほとんどOCamlの仕様と同じです。

    variants = {

      def eval = {
        | `int(n) => n
        | `add(a,b) => eval(a) + eval(b)
        | `mul(a,b) => eval(a) * eval(b)
      }
      printf("%d\n" eval(`add(`mul(`int(10),`int(20)),`int(20))))
    }

## 25. キーワード引数

  TODO:キーワードで良いのか？調べる。
  TODO:仕様を#か、~か確定する。

  キーワードを使って、名前指定の関数定義が出来ます。
  OCamlでは~と?を使い分けていましたが、~か#のどちらかで統一する予定です。
  名前指定の関数呼び出しは、a=1のように=を使って引数を指定します。

    keyword_params = {
      f(#a:int=1 #b:int #c d) = a+b+c+d
      printf("%d\n" f(a=1 b=2 c=5 3))

      f(~a:int=1; ~b:int; ~c; d) = a+b+c+d
      printf("%d\n" f(a=1 b=2 c=5 3))
    }