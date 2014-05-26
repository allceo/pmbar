beanseye代码阅读笔记
=====================
[TOC]

 1. 命令行参数解释
---------

    //定义一个指针
    //flag.String(name string, value string, usage string)
    var conf *string = flag.String("conf", "conf/example.ini", "config path")
    
    func main() {
        //使用的时候，不加这句，是不会解释出传入的参数
        flag.Parse()
        //输出指针内容，需要加上一个星号
        fmt.Println(*conf)
    }

beanseyeread.exe -conf=sdfasdfasdf

 2. struct类型，定义两个接口在里面
---------
    type gzipResponseWriter struct {
        io.Writer
        http.ResponseWriter
    }

其实相当于

    io.Writer io.Writer

这里相当于将io.Writer中的所有字段和方法都添加到gzipResponseWriter中了，所以就可以直接用io.Writer里的方法。

    func (w gzipResponseWriter) Write(b []byte) (int, error) {
        return w.Writer.Write(b)
    }

 3. Comma-ok断言
---------
Go语言里面有一个语法，可以直接判断是否是该类型的变量： value, ok = element.(T)，这里value就是变量的值，ok是一个bool类型，element是interface变量，T是断言的类型。

如果element里面确实存储了T类型的数值，那么ok返回true，否则返回false。

    func timer(v interface{}) string {
        if v == nil {
            return ""
        }
        t := v.(uint64)
        switch {
        case t > 3600*24*2:
            return fmt.Sprintf("%d day", t/3600/24)
        case t > 3600*2:
            return fmt.Sprintf("%d hour", t/3600)
        case t > 60*2:
            return fmt.Sprintf("%d min", t/60)
        default:
            return fmt.Sprintf("%d sec", t)
        }
        return ""
    }

我感觉这个空的接口类型可以存储任意类型数值，当他作为函数参数时，就有点像C#中的泛型。
还可以根据传入参数的类型进行判断。

    func sizer(v interface{}) string {
        var n float64
        switch i := v.(type) {
        case int:
            n = float64(i)
        case uint:
            n = float64(i)
        case int64:
            n = float64(i)
        case uint64:
            n = float64(i)
        case float64:
            n = float64(i)
        case float32:
            n = float64(i)
        default:
            return "0"
        }
        if math.IsInf(n, 0) {
            return "Inf"
        }
        unit := 0
        var units = []string{"", "K", "M", "G", "T", "P"}
        for n > 1024.0 {
            n /= 1024
            unit += 1
        }
        s := fmt.Sprintf("%2.1f", n)
        if strings.HasSuffix(s, ".0") || len(s) >= 4 {
            s = s[:len(s)-2]
        }
        return s + units[unit]
    }


 4. range函数的含义是在一个数组中遍历每一个值，返回该值的下标值和此处的实际值。
----------------------------------------

     for i, num := range nums {
            if num == 3 {
                fmt.Println("index:", i)
            }
        }

再看一个例子，s+=n相当于s=s+n

    func sum(l interface{}) uint64 {
        if li, ok := l.([]uint64); ok {
            s := uint64(0)
            for _, n := range li {
                s += n
            }
            return s
        }
        return 0
    }
    
    kvs := map[string]string{"a": "apple", "b": "banana"}
        for k, v := range kvs {
            fmt.Printf("%s -> %s\n", k, v)
        }

 5. map结构是一个key-value的hash结构。key可以是除了func类型，array,slice,map类型之外的类型。
---------------------------------------------------------------

    m:=map[string]string{}
    m["key1"] = "val1"

map结构和slice是一样的，是一个指针。赋值的时候是将指针复制给新的变量

    m := map[string]string{"key1":"val1"}
    fmt.Println(m)
    m["key2"] = "val2"
    fmt.Println(m)
    p := m["key1"]
    fmt.Println(p)
    delete(m, "key1")
    fmt.Println(m)

 6. go语言的模板，text/template包
----------------------

    import (
        "os"
        "text/template"
    )
    
    type Inventory struct {
        Material string
        Count    uint
    }
    
    func main() {
        sweaters := Inventory{"wool", 17} 
        muban := "{{.Count}} items are made of {{.Material}}"
        tmpl, err := template.New("test").Parse(muban)  //建立一个模板
        if err != nil {   
                panic(err)
        }   
        err = tmpl.Execute(os.Stdout, sweaters) //将struct与模板合成，合成结果放到os.Stdout里
        if err != nil {
                panic(err)
        }   
    
    }

从文件中读取模板

    tmpl, err := template.ParseFiles("mb.txt")  //建立一个模板

