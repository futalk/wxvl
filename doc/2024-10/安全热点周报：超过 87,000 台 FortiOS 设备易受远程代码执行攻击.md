#  安全热点周报：超过 87,000 台 FortiOS 设备易受远程代码执行攻击   
 奇安信 CERT   2024-10-14 17:15  
  
<table><tbody style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><tr bgless="lighten" bglessp="20%" data-bglessp="40%" data-bgless="lighten" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 4px solid rgb(68, 117, 241);visibility: visible;"><th align="center" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;background-color: rgb(254, 254, 254);font-size: 20px;line-height: 1.2;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(68, 117, 241);visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;font-size: 17px;visibility: visible;">安全资讯导视 </span></strong></span></th></tr><tr data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 广东省教育厅短信平台遭入侵，向师生家长群发非法链接短信</p></td></tr><tr data-bglessp="40%" data-bgless="lighten" data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 打破物理隔离！多个政府机密系统遭APT组织攻破</p></td></tr><tr data-bcless="lighten" data-bclessp="40%" style="-webkit-tap-highlight-color: transparent;outline: 0px;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td align="center" valign="middle" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;border-width: 0px;border-style: none;border-color: initial;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;">• 法德网络安全机构联合发布AI编程助手安全使用指南</p></td></tr></tbody></table>  
  
**PART****0****1**  
  
  
**漏洞情报**  
  
  
**1.Mozilla Firefox释放后重用漏洞安全风险通告**  
  
  
10月12日，奇安信CERT监测到Mozilla发布公告称Mozilla Firefox Animation timelines 释放后重用漏洞(CVE-2024-9680)存在在野利用，远程攻击者能够通过利用 Animation timelines 中的释放后使用漏洞在内容进程中实现代码执行。目前，此漏洞已发现在野利用。鉴于此漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**2.GitLab EE权限绕过漏洞安全风险通告**  
  
  
10月10日，奇安信CERT监测到官方修复GitLab EE 权限绕过漏洞(CVE-2024-9164)，攻击者可以在某些情况下以其他用户的身份利用该漏洞在GitLab的任意分支上执行管道，从而可能执行恶意代码或泄露敏感信息。奇安信鹰图资产测绘平台数据显示，该漏洞关联的国内风险资产总数为1500205个，关联IP总数为27508个。鉴于该漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**PART****0****2**  
  
  
**新增在野利用**  
  
  
**1.****Fortinet FortiOS 格式字符串漏洞(CVE-2024-23113)**  
  
  
10月9日，一个影响超过 87,000 台 FortiOS 设备的严重安全漏洞被发现，使这些设备容易受到潜在的远程代码执行 (RCE) 攻击。该漏洞编号为 CVE-2024-23113，影响 FortiOS、FortiProxy、FortiPAM 和 FortiWeb 产品的多个版本。  
  
该漏洞源于 FortiOS fgfmd 守护程序中使用外部控制的格式字符串，这允许未经身份验证的远程攻击者通过特制的请求执行任意代码或命令。此严重缺陷的 CVSS 评分为 9.8（满分 10 分），表明其严重性。  
  
Shadowserver 扫描显示，已确定约 87,390 个与潜在易受攻击的 Fortinet 设备相关的 IP 地址。美国受影响的设备数量最多，达 14,000 台，其次是日本（5,100 台）和印度（4,800 台）。  
  
该漏洞影响 FortiOS 7.0 至 7.4.2 版本，以及 FortiPAM、FortiProxy 和 FortiWeb 的各个版本。Fortinet 已针对受影响的产品发布了补丁，并强烈建议用户升级到最新的安全版本。  
  
美国网络安全和基础设施安全局 (CISA)已将CVE-2024-23113 添加到其已知被利用漏洞目录中，并指出有证据表明该漏洞被积极利用。鉴于 Fortinet 产品在企业和政府网络中的广泛使用，此漏洞对全球组织构成了重大风险。安全专家敦促立即采取行动以减轻威胁。  
  
  
参考链接：  
  
https://cybersecuritynews.com/87000-fortios-rce-attacks/  
  
  
**2.****Ivanti CSA SQL注入漏洞(CVE-2024-9379)&Ivanti CSA 操作系统命令注入漏洞(CVE-2024-9380)**  
  
  
10月9日，美国 IT 软件公司 Ivanti 发布了安全更新，以修复三个被标记为在攻击中被积极利用的新型云服务设备 (CSA) 零日漏洞。正如 Ivanti 透露的那样，攻击者将这三个安全漏洞与9 月份修补的另一个 CSA 零日漏洞结合在一起。  
  
