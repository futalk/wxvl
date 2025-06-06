#  OpenVPN存在RCE 和 LPE 漏洞   
flyme  独眼情报   2024-08-10 12:32  
  
微软研究人员最近在 OpenVPN 中发现了多个中等严重性漏洞，OpenVPN 是一个开源项目，其二进制文件集成到全球的路由器、固件、PC、移动设备和许多其他智能设备中，数量达数百万。攻击者可以串联和远程利用一些发现的漏洞，以实现由远程代码执行 (RCE) 和本地特权提升 (LPE) 组成的攻击链。此攻击链可使攻击者完全控制目标端点，从而可能导致数据泄露、系统入侵和未经授权访问敏感信息。然而，利用这些漏洞需要用户身份验证和对 OpenVPN 内部工作原理的深入了解，以及对操作系统的中级知识。今天，我们在  
Black Hat USA 2024 的  
会议上介绍了这项研究并演示了发现的攻击链。  
  
OpenVPN 被各行各业的  
数千家  
公司广泛使用，涉及 Windows、iOS、macOS、Android 和 BSD 等主要平台。因此，利用已发现的漏洞（影响  
2.6.10  
版（和  
2.5.10 版  
）之前的所有 OpenVPN 版本）可能会使端点和企业面临重大攻击风险。  
  
我们   
 于 2024 年 3 月通过  
Microsoft 安全漏洞研究(MSVR) 的  
协调漏洞披露  
 (CVD) 向 OpenVPN 报告了这一发现，并与 OpenVPN 密切合作，以确保修补漏洞。有关 OpenVPN 发布的解决这些漏洞的安全修复程序的信息可在此处找到：   
OpenVPN 2.6.10  
 。我们强烈敦促 OpenVPN 用户尽快  
应用  
最新的安全更新。我们也感谢 OpenVPN 的合作，并认识到解决这些漏洞的紧迫性。  
  
  
  
  
  
  
以下是本博客中讨论的已发现漏洞的列表：  
  
<table><tbody><tr><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem 1.5rem 1.5rem;vertical-align: top;word-break: break-word;"><span style="font-weight: bolder;">CVE 编号</span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="font-weight: bolder;">OpenVPN 组件</span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="font-weight: bolder;">影响</span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 1.5rem 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="font-weight: bolder;">受影响的平台</span></td></tr><tr><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem 1.5rem 1.5rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">CVE-2024-27459</span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">openvpnserv                             </span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">拒绝服务 (DoS)、本地特权提升 (LPE)</span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 1.5rem 1.5rem 0.75rem;vertical-align: top;word-break: break-word;">windows</td></tr><tr><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem 1.5rem 1.5rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">CVE-2024-24974</span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">openvpnserv                             </span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">未经授权的访问 </span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 1.5rem 1.5rem 0.75rem;vertical-align: top;word-break: break-word;">windows</td></tr><tr><td rowspan="2" style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem 1.5rem 1.5rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">CVE-2024-27903</span></td><td rowspan="2" style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">openvpnserv</span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">远程代码执行（RCE）</span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 1.5rem 1.5rem 0.75rem;vertical-align: top;word-break: break-word;">windows</td></tr><tr><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 0.75rem 1.5rem 1.5rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">本地特权提升 (LPE)、数据操纵</span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(210, 210, 210);border-left: inherit;padding: 1.5rem 1.5rem 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">Android、iOS、macOS、BSD</span></td></tr><tr><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(117, 117, 117);border-left: inherit;padding: 1.5rem 0.75rem 1.5rem 1.5rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">CVE-2024-1305</span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(117, 117, 117);border-left: inherit;padding: 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">Windows TAP 驱动程序 </span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(117, 117, 117);border-left: inherit;padding: 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">拒绝服务 (DoS) </span></td><td style="border-top-color: initial;border-right: inherit;border-bottom-width: 0.0625rem;border-bottom-color: rgb(117, 117, 117);border-left: inherit;padding: 1.5rem 1.5rem 1.5rem 0.75rem;vertical-align: top;word-break: break-word;"><span style="vertical-align: inherit;">视窗</span></td></tr></tbody></table>  
在这篇博文中，我们详细介绍了对发现的漏洞及其利用影响的分析。除了修补之外，我们还提供了缓解和检测试图利用这些漏洞的威胁的指导。这项研究强调了安全社区之间负责任的披露和协作的必要性，以保护跨平台的设备并在整个用户设备生态系统中为所有设备构建更好的保护。这些漏洞的发现进一步凸显了确保企业和端点系统安全的重要性，并强调了持续监控和保护这些环境的必要性。  
## 什么是 OpenVPN？  
  
