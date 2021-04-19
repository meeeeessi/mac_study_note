# Java

## JUC

### 1.volatile特性

[1.1相关文档](https://baijiahao.baidu.com/s?id=1659385012966624306&wfr=spider&for=pc)

[1.2相关文档](https://zhuanlan.zhihu.com/p/56540493)

[1.3相关文档](https://www.cnblogs.com/iou123lg/p/9280639.html)

[1.4双重检索(DCL)的思考: 为什么要加volatile?](https://blog.csdn.net/weixin_37505014/article/details/97302345)



### 2.CAS

[2.1为什么volatile++不是原子性的？](https://blog.csdn.net/dm_vincent/article/details/79604716)

[2.2从volatile说到,i++原子操作,线程安全问题](https://blog.csdn.net/zbw18297786698/article/details/53420780?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&dist_request_id=1328689.9942.16165789878441805&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control)

[2.3Java Unsafe类中的getAndAddInt方法解释](https://blog.csdn.net/littlehaes/article/details/105156443)



### 3.ConcurrentHashMap

[3.1为什么Hashtable, ConcurrentHashMap 的 key和value 不能为null（并发角度分析）](https://blog.csdn.net/csdnlijingran/article/details/89892581?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&dist_request_id=1328690.143.16165055845852659&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control)

[3.2为什么Hashtable ConcurrentHashmap不支持key或者value为null](https://blog.csdn.net/CSDN_586/article/details/80939862?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-9.control&dist_request_id=1328690.143.16165055845852659&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-9.control)

[3.3ConcurrentHashMap是如何实现线程安全的](https://blog.csdn.net/qq_41737716/article/details/90549847)



### 4.CopyOnWriteArrayList/Set

[4.1相关文档](https://blog.csdn.net/weixin_42146366/article/details/88016527)

CopyOnWriteArrayList这是一个ArrayList的线程安全的变体，其原理大概可以通俗的理解为:初始化的时候只有一个容器，很长一段时间，这个容器数据、数量等没有发生变化的时候，大家(多个线程)，都是读取(假设这段时间里只发生读取的操作)同一个容器中的数据，所以这样大家读到的数据都是唯一、一致、安全的，但是后来有人往里面增加了一个数据，这个时候CopyOnWriteArrayList 底层实现添加的原理是先copy出一个容器(可以简称副本)，再往新的容器里添加这个新的数据，最后把新的容器的引用地址赋值给了之前那个旧的的容器地址，但是在添加这个数据的期间，其他线程如果要去读取数据，仍然是读取到旧的容器里的数据。



## 集合

### 1.HashMap

[1.1什么是HashMap（一）初始容量 （16）和 负载因子（0.75）put get](https://blog.csdn.net/dagedeshu/article/details/100686999)

[1.2HashMap的初始容量机制及扩容机制](https://blog.csdn.net/Apeopl/article/details/88918576?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161614611616780265458603%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161614611616780265458603&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v1~rank_blog_v1-2-88918576.pc_v1_rank_blog_v1&utm_term=HashMap)

[1.3HashMap初始容量为什么是2的n次幂及扩容为什么是2倍的形式](https://blog.csdn.net/Apeopl/article/details/88935422?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522161614611616780265458603%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=161614611616780265458603&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v1~rank_blog_v1-1-88935422.pc_v1_rank_blog_v1&utm_term=HashMap)

HashMap/Set无序无重复

ArrayList有序有重复



### 2.快速失败(fail-fast)和安全失败(fail-safe)的区别是什么？

[2.1相关文档](https://blog.csdn.net/tb9125256/article/details/80892859)



### 3.数组和链表的时间复杂度

[3.1相关文档](https://www.cnblogs.com/Stephen-Qin/p/13150087.html)







## 请解释一下extends 和super 泛型限定符

### [1.相关文档](https://blog.csdn.net/u014138443/article/details/90581974)



## 是否可以在static环境中访问非static变量？

### [1.相关文档](https://blog.csdn.net/snail_xinl/article/details/53427572)



## 什么是值传递和引用传递

### [1.相关文档1](https://blog.csdn.net/qq_40574571/article/details/90765349)

### [2.相关文档2](https://blog.csdn.net/bjweimengshu/article/details/79799485?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)



## equals()和HashCode()深入理解

### [1.相关文档1](https://blog.csdn.net/wonad12/article/details/78958411)

### [2.相关文档2](https://www.cnblogs.com/whgk/p/6071617.html)



## java中普通代码块，构造代码块，静态代码块区别及代码示例

### [1.相关文档](https://www.cnblogs.com/sophine/p/3531282.html)



## java基本类型封装类和原始类型的区别及好处

### [1.相关文档](https://blog.csdn.net/qq_33223761/article/details/82876032)



## 泛型

### 1.相关文档

[1.1什么是泛型](https://www.cnblogs.com/lwbqqyumidi/p/3837629.html)

[1.2Java泛型在实际开发中的应用](https://www.cnblogs.com/ldh-better/p/7127308.html#_label1)



## Serializable

### 1.相关文档

[1.1什么是Serializable](https://baijiahao.baidu.com/s?id=1633305649182361563&wfr=spider&for=pc)



## 字符串切割

### 1.最常见

```java
String[] split = data.split("\n");
if (split != null && split.length != 0) {
    for (int i = 0; i < split.length-1; i++) {
        String[] split1 = split[i].split(":");
        ···
    }
}
//or
String str3 = str1.substring(4,7)

//查询某个字符第一次出现的下标
String str ="1-2-3-4-5";
int i = str.indexOf("-", 0);
System.out.println(i);

//查询某个字符最后一次出现的下标
expression2.lastIndexOf(")")
```

### 2.相关文档

[2.1字符串切割相关文档](https://blog.csdn.net/julystroy/article/details/86475381)

> 字符串切割以 [ 作为切割点时 需要 \\\\ 否则报错



## 字符串包含

```java
String expression2="100-600-40*(3-1)";
System.out.println(expression2.indexOf("("));
//结果：11
```



## 获取时间及格式转换

### 1.获取当前时间并将时间转化为指定格式

```java
@Test
void contextLoads() {
    SimpleDateFormat simpleDateFormat=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    Date date=new Date();
    String format = simpleDateFormat.format(date.getTime());
    
    //将时间转化为Timestamp格式
    Timestamp ts = Timestamp.valueOf(format);
    //注：String的类型必须形如： yyyy-mm-dd hh:mm:ss[.f...] 这样的格zhi式，中括号表示可选，否则报错。如果String为其他格式，可考虑重新解析下字符串后再转换。
    System.out.println(ts+"-----------------------");
}
```

### 2.相关文档

[2.1关于SimpleDateFormat的使用](https://www.cnblogs.com/gaopeng527/p/4760394.html)

### 3.String格式的数据转化为Date格式

```java

formatter = new SimpleDateFormat( "yyyy-MM-dd ");
String s = "2011-07-09 "; 
Date date = formatter.parse(s);
```



## 内部类

### 1.定义

**1.1将一个类定义在另一个给类里面或者方法里面，这样的类就被称为内部类。内部类可以分为四种:**成员内部类、局部内部类、匿名内部类、静态内部类

### 2.相关文档

**2.1相关文档**

[相关文档](https://www.cnblogs.com/dearcabbage/p/10609838.html)

### 3.内部类的好处

**3.1完善了Java多继承机制，由于每一个内部类都可以独立的继承接口或类，所以无论外部类是否继承或实现了某个类或接口，对于内部类没有影响。**

**3.2方便写事件驱动程序。**



## Comparable接口

1：所有可以 “排序” 的类都实现了java.lang.Comparable接口，Comparable接口中只有一个方法。
2：public int compareTo(Object obj) ;
该方法：
返回 0 表示 this == obj
返回整数表示 this > obj
返回负数表示 this < obj
3:实现了 Comparable 接口的类通过实现 comparaTo 方法从而确定该类对象的排序方式。

[相关文档](https://blog.csdn.net/nvd11/article/details/27393445?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&dist_request_id=57e19771-1058-4aa5-945e-4607e883a365&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)



## Socket连接

服务端：ServerSocket提供的实例 ServerSocket server = new ServerSocket(端口号) 

客户端：Socket提供的实例 Socket client = new Socket(IP地址，端口号)



## Java运算符

[Java &、&&、|、||、^、<<、>>、~、>>>等运算符](https://cloud.tencent.com/developer/article/1338265)