# vnpy.working

compile on linux：

环境ubantu 14.04 LTS 英文版 64位系统 ctpapi v6.3.5 linux64位（必须按以下顺序处理，且自己编译安装，不要用apt-get）
1、python2.7.11安装 ./configure --enable-unicode = ucs4 需在配置是使用前面的命令
2、编译boost、pyqt4（先安装sip）使用默认编译选项
3、编译ctpmd,使用makefile编译，却掉头文件和.cpp文件里的stdafx.h，把strcpy_s改为strcpy，并去掉第二个参数，把thostmduserapi.so改为libthostmduserapi.so，makefile文件的连接一定要加-lboost_python -lboost_thread
-lboost_python -lthostmduserapi，此处不知道应该-l××的，可以用ldd vnctpmd.so查看依赖包
4、编译ctptd，却掉头文件和.cpp文件里的stdafx.h，把strcpy_s改为strcpy，并去掉第二个参数，把cpp文件里面的有OptionValue的行去掉，把thosttraderapi.so改为libthosttraderapi.so，makefile文件的连接一定要加-lboost_python -lboost_thread
-lboost_python -lthosttraderapi，此处不知道应该-l××的，可以用ldd vnctpmd.so查看依赖包
5、libthostmduserapi.so，libthosttraderapi.so，执行export LD_LIBRARY_PATH=前面两个文件所在位置

附makefile内容只是我的特定，需要自己修改或自己学习makefile撰写方法

vnctpmd.so:vnctpmd.o 
	g++ vnctpmd.o -o vnctpmd.so -shared -fPIC -I/usr/local/include/python2.7 -I/usr/local/include -I/home/peer/redblack/RBCTP -L/usr/lib/python2.7 -L/usr/local/lib -L/home/peer/redblack/RBCTP -lboost_python -lboost_thread -lthostmduserapi
vnctpmd.o:  
	g++ -c vnctpmd.cpp -o vnctpmd.o -fPIC -I/usr/local/include/python2.7 -I/usr/local/include  -I/home/peer/redblack/RBCTP
  
clean:  
	rm -rf vnctpmd.o 
	rm -rf vnctpmd.so  
