---
title: 代理和vpn的区别
date: 2016-06-25 20:54:12
tags:
 - vpn
 - 代理
categories:
 - 折腾
---

之前理解的VPN是为了实现连接到某个局域网内而使用的技术，而代理是为了蕃蔷
用的，后来 经过查找资料还是发现自己的理解还是太片面不够全面。

## VPN

### VPN的简单介绍
首先看一下**VPN**的维基百科:
![VPN](http://blogpicture.dreamleilei.com/blog-vpn-wiki.png)

VPN : *Virtual Private Network*，是私人网络的通信方法。之前可以通过VPN进行蕃蔷

构建VPN的几种方式的比较
常见的VPN链有pptp,l2tp,openvpn
具体的介绍可以参见以下链接:
> [http://www.gxbay.com/1559.html](http://www.gxbay.com/1559.html)

### VPN的蕃蔷原理
 &nbsp;&nbsp; 用VPN 通常需要先安装客户端软件。当你运行 VPN 客户端，它会尝试联到 VPN 服务器
（这点跟加密代理类似）。一旦和 VPN 服务器建立连接，VPN 客户端就会在你的系统
中建立了一个虚拟局域网。而且，你的系统中也会多出一个虚拟网卡（在 Windows 下，
可以用 ipconfig /all 命令，看到这多出来的网卡）。这样一来，你的系统中就有不止
一块网卡。这就引出一个问题：那些访问网络的程序，它的数据流应该通过哪个网卡进出？
&nbsp;&nbsp; 为了解决此问题，VPN 客户端通常会修改你系统的路由表，让那些数据流，优先从虚拟的网卡
进出。由于虚拟的网卡是通往 VPN 服务器的，当数据流到达 VPN 服务器之后，VPN 服务器
再帮你把数据流转向到真正的目的地。
  &nbsp;&nbsp; 前面说了，VPN 为了保证安全，都采用强加密的方式传输数据。这样一来，
  GFW 就无法分析你的网络数据流，进行敏感词过滤。所以，使用墙外的VPN服务器
  ，无形中就能达到翻墙的效果。
  
## 代理

代理一般都是局部的代理,我了解过的代理有http代理和sockets代理，例如chrome
插件下的switchsharp等

## 参考链接
> [http://www.gxbay.com/1559.html](http://www.gxbay.com/1559.html)
> [http://wenda.chinabaike.com/b/38322/2013/1208/713383.html](http://wenda.chinabaike.com/b/38322/2013/1208/713383.html)
> [http://jingpin.org/proxy-ssh-vpn](http://jingpin.org/proxy-ssh-vpn)
> [https://www.zhihu.com/question/25143289](https://www.zhihu.com/question/25143289)
> [https://program-think.blogspot.com/2011/09/gfw-vpn-hotspot-shield.html](https://program-think.blogspot.com/2011/09/gfw-vpn-hotspot-shield.html)
> [https://plus.googleapis.com/+GhostAssassin/posts/c1zb7q6xKMA](https://plus.googleapis.com/+GhostAssassin/posts/c1zb7q6xKMA)
> [http://wrfly.kfd.me/SOCKS%E4%BB%A3%E7%90%86%E5%92%8CHTTP%E4%BB%A3%E7%90%86%E7%9A%84%E5%8C%BA%E5%88%AB/](http://wrfly.kfd.me/SOCKS%E4%BB%A3%E7%90%86%E5%92%8CHTTP%E4%BB%A3%E7%90%86%E7%9A%84%E5%8C%BA%E5%88%AB/)
