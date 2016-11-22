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

[LISTBOX Get information](http://doc.4d.com/4dv15r/help/command/ja/page917.html)

[PLATFORM PROPERTIES](http://doc.4d.com/4dv15r/help/command/ja/page365.html)
