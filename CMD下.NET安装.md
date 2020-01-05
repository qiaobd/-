# CMD下.*NET*安装

本文将要介绍以下内容：


**·** Win2008下安装Microsoft .NET Framework 4.0的正常方法

**·** 命令行下的实现方法

**·** 实现原理

**·** 脚本开发的细节

##### Web Installer

 https://www.microsoft.com/en-us/download/details.aspx?displaylang=en&id=17851 

 Web Installer的文件很小，在安装过程中需要Internet连接来下载其他所需的.NET Framework组件 

 **Standalone Installer** 

需要gui-鼠标点击安装

## CMD安装

1.我们可以通过向安装程序的面板发送按键消息来模拟用户的点击行为

2.为保证在命令行下安装，需要对弹出的对话框发送隐藏窗口的消息

3.为保证按键准确，这里不应该采用计算坐标的方法模拟鼠标点击，而是枚举窗口获得按钮的句柄，向目标句柄发送鼠标点击的消息

#### C代码

```
 \#include <afx.h> #include <Windows.h> BOOL CALLBACK EnumChildWindowProc(HWND Child_hWnd, LPARAM lParam) { WCHAR szTitle[1024]; if (Child_hWnd) { GetWindowText(Child_hWnd, szTitle, sizeof(szTitle)); printf("[*] Handle: %08X\n", Child_hWnd); printf("[*] Caption: %ws\n", szTitle); return true; } return false; } int _tmain(int argc, _TCHAR *argv[]) { HWND hWnd3 = FindWindow(NULL, L"Microsoft .NET Framework 4 Setup"); if (hWnd3 == NULL) { printf("[!] I can't find the main window.\n"); return 0; } EnumChildWindows(hWnd3, EnumChildWindowProc, 0); return 0; } 
```

![image-20191106101415700.png](https://i.loli.net/2019/11/06/GnF1NueSCbmBOHP.png)

### 开源项目地址

 [https://github.com/3gstudent/Homework-of-C-Language/blob/master/Install_.Net_Framework_from_the_command_line.cpp]( https://github.com/3gstudent/Homework-of-C-Language/blob/master/Install_.Net_Framework_from_the_command_line.cpp ) 

 		代码支持命令行下安装Microsoft .NET Framework 4、Microsoft .NET Framework 4.5和Microsoft .NET Framework 4.5.1 
