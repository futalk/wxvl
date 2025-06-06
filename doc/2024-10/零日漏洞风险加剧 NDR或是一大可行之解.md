#  零日漏洞风险加剧 NDR或是一大可行之解   
原创 栗栗  安全419   2024-10-16 18:16  
  
在数字化时代，随着网络攻击手段的不断演进，零日漏洞已成为网络安全的主要威胁之一。那些未被软件厂商知晓的漏洞，一旦被攻击者发现并利用，就能在未打补丁前对系统发起攻击。而攻击者发现并利用漏洞的速度也越来越快，使得零日漏洞成为网络攻击的有力工具。它们不仅影响操作系统和应用软件，还扩展到了物联网和云服务平台。如此看来，基于未知漏洞造成的零日攻击总是防不胜防，传统安全措施难以有效抵御，这也要求各方采取更先进的安全策略和技术，以保护关键数据和系统不受侵害。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/9lmiax2vemggp7HvrLUV4PfAFvmYrPIEy4qXRbQbv2Kic1douiaWaYzLegySibRYqpiaic7GjSq5tnn8mj967eiaztgQQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
  
零日漏洞的威胁持续存在  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/9lmiax2vemggp7HvrLUV4PfAFvmYrPIEyh5PdpKS2cr6O2MbGR14hXtg281ZOr2cNaRBhLAhtv4giatvX7SU6hibA/640?wx_fmt=gif&from=appmsg "")  
  
  
****  
所谓的“零日”，是指黑客对漏洞的处理往往是“发现即公开”。由于没有现成的防御措施，这类安全缺陷成为了攻击者发起隐蔽攻击的理想目标。而随着网络技术的演进，**漏洞从被发现到被利用的时间大大缩减，这一时间间隔已大幅缩短至数天甚至更短，“零日”不一定是真的刚刚发现，黑客完全有可能发现了漏洞却选择隐藏。**  
  
  
近日，高通公司发布了一项重要的安全警告，揭示了其多达64款芯片组存在严重的“零日漏洞”风险。攻击者可以通过运行恶意代码来控制设备，还可能导致数据泄露、系统瘫痪。由于大多数安卓设备使用的是高通芯片，因此安卓手机用户是主要受影响的群体。  
  
  
10月初，云托管服务提供商Rackspace也遭受了一起由第三方软件引发的零日漏洞攻击。该漏洞存在于ScienceLogic的监控应用程序中，允许攻击者远程执行代码，进而入侵Rackspace的内部系统。这次安全事件泄露了敏感的内部信息，暴露了依赖第三方软件服务时可能面临的风险。  
  
  
而此前，Google修复了今年第七个被积极利用的Chrome零日漏洞，这是其一周内修复的第三个漏洞。  
  
  
**零日漏洞的隐蔽性和利用的高效性，使它们成为黑客进行高级持续性威胁（APT）攻击的首选手段。**攻击者通过精心构造的攻击，可以在目标毫无察觉的情况下，悄无声息地窃取数据或破坏系统。这种攻击方式的突发性，给传统基于签名的防御措施带来了巨大挑战，因为这些措施依赖于已知威胁的识别，而对未知的零日漏洞则无能为力。  
  
  
  
传统解决方案为何受挫  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/9lmiax2vemggp7HvrLUV4PfAFvmYrPIEyh5PdpKS2cr6O2MbGR14hXtg281ZOr2cNaRBhLAhtv4giatvX7SU6hibA/640?wx_fmt=gif&from=appmsg "")  
  
  
  
传统的安全解决方案，如安全信息与事件管理 (SIEM)、入侵检测系统 (IDS) 和端点检测与响应 (EDR)，这些工具通常依靠预定义规则、已知签名或行为模式来检测威胁。然而，**零日攻击的本质是全新的、未知的、不可预测的，因此这些被动的安全措施是不够的，其局限性就在于它们对历史数据和静态检测机制的依赖。**例如：  
  
  
**SIEM系统：**根据预定义标准汇总和分析日志数据。如果攻击与已知特征不匹配，就会被忽视。SIEM 中产生的大量错误警报也会削弱 SOC 团队应对“真正”攻击的效率。  
  
  
**IDS工具：**利用已建立的模式监控网络流量中的可疑活动，并及时发现使用新规避技术的零日漏洞。  
  
  
**EDR解决方案：**依赖于签名和行为分析，对使用新型攻击载体的零日漏洞无效。  
  
  
被动预防往往导致检测延迟，使组织在危害发生后才发现漏洞。此外，攻击者越来越多地使用混淆、多态和无文件恶意软件来绕过传统的安全措施。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/9lmiax2vemggp7HvrLUV4PfAFvmYrPIEywFQJ4F3W2Jr6DR98ld35KIhw7sWfibg4kcM5qicEp1a9QOnOTml9HKRA/640?wx_fmt=png&from=appmsg "")  
  
  
因此，组织必须采取更加主动和多层次的安全策略，以识别和防御可能的零日攻击。这包括加强网络监控、实施严格的访问控制、进行定期的安全审计和漏洞评估，以及建立快速响应机制，以便在零日漏洞被公开后，能够迅速采取行动，减轻潜在的损害。同时，提升员工的安全意识，防止通过社会工程等手段被诱导泄露敏感信息，也是防范零日攻击的重要环节。  
  
  
  
高效实现威胁响应  
  
