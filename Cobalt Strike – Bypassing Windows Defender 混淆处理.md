对于所有红色团队成员来说，在交付有效载荷的同时又不拖延组织的所有风吹草动始终是一个挑战。就像所有其他安全解决方案一样，Windows Defender在检测由Cobalt Strike等工具生成的通用有效载荷方面也变得更加出色。

在此示例中，我们将通过Cobalt Strike生成PowerShell负载，并了解如何对其进行操作，使其绕过Windows 10 PC上的Windows Defender执行。这不是从Windows Defender隐藏有效负载的最优雅或更简单的解决方案，但这是我们使用的方法之一，并且可以正常工作。

创建有效负载的过程如下：

![3月14日的截图16 50 50](https://i2.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/Screenshot-at-Mar-14-16-50-50.png?resize=599%2C337)

这将导致创建包含PowerShell命令的payload.txt。

![3月14日16 53 00的屏幕截图](https://i1.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/Screenshot-at-Mar-14-16-53-00.png?resize=598%2C202)

如果我们尝试在受害PC上运行命令，则Windows Defender会向我们打招呼，它将其视为威胁。

![3月12日21 30 46的屏幕截图](https://i2.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/Screenshot-at-Mar-12-21-30-46.png?resize=599%2C324)

为了绕过Windows Defender，我们需要首先了解Cobalt Strike如何创建其有效负载，然后更改其某些签名，希望Windows Defender认为它是安全的。

首先，很明显，通过查看格式或通过*-encodedcommand* PowerShell标志可以对base64编码的有效负载命令进行*编码*。

要解码命令，我们需要剪切

```
powershell.exe` `-nop` `-w` `hidden` `-encodedcommand
```

分开并保留其余部分。

然后使用以下命令解码字符串的其余部分。

```
echo` `'base64 payload'` `| ``base64` `-d
```

![3月14日17 02 40的屏幕截图](https://i0.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/Screenshot-at-Mar-14-17-02-40.png?resize=599%2C189)

生成的解码字符串再次包含base64编码的字符串，但尝试解码将不起作用并吐出乱码，因为该字符串也是从*IEX*明显经过Gzip压缩的*（New-Object IO.StreamReader（New-Object IO.Compression.GzipStream（ $ s [IO.Compression.CompressionMode] :: Decompress）））。ReadToEnd（）*部分，PowerShell命令。

现在我们需要了解此命令中的内容，因为这实际上是触发Windows Defender的部分，即有效负载。通过一些Google搜索，我找到了该PowerShell脚本，它完全可以完成 [http://chernodv.blogspot.com.cy/2014/12/powershell-compression-decompression.html

](http://chernodv.blogspot.com.cy/2014/12/powershell-compression-decompression.html)

```
$data` `= ``[System.Convert]``::FromBase64String(``'gzip base64'``)``$ms` `= ``New-Object` `System.IO.MemoryStream``$ms``.Write(``$data``, 0, ``$data``.Length)``$ms``.Seek(0,0) | ``Out-Null``$sr` `= ``New-Object` `System.IO.StreamReader(``New-Object` `System.IO.Compression.GZipStream(``$ms``, ``[System.IO.Compression.CompressionMode]``::Decompress))``$sr``.ReadToEnd() | ``set-clipboard
```

该脚本首先将对字符串进行base64解码，然后将其解压缩，从而为我们提供完整的代码。它还会将输出的内容复制到剪贴板，以将其粘贴到文本文件中，以备后用。

![3月14日17 42 10的屏幕截图](https://i1.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/Screenshot_at_Mar_14_17-42-10.png?resize=598%2C296)

在**$ var_code**变量保存正在由Windows Defender的探测有效载荷，我们需要换出绕过防守者。

进一步将**$ var_code**解码 是一系列ASCII字符，但此时不需要完全解码。

```
$enc``=``[System.Convert]``::FromBase64String(``'encoded string'``)
```

我们可以通过以下方式阅读部分内容：

```
$readString``=``[System.Text.Encoding]``::ASCII.GetString(``$enc``)
```

![Windows 10 1](https://i2.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/Windows_10_1-1.png?resize=586%2C39)

现在，以上内容显示了有关用户代理和攻击者IP的一些信息。

现在的目标是获取当前有效负载并对其进行混淆，从而欺骗Windows Defender。对于此类工作，最好的工具和首选工具是[Daniel Bohannon的](https://twitter.com/danielhbohannon?lang=en) Invoke-Obfuscation 。可以在[这里](https://github.com/danielbohannon/Invoke-Obfuscation)找到该项目的Github页面。

以Invoke-Obfuscation开头的命令是：

```
Import-Module` `.\``Invoke-Obfuscation``.psd1``Invoke-Obfuscation
```

![3月14日18 09 29的屏幕截图](https://i0.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/Screenshot-at-Mar-14-18-09-29.png?resize=598%2C430)

现在，我们需要定义需要混淆的有效载荷部分。可以使用以下命令完成

```
Set scriptblock ``'final_base64payload'
```

![3月14日18 11 56的屏幕截图](https://i2.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/Screenshot-at-Mar-14-18-11-56.png?resize=598%2C202)

该工具将使用我们的脚本块，然后询问我们要进行的方式。在这种情况下，我选择了COMPRESS，然后选择了1。这并不意味着其他选项将不起作用，但是在撰写本文时，我发现该选项正在起作用。Invoke-Obfuscation将发挥神奇的作用，并打印出PowerShell命令，该命令经过充分处理，可能会绕过Windows Defender。

![2](https://i0.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/2.png?resize=597%2C172)

然后，只需键入Out，然后输入要将其另存为PowerShell脚本的路径即可。

```
Out c:\payload.ps1
```

![3](https://i0.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/3.png?resize=598%2C154)

先前步骤中当前解压缩的有效负载如下所示。

![3月14日的截图18 37 47](https://i2.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/Screenshot-at-Mar-14-18-37-47.png?resize=598%2C296)

因此，这全都归结为以下事实：我们需要用我们从Invoke-Obfuscation新创建的有效负载替换**[Byte []] $ var_code = [System.Convert] :: FromBase64String**内容。为此，我定义了一个新变量，我将其称为$ evil，然后将Invoke-Obfuscation的输出内容放入其中。

**重要提示** –您需要在最后一个|之后去除零件。来自Invoke-Obfuscation的输出，因为这是执行命令的命令。我们将不需要它，因为Cobalt Strike模板将为我们做到这一点。

![4](https://i2.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/4.png?resize=598%2C294)

将编辑后的脚本保存到PowerShell文件中并执行它。如果您使用的是[@sec_groundzero Aggressor ](https://twitter.com/Sec_GroundZero)[脚本，](https://github.com/secgroundzero/CS-Aggressor-Scripts)则结果应为Cobalt Strike中的信标和Slack通知。

![5](https://i2.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/5.png?resize=598%2C214)

![6](https://i2.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/6.png?resize=595%2C135)

如果我们使用Process Hacker来检查原始CS有效负载和修改后的CS有效负载，我们会发现我们没有更改信标的基本行为。

![流程2](https://i0.wp.com/www.offensiveops.io/wp-content/uploads/2018/03/process2.png?resize=598%2C296)
