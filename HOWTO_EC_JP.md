# 使い方（フリマサイトを監視する）
フリマサイトにはお買い得な安い商品を出品する人も多く、そのような商品はすぐに売り切れてしまいます。  
お買い得な商品を買うためには、他の人よりも早く商品を確認して購入しなければいけません。  
ここで説明する方法を実践すれば、出品された新着商品を定期的に監視し、素早く購入することができるようになります。

## WebMonitorJSをインストールする
「WebMonitorJS」はMicrosoftStoreで配信している、Windows10向けのアプリケーションです。
有料ですが、7日間は無料で使用できるため、お試しで使用することができます。
以下のページからインストールしてください。  
[https://www.microsoft.com/ja-jp/p/webmonitorjs/9p29hwb7rx0r?cid=msft_web_chart&activetab=pivot:overviewtab](https://www.microsoft.com/ja-jp/p/webmonitorjs/9p29hwb7rx0r?cid=msft_web_chart&activetab=pivot:overviewtab)

## 監視するURLを決める
ここではメルカリのページを監視してみます。他のサイトでも同様の手順になります。  
特に難しい手順等は無く、メルカリのサイトで監視したい条件で普通に検索を行います。  
試しに以下のような条件のページを監視してみます。  
- 検索ワード：缶バッジ
- 並び替え：出品の新しい順
- 販売状況：販売中

この条件で実際に検索を行い、そのときのURLを取得します。  
ちなみに、以下のようなURLになりますので、このURLを保存しておきます。  
[https://www.mercari.com/jp/search/?sort_order=created_desc&keyword=%E7%BC%B6%E3%83%90%E3%83%83%E3%82%B8&category_root=&brand_name=&brand_id=&size_group=&price_min=&price_max=&status_on_sale=1](https://www.mercari.com/jp/search/?sort_order=created_desc&keyword=%E7%BC%B6%E3%83%90%E3%83%83%E3%82%B8&category_root=&brand_name=&brand_id=&size_group=&price_min=&price_max=&status_on_sale=1)

## 商品の情報を抽出するJavaScript
商品の情報を抽出するためには、HTMLを解析して、JavaScriptを書く必要がありますが、HTMLやJavaScriptを理解する必要があるため、ここでは主要なフリマサイトの商品情報を抽出するJavaScriptを掲載しておきます。  
ここの内容をそのままコピペすれば使用することができます。  
なお、ここの内容は2020年5月時点のものとなりますので、対象のサイトのHTMLが変更された場合は使用することができません。

### メルカリ
```
var domain = window.location.protocol + '//' + document.domain;
var items = document.querySelectorAll(".items-box-content > .items-box");
var results = [];
items.forEach(value => {
	results.push({
		"Id": value.querySelector("a").getAttribute("href").split('/')[3],
		"Name": value.querySelector("h3").innerText,
		"Update": value.querySelector(".items-box-price").innerText,
		"HeaderImageUrl": "https:" + document.querySelector("link[rel='icon']").getAttribute("href"),
		"ImageUrl": value.querySelector(".items-box-photo img").getAttribute("data-src"),
		"Url": domain + value.querySelector("a").getAttribute("href"),
	});
});
JSON.stringify(results, undefined, 2);
```

### ラクマ
```
var domain = window.location.protocol + '//' + document.domain;
var items = document.querySelectorAll(".item-list .item");
var results = [];
items.forEach(value => {
	results.push({
		"Id": value.querySelector(".item-box__image-wrapper a").getAttribute("href"),
		"Name": value.querySelector(".item-box__item-name").innerText,
		"Update": value.querySelector(".item-box__item-price").innerText,
		"ImageUrl": value.querySelector(".item-box__image-wrapper img").getAttribute("data-original"),
		"Url": value.querySelector(".item-box__image-wrapper a").getAttribute("href"),
	});
});
JSON.stringify(results, undefined, 2);
```

### オタマート
```
var domain = window.location.protocol + '//' + document.domain;
var icon = domain + document.querySelector("link[rel='icon']").getAttribute("href");
var items = document.querySelectorAll("#main .list-item");
var results = [];
items.forEach(value => {
	results.push({
		"Id": value.querySelector(".name").getAttribute("href"),
		"Name": value.querySelector(".name").innerText,
		"Update": value.querySelector(".price > .number").innerText,
		"ImageUrl": value.querySelector(".thumbnail img").getAttribute("src"),
		"HeaderImageUrl": icon,
		"Url": domain + value.querySelector(".name").getAttribute("href"),
	});
});
JSON.stringify(results, undefined, 2);
```

### ヤフオク(即決)
```
var domain = window.location.protocol + '//' + document.domain;
var items = document.querySelectorAll(".Products__items > .Product");
var results = [];
items.forEach(value => {
  results.push({
    "Id": value.querySelector("h3  a").getAttribute("href"),
    "Name": value.querySelector("h3").innerText,
    "Update": value.querySelector(".Product__price:last-child > .Product__priceValue").innerText,
    "ImageUrl": value.querySelector(".Product__imageData").getAttribute("src"),
    "Url": value.querySelector("h3 a").getAttribute("href"),
  });
});
JSON.stringify(results, undefined, 2);
```

## WebMonitorJSにページを登録する
それでは実際にWebMonitorJSに監視対象のページを登録してみます。メルカリを例にしてみます。

WebMonitorJSを起動すると、監視対象の一覧が表示されます。  

最初はSampleが登録されていますので、これをメルカリのページを監視する設定に変更します。  
「編集」ボタンを押すと編集画面が表示されます。  

以下のように設定を行います。  
- 名称：分かりやすい任意の名前でいいです。メルカリにしておきます。
- 実行対象のURL：【監視するURLを決める】で取得したURLを貼り付けます。
- 実行間隔：監視する間隔を指定します。あまり短い間隔だとサイトに負荷がかかりますので、短くても20-30秒程度にしておきましょう。
- 実行されるJavaScript：【商品の情報を抽出するJavaScript】のメルカリのコードを貼り付けます。