OpenVPN 是一种虚拟专用网络 (VPN) 系统，可在网络之间创建私密且安全的点对点或站点对站点连接。OpenVPN 开源项目在全球广受欢迎，包括美国、印度、法国、巴西、英国和德国，以及信息技术、金融服务、电信和计算机软件领域的行业。该项目支持不同的主流平台，并已集成到全球数百万台设备中。  
  
OpenVPN 也是其使用的隧道协议的名称，该协议采用安全套接字层 (SSL) 加密协议，使用 AES-256 加密确保通过互联网共享的数据保持私密。由于源代码可供审计，因此可以轻松识别和修复漏洞。  
## OpenVPN分析  
  
我们在检查 OpenVPN 开源项目以提高企业安全标准时发现了这些漏洞。在这项研究中，我们检查了另外两种流行的 VPN 解决方案，发现它们当时受到漏洞 (CVE-2024-1305) 的影响。发现这一漏洞后，我们开始寻找并发现了存在相同问题的其他易受攻击的驱动程序，并决定调查开源 VPN 项目。在确认相同的漏洞位于 OpenVPN 开源存储库中后，我们的研究重点是检查适用于 Windows 系统的 OpenVPN 项目的架构和安全模型。  
### OpenVPN 架构  
#### OpenVPN 服务器客户端架构  
  
OpenVPN 是一个复杂的 VPN 系统，经过精心设计，可建立安全的点对点或站点对站点连接。它支持路由和桥接配置以及远程访问功能，是满足各种网络需求的多功能选择。OpenVPN 包括客户端和服务器应用程序，可确保提供全面的安全通信解决方案。  
  
使用 OpenVPN，对等端可以通过多种方法相互验证身份，包括预共享密钥、证书或用户名/密码组合。在多客户端服务器环境中，服务器可以利用强大的数字签名和受信任的证书颁发机构为每个客户端生成并颁发单独的身份验证证书。这可确保身份验证过程中的安全性和完整性达到更高水平，从而增强 VPN 连接的整体可靠性。   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGJKCV7jHCBgzrxY4mLWTy6Q6bJGdyu3eIIicKLhmG0fbWt1MLvStIZxw/640?wx_fmt=jpeg&from=appmsg "")  
  
图 1. OpenVPN 客户端服务器模型  
### 客户端架构  
  
我们在客户端架构中发现了另外三个漏洞（CVE-2024-27459、CVE-2024-24974 和 CVE-2024-27903）：  
  
OpenVPN的客户端架构可以用下面的简化图来概括：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGNYI8NeukpDFkms5UgV422ZDNNwjpib3OwpUnwwa88icfF150bbgRGiaJg/640?wx_fmt=other&from=appmsg "")  
  
图 2. 已加载 plugin.dll 的 OpenVPN 客户端架构  
#### openvpnserv.exe和openvpn.exe  
  
系统服务代表用户启动提升的命令，处理诸如添加或删除 DNS 配置、IP 地址和路由以及启用动态主机配置协议 (DHCP) 等任务。这些命令是通过为这两个实体创建的命名管道从openvpn.exe  
进程接收的，例如“openvpn/service_XXX”，其中 XXX 是作为命令行参数传递给新创建的进程的线程 ID (TID)。  
  
启动的命令以二进制结构的形式到达，其中包含特定命令的相关信息，结构经过验证后才会启动适当的命令。下图显示了包含添加/删除 DNS 配置信息的结构示例：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGoZb14Y4bYjdRWZmRtmdlkiaic13hONHnYWlV00yt09bIEibhbb9nt5dgg/640?wx_fmt=other&from=appmsg "")  
  
图3. OpenVPN DNS配置管理结构  
  
此外，openvpnserv.exe  
充当管理单元，根据机器上不同用户的请求生成openvpn.exe  
进程。这可以使用 OpenVPN GUI 自动完成，也可以通过发送专门制作的请求完成。此进程的通信通过第二个命名管道进行，例如“openvpn/service”。  
  
Openvpn.exe  
是代表客户端生成的用户模式进程。当openvpn.exe  
启动时，它会接收配置文件的路径（作为命令行参数）。提供的配置文件包含不同的信息。  
  
  
在配置文件  
中可以管理很多字段  
，比如：  
1. 隧道选项  
  
1. 服务器模式选项  
  
1. 客户端模式选项  
  
#### openvpn.exe中的插件机制  
  
