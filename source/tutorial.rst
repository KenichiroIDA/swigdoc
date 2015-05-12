チュートリアル
==============

.. contents::
    :depth: 2

PythonからC++関数を呼び出す
---------------------------

まずはPythonから下記のようなC++関数を呼び出すやり方を説明します。

.. code-block:: cpp
    :caption: hello.h
    :linenos:

    #ifndef HELLO_H_
    #define HELLO_H_

    void Hello();

    #endif  // HELLO_H_

.. code-block:: cpp
    :caption: hello.cpp
    :linenos:

    #include <iostream>

    #include "hello.h"

    void Hello() {
        std::cout << "Hello World!" << std::endl;
    }

Hello()というC++の関数をPythonから呼び出せるようにします。
実行すれば標準出力に::

    Hello World!

と表示する関数です。

最初にインターフェース定義ファイルと呼ばれるファイルを用意します。

.. code-block:: swig
    :caption: hello.i
    :linenos:

    %module hello

    %{

    #include "hello.h"

    %}

    %include "hello.h"

このファイルの意味は後ほど説明することにして、いったん動かすところまで
持って行きたいと思います。

hello.iをSWIGでコンパイルします。

.. code-block:: bash

    $ swig -c++ -python -cppext cpp hello.i

コマンドを実行するとhello.py, hello_wrap.cppが生成されていると思います。
hello.cpp, hello_wrap.cppは動的ライブラリとしてコンパイルする必要があります。
下記のようにしてコンパイルします。

* Windows

    .. code-block:: bash

        $ cl /EHsc /c hello.cpp hello_wrap.cpp
        $ link /DLL /OUT:_hello.pyd hello.obj hello_wrap.obj

* Linux

    .. code-block:: bash

        $ g++ -fPIC -shared hello.cpp hello_wrap.cpp -o _hello.so

ファイル名には注意して下さい。
hello.iの%moduleという行でhelloと付けたら、ファイル名は_hello.pyd, _hello.soでなければなりません。

ここまでできたら、PythonからHello()が呼び出せるようになっています。
下記のようにして呼び出してみましょう。

.. code-block:: python
    :linenos:

    from hello import Hello


    def main():
        Hello()


    if __name__ == '__main__':
        main()

JavaからC++関数を呼び出す
-------------------------

次にJavaから同様の関数を呼んでみます。

インターフェース定義ファイルはPython向けに作成したものと同じものが利用できます。
これを使って下記のようにコンパイルします。

.. code-block:: bash

    $ swig -c++ -java -cppext cpp hello.i

hello.java, helloJNI.java, hello_wrap.cppが生成されます。
hello.cpp, hello_wrap.cppを動的ライブラリとしてコンパイルするところはPythonの場合と同じです。

上記のコマンドだと生成されるJavaのコードはパッケージに属さない形式で
ソースコードが生成されますが、

.. code-block:: bash

    -package <パッケージ名>

を付けてあげれば、そのパッケージに属するようになります。

コンパイルする際はJNIのヘッダがあるパスを含める必要があります。
JNIとはJavaからC/C++関数を呼び出すためにJavaが標準で実装している機構です。
SWIGはこのJNI向けのソースコードを生成してくれているということです。

* Windows

    JNIを使うのに必要なヘッダのパスは、64ビット版であれば

    * C:\\Program Files\\Java\\<jdk version>\\include
    * C:\\Program Files\\Java\\<jdk version>\\include\\win32

    32ビット版であれば

    * C:\\Program Files (x86)\\Java\\<jdk version>\\include
    * C:\\Program Files (x86)\\Java\\<jdk version>\\include\\win32

    に含まれています。これら2つのパスをインクルード検索パスに含めます。

    .. code-block:: bash

        $ cl /EHsc /c /I<JNIへのパス1> /I<JNIへのパス2> hello.cpp hello_wrap.cpp
        $ link /DLL /OUT:hello.dll hello.obj hello_wrap.obj

* Linux

    JNIへのパスはディストリビューションによってまちまちなので
    適宜設定して下さい。

    .. code-block:: bash

        $ g++ -fPIC -shared -I<JNIへのパス> hello.cpp hello_wrap.cpp -o libhello.so

ここまでできたらJavaから呼び出せるようになっています。

.. warning::

    SWIGの公式ドキュメントによると、g++に最適化オプション（-O2など）を付ける場合は
    一緒に-fno-strict-aliasingも付けるべきだと書かれています。
    gccは4.0から最適化がかかりやすくなったことで
    最適化をかけてstrict-aliasingが有効になると
    C/C++とJava間の型マッピングに失敗するおそれがあるそうです。

    strict-aliasingとは簡単に言うと::

        int a;
        short* p = (short*)&a;

    のような、互換性のないキャストに対する操作を無視する最適化です。
    型のサイズが小さくなる方向にキャストしているので
    一見安全そうですが、キャストする型間に互換性があるかどうかは
    言語仕様で決まっている話なので、上記の操作は本来は行うべきではないです。

    -fno-strict-aliasingは上記のような互換性のないキャストに対して
    それを考慮したコンパイルをさせるオプションということです。

.. code-block:: java
    :caption: Main.java
    :linenos:

    public class Main {
        static {
            System.loadLibrary("hello");
        }

        public static void main(String[] args) {
            hello.Hello();
        }
    }

Javaのコードをコンパイルして実行します。

.. code-block:: bash

    $ javac Main.java hello.java helloJNI.java
    $ java Main

まとめ
------

Pythonから呼び出す場合もJavaから呼び出す場合も
インターフェース定義ファイルを書いて動的ライブラリをコンパイルするという
一連の流れは同じであることがわかると思います。
特定の言語に依存せずにC/C++関数をバインド出来るというところがSWIGの魅力なわけです。
