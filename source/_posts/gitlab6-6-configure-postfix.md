---
title: gitlab6.6 简单配置 postfix 发邮件
tags:
  - gitlab
  - postfix
  - sendmail
categories:
  - 技术
  - linux
date: 2014-03-04 22:07:56
---
## **Use postfix to send email**

### **Configure the sendmail/postfix**

- sudo yum remove sendmail;sudo yum install postfix

- vi /etc/postfix/main.cf

``` xml
	myhostname = mail.XXXX.com  (XXXX.com 是你公司的域名，比如 people@company.com, company.com 是domain)
	mydomain = XXXX.com （其实domain可以随便起名字，自己指定你想要的domain名，即出现在@之后的那一部分
	myorigin = $mydomain
	inet_interfaces = localhost （如果想要监听所以的ip，则改成all）
	mydestination = $myhostname, localhost.$mydomain, localhost
	mynetworks = 168.100.189.0/28, 127.0.0.0/8
	relay_domains = $mydestination
	smtp_host_lookup = native（）
```

<!--more-->

<br>

### **Map the [SMTP server] [IP address]**

- vi /etc/hosts

``` xml
	192.168.0.232 XXXX.com
```

- sudo service postfix start<

- echo "Test mail from postfix" | mail -s "Test Postfix" xx@<span>XXXX.com  --> test ok, logs can be seen under: /var/log/maillog

- make sure there is NO SMTP_settings.rb under folder /home/git/gitlab/config/initializers, and
``` xml
    config.action_mailer.delivery_method = :sendmail
```
in /home/git/gitlab/config/environments/production.rb


- modify /home/git/gitlab/config/gilab.yml, make sure the email_from and support_email is using the meaningful words, which will be appeared in the notifications sent out by gitlab server.

- finally, sudo service gitlab restart....

<br>

### **Optional**

It seems that Gitlab cannot successfully send our email by default email settings, so change it to smtp:

- vi /home/git/gitlab/config/environments/production.rb

``` ruby
	config.action_mailer.delivery_method = :smtp
```

 - add new file: /home/git/gitlab/config/initializers/smtp_settings.rb

``` ruby
if Rails.env.production?
    Gitlab::Application.config.action_mailer.delivery_method = :smtp
    ActionMailer::Base.smtp_settings = {
    address: “192.168.0.232”,
    port: 25,
    user_name: “xx”,
    password: “xx”,
    domain: “XXXX.com”,
    authentication: :plain,
    enable_starttls_auto: true
    }
end
```
<br>

### **FAQ**

postfix 日志不能生成, mail.log 被删除的问题

- /etc/init.d/rsyslog restart

<br>

### **Reference**
<a href="http://www.postfix.org/STANDARD_CONFIGURATION_README.html" target="_blank">http://www.postfix.org/STANDARD_CONFIGURATION_README.html</a>

<a href="http://d.hatena.ne.jp/katsuren/20130616/1371392925" target="_blank">http://d.hatena.ne.jp/katsuren/20130616/1371392925</a>

<a href="http://beta.wikiversity.org/wiki/Topic:Git/Setup_gitlab_on_ubuntu" target="_blank">http://beta.wikiversity.org/wiki/Topic:Git/Setup_gitlab_on_ubuntu</a>