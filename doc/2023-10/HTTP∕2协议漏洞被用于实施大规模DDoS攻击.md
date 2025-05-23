#  HTTP/2协议漏洞被用于实施大规模DDoS攻击   
 黑白之道   2023-10-14 09:25  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/3xxicXNlTXLicwgPqvK8QgwnCr09iaSllrsXJLMkThiaHibEntZKkJiaicEd4ibWQxyn3gtAWbyGqtHVb0qqsHFC9jW3oQ/640?wx_fmt=gif "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/INYsicz2qhvYQ0t4NaTFUT0ia9Jia16rrMEibbywns8S2VsGTIIOeYuRNtictjetpsPrKf3I4rdR3j9IPib7aiaggSgmw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
  
**近日，Google、AWS等主要云服务商披露HTTP/2协议中的一个隐藏很久的漏洞被广泛用于实施大规模DDoS攻击。**  
  
  
  
  
  
在过去的两个月中，攻击者一直在滥用HTTP/2网络通信协议的一个“功能”，放大针对Web应用服务器、负载均衡器和Web代理的DDoS攻击流量。Google、AWS、Cloudflare以及其他主要的云基础设施提供商，以及Web服务器供应商一直在私密群组中研究其缓解策略和补丁，直到本周三该漏洞才被正式披露。  
  
  
这种被命名为HTTP/2 Rapid Reset DDoS的拒绝服务攻击利用了HTTP/2协议的流多路复用重置功能的安全漏洞，该功能允许在同一TCP传输连接上并行发送多个HTTP请求，而且允许客户端单方面重置这些流。  
  
  
该漏洞分配的编号为CVE-2023-44487，企业应该尽快检查Web服务器和负载均衡器提供商是否有可用的补丁或缓解建议。  
  
  
**流多路复用使DDoS攻击更高效**  
  
  
  
  
在旧的HTTP/1版本中，多个请求可以在单个TCP连接上以串行方式发送，服务器按照接收顺序处理并响应请求。  
  
  
HTTP/2引入了一种新的功能——流多路复用，即多个被称为流的请求，由HEADERS及DATA帧组成，可以并发且无序地在TCP连接上发送。这是因为每个流都有一个与之关联的ID，因此服务器知道帧是属于哪个流以及如何响应。  
  
  
流多路复用可以显著提高TCP连接的效率并加快页面加载时间。因为现代网页中往往包含大量资源、第三方脚本以及从不同位置加载的图像。通过HTTP/2访问此类页面的浏览器将立即开始并行加载这些资源，并优先加载用户视图中的资源。  
  
  
不幸的是，HTTP/2对合法客户端更高效的功能也可用于使DDoS攻击更高效。谷歌工程师在博客文章中透露：“自2021年底以来，无论是从攻击数量还是峰值请求率，我们在受Cloud Armor保护的Google第一方服务和Google Cloud项目中观察到的大多数第7层DDoS攻击都基于HTTP/2（的漏洞）。”  
  
  
**通过快速重置绕过并发流限制**  
  
  
  
  
由于服务器需要消耗CPU和内存资源来处理每个帧和流，并发流功能如果被滥用可快速耗尽服务器资源，从而导致拒绝服务的情况，因此，协议开发人员添加了一个名为SETTINGS_MAX_CONCURRENT_STREAMS的设置，服务器将通过一个SETTINGS帧在第一次连接时将其传达给端点客户端。  
  
  
默认情况下，此设置的值是无限的，但协议设计者建议它不应低于100，以保持有效的并行性能。因此，在实践中，许多客户端不等待SETTINGS帧，只是假设最小限制为100，并从一开始就发送100个帧。  
  
  
问题出现在另一个名为RST_STREAM（"快速重置流"）的功能上。这是客户端发送到服务器的一种帧类型，用于告诉服务器停止响应先前的请求，防止浪费带宽。  
  
  
这导致一个问题：通过发送RST_STREAM帧，目标流不再计入最大并发流限制，因此在为先前的流发送重置请求后，客户端可以立即打开一个新流。这意味着即使并发流的限制为100，客户端也可以在同一TCP连接上快速连续打开和重置数百个流。  
  
  
**发现多种HTTP/2DDoS攻击变体**  
  
  
  
  
利用HTTP/2流重置漏洞的DDoS攻击仍在继续，Google已经监测到多种变体，其中一些可能是为了应对缓解措施。例如，一种攻击变体会分批打开和重置流，在发送RST_STREAM帧之前等待，然后打开另一批。这可能是为了挫败那些通过检测同一TCP连接上出现大量RST_STREAM帧来关闭连接的缓解措施。  
  
  
另一种变体根本不使用RST_STREAM重置功能，而是尝试打开尽可能多的并发流，忽略服务器通告的限制。HTTP/2标准规定，在这种情况下，超过限制的流应由服务器无效，但不应取消完整的TCP连接。因此，这种攻击变体允许攻击者始终保持请求管道满载。  
  
  
谷歌工程师表示：“我们并不认为简单地阻止单个请求就能有效缓解此类攻击，相反，当检测到滥用行为时，需要关闭整个TCP连接。”  
  
  
**缓解和补丁措施**  
  
  
  
  
HTTP/2 DDoS攻击的缓解策略并不简单，因为RST_STREAM流重置有合法的用途，每位服务器用户都需要根据连接统计信息和业务逻辑来确定何时发生滥用以及采取何种力度的响应措施。例如，如果一个TCP连接有超过100个请求，并且客户端取消了其中超过50%的请求，该连接可能被视为滥用。缓解措施的力度范围可以从发送强制的GOAWAY帧到立即关闭TCP连接。  
  
  
Nginx（一种流行的反向代理和负载均衡器）的开发人员也提供了基于服务器内置功能（例如keepalive_requests、limit_conn和limit_req）的缓解措施。在接下来的几天里，他们还将发布一个补丁，以进一步缓解此类攻击的影响。  
  
  
微软、AWS、F5和其他基础设施公司以及Web服务器或负载均衡软件开发商都已经发布了缓解措施或补丁。用户可以关注CVE漏洞跟踪器中的官方条目，获取有关供应商更新响应的链接。  
  
  
> **文章来源：GoUpSec**  
  
  
  
黑白之道发布、转载的文章中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用，任何人不得将其用于非法用途及盈利等目的，否则后果自行承担！  
  
如侵权请私聊我们删文  
  
  
**END**  
  
