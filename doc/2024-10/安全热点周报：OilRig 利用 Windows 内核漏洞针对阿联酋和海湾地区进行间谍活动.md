#  安全热点周报：OilRig 利用 Windows 内核漏洞针对阿联酋和海湾地区进行间谍活动   
 奇安信 CERT   2024-10-21 17:05  
  
<table><tbody style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><tr bgless="lighten" bglessp="20%" data-bglessp="40%" data-bgless="lighten" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 4px solid rgb(68, 117, 241);visibility: visible;"><th align="center" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;background-color: rgb(254, 254, 254);font-size: 20px;line-height: 1.2;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(68, 117, 241);visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;font-size: 17px;visibility: visible;">安全资讯导视 </span></strong></span></th></tr><tr data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 国家数据局《可信数据空间发展行动计划（2024—2028年）》公开征求意见</p></td></tr><tr data-bglessp="40%" data-bgless="lighten" data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 大量个人信息数据遭境外访问窃取，上海某医疗科技企业被行政处罚</p></td></tr><tr data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 上市公司科沃斯旗下扫地机被黑并发出骚扰声：用户受惊 官方回应</p></td></tr></tbody></table>  
  
**PART****0****1**  
  
  
**漏洞情报**  
  
  
**1.Spring Framework路径遍历漏洞安全风险通告**  
  
  
10月18日，奇安信CERT监测到官方修复Spring Framework路径遍历漏洞(CVE-2024-38819)，在Spring Framework受影响版本中，当应用程序使用WebMvc.fn或WebFlux.fn提供静态资源时容易受到路径遍历攻击。攻击者可以编写恶意HTTP请求并获取目标系统上任何由Spring应用程序正在运行的进程访问的文件，从而导致信息泄露。鉴于此漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**2.Apache Solr身份认证绕过漏洞安全风险通告**  
  
  
10月16日，奇安信CERT监测到官方修复Apache Solr身份认证绕过漏洞(CVE-2024-45216)，该漏洞存在于Apache Solr的PKIAuthenticationPlugin中，该插件在启用Solr身份验证时默认启用。攻击者可以利用在任何Solr API URL路径末尾添加假结尾的方式，绕过身份验证访问任意路由，从而获取敏感数据或进行其他恶意操作。鉴于此漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**PART****0****2**  
  
  
**新增在野利用**  
  
  
**1.****Veeam Backup & Replication 反序列化漏洞(CVE-2024-40711)**  
  
  
10月17日，勒索软件团伙现在利用一个严重的安全漏洞，让攻击者在易受攻击的 Veeam Backup & Replication (VBR) 服务器上获得远程代码执行 (RCE)。  
  
Code White 安全研究员 Florian Hauser 发现，该安全漏洞（目前编号为 CVE-2024-40711）是由不受信任数据反序列化的弱点引起的，未经身份验证的黑客可以利用该漏洞发动低复杂度攻击。  
  
Veeam 于 9 月 4 日披露了该漏洞并发布了安全更新，而 watchTowr Labs 于 9 月 9 日发布了技术分析。然而，watchTowr Labs 推迟到 9 月 15 日才发布概念验证漏洞代码，以便管理员有足够的时间来保护他们的服务器。  
  
此次延迟的原因是企业使用 Veeam 的 VBR 软件作为数据保护和灾难恢复解决方案，用于备份、恢复和复制虚拟、物理和云机器。这使得它成为寻求快速访问公司备份数据的黑客的热门目标。  
  
正如 Sophos X-Ops 事件响应人员在上个月发现的那样，CVE-2024-40711 RCE 漏洞很快被发现并在 Akira 和 Fog 勒索软件攻击中被利用，该漏洞与之前泄露的凭据一起将“点”本地帐户添加到本地管理员和远程桌面用户组。  
  
Sophos X-Ops 表示：“在一个案例中，攻击者投放了 Fog 勒索软件。同一时间段内的另一次攻击试图部署 Akira 勒索软件。所有 4 起案件的迹象都与之前的 Akira 和 Fog 勒索软件攻击重叠。”在每起案件中，攻击者最初都是使用未启用多因素身份验证的受感染 VPN 网关访问目标。其中一些 VPN 运行的是不受支持的软件版本。在 Fog 勒索软件事件中，攻击者将其部署到不受保护的 Hyper-V 服务器，然后使用实用程序 rclone 窃取数据。  
  
由于 Veeam 软件存在缺陷，用户成为黑客提供勒索软件的有利可图的目标，建议用户尽快更新到最新版本以减轻潜在威胁。  
  
  
参考链接：  
  