我们感兴趣的另一个机制是openvpn.exe  
中的插件机制，它可以扩展功能以添加其他逻辑，例如身份验证插件，以便针对轻量级目录访问协议 (LDAP) 或 Radius 或其他可插入身份验证模块  
(PAM) 后端进行身份验证。一些现有的插件包括：  
1. Radiusplugin – 开放 OpenVPN 的 Radius 身份验证支持。  
  
1. Eurephia – OpenVPN 的身份验证和访问控制插件。  
  
1. Openvpn_defer_auth – OpenVPN 插件用于执行延迟的身份验证请求。  
  
插件机制适合前面的图表，如图 2 所示。  
  
该插件作为配置文件中的指令加载，如下所示：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGR534pVW058Tzp1GqAeAQea2aXLj3wyM6uWtYUTTueCUh49hx4r25HA/640?wx_fmt=other&from=appmsg "")  
  
图 4. OpenVPN 客户端指令加载插件  
  
此外，插件中定义了代表加载进程（openvpn.exe ）启动的  
回调  
数量  
，例如：  
1. openvpn_plugin_func_v1  
 – 每次 OpenVPN 到达应该发生插件调用的点时，OpenVPN 都会调用此函数。  
  
1. openvpn_plugin_{open, func}_v3()  
 – 定义 v3 插件参数的版本。  
  
### OpenVPN安全模型  
  
如前所述，我们在 OpenVPN 架构的客户端发现了四个漏洞。  
  
如前所述，openvpnserv.exe  
（SYSTEM 服务）  
根据用户的请求生成openvpn.exe进程。此外，生成的进程在请求创建新进程的用户的上下文中运行，这是通过命名管道  
  
模拟  
实现的，如下图所示：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzG9gKibQ7m3DXRryokw0ggod3fnzDA91OWGQnHlBoJrp0Yw1dwK1hnK7w/640?wx_fmt=other&from=appmsg "")  
  
图 5. 命名管道模拟  
  
ImpersonateNamedPipeClient  
函数  
模拟  
命名管道客户端应用程序  
 。  
  
此外，为了防止不良行为，必须为任何新进程授予特定的 EXPLICIT_ACCESS：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGKYWpWfn9nrqSReCMmARRIw9Wfjy2DhaIxWcXMnu95TNbyFfDZh81oQ/640?wx_fmt=other&from=appmsg "")  
  
图 6. OVPN DACL 的显式访问  
  
这种明确的访问，加上前面描述的由openvpnserv.exe  
根据openvpn.exe  
进程的请求启动的“提升命令”，以及对传递的参数的其他全面检查，确保不能以模拟用户的名义发起恶意行为。  
## 漏洞分析  
### CVE-2024-1305      
  
我们在“tap-windows6”项目中发现了一个漏洞，该项目涉及开发 OpenVPN 使用的终端接入点 (TAP) 适配器。在项目的src  
文件夹中，device.c  
文件包含 TAP 设备对象及其初始化的代码。  
  
在device.c  
文件中，CreateTapDevice  
方法会初始化一个调度表对象，其中包含管理设备各种输入/输出控制 (IOCTL) 的方法的回调。其中一个方法是TapDeviceWrite  
，它处理写入 IOCTL。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzG47NiauiaSWrjm23MhvL9WW5SxzOwo2zLQueja0JqpVylEib1NfKcicPOBw/640?wx_fmt=other&from=appmsg "")  
  
图 7. 野生内核溢出漏洞位置  
  
TapDeviceWrite  
方法执行多项操作，最终调用TapSharedSendPacket  
。此方法又调用NdisAllocateNetBufferAndNetBufferLists两次  
。在一种情况下，它使用fullLength  
参数调用此函数，定义如下：  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGqics7oTiboch9GVzOP8gVooEBwziaoAX1UgezfCUv0K140kRf798y2CicA/640?wx_fmt=other&from=appmsg "")  
  
图 8. 整数溢出  
  
PacketLength  
和PrefixLength  
都是从TapDeviceWrite  
调用传递的参数  
，因此受攻击者控制。如果这些值足够大，它们的和 ( fullLength  
 ) 可能会溢出（32 位无符号整数）。此溢出会导致分配小于预期的内存大小，进而导致内存溢出问题。  
### CVE-2024-27459    
  
我们发现的第二个漏洞存在于openvpn.exe  
进程和openvpnserv.exe  
服务之间的通信机制中  
。如前所述，两者通过命名管道进行通信：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGf1jERZfEbLhAnQ6BNMqJ4KKia2CQPticHia4sUAAPzJM62sDW7Y7wiaYibA/640?wx_fmt=other&from=appmsg "")  
  
