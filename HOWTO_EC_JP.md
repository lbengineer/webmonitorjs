# 使い方（フリマサイトを監視する）
「WebMonitorJS」を使って、フリマサイトの「メルカリ」の商品を監視する方法を説明します。

フリマサイトにはお買い得な安い商品を出品する人も多く、そのような商品はすぐに売り切れてしまいます。  
お買い得な商品を買うためには、他の人よりも早く商品を確認して購入しなければいけません。  
ここで説明する方法を実践すれば、出品された新着商品を定期的に監視し、素早く購入することができるようになります。

## WebMonitorJSをインストールする
「WebMonitorJS」はMicrosoftStoreで配信しています。
有料ですが、7日間は無料で使用できるため、お試しで使用することができます。
以下のページからインストールしてください。  
[https://www.microsoft.com/ja-jp/p/webmonitorjs/9p29hwb7rx0r?cid=msft_web_chart&activetab=pivot:overviewtab](https://www.microsoft.com/ja-jp/p/webmonitorjs/9p29hwb7rx0r?cid=msft_web_chart&activetab=pivot:overviewtab)

## 監視するURLを決める
ここではメルカリのページを監視してみます。
特に難しい手順等は無く、メルカリのサイトで監視したい条件で普通に検索を行います。  
試しに以下のような条件のページを監視してみます。  
- 検索ワード：缶バッジ
- 並び替え：出品の新しい順
- 販売状況：販売中

この条件で実際に検索を行い、そのときのURLを取得します。  
ちなみに、以下のようなURLになりますので、このURLを保存しておきます。  
[https://www.mercari.com/jp/search/?sort_order=created_desc&keyword=%E7%BC%B6%E3%83%90%E3%83%83%E3%82%B8&category_root=&brand_name=&brand_id=&size_group=&price_min=&price_max=&status_on_sale=1](https://www.mercari.com/jp/search/?sort_order=created_desc&keyword=%E7%BC%B6%E3%83%90%E3%83%83%E3%82%B8&category_root=&brand_name=&brand_id=&size_group=&price_min=&price_max=&status_on_sale=1)


