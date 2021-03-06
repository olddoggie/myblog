---
title: mysql 日志爆棚
tags:
  - mysql
  - mysql-bin
categories:
  - 技术
  - 数据库
date: 2015-05-14 09:16:27
---
# **问题**
遇到vps无法访问的问题，登陆后发现是服务器的空间15G全部被使用了。起初以为又被黑客黑了，再仔细排查，原来是mysql的log-bin日志记录把空间都撑爆了。。简单Goggle 一下，解决如下。

---
# **解决**

原文：[http://www.codesky.net/article/201107/146680.html](http://www.codesky.net/article/201107/146680.html)

在MySQL数据库中，mysql-bin.000001、mysql- bin.000002等文件是数据库的操作日志，例如UPDATE一个表，或者DELETE一些数据，即使该语句没有匹配的数据，这个命令也会存储到日志文件中，还包括每个语句执行的时间，也会记录进去的。

<!--more-->

<br>

**这样做的目的:**

1. 数据恢复

如果你的数据库出问题了，而你之前有过备份，那么可以看日志文件，找出是哪个命令导致你的数据库出问题了，想办法挽回损失。

2. 主从服务器之间同步数据

主服务器上所有的操作都在记录日志中，从服务器可以根据该日志来进行，以确保两个同步。

<br>

**处理分为两种情况:**

1. 只有一个mysql服务器，那么可以简单的注释掉这个选项就行了。

<code>vi /etc/my.cnf</code> 把里面的log-bin这一行注释掉，重启mysql服务即可。

2. 如果你的环境是主从服务器，那么就需要做以下操作了。

- 在每个从属服务器上，使用SHOW SLAVE STATUS来检查它正在读取哪个日志。

- 使用SHOW MASTER LOGS获得主服务器上的一系列日志。

- 在所有的从属服务器中判定最早的日志，这个是目标日志，如果所有的从属服务器是更新的，就是清单上的最后一个日志。

- 清理所有的日志，但是不包括目标日志，因为从服务器还要跟它同步。

<br>

## **清理日志方法**

1. <code>PURGE MASTER LOGS TO ‘mysql-bin.010’;</code>
2. <code>PURGE MASTER LOGS BEFORE ‘2008-12-19 21:00:00’;</code>
3. 如果你确定从服务器已经同步过了，跟主服务器一样了，那么可以直接<code>RESET MASTER;</code>将这些文件删除。

<br>

## **单例下的彻底清除**

之前发现自己10G的服务器空间大小,用了几天就剩下5G了,自己上传的文件才仅仅几百M而已,到底是什么东西占用了这么大空间呢?今天有时间彻底来查了一下:

看下上面的目录web根目录是放在/home 里面的,所有文件加起来才不到300M,而服务器上已经占用了近5G空间,恐怖吧,最后经我一步一步查询得知,原来是这个文件夹占了非常多的空间资源:

原来如此,是mysql文件夹下的var目录占用空间最大,那里面是啥 内容呢?我们来看下:

发现了如此多的mysql-bin.0000X文件，这是什么东西呢?原来这是mysql的操作日志文件.我才几十M的数据库,操作日志居然快3G大小了。

如何删除mysql-bin.0000X 日志文件呢?

1. <code>/usr/local/mysql/bin/mysql -uroot -p</code>

2. <code>mysql> reset master;</code> --> 清除日志文件

3. 好了，我们再来查看下mysql文件夹占用多少空间?
<code>du -h –max-depth=1 /usr/local/mysql</code>
整个mysql目录才占用163M大小!OK，没问题，既然mysql-bin.0000X日志文件占用这么大空间，存在的意义又不是特别大，那么我们就不让它生成吧。

4. <code>vi /etc/my.cnf</code> 即修改mysql配置文件,我们将<code>log-bin=mysql-bin</code>这条注释掉即可。

5. 重启下MySQL，一切OK啦！

关于MySQL数据库mysql-bin.000001文件的来源及处理方法就介绍到这里了，希望通过本次的介绍能够带给您一些收获吧，谢谢各位浏览！