---
title: gitlab6.6 简单配置 postfix 发邮件
tags:
  - email notification
  - gitlab
  - postfix
  - sendmail
id: 342
categories:
  - web
  - 技术
date: 2014-03-04 22:07:56
---

#### 
	<span style="background-color:#00D5FF;">Using postfix to send email:&nbsp;</span> 

	<span>0.&nbsp;&nbsp;&nbsp;&nbsp; </span><span>sudo yum remove sendmail </span>--&gt;<span>&nbsp;sudo yum install postfix</span> 

	<span>&nbsp;</span> 

	<span>1.&nbsp;&nbsp;&nbsp;&nbsp; </span><span>vi /etc/postfix/main.cf:</span><span></span> 

	<span>myhostname = mail.XXXX.com<span style="color:#E53333;"> (XXXX.com 是你公司的域名，比如 people@company.com, company.com 是domain)</span></span> 

	<span>**mydomain =**** XXXX.co****m **<span style="color:#E53333;">（其实domain可以随便起名字，自己指定你想要的domain名，即出现在@之后的那一部分）</span></span> 

	<span>myorigin = $mydomain</span> 

	<span>inet_interfaces = localhost <span style="color:#E53333;">（如果想要监听所以的ip，则改成all）</span></span> 

	<span>mydestination = $myhostname, localhost.$mydomain,
localhost</span> 

	<span>mynetworks = 168.100.189.0/28, 127.0.0.0/8</span> 

	<span>relay_domains = $mydestination</span> 

	**<span>smtp_host_lookup
= native （）</span>** 

	**<span>

</span>** 

#### 
	<u><span><span style="background-color:#00D5FF;">Because xxxx in dns cannot be
connected,so we mapped the </span>**<span style="background-color:#00D5FF;">smpt server </span>**<span style="background-color:#00D5FF;">IP address with it in /etc/hosts…</span></span></u> 

	<span>2.&nbsp;&nbsp;&nbsp;&nbsp; </span><span>vi /etc/hosts:&nbsp;</span> 

	**<span>192.168.0.232</span>**<span>&nbsp;XXXX.com</span> 

	<span>&nbsp;</span> 

	<span>3.&nbsp;&nbsp;&nbsp;&nbsp; </span><span>sudo service postfix start</span> 

	<span style="line-height:1.5;">

</span> 

	<span style="line-height:1.5;">4.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;echo "Test mail from postfix" | mail -s "Test Postfix" xx@<span>XXXX.com &nbsp;--&gt; test ok.&nbsp;</span></span><span style="line-height:1.5;">&nbsp;(</span><span id="__kindeditor_bookmark_start_35__" style="line-height:1.5;"></span><span style="line-height:1.5;">logs</span><span id="__kindeditor_bookmark_end_36__" style="line-height:1.5;"></span><span style="line-height:1.5;">&nbsp;can be seen at:** /var/log/maillog**)</span> 

	<span style="line-height:1.5;"><span>

</span></span> 

	<span>&nbsp;5\. &nbsp; &nbsp; make sure there is NO&nbsp;</span>**smtp_settings.rb **under folder /<span>home/git/gitlab/config/initializers, and&nbsp;**config.action_mailer.delivery_method = :sendmail **in**&nbsp;**<span>/home/git/gitlab/config/environments/production.rb</span></span> 

	<span style="line-height:1.5;"><span>

</span></span> 

	<span>&nbsp;6\. &nbsp; &nbsp; modify&nbsp;**&nbsp;**<span>/home/git/gitlab/config/gilab.yml， make sure the **email_from&nbsp;**and **support_email**&nbsp;is using the meaningful words, which will be appeared in the notifications sent out by gitlab server. Finally, sudo service gitlab restart....</span></span> 

	<span>

</span> 

#### 
	<u><span style="background-color:#00D5FF;">Optional: It seems that
Gitlab cannot successfully send our email by default email settings, so change
it to smtp:</span></u> 

	<span>1\. &nbsp;&nbsp;vi /home/git/gitlab/config/environments/production.rb:</span> 

	**<span>config.action_mailer.delivery_method
= :smtp</span>** 

	**<span>&nbsp;</span>** 

	<span>2.&nbsp;&nbsp;&nbsp;</span><span>add new file: /home/git/gitlab/config/initializers/**smtp_settings.rb **:</span> 

	<span>_if Rails.env.production?_</span> 

	<span>_Gitlab::Application.config.action_mailer.delivery_method = :smtp_</span><span style="line-height:1.5;">_&nbsp;_</span> 

	<span>_ActionMailer::Base.smtp_settings = {_</span> 

	<span>_address: "192.168.0.232",_</span> 

	<span>_port: 25,_</span> 

	<span>_user_name: "xx",_</span> 

	<span>_password: "xx",_</span> 

	<span>_domain: "XXXX.com",_</span> 

	<span>_ authentication: :plain,_</span> 

	<span>_ enable_starttls_auto: true_</span> 

	<span>_}_</span> 

	<span>_end_</span> 

	<span>

</span> 

#### 
	<span style="background-color:#00D5FF;">postfix 日志不能生成 mail.log 被删除得建问题:&nbsp;</span> 

		**/etc/init.d/rsyslog restart** 

&nbsp;

#### 
		<span>Reference:</span> 

	<div>
		[<span style="color:#E53333;">http://www.postfix.org/STANDARD_CONFIGURATION_README.html</span>](http://www.postfix.org/STANDARD_CONFIGURATION_README.html)

	</div>

		<span style="color:#E53333;">[<span style="color:#E53333;">http://d.hatena.ne.jp/katsuren/20130616/1371392925</span>](http://d.hatena.ne.jp/katsuren/20130616/1371392925)</span><span style="line-height:1.5;"></span> 

		[<span style="color:#E53333;">http://beta.wikiversity.org/wiki/Topic:Git/Setup_gitlab_on_ubuntu</span>](http://beta.wikiversity.org/wiki/Topic:Git/Setup_gitlab_on_ubuntu)<span style="color:#E53333;"></span> 

&nbsp;</span> 
</h4>