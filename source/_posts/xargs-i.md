title: 说好的 xargs -i 呢
tags:
  - shell
  - xargs
categories:
  - 技术
  - linux
date: 2013-12-07 22:25:00
---
关于linux的学习，最好还是<a href="http://linux.vbird.org/linux_basic/0320bash.php" target="_blank">鸟哥的网站</a>。
<br>

xargs 是一个操作linux命令输出数据的搬运工，他得到上一条命令管道传递到他手里的结果(stdout)，进行加工处理，或者继续传递到他的下一个管道中去。

<br>

简单的记录一下在windows cygwin 和 unix linux 环境下面的差异，应该是内核版本的不一样，导致了我在cygwin里用的很好的 **xargs -i {} **命令在 mac的shell里执行不成功，说是根本没有-i 这个参数。
<!--more-->

<br>

在cygwin下， 假设你的目录下有1.txt 2.txt, 那么很方便的可以用类似的指令：ls *.txt | xargs rm 来进行删除（当然这里你完全不要用到xargs，直接rm就可以了，那我不是再说假如你要用嘛 =_=!!）； 那么如果你是要做到把这两个txt结尾的文件做一个拷贝呢，移动到一个叫test的文件夹内呢，那么更上面不一样的是，这个时候你 ls *.txt 的结果是xargs后面命令中的一个中间参数，而不再是一个直接的操作对象了，这个时候你当然可以使用反引号（就是那个卖萌的~键，当然是不用按shift的）来操作，那么如果使用xargs用的漂亮一些，就要用到 -i 参数了：ls *.txt | xargs -i mv {} ./test .这样的用法在cygwin 的模拟unix 环境下屡试不爽，但是很奇怪的是，在linux和mac机器下，没有-i这个参数，真是大水冲了龙王庙。。

<br>

怎么办呢，只好再回头去查查xargs的参数列表吧，发现竟然他妈的木有类似-i作用的参数，尼玛这不是坑爹嘛！谷歌也查不到什么实质性的关于 -i 参数对应的东西，可见这个问题是多么的没有意义T^T...

<br>

不钻牛角尖了，最后决定直接用xargs 内接 sh -c ‘’来解开问题(个人感觉，这样做不就和反引号一个尿性嘛，不够风骚=.=)，最终linux/mac下的命令：ls *.txt | xargs -n1 sh -c 'mv $0 ./test'

<br>

解释下，在查参数的时候，知道了一些本来不晓得的用途，比如： xargs -t 可以输出命令处理的命令本身，方便矫正。 再比如后来使用的，-n 参数， 因为如果你有1.txt 2.txt, 那么你 mv 的时候到底是用$0呢， 还是$1呢，如果你根本不知道有几个txt，那不就翔了嘛。。所以一般会要用到 -n1, 表示一行显示一个数据块，然后你只要用 $0 就可以得到所有的数据了。 比如你有5个txt, 那么你用了 xargs -n1 然后$0 就是 一行一个的1.txt 2.txt ... 5.txt， 当然你的操作也是针对这5个文件来做了。 如果你用的是 -n2的话，那么这一次数据的分隔就是每行显示2个， 即 第一行1.txt 2.txt , 第二行 3.txt 4.txt, 第三行 5.txt, 这个时候，你的 $0 就是第一列的 1.txt 3.txt 5.txt, 而 $1 就是 2.txt 4.txt， 而 $3， 呵呵，是空的。。 其实仔细看xargs 的help 命令， 他显示 -L 和 -n 好像是差不多的意思，还有 -R, -I 之类的，我没有去深究，应该也都是写操作数据的参数吧。

<br>

好了，扯了这么多，一篇很水的文章，算是自己新博客的技术类型的处女篇，真是酱油的厉害。。O(∩_∩)O~ 闭关了一天，明天希望雾霾散去，去浦图走走。