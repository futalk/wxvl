#  【技术原创】GoAnywhere Managed File Transfer漏洞调试环境搭建   
原创 3gstudent  嘶吼专业版   2023-07-11 12:01  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/wpkib3J60o297rwgIksvLibPOwR24tqI8dGRUah80YoBLjTBJgws2n0ibdvfvv3CCm0MIOHTAgKicmOB4UHUJ1hH5g/640?wx_fmt=gif "")  
# 0x00 前言  
  
本文记录从零开始搭建GoAnywhere Managed File Transfer漏洞调试环境的细节。  
# 0x01 简介  
  
本文将要介绍以下内容：  
  
GoAnywhere Managed File Transfer安装。  
  
GoAnywhere Managed File Transfer漏洞调试环境配置。  
  
数据库操作。  
# 0x02 GoAnywhere Managed File Transfer安装  
  
参考资料：https://static.fortra.com/goanywhere/pdfs/guides/ga6_8_6_installation_guide.pdf  
  
下载地址：https://www.goanywhere.com/products/goanywhere-free/download  
  
需要注册账号获得license。  
  
GoAnywhere Managed File Transfer可以分别安装在Windows和Linux操作系统。  
  
Windows系统下默认的Web路径：C:\Program Files\HelpSystems\GoAnywhere\tomcat\webapps\ROOT。  
  
Linux系统下默认的Web路径：/usr/local/HelpSystems/GoAnywhere/tomcat/webapps/ROOT。  
  
**1.开启远程调试功能**  
  
通过开启Tomcat调试功能来实现，开启Tomcat调试功能的方法如下：  
  
切换至bin目录。  
  
执行命令:catalina jpda start。  
  
Tomcat调试功能开启后默认监听本地8000端口。  
  
对于GoAnywhere Managed File Transfer，开启调试功能的方法如下：  
  
(1)Windows下调试  
  
修改文件C:\Program Files\HelpSystems\GoAnywhere\tomcat\bin\GoAnywhere.exe的文件属性。  
  
双击文件C:\Program Files\HelpSystems\GoAnywhere\tomcat\bin\GoAnywhere.exe，切换到Java标签页，在Java Optinos添加：-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8090，如下图：  
  
重启服务GoAnywhere。  
  
(2)Linux调试  
  
修改文件：/opt/HelpSystems/GoAnywhere/tomcat/bin/start_tomcat.sh，将exec "$PRGDIR"/"$EXECUTABLE" start "$@"修改为exec "$PRGDIR"/"$EXECUTABLE" jpda start "$@"  
  
修改文件: /opt/HelpSystems/GoAnywhere/tomcat/bin/goanywhere_catalina.sh，将JPDA_ADDRESS="localhost:8000"修改为JPDA_ADDRESS="*:8090"  
  
注：  
  
Tomcat默认的调试端口8000同GoAnywhere Managed File Transfer的Web端口冲突，所以这里选择修改Tomcat默认的调试端口为8090  
  
打开防火墙允许外部访问8090端口：iptables -I INPUT -p tcp --dport 8090 -j ACCEPT  
  
启动GoAnywhere进程：/opt/HelpSystems/GoAnywhere/goanywhere.sh start  
# 0x03 数据库操作  
  
GoAnywhere Managed File Transfer使用Apache Derby数据库  
  
Windows下默认数据库存储位置为：C:\Program Files\HelpSystems\GoAnywhere\userdata\database\goanywhere  
  
Linux下默认数据库存储位置为：/opt/HelpSystems/GoAnywhere/userdata/database/goanywhere/  
  
数据库操作的实现细节可从lib文件夹下的ga_classes.jar获得  
  
从中我们可以得到Web用户口令加密的实现细节，对应位置：C:\Program Files\HelpSystems\GoAnywhere\lib\ga_classes.jar!\com\linoma\ga\ui\admin\action\user\ChangeUserPasswordAction.class  
  
提取出的Java实现代码如下：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2ibmoRiaHwFwoaEZezVG2xTXG8KtPKRAewibjpGEeLkXUYC0nNSY9QRfRN2yZCsMtcnRvxic9J621sIWQ/640?wx_fmt=png "")  
  
**1.读取Derby数据库**  
  
(1)命令行实现  
  
使用Apache Derby，下载地址：https://archive.apache.org/dist/db/derby/db-derby-10.14.2.0/db-derby-10.14.2.0-bin.zip  
  
运行bin目录下的ij.bat  
  
连接数据库：connect 'jdbc:derby:C:\Program Files\HelpSystems\GoAnywhere\userdata\database\goanywhere;';  
  
查询用户配置：SELECT * FROM DPA_USER;  
  
(2)界面化实现  
  
使用DBSchema，下载地址：https://dbschema.com/download.html  
  
启动DBSchema后，选择连接Derby数据库，JDBC Driver选择derbytools.jar org.apache.derby.jdbc.EmbeddedDriver，Folder选择C:\Program Files\HelpSystems\GoAnywhere\userdata\database\goanywhere  
  
查询用户数据表，如下图：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2ibmoRiaHwFwoaEZezVG2xTXGEpKCTEG8ibZOYMWicjFVavkRxGpHaOR2YtaswqqahtYQxqADc0ibAPMtA/640?wx_fmt=png "")  
  
可以看到默认用户有以下三个：  
  
Administrator，未启用。  
  
root，未启用。  
  
admin，默认用户。  
  
**2.修改数据库**  
  
GoAnywhere Managed File Transfer的Derby数据库使用了内嵌模式，其他应用程序不可访问，所以有以下两种修改数据的方法：  
  
(1)GoAnywhere Managed File Transfer处于运行状态。  
  
可以通过写入jsp文件实现数据库的修改。  
  
(2)GoAnywhere Managed File Transfer处于关闭状态。  
  
可以选择Apache Derby或DBSchema打开数据库文件夹，直接进行修改。  
  
修改数据库的命令示例：  
  
启用root用户：UPDATE APP.DPA_USER SET ENABLED='1' WHERE USER_NAME='root';  
  
设置root用户口令：UPDATE APP.DPA_USER SET USER_PASS='$5$mpoe6zI4B6+LHRMdbFKr8g==$RnAILbYe9KDauKE3wXTFVvlXQNZeM4Z2c7x1aEtME/U=' WHERE USER_NAME='root';  
# 0x04 小结  
  
在我们搭建好GoAnywhere Managed File Transfer漏洞调试环境后，接下来就可以着手对漏洞进行学习。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2ibmoRiaHwFwoaEZezVG2xTXGe7IncicOkAerS3WsoNmuucaJM8D7yLrEU1bUzwO6TdxbStQgqtupO2w/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2ibmoRiaHwFwoaEZezVG2xTXGL7N5no1eLahjxtt59oLSPdcFiapqgG9PYQibibHBKYOoLt5N4OlP7zURA/640?wx_fmt=png "")  
  
  
