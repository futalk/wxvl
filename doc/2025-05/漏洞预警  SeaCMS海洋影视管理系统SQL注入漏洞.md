#  漏洞预警 | SeaCMS海洋影视管理系统SQL注入漏洞   
浅安  浅安安全   2025-05-10 00:02  
  
**0x00 漏洞编号**  
- # CVE-2025-44072  
  
- # CVE-2025-44074  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
海洋CMS是为解决站长核心需求而设计的内容管理系统，一套程序自适应电脑、手机、平板、APP多个终端入口，无任何加密代码、安全有保障，是您最佳的建站工具。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SVAS3cgxzDKew2rV0NjbAZGHtibZEdHTrPpsFL4Mgko79pJicmq3XiarhxlzdCkrkibN4f6Fd8cibA5Y3g/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2025-44072**  
  
**漏洞类型：**  
SQL注入  
  
**影响：**  
获取敏感信息  
  
****  
  
**简述：**  
海洋CMS的  
/admin_manager.php?action=delall接口存在SQL注入漏洞，获取授权的攻击者可以通过该漏洞执行任意SQL语句，从而获取数据库敏感信息。  
  
CVE-2025-44074  
  
**漏洞类型：**  
SQL注入  
  
**影响：**  
获取敏感信息  
  
****  
  
**简述：**  
海洋CMS的  
admin_topic.php?actin=edit接口存在SQL注入漏洞，  
获取授权的攻击者可以通过该漏洞执行任意SQL语句，从而获取数据库敏感信息。  
  
**0x04 影响版本**  
- 海洋CMS  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.seacms.net/  
  
  
  
