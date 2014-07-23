使用Apache Benchmark给自己的网站进行测试
=====================

官方说明文档
http://httpd.apache.org/docs/2.0/programs/ab.html

下边介绍ab最简单的使用方法

    ab www.baidu.com/
    
这就是ab最简单的测试命令，这一命令显示了百度页面的相关数据。

    Benchmarking www.baidu.com (be patient).....done
    Server Software:        BWS/1.0
    Server Hostname:        www.baidu.com
    Server Port:            80
    Document Path:          /
    Document Length:        11006 bytes
    Concurrency Level:      1
    Time taken for tests:   0.237059 seconds
    Complete requests:      1
    Failed requests:        0
    Write errors:           0
    Total transferred:      11550 bytes
    HTML transferred:       11006 bytes
    Requests per second:    4.22 [#/sec] (mean)
    Time per request:       237.059 [ms] (mean)
    Time per request:       237.059 [ms] (mean, across all concurrent requests)
    Transfer rate:          46.40 [Kbytes/sec] received
    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:       77   77   0.0     77      77
    Processing:   160  160   0.0    160     160
    Waiting:       82   82   0.0     82      82
    Total:        237  237   0.0    237      23
    
上边的数据中，HTML transferred，Requests per second，Time per request是我们需要重点关注的，根据这些数据，我们能大概了解Web服务器的性能水平。

接下来给出相关字段说明：

字段 | 说明
--- | ---
Server Software | 服务器系统
Server Hostname | 服务器域名
Server Port | 服务器端口
Document Path | 访问的路径
Document Length | 访问的文件大小
Concurrency Level | 并发请求数，可以理解为同一时间的访问人数
Time taken fortests | 响应时间
Complete requests | 总共响应次数
Failed requests | 失败的请求次数
Write errors | 失败的写入次数
Total transferred | 传输的总数据量
HTML transferred | HTML页面大小
Requests per second | 每秒支持多少人访问
Time per request | 满足一个请求花费的总时间
Time per request | 满足所有并发请求中的一个请求花费的总时间
Transfer rate | 平均每秒收到的字节
--- | ---

最后的数据包括Connect,Processing,Waiting,Total字段。这些数据能大致说明测试过程中所需要的时间。其实我们可以只看Total字段中的min，max两列数据值，这两个值分别显示了测试过程中，花费时间最短和最长的时间。

ab命令还有很多可选参数，但常用的其实就下边三个：

参数 | 功能解释
--- | ---
-n | 设置ab命令模拟请求的总次数
-c | 设置ab命令模拟请求的并发数
-t | 设置ab命令模拟请求的时间
-k | 设置ab命令允许1个http会话响应多个请求
--- | ---

这三个参数的用法如下：

1）使用ab命令加上“-n”参数模拟1个用户访问百度总共5次

    ab -n 5 www.baidu.com/
2）使用ab命令加上“-n”与“-c”参数模拟5个用户同时访问百度总共9次

    ab -c 5 -n 9 www.baidu.com/
3）使用ab命令加上“-c”与“-t”参数模拟5个用户同时访问百度总共9秒

    ab -c 5 -t 9 www.baidu.com/
4）使用ab命令加上“-c”与“-t”附加“-k”参数模拟5个用户同时访问百度总共9秒,百度会打开5个并发连接，从而减少web服务器创建新链接所花费的时间。

    ab -c 5 -t 9 -k www.baidu.com/
使用ab命令的时候，有几点点要说明：

1）ab命令必须指定要访问的文件，如果没指定，那必须得在域名的结尾加上一个反斜杠，例如

    ab www.baidu.com
得改写为

    ab www.baidu.com/
2）ab命令可能会由于目标web服务器做了相应的过滤处理，导致在某些情况下收不到任何数据，这个时候可以使用“-H”参数，来模拟成浏览器发送请求。例如：

模拟成Chrome浏览器向百度发送1个请求

    ab -H "Mozilla/5.0 (Windows; U; Windows NT 5.1; en-Us) AppleWeb Kit/534.2 (KHTML, like Gecko) Chrome/6.0.447.0 Safari/534.2" www.baidu.com/
3）最后，要注意的就是，在使用ab命令测试服务器时，千万要小心，并且要限制对服务器发出的请求数量，希望大家理性使用这些压力测试工具，我们都不希望任何一台正常的服务器陷入不必要的麻烦。

更多的可选参数如下：

参数 | 功能解释
--- | ---
-A | 采用base64编码向服务器提供身份验证信息，用法: -A 用户名:密码
-C | cookie信息，用法: -C mo2g=a
-d | 不显示pecentiles served table
-e | 保存基准测试结果为csv格式的文件
-g | 保存基准测试结果为gunplot或TSV格式的文件
-h | 显示ab可选参数列表
-H | 采用字段值的方式发送头信息和请求
-i  | 发送HEAD请求，默认发送GET请求
-p  | 通过POST发送数据，用法： -p blog=a&mo2g=b
-P  | 采用base64编码向服务器提供身份验证信息，用法: -A 用户名:密码
-q  | 执行多余100个请求时隐藏掉进度输出
-s  | 使用Https协议发送请求，默认使用Http
-S  | 隐藏中位数和标准偏差值
-v  | -v 2 及以上将打印警告和信息，-v 3 打印http响应码，-v 4 打印头信息
-V  | 显示ab工具的版本号
-w  | 采用HTML表格打印结果
-x  | HTML标签属性，使用 -w 参数时，将放置在`<table>`标签中
-X  | 设置代理服务器，用法 -X 192.168.1.1:80
-y  | HTML标签属性，使用 -w 参数时，将放置在`<tr>`标签中
-z  | HTML标签属性，使用 -w 参数时，将放置在`<td>`标签中
--- | ---