https://www.bleepingcomputer.com/news/security/akira-and-fog-ransomware-now-exploiting-critical-veeam-rce-flaw/  
  
  
**2.****Windows 内核权限提升漏洞(CVE-2024-30088)**  
  
  
10月15日，名为OilRig 的伊朗黑客利用现已修复的权限提升漏洞影响 Windows 内核，这是针对阿联酋及更广泛的海湾地区的网络间谍活动的一部分。  
  
趋势科技研究人员 Mohamed Fahmy、Bahaa Yamany、Ahmed Kamal 和 Nick Dai在周五发表的分析报告中表示：“该组织采用了复杂的策略，包括部署利用 Microsoft Exchange 服务器窃取凭证的后门，以及利用 CVE-2024-30088 等漏洞进行权限提升。”  
  
该网络安全公司正在追踪名为 Earth Simnavaz 的黑客，该黑客也被称为 APT34、Crambus、Cobalt Gypsy、GreenBug、Hazel Sandstorm（以前称为 EUROPIUM）和 Helix Kitten。  
  
攻击链需要部署一个以前未记录的植入物，该植入物能够通过内部 Microsoft Exchange 服务器窃取凭据，这是攻击者过去采用的久经考验的策略，同时还将最近披露的漏洞纳入其漏洞利用库。  
  
CVE-2024-30088 由微软于 2024 年 6 月修补，涉及 Windows 内核中的权限提升情况，可被利用来获取 SYSTEM 权限，假设攻击者能够赢得竞争条件。通过渗透到易受攻击的 Web 服务器以放置 Web Shell，然后放置 ngrok 远程管理工具以保持持久性并移动到网络中的其他端点，可以实现对目标网络的初始访问。权限提升漏洞随后成为传递代号为 STEALHOOK 的后门的渠道，负责通过 Exchange 服务器将收集的数据以附件的形式传输到攻击者控制的电子邮件地址。  
  
OilRig 在最新一组攻击中采用的一个值得注意的技术是滥用提升的权限来删除密码过滤策略 DLL（psgfilter.dll），以便通过域控制器或本地计算机上的本地帐户从域用户那里提取敏感凭据。  
  
研究人员表示：“黑客在实施密码过滤器导出功能时非常小心地处理明文密码。黑客还利用明文密码获取访问权限并远程部署工具。明文密码首先被加密，然后在通过网络发送时被泄露。”值得注意的是，早在 2022 年 12 月，人们就观察到psgfilter.dll 的使用，该活动与针对中东组织的 OilRig 活动有关，该活动使用了另一个名为 MrPerfectionManager 的后门。  
  
研究人员指出：“他们最近的活动表明，Earth Simnavaz 专注于利用地缘政治敏感地区关键基础设施的漏洞。他们还试图在受感染实体中建立持久立足点，以便将其用作武器，对其他目标发动攻击。”  
  
  
参考链接：  
  
https://thehackernews.com/2024/10/oilrig-exploits-windows-kernel-flaw-in.html  
  
  
**3.****Mozilla Firefox Animation timelines 释放后重用漏洞(CVE-2024-9680)**  
  
  
10月15日，Mozilla 透露，影响 Firefox 和 Firefox 扩展支持版本 (ESR) 的一个严重安全漏洞已被广泛利用。  
  
该漏洞的编号为CVE-2024-9680（CVSS 评分：9.8），被描述为动画时间线组件中的释放后重用漏洞。  
  
Mozilla 在公告中表示：攻击者可以利用动画时间线中的释放后使用漏洞，在内容进程中实现代码执行。他们收到了有关此漏洞正在被利用的报告。  
  
斯洛伐克公司 ESET 的安全研究员 Damien Schaeffer 因发现并报告该漏洞而受到赞誉。  
  
目前尚无关于该漏洞在现实的攻击中如何被利用以及其背后黑客身份的详细信息。也就是说，这种远程代码执行漏洞可以通过多种方式武器化，要么作为针对特定网站的水坑攻击的一部分，要么通过诱骗用户访问虚假网站的驱动下载活动。  
  
建议用户更新到最新版本以防范主动威胁。  
  
  
参考链接：  
  
https://thehackernews.com/2024/10/mozilla-warns-of-active-exploitation-in.html  
  
  
**4.****SolarWinds Web Help Desk 硬编码凭证绕过漏洞(CVE-2024-28987)**  
  
  
10月15日，CISA 将影响 SolarWinds Web Help Desk (WHD) 软件的严重安全漏洞添加到其已知被利用漏洞 ( KEV ) 目录中，并指出有证据显示该漏洞存在主动利用。  
  
SolarWinds 发布了针对严重 Web Help Desk 漏洞的修补程序，该漏洞允许攻击者使用硬编码凭据登录未修复的系统。Web Help Desk (WHD) 是一款 IT 帮助台软件，被政府机构、大型企业以及医疗保健和教育机构广泛使用，用于自动化和简化帮助台管理任务。SolarWinds 的 IT 管理产品被全球超过 300,000 名客户使用。  
  