在 CVE-2024-9379 中，Ivanti 云服务设备 (CSA) 5.0.2 之前的版本在管理 Web 控制台中包含一个 SQL 注入漏洞，该漏洞允许以管理员身份身份验证的远程攻击者运行任意 SQL 语句；在 CVE-2024-9380 中，Ivanti 云服务设备 (CSA) 在管理控制台中包含一个操作系统命令注入漏洞，该漏洞允许具有应用程序管理员权限的经过身份验证的攻击者将命令传递到底层操作系统。成功利用这些漏洞可以让远程攻击者通过 SQL 注入运行 SQL 语句，通过命令注入执行任意代码，以及通过滥用易受攻击的 CSA 网关（用于为企业用户提供对内部网络资源的安全访问）上的路径遍历弱点来绕过安全限制。  
  
Ivanti警告称：“我们了解到，运行 CSA 4.6 patch 518 及之前版本的少数客户在 CVE-2024-9379、CVE-2024-9380 或 CVE-2024-9381 与 CVE-2024-8963 结合时遭到利用。”该公司表示，这些漏洞影响 CSA 5.0.1 及更早版本，并建议怀疑其系统已在这些攻击中受到损害的客户使用 5.0.2 版本重建其 CSA 设备。  
  
为了检测漏洞利用尝试，管理员应查看来自端点检测和响应 (EDR) 或其他安全软件的警报。他们还可以通过检查新的或修改后的管理员用户来观察入侵迹象。  
  
由于 CSA 4.6 是已停产的产品，并于 9 月份发布了最后一个安全补丁，因此建议仍在运行此版本的客户尽快升级到 CSA 5.0.2。  
  
  
参考链接：  
  
https://www.bleepingcomputer.com/news/security/ivanti-warns-of-three-more-csa-zero-days-exploited-in-attacks/  
  
  
**3.****Microsoft Management Console 远程代码执行漏洞(CVE-2024-43572)**  
  
  
10月9日，微软共发布了117个漏洞的补丁程序，修复了Microsoft Management Console、 Windows MSHTML Platform、Microsoft Configuration Manager等产品中的漏洞。CVE-2024-43572是 Microsoft 管理控制台 (MMC) 中的一个 RCE 漏洞。它的 CVSSv3 评分为 7.8，被评为重要。攻击者可以通过使用社交工程策略诱使易受攻击的目标打开特制文件来利用此漏洞。成功利用将允许攻击者执行任意代码。据 Microsoft 称，CVE-2024-43572 被广泛用作零日漏洞。这是 Microsoft 连续第二个月修补 MMC 中的 RCE 漏洞，因为 Microsoft在其2024 年 9 月补丁星期二版本中解决了CVE-2024-38259。  
  
作为 CVE-2024-43572 补丁的一部分，微软改变了 Microsoft 保存的控制台 (MSC) 文件的行为，防止在系统上打开不受信任的 MSC 文件。  
  
建议管理员和家庭用户尽快测试和部署微软官方发布的补丁，以避免已知漏洞的攻击。  
  
  
参考链接：  
  
https://www.tenable.com/blog/microsoft-october-2024-patch-tuesday-addresses-117-cves-cve-2024-43572-cve-2024-43573  
  
  
**4.****Windows MSHTML 平台欺骗漏洞(CVE-2024-43573)**  
  
  
10月9日，微软共发布了117个漏洞的补丁程序，修复了Microsoft Management Console、 Windows MSHTML Platform、Microsoft Configuration Manager等产品中的漏洞。CVE-2024-43573是 Windows MSHTML 平台中的一个欺骗漏洞。它的 CVSSv3 评分为 6.5，评级为中等。未经身份验证的远程攻击者可以通过诱使潜在目标打开恶意文件来利用此漏洞。据 Microsoft 称，CVE-2024-43573 已被广泛利用为零日漏洞。  
  
微软并未提及这个漏洞是如何被利用的，被谁利用，以及漏洞的传播范围有多广。微软对报告 CVE-2024-43572 的研究人员 Andres 和 Shady 表示感谢，但对 CVE-2024-43573 却未表示感谢，因此这可能是一个补丁绕过案例。  
  
Tenable 高级研究工程师 Satnam Narang 在与一份声明中表示：“自从发现 CVE-2024-43572 以来，微软现在阻止在系统上打开不受信任的 MSC 文件。”  
  
