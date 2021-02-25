# Java

## java基本类型封装类和原始类型的区别及好处

[相关文档](https://blog.csdn.net/qq_33223761/article/details/82876032)



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