CVE-2024-28987 允许未经身份验证的攻击者在成功利用后访问内部功能并修改目标设备上的数据。此漏洞由 Horizon3.ai 的漏洞研究员 Zach Hanley 发现并报告。  
  
安全研究员扎克·汉利 (Zach Hanley) 表示，该漏洞“允许未经身份验证的攻击者远程读取和修改所有帮助台票证详细信息 - 通常包含重置请求的密码和共享服务帐户凭据等敏感信息”。  
  
目前尚不清楚该漏洞在实际攻击中是如何被利用的，以及被谁利用。尽管如此，这一进展是在 CISA 将同一软件中的另一个漏洞（CVE-2024-28986，CVSS 评分：9.8）添加到 KEV 目录两个月后取得的。  
  
鉴于正在发生的在野利用行为，建议受影响的用户应用最新修复程序（版本 12.8.3 Hotfix 2 或更高版本）以保护其网络的安全。  
  
  
参考链接：  
  
https://thehackernews.com/2024/10/cisa-warns-of-active-exploitation-in.html  
  
**PART****0****3**  
  
  
**安全事件**  
  
  
**1.国家安全部：某境外企业“借壳”国内测绘企业非法窃取测绘地理信息**  
  
  
10月16日国家安全部公众号消息，国家安全部公众号发文称，国家安全机关工作发现，某境外企业A公司通过与我国具有测绘资质的B公司合作，以开展汽车智能驾驶研究为掩护，在我国内非法开展地理信息测绘活动。为尽可能直接获取原始测绘数据，A公司越过项目转包的层层节点，全程主导测绘项目进展，直接指挥B公司人员在我国内多省份开展测绘，更是专门委派外籍技术专家对B公司的测绘人员开展实操指导，重点把控测绘数据的存储、处理和流转等环节。最后在A公司的操控指使下，B公司将测绘所得数据转移出境。经鉴定，A公司采集的数据多项属于国家秘密。在此事件中，B公司开展测绘活动时忽视了测绘行业相关规定要求，任由境外企业把控数据流向，导致原始测绘数据失控外传。针对以上情况，国家安全机关会同有关部门开展了联合执法活动。涉事企业和有关责任人员受到了法律追究。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/R7d0-O3mbDndUIyfl5p6lg  
  
  
**2.大量个人信息数据遭境外访问窃取，上海某医疗科技企业被行政处罚**  
  
  
10月14日网信上海公众号消息，上海市网信办接到线索，反映属地某医疗科技公司所属系统存在网络安全漏洞，致使系统大量个人信息数据发生泄漏被境外IP访问窃取。通过调查核实，涉事医疗科技公司为民营医疗机构，主要从事医疗领域教育培训的技术开发服务，涉事系统为该企业内部生产测试系统，部署于云服务平台，系统数据库内存储大量个人信息数据，包含姓名、单位名称、所属省市、所在乡镇/街道、手机号（已采取加密措施）等。该系统未采取有效网络安全防护措施，存在未授权访问漏洞，网络和数据安全管理制度不完善，网络日志留存不足6个月，造成数据泄漏被窃取，违反了《数据安全法》第二十七条规定。针对以上违法情况，上海市网信办依据《数据安全法》第四十五条规定对该医疗科技公司给予警告，并处以罚款的行政处罚。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/p1zx0XpCV6nQwNb9vnD0dQ  
  
  
**3.上市公司科沃斯旗下扫地机被黑并发出骚扰声：用户受惊 官方回应**  
  
  
10月13日The Verge消息，澳媒ABC新闻日前报道称，今年5月，黑客获取了科沃斯地宝 X2 Omni 扫地机器人在多个美国城市的控制权，利用这些机器人追赶宠物并向主人大喊种族歧视性言论，明尼苏达州、埃尔帕索、洛杉矶等多地均有用户反馈。科沃斯公司随后对此声明称，经调查，他们确认了一次“凭证填充攻击”事件，并已屏蔽了相关的 IP 地址。公司强调，目前“没有证据显示”攻击者获得了用户的用户名和密码。  
  
  
原文链接：  
  
https://www.theverge.com/2024/10/12/24268508/hacked-ecovacs-deebot-x2-racial-slurs-chase-pets  
  
  
**4.伊朗政府部门和核设施遭受大规模网络攻击**  
  
  
10月12日Security Affairs消息，伊朗遭受大规模网络攻击，导致伊朗政府服务中断、重要信息被窃取，尤其是核设施也受到影响。伊朗最高网络安全委员会前秘书菲鲁扎巴迪表示，此次网络攻击影响了伊朗政府内部的关键部门，包括司法、立法和行政部门，大量重要信息也因此被窃取；伊朗的核设施以及燃料分配、市政服务、交通和港口的关键网络也成为攻击目标。此次网络攻击被视为以色列对伊朗本月早些时候导弹袭击的可能报复，加剧了两国间持续的紧张局势，可能引爆更大范围的冲突。  
  
  
原文链接：  
  
