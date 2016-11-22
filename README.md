# 4d-tips-listbox-header-border

###概要

近年，グラデーションなどを多用した立体的なデザインよりも「フラット」なユーザーインタフェースが好まれています。たとえば，Windows 10の場合，ウインドウタイトルバーとメニューバーが同色（ホワイト）であり，とても省略されたデザインが採用されています。

4Dは，描画にシステム既定（ネイティブ）のレンダリングエンジンを使用しています。オペレーションシステムのアピアランスは，ユーザーが自由にカスタマイズできるだけでなく，システムのバージョン毎にも違うことが少なくありません。ネイティブエンジンを使用することには，そうした変更が自動的に反映される，というメリットあります。同時に，アピアランスが環境に左右されてしまう，というデメリットもあります。

たとえば，Windows 10で**リストボックスの背景色**をホワイトに設定すると，ヘッダーと本体がつながっているかのように表示されます。（Windows 8.1では，わずかに境界が認識できました。）

![](https://github.com/4D-JP/4d-tips-listbox-header-border/blob/master/screenshot.png)

リストのヘッダーをどのように表示するかは，Windowsが決めることであり，**これは4Dの不具合ではありません**。

###回避策

![](https://github.com/4D-JP/4d-tips-listbox-header-border/blob/master/result.png)

4Dのコマンドを活用し，ヘッダーと本体の間にラインを表示して境界を強調することができます。

* 使用するコマンド

[OBJECT SET RGB COLORS](http://doc.4d.com/4dv15r/help/command/ja/page628.html), ``Background color none`` (v14) 

[OBJECT SET COORDINATES](http://doc.4d.com/4dv15r/help/command/ja/page1248.html) (v14)

[OBJECT GET COORDINATES](http://doc.4d.com/4dv15r/help/command/ja/page663.html)

[LISTBOX Get information](http://doc.4d.com/4dv15r/help/command/ja/page917.html)

[PLATFORM PROPERTIES](http://doc.4d.com/4dv15r/help/command/ja/page365.html)

####プラットフォームの特定

OSのタイプ（WindowsまたはmacOS）とバージョンは``PLATFORM PROPERTIES``で調べることができます。

```
C_LONGINT($platform;$system)
PLATFORM PROPERTIES($platform;$system)

If ($platform=Windows)

$version:="<!--#4dtext $1-->.<!--#4dtext $2-->"
PROCESS 4D TAGS($version;$version;$system%256;($system\256)%256)

Case of 
: ($version="6.0")
  //vista
: ($version="6.1")
  //7
: ($version="6.2")
  //8
: ($version="6.3")
  //8.1
: ($version="10.0")
  //latest:10
$doIt:=True
Else 
  //unknown
$doIt:=True
End case 

End if 
```

Windowsのバージョンコードは，数値（Windows 10であれば``10``）が返されます。Windows Vistaから8.1まではすべてメジャーバージョン番号が``6``なので，これを文字列に変換する必要があるかもしれません。計算の方法は``PLATFORM PROPERTIES``のドキュメントに掲載されています。

###背景を透明にする

オブジェクトの背景色を「透過」に設定するためのプロパティは以前から存在しましたが，v14以降，これをプログラムで指定することもできるようになりました。テキスト入力・リストボックス・ピクチャなどの背景色を透明にするには，``OBJECT SET RGB COLORS``に定数の``Background color none``を指定します。リストボックスの背景を透明に設定すれば，背後のオブジェクトがそのまま透けて表示されるようになります。

```
OBJECT SET RGB COLORS(*;"List Box";Foreground color;Background color none)
```

###リストボックスのサイズを計算する

リストボックスは，ヘッダー・フッター・各スクロールバーなどのサブオブジェクトで構成されており，それぞれサイズが変更されていたり，非表示にされていたりするかもしれません。リストボックス全体のサイズは``OBJECT GET COORDINATES``で取得できますが，実際にデータが表示される部分ののサイズは，外縁にあるサブオブジェクトのサイズを引いたものになります。もっとも，今回は背面にオブジェクトを表示することが目的なので，実際にはリストボックス全体のサイズに合わせてもデザイン上は問題ないかもしれません。

```
OBJECT GET COORDINATES(*;"List Box";$left;$top;$right;$bottom)
$HeaderHeight:=LISTBOX Get information(*;"List Box";Listbox header height)
$FooterHeight:=LISTBOX Get information(*;"List Box";Listbox footer height)
$RightScrollbarWidth:=LISTBOX Get information(*;"List Box";Listbox ver scrollbar width)
$BottomScrollbarHeight:=LISTBOX Get information(*;"List Box";Listbox hor scrollbar height)

$top:=$top+$HeaderHeight
$bottom:=$bottom-$BottomScrollbarHeight-$FooterHeight
$right:=$right-$RightScrollbarWidth

OBJECT SET COORDINATES(*;"Rectangle";$left;$top;$right;$bottom)
OBJECT SET COORDINATES(*;"Line";$left;$top;$right;$top)
```

###背景オブジェクトを表示する

デフォルトの背景に代わるものとして，簡単な四角形と線を表示します。オブジェクトは「デフォルトで非表示」のプロパティが選択されており，リサイズのプロパティもリストボックスに合わせて設定されています。v14以降，フォームオブジェクトの「配置を記憶」させることができるようになりましたが，リサイズ（移動）のプロパティが設定されていないと，ぴったり合わせたつもりのオブジェクトが実際にはずれて表示されることになるので注意が必要です。

###その他

Windows 10では，リストボックスを印刷した場合も，ヘッダーと本体の間に境界がないデザインになります。
