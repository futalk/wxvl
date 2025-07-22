#  Nvidia工具包严重漏洞导致AI云服务易受黑客攻击  
会杀毒的单反狗  军哥网络安全读报   2025-07-22 01:00  
  
**导****读**  
  
  
  
云安全公司 Wiz 的研究人员发现了 Nvidia 的 Container Toolkit 中的一个严重漏洞，并警告说它可能对托管 AI 云服务构成严重威胁。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/AnRWZJZfVaFnHHLLDG3Y6CahZlmucfaxysfybGgBuibAwfOWEScOg4k9Wib0Tq1AhMvTEXISU0nB1kmv0NWALPSQ/640?wx_fmt=png&from=appmsg "")  
  
  
该漏洞被命名为NVIDIAScape，官方编号为 CVE-2025-23266。今年早些时候， Wiz 研究人员在Pwn2Own 柏林黑客大赛上演示了该漏洞，并因此获得了 3 万美元的奖金。  
  
  
Nvidia 在上周发布的安全公告中告知客户该漏洞以及可用的补丁。该供应商表示，该漏洞（CVSS 评分为 9.0）可能导致权限提升、信息泄露、数据篡改和 DoS 攻击。  
  
  
Nvidia Container Toolkit 专为构建和运行 GPU 加速容器而设计，Wiz 表示它经常被主要云提供商用于托管 AI 服务。  
  
  
据 Wiz 称，CVE-2025-23266 是由与处理开放容器计划 (OCI)钩子相关的配置错误引起的，该钩子使用户能够在容器生命周期的指定点定义和执行操作。  
  
  
最大的风险是托管 AI 云服务允许用户在共享 GPU 基础架构上运行自己的容器。  
  
  
恶意容器可利用 NVIDIAScape 漏洞绕过隔离并获得主机的完全 root 访问权限。攻击者可能能够从主机窃取或操纵使用相同硬件的所有其他客户的敏感数据和专有 AI 模型。  
  
  
Wiz 分享了该漏洞的技术细节，并展示了如何利用恶意负载和放置在容器镜像内的三行 Docker 文件来利用该漏洞。  
  
  
“这项研究再次强调，容器并非一道强大的安全屏障，不应被依赖为唯一的隔离手段。”Wiz 警告称：“在设计应用程序时，尤其是在多租户环境中，应该始终‘假设存在漏洞’，并至少实现一道强大的隔离屏障，例如虚拟化。”  
  
  
漏洞技术分析：  
  
https://www.wiz.io/blog/nvidia-ai-vulnerability-cve-2025-23266-nvidiascape  
  
  
Nvidia 官方安全公告：  
  
https://nvidia.custhelp.com/app/answers/detail/a_id/5659  
  
  
新闻链接：  
  
https://www.securityweek.com/critical-nvidia-toolkit-flaw-exposes-ai-cloud-services-to-hacking/  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/AnRWZJZfVaGC3gsJClsh4Fia0icylyBEnBywibdbkrLLzmpibfdnf5wNYzEUq2GpzfedMKUjlLJQ4uwxAFWLzHhPFQ/640?wx_fmt=jpeg "")  
  
扫码关注  
  
军哥网络安全读报  
  
**讲述普通人能听懂的安全故事**  
  
  
