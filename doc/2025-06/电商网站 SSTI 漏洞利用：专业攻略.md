#  电商网站 SSTI 漏洞利用：专业攻略  
红云谈安全  红云谈安全   2025-06-07 07:46  
  
**免责声明**  
  
由于传播、利用本公众号红云谈安全所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号红云谈安全及作者不为**此**  
承担任何责任，一旦造成后果请自行承担！  
如有侵权烦请告知，我们会立即删除并致歉。谢谢！请在授权的站点测试，遵守网络安全法！  
**仅供**  
**学习使用，****如若非法他用，与平台和本文作者无关，需自行负责！**  
# 步骤 1：初步探索和帐户创建  
  
为了开始评估，我在网站上创建了一个帐户，并导航到个人信息部分。此部分有多个输入字段，包括**称谓**、**名字**、**姓氏**、**出生日期**和**电子邮件地址**。我主要关注的是**姓氏**字段，因为它似乎接受了未经过滤的用户输入。  
# 步骤2：测试SSTI漏洞  
  
**由于怀疑存在潜在的 SSTI 漏洞，我向姓氏**字段注入了以下有效载荷：  
```
```  
  
  
如果此有效载荷由服务器的模板引擎执行，它将执行一个简单的算术运算并显示结果。保存更改后，我在网页上观察了结果。  
# 步骤3：验证SSTI执行  
  
保存信息并刷新页面后，发现{{{{7*7}}}}payload已经被服务器处理并渲染为{{7*7}}。刷新页面后，计算结果就显示49在页面上方角落，靠近**Test**标签的位置。  
  
# 截图证明：  
  
这证实了服务器端模板引擎在没有经过充分清理的情况下评估用户输入，从而允许 SSTI。  
  
当计算结果显示出来时，显示49在页面顶角的**Test**标签旁边。  
  
# 步骤 4：分析和后续步骤  
# 了解风险：  
  
SSTI 漏洞构成重大安全风险。一旦被利用，可能导致：  
- **数据暴露**：攻击者访问敏感的服务器端数据。  
- **远程代码执行 (RCE)**：可能在服务器上执行任意命令。  
# 展望未来：  
  
确认 SSTI 后，我的下一步包括：  
1. **探索服务器配置**：  
- jinja2  
- {{ config.items() }}  
1. 该有效载荷可以揭示配置细节和环境变量。  
1. **远程代码执行测试**：为了评估严重性，我考虑使用如下有效载荷：  
- jinja2  
- {{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('id').read() }}  
1. 如果成功，此有效载荷将执行命令以在基于 Unix 的系统上显示用户信息。  
# 结论  
  
在实时电商网站上发现 SSTI 漏洞，凸显了强大的输入过滤和安全编码实践的必要性。开发人员应该使用默认转义用户输入的模板引擎，或实施适当的验证和编码机制来降低此类风险。  
  
  
往期回顾：  
  
[逻辑-绕过限制访问敏感用户文件并共享它们](https://mp.weixin.qq.com/s?__biz=MzI0MTUwMjQ5Nw==&mid=2247488915&idx=1&sn=db7b321c1f94564e83ea0cc5e15a316c&scene=21#wechat_redirect)  
  
  
[通过有趣的逻辑问题 $$$$ 接管帐户](https://mp.weixin.qq.com/s?__biz=MzI0MTUwMjQ5Nw==&mid=2247488914&idx=1&sn=533803a9b489bcfd7c7ec3a0fc0cc78e&scene=21#wechat_redirect)  
  
  
[图片上传识别功能还能这样被利用？](https://mp.weixin.qq.com/s?__biz=MzI0MTUwMjQ5Nw==&mid=2247488913&idx=1&sn=89eb0c85d8e8dbe5ba63f6b6d27e9757&scene=21#wechat_redirect)  
  
  
[逻辑漏洞-使用 SSO 登录进行帐户接管](https://mp.weixin.qq.com/s?__biz=MzI0MTUwMjQ5Nw==&mid=2247488861&idx=1&sn=36092d38e428c6737bfc7a2291f944c2&scene=21#wechat_redirect)  
  
  
  
  
