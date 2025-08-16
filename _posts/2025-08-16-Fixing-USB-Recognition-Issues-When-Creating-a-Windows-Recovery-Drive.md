---
title: 回復ドライブの作成時にUSBが認識されない
date: 2025-08-16 00:00:00 +0900
tags: [Windows, Recovery Drive, USB, Troubleshooting, Command Prompt, PC Repair]
---

<!-- 本記事では、Windowsの回復ドライブ作成時にUSBが認識されない問題について解説します。 -->
<!-- この記事は、https://shop.applied-net.co.jp/blog/cate_news/24894/ の内容に基づいています。 -->

## はじめに

Windowsの回復ドライブを作成する際、USBドライブが認識されないという問題に直面することがあります。
アプライドの記事で、かなり有益な情報があり、良く参考にしているのだが、なぜ解決できるのか、という仕組みの部分をよく知らなかったのでちょっと調べてみる。

## 問題の背景

該当の記事はこちら。\
[【テクニカルサービス事例集】Windowsのパソコンで回復ドライブ作成時にUSBが認識されない場合の対処法](https://shop.applied-net.co.jp/blog/cate_news/24894/)\
メーカー製のPCで、Windowsの回復ドライブを作成しようとした際に、「USBフラッシュドライブの接続 ドライブは32GB以上のデータを格納できる必要があり、ドライブ上のすべてのデータは削除されます」
というメッセージが表示され、回復ドライブが作成できないという問題に対し、コマンドプロンプトを使用して解決する方法を紹介しています。\
実行するコマンドは以下のものになるのですが、実際にやってみたこともありますが何をやっているのか全然理解していなかったので、調べてみました。

```bash
C:¥Windows¥system32>cd C:¥Recovery¥OEM
C:¥Recovery¥OEM>del install *.*
C:¥Recovery¥OEM¥*.*よろしいですか (Y/N)? y
C:¥Recovery¥OEM>
```

## C:¥Recovery¥OEMとは何か
C:¥Recovery¥OEMは、Windowsの回復環境に関連するファイルが格納されているディレクトリっぽいです。具体的には、下記のファイルが含まれています。

* どのスクリプトを実行するかを定義する PC のリカバリーの構成ファイル (ResetConfig.xml)
* 拡張スクリプト
* 拡張スクリプトに必要なファイル

[PC のリカバリーへの拡張スクリプトの追加 - Microsoft Learn](https://learn.microsoft.com/ja-jp/windows-hardware/manufacture/desktop/add-a-script-to-push-button-reset-features?view=windows-11)

## `del install *.*` とは何をやっているのか
上記のコマンドでは、`C:¥Recovery¥OEM¥`の中で、del install*.*を実行しています。
この構文では、OEMフォルダの中の、installと名の付くファイルを拡張子関係なくすべて削除しています。\
[del - Microsoft Learn](https://learn.microsoft.com/ja-jp/windows-server/administration/windows-commands/del)

では、OEMフォルダの中のinstallと名の付くファイルは何かというと、代表的なものに`install.wim`があります。これが、まさにWindowsのイメージファイルで、回復ドライブの作成に必要なファイルが含まれています。

[PC のリカバリ機能を展開する - Microsoft Learn](https://learn.microsoft.com/ja-jp/windows-hardware/manufacture/desktop/deploy-push-button-reset-features?view=windows-11)

## なぜこれで解決するのか
ご存じかと思いますが、回復ドライブを作成するにあたって、USB内のデータがすべて削除されます。\
おそらく、この際にUSBメモリがFAT32形式でフォーマットされているのではないかと考えられます。下記はSurfaceの記事ですが、そのような文言があります。\
[Surface の USB 回復ドライブの作成と使用 - Microsoft サポート](https://support.microsoft.com/ja-jp/surface/surface-%E3%81%AE-usb-%E5%9B%9E%E5%BE%A9%E3%83%89%E3%83%A9%E3%82%A4%E3%83%96%E3%81%AE%E4%BD%9C%E6%88%90%E3%81%A8%E4%BD%BF%E7%94%A8-677852e2-ed34-45cb-40ef-398fc7d62c07)

そして、FAT32形式では、4GBを超えるファイルを保存できません。\
そのため、install.wimが4GBを超える場合、USBメモリに保存できないため、回復ドライブの作成ができないという問題が発生しているのではないかと考えられます。

ちなみに、こういうことはよくあるのですが、イメージを自作するときには、手動で分割する方法もあるようです。\
[Windows 10のFAT32のISOサイズの問題の解決：Install.wimの分割または抽出 - Dell 日本](https://www.dell.com/support/kbdoc/ja-jp/000127789/)\
この記事にも下記のような記述があるので、install.wimが4GBを超えているので、回復ドライブが作れないという問題が発生している、と考えられそうですね。
>MicrosoftのWindows 10ボリューム ライセンス用ISOファイルや、おそらくその他のISOファイルも、合計4 GBを超えるサイズですが、4 GBを超えるinstall.wimも含まれています。たとえば、FAT32ファイル システムを使用してUSBスティックを作成することはできず、代わりにNTFSを使用する必要があります。

## まとめ、というかなんというか
該当の記事では、FAT32の4GB縛りを回避するために、C:¥Recovery¥OEMフォルダ内のinstall.wimファイルを削除しているようです。
気になるのは、これをやった時に、メーカーが提供している独自のイメージを利用した回復機能が損なわれるのではないか、という点です。
ほんとうはそこについても検証したいのですが、いかんせん、手元にOEM製PCない(あったとしても、クリーンインストール済みのため、OEMの回復機能が使えない)ので検証不可なのが悔やまれます。
ちなみに、Geminiに聞いてみたところ、以下のような回答がありました。

> OEMの回復機能は、install.wimがなくても動作する可能性\
ただ、OEMの回復機能は、C:¥Recovery¥OEM¥ResetConfig.xmlに定義されているスクリプトを実行することで、PCを初期状態に戻す機能であるため、install.wimがなくても、ResetConfig.xmlに定義されているスクリプトが正常に動作すれば、回復機能は利用できるのではないかと考えています。
ただし、これはあくまで推測に過ぎないため、実際に試してみることをお勧めします。

---
**参考資料:**
[【テクニカルサービス事例集】Windowsのパソコンで回復ドライブ作成時にUSBが認識されない場合の対処法](https://shop.applied-net.co.jp/blog/cate_news/24894/)
