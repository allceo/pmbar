Golang 如何在windows平台下使用LiteIDE交叉编译linux执行程序
=====================

在Windows平台使用LiteIDE编译golang程序为*.exe可执行文件，但是我们有时候需要编译linux下可执行的程序怎么办，我们在LiteIDE下加载交叉编译环境，编译报错go build runtime: linux/amd64 must be bootstrapped using make.bat，我们如何解决这个问题呢？
 
一、在windows环境下使用交叉编译，需要编译工具GCC，推荐使用MinGW:
---------
http://sourceforge.net/projects/mingw/files/Installer/mingw-get-inst/mingw-get-inst-20120426/mingw-get-inst-20120426.exe/download

安装完成后运行MinGW Installation Mannger

选择安装mingw32-ggc-g++

安装完成后设置环境变量，系统环境变量PATH中添加C:\MinGW\bin（安装目录）

二、在golang安装目录下C:\Go\src目录下新建cc.bat文件，内容如下：
---------
    set CGO_ENABLED=0
    ::x86
    set GOARCH=386
    set GOOS=windows
    call make.bat --no-clean
      
    set GOOS=linux
    call make.bat --no-clean
      
    set GOOS=freebsd
    call make.bat --no-clean
      
    set GOOS=darwin
    call make.bat --no-clean
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
      
    ::x64
    set GOARCH=amd64
    set GOOS=linux
    call make.bat --no-clean
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
      
    ::arm
    set GOARCH=arm
    set GOOS=linux
    call make.bat --no-clean
    ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
      
    set GOARCH=386
    set GOOS=windows
    go get github.com/nsf/gocode
    pause
完了之后，双击运行批处理文件

三、打开LiteIDE x21选择交叉编译环境
---------
然后编译程序，在源文件目录下发现linux可执行文件可以成功生成了。