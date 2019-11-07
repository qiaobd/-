# Windows日志分析工具

##  **查看系统日志方法：** 

在**“开始”**菜单上，依次指向**“所有程序”**、**“管理工具”**，然后单击**“事件查看器”**

按 "**Window+R**"，输入 ”**eventvwr.msc**“ 也可以直接进入“**事件查看器**”

对于Windows事件日志分析，不同的EVENT ID代表了不同的意义，摘录一些常见的安全事件的说明：

| 事件ID | 说明                             |
| :----- | :------------------------------- |
| 4624   | 登录成功                         |
| 4625   | 登录失败                         |
| 4634   | 注销成功                         |
| 4647   | 用户启动的注销                   |
| 4672   | 使用超级用户（如管理员）进行登录 |
| 4720   | 创建用户                         |

每个成功登录的事件都会标记一个登录类型，不同登录类型代表不同的方式：

| 登录类型 | 描述                            | 说明                                             |
| :------- | :------------------------------ | :----------------------------------------------- |
| 2        | 交互式登录（Interactive）       | 用户在本地进行登录。                             |
| 3        | 网络（Network）                 | 最常见的情况就是连接到共享文件夹或共享打印机时。 |
| 4        | 批处理（Batch）                 | 通常表明某计划任务启动。                         |
| 5        | 服务（Service）                 | 每种服务都被配置在某个特定的用户账号下运行。     |
| 7        | 解锁（Unlock）                  | 屏保解锁。                                       |
| 8        | 网络明文（NetworkCleartext）    | 登录的密码在网络上是通过明文传输的，如FTP。      |
| 9        | 新凭证（NewCredentials）        | 使用带/Netonly参数的RUNAS命令运行一个程序。      |
| 10       | 远程交互，（RemoteInteractive） | 通过终端服务、远程桌面或远程协助访问计算机。     |
| 11       | 缓存交互（CachedInteractive）   | 以一个域用户登录而又没有域控制器可用             |

在**“开始”**菜单上，依次指向**“所有程序”**、**“管理工具”**，然后单击**“事件查看器”**；

在事件查看器中，单击**“安全”**，查看安全日志；

在安全日志右侧操作中，点击**“筛选当前日志”**，输入事件ID进行筛选。

4624  --登录成功   

4625  --登录失败  

4634 -- 注销成功

4647 -- 用户启动的注销   

4672 -- 使用超级用户（如管理员）进行登录

### 日志分析工具

#### Log Parser

 Log Parser 2.2下载地址：https://www.microsoft.com/en-us/download/details.aspx?id=24659 

**基本查询结构** 

```
Logparser.exe –i:EVT –o:DATAGRID "SELECT * FROM c:\xx.evtx"
```

1、查询登录成功的事件

```
登录成功的所有事件LogParser.exe -i:EVT –o:DATAGRID  "SELECT *  FROM c:\Security.evtx where EventID=4624"指定登录时间范围的事件：LogParser.exe -i:EVT –o:DATAGRID  "SELECT *  FROM c:\Security.evtx where TimeGenerated>'2018-06-19 23:32:11' and TimeGenerated<'2018-06-20 23:34:00' and EventID=4624"提取登录成功的用户名和IP：LogParser.exe -i:EVT  –o:DATAGRID  "SELECT EXTRACT_TOKEN(Message,13,' ') as EventType,TimeGenerated as LoginTime,EXTRACT_TOKEN(Strings,5,'|') as Username,EXTRACT_TOKEN(Message,38,' ') as Loginip FROM c:\Security.evtx where EventID=4624"
```

 2、查询登录失败的事件

```
登录失败的所有事件：LogParser.exe -i:EVT –o:DATAGRID  "SELECT *  FROM c:\Security.evtx where EventID=4625"提取登录失败用户名进行聚合统计：LogParser.exe  -i:EVT "SELECT  EXTRACT_TOKEN(Message,13,' ')  as EventType,EXTRACT_TOKEN(Message,19,' ') as user,count(EXTRACT_TOKEN(Message,19,' ')) as Times,EXTRACT_TOKEN(Message,39,' ') as Loginip FROM c:\Security.evtx where EventID=4625 GROUP BY Message"
```

3、系统历史开关机记录：

```
LogParser.exe -i:EVT –o:DATAGRID  "SELECT TimeGenerated,EventID,Message FROM 
```