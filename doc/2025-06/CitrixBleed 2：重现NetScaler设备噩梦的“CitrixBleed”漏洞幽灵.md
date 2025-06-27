#  CitrixBleed 2：重现NetScaler设备噩梦的“CitrixBleed”漏洞幽灵  
鹏鹏同学  黑猫安全   2025-06-27 01:26  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/8dBEfDPEceibqiacckIpMAUAcEAiax4sINRUvhibozZaibtVIydEnbDD2dicCuWvwLAqEr82aW1hIGgiamvo7wmzh4YRA/640?wx_fmt=png&from=appmsg "")  
  
思杰（Citrix）NetScaler ADC和网关设备中新发现的漏洞被命名为"CitrixBleed 2"（CVE-2025-5777，CVSS v4.0基础评分9.3），该漏洞允许未经认证的攻击者窃取会话Cookie，其危害性与2023年震惊业界的CitrixBleed漏洞（CVE-2023-4966）如出一辙。  
### 漏洞详情  
  
该漏洞属于输入验证不充分导致的内存越界读取问题，影响以下两种配置场景的NetScaler设备：  
- 作为网关使用（VPN虚拟服务器、ICA代理、CVPN、RDP代理）  
  
- 或配置为AAA虚拟服务器  
  
### 受影响版本  
- NetScaler ADC 12.1-FIPS **早于**  
 12.1-55.328-FIPS  
  
- NetScaler ADC/网关 14.1 **早于**  
 14.1-43.56  
  
- NetScaler ADC/网关 13.1 **早于**  
 13.1-58.32  
  
- NetScaler ADC 13.1-FIPS及NDcPP **早于**  
 13.1-37.235-FIPS及NDcPP  
  
### 历史重演  
  
安全研究员Kevin Beaumont指出，CVE-2025-5777与CitrixBleed漏洞（CVE-2023-4966）存在惊人相似性："还记得CitrixBleed吗？只需一个HTTP请求就能泄露内存数据、暴露会话令牌的漏洞？现在它像坎耶·韦斯特时隔两年重返推特一样卷土重来了，这次编号CVE-2025-5777。"  
### 攻击原理  
  
当NetScaler设备作为网关或AAA虚拟服务器时（常见于大型机构的远程访问架构），攻击者可利用该漏洞读取设备内存。专家强调："内存中可能包含敏感信息，攻击者能通过重放会话令牌劫持Citrix会话，甚至绕过多因素认证（MFA）——这正是当年CitrixBleed的核心威胁。"  
### 风险规模  
  
Beaumont通过Shodan扫描发现超过56,500个暴露在公网的NetScaler端点，但具体受影响数量尚不明确。  
### 同步修复的高危漏洞  
  
思杰同时修复了另一个高危漏洞CVE-2025-5349，该漏洞影响NetScaler管理接口，源于访问控制不当。攻击者需访问NSIP、集群IP或本地GSLB IP方可利用。  
### 升级建议  
  
思杰建议所有用户立即升级至已修复版本，并特别提醒：**完成升级后需执行命令终止所有活跃的ICA和PCoIP会话**  
以实现完全防护。本次漏洞致谢名单包括Positive Technologies和意大利国防部CERT，但CVE-2025-5777的具体发现者尚未披露。  
  
  