模板套用模板

    muban1 := `hi, {{template "M2"}},
    hi, {{template "M3"}}
    `
    muban2 := "我是模板2，{{template "M3"}}"
    muban3 := "ha我是模板3ha!"
    
    tmpl, err := template.New("M1").Parse(muban1)
    tmpl.New("M2").Parse(muban2)
    tmpl.New("M3").Parse(muban3)
    
    err = tmpl.Execute(os.Stdout, nil)

模板中可以带语法

    const letter = `Dear {{.Name}},
    {{if .Attended}}It was a pleasure to see you at the wedding.
    如果Attended是true的话，这句是第二行{{else}}It is a shame you couldn't make it to the wedding.
    如果Attended是false的话，这句是第二行{{end}}
    {{with .Gift}}Thank you for the lovely {{.}}.
    {{end}}
    Best wishes,
    Josie
    `

 7. Go中的make和new的区别
---------------
make用于内建类型（map、slice 和channel）的内存分配。new用于各种类型的内存分配。

内建函数new本质上说跟其它语言中的同名函数功能一样：new(T)分配了零值填充的T类型的内存空间，并且返回其地址，即一个*T类型的值。用Go的术语说，它返回了一个指针，指向新分配的类型T的零值。有一点非常重要：new返回指针。

内建函数make(T, args)与new(T)有着不同的功能，make只能创建slice、map和channel，并且返回一个有初始值(非零)的T类型，而不是*T。

本质来讲，导致这三个类型有所不同的原因是指向数据结构的引用在使用前必须被初始化。例如，一个slice，是一个包含指向数据（内部array）的指针、长度和容量的三项描述符；在这些项目被初始化之前，slice为nil。对于slice、map和channel来说，make初始化了内部的数据结构，填充适当的值。make返回初始化后的（非零）值。

make 是 引用类型 初始化的方法。

 8. Go的异常处理 defer, panic, recover
-----------------------------
Go中可以抛出一个panic的异常，然后在defer中通过recover捕获这个异常，然后正常处理。

    package main
    
    import "fmt"
    
    func main(){
        defer func(){ // 必须要先声明defer，否则不能捕获到panic异常
            fmt.Println("c")
            if err:=recover();err!=nil{
                fmt.Println(err) // 这里的err其实就是panic传入的内容，55
            }
            fmt.Println("d")
        }()
        f()
    }
    
    func f(){
        fmt.Println("a")
        panic(55)
        fmt.Println("b")
        fmt.Println("f")
    }

输出结果：
a
c
55
d
exit code 0, process exited normally.

 9. runtime.GOMAXPROCS(runtime.NumCPU())
------------------------------------

 10. 字符串转数字
------
strconv包中

    start, _ := strconv.Atoi(s[p-1 : p])
    
11.Gob编码
--------
gob是Golang包自带的一个数据结构序列化的编码/解码工具。编码使用Encoder，解码使用Decoder。一种典型的应用场景就是RPC(remote procedure calls)。

gob和json的pack之类的方法一样，由发送端使用Encoder对数据结构进行编码。在接收端收到消息之后，接收端使用Decoder将序列化的数据变化成本地变量。

Encode是将结构体传递过来，但是Decode的函数参数却是一个pointer！

    func Serialize(d interface{}) (r []byte, err error) {
    	buffer := new(bytes.Buffer)
    	enc := gob.NewEncoder(buffer)
    	err = enc.Encode(d)
    	if err != nil {
    		return nil, err
    	}
    	return buffer.Bytes(), nil
    }

    func DeSerializeNew(d []byte, t interface{}) {
    	buffer := new(bytes.Buffer)
    	buffer = bytes.NewBuffer(d)
    	dec := gob.NewDecoder(buffer)
    	dec.Decode(t)
    }
使用：

    type person struct {
    	Name string
    	Age  int
    }
    
    var msg person       // P现在就是person类型的变量了
    msg.Name = "Astaxie" // 赋值"Astaxie"给P的name属性.
    msg.Age = 25          // 赋值"25"给变量P的age属性
    
    //向服务器发送加密的数据
    r, _ := Serialize(msg)
    n, err := conn.Write(r)
    if err != nil {
    	println("Write Buffer Error:", err.Error())
    	break
    }
    
    //从服务器端接收返回数据并解密
    n, err = conn.Read(buf)
    if err != nil {
    	println("Read Buffer Error:", err.Error())
    	break
    }
    revmsg := new(person)
    DeSerialize(buf[0:n], &revmsg)
    fmt.Println("服务端返回：")
    fmt.Println(revmsg.Name)
    fmt.Println(revmsg.Age)