图 9. 从命名管道读取大小  
  
openvpnserv.exe  
服务会以无限循环的方式从openvpn.exe  
进程中读取消息大小  
，然后通过调用HandleMessage  
方法处理收到的消息。HandleMessage  
方法读取无限循环提供的大小，并将读取的字节数相应地转换为相关类型：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGEINia8A9WcvYJA08wbzKQySib6TsbPtnOSTgGPL8VVJlAYbeft8Kv5vg/640?wx_fmt=other&from=appmsg "")  
  
图 10. 堆栈溢出漏洞位置  
  
这种通信机制存在一个问题，因为将“用户”提供的字节数读取到位于堆栈上的“n 字节”长的结构上会产生堆栈溢出漏洞。  
### CVE-2024-24974    
  
第三个漏洞涉及对操作系统资源的非特权访问。openvpnserv.exe  
服务根据通过“\\openvpn\\service”命名管道收到的用户请求生成一个新的openvpn.exe进程。此漏洞允许远程访问命名服务管道，使攻击者能够远程与其交互  
并对其启动操作。  
### CVE-2024-27903    
  
最后，我们发现 OpenVPN 插件机制中存在一个漏洞，该漏洞允许从终端设备上的各种路径加载插件。攻击者可以利用此行为从这些不同路径加载有害插件。  
## 利用并串联漏洞  
  
一旦攻击者获得用户的 OpenVPN 凭据，就可以利用所有已识别的漏洞，攻击者可以使用凭据窃取技术来实现这一目的，例如在暗网上购买被盗凭据、使用信息窃取恶意软件或嗅探网络流量以捕获 NTLMv2 哈希，然后使用 HashCat 或 John the Ripper 等破解工具对其进行解码。然后可以组合发现的漏洞以实现不同的利用结果，或将它们串联起来形成复杂的攻击链，如下节所述。  
### RCE 利用  
  
我们首先探讨了攻击者如何使用 CVE-2024-24974 和 CVE-2024-27903 实现远程代码执行 (RCE) 利用。  
  
要成功利用这些漏洞并实现 RCE，攻击者必须首先获取 OpenVPN 用户的凭据。然后，攻击者的设备必须使用窃取的凭据启动NET USE  
命令，以远程访问操作系统资源并授予攻击者对命名管道对象设备的访问权限。  
  
接下来，攻击者可以向“\\openvpn\\service”命名管道发送“connect”请求，以  
代表其启动openvpn.exe的新实例。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzG2teombE3wKAicX2cLZ2rb68zibSMcqRAPfYctsFl82xvTbJ3GC3O2icUA/640?wx_fmt=other&from=appmsg "")  
  
图 11. 从远程位置初始化 OpenVPN（其中 {TARGET_MACHINE_PLACEHOLDER} 可以用不同的端点替换）  
  
请求中  
指定了位于攻击者控制的设备上配置文件 ( \\\\DESKTOP-4P6938I\\share\\OpenVPN\\config\\sample.ovpn ) 的路径。还提供了加载的插件将在其中写入日志的日志路径 (   
“–log \\\\\{TARGET_MACHINE_PLACEHOLDER} \\share\\OpenVPN\\log\\plugin_log.txt\  
 )。  
  
提供的配置有加载恶意插件的指令，如下所示：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGIZ495MKljFUkqlWS9SCcjSdahRRLLobEytOc4m4Hjib8IQ5XaozFdkw/640?wx_fmt=other&from=appmsg "")  
  
图 12. 来自远程位置的恶意插件加载指令  
  
成功利用后，攻击者可以读取攻击者控制的设备上提供的日志。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGlnOZeqFCic5rAkkndpzWMibQ3QEuFA35ENSxJ31Ifr72msJG0q1Cv4VQ/640?wx_fmt=other&from=appmsg "")  
  
图 13. 攻击者控制的设备上插件日志  
### LPE 开发  
  
接下来，我们研究了攻击者如何使用 CVE-2024-27459 和 CVE-2024-27903 实现本地特权执行 (LPE)。要在此上下文中成功实现 LPE 漏洞利用，攻击者必须使用恶意配置文件将恶意插件加载到openvpn.exe  
的正常启动过程中。  
  
首先，攻击者将使用命令连接到本地设备“\\openvpn\\service”命名管道，该命令指示openvpnserv.exe  
根据攻击者提供的恶意配置  
启动openvpn.exe 。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzG9Dzicz2ib7JyOa5ywFYDNKUZ62OkfADWvCVENKB6ujEX0ib1Dx0ueEy5A/640?wx_fmt=other&from=appmsg "")  
  
