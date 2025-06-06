#  微软公开披露关键 ChromeOS 漏洞的细节   
 网络安全应急技术国家工程中心   2022-08-25 15:43  
  
微软分享了一个关键的 ChromeOS 漏洞的技术细节，该漏洞可被利用来触发 DoS 条件或远程代码执行。被跟踪为 CVE-2022-2587(CVSS 得分为 9.8)的关键 ChromeOS 漏洞的详细信息。该漏洞是 OS Audio Server 中的一个越界写入问题，可被利用来触发 DoS 条件，或者在特定情况下实现远程代码执行。  
  
“微软在 ChromeOS 组件中发现了一个内存损坏漏洞，该漏洞可以远程触发，允许攻击者执行拒绝服务(DoS)，或者在极端情况下执行远程代码执行(RCE)。” 阅读微软发布的公告。  
  
作为 Chromium 错误跟踪系统的一部分，微软于2022年4月向谷歌报告了该问题。  
   
  
谷歌在 6 月份解决了该漏洞，攻击者可以使用与歌曲相关的格式错误的元数据触发该漏洞。  
  
Microsoft 在服务器中发现了一个未检查用户提供的“身份”参数的函数，导致基于堆的缓冲区溢出。  
  
OS 音频服务器包含一种从表示歌曲标题的元数据中提取“身份”的方法。攻击者可以通过在播放新歌曲时从浏览器或蓝牙修改音频元数据来触发该漏洞。  
   
  
“我们发现该漏洞可以通过操纵音频元数据远程触发。攻击者可能会诱使用户满足这些条件，例如通过简单地在浏览器或配对的蓝牙设备中播放一首新歌，或利用中间对手 (AiTM) 功能远程利用该漏洞。” 继续咨询。“基于堆的缓冲区溢出的影响范围从简单的 DoS 到成熟的 RCE。虽然可以通过媒体元数据操作来分配和释放块，但在这种情况下执行精确的 堆清理 并非易事，攻击者需要将漏洞与其他漏洞链接起来才能成功执行任意代码。”  
  
   
  
  
原文来源  
：E安全  
  
“投稿联系方式：孙中豪 010-82992251   sunzhonghao@cert.org.cn”  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/GoUrACT176njVOPvfib4X3jQ6GIHLtX8SSDvbpmcpr4uu3X7ELG7PDjdaLVeq4Er02ZoicTPvxrC6KCVH3bssUVw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1 "")  