NDR主动出击  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/9lmiax2vemggp7HvrLUV4PfAFvmYrPIEyh5PdpKS2cr6O2MbGR14hXtg281ZOr2cNaRBhLAhtv4giatvX7SU6hibA/640?wx_fmt=gif&from=appmsg "")  
  
  
****  
鉴于传统解决方案的局限性，积极主动的安全方法至关重要。网络检测和响应（NDR）在这一方面就发挥了关键作用。  
  
  
NDR是一款网络威胁追踪溯源系统，运用先进的威胁检测技术和大数据分析能力，实时监控网络环境，及时发现并防御各种潜在威胁。我们可以将NDR比作高速公路上的巡警。一旦发现违规行为，就会立即采取必要措施，以确保交通安全。同样，NDR可以触发警报、丢弃可疑流量、隔离受影响设备，并生成取证证据。  
  
  
Gartner在今年发布的**《NDR市场指南》**中指出，**“NDR 除了独立部署运行之外，也可以作为更广泛的SOC威胁检测技术的一部分”。**与传统工具不同，即使没有预定义的规则下，NDR都可以利用机器学习和异常检测来识别异常行为和可疑活动。通过持续分析网络流量和元数据，NDR可以识别正常模式的偏差，从而及早发现零日漏洞。这种方法通过提供早期预警和更快的事件响应，大大降低了造成严重影响的风险。  
  
  
NDR解决方案的主要特点：  
  
**实时威胁检测：**通过持续监控网络流量元数据，NDR能够在不依赖静态签名的情况下发现可疑活动。  
  
**机器学习：**启发式分析和AI驱动的算法能够识别新型攻击载体，最小化漏报的可能性。  
  
**详细洞察：**NDR提供了深入的网络活动可见性，使安全团队能够迅速而准确地应对新出现的威胁。  
  
  
目前，NDR技术处于快速发展期，网络威胁仍存在网络攻击常态化、内网资产更新难、海量告警缺关联以及风险处置效率低等普遍问题。安全419了解到，**山石智·感（BDS）**作为NDR流量检测的代表产品，是山石网科内网安全解决方案的核心组件产品。其聚焦于内网风险态势感知，能够有效地帮助用户实时检测南北向渗透的已知及未知网络威胁，识别透视东西向的威胁扩散及异常流量，并针对威胁及异常进行精准的定位和详细的溯源取证，同时可根据用户的业务需求，选择手动或自动联动山石网科下一代防火墙或入侵检测系统进行威胁及异常控制拦截。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/9lmiax2vemggp7HvrLUV4PfAFvmYrPIEyqib0oT9mib8jBbictfGJpiaGbmETPyoJ47SbRibpjicbMZN34h8xxlgg68dw/640?wx_fmt=png&from=appmsg "")  
  
山石智·感（BDS）产品能力  
  
  
绿盟科技在原有威胁检测探针UTS基础上，推出了全新下一代**威胁检测一体机UTS-NDR**，集威胁检测、关联分析、专项分析、回溯分析、态势呈现于一体。此外还推出集合平台的全新形态——**绿盟威胁检测探针UTS-NDR全流量威胁分析响应一体机**，践行常态化攻防演练新理念，构建体系化安全防护能力，检测、分析、阻断、溯源。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/9lmiax2vemggp7HvrLUV4PfAFvmYrPIEyxnArzvUjNoGhfkEes6nVOS3vNorfCRzt37xlAPibCpiazWCr99MZNWHg/640?wx_fmt=png&from=appmsg "")  
  
UTS-NDR功能  
  
  
  
零日漏洞的风险正在加剧，网络安全形势日益严峻。在这种背景下，NDR解决方案被视为一种可行的应对策略。NDR的引入为企业提供了一种更为全面和动态的安全防护方案，使其能够在面对零日漏洞等复杂威胁时，保持更高的安全性和灵活性。通过实施NDR，组织不仅能够增强自身的安全防护能力，还能在快速变化的网络环境中，保持对潜在威胁的敏感性和应对能力。  
  
  
  
END  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/9lmiax2vemggp7HvrLUV4PfAFvmYrPIEykFmLlDQOFV3zav3icwiaTm4CJgzlZGh2ice44kJHO7Xic1WLHssZ6abvJQ/640?wx_fmt=gif&from=appmsg "")  
  
  
✦  
  
**推荐阅读**  
  
✦  
  
  
[](http://mp.weixin.qq.com/s?__biz=MzUyMDQ4OTkyMg==&mid=2247543192&idx=1&sn=7b974c82e5c7cfb2ba02420c3bf91541&chksm=f9ebf735ce9c7e2369b2e52f5261d57380988a273d72ae036209c32461acaf1e695d179fbb97&scene=21#wechat_redirect)  
  
[](http://mp.weixin.qq.com/s?__biz=MzUyMDQ4OTkyMg==&mid=2247543130&idx=1&sn=7a97df410f0a96beab6dee67d4ebd3a3&chksm=f9ebf7f7ce9c7ee1e750375045b61788f46d61bb6b4a6fa7578161d9392764ae18508e05c85e&scene=21#wechat_redirect)  
  
[](http://mp.weixin.qq.com/s?__biz=MzUyMDQ4OTkyMg==&mid=2247543037&idx=1&sn=8cc124cd63757a234832beee2f81ce23&chksm=f9ebf650ce9c7f46fc3ca7d0e84d86f68612857300965609a601d750dbeb91121fa5f5b2d1cd&scene=21#wechat_redirect)  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/9lmiax2vemggp7HvrLUV4PfAFvmYrPIEyyjolnqTbxSmgB6nLrR5gVbumXR8o7xJIYB0xPuyiaiaI7QWVsibDGBxcQ/640?wx_fmt=jpeg&from=appmsg "")  
  
**粉丝福利群开放啦**  
  
加安全419好友进群  
  
红包/书籍/礼品等不定期派送  
  
