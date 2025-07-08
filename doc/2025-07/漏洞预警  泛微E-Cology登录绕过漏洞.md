#  漏洞预警 | 泛微E-Cology登录绕过漏洞  
浅安  浅安安全   2025-07-08 00:00  
  
**0x00 漏洞编号**  
- # 暂无  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
泛微协同管理应用平台（e-cology）是一套兼具企业信息门户、知识管理、数据中心、工作流管理、人力资源管理、客户与合作伙伴管理、项目管理、财务管理、资产管理功能的协同商务平台。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SUrJBibTRVnu4S9micz4sX1rDm6wgINB5BOuEZ1swwx74zHow6XXph4ShdJXYO1oVtoozO5uSvoW9dQ/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
**0x03 漏洞详情**  
###   
  
**漏洞类型：**  
登录绕过  
  
**影响：**  
获取敏感信息  
  
**简述：**  
泛微E-cology存在登录绕过漏洞，涉及接口/dwr/call/plaincall、/mobilemode/mobile/server.jsp和/weaver/ImgFileDownload/a.swf，未经身份验证的攻击者可以通过该漏洞绕过身份验证，从而远程执行任意代码控制目标服务器。  
  
**0x04 影响版本**  
- 泛微e-cology  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.weaver.com.cn/  
  
  
  
