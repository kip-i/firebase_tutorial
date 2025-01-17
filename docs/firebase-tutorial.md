class: center,middle
# Firebase を用いて ToDo アプリ を動かそう！
---

## アプリ画面イメージ

.left-column[PC]

.right-column[スマホ]

.left-column[![:scale 90%](https://i.imgur.com/rULzRnp.png)]

.right-column[![:scale 40%](https://i.imgur.com/JRmyXfI.png)]
---
## ファイル構成

```bash
firebase-tutorial
├── README.md
├── database.rules.json
├── firebase.json
├── firestore.indexes.json
├── firestore.rules
└── public
    └── sample.html
```
---
## 注意事項

* ブラウザからソースコードをコピペされると他の人からも操作できる状態になっているので、自由にデータベースを更新できてしまいます。そのまま公開（他人にリンクを教えるなど）はしないで下さい
* 公開したい場合は Firebase Authentication の機能を利用して、別途ユーザー作成、権限管理をするなどの認証機能を導入しましょう。
* Firebase はクレジットカードを登録せずに利用できます。無料枠を使い切った場合はサーバなどの稼働が止まってしまうので注意しましょう。
* ただし、本格的にサービスを公開する場合など、上位のプランを選択した場合には **課金が発生します**。詳しくは [こちら](https://firebase.google.com/pricing/) をご確認下さい。
* （HackU 関連イベント開催期間中のみ）このレポジトリに関する不明点や疑問があれば、イベント開催時のSlack等でサポーターに質問してください。
---
## 前提

* ここでは、Firebase の初期設定の仕方と、サンプルコードを動かします。エディタや Git に関する説明は行いません、また、コードを編集することも行いません。
* 使用するエディターは自由ですが、質問対応等の場合に環境があっていることが望ましいため、特別こだわりがなければ**VSCode**を推奨します。
* Macでの操作を前提としております。Firebaseの操作等は基本的にはどのOSでも変わらないため、Windowsでも開発可能ですがここでの説明ではカバーしておりません。 [こちら](./vscode_setup.pdf) を参考に環境構築 の設定をした上で、Mac と同じ手順で進めましょう
* 2021/09/14 現在の情報です。Firebase に関する最新の情報は [公式ページ](https://firebase.google.com/) をご確認下さい
---
## 主な利用技術/ツール

* Firebase CLI:
 * ターミナルからFirebaseの設定や操作を行うためのコマンドラインツール
* Firebase Hosting:
 * Webサイトを提供できる機能
* Firebase Cloud Firestore:
 * データベース(NoSQL)
---
## このチュートリアル完了までの目安時間

* 1時間程度

## 検証済みの環境

* OSX Catalina 10.15.5
* Firebase CLI 8.6.0
* jQuery 3.5.1
* firebasejs 6.2.0
---
layout:true
## Firebaseにログインし、プロジェクトを作る
---

 https://firebase.google.com/?hl=ja へアクセスし、「コンソールへ移動」を選択します。

![:scale 60%](https://i.imgur.com/IQiYKab.png)
---

Googleアカウントでログインします。

![:scale 70%](https://i.imgur.com/NWhMIMa.png)
---

再度、「コンソールへ移動」を選択します。

![:scale 60%](https://i.imgur.com/TxxlQB0.png)
---

プロジェクトを作成します。

![:scale 70%](https://i.imgur.com/fZSohD6.png)
---

プロジェクト名を入力し、Firebaseの規約に同意します。

![:scale 80%](https://i.imgur.com/S08NNLu.png)
---

Google アナリティクスを有効化します。（必須ではありません）

アナリティクス用のアカウントを求められた場合はDefaultのアカウントを選びましょう。

.left-column[![:scale 110%](https://i.imgur.com/o5Gy6Ql.png)]

.right-column[![:scale 110%](https://i.imgur.com/60ibBSM.png)]

---

これでプロジェクトの準備は完了です

![:scale 40%](https://i.imgur.com/SRuQ83p.png)

---
layout:true
## データベース（Firestore）の作成
---

次に、Firestore の機能を使ってデータベースを作成します。

まずは、[こちら](https://console.firebase.google.com/u/0/) から作成したプロジェクトを選択します。

![:scale 50%](https://i.imgur.com/OEYNshc.png)
---

左のメニューから「Firestore Database」（Cloud Firestore）を選択して下さい。

![:scale 70%](https://i.imgur.com/10KzHVS.png)
---

このような画面になったら「データベースの作成」を行います。

![:scale 50%](https://i.imgur.com/hPSDky3.png)
---

「本番環境モードで開始」を選択します。

![:scale 60%](https://i.imgur.com/eL7xc6K.png)
---

ロケーションを選択します。（`asia-northeast1`(東京) がオススメ）

![:scale 45%](https://i.imgur.com/Lp1sQaJ.png)

数十秒ほど待って、画面が移行すればここの作業は完了です。

[Cloud Firestore](https://firebase.google.com/docs/firestore/quickstart?authuser=1) についての説明はこちらから確認できます。

---
layout:true
## Firebase CLI を利用した Deploy
---

左のメニューの「プロジェクトの概要」をクリックし、

「開始するにはアプリを追加してください」と書かれた箇所のウェブアイコン 「`</>`」 をクリックしてください。
![:scale 65%](https://i.imgur.com/10KzHVS.png)

---

アプリのニックネームを入力し、Firebase Hostingの設定をします。

![:scale 70%](https://i.imgur.com/4gXKQJT.png)

---

「次へ」を選択します。

※「Firebase SDKの追加」の作業は今回は不要です

![:scale 60%](https://i.imgur.com/oAyZQYN.png)

---

「次へ」を選択します。

※Firebase CLIのインストールはこの後行います。

![:scale 70%](https://i.imgur.com/vv6uNDV.png)

---

この README がある Repository を clone します。

```bash
$ git clone https://github.com/hackujp/firebase-tutorial
$ cd firebase-tutorial
```

gitが使えない場合は、[githubのページ](https://github.com/hackujp/firebase_tutorial)から
`[Code]`メニューの中の「Download ZIP」でダウンロードをし、zipファイルの展開をしましょう。その後ターミナルでそのフォルダ直下(firebase-tutorial/)の階層に移動してください。

![:scale 50%](https://imgur.com/iGIvRx3.png)

---

Firebase CLI をインストールします

Node.js を利用できる環境上であれば、 `npm` を利用してインストールができます。

```bash
$ npm install -g firebase-tools
```

Node.js を利用できる環境上でなければ、下記リンクを参考にインストールを行いましょう。
※Windowsの場合、ダウンロードに使うブラウザはChromeなど、Edge以外を推奨します

https://firebase.google.com/docs/cli?hl=ja

---

![:scale 90%](https://i.imgur.com/vt8b9M3.png)

完了まで1分ほど待機、完了したら「次へ」

---

ここからは画面上の指示に従って、デプロイ（Web上へのアップロード）を実施します。

![:scale 70%](https://i.imgur.com/cRoAjA2.png)

---

Firebase CLI でログインを実施します。（ブラウザでFirebaseのGoogleアカウントへのアクセスを許可してください）

***以降、CLIでのfirebaseの作業は必ず firebase-tutorial ディレクトリの中で実行するようにしてください。(publicディレクトリの中などだとうまく動作しません)***

```shell
$ firebase login
```

![:scale 95%](https://i.imgur.com/Au8iv8V.png)

---

`firebase init` コマンドを実行して、いくつか質問をされるので space で回答します。

```shell
$ firebase init
```

*Firestore: Configure security rules and indexes files for Firestore* と
*Hosting: Configure files for Firebase Hosting and (optionally) set up GitHub Action deploys*
の **2つ** を space で選択して、Enter を押下します。
![:scale 70%](https://imgur.com/Rz5tisf.png)

---

![:scale 90%](https://i.imgur.com/4gh1FRo.png)

次の質問 `Please select an option:` には `Use an existing project` を Enter で選択します。

![](https://i.imgur.com/qXP3LOp.png)
---

`Select a default Firebase project for this directory: ` には、先程作成したプロジェクト名を Enter で選択しましょう。（以前に Firebase を別の用途で使用したことがある人は複数提示されます）

![](https://i.imgur.com/ItQqmno.png)
---

Firestore Setup と出てくる

`What file should be used for Firestore Rules? ` には何も選択せず Enter

![](https://i.imgur.com/2bcRbi9.png)

`File firestore.rules already exists. Do you want to overwrite it with the Firestore Rules from the Firebase Console?` は `N` を入力して Enter

![](https://i.imgur.com/B7qTwM9.png)
 
`What file should be used for Firestore indexes?` は何も選択せず Enter

![](https://i.imgur.com/KfOh26o.png)

---

`File firestore.indexes.json already exists. Do you want to overwrite it with the Firestore Indexes from the Firebase Console?` は `N` を入力して Enter

![](https://i.imgur.com/nXTAzOV.png)

`? What do you want to use as your public directory?` は何も選択せず Enter

![](https://i.imgur.com/jzibUS5.png)

`Configure as a single-page app (rewrite all urls to /index.html)? (y/N) ` は `y` を入力して Enter

![](https://i.imgur.com/9GZGqjV.png)

`File public/index.html already exists. Overwrite?` は `N`

![](https://i.imgur.com/Tr8dZAC.png)

---

ここまで実施できたら、`firebase serve` で このレポジトリのプログラムを動かしてみましょう。

```shell
$ firebase serve
```

下記のようなコマンドの実行結果となるので、表示された http://localhost:5000 を開いてみましょう。

![](https://i.imgur.com/Xo7o2iu.png)

---

下記のように表示されれば成功です。

![:scale 80%](https://i.imgur.com/G9JK7Ot.png)

---

`firebase serve`はターミナルを選択した状態で、`Ctrl + C` (Windowsの場合は `Command + C`) で停止します。

動作が確認できたら、Firebase Hosting の機能を利用して、クラウド（インターネット）上に Deploy します。

```shell
$ firebase deploy
```

Deploy 後、ターミナルに表示された「Hosting URL: 」のURLか、
ブラウザの下記で表示されるリンクからアクセスします。

![](https://i.imgur.com/QyLuqWO.png)

---

下記のように表示されれば成功です。これで、このページはインターネット上に公開され、他の人からも閲覧できる状態になりました。

![:scale 75%](https://i.imgur.com/G9JK7Ot.png)

`firebase serve` ではローカル上（自分のPC上）でプログラムを実行させているのに対し、`firebase deploy` ではクラウド上に Deploy してインターネット上に公開させているということを理解しましょう。

---
layout:true
## HTML の編集が反映されることを確認する
---

次に、自分のプロジェクトで `public/index.html` をエディタなどで開き、下記箇所を変更します。

`public/index.html`

変更前
```htmlembedded=34
<div id="message">
  <h2>Welcome</h2>
  <h1>Firebase Hosting Setup Complete</h1> <!-- この行を編集 -->
  <p>You're seeing this because you've successfully setup Firebase Hosting. Now it's time to go build something extraordinary!</p>
  <a target="_blank" href="https://firebase.google.com/docs/hosting/">Open Hosting Documentation</a>
</div>
<p id="load">Firebase SDK Loading&hellip;</p>
```
---

変更後

```htmlembedded=34
<div id="message">
  <h2>Welcome</h2>
  <h1>テスト</h1>
  <p>You're seeing this because you've successfully setup Firebase Hosting. Now it's time to go build something extraordinary!</p>
  <a target="_blank" href="https://firebase.google.com/docs/hosting/">Open Hosting Documentation</a>
</div>
<p id="load">Firebase SDK Loading&hellip;</p>
```

変更が完了したら、firebase serveで確認をします

```shell
$ firebase serve
```
---

下記のようにWelcomeの文字列がテストに変更されていれば完了です。

（自分の好きな文章に変えて試してもOKです）

![:scale 70%](https://i.imgur.com/1C4bh8m.png)

---
layout:true
## ToDo アプリ を動かしてみる
---

いよいよTodoアプリを動かしてみましょう。

既存の index.html をバックアップしたうえで、sample.htmlを index.html に置き換えます。

```shell
$ mv public/index.html public/_index.html 
$ cp public/sample.html public/index.html
```
※コマンドでの変更でなくとも、publicフォルダの中のファイル名が、「index.html」→「_index.html」に、「sample.html」→「index.html」に変更できていればOKです。

![:scale 20%](https://imgur.com/5VAAAuN.png)

---

ローカルで確認します。

```shell
$ firebase serve
```

`firebase serve`ができたら、先ほどと同じ URL をブラウザから開き、下図のような表示になるかを確認します。[こちら](https://www.atmarkit.co.jp/ait/articles/1403/20/news050.html) の方法を用いて、PC上でもスマホでの挙動をエミュレートして確認する事ができます。


.left-column[PC]

.right-column[スマホ]

.left-column[![:scale 90%](https://i.imgur.com/rULzRnp.png)]

.right-column[![:scale 45%](https://i.imgur.com/JRmyXfI.png)]

---

うまく動くことを確認できたら、deployをして公開しましょう
```shell
$ firebase deploy
```

### これでFirebaseチュートリアルは終了です！お疲れさまでした

*実際にどう動いているか、という点についてはソースコードにコメントをつけて簡単に説明を加えています。[Firebase のドキュメント](https://firebase.google.com/docs/) と合わせてご確認下さい。*

余力があれば、`firebase serve`で確認しながら、少しづつ自分でコードにアレンジを加えてみてみると良い学習になると思います！

---
layout:false
## その他、Firebase の機能例

* [Firebaseの各機能を3行で説明する](https://qiita.com/shibukk/items/4a015c5b3296563ac19d)

## 参考リンク　

* [Google の Firebase サンプル](https://firebase.google.com/docs/samples?authuser=1#web)
* [Firebase入門 フリマアプリを作りながら、認証・Firestore・Cloud Functionsの使い方を学ぼう！](https://employment.en-japan.com/engineerhub/entry/2019/06/07/103000)
