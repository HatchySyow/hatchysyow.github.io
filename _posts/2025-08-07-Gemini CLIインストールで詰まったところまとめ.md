---
title: "Gemini CLIをインストールするときに詰まったところ"
date: 2025-08-11 00:00:00 +0900
tags: ["Gemini", "CLI", "PowerShell", "npm"]
pin: false
---

## VS CodeのターミナルでGeminiコマンドが通らない
下記のメッセージで詰まった。
PC上のPowerShellではGemini CLIがうまく動くのに、VS Codeのターミナルでは動かない。
npmコマンドも同様に動かない。

```bash
PS C:\Dev\Project> gemini
gemini : File C:\Users\Anonymous\AppData\Roaming\npm\gemini.ps1
 cannot be loaded because running scripts is disabled on th
is system. For more information, see about_Execution_Polici
es at https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ gemini
+ ~~~~~~
    + CategoryInfo          : SecurityError: (:) [], PSSec 
   urityException
    + FullyQualifiedErrorId : UnauthorizedAccess
PS C:\Dev\Project> npm
npm : File C:\Program Files\nodejs\npm.ps1 cannot be loaded
 because running scripts is disabled on this system. For mo
re information, see about_Execution_Policies at https:/go.m
icrosoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ npm
+ ~~~
    + CategoryInfo          : SecurityError: (:) [], PSSec 
   urityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

## 解決策
結果として、Geminiに聞いて、PowerShellの実行ポリシーを変更することで解決した。

方法は以下の通り。

1. Windowsのスタートメニューから「PowerShell」と検索します。
2. 「Windows PowerShell」を右クリックし、「管理者として実行」を選択します。
3. PowerShellのウィンドウが開いたら、`Set-ExecutionPolicy RemoteSigned`を入力してEnterキーを押します。
4. 実行ポリシーの変更を確認するメッセージが表示され、「はい」または「いいえ」の選択肢が出ます。A（はい、すべて）または Y（はい）を入力してEnterキーを押します。
5. コマンドが正常に完了したら、PowerShellのウィンドウを閉じます。

```bash
Set-ExecutionPolicy RemoteSigned
```

```bash
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\WINDOWS\system32> Set-ExecutionPolicy RemoteSigned

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): A
```

とりあえずこれでGemini CLIとnpmコマンドがVS Codeのターミナルでも動くようになった。
よかったよかった。

今回の問題は、PowerShellの実行ポリシーがデフォルトでスクリプトの実行を禁止しているために発生したっぽい。\
今回は`RemoteSigned`に設定したけど、セキュリティ上の理由から、必要なときだけ変更するのが良いかもしれない。

## 参考情報
[about_Execution_Policies - PowerShell - Microsoft Learn](https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.5)