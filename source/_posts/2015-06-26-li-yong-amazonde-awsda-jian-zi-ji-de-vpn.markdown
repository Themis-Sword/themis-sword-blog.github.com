---
layout: post
title: "利用Amazon的AWS搭建自己的VPN"
date: 2015-06-26 13:21:35 +0800
comments: true
categories: web
keywords: Amazon, AWS, VPN
description: 利用Amazon的AWS搭建自己的VPN
---
{% img /images/aws.jpg %}
####預備
1. 申請[Amazon AWS服務](https://aws.amazon.com)
2. 進入[AWS控制台](http://console.aws.amazon.com)，選擇亞太區東京的數據中心
3. 在Compute下創建EC2實例，選擇Ubuntu-64位操作系統，下載SSH密鑰文件並保存好
4. Security Group下除了默認的SSH的22端口打開，增加一個新的TCP端口1723，以建立PPTP服務
5. 啟動實例
6. 在Elastic IP下申請一個固定的IP地址，並與上述實例綁定(如果停止實例，一定要把Elastic IP解除綁定，否則將被計費)<!--more-->

####從本地連接實例
1. Open an SSH client. ([find out how to connect using PuTTY](https://docs.aws.amazon.com/console/ec2/instances/connect/putty))
2. Locate your private key file (xxxx.pem). The wizard automatically detects the key you used to launch the instance.
3. Your key must not be publicly viewable for SSH to work. Use this command if needed:

		chmod 400 xxxx.pem

4. Connect to your instance using its Elastic IP:
127.0.0.1
Example:

		ssh -i xxxx.pem ubuntu@127.0.0.1  

Please note that in most cases the username above will be correct, however please ensure that you read your AMI usage instructions to ensure that the AMI owner has not changed the default AMI username.  
If you need any assistance connecting to your instance, please see our [connection documentation](https://docs.aws.amazon.com/console/ec2/instances/connect/docs).

####EC2實例下安裝PPTP VPN
#####安装pptpd

	sudo apt-get install pptpd

修改/etc/pptpd.conf文件, 在最下面添加以下2行

	localip 192.168.9.1 
	remoteip 192.168.9.11-30

之後修改/etc/ppp/options.pptpd文件, 加上Google的DNS

	ms-dns 8.8.8.8 
	ms-dns 8.8.4.4

接下来修改/etc/ppp/chap-secrets文件, 配置自己VPN的用户名/密码, 格式如下:

	<username> pptpd <passwd> *

例如  

	name pptpd passwd *

修改/etc/sysctl.conf文件, 添加以下内容

	net.ipv4.ip_forward=1

執行

	sudo /sbin/sysctl -p

重新加载配置

啟用iptables的NAT configuration

	sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

為了保證每次EC2實例重啟後NAT configuration能啟動, 還要修改/etc/rc.local文件, 在exit 0這行上面加上

	iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

最後重啟pptpd服務

	sudo /etc/init.d/pptpd restart

####連接VPN
注意AWS免費賬戶每月只有15GB免費流量，用超了要從**信用卡扣費**