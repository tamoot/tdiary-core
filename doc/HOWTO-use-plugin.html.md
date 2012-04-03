tDiary: How to use plugin

プラグインの使い方
=========

**※プラグインの作り方については、[HOWTO-make-plugin.html](HOWTO-make-plugin.html)を参照してください。**

プラグインはシステムに簡単に機能を追加する仕組みです。プラグインは[tDiary.org](http://www.tdiary.org/)から入手することができます。また、tDiaryフルセットを利用している場合には、misc/pluginディレクトリに単独に配布されているのと同等のものが含まれています。これらの.rbファイルを、ファイルごとインストール先にあるmisc/pluginディレクトリに移動することで、利用できるようになります。

プラグインにはさまざまなものがありますが、日記中に自動的に何かの文字列を埋め込んだり、特殊な処理をさせるのが主な目的です。また、特定のプラグインを作ることで、tDiaryのメッセージをカスタマイズすることもできます。

以下では、プラグインについてその「使い方」「プラグイン集について」を説明します。なお、.rbファイルのことを「プラグインファイル」、実際に日記中で呼び出して使う機能(主にRubyのメソッド)のことを「プラグイン」と呼びます。ひとつのプラグインファイルは、複数のプラグインを含みます。

プラグインの使い方
---------

プラグインを使うには、そのプラグインを含んだプラグインファイルが、tDiaryインストール先にあるmisc/pluginディレクトリになければなりません。あらかじめ使いたいプラグインファイルをmisc/pluginディレクトリにコピーしておく必要があります。その上で、設定画面のプラグイン選択にて使いたいプラグインを有効にしましょう。プラグインの中には高機能で処理に時間のかかるものも含まれているので、必要でないプラグインまでコピーすると、日記の表示が遅くなったりします。必要なプラグインだけを使うように、きちんと選択してください。

プラグインは、eRubyと呼ばれる文法を使って、日記のタイトルや本文、ヘッダやフッタに埋め込むことで利用できます。日記本文で使う場合には、使っている記法によってプラグインの呼び出し方が異なります。「calendar」や「navi」というデフォルトプラグインは、すでにヘッダの中で使っているので見たことがあるでしょう。プラグインの機能を日記に埋め込むときのもっとも簡単な書式は以下のとおりです。

tDiaryスタイルおよびヘッダ/フッタで使う場合:

```
<%=プラグイン名%>
```

Wikiスタイルの場合:

```
{{プラグイン名}}
```

たとえば、カレンダーを埋め込むプラグイン「calendar」は、以下のようにして記述します。

tDiaryスタイルおよびヘッダ/フッタで使う場合:

```
<%=calendar%>
```

Wikiスタイルの場合:

```
{{calendar}}
```

プラグインの種類によっては、以下のようにパラメタを指定して動作を変えることもできます。

```
<%=プラグイン名 パラメタ1, パラメタ2, ...%>
```

各プラグインがどのようなパラメタを利用できるかは、プラグイン選択画面のリンクからたどれるヘルプを見るか、[tDiaryドキュメント](http://docs.tdiary.org/ja/)から探せます。また、プラグインによっては、上記のような呼び出しが不要で、プラグイン選択で有効にするだけで動作するものもあります。

プラグインを使えるのは、日記のタイトルと本文、およびヘッダとフッタです。すべてのページに表示したい場合にはヘッダかフッタを、その日の日記にだけ使いたい場合にはタイトルや本文に使います。また、ツッコミには使えません。

プラグインのカスタマイズ
------------

呼び出し時のパラメタを使う以外に、設定画面やtdiary.conf内でオプションを指定する方法でカスタマイズができるものもあります。主に、コピーするだけで動くプラグインの動作を変えたり、パラメタを指定しない時の挙動を変更する場合に使います。設定画面でカスタマイズできるプラグインは、有効にしたあとに設定画面のメニューが増えます。

tdiary.confでのオプションの指定には、@optionsという変数を使います。例えば、hogeというプラグインのfugaというパラメタをカスタマイズするには、以下のような指定方法を使います。

```
 @options['hoge.fuga'] = 'なんとか'
```

どのようなパラメタが指定できるかは、プラグインのドキュメントに書かれています。

また、多くのプラグインで共通に使われるオプション指定がいくつかあります。

まず、apply\_pluginというパラメタは、プラグインが展開したあとの文字列にさらにプラグイン呼び出しの指定があった時に、それをもう一度展開するかどうかを指定するパラメタです。

```
 @options['apply_plugin'] = true
```

無指定時には再展開はしないようになっているので、apply\_pluginに対応したプラグインで再展開機能を使いたい場合には上記の指定をtdiary.confに書いておく必要があります。

さらに、botというパラメタもあります。これは、検索エンジンの巡回ロボットが、「本日のリンク元」を見て本文と無関係な検索キーワードを拾うのを防ぐためのものです。値には巡回ロボットのUser-Agentの配列を指定しますが、主要なロボットは登録済みなので、これをいじる必要はあまりないでしょう。

```
 @options['bot'] = ['GoogleBot', 'Hatena Antenna']
```

企業内にtDiaryを設置する場合に、インターネット上のサービスを利用するいくつかのプラグインやフィルタが、プロクシを経由しないと動作しない場合があります。このときに利用できるパラメタにproxyがあります。このオプションには、プロクシサーバのホスト名とポート番号を「:」で区切って指定します。

```
 @options['proxy'] = 'proxy.example.com:8080'
```

デフォルトプラグインの説明
-------------

「デフォルトプラグイン」というプラグインファイル「00default.rb」がすでにpluginディレクトリに置いてあります。まず、これらのプラグインについて説明します。

### naviプラグイン

日記ページの先頭にある、「トップ」「最新」「更新」などのナビゲーションボタンを表示するためのプラグインです。パラメタはありません。ヘッダやフッタに埋め込むことで、ページによって最適なボタンを表示します。このプラグインをどこかに埋め込んでおかないと、日記の更新ができなくなってしまいます。

※もしnaviプラグインを埋め込み忘れてしまい、日記の更新ができなくなってしまったら、日記のURLの後ろに「update.rb」(環境によっては「update.cgi」など)を付けて呼び出してみましょう。更新ページが表示されます。

※naviプラグインは、更新や設定のページに内部的に埋め込まれています。もとの挙動を大きく変えるような改造をすると、操作性を大きく損ねるおそれがあるので注意してください。

### navi\_userプラグイン

naviプラグインから内部的に呼び出されているプラグインですが、単独で使うこともできます。このプラグインは、ナビゲーションボタンのうち、日記閲覧者にのみ必要なボタン(「トップ」「最新」「」)を表示します。これを使うことで、「更新」のような日記オーナーにだけ必要なボタンを表示させないなどのカスタマイズを行えます。

パラメタはありませんが、naviプラグインを使う場合に比べて、前後のPタグが省略されるので、以下のようにして埋め込むと良いでしょう。

```
<p class="adminmenu"><%=navi_user%></p>

```

### navi\_adminプラグイン

navi\_userプラグインと反対に、管理用に必要なボタン「更新」「設定」を表示します。使い方はnavi\_userと同じです。

### calendarプラグイン

カレンダーを表示します。日記が書かれている月のみをリストします。ヘッダやフッタに埋め込んで使います。パラメタはありません。このプラグインをどこかに入れておかないと、日記閲覧者は過去の日記を読むことができなくなってしまいます。

### insertプラグイン

外部ファイルを日記に読み込みます。主な用途は、ヘッダやフッタに、すでにある定型ファイルを挿入したいような場合です。そのままの形で埋め込むので、挿入するファイルはHTMLでなければなりません。パラメタとしてひとつのファイル名をとるので、以下のように使います。

```
<%=insert 'menu.html'%>
```

このプラグインは、ファイルにアクセスするため、tdiary.confで@secureがfalseに設定されていないと利用できません。

### myプラグイン

日記本文中で、自分の日記内へのリンクを簡単に作成します。[...](〜)とやってもよいのですが、こちらの方がサーバの引っ越しやmod\_rewriteを利用してURLを変更した場合などに強くできます。

最初のパラメタにリンク先の日付とアンカーを「YYYYMMDD#pXX」(YYYY年MM月DD日のXX番目のセクション)や「YYYYMMDD#cXX」(YYYY年MM月DD日のXX番目のツッコミ)のような形式で指定します。2番目のパラメタには、リンクにする文字列を指定します。3番目のパラメタには、必要であればリンクタイトルを指定できます。

```
<%=my '20020301#c01', 'そのツッコミ' %>はひどい。
```

プラグイン集について
----------

続いて、プラグイン集について説明します。プラグイン集は、tDiaryユーザが作った数々のプラグインを集めたものです。tDiaryフルパッケージをインストールした場合にはmisc/pluginディレクトリにありますし、そうでない場合にも[tDiary.org](http://www.tdiary.org/)から入手することができます。これらのプラグインファイルは、は自分でmisc/pluginディレクトリにコピーして利用します。

それぞれのプラグインの詳しい使い方は、各プラグインのドキュメントを読んでください。
