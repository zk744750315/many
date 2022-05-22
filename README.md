

# qt多窗口 的非模态响应：https://blog.csdn.net/u011555996/article/details/124460064


彻底解决Qt报错：无法定位程序输入点于动态链接库 更改环境变量中编译器所在的位置，用的编译器要往前挪，详细见：https://blog.csdn.net/qq_41709234/article/details/123410055



qt控件属性的详细介绍：https://blog.csdn.net/ClementCXL/article/details/118935444?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_ecpm_v1~rank_v31_ecpm-1-118935444-null-null.pc_agg_new_rank&utm_term=widget+%E5%92%8Cframe%E7%9A%84%E5%8C%BA%E5%88%AB&spm=1000.2123.3001.4430


有时候会遇到乱码，解决：
1 运行时设置为utf_8(  #pragma execution_character_set("utf-8")  ),然后在将运行时修改为utf_8，在main函数中添加语句：QTextCodec::setCodecForLocale(QTextCodec::codecForName("UTF-8"));
