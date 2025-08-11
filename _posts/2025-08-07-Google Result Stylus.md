---
title: "Googleの検索結果を中央に寄せる方法"
date: 2025-08-06 00:00:00 +0900
tags: [Google, Google Chrome, 拡張機能, Stylus, CSS,]
pin: false
---

# 検索結果を中央に寄せたい
普通にGoogle検索をすると、検索結果がページの左側に寄ってしまって、見づらいことがある。
これを(もう少しでもいいから)中央に寄せたい。

![alt text](assets/img/{190F7B20-B2AB-4B00-8A44-427B8FDEE60C}.png)

## Sytlusという選択肢
[Chrome ウェブストア](https://chromewebstore.google.com/)に、「Stylus」という拡張機能がある。
一言でいえば、ページのCSSを一部変更することができる拡張機能。
これを使って、Googleの検索結果を少し右側に寄せてみる。\
※CSSが何ぞや、というのは[Cascading Style Sheets - Wikipedia](https://ja.wikipedia.org/wiki/Cascading_Style_Sheets)を参照ください。簡単に言えばWebサイトの表示を制御する言語です(多分)。

[Stylus - Chrome ウェブストア](https://chromewebstore.google.com/detail/clngdbkpkpeebahjckkjfobafhncgmne?utm_source=item-share-cb)

とりあえず入れてみる。

## 設定
Geminiに書かせたコードが以下のものです。\
Stylusをインストールしたら、Googleの検索結果を中央に寄せるための設定を行います。
Stylusを開いて、Edit Styleを選択し、URLs on the Domain を設定して、下記コードを入力し、左側メニューのSaveを押下します。

```bash
  #main {
    /* コンテンツ全体の最大幅を設定します。この数値を変更すると幅を調整できます。*/
    max-width: 1500px !important;

    /* コンテンツ全体をページの中央に配置します */
    margin: 0 auto !important;
  }
```

## まとめ
Stylusを使うことで、Googleの検索結果を中央に寄せることができました。\
max-widthのピクセル数を変更すれば、表示幅とともに、どこまで右側に寄せるかをある程度調整可能です。

![alt text](assets/img/{2395D935-582E-4A81-8FBF-2422122AA401}.png)

この設定が少しでも皆様のGoogle検索を快適にする手助けになれば幸いです。
