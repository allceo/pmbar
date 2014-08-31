使用Polipo做代理
===================

1. 下载 Polipo，请至其官网
-------------

http://www.pps.jussieu.fr/~jch/software/polipo/

2. 配置 Polipo
-------------------

 1. 将 Polipo 解压到任意目录。
    
 2. 将 config.sample 文件更名为 config 即可，无扩展名。
    
 3. 编辑 config 文件。

> 	proxyAddress = "0.0.0.0"    # IPv4 only 	
> proxyPort = 8080
> 	#dnsNameServer = "8.8.8.8"
> logFile = "polipo.txt" 	
> logLevel = 0xFF
> 	disableServersList = false 	
> diskCacheRoot = "\"D:/use/Plugin/Internet/Proxy/polipo-20140107-win32/cache\"" 
> 	authCredentials = admin:123abc

logLevel
> L_ERROR 0x1
L_WARN 0x2
L_INFO 0x4
L_FORBIDDEN 0x8
L_UNCACHEABLE 0x10
L_SUPERSEDED 0x20
L_VARY 0x40

> D_SERVER_CONN 0x100
D_SERVER_REQ 0x200
D_CLIENT_CONN 0x400
D_CLIENT_REQ 0x800
D_ATOM_REFCOUNT 0x1000
D_REFCOUNT 0x2000
D_LOCK 0x4000
D_OBJECT 0x8000
D_OBJECT_DATA 0x10000
D_SERVER_OFFSET 0x20000
D_CLIENT_DATA 0x40000
D_DNS 0x80000
D_CHILD 0x100000
D_IO 0x200000

> LOGGING_MAX 0xFF

3.管理界面
-------------------
在浏览器中打入 http://localhost:port/polipo/ 便可以进入网页管理界面,比如 http://127.0.0.1:8080/polipo/
