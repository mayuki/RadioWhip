RadioWhip
=========
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/)

RSS/Atomフィードがないページからフィードを作ったり、フィードに全文を入れ込むツールです。それPla。

- RSS/Atomフィードのない普通のWebページからフィードを生成する
- フィードのフィルター
  - 本文が省略されている場合に本文をWebページから取得して差し込む
  - フィード中の広告を削除する
  - などなど

自分の環境で使う
----------------
Azure Websitesで利用するのであれば[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/)ボタンをクリックしてデプロイするだけの簡単作業です。

個々の設定やカスタムなフィード生成を行いたい場合にはForkして適当に追加して、それをデプロイしてください。

カスタムフィード
----------------
フィードのないページからフィードを作るには適当にcshtmlファイルを作りましょう(後で書く)

フィードのフィルタ
-----------------
フィードのフィルタは FeedFilter のクエリパラメータにURLを渡して実行します。
```
http://www.example.com/FeedFilter?http://feedblog.ameba.jp/rss/ameblo/toyosakiaki-blog/rss20.xml
```

### フィルターを定義する
フィードのフィルタは自分で増やすことができます。App_Data/RadioWhip/Filter以下にcshtmlを作ることで定義・実装できます。(後で書く)
```cshtml
@RadioWhip.RegisterFilter(
    (item, entryUrl, feedUrl) => entryUrl.StartsWith("http://www.example.com/") ? null : item
)
```
RegisterFilterでフィルターを登録します。nullを返すとフィルターされます。

### 全文を差し込む処理を定義する
全文を差し込む処理の詳細は App_Data/RadioWhip/FullFeed に置くことで定義できます。
```cshtml
@RadioWhip.RegisterHandler(
    (entryUrl, feedUrl) => entryUrl.StartsWith("http://www.example.jp/"),
    (item, content, cq) => cq[".subContentsInner"].Html()
)
```
RegisterHandlerで対象となるURLかどうかのチェックハンドラ(第一引数)と内容を返すハンドラ(第二引数)を登録します。
