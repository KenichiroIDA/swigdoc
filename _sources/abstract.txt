SWIGとは
========

SWIGとはC/C++で書かれた関数を別の言語から呼び出すための機構です。
SWIGを使えば、例えばPythonからCの関数を呼べるようになり、
Pythonの表現力とCのパフォーマンスの両方を兼ね備えた
プログラムを書くことが出来るようになります。

対応する言語は下記の通り、かなりたくさんの言語に対応しています。

* ALLEGROCL
* CHICKEN
* CLISP
* CFFI
* C#
* D
* Go
* Guile
* Java
* JavaScript
* Lua
* Modula 3
* Mzscheme
* Ocaml
* Octave
* Perl
* PHP
* Pike
* Python
* R
* Ruby
* Scilab
* Lisp
* Tcl
* Common Lisp
* XML

他言語同士を結びつけるインターフェースのことをFFI(Foreign Function Interface)
といいます。
このドキュメントでは、主にPython向けのFFIを作成する方法について説明していきますが
他の言語を使用する場合も基本的には同じです。
