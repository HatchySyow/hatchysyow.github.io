---
title: "Dell PowerEdge Server"
date: Now
tags: [PowerEdge, x86 Server, Firmware, Driver, BIOS,]
pin: false
---

# Firmware Updateの重要性
突然ですが、皆さん、身近な機器のFirmware Update、していますか？\
してません、という方も多いかもしれませんし、そもそもなんですかそれ、という方も多いかもしれません。

しかし、Firmwareを日ごろから最新のものに更新しておくことで、突然のHardwareの故障率を下げたり、より良い性能を引き出したりすることができるようになるかもしれません。\
今回は、Dell Technologiesから発売されているx86 Server製品、PowerEdgeのFirmware Updateの方法について、紹介していきます。

### PowerEdgeでFirmware更新をしたい
そもそも、Firmware Updateをするだけなのになぜこの記事を書いているかというと、サーバーにOSをインストールした後、DriverやFirmwareを更新する必要があるのですが、どのFirmwareをバージョンアップすればよいのか、また、どのように実施すればよいのかがわかりづらかったりします。めんどくさいので、一気に更新したい。そういった場合に使えるツールについて、自分なりの解釈として残しておこうと思い、執筆に至りました。

Firmware更新ツールについては、複数の方法があり、下の記事にまとめられています。

[PowerEdge: ファームウェア更新ツールのご紹介 | Dell 日本](https://www.dell.com/support/kbdoc/ja-jp/000240818/)

今回はその中でも、Dell System Update（DSU）に焦点をあてて、紹介していきます。

## DSU、という選択肢
Dell Technologiesからは、第14世代以降のPowerEdgeサーバー向けに、Dell System Update（DSU）というツールをリリースしています。これは、サーバーの
>Dell System Update (DSU)は、Dell Update Packages (DUP)をDell PowerEdgeサーバーに適用するために、スクリプトを最適化したアップデート導入ツールです。

[Dell System Update（DSU） | Dell 日本](https://www.dell.com/support/kbdoc/ja-jp/000130590/)\
[DSU: Windows Serverオペレーティング システムでDELL EMC System Update(DSU)を使用してドライバーとファームウェアをインストールする方法 | Dell 日本](https://www.dell.com/support/kbdoc/ja-jp/000116751/)