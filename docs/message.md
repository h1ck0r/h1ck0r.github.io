# 渗透测试之信息收集

!>渗透的本质就是信息收集



## 一、域名信息

> Whois信息

通常来说，就是一个用于查询域名是否已经被注册，以及注册域名的详细信息（如域名所有人、域名注册商、域名注册日期和过期日期等）

站长之家：https://whois.chinaz.com/

阿里云域名信息查询：https://whois.aliyun.com

爱站：https://whois.aizhan.com/

微步：https://x.threatbook.cn/

域名查询网：http://123.4.cn

操作系统whois

> Whois反查

------

WHOIS反查，是指可以通过一个已知域名的WHOIS信息中的部分信息作为条件反过来查询与此条件相匹配的一系列其它域名列表情况。借此我们可以知道该注册人拥有哪些域名，或者说是拥有哪些站点，那些域名的注册信息具体是什么等等相关信息，因此WHOIS反查也可称之为域名反查。

------

​	**Whois反查方式：**

（1）根据已知域名反查，分析出此域名的注册人、邮箱、电话等字段；

（2）根据已知域名WHOIS中的注册邮箱来反查得出其它域名WHOIS中注册邮箱与此相同的域名列表；

（3）根据已知域名WHOIS中的注册人来反查得出其它域名WHOIS中注册人与此相同的域名列表；

------

站长之家：https://whois.chinaz.com/

爱站：https://whois.aizhan.com/reverse-whois/

域名查询网：http://123.4.cn/reverse

> ##### 备案信息查询

常用备案信息查询网站，获取网站的详细信息
ICP备案查询网：http://www.beianbeian.com/
天眼查：https://www.tianyancha.com/
爱站网：https://www.aizhan.com/

## 二、子域名

通过证书收集
https://censys.io/
https://crt.sh/

Google语法
利用搜索引擎查询（site:www.xxx.com）

工具爆破枚举
layer子域名挖掘机、御剑、subDomainsBrute、K8

在线查询
http://sbd.ximcx.cn/
站长工具:http://tool.chinaz.com/subdomain/

## 三、旁站、C段

旁站：是和目标网站在同一台服务器上的其它网站

C端：是和服务器IP处在一个C段的其他服务器

在线：
https://webscan.cc/

fofa、shodan在线工具 语法:ip="106.15.141.18/24"

本地：

```
    namp

    nmap -p 22,21,443,8080-Pn 172.178.40.0/24

    masscan

    masscan -p 22,21,443,8080-Pn --rate=1000172.178.40.0/24
```

goby 自动探测当前网络空间存活的IP及解析域名到IP

K8旁站 K8Cscan是款专用于大型内网渗透的高并发插件化扫描神器，可以用来扫描C段、旁站

御剑1.5这个就不用多说什么了

## 四、网站信息

### 网站架构探测

探测目标站点架构：操作系统、中间件、脚本语言、数据库、服务器、web容器。

cms指纹识别：

cms指纹识别：http://whatweb.bugscaner.com/look/

云悉：http://www.yunsee.cn/info.html

潮汐指纹：http://finger.tidesec.net/

操作系统类型识别:

通过ping目标主机返回的TTL值判断服务器类型 win128 linux 64 ，大小写敏感区分

工具：namp、p0f

综合探测工具：
shodan、whatweb（kali集成）、wappalyzer插件

### WAF信息

扫描工具：whatwaf、wafw00f
在线识别工具：https://scan.top15.cn/web
扫描IP C段，防火墙一般都会有web管理

### 历史漏洞

公开漏洞查询

确认网站的运行的cms或者运行的服务后，可通过公开漏洞进行查询

http://cve.mitre.org/find/search_tips.html

通过查询cve漏洞库查找当前cms或者服务是否存在已知公开漏洞，结合google获取详细漏洞信息

https://www.exploit-db.com/

Google等搜索引擎确定cms或者服务后，在后面加上exp、poc、漏洞等词汇获取详细漏洞信息。

https://vulners.com/

可通过漏洞相关关键字获取详细漏洞信息

历史漏洞查询

确认网站所属单位，可通过查询历史漏洞或者该集团、单位历史漏洞辅助攻击（例如获取内网ip段、历史账号密码）

seebug：https://www.seebug.org/

CNVD：https://www.cnvd.org.cn/

## 五、敏感目录文件

Google语法是万能的

DirBuster（kali自带的一款扫描工具）

Webdirscan（python编写的简易的扫描工具）

御剑（操作简易方便）

dirmap（一款高级web目录扫描工具，功能比较强大）

这些工具都自带字典，也可以自己手动添加，拥有强大的字典也是很关键的

## 六、真实IP查询

确定有无CDN

1.多地ping，看对应IP地址是否唯一，多地ping在线网站：

http://ping.chinaz.com/

2.nslookup，同样是看返回的IP地址的数量进行判断

### 绕过CDN查找网站真实IP

1.DNS历史解析

查看IP与域名绑定的历史记录，可能会存在使用CDN前的记录，相关网站：
https://dnsdb.io/zh-cn/
https://community.riskiq.com
https://x.threatbook.cn/
https://tools.ipip.net/cdn.php

2.查询子域名

毕竟CDN不便宜，所以很多站都是主站做了CDN，而很多小站没做CDN所以可以，通过上面收集到的子域名查询到真实的IP地址

3.网络空间引擎搜索法
通过shadan、fofa等搜索引擎，通过对目标网站的特征进行搜索，很多时候可以获取网站的真实IP

4.利用SSL证书查询
https://censys.io/certificates/

5.邮件订阅
一些网站有发送邮件的功能，如Rss邮件订阅，因为邮件系统一般都在内部，所以就可以通过邮箱获得真实的IP

6.国外访问
一般的站点在国内可能会有CDN，但是在国外的用户覆盖率比较低，所以通过国外的节点进行请求往往能获取真实IP

## 七、端口测试

在获取了真实IP之后，进一步就是对目标IP的端口进行扫描

最常用的就是nmap、masscan还有御剑端口高速扫描等工具

在线端口查询：http://coolaf.com/tool/port

https://tool.lu/portscan/index.html

在我们扫描出对方端口应该针对相应的端口进行一个测试呢，这里就需要对每一个端口的常用服务与常

漏洞进行一个了解。

常见端口服务与攻击方向