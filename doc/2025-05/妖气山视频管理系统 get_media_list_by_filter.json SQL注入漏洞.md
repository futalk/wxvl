#  妖气山视频管理系统 get_media_list_by_filter.json SQL注入漏洞   
Superhero  Nday Poc   2025-05-18 02:08  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOJECe5vg2C5YWgpyo1D5bCkYN4sZibCVo6EFo0N9b7Kib4I4N6j6Y10tynLOdgov9ibUmaNwW5yeoCbQ/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOJECe5vg2C5YWgpyo1D5bCkhic5lbbPcpxTLtLccZ04WhwDotW7g2b3zBgZeS5uvFH4dxf0tj0Rutw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOJECe5vg2C5YWgpyo1D5bCk524CiapZejYicic1Hf8LPt8qR893A3IP38J3NMmskDZjyqNkShewpibEfA/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
内容仅用于学习交流自查使用，由于传播、利用本公众号所提供的  
POC  
信息及  
POC对应脚本  
而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号Nday Poc及作者不为此承担任何责任，一旦造成后果请自行承担！  
  
  
**01**  
  
**漏洞概述**  
  
  
妖气山视频管理系统  get_media_list_by_filter.json存在SQL注入漏洞，攻击者除了可以利用 SQL 注入漏洞获取数据库中的信息（例如，管理员后台密码,站点的用户个人信息）之外，甚至在高权限的情况可向服务器中写入木马，进一步获取服务器系统权限。  
**02******  
  