图 14. 从本地配置初始化 OpenVPN  
  
恶意配置将包含如下例所示的一行：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzG2kHP4ibwwXgibLggnQ3a46u2y6vFMZAL6aEA9MbI5GQlsb14A2Ky7Gmg/640?wx_fmt=other&from=appmsg "")  
  
图 15. 来自本地位置的恶意插件加载指令  
  
恶意插件要想成功与openvpnserv.exe 通信，必须劫持  
openvpn.exe  
用来与连接openvpv.exe  
进程和openvpnserv.exe  
服务的内部命名管道通信的句柄号  
。例如，可以通过解析命令行参数来实现，如下所示：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGedDkrhnuafqxXqSEOnynLczuqnLxOa3BrYDL77rsQUVLickLUlic8dXg/640?wx_fmt=other&from=appmsg "")  
  
图 16. 解析命令行参数以提取线程 ID (TID)  
  
这是可行的，因为当openvpn.exe  
进程生成时，它会被传递内部命名管道（用于此特定 OpenVPN 实例与openvpnserv.exe  
服务之间的通信）将具有的 TID（作为命令行参数）。例如，如果创建的内部命名管道是“\\openvpn\\service_1234”，那么openvpn.exe  
将使用额外的参数 1234 启动。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGibZ8pbicN5obq0Ls1W8SGCtUB9hfd8VYokvWwZdwoYhMoKiaSvibKoCQdg/640?wx_fmt=other&from=appmsg "")  
  
图 17. 将 TID 作为命令行参数传递  
  
接下来，攻击者可以通过发送大于 MSG 结构的数据来利用堆栈溢出漏洞。值得注意的是，有堆栈保护机制，称为  
堆栈金丝雀  
，这使利用变得更加困难。因此，在触发溢出时：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGN5f38MGOoWBXmxicuo5LicIU5BKgAdVibtJSzckwbAOpQqKZWIicbFfvAg/640?wx_fmt=other&from=appmsg "")  
  
图 18. 触发堆栈溢出  
  
openvpnserv.exe  
崩溃后  
，攻击者有一段时间可以重新获得命名管道“\\openvpn\\service”。  
  
如果成功，攻击者就会冒充命名管道“\\openvpn\\service”的服务器客户端。从那一刻起，每次尝试连接到“\\openvpn\\service”命名管道都将导致与攻击者的连接。如果具有足够权限的用户（例如 SYSTEM 或管理员用户）连接到命名管道，攻击者可以冒充该用户：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/KgxDGkACWnRU3lnnZAEvR1xtNweMicnzGD9h05600VPucWKuzT8rhAFOl7XyntL6lNMWhMIy02zbwicwSgxoW6bQ/640?wx_fmt=other&from=appmsg "")  
  
图 19. 冒充特权用户  
  
然后，攻击者可以代表用户启动提升的进程，从而实现 LPE。  
### 把所有东西串联起来  
  
我们的研究表明，攻击者可以利用已发现的四个漏洞中的至少三个来创建漏洞利用程序以实现 RCE 和 LPE，然后将它们链接在一起以创建强大的攻击链。  
  
要利用本博文中介绍的完整攻击链，需要进行一些调整，主要是使openvpnserv.exe崩溃的恶意负载和在  
openvpnserv.exe  
崩溃后实际表现为openvpnserv.exe  
的恶意负载  
都必须加载恶意插件。成功实现 LPE 后，攻击者将使用不同的技术，例如  
自带易受攻击的驱动程序 (BYOVD)  
或利用已知漏洞，以更牢固地控制端点。通过这些技术，攻击者可以禁用 Microsoft Defender 等关键进程的 Protect Process Light (PPL) 或绕过和干扰系统中的其他关键进程。这些操作使攻击者能够绕过安全产品并操纵系统的核心功能，进一步巩固他们的控制并避免被发现。  
  
  
## 端点安全对于私营和企业部门至关重要  
  
由于 OpenVPN 被广泛用于各种供应商、行业和领域，因此所提出的漏洞可能会影响众多行业、设备类型和垂直行业。利用这些漏洞需要用户身份验证、深入了解 OpenVPN 的内部工作原理以及对操作系统的中级了解。但是，成功的攻击可能会严重影响私营和企业部门的端点。攻击者可以使用易受攻击的 OpenVPN 版本对设备发起全面的攻击链，从而完全控制目标端点。这种控制使他们能够窃取敏感数据、篡改数据，甚至擦除和销毁关键信息，从而对私营和企业环境造成重大损害。  
  
