go 语言重点知识
=====================
一、channel的使用
---------
在Go里，写多线程程序变的更简单了，比如，我们要自己手写实现个简单的数据库连接池，在Java里，我们需要一个数组来存放数据库连接，连接池的所有操作方法都要对其加上锁，以确保两个线程没有共用一个连接，连接池里没连接了或连接满了怎么办等。然而在Go里，我们只需要一个具有缓冲区的channel就行了：

    pool := make(chan mysql.Conn, size)
    conn := <-p.pool //从连接池取一个连接
    p.pool <- conn //在把连接放回连接池
就这三句简单的代码组成了实现一个完美支持多线程的数据库连接池类库的核心基石，剩下的就是对该类库功能上的完美封装与优化了。

二、map的使用
---------
Go语言的map，是将map这种复杂的数据结构做为语言的内置类型来实现的。

    oneMap[key] = value //直接赋值
    value,ok := oneMap[key] //取一个map的值与判断该key是否存在一体化完成
    for key,value := range oneMap {} //简单遍历一个map，取得key与其value
    delete(oneMap[key]) //删除一个key，不用担心该key不存在于map中而报错
单单以上四项，就不知给我的开发带来了多少的方便，而且增强了程序的健壮性。

三、json的使用
---------
由于项目中很多地方需要用到使用json文件做为程序的配置文件，因此，解析json数据也是经常做的事，Go语言提供给我们的json库能让我们很方便的解析自己想要的json数据，只要定义一个与json文件结构一样的struct或map就可以了。

    json.Unmarshal(jsonbytes, jsonObj) //jsonObj的结构要与数据本身格式一样
而且，json库还提供了MarshalJSON接口方法，外部程序只要实现了该接口，就可以自定义生成json数据的方式了。

四、格式化生成字符串
---------
在Go里，使用了fmt.Sprintf()方法，在方法里传入自己预先定义好的模版，然后按顺序传入自己想往模版里填的数据，就生成自己想要的格式化字符串了。这点尤其适用于我们生成具有固定格式的日志数据，比如：

    fmt.Sprintf("[%s] %s %s",time,ip,log)
就能格式化一条形式如：[2012-12-20] 127.0.0.1 LOG_CONTENT 的日志，很方便有木有。

五、自定义类型的使用
---------
有一次自己想实现一套类似这样的机制，就是所有方法名放在一个配置文件里，然后在运行时可以根据命令行指定方法名执行相应的实现方法，按以前的思路，是要实现一套工厂模式的代码，但在Go里，一个map就搞定了：


    type CreatorImpl func() ICreater
    
    var funcmap = map[string]CreatorImpl{"Method1": Method1Impl,
        "Method2":      Method2Impl}
    
    funcmap[builder]() //builder为外部传入的参数，执行指定的方法
    
    func Method1Impl() ICreater {
        //实现1
    }
    func Method2Impl() ICreater {
        //实现2
    }

将方法的声明定义为一个类型CreatorImpl，将CreatorImpl做为一个map的值类型生成一个map，实现一个类似工厂模式功能的代码就这么简单。

六、反射的使用
---------
反射是把双刃剑，在Go中，使用反射倒也没有觉得比其他语言有更优秀，但对于特定应用场景，适当的使用反射，能让我们的程序更优雅和简洁，附上一段我在项目中统一判断具有不同类型和结构的对象里的属性是否包含空串的代码：


    logvalue := reflect.ValueOf(log).Elem()
    
    islegal := true
    
    for i := 1; i < logvalue.NumField(); i++ {
        f := logvalue.Field(i)
        str := fmt.Sprintf("%v", f.Interface())
        islegal = (str != "")
        if !islegal {
            return islegal
        }
    }