建议管理员和家庭用户尽快测试和部署微软官方发布的补丁，以避免已知漏洞的攻击。  
  
  
参考链接：  
  
https://www.tenable.com/blog/microsoft-october-2024-patch-tuesday-addresses-117-cves-cve-2024-43572-cve-2024-43573  
  
  
**5.****Qualcomm Chipsets 释放后重用漏洞(CVE-2024-43047)**  
  
  
10月8日，高通公司发布了针对数字信号处理器 (DSP) 服务零日漏洞的安全补丁，该漏洞影响了数十款芯片组。该安全漏洞 ( CVE-2024-43047 ) 由 Google Project Zero 的 Seth Jenkins、安全研究员 Conghui Wang 和国际特赦组织安全实验室报告。该漏洞由释放后重用引起，如果本地攻击者以较低的权限成功利用该漏洞，则可能导致内存损坏。  
  
目前，DSP 使用未使用的 DMA 句柄 fds 更新标头缓冲区。在 put_args 部分中，如果标头缓冲区中存在任何 DMA 句柄 FD，则释放相应的映射。但是，由于未签名的 PD 中的头缓冲区暴露给用户，因此用户可以更新无效的 FD。如果此无效 FD 与任何已在使用的 FD 匹配，则可能导致释放后重用 (UAF) 漏洞。  
  
正如该公司在周一的安全公告中警告的那样，谷歌威胁分析小组和安全实验室的研究人员将该漏洞标记为已被广泛利用。这两个组织都因发现间谍软件利用的零日漏洞而出名，这些漏洞针对的是高风险个人（包括记者、反对派政客和异见人士）的移动设备  
  
高通警告称：“谷歌威胁分析小组的迹象表明，CVE-2024-43047 可能受到有限的、有针对性的利用。”针对影响 FASTRPC 驱动程序的问题的补丁已提供给 OEM，并强烈建议尽快在受影响的设备上部署更新。  
  
高通还敦促用户联系其设备制造商，以获取有关其特定设备补丁状态的更多详细信息。  
  
  
参考链接：  
  
https://www.bleepingcomputer.com/news/security/qualcomm-patches-high-severity-zero-day-exploited-in-attacks/  
  
**PART****0****3**  
  
  
**安全事件**  
  
  
**1.广东省教育厅短信平台遭入侵，向师生家长群发非法链接短信**  
  
  
10月12日综合消息，广东省教育厅发布声明称，发现有不法分子入侵该部门短信平台，以“广东省教育厅”名义向师生和家长发送包含非法链接的短信。该部门已第一时间向公安机关报案，并配合开展调查。请广大师生和家长提高警惕，切勿点击短信中的非法链接，避免个人信息泄露或遭受财产损失。此前，社交媒体上有多位用户表示，收到一条来自“广东省教育厅”的短信，写着“深夜必备成人电影戳链接”与非法内容链接。  
  
  
原文链接：  
  
https://www.secrss.com/articles/71121  
  
  
**2.打破物理隔离！多个政府机密系统遭APT组织攻破**  
  
  
10月8日BleepingComputer消息，欧洲网络安全厂商ESET发布报告称，名为GoldenJackal的APT组织成功攻破了欧洲政府机构的气隙隔离系统。该组织至少成功实施了两起攻击行动，第一起发生在2019年9月和2021年7月，目标是某南亚国家驻白俄罗斯大使馆。第二起针对的是一个欧洲政府机构，发生在2022年5月至2024年3月之间。黑客使用了两套自定义工具集窃取了大量敏感数据，包括电子邮件、加密密钥、图像、档案以及文件。此前2023年5月，卡巴斯基发布关于GoldenJackal组织攻击活动的报告，指出该组织专注于政府和外交机构，主要目的是进行间谍活动。  
  
  
原文链接：  
  
https://www.bleepingcomputer.com/news/security/european-govt-air-gapped-systems-breached-using-custom-malware/  
  
  
**3.美国水务巨头遭网络攻击：水计费系统瘫痪，上千万人无法处理账单**  
  
  
10月8日The Record消息，美国水务公司（American Water Works）7日发布SEC文件称，其供水和废水设施未受到上周发生的网络攻击影响。该公司管理层在网站上警告，为遏制此次攻击采取了停用和隔离措施，客户目前无法访问用于管理个人账户和支付水费的门户网站。目前公司的MyWater账户系统已瘫痪，所有客户预约的服务将被重新安排。所有账单处理已暂停，直至另行通知。但是，系统恢复上线之前，不会产生逾期费用或停止服务。该公司的呼叫中心也无法正常运作。美国水务公司是美国最大的受监管水务公共事业公司。  
  
  
原文链接：  
  