这些漏洞的发现凸显了负责任地披露信息对于保护企业和终端系统的重要性，此外，安全社区也需要共同努力保护跨平台的设备，为每个人建立更强大的保障措施。我们再次感谢 OpenVPN 的合作以及他们迅速采取行动解决这些漏洞。  
## 缓解和保护指导  
  
2.5.10 和 2.6.10 之前的 OpenVPN 版本容易受到上述漏洞的影响。  
  
建议首先确定是否安装了易受攻击的版本，如果是，请立即应用此处的相关补丁：  
OpenVPN 2.6.10  
。  
  
此外，请遵循以下建议以进一步减轻与发现的漏洞相关的潜在利用风险：  
- 对网络中受影响的设备应用补丁。查看  
OpenVPN 网站  
以获取最新补丁。  
  
- 确保 OpenVPN 客户端已与互联网断开连接并已分段。  
  
- 仅限授权用户访问 OpenVPN 客户端。   
  
- 由于 CVE 的性质，仍需要用户名和密码，因此很难确定修补的优先级。通过确保适当的分段、要求使用  
强用户名和密码  
以及减少具有写入身份验证的用户数量来降低风险。  
  
### Microsoft Defender XDR 检测  
  
适用于端点的 Microsoft Defender  
  
以下 Microsoft Defender for Endpoint 警报可能指示相关威胁活动：  
- 可疑的 OpenVPN 命名管道活动  
  
Microsoft Defender 漏洞管理  
  
Microsoft Defender 漏洞管理列出了可能受此威胁所利用的以下漏洞影响的设备：  
- CVE-2024-27459  
  
- CVE-2024-24974  
  
- CVE-2024-27903  
  
- CVE-2024-1305  
  
适用于 IoT 的 Microsoft Defender  
  
Microsoft Defender for IoT 针对与此威胁相关的以下漏洞、攻击和行为发出警报：  
- 怀疑存在恶意活动  
  
### 狩猎查询  
  
Microsoft Defender XDR  
  
Microsoft Defender XDR 客户可以运行以下查询来查找其网络中的相关活动：  
  
此查询识别从远程主机到 OpenVPN 的命名管道的连接：  
<table><tbody style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important; vertical-align: baseline !important;width: auto !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; "><tr style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important; vertical-align: baseline !important;width: auto !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; "><td style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;padding: 0px !important; vertical-align: baseline !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; " width="945" height="NaN"><section class="code-snippet__fix code-snippet__js"><ul class="code-snippet__line-index code-snippet__js"><li></li><li></li><li></li><li></li><li></li></ul><pre class="code-snippet__js" data-lang="bash"><code><span class="code-snippet_outer">DeviceEvents </span></code><code><span class="code-snippet_outer">| <span class="code-snippet__built_in">where</span> ActionType == <span class="code-snippet__string">&#34;NamedPipeEvent&#34;</span></span></code><code><span class="code-snippet_outer">| extend JsonAdditionalFields=parse_json(AdditionalFields)</span></code><code><span class="code-snippet_outer">| extend PipeName=JsonAdditionalFields[<span class="code-snippet__string">&#34;PipeName&#34;</span>]</span></code><code><span class="code-snippet_outer">| <span class="code-snippet__built_in">where</span> PipeName == <span class="code-snippet__string">&#34;\\Device\\NamedPipe\\openvpn\\service&#34;</span> and isnotempty( RemoteIP)</span></code></pre></section></td></tr></tbody></table>  
此查询识别从共享文件夹加载到 OpenVPN 进程中的图像：  
<table><tbody style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important; vertical-align: baseline !important;width: auto !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; "><tr style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important; vertical-align: baseline !important;width: auto !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; "><td style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;padding: 0px !important; vertical-align: baseline !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; " width="901" height="NaN"><section class="code-snippet__fix code-snippet__js"><ul class="code-snippet__line-index code-snippet__js"><li></li><li></li></ul><pre class="code-snippet__js" data-lang="bash"><code><span class="code-snippet_outer">DeviceImageLoadEvents</span></code><code><span class="code-snippet_outer">|<span class="code-snippet__built_in">where</span> InitiatingProcessFileName == <span class="code-snippet__string">&#34;openvpn.exe&#34;</span> and FolderPath startswith <span class="code-snippet__string">&#34;\\\\&#34;</span></span></code></pre></section></td></tr></tbody></table>  
此查询将连接到 OpenVPN 的命名管道的进程标识为服务器，但它不是openvpnserv.exe  
：  
<table><tbody style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important; vertical-align: baseline !important;width: auto !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; "><tr style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important; vertical-align: baseline !important;width: auto !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; "><td style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;padding: 0px !important; vertical-align: baseline !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; " width="1517" height="NaN"><section class="code-snippet__fix code-snippet__js"><ul class="code-snippet__line-index code-snippet__js"><li></li><li></li><li></li><li></li><li></li></ul><pre class="code-snippet__js" data-lang="bash"><code><span class="code-snippet_outer">DeviceEvents </span></code><code><span class="code-snippet_outer">| <span class="code-snippet__built_in">where</span> ActionType == <span class="code-snippet__string">&#34;NamedPipeEvent&#34;</span></span></code><code><span class="code-snippet_outer">| extend JsonAdditionalFields=parse_json(AdditionalFields)</span></code><code><span class="code-snippet_outer">| extend PipeName=JsonAdditionalFields[<span class="code-snippet__string">&#34;PipeName&#34;</span>], NamedPipeEnd=JsonAdditionalFields[<span class="code-snippet__string">&#34;NamedPipeEnd&#34;</span>]</span></code><code><span class="code-snippet_outer">|<span class="code-snippet__built_in">where</span> PipeName == <span class="code-snippet__string">&#34;\\Device\\NamedPipe\\openvpn\\service&#34;</span> and NamedPipeEnd == <span class="code-snippet__string">&#34;Server&#34;</span> and InitiatingProcessFileName != <span class="code-snippet__string">&#34;openvpnserv.exe&#34;</span></span></code></pre></section></td></tr></tbody></table>  
微软  
Sentinel  
  
