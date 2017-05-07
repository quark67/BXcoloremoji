BXcoloremoji パッケージバンドル
===============================

LaTeX： カラー絵文字を出力する

### 前提環境

  - TeX 処理系： e-TeX 拡張をサポートするもの
  - DVI ウェア（DVI 出力時）： dvipdfmx
  - 前提パッケージ：
      * etoolbox パッケージ
      * binhex パッケージ

### インストール

  - `*.sty`   → `$TEXMF/tex/latex/BXcoloremoji`
  - `emoji_images` のディレクトリをそのまま
    `$TEXMF/tex/latex/BXcoloremoji` の下に移動する。

[coloremoji パッケージ]の画像データを利用したい場合は，画像ファイルを
含む `hires`，`lowres`，`twitter` のディレクトリが `$TEXMF/tex/latex`
以下のどこかにある `emoji_images` の直下に配置されるようにする。

[coloremoji パッケージ]: https://github.com/doraTeX/coloremoji

### ライセンス

画像データについては以下が適用される：

Graphics work is licenced under:

CC-BY 4.0.
Copyright 2014 Twitter, Inc and other contributors

その他の著作物にはは以下が適用される：

Other work is licensed under:

the MIT License.
Copyright 2016 Takayuki YATO (aka. "ZR")

bxcoloremoji パッケージ
-----------------------

### パッケージ読込

DVI 出力のエンジンの場合、事前に graphicx パッケージを読みこむ必要が
ある。（PDF 出力の場合は自動的に読み込まれる。）

    \usepackage[dvipdfmx]{graphicx} % dvipdfmx の場合

また，(pdf)LaTeX および pLaTeX の場合は，utf8 入力エンコーディングを
有効化する必要がある。

    \usepackage[utf8]{inputenc}

その後に bxcoloremoji パッケージを読みこむ。

    \usepackage[<オプション>]{bxcoloremoji}

利用可能なオプションは以下の通り。

  * 絵文字画像の種類を指定するオプション。（既定値 = `twemoji-pdf`）
      - `twemoji-pdf`： twemoji の SVG 画像から変換した PDF 画像。
      - `twemoji-png`： twemoji の 72 ピクセルの PNG 画像。
      - `twitter`／`lowres`／`hires`： coloremoji パッケージの
        画像ファイルを流用する。
  * `scale=<実数>`： 絵文字のサイズを標準値に対する倍率で指定する。
    （既定値 = 1）  
    ※標準のサイズは (u)pLaTeX では 1zw，LuaLaTeX + LuaTeX-ja では
    1`\zw`、それ以外は 1em。
  * `basedir=<パス名>`： 画像ファイル用のディレクトリ（`twemoji-pdf`
    等）を配置するパスの名前を指定する。
    （既定値 = `emoji_images/`）

### 使い方

  * `\coloremoji[*]{<文字列>}`： 引数の文字列をカラー絵文字として出力
    する。ただし，対象の画像がないなどの理由で絵文字として出力できない
    場合は，通常のテキスト出力にフォールバックする。
      - (u)pLaTeX および LuaLaTeX + LuaTeX-ja では絵文字は和文扱いと
        なる。ただし，`*`付で実行した場合および数式中では非和文扱いと
        なる。
      - それ以外の環境では絵文字は常に非和文扱いで，`*`指定は無視する。
  * `\coloremojiucs[*]{<符号値列>}`： 文字の Unicode 符号値で入力して
    カラー絵文字を出力する。引数は符号値の16進表記を空白区切りで並べて
    指定する。`*`指定の意味は `\coloremoji` と同じ。  
    例： `\coloremojiucs{23 20E3 1F363}`

### PDF 文字列中での絵文字の利用

hyperref 使用時の文書情報文字列（“PDF 文字列”と呼ぶ）の入力の中でも
`\coloremoji` （および `\coloremojiucs`）命令を使用できる。例えば、
`\section` の引数の中で `\coloremoji` を含めた場合、版面の上では絵文字
の画像として出力され、PDF のしおりの中では文字として表示される。

ただし「PDF 文字列中の Unicode 文字が正しく処理される」状態が担保されて
いることが前提となる。具体的には、次の設定が必要である。

  - (pdf)LaTeX、XeLaTeX、LuaLaTeX では hyperref パッケージに `unicode`
    オプションを付ける必要がある。

        \usepackage[unicode]{hyperref}

  - upLaTeX の場合、pxjahyper パッケージなどの、適切な“ToUnicode 変換”
    を行うパッケージを併用する必要がある。
      - 特に、`\coloremojiucs` 命令の処理には pxjahyper パッケージ
        （そのもの）が必要である。
      - 絵文字の多くは BMP 外の文字であるので、pxjahyper パッケージを
        利用する場合、大抵は `bigcode` オプションが必要になる。

            \usepackage[bigcode]{pxjahyper}

  - pLaTeX ではそもそも PDF 文字列中に JIS 外の文字を含ませることが
    できないため、`\coloremoji(ucs)` の PDF 文字列中での使用について
    も対応できない。

更新履歴
--------

  * Version 0.3c 〈2017/05/07〉
      - PDF 出力時は graphicx を自動で読み込む。
      - バグ修正。
  * Version 0.3b 〈2016/05/22〉
      - PDF 文字列中での入力に対応させた。
      - `\coloremoji(ucs)` 命令を頑強にした。
  * Version 0.3a 〈2016/05/04〉
      - (u)pLaTeX および LuaTeX-ja の縦組みモードに対応。
      - CJK パッケージの CJK 環境中でも正しく動作させる。
      - バグ修正。
  * Version 0.3  〈2016/05/03〉
      - ZWJ 列をサポートするために文字列パーザを書き直した。
  * Version 0.2  〈2016/04/30〉
      - 画像ファミリ `twemoji-pdf`，`twemoji-png` をサポート。
  * Version 0.1  〈2015/09/22〉
      - 最初の公開版。

--------------------
Takayuki YATO (aka. "ZR")  
http://zrbabbler.sp.land.to/
