インストール
============

OSごとにインストール方法を説明します。

.. contents::
    :depth: 3

Windows
-------

SWIGのインストール
^^^^^^^^^^^^^^^^^^

ダウンロードページ_ からswigwin-x.x.x（swig-x.x.xではない）
というリンクを踏んでバイナリをダウンロードします。
ダウンロードしたら適当なところに展開して下さい。

展開したファイルの中にswig.exeがあると思います。
ここへのパスを環境変数PATHに追加しておきましょう。

インストールは以上です。

.. note::

    SWIGでPython向けのバイナリを作る際はVisual C++が必要になりますが、
    Visual C++のバージョンはPython自体をコンパイルした時と
    同じバージョンである必要があります。
    公式にはVisual C++ 2005でPythonがコンパイルされています。

    MicrosoftはPython向けのバイナリを作成できるように
    Python向けのVisual C++ 2005というものを提供しています。
    下記からそれをダウンロードしてインストールしておきましょう。

    http://www.microsoft.com/en-us/download/details.aspx?id=44266

Linux
-----

ダウンロードページ_ からswig-x.x.x（swigwin-x.x.xではない）
というリンクを踏んでバイナリをダウンロードします。
ダウンロードしたら適当なところに展開して下さい。

あとは下記のとおりに実行することでインストールは完了です。

.. code-block:: bash

    $ ./configure
    $ make
    $ sudo make install

SWIGでPython向けのバイナリを作りたい場合
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Python向けのバイナリを作りたい場合はPython.hなどのヘッダが
環境にインストールされている必要があります。



.. _ダウンロードページ: http://www.swig.org/download.html
