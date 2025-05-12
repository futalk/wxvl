#  漏洞预警 | 锐捷EWEB任意文件读取漏洞   
浅安  浅安安全   2025-05-12 00:00  
  
**0x00 漏洞编号**  
- # 暂无  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
锐捷睿易是锐捷网络针对商业市场的子品牌。拥有易网络、交换机、路由器、无线、安全、云服务六大产品线，解决方案涵盖商贸零售、酒店、KTV、网吧、监控安防、物流仓储、制造业、中小教育、中小医疗、中小政府等商业用户。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SXnlC3jOakKJzvt6mcRvjchLw9tUm2n4iciaz6Qa5WkY2uo1D5dTUTGQ5yzAbpoQSCF1sTH3A8uAggw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
**0x03 漏洞详情**  
###   
  
**漏洞类型：**  
任意文件读取  
  
**影响：**  
泄露  
敏感信息  
  
**简述：**  
锐捷EWEB的  
/ddi/server/dhcp.php?a=csv接口存在任意文件读取漏洞，攻击者可以利用该漏洞读取设备上任意文件内容，造成敏感信息泄露。  
  
**0x04 影响版本**  
- 锐捷EWEB  
 <= 2022.07.28.01  
  
**0x05****POC状态**  
- **已公开**  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.ruijie.com.cn/  
  
  
  
