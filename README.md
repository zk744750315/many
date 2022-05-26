

# qt多窗口 的非模态响应：https://blog.csdn.net/u011555996/article/details/124460064


彻底解决Qt报错：无法定位程序输入点于动态链接库 更改环境变量中编译器所在的位置，用的编译器要往前挪，详细见：https://blog.csdn.net/qq_41709234/article/details/123410055



qt控件属性的详细介绍：https://blog.csdn.net/ClementCXL/article/details/118935444?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-118935444-null-null.pc_agg_new_rank&utm_term=widget+%E5%92%8Cframe%E7%9A%84%E5%8C%BA%E5%88%AB&spm=1000.2123.3001.4430


有时候会遇到乱码，解决：
1 运行时设置为utf_8(  #pragma execution_character_set("utf-8")  ),然后在将运行时修改为utf_8，在main函数中添加语句：QTextCodec::setCodecForLocale(QTextCodec::codecForName("UTF-8"));



#关于vs调用dll文件若干问题
1：dll  lib文件的调用详细可参考：https://javajgs.com/archives/75992
     添加lib 头文件：依次点击“项目——配置属性——C/C++——常规”，在“附加包含目录”中加入.h文件所在的文件夹路径       依次点击“项目——配置属性——链接器——常规”，
在“附加库目录”中加入.lib所在的文件夹的路径      在“链接器”中找到“输入”，在“附加依赖项”中加入需要加入的xxx.lib;（要用;和其他链接库分隔开）；
     添加dll文件：debug生成的dll文件要放在使用dll项目的debug目录下，同样，release成成的dll要放在使用该dllw文件的release目录下，放错是会报错的。releas模式下，添加dll文件以后还不能运行，需要将程序所有依赖的包添加进来，采用msvc_32  msvs_64对应的windeployqt.exe工具打包生成，这里以msvs_64为例，对应的windeployqt.exe文件在D:\workSoftware\qt\qt\5.12.9\msvc2017_64\bin目录下，其他的也在相应文件夹下面。假如我们要发布的可执行文件为：D:\data\test\pluginPro\test1_vs\WidgetLibUse\WidgetlibUse\release\WidgetlibUse.exe, 命令右键打开msvs_64的命令提示符窗口，输入windeployqt D:\data\test\pluginPro\test1_vs\WidgetLibUse\WidgetlibUse\release\WidgetlibUse.exe,即可（https://www.jb51.net/article/226929.htm）
     
     
#关于qt工程（vs中的qt同样）调试时出现“未加载 Qt5Cored.pdb 包含查找模块 Qt5Cored.dll 的源文件....”类似的问题
这是调试qt文件时缺少调试的信息出现的，跟自己写的代码没有关系，也不用去关心它，
直接在调用堆栈中跳过QT本身的调用栈，到自己关心的函数入口即可。（崩溃的时候在弹出的对话框按相应按钮进入调试，按Alt+7键查看Call Stack即“调用堆栈”里面从上到下列出的对应从里层到外层的函数调用历史。双击某一行可将光标定位到此次调用的源代码或汇编指令处，看不懂时双击下一行，直到能看懂为止。）
