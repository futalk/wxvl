#  首次利用OpenAI o3模型发现Linux内核零日漏洞 | Palo Alto公布千亿营收冲刺计划，网络安全迈向平台化寡头竞争   
e安在线  e安在线   2025-05-27 02:29  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1Y08O57sHWiahTldalExhOyzXNMO6kcO7ULmiclhSZfg8zVMLHEMUGBu3lBjFbjib8vsYDZzplofMSC7epkHHWpibw/640?wx_fmt=png&from=appmsg "")  
# 首次利用OpenAI o3模型发现Linux内核零日漏洞  
  
AI成功找到Linux安全漏洞，还是**内核级别的零日漏洞**  
。  
  
刚刚，OpenAI总裁转发了独立研究员  
Seen Heelan  
的实验成果：用o3模型找到了Linux内核SMB实现中的一个远程零日漏洞。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWgVNceuZvO3oib6GbYo1AV5ia6WxcUjgFP07Mtv1zoSByG6IicD7S8LXdFSy9cPXdSMVIxfCicvia92XWA/640?wx_fmt=jpeg "")  
  
更让人惊讶的是，整个过程中**没有用到任何复杂的工具**  
——没有脚手架、没有智能体框架、没有工具调用，仅仅是o3 API本身。  
  
这个漏洞被编号为CVE-2025-37899，是SMB”注销”命令处理程序中的一个释放后使用（use-after-free）漏洞。  
  
据作者透露，这是首次公开讨论的由大模型发现的此类漏洞。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWgVNceuZvO3oib6GbYo1AV5iaLbLibIERGB87UO7iaEQjnLdMiaFbaoFdQBOUzfm9AU4KYFwiapLZzw1oYQ/640?wx_fmt=jpeg "")  
  
有网友看过发现过程后感叹，原以为会有很疯狂的实验设置，但其实只是把一堆代码缝到一起，让o3检查100次。  
> 希望其他白帽黑客已经开始像这样检查其他关键操作系统了。  
  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWgVNceuZvO3oib6GbYo1AV5iacNtBcY8wpZIe7Xibm4gmdyeI1NQs7ZW6iaxWRiancV0lBtqzPib5OaUWzw/640?wx_fmt=jpeg "")  
  
OpenAI首席研究官Mark Chen表示：像o3这样的推理模型正开始助力深度技术工作和有意义的科学发现。接下来一年，类似这样的成果将会越来越普遍：  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWgVNceuZvO3oib6GbYo1AV5iaRia4UAIlory3uykXq8FhaibBaxf4s3cU2ALemxsvLN6tOdMEicEAc8zEA/640?wx_fmt=jpeg "")  
## AI不仅找到漏洞，还能辅助修复  
  
Sean Heelan是一位独立研究员，专注于基于大模型的漏洞研究和漏洞利用自动化生成。  
  
他原本在手动检查Linux内核的KSMBD（内核态SMB3协议实现）漏洞，想要暂时远离大模型相关的工具开发。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWgVNceuZvO3oib6GbYo1AV5iauKcWgev0b46yBu9SExsb30G2fMplrA7sTcPhN3ZcvficH65ztWAT7Ew/640?wx_fmt=jpeg "")  
  
但o3发布后，他忍不住想测试一下：”既然我手里已经有这些漏洞，不如看看o3能不能找到它们？”  
  
他先用一个自己手动发现的漏洞CVE-2025-37778作为测试。这是Kerberos认证路径中的一个释放后使用漏洞，是测试大模型能力很好的基准，因为：  
- 它是Linux 内核远程攻击面的一部分  
  
- 它并不简单，发现他需要理解如何触发特定的会话状态、认证函数的多条执行路径、跨函数的对象生命周期管理  
  
- 但它也并非极其复杂，作者本人可以在10分钟内带着同时了解涉及到的约3300行代码  
  
接下来开始实验之前，还需要搞清楚一个问题，该给大模型展示哪些代码？  
  
理想情况当然是大模型读取整个代码库并输出结果，但实践中上由于上下文窗口的限制还无法事先。  
  
最终Heelan为 LLM 提供了“会话设置”命令处理程序的代码，包括它调用的所有函数的代码等等，直到调用深度达到3（这是包含推理漏洞所需代码所需的深度），还包含了命令处理器、连接建立/拆除、请求分发等完整上下文。  
  
