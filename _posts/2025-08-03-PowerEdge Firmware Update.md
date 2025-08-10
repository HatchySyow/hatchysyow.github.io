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
また、Dell Technologiesなどの各種ベンダーからも、Firmware更新を推奨する情報が出ていたりします。

[PowerEdge:第14世代向けBIOSおよびiDRAC9のアップグレードを推奨 - Dell 日本](https://www.dell.com/support/kbdoc/ja-jp/000215873/)

[デル・テクノロジーズでは、第15世代PowerEdgeサーバーでBIOSとiDRAC9をアップグレードすることを推奨しています - Dell 日本](https://www.dell.com/support/kbdoc/ja-jp/000222827/)

ベンダーから推奨されるバージョンに更新しておくことで、サポートを受ける際に、よりスムーズにトラブルシューティングが行えるようになります。

[PowerEdge：最小、推奨、および最新のコード バージョン - Dell 日本](https://www.dell.com/support/kbdoc/ja-jp/000227230/)

今回は、Dell Technologiesから発売されているx86 Server製品、PowerEdgeのFirmware Updateの方法について、紹介していきます。

### PowerEdgeでFirmware更新をしたい
そもそも、Firmware Updateをするだけなのになぜこの記事を書いているかというと、サーバーにOSをインストールした後、DriverやFirmwareを更新する必要があるのですが、どのFirmwareをバージョンアップすればよいのか、また、どのように実施すればよいのかがわかりづらかったりします。めんどくさいので、一気に更新したい。そういった場合に使えるツールについて、自分なりの解釈として残しておこうと思い、執筆に至りました。

Firmware更新ツールについては、複数の方法があり、どの場合にどれを選べばよいかは、[フローチャート](https://www.dell.com/support/kbdoc/ja-jp/000240818/)を参考にしてください。

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

<details>
<summary>実際の実行結果</summary>

```shell
PS C:\Users\Administrator> dsu
DELL System Update 2.1.1.0
Copyright (C) 2014 -- 2024 DELL Proprietary.
Downloading the Index catalog
Extracting C:\ProgramData\Dell\DELL System Update\dell_dup\CatalogIndex.cab
Reading the Index catalog
Downloading the catalog
Extracting C:\ProgramData\Dell\DELL System Update\dell_dup\Catalog.cab
Reading the catalog ...
Verifying inventory collector installation
Getting System Inventory ...
Determining Applicable Updates ...

|------------DELL System Update-----------|
[ ] represents 'not selected'
[*] represents 'selected'
[-] represents 'Component already at repository version (can be selected only if /e option is used)'
Choose:  q - Quit without update, c to Commit, <number> - To Select/Deselect, a - Select All, n - Select None
[-]1 BIOS
Current Version : 2.19.0 Same as : 2.19.0, Criticality : Urgent, Type : BIOS

[-]2 Firmware for  - Disk 0 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : DK05 Same as : DK05, Criticality : Recommended, Type : Firmware

[-]3 Firmware for  - Disk 1 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : DK05 Same as : DK05, Criticality : Recommended, Type : Firmware

[ ]4 Firmware for  - Disk 2 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : LS08 Upgrade to : LS0C, Criticality : Recommended, Type : Firmware

[ ]5 Firmware for  - Disk 3 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : LS08 Upgrade to : LS0C, Criticality : Recommended, Type : Firmware

[ ]6 Firmware for  - Disk 4 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : LS08 Upgrade to : LS0C, Criticality : Recommended, Type : Firmware

[ ]7 Firmware for  - Disk 5 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : LS08 Upgrade to : LS0C, Criticality : Recommended, Type : Firmware

[ ]8 Firmware for  - Disk 6 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : DE09 Upgrade to : DE11, Criticality : Recommended, Type : Firmware

[-]9 PERC H730 Adapter Controller 0 Firmware
Current Version : 25.5.9.0001 Same as : 25.5.9.0001, Criticality : Recommended, Type : Firmware

[-]10 Dell OS Driver Pack, 18.11.00, A00
Current Version : 18.11.00 Same as : 18.11.00, Criticality : Optional, Type : Application

[-]11 Integrated Dell Remote Access Controller
Current Version : 2.86.86.86 Same as : 2.86.86.86, Criticality : Recommended, Type : Firmware

[-]12 OS COLLECTOR, v6.0, A00
Current Version : 6.0 Same as : 6.0, Criticality : Recommended, Type : Application

[-]13 [0001] Broadcom NetXtreme Gigabit Ethernet
Current Version : 22.00.6 Same as : 22.00.6, Criticality : Optional, Type : Firmware

[-]14 [0002] Broadcom NetXtreme Gigabit Ethernet #2
Current Version : 22.00.6 Same as : 22.00.6, Criticality : Optional, Type : Firmware

Enter your choice : a

|------------DELL System Update-----------|
[ ] represents 'not selected'
[*] represents 'selected'
[-] represents 'Component already at repository version (can be selected only if /e option is used)'
Choose:  q - Quit without update, c to Commit, <number> - To Select/Deselect, a - Select All, n - Select None
[-]1 BIOS
Current Version : 2.19.0 Same as : 2.19.0, Criticality : Urgent, Type : BIOS

[-]2 Firmware for  - Disk 0 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : DK05 Same as : DK05, Criticality : Recommended, Type : Firmware

[-]3 Firmware for  - Disk 1 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : DK05 Same as : DK05, Criticality : Recommended, Type : Firmware

[*]4 Firmware for  - Disk 2 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : LS08 Upgrade to : LS0C, Criticality : Recommended, Type : Firmware

[*]5 Firmware for  - Disk 3 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : LS08 Upgrade to : LS0C, Criticality : Recommended, Type : Firmware

[*]6 Firmware for  - Disk 4 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : LS08 Upgrade to : LS0C, Criticality : Recommended, Type : Firmware

[*]7 Firmware for  - Disk 5 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : LS08 Upgrade to : LS0C, Criticality : Recommended, Type : Firmware

[*]8 Firmware for  - Disk 6 in Backplane 1 of PERC H730 Adapter Controller 0 in Slot 3
Current Version : DE09 Upgrade to : DE11, Criticality : Recommended, Type : Firmware

[-]9 PERC H730 Adapter Controller 0 Firmware
Current Version : 25.5.9.0001 Same as : 25.5.9.0001, Criticality : Recommended, Type : Firmware

[-]10 Dell OS Driver Pack, 18.11.00, A00
Current Version : 18.11.00 Same as : 18.11.00, Criticality : Optional, Type : Application

[-]11 Integrated Dell Remote Access Controller
Current Version : 2.86.86.86 Same as : 2.86.86.86, Criticality : Recommended, Type : Firmware

[-]12 OS COLLECTOR, v6.0, A00
Current Version : 6.0 Same as : 6.0, Criticality : Recommended, Type : Application

[-]13 [0001] Broadcom NetXtreme Gigabit Ethernet
Current Version : 22.00.6 Same as : 22.00.6, Criticality : Optional, Type : Firmware

[-]14 [0002] Broadcom NetXtreme Gigabit Ethernet #2
Current Version : 22.00.6 Same as : 22.00.6, Criticality : Optional, Type : Firmware

Enter your choice : c
Trying to connect using https
Fetching SAS-Drive_Firmware_8FF65_WN64_LS0C_A00 ...
Trying to connect using https
Fetching SAS-Drive_Firmware_MCM30_WN64_DE11_A00 ...
Installing SAS-Drive_Firmware_8FF65_WN64_LS0C_A00
Installed successfully
Installing SAS-Drive_Firmware_MCM30_WN64_DE11_A00
Installed successfully
Done! Please run 'dsu --inventory' to check the inventory
Progress report is available at:C:\ProgramData\Dell\DELL System Update\dell_dup\DSU_STATUS.json
Exiting DSU!
```　
</details>

## まとめ
DSUを使用することで、Dellのリポジトリから簡単に最新のFirmware、Driverを取得してきて、更新することができます。\
ぜひ、PowerEdgeをお使いの方は、DSUを使用してFirmware、Driverの更新を行ってみてください。

ちなみに、DSU以外にも使用できるツールはあります。
代表的なものは、SUUですね。DSUは、Dellのリポジトリから最新のFirmware、Driverを取得してきて更新するのに対し、SUUは、あらかじめダウンロードしたISOイメージから更新を行うツールになります。

[PowerEdge：Dell SUU を使用したファームウェアとドライバのアップデート - Dell 日本](https://www.dell.com/support/kbdoc/ja-jp/000226185/)

SUUはISOイメージから更新を行うため、イメージさえあればインターネット接続がない状態でも、更新を行うことができます。

以上になります。また、気分が乗ったら、SUUとか他のツールについても書いていこうと思います。