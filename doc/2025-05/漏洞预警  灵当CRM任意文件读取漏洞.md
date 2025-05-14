#  漏洞预警 | 灵当CRM任意文件读取漏洞   
浅安  浅安安全   2025-05-14 00:00  
  
**0x00 漏洞编号**  
- # 暂无  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
灵当CRM是一款专为中小企业打造的智能客户关系管理工具。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SVt7YFyTQQEfhb7U1ia1Eib9LbzN6IooEiaAu8gmJRoBbalGtmLHyIvibmIAvPanxUHCsPxmIasVyUNAA/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
**0x03 漏洞详情**  
###   
  
**漏洞类型：**  
任意文件读取  
  
**影响：**  
获取  
敏感信息  
  
**简述：**  
灵当CRM的  
/crm/modules/Accounts/Playforrecord.php接口存在任意文件读取漏洞，未经身份验证的攻击者可以通过该漏洞读取服务器敏感信息。  
  
**0x04 影响版本**  
- 灵当CRM  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.51mis.com/  
  
  
  
