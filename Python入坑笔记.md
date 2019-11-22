### Python入坑记
#### 两个for 循环同时输出+正则文章
```
zip(list1,list2)
```
zip函数同时便利两个列表

```
import sys
import requests
import re
from bs4 import BeautifulSoup
hot="https://wiki.17-apt.tk"
a=requests.get(hot).content
b=BeautifulSoup(a)
#d=print(b)

c=re.findall('filePath":(.+?).md',str(b))
d=re.findall('documentURL":(.+?).createdAt',str(b))

for line,urls in zip(c,d):
    print(line,urls)


```

#### Linux基本信息收集

```
***************************
import sys
import os

#c2=os.system("uname -a")
#c3=os.system("pwd")
print('***************************')
print(os.system("whoami"))

#print('系统内核'+c2)
print('***************************')
#print('当前路径'+c3)
print(os.system("uname -a"))
print('***************************')
print(os.system("pwd"))
print('***************************')
print(os.system("ls -l"))
print('***************************')                                                                                                                          
```

![16741210545451.png](https://i.loli.net/2019/11/16/6nXJHmgaqOzVy8M.png)

#### 基于PHPSTUDY l.php的一个MySQL弱口令检测工具

```

import requests
from bs4 import BeautifulSoup
import re
from requests import ReadTimeout

proxies = {
"http":"http://127.0.0.1:8080",

"https":"https://127.0.0.1:8080",
}

data1={
    "host":"localhost",
    "port":"3306",
    "login":"root",
    "password":"abc@123456",
    "act":"MySQL检测",
    "funName":""
}
headers={
    "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:70.0) Gecko/20100101 Firefox/70.0",
    "Accept-Language":"zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2"
}
a=open('ip.txt',encoding='utf-8')
for ip in a.readlines():
    try:

        url="http://%s/l.php"%ip
        respone=requests.post(url,data=data1,headers=headers,timeout=5).content.decode('utf-8')
        soup=BeautifulSoup(respone)
        t=re.findall('alert\(\'(.+?).\)',str(soup))
        if '连接到MySql数据库正常' in str(t):
            print(ip+"------"+"存在弱口令")
        else:
            print(ip+"------"+"不存在弱口令！！！！")
    except (ConnectionError, ReadTimeout):
        print(ip+"------"+"超时！！！")
    except:
        pass
        
```

#### 查询手机归属地

```

import pandas as pd
import xlwt import *
from phone import Phone
#book=xlwt.Workbook('hhcl.xlsx')
#sheet=book.add_sheet('hhlc')

book = xlwt.Workbook(encoding='utf-8',style_compression=0)
sheet = book.add_sheet('mysheet',cell_overwrite_ok=True)

f1=open(r'12232.txt','r')
f2=open('doct1.csv','wb')
for phoneNUM in f1.readlines():
	phoneNum=phoneNUM.strip("\t\r\n")
	#phoneNUM=str(phoneNUM)
	#print(phoneNum)
	p=Phone()
	info=p.find(phoneNum)
	if info is 	None:
		pass
	else:
		print(info['phone']+"------"+info['province']+"------"+info['city']+"-------"+info["area_code"]+"-------"+info["phone_type"])
		#'cityzip_code','area_code','phone_type'

```


#### 函数调用

```

import requests
from bs4 import BeautifulSoup
url="http://www.baidu.com"
def re_url(url):#设置请求 
    respone=requests.get(url).content
    return respone#返回参数

def soup_txt(res):#soup格式化
    sou_text=BeautifulSoup(res,"html.parser")
    #print(sou_text)
    return sou_text
    
res=re_url(url)#调用方法
html=soup_txt(res)#调用方法
print(html)

```
#### python获取SRC列表

```
import requests
from bs4 import BeautifulSoup
import re
url="https://www.anquanke.com/src"
def res_src(url):
    respone=requests.get(url).content.decode('utf-8')
    return respone
res=res_src(url)
code=re.findall(r'<a target="_blank" rel="noopener noreferrer" href="/src/(.*)">',str(res))
for codes in code:
    respones_src=requests.get("https://www.anquanke.com/src/%s"%codes).content.decode('utf-8')
    src_list=re.findall(r'<a rel="noopenner noreferrer" href="(.*)">',str(respones_src))
    print("网址："+src_list[0]+"----------------------------------"+"邮箱："+src_list[2])

效果
网址：https://security.360.cn/----------------------------------邮箱：mailto:security@360.cn
网址：https://security.iqiyi.com/----------------------------------邮箱：mailto:71src@qiyi.com
网址：https://security.alibaba.com/----------------------------------邮箱：mailto:security@service.alibaba.com
网址：http://security.safedog.cn/reporter_center.html----------------------------------邮箱：mailto:Lab@safedog.cn
网址：https://security.alipay.com/sc/afsrc/home.htm----------------------------------邮箱：mailto: security@alipay.com
网址：http://sec.baidu.com/----------------------------------邮箱：mailto:security@baidu.com
网址：https://www.bugx.io/----------------------------------邮箱：mailto:Cathy@bugx.io
网址：https://security.bilibili.com/----------------------------------邮箱：mailto:
网址：https://www.butian.net----------------------------------邮箱：mailto:butian_report@qianxin.com
网址：https://security.58.com/----------------------------------邮箱：mailto:sec@58ganji.com
网址：正在启动，敬请期待ing----------------------------------邮箱：mailto:bug@carsrc.org
网址：https://sec.cainiao.com/----------------------------------邮箱：mailto:
网址：http://sec.didichuxing.com/----------------------------------邮箱：mailto:dsrc@didichuxing.com
网址：http://security.eastmoney.com/----------------------------------邮箱：mailto:security@eastmoney.com
网址：https://security.douyu.com/----------------------------------邮箱：mailto:security@douyu.com
网址：https://security.dianrong.com/----------------------------------邮箱：mailto:security@dianrong.com
网址：https://dvpnet.io/----------------------------------邮箱：mailto:service@dvpnet.io
网址：https://security.ele.me/----------------------------------邮箱：mailto:
网址：https://fsrc.fuiou.com/home/index.html----------------------------------邮箱：mailto:zhoujia@fuiou.com
网址：https://security.guazi.com----------------------------------邮箱：mailto:GZSRC@guazi.com
网址：https://src.saicmobility.com----------------------------------邮箱：mailto:sec_src@saicmobility.com
网址：http://src.100tal.com/----------------------------------邮箱：mailto:security@100tal.com
网址：https://www.huobigroup.com/----------------------------------邮箱：mailto:sec@huobi.com
网址：https://www.huawei.com/cn/psirt----------------------------------邮箱：mailto:PSIRT@huawei.com
网址：https://sec.tutorabc.com.cn----------------------------------邮箱：mailto:sec@vipabc.com
网址：http://security.jd.com/----------------------------------邮箱：mailto:security@jd.com
网址：https://security.focuschina.com/----------------------------------邮箱：mailto:security@focuschina.com
网址：https://security.jj.cn/----------------------------------邮箱：mailto:jjsrc@service.jj.cn
网址：http://sec.kingsoft.com/----------------------------------邮箱：mailto:security#kingsoft.com
网址：https://security.kuaishou.com----------------------------------邮箱：mailto:sec@kuaishou.com
网址：https://security.itiger.com/----------------------------------邮箱：mailto:sec@itiger.com
网址：http://sec.le.com/----------------------------------邮箱：mailto:lesrc@le.com
网址：http://security.lexinfintech.com/security/index----------------------------------邮箱：mailto:
网址：https://lsrc.vulbox.com/----------------------------------邮箱：mailto:lsrc@lenovo.com
网址：https://sec.ly.com/----------------------------------邮箱：mailto:sec@ly.com
网址：http://security.mogujie.com/#/----------------------------------邮箱：mailto:security@meili-inc.com
网址：http://sec.meizu.com----------------------------------邮箱：mailto:huangxin1@meizu.com
网址：https://security.immomo.com/----------------------------------邮箱：mailto:
网址：https://security.mafengwo.cn/----------------------------------邮箱：mailto:mfsrc@mafengwo.com
网址：https://security.mobike.com----------------------------------邮箱：mailto:security@mobike.com
网址：https://security.meituan.com/----------------------------------邮箱：mailto:security@meituan.com
网址：http://www.niwodai.com/sec/index.do----------------------------------邮箱：mailto:src@niwodai.net
网址：https://security.oppo.com/----------------------------------邮箱：mailto:security@oppo.com
网址：http://anquan.163.com/----------------------------------邮箱：mailto:
网址：http://security.pingan.com/----------------------------------邮箱：mailto:
网址：https://security.qunar.com/----------------------------------邮箱：mailto:security@qunar.com
网址：https://security.rong360.com/#/----------------------------------邮箱：mailto:rsrc@rong360.com
网址：https://www.seebug.org/----------------------------------邮箱：mailto:s1@seebug.org
网址：http://sec.sina.com.cn/----------------------------------邮箱：mailto:
网址：http://sfsrc.sf-express.com/index----------------------------------邮箱：mailto:sfsrc@sf-express.com
网址：http://sec.sogou.com/----------------------------------邮箱：mailto:
网址：https://security.suning.com----------------------------------邮箱：mailto:security@suning.com
网址：http://sec.tuniu.com/----------------------------------邮箱：mailto:sec@tuniu.com
网址：https://security.tencent.com/----------------------------------邮箱：mailto:
网址：https://sec.vip.com/----------------------------------邮箱：mailto:sec@vipshop.com
网址：https://security.vipkid.com.cn/----------------------------------邮箱：mailto:security@vipkid.com.cn
网址：https://security.vivo.com.cn----------------------------------邮箱：mailto:security@vivo.com
网址：https://sec.wacai.com/----------------------------------邮箱：mailto:src@wacai.com
网址：http://wsrc.weibo.com/----------------------------------邮箱：mailto:
网址：https://security.webank.com/----------------------------------邮箱：mailto:security@webank.com
网址：http://security.wanmei.com----------------------------------邮箱：mailto:src@pwrd.com
网址：https://sec.weidai.com.cn----------------------------------邮箱：mailto:secinfo@wdai.com
网址：https://sec.wifi.com/----------------------------------邮箱：mailto:sec@wifi.com
网址：https://sec.xiaomi.com/----------------------------------邮箱：mailto:
网址：https://security.ximalaya.com/----------------------------------邮箱：mailto:security@ximalaya.com
网址：https://sec.ctrip.com/----------------------------------邮箱：mailto:
网址：https://security.creditease.cn/index.html----------------------------------邮箱：mailto:cesrc@creditease.cn
网址：https://sec.zbj.com/----------------------------------邮箱：mailto:sec@zbj.com
网址：https://security.bytedance.com----------------------------------邮箱：mailto:security@bytedance.com
网址：https://sec.zto.com/----------------------------------邮箱：mailto: security@zto.cn
网址：https://src.zhaopin.com----------------------------------邮箱：mailto:src@zhaopin.com.cn
```