Microsoft Sentinel 客户可以使用 TI Mapping 分析（一系列以“TI map”为前缀的分析）自动将本博客文章中提到的恶意域指标与其工作区中的数据进行匹配。如果当前未部署 TI Map 分析，客户可以从 Microsoft Sentinel Content Hub 安装威胁情报解决方案，以将分析规则部署在其 Sentinel 工作区中。有关 Content Hub 的更多详细信息，请访问此处：    
https://learn.microsoft.com/azure/sentinel/sentinel-solutions-deploy  
。  
  
存在 OpenVPN 漏洞的设备列表  
<table><tbody style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important; vertical-align: baseline !important;width: auto !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; "><tr style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important; vertical-align: baseline !important;width: auto !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; "><td style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;padding: 0px !important; vertical-align: baseline !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; " width="1600" height="NaN"><section class="code-snippet__fix code-snippet__js"><ul class="code-snippet__line-index code-snippet__js"><li></li><li></li><li></li><li></li><li></li><li></li><li></li><li></li></ul><pre class="code-snippet__js" data-lang="cs"><code><span class="code-snippet_outer">DeviceTvmSoftwareVulnerabilities</span></code><code><span class="code-snippet_outer">| <span class="code-snippet__keyword">where</span> OSPlatform contains <span class="code-snippet__string">&#34;Windows&#34;</span></span></code><code><span class="code-snippet_outer">| <span class="code-snippet__function"><span class="code-snippet__keyword">where</span> CveId <span class="code-snippet__title">in</span> (<span class="code-snippet__params"><span class="code-snippet__string">&#34;CVE-2024-27459&#34;</span>,<span class="code-snippet__string">&#34;CVE-2024-24974&#34;</span>,<span class="code-snippet__string">&#34;CVE-2024-27903&#34;</span>,<span class="code-snippet__string">&#34;CVE-2024-1305&#34;</span></span>)</span></span></code><code><span class="code-snippet_outer">| project DeviceId,DeviceName,OSPlatform,OSVersion,SoftwareVendor,SoftwareName,SoftwareVersion,</span></code><code><span class="code-snippet_outer">CveId,VulnerabilitySeverityLevel</span></code><code><span class="code-snippet_outer"><span class="code-snippet_outer">| <span class="code-snippet__keyword">join</span> kind</span>=inner ( DeviceTvmSoftwareVulnerabilitiesKB | project CveId, CvssScore,IsExploitAvailable,VulnerabilitySeverityLevel,PublishedDate,VulnerabilityDescription,AffectedSoftware ) <span class="code-snippet__keyword">on</span> CveId</span></code><code><span class="code-snippet_outer">| project DeviceId,DeviceName,OSPlatform,OSVersion,SoftwareVendor,SoftwareName,SoftwareVersion,</span></code><code><span class="code-snippet_outer">CveId,VulnerabilitySeverityLevel,CvssScore,IsExploitAvailable,PublishedDate,VulnerabilityDescription,AffectedSoftware</span></code></pre></section></td></tr></tbody></table>  
OpenVPN的命名管道创建活动  
<table><tbody style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important; vertical-align: baseline !important;width: auto !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; "><tr style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;height: auto !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important; vertical-align: baseline !important;width: auto !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; "><td style=" box-sizing: content-box !important;border-radius: 0px !important;background: none !important;border-width: 0px !important;border-style: initial !important;border-color: initial !important;inset: auto !important;float: none !important;line-height: 1.1em !important;outline: 0px !important;overflow: visible !important;padding: 0px !important; vertical-align: baseline !important;font-size: 1em !important;direction: ltr !important;box-shadow: none !important; " width="1473" height="NaN"><section class="code-snippet__fix code-snippet__js"><ul class="code-snippet__line-index code-snippet__js"><li></li><li></li><li></li><li></li><li></li><li></li><li></li><li></li><li></li><li></li></ul><pre class="code-snippet__js" data-lang="bash"><code><span class="code-snippet_outer"><span class="code-snippet__built_in">let</span> PipeNames = pack_array(<span class="code-snippet__string">&#39;\\openvpn/service&#39;</span>,<span class="code-snippet__string">&#39;\\openvpn/service_&#39;</span>,<span class="code-snippet__string">&#39;openvpn&#39;</span>,<span class="code-snippet__string">&#39;openvpn/service&#39;</span>,<span class="code-snippet__string">&#39;\\openvpn\\service_&#39;</span>);</span></code><code><span class="code-snippet_outer">DeviceEvents</span></code><code><span class="code-snippet_outer">| <span class="code-snippet__built_in">where</span> TimeGenerated &gt; ago(30d)</span></code><code><span class="code-snippet_outer">| <span class="code-snippet__built_in">where</span> ActionType == <span class="code-snippet__string">&#34;NamedPipeEvent&#34;</span></span></code><code><span class="code-snippet_outer">| <span class="code-snippet__built_in">where</span> ProcessCommandLine contains <span class="code-snippet__string">&#34;openvpn.exe&#34;</span> or InitiatingProcessCommandLine contains <span class="code-snippet__string">&#34;openvpn.exe&#34;</span></span></code><code><span class="code-snippet_outer">| extend Fields=parse_json(AdditionalFields)</span></code><code><span class="code-snippet_outer">| <span class="code-snippet__built_in">where</span> Fields.FileOperation == <span class="code-snippet__string">&#34;File created&#34;</span></span></code><code><span class="code-snippet_outer">| <span class="code-snippet__built_in">where</span> Fields.PipeName has_any (PipeNames)</span></code><code><span class="code-snippet_outer">| project TimeGenerated,ActionType,DeviceName,InitiatingProcessAccountDomain,InitiatingProcessAccountName,InitiatingProcessFolderPath,</span></code><code><span class="code-snippet_outer">InitiatingProcessCommandLine,ProcessCommandLine,Fields.FileOperation,Fields.PipeName</span></code></pre></section></td></tr></tbody></table>  
  
Vladimir Tokarev  
  
Microsoft 威胁情报社区  
### 参考  
- https://blackhat.com/us-24/briefings/schedule/#ovpnx–zero-days-leading-to-rce-lpe-and-kce-via-byovd-affecting-millions-of-openvpn-endpoints-across -全球-38900  
  
- https://enlyft.com/tech/products/openvpn  
  
- https://github.com/OpenVPN/openvpn/blob/v2.6.10/Changes.rst  
  
- https://github.com/OpenVPN/openvpn/blob/v2.5.10/Changes.rst  
  
- https://forums-new.openvpn.net/forum/announcements/69-release-openvpn-version-2-6-10  
  
- https://openvpn.net/community-downloads/  
  
- https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-27459  
  
- https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-24974  
  
- https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-27903  
  
- https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2024-1305  
  
- https://openvpn.net/as-docs/site-to-site-routing.html#site-to-site-routing  
  
- https://openvpn.net/community-resources/reference-manual-for-openvpn-2-4/  
  
- https://github.com/OpenVPN/openvpn/blob/master/include/openvpn-plugin.h.in  
  
- https://www.lifewire.com/net-use-command-2618096  
  
- https://wikipedia.org/wiki/Stack_buffer_overflow#Stack_canaries  
  
- https://community.openvpn.net/openvpn/wiki/Downloads  
  
- https://www.cisa.gov/secure-our-world/use-strong-passwords  
  
