#  Zyxel（合勤科技）路由器中的严重漏洞   
网络研究观  网络研究观   2024-09-07 22:00  
  
![](https://mmbiz.qpic.cn/mmbiz_png/yvLFKBRPQxPLfG7zzrdjcyqibbpCTEdibXxvySxAD76ABLq9O5KpXV78vXL8icIaIM2c9R7v69fetmtmoFnpjVwyQ/640?wx_fmt=png&from=appmsg "")  
  
  
Zyxel （  
合勤科技）已发布补丁来解决影响多个企业路由器型号的严重  
漏洞  
，并可能允许未经身份验证的攻击者执行命令注入。  
  
合勤科技还修复了其他产品中的近十几个  
漏洞。  
  
所有已修复的漏洞中最危险的漏洞的标识符为   
CVE-2024-7261  
  ，其CVSS 评分为 9.8，被认为是严重漏洞。  
  
该问题是由于对用户提供的数据处理不当造成的，导致远程攻击者可以在主机操作系统上执行任意命令。  
  
错误地中和某些接入点和路由器型号上的 CGI 程序中的主机参数中的特殊元素可能会导致未经授权的攻击者通过向易受攻击的设备发送特制的 cookie 来执行命令。  
  
CVE-2024-7261 影响以下设备。  
- NWA 系列 ：NWA50AX、NWA50AX PRO、NWA55AXE、NWA90AX、NWA90AX PRO、NWA110AX、NWA130BE、NWA210AX、NWA220AX-6E。7.00 以下的所有固件版本都容易受到攻击；我们建议更新到 7.00 (ABYW.2) 及更高版本。  
  
  
- NWA1123-AC PRO：  6.28 以下的所有固件版本都存在漏洞，建议更新到 6.28 (ABHD.3) 及更高版本。  
  
  
- NWA1123ACv3、WAC500、WAC500H：  6.70 以下的所有固件版本都存在漏洞，建议更新到 6.70 (ABVT.5) 及更高版本。  
  
  
- WAC系列 ：WAC6103D-I、WAC6502D-S、WAC6503D-S、WAC6552D-S、WAC6553D-E，所有固件版本至6.28均存在漏洞，建议更新至6.28（AAXH.3）及以上版本。  
  
  
- WAX系列 ：WAX300H、WAX510D、WAX610D、WAX620D-6E、WAX630S、WAX640S-6E、WAX650S、WAX655E，所有固件版本7.00均存在漏洞，建议升级到7.00（ACHF.2）及以上版本。  
  
  
- WBE 系列 ：WBE530、WBE660S，7.00 以下的所有固件版本均存在漏洞，建议更新至 7.00 (ACLE.2) 及更高版本。  
  
  
Zyxel 还报告称，运行 V2.00 (ACIP.2) 的 USG LITE 60AX 路由器也受到此问题的影响，但该型号会通过云端自动更新到版本 V2.00 (ACIP.3)，该版本已包含针对CVE-2024-7261。  
  
然而，关键错误并不是合勤科技本周修复的唯一问题。  
  
因此，  
制造商  
警告了  
影响某些防火墙系列的另外七个漏洞，包括 ATP、USG-FLEX 和 USG FLEX 50(W)/USG20(W)-VPN：  
- CVE-2024-6343  （CVSS 评分 4.9）：CGI 中的缓冲区溢出允许具有管理权限的经过身份验证的攻击者通过发送特制的 HTTP 请求来导致拒绝服务；  
  
  
- CVE-2024-7203  （CVSS 分数 7.2）：身份验证后命令注入，允许具有管理权限的经过身份验证的攻击者通过执行特制的 CLI 命令来执行命令。  
该漏洞是由 HackerHood 小组的 Red Hot Cyber 研究人员发现的。  
  
  
- CVE-2024-42057  （CVSS 评分 8.1）：IPSec VPN 中的命令注入，允许未经身份验证的攻击者通过发送欺骗性用户名来执行命令（仅当设备配置为基于 PSK 用户的身份验证并且具有名称长度超过 28 个字符的用户）。  
  
  
- CVE-2024-42058（  CVSS 分数 7.5）：某些防火墙版本中的 NULL 指针取消引用，允许未经身份验证的攻击者通过发送修改后的数据包来进行拒绝服务攻击。  
  
  
- CVE-2024-42059  （CVSS 评分 7.2）：身份验证后命令注入，允许具有管理权限的经过身份验证的攻击者通过 FTP 上传压缩语言文件在受影响的设备上执行命令。  
  
  
- CVE-2024-42060  （CVSS 评分 7.2）：身份验证后命令注入，允许具有管理权限的经过身份验证的攻击者通过将修改后的内部用户合同文件下载到受影响的设备来执行命令。  
  
  
- CVE-2024-42061  （CVSS 得分 6.1）：反射 CGI XSS Dynamic_script.cgi 允许攻击者诱骗用户访问带有 XSS 负载的精心设计的 URL。如果在受害者的浏览器中执行恶意脚本，攻击者将能够获取浏览器信息。  
  
  
50 个合勤科技产品中发现了另一个漏洞（    
CVE-2024-5412 ，CVSS 评分 7.5）   
，包括一些客户端设备、光纤终端和路由器。  
  
该问题与 libclinkc 库中的缓冲区溢出有关，允许未经身份验证的攻击者通过发送修改后的 HTTP 请求来进行 DoS 攻击。  
  