https://securityaffairs.com/169693/cyber-warfare-2/cyber-attack-hit-iranian-nuclear-facilities.html  
  
  
**PART****0****4**  
  
  
**政策法规**  
  
  
**1.国家数据局《可信数据空间发展行动计划（2024—2028年）》公开征求意见**  
  
  
10月18日，国家数据局研究起草了《可信数据空间发展行动计划（2024—2028年）》，现向社会公开征求意见。该文件指出，可信数据空间是基于共识规则，联接多方主体，实现数据资源共享共用的数据流通利用基础设施。该文件在安全保障方面提出加强两方面能力，一是安全防护能力，可信数据空间应针对数据流通的全生命周期，构建必要的防范和检测技术手段，防止数据泄露、窃取、篡改等危险行为发生，并建立相关的管理制度和应急处置措施；二是合规监管能力，可信数据空间应监测空间中违反相关法律法规的行为，并应在行为发生时及时采取相应的处置措施。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/AeejoraZiqtFRaK9mo2_jw  
  
  
**2.新加坡网络安全局发布《AI系统安全指南》**  
  
  
10月15日，新加坡网络安全局发布了《AI系统安全指南》《AI系统安全配套指南》两份文件，以帮助AI系统所有者在全生命周期内保护AI。这两份文件将有助于保护AI系统，免受供应链攻击等传统网络安全风险和对抗性机器学习等新风险的影响。其中，《AI系统安全配套指南》是一份社区合作编写的参考文件，从工业和学术界精选了实用的措施、安全控制和最佳实践。这两份文件将动态更新，以反映该领域的最新发展。  
  
  
原文链接：  
  
https://www.csa.gov.sg/Tips-Resource/publications/2024/guidelines-on-securing-ai  
  
  
**3.美国防部发布网络安全成熟度模型认证计划最终规则**  
  
  
10月11日，美国防部发布网络安全成熟度模型认证（CMMC）计划最终规则。该规则将评估级别从原来的5个减少至3个，并允许企业视情况对其合规性进行自我评估，以简化中小企业合规成本。联邦合同信息（FCI）的基本保护将需要CMMC1级的自我评估；受控非机密信息（CUI）的一般保护将需要第三方评估或CMMC2级的自我评估；部分需要更高级别保护以防范APT风险的CUI，需要由国防工业基础网络安全评估中心牵头进行CMMC3级评估。  
  
  
原文链接：  
  
https://www.defense.gov/News/Releases/Release/Article/3932947/cybersecurity-maturity-model-certification-program-final-rule-published/  
  
  
**4.欧盟理事会通过《网络弹性法案》**  
  
  
10月10日，欧盟理事会宣布通过《网络弹性法案》，以保护欧盟的所有数字产品免受网络威胁。《网络弹性法案》是全球首个数字产品安全立法，它对所有硬件和软件设置了不同级别的强制性网络安全要求，制造商需要在产品生命周期内实施对应措施，并附带CE标志表明符合法律要求，才可以在欧盟销售。该法案已于3月12日经欧洲议会批准通过，下一步需欧盟理事会主席和欧洲议会主席签署发布才能成为法律。  
  
  
原文链接：  
  
https://www.consilium.europa.eu/en/press/press-releases/2024/10/10/cyber-resilience-act-council-adopts-new-law-on-security-requirements-for-digital-products/  
  
  
**往期精彩推荐**  
  
  
[Spring Framework 路径遍历漏洞(CVE-2024-38819)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502299&idx=1&sn=069a958959ae15278139095c33508054&chksm=fe79ed43c90e6455a2fc9c729de185bf1defc9325a602adc4c8e65889bcf3917632f49bdf156&token=100760615&lang=zh_CN&scene=21#wechat_redirect)  
[Apache Solr 身份认证绕过漏洞(CVE-2024-45216)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502291&idx=1&sn=cde9d034ddebd20192a2489ce2c627d0&chksm=fe79ed4bc90e645dd083897968e7cf49b93a7df4958413e470c664033ac34e6217417b48facf&token=100760615&lang=zh_CN&scene=21#wechat_redirect)  
  
[Oracle 2024年10月补丁日多产品高危漏洞安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502283&idx=1&sn=834229924382fbc68f713cf9d9a8e2ce&chksm=fe79ed53c90e644508dde2790e07290568b6a1bb509d06d88b8de01fefcc99e727038963686d&token=100760615&lang=zh_CN&scene=21#wechat_redirect)  
  
  
  
  
本期周报内容由安全内参&虎符智库&奇安信CERT联合出品！  
  
  
  
  
  
  
  
  
