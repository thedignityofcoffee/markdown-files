# GFW原理和封锁技术

> 原文：[GFW原理和封锁技术](https://xuranus.github.io/2017/10/09/GFW%E5%8E%9F%E7%90%86%E5%92%8C%E5%B0%81%E9%94%81%E6%8A%80%E6%9C%AF)&emsp;作者：[XUranus](https://xuranus.github.io/)

GFW是Great Fire Wall的缩写，即“长城防火墙”。这个工程由若干个部分组成，实现不同功能。长城防火墙主要指TG监控和过滤互联网内容的软硬件系统，由服务器和路由器等设备，加上相关的应用程序所构成。

首先，需要强调的是，由于中国网络审查广泛，中国国内含有“不合适”内容的的网站，会受到政府直接的行政干预，被要求自我审查、自我监管，乃至关闭，所以GFW的主要作用在于分析和过滤中国境内外网络的资讯互相访问。GFW对网络内容的过滤和分析是双向的，GFW不仅针对国内读者访问中国境外的网站进行干扰，也干扰国外读者访问主机在中国大陆的网站。

## 一 关键字过滤阻断

关键字过滤系统。此系统能够从出口网关收集分析信息，过滤、嗅探指定的关键字。主要针对HTTP的默认端口：80端口，因为HTTP传播的内容是明文的内容，没有经过加密，而GFW是一个IDS\(Intrusion detection system\)。普通的关键词如果出现在HTTP请求报文的头部\(如“Host: www.youtube.com”\)时，则会马上伪装成对方向连接两端的计算机发送RST包\(reset\)干扰两者正常的TCP连接，进而使请求的内容无法继续查看。如果GFW在数据流中发现了特殊的内文关键词\(如轮子，达赖等\)时，其也会试图打断当前的连接，从而有时会出现网页开启一部分后突然停止的情况。在任何阻断发生后，一般在随后的90秒内同一IP地址均无法浏览对应IP地址相同端口上的内容。

## 二 IP地址封锁 

IP地址封锁是GFW通过路由器来控制的，在通往国外的最后一个网关上加上一条伪造的路由规则，导致通往某些被屏蔽的网站的所有IP数据包无法到达。路由器的正常工作方式是学习别的路由器广播的路由规则，遇到符合已知的IP转发规则的数据包，则按已经规则发送，遇到未知规则IP的数据，则转发到上一级网关。

而GFW对于境外\(中国大陆以外\)的XX网站会采取独立IP封锁技术。然而部分XX网站使用的是由虚拟主机服务提供商提供的多域名、单\(同\)IP的主机托管服务，这就会造成了封禁某个IP地址，就会造成所有使用该服务提供商服务的其它使用相同IP地址服务器的网站用户一同遭殃，就算是正常的网站，也不能幸免。其中的内容可能并无不当之处，但也不能在中国大陆正常访问。现在GFW通常会将包含XX信息的网站或网页的URL加入关键字过滤系统，并可以防止民众透过普通海外HTTP代理服务器进行访问。

## 三 特定端口封锁 

GFW会丢弃特定IP地址上特定端口的所有数据包，使该IP地址上服务器的部分功能\(如SSH的22、VPN的1723或SSL的443端口等\)无法在中国大陆境内正常使用。

在中国移动、中国联通等部分ISP\(手机IP段\)，所有的PPTP类型的VPN都被封锁。

2011年3月起，GFW开始对Google部分服务器的IP地址实施自动封锁\(按时间段\)某些端口，按时段对www.google.com\(用户登录所有Google服务时需此域名加密验证\)和mail.google.com的几十个IP地址的443端口实施自动封锁，具体是每10或15分钟可以连通，接着断开，10或15分钟后再连通，再断开，如此循环，令中国大陆用户和Google主机之间的连接出现间歇性中断，使其各项服务出现问题。GFW这样的封锁手法很高明，因为Gmail并非被完全阻断，这令问题看上去好像出自Google本身。这就是你们认为Google抽风的原因。

## 四 SSL连接阻断 
GFW会阻断特定网站的SSL加密连接，方法是通过伪装成对方向连接两端的计算机发送RST包\(RESET\)干扰两者间正常的TCP连接，进而打断与特定IP地址之间的SSL\(HTTPS，443端口\)握手\(如Gmail、Google文件、Google网上论坛等的SSL加密连接\)，从而导致SSL连接失败。

当然由于SSL本身的特点，这并不意味着与网站传输的内容可被破译。

## 五 DNS劫持和污染

GFW主要采用DNS劫持和污染技术，使用Cisco提供的IDS系统来进行域名劫持，防止访问被过滤的网站，2002年Google被封锁期间其域名就被劫持到百度。中国部分ISP也会通过此技术插入广告。

对于含有多个IP地址或经常变更IP地址逃避封锁的域名，GFW通常会使用此方法进行封锁。具体方法是当用户向DNS服务器提交域名请求时，DNS返回虚假\(或不解析\)的IP地址。

全球一共有13组根域名服务器\(Root Server\)，目前中国大陆有F、I这2个根域DNS镜像，但现在均已因为多次DNS污染外国网络，而被断开与国际互联网的连接。

DNS劫持和污染是针对某些网站的最严重的干扰。

干扰的方式有两种：

一种是通过网络服务提供商\(Internet Service Provider\)提供的DNS服务器进行DNS欺骗，当人们访问某个网站时，需要要把域名转换为一个IP地址，DNS服务器负责将域名转换为IP地址，中国大陆的ISP接受通信管理局的屏蔽网站的指令后在DNS服务器里加入某些特定域名的虚假记录，当使用此DNS服务器的网络用户访问此特定网站时，DNS服务便给出虚假的IP地址，导致访问网站失败，甚至返回ISP运营商提供的出错页面和广告页面。

另一种是GFW在DNS查询使用的UDP的53端口上根据blacklist进行过滤，遇到通往国外的使用UDP53端口进行查询的DNS请求，就返回一个虚假的IP地址。  

