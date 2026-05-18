---
title: "CMLでOSPFの基本設定を試す"
description: "CMLでOSPFの基本設定を試す記事"
pubDate: 2026-05-18
tags: ["Network", "CML", "OSPF"]
---
## 概要
CMLを使用してOSPFの基本設定を試します。

## CML(Cisco Modeling Labs)とは
ネットワークシミュレーションツールです。
PC上で疑似的にルータやスイッチを配置して、設定を試すことができます。

## 使用ノード
IOLを使用します。
![画像1](/images/02_OSPF_CML/1.png)

## ネットワーク構成
![画像2](/images/02_OSPF_CML/2.png)
ルータ(IOL)を配置し、アドレス情報を記載した画面になります。  
ルータ間のリンクも繋げています。  
Router01とRouter02でOSPFの設定をします。

## 設定内容
### Router01
```
hostname Router01

interface ethernet0/0
 ip address 192.168.20.1 255.255.255.0
 no shutdown

interface ethernet0/1
 ip address 192.168.10.1 255.255.255.0
 no shutdown

router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
```
 
 ### Router02
```
hostname Router02

interface ethernet0/0
 ip address 192.168.20.2 255.255.255.0
 no shutdown

interface ethernet0/1
 ip address 192.168.30.1 255.255.255.0
 no shutdown

router ospf 1
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 aera 0
```

### Router10
```
hostname Router10

interface ethernet0/0
 ip address 192.168.10.10 255.255.255.0
 no shutdown

ip route 0.0.0.0 0.0.0.0 192.168.10.1
```

### Router20
```
hostname Router20

interface ethernet0/0
 ip address 192.168.30.10 255.255.255.0
 no shutdown

ip route 0.0.0.0 0.0.0.0 192.168.30.1
```

## 設定確認
#### ネイバーの情報確認
```
Router01#show ip ospf neighbor 

Neighbor ID     Pri   State           Dead Time   Address         Interface
192.168.30.1      1   FULL/BDR        00:00:37    192.168.20.2    Ethernet0/0
```
ステータスがFULLとなっています。

#### LSDBの確認
```
Router01#show ip ospf database 

            OSPF Router with ID (192.168.20.1) (Process ID 1)

                Router Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum Link count
192.168.20.1    192.168.20.1    560         0x80000005 0x00C6D7 2         
192.168.30.1    192.168.30.1    516         0x80000005 0x00472E 2         

                Net Link States (Area 0)

Link ID         ADV Router      Age         Seq#       Checksum
192.168.20.1    192.168.20.1    560         0x80000001 0x0077BE
```

#### ルーティングテーブルの確認
```
Router01#show ip route 
～省略～
Gateway of last resort is not set

      192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.10.0/24 is directly connected, Ethernet0/1
L        192.168.10.1/32 is directly connected, Ethernet0/1
      192.168.20.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.20.0/24 is directly connected, Ethernet0/0
L        192.168.20.1/32 is directly connected, Ethernet0/0
O     192.168.30.0/24 [110/20] via 192.168.20.2, 00:12:45, Ethernet0/0
```

#### Router10からRouter20へping
```
Router10#ping 192.168.30.10
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.30.10, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/2/3 ms
```
pingが成功しています。