https://therecord.media/american-water-works-cyberattack-utility  
  
  
**PART****0****4**  
  
  
**政策法规**  
  
  
**1.国家发改委《公共数据资源登记管理暂行办法》公开征求意见**  
  
  
10月12日，国家发展改革委会同有关部门起草了《公共数据资源登记管理暂行办法》，现向社会公开征求意见。该文件共5章27条，包括总则、登记要求、登记程序、登记管理、监督管理。该文件提出，公共数据资源登记应当维护国家安全和公共利益，保护国家秘密、商业秘密、个人隐私和个人信息权益，遵循依法合规、公开透明、标准规范、安全高效的原则。该文件要求，登记机构应建立健全数据资源登记管理责任机制，履行数据安全保护义务，妥善保管登记信息，登记主体在申请登记前应当自行或委托第三方专业服务机构，通过技术手段在保障安全的前提下对公共数据资源进行存证，确保来源可查、加工可控。  
  
  
原文链接：  
  
https://yyglxxbs.ndrc.gov.cn/file-submission/20241012164606227325.doc  
  
  
**2.国家数据局《公共数据资源授权运营实施规范（试行）》公开征求意见**  
  
  
10月12日，国家数据局会同有关部门研究起草了《公共数据资源授权运营实施规范（试行）》（公开征求意见稿），现向社会公开征求意见。该文件共7章26条，包括总则、基本要求、方案编制、协议签订、运营实施、运营管理、附则。该文件提出，公共数据资源授权运营应遵循依法合规、公平透明、公益优先、合理收益、安全可控的原则。该文件要求，实施机构应建立健全管理制度，明确数据分类分级安全保护要求，加强技术支撑保障和数据安全管理；运营机构应履行数据安全主体责任，加强内控管理、技术管理和人员管理，不得超授权范围使用公共数据资源，严防数据加工、处理、运营、服务等环节数据安全风险。实施机构、运营机构应通过管理和技术措施，加强数据关联汇聚风险识别和管控，保障数据安全。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/Mewd4PC29jN-z1CC-myhcQ  
  
  
**3.住建部公布《“数字住建”建设整体布局规划》**  
  
  
10月12日，住房和城乡建设部公布《“数字住建”建设整体布局规划》。该文件提出到2027年底，“数字住建”建设取得显著成效，一体化数字基础设施和数据资源体系建成运行，数字化政策标准和安全防护支撑能力明显提升。该文件要求坚持安全可控原则，落实网络和数据安全主体责任，构建制度、管理和技术衔接配套的安全防护体系，强化基础设施、数据资源和应用平台等安全保障能力，守牢网络和数据安全底线。  
  
  
原文链接：  
  
https://www.mohurd.gov.cn/gongkai/zhengce/zhengcefilelib/202410/20241011_780320.html  
  
  
**4.中共中央办公厅、国务院办公厅发布《关于加快公共数据资源开发利用的意见》**  
  
  
10月9日，中共中央办公厅、国务院办公厅联合发布《关于加快公共数据资源开发利用的意见》。该文件共6章17条，其中第12条要求加强安全管理。具体包括强化数据安全和个人信息保护，加强对数据资源生产、加工使用、产品经营等开发利用全过程的监督和管理。建立健全分类分级、风险评估、监测预警、应急处置等工作体系，开展公共数据利用的安全风险评估和应用业务规范性审查。运营机构应依据有关法律法规和政策要求，履行数据安全主体责任，采取必要安全措施，保护公共数据安全。加强技术能力建设，提升数据汇聚关联风险识别和管控水平。依法依规予以保密的公共数据不予开放，严格管控未依法依规公开的原始公共数据直接进入市场。  
  
  
原文链接：  
  
