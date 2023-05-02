# 初めての Go 言語｜5章 関数

## 5.1 関数の宣言と呼び出し

構造体を使って「名前付き引数」を真似ることができるが、そもそも関数はあまり多くの引数を持つべきではないので、やらないにこしたことはない

値を返す関数でブランク return を使ってはいけない

## 5.4 defer

後始末のコードを、関数実行後に遅延実行できる

LIFO で実行される.

## 5.5 Go は値渡し

関数に引数を渡した際、必ず引数の値のコピーを作る (Not 参照渡し)