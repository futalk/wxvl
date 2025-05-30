#  FortiWLM重大安全漏洞曝光，远程攻击者可夺管理员权限   
看雪学苑  看雪学苑   2024-12-20 09:59  
  
近日，Fortinet公司披露了其无线网络管理工具FortiWLM中存在一个严重的安全漏洞，编号为  
CVE-2023-34990。这一漏洞  
允许远程攻击者通过发送特制的Web请求，执行未授权的代码或命令，进而完全控制受影响的设备。FortiWLM作为一款被广泛部署于政府机构、医疗保健组织、教育机构以及大型企业中的集中式管理工具，其安全性的任何疏漏都可能带来严重后果。  
  
  
该漏洞被评定为相对路径遍历缺陷，CVSS得分高达9.8，显示出其危险性极高。Horizon3的安全研究员Zach Hanley在2023年5月首次发现并向Fortinet披露了这一漏洞，然而在长达十个月的时间里，该漏洞未得到修复。在多次催促无果后，Hanley于2024年3月14日决定公开相关信息和概念验证（POC），以期引起用户和厂商的重视。  
  
  
攻击者可以利用FortiWLM的'/ems/cgi-bin/ezrf_lighttpd.cgi'端点中不当的输入验证，通过目录遍历技术读取系统敏感日志文件。这些日志文件中常包含管理员的会话ID，攻击者可以利用这些信息劫持管理员会话，获得设备的高级访问权限。Hanley解释称，FortiWLM的日志记录非常详细，记录了所有经过身份验证用户的会话ID，攻击者可以借此读取任意日志文件，进而获取用户会话ID并滥用认证端点。  
  
  
受影响的FortiWLM版本包括8.6.0至8.6.5和8.5.0至8.5.4。尽管研究员已经发出警告，但由于缺少CVE编号和安全公告，许多用户对这一风险并不知情，也未能及时升级到安全版本。根据Fortinet在2023年12月18日发布的安全公告，CVE-2023-34990漏洞已在8.6.6和8.5.5版本中得到修复，这两个版本在2023年9月底发布。  
  
  
鉴于FortiWLM在关键环境中的部署，它成为了攻击者的一个高价值目标。一旦被远程入侵，可能导致整个网络的中断和敏感数据的泄露。因此，强烈建议FortiWLM的管理员在更新可用时立即应用所有可用的更新，以确保网络和数据的安全。  
  
  
  
资讯来源：  
bleepingcomputer  
  
转载请注明出处和本文链接  
  
  
  
﹀  
  
﹀  
  
﹀  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/Uia4617poZXP96fGaMPXib13V1bJ52yHq9ycD9Zv3WhiaRb2rKV6wghrNa4VyFR2wibBVNfZt3M5IuUiauQGHvxhQrA/640?wx_fmt=jpeg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1UG7KPNHN8E9S6vNnUMRCOictT4PicNGMgHmsIkOvEno4oPVWrhwQCWNRTquZGs2ZLYic8IJTJBjxhWVoCa47V9Rw/640?wx_fmt=gif "")  
  
**球分享**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1UG7KPNHN8E9S6vNnUMRCOictT4PicNGMgHmsIkOvEno4oPVWrhwQCWNRTquZGs2ZLYic8IJTJBjxhWVoCa47V9Rw/640?wx_fmt=gif "")  
  
**球点赞**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1UG7KPNHN8E9S6vNnUMRCOictT4PicNGMgHmsIkOvEno4oPVWrhwQCWNRTquZGs2ZLYic8IJTJBjxhWVoCa47V9Rw/640?wx_fmt=gif "")  
  
**球在看**  
  
****  
****  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/1UG7KPNHN8FxuBNT7e2ZEfQZgBuH2GkFjvK4tzErD5Q56kwaEL0N099icLfx1ZvVvqzcRG3oMtIXqUz5T9HYKicA/640?wx_fmt=gif "")  
  
戳  
“阅读原文  
”  
一起来充电吧！  
  
