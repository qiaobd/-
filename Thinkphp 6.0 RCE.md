		ThinkPHP是用于敏捷Web应用程序开发的开源PHP开发框架。该框架在全球范围内被广泛采用，Shodan的快速搜索显示了40,000多个有效部署。



		1月15 日，由供应商修补了ThinkPHP中的一个新漏洞。该漏洞使攻击者可以写入或覆盖系统中的任意文件。该漏洞的根本原因是会话管理功能，该会话管理功能使用会话cookie的用户控制值作为文件系统中保存的文件的名称。通过使用目录遍历，攻击者可以将文件保存在系统中的任何位置。如果文件的内容（取决于特定的应用程序逻辑）也是可控制的，则攻击者可以将Web Shell写入系统并对其进行访问。



>重要的是要注意，会话启动默认情况下未启用，并且需要手动更改配置。该漏洞影响ThinkPHP 6.0.0-6.0.1版本。



**技术细节**



攻击者将自定义PHPSESSID发送到服务器：

![img](https://devcentral.f5.com/servlet/servlet.ImageServer?id=0151T000003lUotQAE&oid=00D00000000hXqvEAE)

图1： *带有用户控制的会话cookie的请求*



服务器处理请求，并使用PHPSESSID cookie值来设置用户的会话：

![img](https://devcentral.f5.com/servlet/servlet.ImageServer?id=0151T000003lUouQAE&oid=00D00000000hXqvEAE)

图2： *使用用户控制的值设置会话ID*



﻿﻿﻿﻿﻿﻿﻿﻿﻿如果满足条件，则应用程序将验证PHPSESSID值是一个32字节的字符串，然后接受并设置会话值：

![img](https://devcentral.f5.com/servlet/servlet.ImageServer?id=0151T000003lUoyQAE&oid=00D00000000hXqvEAE)

图3：*验证值是一个32字节的字符串*



建立会话ID值之后，在构造响应时，应用程序将会话信息保存到以会话ID值作为名称的文件中：

![img](https://devcentral.f5.com/servlet/servlet.ImageServer?id=0151T000003lUp3QAE&oid=00D00000000hXqvEAE)



![img](https://devcentral.f5.com/servlet/servlet.ImageServer?id=0151T000003lUpIQAU&oid=00D00000000hXqvEAE)

图4、5： *使用会话cookie的值将文件写入系统*



﻿﻿﻿﻿﻿﻿

![img](https://devcentral.f5.com/servlet/servlet.ImageServer?id=0151T000003lUpNQAU&oid=00D00000000hXqvEAE)

图6： *在文件系统上创建的文件*



供应商修补了ThinkPHP，并添加了对PHPSESSID值的附加检查，该检查仅允许使用字母数字字符，从而防止了目录遍历的可能性： 



![img](https://devcentral.f5.com/servlet/servlet.ImageServer?id=0151T000003lUpSQAU&oid=00D00000000hXqvEAE)



**使用BIG-IP ASM缓解漏洞**

任何受支持的BIG-IP版本下的BIG-IP ASM客户都已受到保护，免受此漏洞的影响。在利用此漏洞时，攻击者将尝试发送包含目录遍历的有效负载。现有的攻击特征将检测出利用尝试。

![img](https://devcentral.f5.com/servlet/servlet.ImageServer?id=0151T000003lUpXQAU&oid=00D00000000hXqvEAE)

图8：*利用攻击签名阻止的利用（200000190）*

﻿﻿﻿

![img](https://devcentral.f5.com/servlet/servlet.ImageServer?id=0151T000003lUpcQAE&oid=00D00000000hXqvEAE)

图9：*利用攻击签名阻止的利用（200101550）*



此外，如果攻击者尝试将PHP代码注入到存储在会话文件中以进行进一步利用，则签名会检测到它，这些签名可以在包括“命令执行”和“服务器端代码注入”攻击类型的签名集中找到。或“ PHP”系统。
