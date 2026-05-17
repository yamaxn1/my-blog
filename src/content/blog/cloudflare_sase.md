---
title: "CloudflareのZero Trustを自宅のPCで試す"
description: "Cloudflareのブログ記事"
pubDate: 2026-05-17
tags: ["Network", "SASE", "Cloudflare"]
---

## はじめに
CloudflareのZero Trustを試します。  
ここではDNSポリシーの設定を行います。

## CloudflareのZero Trustを試す
「https://www.cloudflare.com/ja-jp/
」にアクセスし、「無料で始める」をクリックします。
![画像1](/images/Cloudflare_DNS_policy/1.png)

メールアドレスとパスワードを入力して「Sign up」を押します。  
![画像2](/images/Cloudflare_DNS_policy/2(2).png)

以下は「Skip」します。
![画像3](/images/Cloudflare_DNS_policy/3(2).png)

左のメニューから、「Protect & Connect」の「Zero Trust」を選択します。
![画像5](/images/Cloudflare_DNS_policy/5(2).png)

「Get started」を押します。
![画像6](/images/Cloudflare_DNS_policy/6.png)

チーム名を決めて入力します。
![画像7](/images/Cloudflare_DNS_policy/7.png)

「Zero Trust Free」を選択します。
![画像8](/images/Cloudflare_DNS_policy/8.png)

「Add payment method」を押します。
![画像9](/images/Cloudflare_DNS_policy/9.png)

支払情報を入力して、「Review and purchase」を押下します。
![画像10](/images/Cloudflare_DNS_policy/10.png)

「Purchase」を押下します。
![画像11](/images/Cloudflare_DNS_policy/11.png)

「Skip this for now」を押します。
![画像12](/images/Cloudflare_DNS_policy/12.png)

右上の×を押します。
![画像13](/images/Cloudflare_DNS_policy/13.png)

「Access controls」の「Policies」を押します。
![画像14](/images/Cloudflare_DNS_policy/14.png)

「Add a policy」を押下します。
![画像15](/images/Cloudflare_DNS_policy/15.png)

下記内容を選択、入力します。  
Plicy name:Allow_Policy  
Action:Allow  
Selector:Emails  
Value：自分のメールアドレス  

右下の「Save」を押します。
![画像16](/images/Cloudflare_DNS_policy/16.png)

「Allow_Policy」が作成されました。
![画像17](/images/Cloudflare_DNS_policy/17.png)

「Team & Resources」の「Devices」を押します。
![画像18](/images/Cloudflare_DNS_policy/18.png)

「Management」タブを選択し、「Device enrollment」の「Manage」を押します。
![画像19](/images/Cloudflare_DNS_policy/19.png)

「Select existing policies」を押します。
![画像20](/images/Cloudflare_DNS_policy/20.png)

「Allow_Policy」にチェックをつけ、Confirmを押します。
![画像21](/images/Cloudflare_DNS_policy/21.png)

右下のSaveを押します。
![画像22](/images/Cloudflare_DNS_policy/22(2).png)

次に、自分のPCにWARPをインストールします。  
※現在は「CloudFlare One Client」になっている様です。  
[Download Cloudflare One Client](https://developers.cloudflare.com/cloudflare-one/team-and-resources/devices/cloudflare-one-client/download/)にアクセスし、「Download latest stable release」を押します。
![画像23](/images/Cloudflare_DNS_policy/23.png)

ダウンロードフォルダの「Cloudflare～.msi」をダブルクリックします。
![画像24](/images/Cloudflare_DNS_policy/24.png)

「Next」  
![画像25](/images/Cloudflare_DNS_policy/25.png)

「Install」  
![画像26](/images/Cloudflare_DNS_policy/26.png)

「このアプリがデバイスに変更を加えることを許可しますか？」で「はい」を選択します。  
「Finish」を押します。

タスクバーの雲のアイコンをクリックします。
![画像27](/images/Cloudflare_DNS_policy/27_2.png)

「次へ」  
![画像28](/images/Cloudflare_DNS_policy/28_2.png)

「同意する」  
![画像29](/images/Cloudflare_DNS_policy/29_2.png)

歯車マークをクリックします。
![画像30](/images/Cloudflare_DNS_policy/30_2.png)

「環境設定」をクリックします。
![画像31](/images/Cloudflare_DNS_policy/31_1.png)

「アカウント」→「Cloudflare Zero Trustにログイン」を押します。
![画像32](/images/Cloudflare_DNS_policy/32_2.png)

「次へ」
![画像33](/images/Cloudflare_DNS_policy/33_2.png)

「同意する」
![画像34](/images/Cloudflare_DNS_policy/34_2.png)

前の手順で入力したチーム名を入力します。
![画像35](/images/Cloudflare_DNS_policy/35_2.png)

「Add policy」で入力した自分のメールアドレスを入力して、  
「Send me a code」をクリックします。
![画像36](/images/Cloudflare_DNS_policy/36.png)

メールを確認し、送られてきたコードを入力します。  
「Sign in」を押します。
![画像37](/images/Cloudflare_DNS_policy/37.png)

「Cloudflare WARPを開く」をクリックします。
![画像38](/images/Cloudflare_DNS_policy/38.png)

大きなスイッチマークをクリックします。
![画像39](/images/Cloudflare_DNS_policy/39_2.png)

スイッチマークが青色に変わり、「接続済み」と表示されました。
![画像40](/images/Cloudflare_DNS_policy/40_2.png)

続いて、悪意のあるサイトを遮断できるようにするため、DNSポリシーを作成します。  
「Traffic policies」の「Firewall policies」を押します。
![画像41](/images/Cloudflare_DNS_policy/41.png)

「DNS」タブで「Add DNS policy」を押します。
![画像42](/images/Cloudflare_DNS_policy/42.png)

Policy name：Block_Security_Threats  
「Traffic」の「Add condition」をクリックします。
![画像43](/images/Cloudflare_DNS_policy/43_2.png)

Selector:Security Categories  
Operator:in  
Value:Malware,Phishing,Spam,Command and Control & Botnet  
![画像44](/images/Cloudflare_DNS_policy/44.png)

少し下へスクロールし、「Action」を「Block」にします。
![画像45](/images/Cloudflare_DNS_policy/45.png)

下へスクロールし、「Create Plicy」を押下します。
![画像46](/images/Cloudflare_DNS_policy/46.png)

「Block_Security_Threats」が表示されています。
![画像47](/images/Cloudflare_DNS_policy/47.png)

DNSポリシーが正しく動作しているか確認するため、
Cloudflareがテスト用に用意しているサイトにアクセスします。

Googleで「Cloudflare test malware」と検索します。
以下のページで下にスクロールします。
![画像48](/images/Cloudflare_DNS_policy/48.png)

「malware.testcategory.com」をコピーしてブラウザに貼り付けます。
![画像49](/images/Cloudflare_DNS_policy/49.png)
![画像50](/images/Cloudflare_DNS_policy/50.png)

「このサイトにアクセスできません」と表示されました。
![画像51](/images/Cloudflare_DNS_policy/51.png)

ログを確認します。  
「Insights」の「Logs」をクリックします。
![画像52](/images/Cloudflare_DNS_policy/52.png)

「DNS query logs」を押します。
![画像53](/images/Cloudflare_DNS_policy/53.png)

「Action」を「Blocked」にして、「Apply filters」を押します。
![画像54](/images/Cloudflare_DNS_policy/54.png)

「malware.testcategory.com」がBLOCKされているようです。
![画像55](/images/Cloudflare_DNS_policy/55_2.png)
