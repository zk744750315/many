

# qt多窗口 的非模态响应：https://blog.csdn.net/u011555996/article/details/124460064


彻底解决Qt报错：无法定位程序输入点于动态链接库 更改环境变量中编译器所在的位置，用的编译器要往前挪，详细见：https://blog.csdn.net/qq_41709234/article/details/123410055



qt控件属性的详细介绍：https://blog.csdn.net/ClementCXL/article/details/118935444?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-118935444-null-null.pc_agg_new_rank&utm_term=widget+%E5%92%8Cframe%E7%9A%84%E5%8C%BA%E5%88%AB&spm=1000.2123.3001.4430


有时候会遇到乱码，解决：
1 运行时设置为utf_8(  #pragma execution_character_set("utf-8")  ),然后在将运行时修改为utf_8，在main函数中添加语句：QTextCodec::setCodecForLocale(QTextCodec::codecForName("UTF-8"));
QTextCodec::setCodecForLocale(QTextCodec::codecForName("UTF-8"));这句话是将全局的字符编码保存为utf8编码,也可以设置为GB2312。验证：QString a = "我是赵凯"；
qDebug()<<(a.toLocal8Bit());



#关于vs调用dll文件若干问题
1：dll  lib文件的调用详细可参考：https://javajgs.com/archives/75992
     添加lib 头文件：依次点击“项目——配置属性——C/C++——常规”，在“附加包含目录”中加入.h文件所在的文件夹路径       依次点击“项目——配置属性——链接器——常规”，
在“附加库目录”中加入.lib所在的文件夹的路径      在“链接器”中找到“输入”，在“附加依赖项”中加入需要加入的xxx.lib;（要用;和其他链接库分隔开）；
     添加dll文件：debug生成的dll文件要放在使用dll项目的debug目录下，同样，release成成的dll要放在使用该dllw文件的release目录下，放错是会报错的。releas模式下，添加dll文件以后还不能运行，需要将程序所有依赖的包添加进来，采用msvc_32  msvs_64对应的windeployqt.exe工具打包生成，这里以msvs_64为例，对应的windeployqt.exe文件在D:\workSoftware\qt\qt\5.12.9\msvc2017_64\bin目录下，其他的也在相应文件夹下面。假如我们要发布的可执行文件为：D:\data\test\pluginPro\test1_vs\WidgetLibUse\WidgetlibUse\release\WidgetlibUse.exe, 命令右键打开msvs_64的命令提示符窗口，输入windeployqt D:\data\test\pluginPro\test1_vs\WidgetLibUse\WidgetlibUse\release\WidgetlibUse.exe,即可（https://www.jb51.net/article/226929.htm）
     
     
#关于qt工程（vs中的qt同样）调试时出现“未加载 Qt5Cored.pdb 包含查找模块 Qt5Cored.dll 的源文件....”类似的问题
这是调试qt文件时缺少调试的信息出现的，跟自己写的代码没有关系，也不用去关心它，
直接在调用堆栈中跳过QT本身的调用栈，到自己关心的函数入口即可。（崩溃的时候在弹出的对话框按相应按钮进入调试，按Alt+7键查看Call Stack即“调用堆栈”里面从上到下列出的对应从里层到外层的函数调用历史。双击某一行可将光标定位到此次调用的源代码或汇编指令处，看不懂时双击下一行，直到能看懂为止。）



#无法定位程序输入点的解决办法：
// 打开QT生成的exe显示，缺少动态链接库
将exe赋值到编译器bin路径下，exe能够正常打开。初步确定是环境变量问题

// 本机用的编译器路径bin路径
C:\qt5.12.2\installPath\5.12.2\mingw73_64\bin

// 将路径写入系统环境变量，并将其移动到其他变量值之前
此电脑 -> 属性 -> 高级系统设置 -> 高级 -> 环境变量  -> 系统变量 -> 双击path -> 新建 -> 填入QT编译器的bin路径 -> 上移（移动到最前）-> 确定  -> 确定  -> 确定  

// 此时再双击QT release文件下生成的exe，显示无法定位程序输入点
1、将生成的exe赋值到一个新建的文件夹，如桌面test文件
2、用编译器命令窗下调用windeployqt命令，自动寻找需要的动态链接库。（windeployqt是调用编译器的windeployqt.exe程序，我本机的该文件路径为C:\qt5.12.2\installPath\5.12.2\mingw73_64\bin\windeployqt.exe）








qt枚举的用法参考https://blog.csdn.net/For_1ove/article/details/123887298
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QPaintEvent>



QT_BEGIN_NAMESPACE


namespace Ui { class MainWindow; }
QT_END_NAMESPACE

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();
    virtual void paintEvent(QPaintEvent *);
    QImage* img = nullptr;

    enum myEnum{
        one,
        two,
        three,
    };
    Q_ENUM(myEnum); //Q_ENUM不能在全局使用，只能在类内 或者 namespace中使用(namespace使用Q_ENUM_NS)
                    //使用Q_ENUM申明之后，该枚举就可以被QMetaEnum管理
private:
    Ui::MainWindow *ui;
};
#endif // MAINWINDOW_H




//for starf study

#include "mainwindow.h"
#include "ui_mainwindow.h"

#include<QDebug>
#include<QMetaEnum>


//QMetaEnum类提供有关枚举器的元数据。



MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    QMetaEnum metaEnum = QMetaEnum::fromType<MainWindow::myEnum>();//返回与模板参数中的类型对应的QMetaEnum。枚举需要用Q_enum声明。
    qDebug()<<metaEnum.enumName(); //“myEnum”
    qDebug()<<metaEnum.isFlag();  //如果此枚举数用作标志，则返回true；否则返回false。 当用作标志时，可以使用OR运算符组合枚举数。
    qDebug()<<metaEnum.isScoped();//如果此枚举数声明为C++11枚举类，则返回true；否则返回false。
    qDebug()<<metaEnum.isValid(); //如果此枚举有效（有名称），则返回true；否则返回false。
    qDebug()<<metaEnum.key(2); //返回具有给定索引的键，如果不存在这样的键，则返回0。 "three"
     qDebug()<<metaEnum.keyCount(); //返回键个数
     qDebug()<<metaEnum.keyToValue("two");//返回给定枚举键的整数值，如果未定义键，则返回-1。
     qDebug()<<metaEnum.keysToValue("two|three");//返回使用OR运算符组合键的值，如果未定义键，则返回-1。请注意，键中的字符串必须以“|”分隔。
    qDebug()<<metaEnum.name(); //返回枚举名称  myEnum
    qDebug()<<metaEnum.scope(); //返回此枚举数在其中声明的范围。即所属类/namespace的名字  MainWindow
    qDebug()<<metaEnum.value(2); //返回具有给定索引的值；如果没有这样的值，则返回-1。  2
    qDebug()<<metaEnum.valueToKey(2); //根据键返回名称  three
    qDebug()<<metaEnum.valueToKeys(1|2); // 返回表示给定值的由“|”分隔键组成的字节数组。 "two|three"
}

MainWindow::~MainWindow()
{
    delete ui;
}