提示词中告诉o3专门寻找use-after-free漏洞，提供KSMBD架构说明，强调要避免误报，每个实验运行100次，统计成功率。  
  
相关代码和提示词已开源在GitHub上。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWgVNceuZvO3oib6GbYo1AV5iaPYicbhn7IK2cicX9Qznv4goI29Ip1NbLPt8fGxdYYz01yrXX49I2qSBQ/640?wx_fmt=jpeg "")  
  
Heelan还按同样流程测试了Claude3.7和Claude 3.5作为对比，结果o3的表现让人眼前一亮：  
- OpenAI o3: 100次运行中成功找到8次漏洞  
  
- Claude 3.7: 100次中找到3次  
  
- Claude 3.5: 100次中一次都没找到  
  
此外有意思的的是，o3和Claude的输出风格完全不同。  
  
o3生成的像人类写的漏洞报告，结构清晰、重点突出  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWgVNceuZvO3oib6GbYo1AV5iavn9uVXMp5zamA3MicXzC0v3tjjfLD5FiatgjLELibsiccXsJgniaxzo3tuQ/640?wx_fmt=jpeg "")  
  
Claude更像思维流或工作日志，细节丰富但略显冗长。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWgVNceuZvO3oib6GbYo1AV5ian8PbTOI33SMgKJn9sZARm4gp423PpPf2m4qA84yzX6eI88Q1QEsMzg/640?wx_fmt=jpeg "")  
  
接下来发生的事情更精彩了。  
  
Heelan想测试o3在更大代码量下的表现，于是给它提供了所有SMB命令处理程序的代码，约**12000行**  
代码。  
  
虽然找到原始漏洞的成功率降到了1%，性能明显下降。但在运行结果中，o3报告了一个全新的漏洞。  
  
Heelan自己之前也不知道这个漏洞，它同样是由于sess->user的释放造成的，但这次是在会话注销处理程序中。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWgVNceuZvO3oib6GbYo1AV5iaxW0LWA2W0e6LY6h7qf7f1icsyHWdIbRycszsBA1N3HzicEdcSjRE1bOA/640?wx_fmt=jpeg "")  
  
o3自己对这个漏洞的解释如下。  
> 当一个ksmbd工作线程还在执行使用sess->user的请求时，另一个处理同一会话SMB2 LOGOFF的线程释放了该结构。没有同步机制保护该指针，所以第一个线程会解引用已释放的内存——这是一个经典的释放后使用漏洞，会导致内核内存损坏和任意代码执行。  
  
  
更有意思的是，研究员发现o3不仅能找到漏洞，某些情况下修复方案比人类更完善。  
  
对于之前的Kerberos认证漏洞，Heelan最初提出的修复是简单地将指针置空：  
```
if (sess->state == SMB2_SESSION_VALID) {
    ksmbd_free_user(sess->user);
    sess->user = NULL;  // 添加这行
}
```  
  
但o3的报告中有指出这个修复不够充分，因为SMB协议允许两个不同的连接”绑定”到同一个会话，在释放和置空之间的短暂窗口期，另一个线程仍可能访问这个指针。  
  
最终Heelan承认，如果使用o3来找和修复原始漏洞，“理论上”会比自己完成更好。  
  
之所以加上“理论上”的限定，是因为现在AI误报的比例有点高，人类很难认真仔细地查看o3的每份报告。  
  
不过他也认为随着技术的发展，这个比例只会越来越低。  
  
Heelan在报告结尾感慨道：  
> 大模型在程序分析技术的能力空间中，处于一个比我们见过的任何东西都更接近人类的位置。考虑到创造力、灵活性和通用性，LLM更像是人类代码审计员，而不是符号执行、抽象解释或模糊测试。  
  
  
他特别强调，如果你从事安全研究工作，现在应该开始密切关注了：  
- 专家级研究员不会被取代，反而会变得更高效  
  
- 对于10000行以内的代码问题，o3有相当大的概率能解决或帮助解决  
  
- 虽然仍有约1:50的信噪比问题，但这已经值得投入时间和精力  
  
不过也有人看到了其中的风险：  
  