https://www.gov.cn/zhengce/202410/content_6978910.htm  
  
  
**5.《网络安全技术 办公设备安全规范》等9项网络安全国家标准获批发布**  
  
  
10月9日，根据2024年9月29日国家市场监督管理总局、国家标准化管理委员会发布的中华人民共和国国家标准公告（2024年第22号），全国网络安全标准化技术委员会归口的9项国家标准正式发布。具体包括《网络安全技术 信息安全控制》《网络安全技术 网络和终端隔离产品技术规范》《网络安全技术 办公设备安全规范》《网络安全技术 智能门锁网络安全技术规范》《网络安全技术 实体鉴别 第2部分：采用鉴别式加密的机制》《网络安全技术 消息鉴别码 第2部分：采用专门设计的杂凑函数的机制》《网络安全技术 杂凑函数 第1部分：总则》《网络安全技术 杂凑函数 第2部分：采用分组密码的杂凑函数》《网络安全技术 杂凑函数 第3部分：专门设计的杂凑函数》。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/735HV8NvO0hOuTlWEFNdow  
  
  
**6.我国工业互联网安全领域首批国家标准发布**  
  
  
10月8日，根据2024年9月29日国家市场监督管理总局、国家标准化管理委员会发布的中华人民共和国国家标准公告（2024年第22号），全国通信标准化技术委员会归口的《工业互联网企业网络安全 第1部分：应用工业互联网的工业企业防护要求》、《工业互联网企业网络安全 第2部分：平台企业防护要求》、《工业互联网企业网络安全 第3部分：标识解析企业防护要求》三项工业互联网企业网络安全国家标准发布。这三项标准聚焦三类工业互联网企业网络安全防护需求，提出不同级别企业的网络安全防护要求，为企业实施工业互联网安全分类分级管理工作提供指导，是我国实施工业互联网企业网络安全分类分级管理与防护工作的创新成果与经验总结。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/ULN-4m_2AP0Czg7vLTCd7Q  
  
  
**7.国家发改委等六部门联合印发《国家数据标准体系建设指南》**  
  
  
10月8日，国家发展改革委、国家数据局、中央网信办、工业和信息化部、财政部、国家标准委联合印发《国家数据标准体系建设指南》。该文件提出，到2026年底基本建成国家数据标准体系。该文件以数据“供得出、流得动、用得好、保安全”为指引，从基础通用、数据基础设施、数据资源、数据技术、数据流通、融合应用、安全保障等7个部分，加快构建数据标准体系，全面指导数据标准化工作开展。其中安全保障部分包括数据基础设施安全、数据要素市场安全、数据流通安全等三方面。  
  
  
原文链接：  
  
https://www.ndrc.gov.cn/xxgk/zcfb/tz/202410/P020241008585506324012.pdf  
  
  
**8.法德网络安全机构联合发布AI编程助手安全使用指南**  
  
  
10月4日，法国网络安全局（ANSSI）和德国联邦信息安全办公室（BSI）共同编制发布了《AI编程助手》，为开发人员安全使用AI编码助手提供了风险和安全建议。该文件提出，在软件开发中使用AI编程助手可能面临敏感信息通过用户输入泄露、输出低质量或不安全的代码、供应链攻击、被攻击者滥用等风险。该文件指出，AI编程助手无法替代经验丰富的开发人员，无节制使用此类工具将带来严重安全隐患。该文件建议，引入AI工具应进行系统的风险分析，并加强质量管理。  
  
  
原文链接：  
  
https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/KI/ANSSI_BSI_AI_Coding_Assistants.pdf  
  
  
**往期精彩推荐**  
  
  
[【在野利用】Mozilla Firefox 释放后重用漏洞(CVE-2024-9680)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502274&idx=1&sn=7cfe89f647e27a1ff53d00457d072c3c&chksm=fe79ed5ac90e644c9213d0dd5c02c04727b85122b3e6558a7599c86fff12346e349b4aa8de03&token=1666619612&lang=zh_CN&scene=21#wechat_redirect)  
[GitLab EE 权限绕过漏洞(CVE-2024-9164)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502254&idx=1&sn=fec9775ba1e86303d57983d5abccbda0&chksm=fe79ed36c90e64201cf1e17a80cb904bde5f1f28ef71bb5d77de8cf9f881d32adf70040580da&token=1666619612&lang=zh_CN&scene=21#wechat_redirect)  
  
[微软10月补丁日多个产品安全漏洞风险通告：2个在野利用、3个紧急漏洞](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247502240&idx=1&sn=9d1336306a715b56d6e6705fbe518d38&chksm=fe79ed38c90e642e4a1c2d6f9c60bac4b495f718856cefabd2e6b3466ec79ce5575465d27828&token=1666619612&lang=zh_CN&scene=21#wechat_redirect)  
  
  
  
  
本期周报内容由安全内参&虎符智库&奇安信CERT联合出品！  
  
  
  
  
  
  
  
  
