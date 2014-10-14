# C言語風言語におけるカリー化された関数の簡潔な記法


## はじめに

C言語に似た言語では、関数の呼び出しはa(b)のように括弧が必要になる。
したがって、例えば５つの引数を持つカリー化された関数を呼び出すには

    a(1)(2)(3)(4)(5)

のように記述しなくてはならない。
本提案では、この記述を

    a(1 2 3 4 5)

あるいは、

    a(1; 2; 3; 4; 5)

のように記述する事で、簡潔に記述する事を提案する。
また、実際にOCamlへのトランスレータを実装する事で現実的であるかどうかを検証する。

## 記述例による比較

引数0の場合

    ocaml     : let f0:()->int = (fun () -> 0)
    newml     : f0 :()=>int = { () => 0 }

    ocaml  (2): printf "%d\n" (f0 ())
    scala  (3): printf("%d\n")(f0())
    newml  (2): printf("%d\n" f0())
    newml2 (2): printf("%d\n"; f0())

引数が0の場合、本提案の手法newmlの文字数はocamlの標準的な記述方法に比べると2文字短く記述出来る。newml2とscalaが次に少なく記述出来る。ocamlが最も多くなる。

引数1の場合

    newml     : f1 :int=>int = { a => a }

    ocaml  (1): printf "%d\n" (f1 1)
    scala  (3): printf("%d\n")(f1(1))
    newml  (2): printf("%d\n" f1(1))
    newml2 (2): printf("%d\n"; f1(1))

引数が1つの場合はocamlとnewmlが短く、scalaとnewml2は長い

引数2の場合

    newml     :  f2 :int=>int=>int= { a b => a + b }

    ocaml  (1):  printf "%d\n" (f2 1 2)
    scala  (4):  printf("%d\n")(f2(1)(2))
    newml  (2):  printf("%d\n" f2(1 2))
    newml2 (2):  printf("%d\n"; f2(1; 2))

引数が2つの場合もocamlとnewmlが短く、scalaとnewml2は長い

引数3の場合

    newml     : f3 :int=>int=>int=>int= { a b c=> a + b + c }

    ocaml  (1): printf "%d\n" (f3 1 2 3)
    scala  (5): printf("%d\n")(f3(1)(2)(3))
    newml  (2): printf("%d\n" f3(1 2 3))
    newml2 (2): printf("%d\n"; f3(1; 2; 3))

引数が3つの場合もocamlとnewmlが短く、scalaとnewml2は長い

引数全てを出力する場合

    ocaml  (5): printf
                  "%d %d %d %d\n"
                  (f0 ())
                  (f1 1)
                  (f2 1 2)
                  (f3 1 2 3)

    scala (12): printf
                  ("%d %d %d %d\n")
                  (f0())
                  (f1(1))
                  (f2(1)(2))
                  (f3(1)(2)(3))

    newml  (5): printf(
                  "%d %d %d %d\n"
                  f0()
                  f1(1)
                  f2(1 2)
                  f3(1 2 3)
                )
    newml2 (5): printf(
                  "%d %d %d %d\n";
                  f0();
                  f1(1);
                  f2(1; 2);
                  f3(1; 2; 3)
                )

以上のように記述可能である。
引数が0の呼び出しの場合は例外的にocamlの記述方法が長いが、引数1つ以上の場合になると、ocamlとnewmlの手法が同等の記述量で記述する事が出来る。
分かりやすさは、主観的であるが、newmlとnewml2は括弧の数が少ない

引数がマイナス値

    ocaml  (4): printf "%d\n" (f3 (-1) (-2) (-3))
    ocaml2 (4): printf "%d\n" (f3(-1)(-2)(-3))
    scala  (5): printf("%d\n")(f3(-1)(-2)(-3))
    newml  (2): printf("%d\n" f3(-1; -2; -3))
    newml2 (2): printf("%d\n"; f3(-1; -2; -3))

引数がマイナスの値の場合は、ocamlでは括弧が必要になる。newmlの方式ではnewml2の;を使う事で括弧を増やさず記述可能であり、短く書く事が出来ている。

括弧の数を見ると、newmlとnewml2の手法では、括弧の数は、カリー化された関数の呼び出し回数になっている事が分かる。scalaの括弧の数は全ての関数の呼び出しの数になっている。
また、newml2の;の数はカリー化の数になっている事は興味深い点である。

## 実装

実装は、OCamlを用いて新しい記述の言語を作成した。
構文木(ast.ml)を定義しOCamlLexで字句解析器(lexer.ml)をOCamlYaccでパーサ(parser.ml)を作成し、出力処理(gen_ml.ml)で出力を行った。
メインの処理はmain.mlに記述してある。


## まとめ

本提案手法では、殆どの場合でocamlと同等あるいは、短い記述で記述する事が出来た。
また、newmlではカリー化された関数呼び出しの回数を括弧の数で数える事が出来て、カリー化されている数が;の数に対応している事が分かった。

ocamlは記述量が不安定で最小の場合は最も短く記述出来るが最悪のケースでは最も長くなる。

newmlの手法は短く書く事が出来るが、不安定で一部ではnewml2の手法を取り入れる必要がある。
newml2の手法は若干長いが、安定的で関数呼び出しやカリー化数が分かる利点がある。

本論分で提案する手法は従来の手法よりも安定的で短く記述することが出来た。

## 今後

今回作成したトランスレータは、OCamlのトランスレータとしては完全ではないので、完全にOCamlの文法を再現する事が出来ればよいだろう。
また、現状は型チェック等が行われていないので行うと良いだろう。
型チェックは

    # 619 "parser.ml"

のようにプログラムの位置を埋め込む事だけでも大分違うだろう。
