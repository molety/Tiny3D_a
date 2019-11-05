＝＝＝＝＝ くるくる3Dデモ アプレット版 ＝＝＝＝＝

★これは何？

Webブラウザ上で動作する、デモソフトです。
3Dの何かがくるくる回ります。

もともとは、3Dのライブラリが用意されていないauオープンアプリプレイヤーの環境で
3D描画を実現してみるという目的で作ったソフトです。
iアプリにしたらどのくらいの性能が出るのか試してみるため、iアプリに移植しました。
さらに、アプレットにもできそうだったので、やってみました。


★操作

↑キー：上を向く
↓キー：下を向く
←キー：左を向く
→キー：右を向く
 Hキー：左に傾く
 Lキー：右に傾く
 Kキー：近づく
 Jキー：遠ざかる
 0キー：元の位置に「うにょっ」と戻る

 1キー：1画面モード
 2キー：2画面モード

 スペースキー：マニュアルモード
 Qキー：終了


★ライセンス

本ソフトウェアは、MIT Licenseの下で公開しています。(NYSLから変更しました)
LICENSEファイルをお読みください。


★ソースについて

T3*.javaがライブラリ、Tiny3D*.javaがそれを使ったアプリケーション、
という想定で書いてあります。

JDKでビルドできますが、この
アーカイブに含まれているのと同等のjarファイルを作成するには、
他にProGuardと7-Zipが必要です。

また、ProGuardや7-Zipを使ったjar作成を手作業でやるのは煩雑なので、
自動化のためにRakefileを用意しています。これを使うためには、
さらにRubyとRakeが必要です。

Rakefileの使い方は以下の記事を参照ください。
(ただし、iアプリ開発向けに書いた記事なので若干異なるところがあります。)
    http://d.hatena.ne.jp/molety/20081206/1228575410


★参考にしたもの

[書籍]
金谷 一朗「3D-CGプログラマーのための実践クォータニオン」、工学社、2004年、ISBN4-7775-1031-X
床井 浩平「GLUTによるOpenGL入門」、工学社、2005年、ISBN4-7775-1134-0
金谷 一朗「3D-CGプログラマーのためのクォータニオン入門」、工学社、2004年、ISBN4-7775-1016-6

[Web]
http://3dinc.blog45.fc2.com/blog-entry-392.html  (gluLookAt)
http://www.manpagez.com/man/3/glFrustum/  (glFrustum)
http://marupeke296.com/FBX_No7_UV.html  (ライティング)
http://marina.sys.wakayama-u.ac.jp/~tokoi/?date=20051007  (ライティング)
http://f4.aaa.livedoor.jp/~pointc/203/No.7397.htm  (非再帰版マージソート)
http://garugari.jugem.jp/?eid=433  (平方根の逆数の高速化)
http://niffy-you-nats.hp.infoseek.co.jp/iappli/LunaMath.java  (平方根の逆数の高速化、Java版)
http://www.yamagami-planning.com/soft/optimize/optimize01/  (三角関数のテーブル化)


★履歴

1.0.0 (2008/12/09)
- docomo iアプリからアプレットへ移植した。
