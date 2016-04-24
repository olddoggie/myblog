---
title: 在vps构建lnmp+Wordpress博客环境
tags:
  - lnmp
  - vps
  - wordpress
  - 博客搭建
categories:
  - 技术
  - web
date: 2013-12-14 08:11:12
---
送妈妈上了车后，自己绕着家门口转了一圈，看着渐渐发亮的天际，听着慢慢喧嚣的街道，早晨的风吹得人毫无睡意。突然想到应该要交代一下自己这个博客是怎么来的。理论上这个应该是紧接着hello夏雪天的一篇文章，只因为一直在忙其他东西，贪图写日记的快感，反倒忘记了一直以来想要写的一篇。

其实也没啥好说的╮(╯▽╰)╭。。。。。

很简单，你要构建自己的主站，首先你要有爱，要有追求。如果文艺小清新什么的还是建议腾讯空间，微信朋友圈，新浪微博，真要写博客的话就去[博客大巴](http://www.blogbus.com/)，[CSDN](http://blog.csdn.net/)等等类似这种地方，现成的东西摆在那里等你调戏，也不要瞎折腾自己了=.=

<!--more-->

好，那么自己建站其实真的很简单。

第一，给自己想好一个有爱有理想的域名，去[godaddy](http://www.godaddy.com/)或者[<span style="color:#E53333;">namecheap</span>](https://www.namecheap.com/)<span style="color:#E53333;"></span>申请。记住域名一定要让自己也让别人看着不起鸡皮疙瘩，以至于进来之后心思全在想，起这么恶心的名字的是他妈怎样的一个怪人啊。。所以千万不要跟本人一样，到头来莫名其妙弄了一个诡异的名字，有点汉堡包夹油面筋的感觉。。还有就是后缀最好是高大上的 org，或者 com 就更好了。还是千万不要跟本人一样， 为了贪图几刀美元，搞了一个info结尾的类似小广告垃圾网站的恶心后缀 (一个info域名好像是3.99刀每年)，为江湖所不齿。。其实第二年的时候我本来是要换域名的，可他妈的namecheap这个坑爹货帮我做了自动续费，你怎么知道老子那张土豪信用卡今年还没有被取缔啊！T^T

第二，也是最基本的，你得有个自己的服务器啊。不一定是要国外vps(Virtual Private Server 虚拟专用服务器)，在云技术崛起的时代，阿里云，亚马逊云什么的都做的很好，应该也是不错的选择，同事小马哥好像就免费的使用过amazon cloud，只不过这家伙貌似长久不更新，后来莫名其妙被亚马逊charge了一笔钱，真是灰头土脸，最后丫用蹩脚的英文去抱人家service的大腿，终于没有白白丢失银子，当然也就从此不敢再质疑我买vps的意义。。。 vps 里比较有名的是[linode](https://www.linode.com/)，但这个太过重型，最便宜的也要20刀一个月，像我这样的轻量级选手也没有什么听众也没有什么商业价值和研究方向的根本用不到这种1GB Ram 的 私服。 我用的是[<span style="color:#E53333;">frantech</span>](http://buyvm.net/)的vps， 最最最低档的配置，128的Ram 和 15G的disk, 15刀一年，写个博客，跑些crontab job 脚本做做research什么的足够了。

第三，有了vps和domain，你就好像那个玉玺在手，天下我有！瞬间条条大路通罗马，罗马也不是一日就日成的。 什么乱七八糟的，然后呢，我们把私服的ip地址用ifconfig得到，并且在namecheap的控制板里绑定你的域名。再然后，我们就可以去vps的控制板里面选择我们要的环境了。谢耳朵用的Debian不大适合我，centos又太老了一点，后来决定还是Ubuntu吧，系统自动安装完了他会给你发邮件和密码，让你用root登陆。登陆之后我们就可以开始lnmp了。 lnmp是什么，请戳[<span style="color:#E53333;">http://lnmp.org/</span>](http://lnmp.org/)<span style="color:#E53333;"></span>， 基本上安装就按照它Install页面上面的来(lnmp包的安装教程很详细了，安装基本就一键式，这点非常好，也足以看出这个环境实在是用户群很大)，建议先用 apt-get update 一下你的包管理器，然后 wget -c http://soft.vpser.net/lnmp/lnmp1.0-full.tar.gz &amp;&amp; tar zxvf lnmp1.0-full.tar.gz &amp;&amp; cd lnmp1.0-full &amp;&amp; ./centos.sh，即开始安装。安装过程中会有一些交互式的录入，基本根据你自己的实际情况，比如mysql的root密码，nginx还是apache啊，要不要php5.3.17啊，virtual host的名字啊(即你的域名)，等等，基本秒懂无难度。安装成功后，就可以看到你的mysql，php，nginx都启动了，通过/root/lnmp {start|stop|reload|restart|kill|status} 查看或者修改你的状态。 同时，你的web主目录入口应该是/home/wwwroot， 此时访问你的域名，应该能看到lnmp的默认页面。 ^_^

第四，万事俱备只欠东风了。WordPress是很好的流行度很广的博客软件，他操作简单，风格清新，插件自由，功能强大，真是居家装逼的必备武器。。 前天起，好像WordPress3.8 也出来了，具体安装也很简单，按照[<span style="color:#E53333;">Wiki- how to insatll wordpress</span>](http://codex.wordpress.org.cn/WordPress%E7%9A%84%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B)[](http://codex.wordpress.org.cn/WordPress%E7%9A%84%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B)或者 直接去[http://codex.wordpress.org/Main_Page](http://codex.wordpress.org/Main_Page)应该马上就会了。总结起来就是先wget下载到安装包，然后解压到 /home/wwwroot的虚拟主机目录里面去，在打开浏览器前，新建mysql的WordPress用户和table，然后就可以访问http://域名/wp-admin/install.php 来执行安装。 友情提示，确保wwwroot有你www用户和组(nginx里配置的访问web目录的默认用户和组)的写入的权限755。 安装完成后即可看到自己的默认首页了，加上后缀/wp-admin 进入管理员后台，高兴的话，可以去Gravatar弄一个全球头像，跟邮箱绑定，你的博客就活了。

第五，因为server性能的缘故，要把lnmp的消耗降到最低。所以还需要把 mysql的配置文件 my.cnf,nginx的conf文件，fast-php的配置文件做一点小改动，把所有的M都改成1M, 把所有的K都改成16K， 改完重启lnmp。

最后，hello world， 日出东方，恭喜你，你丫也是一个blogger了。