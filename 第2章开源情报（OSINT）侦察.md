# 第2章开源情报（OSINT）侦察

贡献者：伊恩·巴维斯翻译：BugMan

哇，慢点牛仔吧！在我们深入探讨“做性感时光”（笑话）红队闻名的黑客冒险，还有一些作业要做。五分之一的专业人士从未学习或做过任何工作作业” 。关键的第一步，收集有关特定目标的信息，在操作范围内，允许攻击者发现潜在的突破并组织的防御系统中可能被利用的弱点；他们是身体上的，社会工程学，逻辑或三者的结合。信息是新的交流商品因此，字面上有大量关于几乎免费提供的任何主题的信息

> 那么OSINT到底是什么意思？

开源情报（OSINT）使用公开可用的来源来收集来自各种来源的有关个人或实体的信息（即情报包括互联网。）OSINT通常在黑客侦察和信息侦察阶段执行从此阶段收集的数据将继续进行到网络枚举阶段。由于广阔]（[https://osintframework.com/）网络上的大量信息，攻击者必须进行清晰，明确的搜索](https://osintframework.com/)网络上的大量信息，攻击者必须进行清晰，明确的搜索) ]（[https://osintframework.com/）[框架\]（https：/ /osintframework.com/），以及各种的OSINT收集工具，以方便处理数据；否则，他们可能会在压倒性的信息海中迷失方向。OSINT侦察](https://osintframework.com/)[框架](https://osintframework.com/)，以及各种各样的OSINT收集工具，以方便处理数据;否则，他们可能会在压倒性的信息海中迷失方向。OSINT侦察)

可以进一步细分为以下五个子阶段：

OSINT流程的各个阶段；

[![3QK0C6.th.png](https://camo.githubusercontent.com/4a7aa0625672fd1b4e09952c5bf5b1081899dd5c/68747470733a2f2f73322e617831782e636f6d2f323032302f30322f32322f33514b3043362e74682e706e67)](https://imgchr.com/i/3QK0C6)

图片由[OSINT PROCESS提供](https://cdn-images-1.medium.com/max/1600/0*V-fQOKyoNZvPZYAl)

> 来源识别：在此初始阶段，攻击者识别出潜在的信息来源。在整个过程中，内部记录了来源详细的注释，如有必要，请稍后再讲。

> 数据收集：在此阶段，攻击者从计算机收集和收集信息导致的来源以及在此阶段发现的其他来源。
>
> 数据处理和集成：在此阶段，攻击者处理收集的数据通过搜索可能导致以下方面的信息来获取可操作情报的数据：。

> 数据分析：这里，攻击者使用OSINT工具。

> 结果交付：在最后阶段，将发现结果提交/报告给其他成员

# OSINT工具

一分钱。因为涵盖每一个OSIN工具超过了本章的范围，但我们将介绍一些可能对Red Team经营有用的较流行的工具。执行OSINT是关于获取一些点滴滴的信息外推一个特定的人或实体，并通过OSINT工具运行该信息看看还有什么可以发现的。

> Google搜索和Dorking

```
举例来说，假设您已受聘去一家名为Exploration Media的公司  [组; 您执行Google搜索，该搜索在顶部返回以下网站域名  ](http://www.explorationsmediagroup.com/)[结果：www.explorationsmediagroup.com。您通过单击链接导航到该站点，然后  ](http://www.explorationsmediagroup.com/)[在网站底部发现一些网站链接标题为“ Other Notable Web”  ](http://www.theworldsworstwebsiteever.com/)属性。” 您单击第一个选项[www.theworldsworstwebsiteever.com](http://www.theworldsworstwebsiteever.com/)，然后选择  查找有关此站点的更多信息（顺便说一下，这是一个令人发指的网页  （1980年的倒叙））。如果您决定沿这个线索进一步走下互联网的困境，  您如何找到有关此站点的更多信息？  一种方法是使用所谓的“ [Google Dorking](https://www.techworm.net/2016/04/make-advanced-search-google-using-google-dorking.html) ”（也称为Google Hacking），  这是网络浏览器中使用的高级搜索字符串。本质上，我们正在使用  Google网络搜寻器搜索引擎。这是黑客如何采取行动的一个例子  技术并将其颠倒过来，使其以不一定设计的方式起作用。  与这些Google Dorks一起玩耍，了解您可以获得什么样的结果。  简单的Google Dorks清单；由[Techworm提供  ](https://www.techworm.net/2016/04/make-advanced-search-google-using-google-dorking.html) 然后，我们可以直接在浏览器中输入[Google Dork命令](https://gist.github.com/stevenswafford/393c6ec7b5375d5e8cdc)，例如：  

网站：www.theworldsworstwebsiteever.com分机：（doc | pdf | xls | txt  |ps | rtf | odt | sx [w | ps](https://www.peerlyst.com/tags/salary) w | ppt | pps | xml）  （intext：机密[薪金](https://www.peerlyst.com/tags/salary) | intext：“预算批准”）  inurl：机密  虽然此特定查询不会返回任何结果，但我们可以通过添加一个 布尔搜索运算符（例如“ OR”），我们可以看到所有这些类型的结果：  网站：www.theworldsworstwebsiteever.com 或分机：（doc | pdf | xls |  txt | ps | rtf | odt | sxw | psw | ppt | pps | xml）  

（intext：机密薪金| intext：“预算批准”）  inurl：机密  
```

[谁是](https://www.whois.net/)

[在上述示例的基础上，您可以使用几种WHOIS工具之一来解析域 ](http://www.theworldsworstwebsiteever.com/)[ [www.theworldsworstwebsiteever.com名称，会您发现您得到了一些信息](http://www.theworldsworstwebsiteever.xn--com%2C-o84fu1aie35bj5c07jb5eruaf41jtqcgyac7393fzrxa/) 例如注册商信息（godaddy.com）;它的创建时间（2008-05-14）；和ICAN查询产生两个服务器名称（NS1.EXPMG.NET和NS2.EXPMG.NET）。但是，您会注意到该IP地址丢失。嗯？你为什么想知道？这是因为WHOIS网站认为其保护了这些“危险”信息。换句话说，他们想

您为它的工作。但是，有了这个，您就可以继续学习，还有很多其他方法可以获取网站的IP地址。使用WHOIS.net [工具](https://www.peerlyst.com/tags/tool)获取网站域名OSINT使用WHOIS.icann.org工具获取网站域名OSINT

> 命令规范

使用任何xterm（Unix / Linux），命令转换（MS-DOS Windows）或PowerShell控制台（MS-DOS Windows），您都可以使用，无论如何，作为黑客，您可能更喜欢使用命令而不是GUI工具。可以使用以下命令对网站进行类似的查询：Tracert [www.theworldsworstwebsiteever.com](http://www.theworldsworstwebsiteever.com/) 在Linux中，正确的命令是traceroute。顺便说一PowerShell功能更强大系统管理工具的概述，而不是简单的MS-DOS命令示例。如果不是精通PowerShell您可能想要解决这个问题。在PowerShell控制台中使用tracert命令确定网站IP地址现在，我们有了一个IP地址，可以对它进行Nmap扫描。您也可以使用该IP [地址并通过另选一个OSINT工具运行它，该工具专门枚举IP地址，例如 ](https://www.onyphe.io/)[ONYPHE](https://www.onyphe.io/)

> [> Onyphe](https://www.onyphe.io/search/?query=50.62.134.34) IP地址扫描结果

如您所见，Onyphe搜索产生了很多有用的信息，我们以后可以在

> 枚举阶段。

[Spokeo](https://www.spokeo.com/)

诸如Spokeo等人搜索引擎将通过社交媒体网站进行爬网，白页，电子邮件地址，可公开获取的记录（例如犯罪录入或学校记录），以及许多其他类型的公开可用信息源。的名字在目标组织（例如，Explorations Media Group）内部，例如一个虚构的首席执行官。18岁“ John Jacob Jingleheimer Schmidt，” Spokeo的搜索引擎将返回几条线索，可以进一步缩小搜索参数的范围现在，Pipl，那是他们，英特尔技术，ZoomInfo目录，Zaba搜索，USSearch，史努比站，Radaris，仅举几例。有很多，还有更多尝试。现在您可能会开始明白为什么收集可识别个人身份的信息（PII）转换为其出售给投资者的第三方是一项利润丰厚的业务，并且将您的私人信息拒之门外是多么困难。作为红队成员，您应该对自己执行 同类型的查询，以确保您的私人信息或至少任何潜在的破坏性信息不会发布给所有人查看。

[检查OSINT框架](https://osintframework.com/)以获取更完整的人员搜索工具列表以及其他类型的OSINT工具。您还可以在Internet中对某人的姓名进行基本搜索

> 搜索引擎，例如Google，Bing和Yaahoo。

[Shodan](https://www.shodan.io/)

Shodan是一种流行的OSINT工具，专门用于与Internet连接的设备（例如，包括ICS，IoT，视频游戏系统等）。您可以关闭网站，提供一些附加功能；您可以查看实时摄像机的提要在地理上描述裂缝在世界各地的位置。您也可以执行与Shodan从命令中枚举IP地址相同类型的扫描进入枚举阶段时，请使用Nmap扫描仪工具执行以下操作：

```
nmap -sn -Pn -n --script = shodan-api -script-args  

'shodan-api.apikey = XXXXXX'worldsworstwebsiteever.com  
```

对于上面的命令，-sn添加端口扫描；-P n跳过主机发现和不ping主人；和-n跳过DNS解析。

探索Shodan搜索引擎

[数据宝](https://github.com/DataSploit/datasploit)

[Datasploit是在Kali或](https://www.kali.org/)[BlackArch Linux OS发行版中](https://blackarch.org/tools.html)[找到的另一个OSINT工具](https://www.kali.org/)[， ](https://blackarch.org/tools.html)收集您要定位的特定域，电子邮件，用户名或电话号码上的数据，然后将结果统一组织在HTML和JSON报告或文本中文件中。Datasploit将尝试查找替代，API密钥，令牌，子域，域历史记录，旧版门户和更多。

[![3QKjP0.md.png](https://camo.githubusercontent.com/461aca3d4d11392e65a668c9d31c34271225ff04/68747470733a2f2f73322e617831782e636f6d2f323032302f30322f32322f33514b6a50302e6d642e706e67)](https://imgchr.com/i/3QKjP0)

Datasploit OSINT工具；图片由[KitPloit提供](https://www.kitploit.com/2018/09/datasploit-osint-framework-to-perform.html)

> [马尔特戈](https://www.paterva.com/web7/buy/maltego-clients/maltego-ce.php)

Maltego社区版（CE）是Paterva提供的免费OSINT工具，具有相当多的分析现实世界中公开可用的关系信息的能。麦芽糖罐用于社交网站的足迹Internet基础架构，并收集有关使用它的人。 Maltego将查询DNS记录，whois记录，搜索引擎，社交网络，各种在线应用程序编程接口（API）并提取使用查找姓名，电子邮件地址，别名，组之间的相关关系，公司，组织，网站，域，DNS名称，网络块，IP地址，从属关系，文档和文件。

[![3QMCqJ.th.png](https://camo.githubusercontent.com/b349b2ce18d3a91ba6470525b72035aedfe32bf3/68747470733a2f2f73322e617831782e636f6d2f323032302f30322f32322f33514d43714a2e74682e706e67)](https://imgchr.com/i/3QMCqJ)

Maltego OSINT工具；图片由[Paterva.com提供](https://www.paterva.com/web7/buy/maltego-clients/maltego-ce.php)

> 社交媒体

社交网站，例如LinkedIn，Facebook，Peerlyst，Twitter，Google +，Instagram和

Snapchat可以成为寻求信息者的金矿。如果您考虑个人类型这些网站要求用户输入的信息，以及有时非常个人内容的类型用户经常发布到社交媒体上，它应该是Red的OSINT阶段的第一站之一 [团队合作。例如，要LinkedIn上收集信息，您可能需要](https://github.com/dchrastil/ScrapedIn)[签](https://github.com/dchrastil/ScrapedIn)[出 ](https://github.com/dchrastil/ScrapedIn)[ScrapedIn ](https://github.com/dchrastil/ScrapedIn)[。对于Facebook有](https://www.stalkscan.com/)[StalkScan ](https://www.stalkscan.com/)[；Twitter并没有](https://www.stalkscan.com/)[GeoChirp ](http://www.geochirp.com/)[，](http://www.geochirp.com/)[Tweepsmap](http://tweepsmap.com/)为

[位置数据和](https://tinfoleak.com/)[Tinfoleak](https://tinfoleak.com/)网页进行分析。约会网站，如Match.com，eHarmony公司，大量的鱼类，火种，OkCupid和阿什利麦迪逊的人也可能是潜在的宝藏检查特定的目标名称并收集更多信息。通过人们搜索，确实只有受您要走多远的限制。您可以在许多这些网站上进行进行钻取进一步尝试并获取更多信息，但是如果目标是特定的公司或组织。

> [自动机](http://www.tekdefense.com/automater/)

Automater是一个URL /域，IP地址和MD5哈希工具，试图进行分析关于入侵分析人员而言，此过程更加容易。给定一个目标（URL，IP或哈希）或一个充满目标的文件， Automater重新修改IPvoid.com，Robtex.com，

Fortiguard.com，unshorten.me，Urlvoid.com，Labs.alienvault.com，ThreatExpert，VxVault，VirusTotal。

[![3QMuse.th.png](https://camo.githubusercontent.com/b3795906f73f79028eb20e5f1fd716a96ce70c1a/68747470733a2f2f73322e617831782e636f6d2f323032302f30322f32322f33514d7573652e74682e706e67)](https://imgchr.com/i/3QMuse)

[自动OSINT工具；图片由SecurityOnline.com提供](https://securityonline.info/tag/automater/)

# [对于深层网络的OSINT侦查，可以使用多种搜索引擎](https://pubpeer.com/)

> [如](https://guides.library.harvard.edu/hks/think_tank_search)[PubPeer ](https://pubpeer.com/)[，](https://pubpeer.com/)[Google Scholar](https://scholar.google.com/)，康奈尔大学的[arXiv.org ](https://arxiv.org/)[和哈佛大学的](https://arxiv.org/)[Think](https://guides.library.harvard.edu/hks/think_tank_search)

[坦克搜索](https://guides.library.harvard.edu/hks/think_tank_search)[。使用深度Web搜索时，您主要是在查找文章，论文和 ](https://guides.library.harvard.edu/hks/think_tank_search)在学术期刊和专业出版物上发表的研究。

康奈尔大学的DeepWeb OSINT的arXiv.org

> [对于暗网的OSINT侦查，请使用](https://www.reddit.com/r/deepweb)[DeepDotWeb ](https://www.deepdotweb.com/)[，](https://www.reddit.com/r/deepweb)[Reddit ](https://www.reddit.com/r/deepweb)[等搜索引擎](https://www.reddit.com/r/deepweb)

> [Deep Web ](https://www.reddit.com/r/deepweb)[，](https://www.reddit.com/r/deepweb)[Reddit DarkNetMarkets ](https://www.reddit.com/r/darknetmarkets)[，](http://thehiddenwiki.org/)[隐藏的Wiki ](http://thehiddenwiki.org/)[，](http://thehiddenwiki.org/)[Core.onion ](http://eqt5g4fuenphqinx.onion/)[（来自Tor浏览器），](http://eqt5g4fuenphqinx.onion/)

> [OnionScan ](https://github.com/s-rah/onionscan)[和](https://github.com/s-rah/onionscan)[Tor Scan](http://www.torscan.io/)可能会提供一些有用的信息。但是，有了Dark Web，

有些网站和服务仅受邀请，可以使他们找到非常困难，因为它们不会出现在常规的暗网搜索中。

[在深色Web中进行分析是找到这些类型站点的唯一真实方法。记得 ](https://github.com/freenet/fred)还有到不是进入深色Web的唯一入口，还有[Freenet和](https://github.com/freenet/fred)[I2P](https://geti2p.net/en/)。使用OnionScan OSINT工具扫描深色网；图片由[Mascherari.press提供 ](https://mascherari.press/onionscan-whats-new-and-whats-next/)OSINT系列仅受您的想象力限制。您可以使用任何数量的这些工具或者

搜索示例并根据您的需求进行调整，逐步更好的结果。我们只涵盖

有这么多更多发现和试验工具，其中许多都包含在Kali或者BlackArch中
