#  Struts2全版本漏洞检测工具更新！V19.68  
abc123info  夜组安全   2025-07-10 00:02  
  
免责声明  
  
由于传播、利用本公众号夜组安全所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号夜组安全及作者不为此承担任何责任，一旦造成后果请自行承担！如有侵权烦请告知，我们会立即删除并致歉。谢谢！  
**所有工具安全性自测！！！VX：**  
**baobeiaini_ya**  
  
朋友们现在只对常读和星标的公众号才展示大图推送，建议大家把  
**夜组安全**  
“**设为星标**  
”，  
否则可能就看不到了啦！  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2WrOMH4AFgkSfEFMOvvFuVKmDYdQjwJ9ekMm4jiasmWhBicHJngFY1USGOZfd3Xg4k3iamUOT5DcodvA/640?wx_fmt=png&from=appmsg "")  
  
## 工具介绍  
  
Struts2全版本漏洞检测工具 by:ABC_123  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2V4qLgKdCT8wyMrZtPqLexST99MHIou0HxtN8WMNtGb73Xz8rXtWtJaJJvb1Gia4sMSxibEmDkvjnIA/640?wx_fmt=png&from=appmsg "")  
  
1、点击“检测漏洞”，会自动检测该URL是否存在S2-001、S2-005、S2-009、S2-013、S2-016、S2-019、S2-020/021、S2-032、S2-037、DevMode、S2-045/046、S2-052、S2-048、S2-053、S2-057、S2-061、S2相关log4j2十余种漏洞。  
  
2、“批量验证”，（为防止批量geshell，此功能已经删除，并不再开发）。  
  
3、S2-020、S2-021仅提供漏洞扫描功能，因漏洞利用exp很大几率造成网站访问异常，本程序暂不提供。  
  
4、对于需要登录的页面，请勾选“设置全局Cookie值”，并填好相应的Cookie，程序每次发包都会带上Cookie。  
  
5、作者对不同的struts2漏洞测试语句做了大量修改，执行命令、上传功能已经能通用。  
  
6、支持GET、POST、UPLOAD三种请求方法，您可以自由选择。（UPLOAD为Multi-Part方式提交）  
  
7、部分漏洞测试支持UTF-8、GB2312、GBK编码转换。  
  
8、每次操作都启用一个线程，防止界面卡死。  
## 更新  
  
2025.07.05 新增检测Struts2框架下Log4j2漏洞的新语句。  
  
2025.07.04 新增下载文件到指定目录功能，应对上传webshell过程无法绕过WAF的情况。  
  
2025.07.04 新增延时方法判断Struts2漏洞，解决Struts2框架无回显情况下的漏洞检测。  
  
2025.07.02 新增S2-018漏洞检测方法。  
  
2025.07.02 新增S2-017漏洞新的检测语句，进一步提升漏洞检测的准确度。  
  
2025.07.02 新增S2-019漏洞新的检测语句，进一步提升漏洞检测的准确度。  
  
## 工具获取  
  
  
  
点击关注下方名片  
进入公众号  
  
回复关键字【  
250710  
】获取  
下载链接  
  
  
## 往期精彩  
  
  
往期推荐  
  
[X-SAST 专业多语言代码安全审计工具套件](http://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247494700&idx=1&sn=aec4f555c2c98fedfd9ab4bab2996520&chksm=c36ba8d4f41c21c210127adf651bcaffc719cf01d35d586073fb48fe77e04dcfdabc53e884e7&scene=21#wechat_redirect)  
  
  
[Fiora：漏洞PoC框架Nuclei的图形版。快捷搜索PoC、一键运行Nuclei。可作为独立程序运行也可burp插件使用。](http://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247494699&idx=1&sn=69f6bcf949f34426d14e3cb19a6128ed&chksm=c36ba8d3f41c21c51e548b1af79178e2116e52a96cf84feea7627cdf67668ce6fccdeca2686c&scene=21#wechat_redirect)  
  
  
[一个用于 Burp Suite 的插件，专为检测和分析 SQL 注入漏洞而设计。](http://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247494698&idx=1&sn=47fe75c62630836696f6a32f42bef890&chksm=c36ba8d2f41c21c4fb95dbebe76e1974b261c8850be5da2969c90a9c0a2d6079c7072fedfe6b&scene=21#wechat_redirect)  
  
  
[十款免费的恶意样本分析工具](http://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247494690&idx=1&sn=32c119203374701411a8d9572174b5dc&chksm=c36ba8daf41c21cc7ec7d636f92bfbaaab079f8a55292cb65b31395bfbe9b79d2ae45613ec0d&scene=21#wechat_redirect)  
  
  
[一个专为数字钱包助记词本地安全备份设计的加密工具。助记词数据加密 & 解密](http://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247494661&idx=1&sn=347e22606381188baf5cf640a969e519&chksm=c36ba8fdf41c21eb792672415d86141a93ad6e8a9abbd9b0b852a7cddd966b2cf40db99d8e4d&scene=21#wechat_redirect)  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/OAmMqjhMehrtxRQaYnbrvafmXHe0AwWLr2mdZxcg9wia7gVTfBbpfT6kR2xkjzsZ6bTTu5YCbytuoshPcddfsNg/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&random=0.8399406679299557&tp=webp "")  
  
