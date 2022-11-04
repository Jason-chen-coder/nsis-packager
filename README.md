
### 1.	定制产品名称
修改SetupScripts目录下的对应项目目录下的 xxx_setup.nsi 文件， 
##### ====================== 自定义宏 产品信息==============================
!define PRODUCT_NAME           		"牛牛软件"
!define PRODUCT_PATHNAME 			"Leeqia_Capture"  #安装卸载项用到的KEY
!define INSTALL_APPEND_PATH         "Leeqia_Capture"	  #安装路径追加的名称 
!define INSTALL_DEFALT_SETUPPATH    ""       #默认生成的安装路径  
!define EXE_NAME               		"牛牛软件.exe"
!define PRODUCT_VERSION        		"2.5.0.0"
!define PRODUCT_PUBLISHER      		"NIUNIU"
!define PRODUCT_LEGAL          		"NIUNIU Copyright（c）2020"
!define INSTALL_OUTPUT_NAME    		"NIUNIU_PC_Setup_v2.5.0.exe"

##### ====================== 自定义宏 安装信息==============================
!define INSTALL_7Z_PATH 	   		"..\app.7z"
!define INSTALL_7Z_NAME 	   		"app.7z"
!define INSTALL_RES_PATH       		"skin.zip"
!define INSTALL_LICENCE_FILENAME    "licence.rtf"
!define INSTALL_ICO 				"logo.ico"
!define UNINSTALL_ICO				"uninst.ico"

### 2.	复制将要打包的文件
将要安装的文件信息放到FilsToInstall目录下;打包脚本将会对这个目录下（包含多级子目录）进行压缩打包

### 3.	在SetupScripts下对应的项目目录下的skin目录下配置界面
install.xml 这是一个总领脚本
configpage.xml  配置打开安装包后显示的第一个界面，也是用于选择安装路径等的界面 
licensepage.xml 配置许可协议显示界面 
installingpage.xml  安装过程中的界面 
finishpage.xml  安装完成的界面 

uninstallpage.xml  卸载入口界面
uninstallingpage.xml  安装过程中的界面 
uninstallfinishpage.xml  卸载完成的界面 
msgBox.xml       二级弹窗
images子目录用于存储界面所需要的图片信息 

### 4.	对安装脚本及流程进行修改
如果有特殊的安装功能项需要到SetupScripts下对应的项目目录下的xxx_setup.nsh文件中修改 
Function DUIPage      安装入口脚本，用于初始化一些信息
un.DUIPage            卸载入口脚本
BindUIControls        绑定按钮及其他响应事件
ShowMsgBox            显示二级子窗口 
OnBtnInstall          安装主流程控制

### 5.	启动打包
1) 直接在目录下执行  xxxx-nozip.bat  即可完成打包
2) 打好的包文件放在Output目录下
3) 注意，不要将打包脚本目录放到中文目录下 

6.	其他注意事项 
1) 目前配置的NSIS是Unicode版本  所以在控制台看到的中文会是一个？号，不用管它

@rem 如果要调试错误，请使用下面的脚本，这样会打开编译界面（命令行界面中文会显示成?号）
".\NSIS\makensisw.exe" xxx

2) nsi文件是配置安装的相关信息 真正的安装逻辑在commonfunc.nsh以及与nsi文件在一起的 nsh文件中


3) 有接口不明白的，请参考示例以及文档描述  

4) 所有的nsis的nsi以及 nsh文件均需要是unicode或者utf-8编码 

5) 对于ShowMsgBox接口，增加了一个参数，用于指定弹出窗口使用的样式文件，例如：
nsNiuniuSkin::ShowMsgBox "notice title" "notice message." 0 "msgBox2.xml"
这样则会使用msgBox2.xml中描述的样式来显示窗口，便于个性化定制提示弹出窗口  

6) 要使用rtf文件作为许可协议文件，将协议文件改成rtf即可（参考nim示例）

7) 经常会有朋友反馈安装包有报毒的情况，请按如下方式处理
通过执行setup.exe（不要签名），先生成卸载程序，然后对卸载程序进行签名，再将卸载程序打包进setup.exe。
1）首先，由NSIS生成setup.exe，注意：不要对setup.exe进行签名。
2）执行setup.exe，待安装完成后，到安装目录找到uninst.exe，然后对uninst.exe进行签名。
3）将uninst.exe文件与需要被释放到安装目录下的文件一起打包 (放到FilesToInstall目录下)。
4）注释掉commonfunc.nsh文件中的这句：WriteUninstaller "$INSTDIR\uninst.exe"。
5）重新编译setup.nsi生成setup.exe，并对其进行签名。

参考地址：
http://blog.csdn.net/hellokandy/article/details/52212495