**搜索引擎**  
  
  
FOFA:  
```
body="yaoqishan-header"
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwLRQbnKKnVsT5oB90fgWfgKB4LPpwDC7e5SibEiczUXVoROLrLYF9dAcghAVMlm6HNUIIVjpOopXhcg/640?wx_fmt=png&from=appmsg "")  
  
  
**03******  
  
**漏洞复现**  
  
延时4秒，执行2次  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwLRQbnKKnVsT5oB90fgWfgKn5h5yfyawlj0GTSFdeicpyAq6fMj9BZkul8b4zO0n72k29PJvsbYibfQ/640?wx_fmt=png&from=appmsg "")  
  
sqlmap验证  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwLRQbnKKnVsT5oB90fgWfgKBibuh9aquF6p6hQfthEW49xqpbHK3ljM53aL5CJ80rag0icIu8sanqtg/640?wx_fmt=png&from=appmsg "")  
  
  
**04**  
  
**自查工具**  
  
  
nuclei  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwLRQbnKKnVsT5oB90fgWfgKrEQpLiaQ0mviaP5vMJjs92PiaJ0P6RqBsVicOYWWAA0pvYyl1ySSyqBLBQ/640?wx_fmt=png&from=appmsg "")  
  
afrog  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwLRQbnKKnVsT5oB90fgWfgKSg4BofDGzfsXG0wyBPY1j7Fkicrib7r56MwLNPpKvGLPejhPQKBuicyWQ/640?wx_fmt=png&from=appmsg "")  
  
xray  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwLRQbnKKnVsT5oB90fgWfgKIibnSHcZvchaenNbX2ib1nyB1ZXIArDpyumdh57KN0sf73A0upVbF08A/640?wx_fmt=png&from=appmsg "")  
  
  
**05******  
  
**修复建议**  
  
  
1、关闭互联网暴露面或接口设置访问权限  
  
2、  
升级至安全版本  
  
  
**06******  
  
**内部圈子介绍**  
  
  
【Nday漏洞实战圈】🛠️   
  
专注公开1day/Nday漏洞复现  
 · 工具链适配支持  
  
 ✧━━━━━━━━━━━━━━━━✧   
  
🔍 资源内容  
  
 ▫️ 整合全网公开  
1day/Nday  
漏洞POC详情  
  
 ▫️ 适配Xray/Afrog/Nuclei检测脚本  
  
 ▫️ 支持内置与自定义POC目录混合扫描   
  
🔄 更新计划   
  
▫️ 每周新增7-10个实用POC（来源公开平台）   
  
▫️ 所有脚本经过基础测试，降低调试成本   
  
🎯 适用场景   
  
▫️ 企业漏洞自查 ▫️ 渗透测试 ▫️ 红蓝对抗   
▫️ 安全运维  
  
✧━━━━━━━━━━━━━━━━✧   
  
⚠️ 声明：仅限合法授权测试，严禁违规使用！  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwI0beBCCyKGykkAazuPyvibgC0ooBGy9elQQ72f1WIB73UDYuPhx8cnCobvnOBdTcxmdwBbt2eAYIQ/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
### 🎁 周年庆限时特惠 | 历史最低价 | 限量优惠券发放  
  
**活动时间：2025年5月16日 00:00 - 2025年5月18日 23:59**  
  
（仅限3天，永不返场）  
<table><thead><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><th style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 10px 10px 10px 0px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border-top: none;border-right: 1px solid rgb(221, 221, 221);border-bottom: 1px solid rgb(221, 221, 221);border-left: 1px solid rgb(221, 221, 221);border-image: initial;background: rgb(247, 247, 247);max-width: 100%;box-sizing: border-box !important;color: rgb(var(--ds-rgb-label-1));font-weight: 600;font-size: 15px;line-height: 1.72;text-align: left;visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">用户类型</span></strong></th><th style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border-top: none;border-right: 1px solid rgb(221, 221, 221);border-bottom: 1px solid rgb(221, 221, 221);border-left: 1px solid rgb(221, 221, 221);border-image: initial;background: rgb(247, 247, 247);max-width: 100%;box-sizing: border-box !important;color: rgb(var(--ds-rgb-label-1));font-weight: 600;font-size: 15px;line-height: 1.72;text-align: left;visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">原价</span></strong></th><th style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border-top: none;border-right: 1px solid rgb(221, 221, 221);border-bottom: 1px solid rgb(221, 221, 221);border-left: 1px solid rgb(221, 221, 221);border-image: initial;background: rgb(247, 247, 247);max-width: 100%;box-sizing: border-box !important;color: rgb(var(--ds-rgb-label-1));font-weight: 600;font-size: 15px;line-height: 1.72;text-align: left;visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">周年价</span></strong></th><th style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border-top: none;border-right: 1px solid rgb(221, 221, 221);border-bottom: 1px solid rgb(221, 221, 221);border-left: 1px solid rgb(221, 221, 221);border-image: initial;background: rgb(247, 247, 247);max-width: 100%;box-sizing: border-box !important;color: rgb(var(--ds-rgb-label-1));font-weight: 600;font-size: 15px;line-height: 1.72;text-align: left;visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">直降幅度</span></strong></th><th style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border-top: none;border-right: 1px solid rgb(221, 221, 221);border-bottom: 1px solid rgb(221, 221, 221);border-left: 1px solid rgb(221, 221, 221);border-image: initial;background: rgb(247, 247, 247);max-width: 100%;box-sizing: border-box !important;color: rgb(var(--ds-rgb-label-1));font-weight: 600;font-size: 15px;line-height: 1.72;text-align: left;visibility: visible;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">限量优惠券</span></section></th></tr></thead><tbody><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><td style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 10px 10px 10px 0px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border-width: 1px;border-style: solid;border-color: var(--dsr-border-2);border-image: initial;max-width: max(30vw, 320px);box-sizing: border-box !important;font-size: 15px;line-height: 1.72;min-width: 100px;visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">新用户首年入圈</span></strong></td><td style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border-width: 1px;border-style: solid;border-color: var(--dsr-border-2);border-image: initial;max-width: max(30vw, 320px);box-sizing: border-box !important;font-size: 15px;line-height: 1.72;min-width: 100px;visibility: visible;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">59.9元</span></section></td><td style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border-width: 1px;border-style: solid;border-color: var(--dsr-border-2);border-image: initial;max-width: max(30vw, 320px);box-sizing: border-box !important;font-size: 15px;line-height: 1.72;min-width: 100px;visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">35元</span></strong></td><td style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border-width: 1px;border-style: solid;border-color: var(--dsr-border-2);border-image: initial;max-width: max(30vw, 320px);box-sizing: border-box !important;font-size: 15px;line-height: 1.72;min-width: 100px;visibility: visible;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">↘24.9元</span></section></td><td style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border-width: 1px;border-style: solid;border-color: var(--dsr-border-2);border-image: initial;max-width: max(30vw, 320px);box-sizing: border-box !important;font-size: 15px;line-height: 1.72;min-width: 100px;visibility: visible;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">30张</span></section></td></tr></tbody></table>  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/wnJTy44dqwJTjuqzDicLFwqeBRkTxMCkr6nCxpL73dNC89zOA5eiaglSCZiaduXv2MRDLIQbJ1KA8EDnXxNIgB6WQ/640?wx_fmt=jpeg&from=appmsg&watermark=1&wxfrom=5&wx_lazy=1&tp=webp "")  
  
  
