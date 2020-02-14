# Python3正则去掉HTML标签

1.引用一段代码

```
import re
 
html = '<pre class="line mt-10 q-content" accuse="qContent">\
目的是通过第一次soup.find按class粗略筛选并通过soup.find_all筛选出列表中的a标签并读入href和title属性<br><br>\
但是由于目标链接可能有图片链接,而这是我不想要的.请问如何去除?<br></pre>'
 
reg = re.compile('<[^>]*>')
 
print(reg.sub('',html))
```

2.重点

```
reg = re.compile('<[^>]*>')
 
print(reg.sub('',html))
```

3.实例

开始

```
import requests
import re
from bs4 import BeautifulSoup
retxt=open('test.log','r')
for x in range(250,999):
    #rurl=rurl.strip('\n')
    url='http://ananas.mooc1.mti100.com/tologin?fid={0}'.format(x)
    #print(url)
    try:

        response=requests.get(url,timeout=1).text
        #print(response)
        soup=BeautifulSoup(response,features="lxml")
        result=soup.find_all('span',attrs={'class':'l_schoolName2'})
        print('学校：{0}'.format(result))
    except requests.exceptions.InvalidURL:
        pass
    except requests.exceptions.ConnectionError:
        pass
    except requests.exceptions.ReadTimeout:
        pass
```

输出

```
学校：[<span class="l_schoolName2" id="schoolName2">
                                杭州师范大学
                        </span>]
学校：[<span class="l_schoolName2" id="schoolName2">
```

去除标签之后

```
import requests
import re
from bs4 import BeautifulSoup
#retxt=open('test.log','r')
for x in range(250,999):
    #rurl=rurl.strip('\n')
    url='http://ananas.mooc1.mti100.com/tologin?fid={0}'.format(x)
    #print(url)
    try:

        response=requests.get(url,timeout=1).text
        #print(response)
        soup=BeautifulSoup(response,features="lxml")
        result=soup.find_all('span',attrs={'class':'l_schoolName2'})
        reg=re.compile('<[^>]*>',re.S)
        print('学校：{0}'.format(reg.sub('',str(result))))
    except requests.exceptions.InvalidURL:
        pass
    except requests.exceptions.ConnectionError:
        pass
    except requests.exceptions.ReadTimeout:
        pass
```

输出

```
学校：[]
学校：[]
学校：[
                                上海电子信息职业技术学院
                        ]
学校：[]
学校：[
                                超星大学
```

