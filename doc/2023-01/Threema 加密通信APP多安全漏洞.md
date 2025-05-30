#  Threema 加密通信APP多安全漏洞   
 关键基础设施安全应急响应中心   2023-01-20 15:49  
  
研究人员在端到端加密通信APP Threema发现多个安全漏洞。  
  
Threema是一款瑞士的加密通信APP，有超过100万用户和7000本地部署用户。其中，Threema重要用户包括瑞士政府和军方，以及德国总理。Threema广告宣称是其他即使通信应用的安全可替代产品。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/wpkib3J60o2icMmUcTibQRsk321YMsTrQNSO9xQicSaepdFKmQjcwd2Tl4m4DPGIM33jYQicRz4RZCXOm26QTicHFjjQ/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
**Threema攻击**  
  
瑞士苏黎世联邦理工学院 (ETH Zurich) 研究人员共涉及了7种针对Threema协议的安全攻击，可打破Threema APP的通信隐私，包括窃取私有密钥、删除消息、破解认证、欺骗服务器等。  
  
· 临时密钥入侵假冒（Ephemeral key compromise impersonation）：攻击者窃取临时密钥伪装成客户端。Threema貌似重用了临时密钥，即临时密钥不是一次性的。  
  
· Vouch box 伪造：攻击者可诱使用户发送有效的vouch box，然后用它来假冒客户端。  
  
· 消息重排和删除：恶意服务器可转发来自用户的消息，攻击者可以对消息重新排序，此外，服务器还可以将消息删除。  
  
· 重放和反射攻击：Threema安卓版本的消息nonce数据库是不可迁移的，因此可以实现消息重放和反射攻击。  
  
· Kompromat攻击：恶意服务器可以诱使客户端在初始化注册协议阶段与服务器会话时和在端到端协议中与其他用户会话时使用的密钥。  
  
· 通过Threema ID export克隆：攻击者可在其设备上克隆其他用户的账户，但要求用户设备是未锁定的。  
  
· 压缩侧信道：研究人员在Threema加密方法中发现一个安全漏洞，允许攻击者通过控制用户名和安卓设备上的多个备份来提取用户的私钥。但攻击过程需要几个小时。  
  
**Threema回应**  
  
ETH Zurich研究人员于2022年10月3日将研究成果报告给Threema。11月29日，Threema发布了新的更加安全的协议——Ibex，来解决潜在的安全问题。但是Threema称ETH Zurich研究的Threema协议其已不再使用，并称其研究发现并不会对其产品带来任何影响。比如，通过Threema ID export实现克隆攻击已在2021年修复。Vouch box伪造攻击依赖社会工程攻击，需要目标用户的参与。其他攻击还需要对设备的物理访问，且要求Threema设备未锁定。  
  
研究论文参见：https://breakingthe3ma.app/files/Threema-PST22.pdf  
  
PoC和其他技术细节参见：https://breakingthe3ma.app/  
  
**参考及来源：**  
  
https://www.bleepingcomputer.com/news/security/threema-claims-encryption-flaws-never-had-a-real-world-impact/  
  
  
  
原文来源：嘶吼专业版  
  
“投稿联系方式：010-82992251   sunzhonghao@cert.org.cn”  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/iaz5iaQYxGogucKMiatGyfBHlfj74r3CyPxEBrV0oOOuHICibgHwtoIGayOIcmJCIsAn02z2yibtfQylib07asMqYAEw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
