# Java

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
```

### 2.相关文档

[2.1字符串切割相关文档](https://blog.csdn.net/julystroy/article/details/86475381)

> 字符串切割以 [ 作为切割点时 需要 \\\\ 否则报错

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