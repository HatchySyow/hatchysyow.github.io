---
title: "Dell System Updateについて"
date: 2025-08-03 00:33:00 +0900
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

Firmware更新ツールについては、複数の方法があり、下の図にまとめられています。

![推奨ツール確認フローチャート](assets/images/ka06P0000009DzdQAE.png)

[PowerEdge: ファームウェア更新ツールのご紹介](https://www.dell.com/support/kbdoc/ja-jp/000240818/)

## DSU、という選択肢
今回はその中でも、Dell System Update（DSU）に焦点をあてて、紹介していきます。\
Dell Technologiesからは、PowerEdgeサーバー向けに、Dell System Update（DSU）というツールをリリースしています。これは、サーバーのFirmware、Driverを一括で更新してくれるツールになります。

[Dell System Update（DSU） | Dell 日本](https://www.dell.com/support/kbdoc/ja-jp/000130590/)
>Dell System Update (DSU)は、Dell Update Packages (DUP)をDell PowerEdgeサーバーに適用するために、スクリプトを最適化したアップデート導入ツールです。

これを使用することで、現在、サーバーに搭載されている各コンポーネントのFirmware、Driverのバージョンを知ることができます。
また、Firmware、Driverの最新版の情報(メタデータ)を集めたカタログとの差分を表示し、更新余地があるものについては、カタログのバージョンに更新することが可能です。\
このツールは、[iDRAC](https://www.dell.com/ja-jp/lp/dt/open-manage-idrac)や[OpenManage Server Administrator](https://www.dell.com/support/kbdoc/ja-jp/000132087/)といったサーバー管理ツールがない状態でも、使用することができます。要するに、OS上にツールを入れて実行するだけで、FirmwareやDriverを全部一気に更新できます。

カタログの内容については下記のページを参照ください。このカタログは、毎週月曜日に更新がかかっていて、常に最新のFirmware、Driverが登録されるようになっているようです。DSUの素晴らしい点は、このカタログのバージョンに一括で更新してくれる、というところです。\
[PowerEdge：サーバーのカタログ リンク](https://www.dell.com/support/kbdoc/ja-jp/000132986/)


## 使い方

使用方法は非常に簡単です。詳細については、下記の公式記事を参照いただければと思いますが、ツールをダウンロードして、PowerShell(管理者)上で、下記コマンドを実行するだけ、です。

```bash
PS C:\Windows\system32> DSU
```

[DSU: Windows Serverオペレーティング システムでDELL EMC System Update(DSU)を使用してドライバーとファームウェアをインストールする方法 - Dell 日本](https://www.dell.com/support/kbdoc/ja-jp/000116751/)

カタログから取得してきた情報一覧が表示されるので、そこから、どのFirmware、Driverを更新したいか、を選択し、`C+Enter`を押下することで、インストールできるようになります。
※カタログのすべてのFirmware、Driverを更新したい場合は、`a` を押下します。

以上になります。とにかく、DSUはいいぞ！っていうのを伝えたい、そんな記事でした。