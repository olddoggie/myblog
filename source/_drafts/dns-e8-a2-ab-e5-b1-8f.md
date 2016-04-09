---
title: DNS 被屏
tags:
  - dns
  - 域名国内无法访问
id: 700
categories:
  - web
date: 2015-12-08 20:08:48
---

刚刚架设好博客到新域名的时候，自己感觉还是相当好的，毕竟之前用了很久的域名一直在国内访问很通顺。但不知道究竟是本次改用了com顶级域名，还是之前一直拿博客作同步试验的缘故，突然之间国内就访问不能了，更恶心的是一旦访问就跳到一个韩国网站，myGod！

	之后试了很多方法，比如用linode的DNS服务，不行；又切换会namecheap的default DNS，配置有问题，导致在代理的情况下也访问不了；再发了许多的tickets，和数名网站support人员的舌战之后，终于把namecheap的默认配置给修好了，结果又回到了原点，国外ok，国内歇菜。

	无奈之下，打算尝试一下国内的DNS服务商，于是注册了[360dns](http://www.360dns.com/)，不要误会，他和360没有任何关系，直接申请用了免费的DNS，然后再把domain和Ip重新做了配置：

> @ &nbsp;&nbsp;&nbsp;&nbsp;A &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;139.162.16.182&nbsp;
> www CNAME jk-vv.com
最后回到namecheap，把DNS指向360dns的服务器，现在刚刚改完，等待48小时看结果。

	希望这次能够秒开。 Amen！

<div id="search_list" style="padding:0px;margin:0px;font-family:宋体;background-color:#FFFFFF;">
	<table cellpadding="0" cellspacing="0" align="center" id="nodisp993733" style="width:750px;">
		<tbody>
			<tr>
				<th width="6%" style="font-weight:500;text-align:center;">

				</th>
				<th width="14%" style="font-weight:500;text-align:center;">

				</th>
				<th width="10%" style="font-weight:500;text-align:center;">

				</th>
				<th id="view_2" width="10%" style="font-weight:500;text-align:center;">

				</th>
				<th width="15%" style="font-weight:500;text-align:center;">

				</th>
				<th width="9%" style="font-weight:500;text-align:center;">

				</th>
				<th width="8%" style="font-weight:500;text-align:center;">

				</th>
				<th width="5%" style="font-weight:500;text-align:center;">
				</th>
				<th width="18%" style="font-weight:500;">
					<div id="line_right_2" style="padding:0px;margin:0px;">
						<div style="padding:0px;margin:0px;">
							&nbsp;
						</div>
					</div>
				</th>
			</tr>
		</tbody>
	</table>
</div>