---
title: "WireSharkで通信を確認する"
description: "Wiresharkの記事"
pubDate: 2026-11-10
tags: ["Wireshark", "Network"]
---
### 概要
WireSharkで通信を確認します。  

### 必要な道具を準備する
・WireShark  

### WireSharkを起動する
Wiresharkを起動し、「Adapter for loopback traffic capture」を選択します。  
![画像](/images/04_Wireshark/1.png)  
上部に以下のように入力してEnterを押し、フィルターを設定しておきます。    
```
tcp.port == 8000
```
![画像](/images/04_Wireshark/2.png)  

### 通信を発生させる
PowershellでWebサーバを起動する。
```
python -m http.server 8000 --bind 127.0.0.1
```
![画像](/images/04_Wireshark/3.png)  

別のPowerShellで以下コマンドを実行し、HTTPリクエストを送る。
```
curl.exe http://127.0.0.1:8000/
```
![画像](/images/04_Wireshark/4.png)  

通信が発生しているのが確認できる。

一番上の3行で、TCPの3ウェイハンドシェイクが確認できる。
![画像](/images/04_Wireshark/5.png)  