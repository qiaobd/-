# SHELL下打包文件

在我们拿下webshell的时候，想要获取数据或者源码往往会用菜刀或者蚁剑去打包，但是这个时候往往就会出现很多问题，列如打包失败，或者是打包得不完整等等。

这个时候如果对方是windows服务器的话，我们可以将我们本地装的winrar.exe上传过去

![img](https://mmbiz.qpic.cn/mmbiz_png/Y4GGz5GfRtR5FXFEduwT0tniaOBd9icIsRsBWtCyrJHH0pdKYIpeQX26icicxgKGVzI1jM7vDo3wX8lLicOQIqeQHog/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

压缩盘下的dat文件夹，并且命名为bat.rar winrar.exe a -ag -k -r -s -ibck c:/bak.rar c:/dat/



压缩多个文件 winrar a -ag -ibck bak.rar filename1 filename2 filename....
