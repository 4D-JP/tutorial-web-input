# tutorial-web-input
Webからデータベースを更新する方法を学ぶことができます。(v14+)

---

はじめに```INIT```メソッドを実行してください。サンプルデータが作成されます。

![](https://github.com/4D-JP/tutorial-web-input/blob/master/images/1.png)

**ポイント**: QRコードは，各レコードのURLです。

ブラウザを起動し，```http://localhost/product/```を開いてください。

**ポイント**: それ以外のURLを入力した場合，自動的にリダイレクトされます。

製品コード（データベースに存在するもの）を入力してください。有効な製品コードであれば，在庫数がひとつずつ増えてゆきます。

![](https://github.com/4D-JP/tutorial-web-input/blob/master/images/2.png)

**ポイント:** バーコードリーダーによる入力を想定しています。```Tab```または```Return```で入力が確定します。

```http://localhost/product/{製品番号}``` にジャンプすれば，個別に在庫数を入力することもできます。

![](https://github.com/4D-JP/tutorial-web-input/blob/master/images/3.png)

**ポイント:** 数字キー，または上下矢印キーで在庫数を更新することができます。連続して矢印キーや数字キーを入力した場合，0.4秒ほどのウエイト時間を置いて最後の値がサーバーに送られます。入力途中の値で在庫数を更新してしまわないためです。

解説
---

*ローカルIPv4アドレスを取得する*

Internet Commandsの[```IT_MyTCPAddr```](http://doc.4d.com/4Dv14/4D-Internet-Commands/14/IT-MyTCPAddr.301-1237736.ja.html)でも取得できますが，今回は```LAUNCH EXTERNAL PROCESS```を使用しています。

```
C_TEXT($0)

C_TEXT($stdIn;$stdOut;$stdErr)

ARRAY LONGINT($pos;0)
ARRAY LONGINT($len;0)

If (Folder separator=":")

  LAUNCH EXTERNAL PROCESS("ifconfig";$stdIn;$stdOut;$stdErr)

  If (Match regex("(?ms)^en0:\\s+.+?inet\\s+(\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3})";$stdOut;1;$pos;$len))

    $0:=Substring($stdOut;$pos{1};$len{1})

  End if 

Else 

  SET ENVIRONMENT VARIABLE("_4D_OPTION_HIDE_CONSOLE";"True")
  LAUNCH EXTERNAL PROCESS("ipconfig.exe";$stdIn;$stdOut;$stdErr)

  If (Match regex("IPv4.+?:\\s+(\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3})";$stdOut;1;$pos;$len))

    $0:=Substring($stdOut;$pos{1};$len{1})

  End if 

End if 
```


