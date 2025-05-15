#  漏洞预警 Salia PLCC check_req.php 命令执行漏洞   
by 融云安全-cas  融云攻防实验室   2025-05-15 03:10  
  
**0x01 阅读须知**  
  
**融云安全的技术文章仅供参考，此文所提供的信息只为网络安全人员对自己所负责的网站、服务器等（包括但不限于）进行检测或维护参考，未经授权请勿利用文章中的技术资料对任何计算机系统进行入侵操作。利用此文所提供的信息而造成的直接或间接后果和损失，均由使用者本人负责。本文所提供的工具仅用于学习，禁止用于其他！！**  
  
**0x02 漏洞描述（由于博主被经常被警告，厂商名不敢放出）**  
  
Salia PLCC 提供的产品包括通信模块、智能电表、数据采集设备等，旨在帮助用户实现电力系统的远  
程监控、数据传输和智能化管理。他们的解决方案可以提高能源利用效率，改善电力系统的稳定性和可靠性。  
Salia PLCC check_req.php 存在命令执行漏洞。![](https://mmbiz.qpic.cn/mmbiz_png/GWXBjgPE49zzM1iamxqPDHAlYPyABvoAkrdLguLcnR7m7ATS6icSFR3K9AItKJMsdXlQluSC9nMibNc8Tm5rId5Fg/640?wx_fmt=png&from=appmsg "")  
  
  
**0x03 漏洞复现**  
  
**f****o****fa:body="Salia PLCC"**  
  
1.执行poc 进行命令执行，访问得到结果  
  
**2.代码审计过程和nuclei脚本发布在知识星球（AI时代，不会审计比原来更难拿到高薪。这位是七八年的审计经验大哥的代码审计星球。加入星球一起学习，全年POC 0 day、代码审计技巧服务）**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/GWXBjgPE49zzM1iamxqPDHAlYPyABvoAkb9lGiaNdpNE2hu0peMUdjsJ6jBotQCXt47IzX40W8DgerjAXXlEvCaA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/GWXBjgPE49zzM1iamxqPDHAlYPyABvoAk5kibnHFpbiaYdbLicTIYqFaeKvsQ22icz8zm5nuD2ttbknmzRRDw4P6SYA/640?wx_fmt=png&from=appmsg "")  
  
**高质量漏洞利用研究，代码审计圈子等。**  
  
**【圈子权益】**  
  
**1，一年至少200+漏洞Poc及对应漏洞批量利用工具**  
  
**2，各种漏洞利用工具及后续更新，渗透工具、文档资源分享**  
  
**星球目前在推广期价格为79元，优惠30元，后面恢复原价。**  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/GWXBjgPE49xKicPTzjrTnfF8oYyhDcc1ITEfg8wqjLIicS2XeTVb9q8lpGbicVh9vT9V7lDUU5lgF3Kv62ciabVmQg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
****  
****  
  
**0x05 公司简介**  
  
江西渝融云安全科技有限公司，2017年发展至今，已成为了一家集云安全、物联网安全、数据安全、等保建设、风险评估、信息技术应用创新及网络安全人才培训为一体的本地化高科技公司，是江西省信息安全产业链企业和江西省政府部门重点行业网络安全事件应急响应队伍成员。  
  
   公司现已获得信息安全集成三级、信息系统安全运维三级、风险评估三级等多项资质认证，拥有软件著作权十八项；荣获2020年全国工控安全深度行安全攻防对抗赛三等奖；庆祝建党100周年活动信息安全应急保障优秀案例等荣誉......  
****  
‌  
  
**编制：sm**  
  
**审核：fjh**  
  
**审核：Dog**  
  
****  
**1个1朵********5毛钱**  
  
**天天搬砖的小M**  
  
**能不能吃顿好的**  
  
**就看你们的啦**  
  
****  
  