如果坏人利用AI的能力找到类似的漏洞并攻击系统又如何呢？  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWgVNceuZvO3oib6GbYo1AV5iaKicWYP1ficmTzOBJkc4qfqEp8WNUjbuAqfyx3Z4BujVaLq4eichLtTuag/640?wx_fmt=jpeg "")  
#   
# Palo Alto公布千亿营收冲刺计划，网络安全迈向平台化寡头竞争  
  
5月26日消息，华尔街分析师表示，Palo Alto Networks推动客户在其广泛的网络安全平台上整合工具的战略正在持续奏效。  
  
在审阅该公司上周发布的最新季度财报后，分析师们向投资者表示，他们对Palo Alto Networks的“平台化”战略信心不断增强。  
  
  
**平台化产品客户高速增长**  
  
  
这家网络安全巨头披露，2025财年第三季度（截至4月30日），其平台化客户总数约为1250家。美国投行TD Cowen的董事总经理兼高级分析师Shaul Eyal发布投资报告指出，该季度新增约90笔平台化交易，高于上一季度的75笔。  
  
Eyal表示，这种“平台化的加速”是Palo Alto Networks一年多前启动的新增长战略所展现出的多个积极信号之一。2024年2月，公司首席执行官Nikesh Arora宣布对整体战略进行全面调整，旨在推动客户更快速地实现平台整合。  
  
Eyal在报告中写道，在一定程度上得益于平台化的推进，可以明显看出，Palo Alto Networks朝着到2030财年实现150亿美元（约合人民币1075亿元）年经常性收入（ARR）目标的努力“依然坚定不移地沿着正确方向前进”。  
  
Wedbush Securities董事总经理兼高级股票研究分析师Daniel Ives在投资者报告中指出，从20日发布的季度财报来看，Palo Alto Networks正在为其平台化战略迈入2026财年奠定基础，而AI正处于这一战略的核心。  
  
Ives写道：“我们认为，2025年将是该公司的过渡之年，因为平台化交易与云端渗透正带来强劲的交易动能和更稳定的销售管道，同时也让公司成为AI支出带来的主要受益者之一。”  
  
  
**SIEM市场正迎来颠覆性替代机遇**  
  
  
在公司20日的季度电话会议上，Arora提到本季度的一些关键平台化成果，包括与一家全球咨询公司达成的一项价值超过9000万美元的交易。他表示，该交易使用Palo Alto Networks的Cortex XSIAM替代了一家“传统”的安全信息与事件管理（SIEM）提供商。  
  
Arora指出，这项平台化交易“通过整合4款产品降低了成本”，同时也帮助客户“显著提升”了对安全事件的平均响应速度。  
  
Cortex XSIAM是Palo Alto Networks推出的一款可替代传统SIEM产品的解决方案。根据Arora的说法，该产品在本季度末的ARR同比增长高达200%，成为本季度的另一亮点。  
  
他表示，该产品的增长速度“几乎是我们最接近的下一代SIEM竞争对手的两倍”，而目前XSIAM的总预订金额已接近10亿美元。  
  
展望未来，Arora预测，传统SIEM市场将在几年内被“新时代玩家”所取代，这些新兴企业通过AI和现代架构赋能安全运营团队。  
  
他说：“我认为，在未来3到5年内，传统SIEM将被彻底替代。”  
  
  
**关税未对公司造成影响**  
  
  
Palo Alto Networks首席财务官Dipak Golechha表示，特朗普政府的关税政策至今尚未对Palo Alto Networks造成影响，该公司是全球主要的硬件防火墙制造商之一。  
  
他说，公司正将生产转移至德克萨斯州的一家合同制造工厂，并通过“在美国境内组装全部硬件”来实现与竞争对手的差异化。  
  
Golechha表示：“因此，关税对我们业务的影响可以说是微乎其微。”  
  
  
  
  
声明：除发布的文章无法追溯到作者并获得授权外，我们均会注明作者和文章来源。如涉及版权问题请及时联系我们，我们会在第一时间删改，谢谢！文章来源：  
安全内参、  
参考资料：crn.com  
  
  
  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWiaM9uv5Q89hYMT8zuKQtQYuvSPy0HyyLwRShZOMcoGgoBy6qiatgDhW3UhCXGVXiaEbS8ANmZwViaMAw/640?wx_fmt=jpeg&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=wxpic "")  
  
  
