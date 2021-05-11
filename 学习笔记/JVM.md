# JVM

## 简介

**Java虚拟机就是二进制字节码的运行环境**

## JVM的整体结构（P34）

![截屏2021-04-22 上午9.42.18](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-04-22 上午9.42.18.png)

## Java代码的执行流程（P36）

![截屏2021-04-22 上午9.39.30](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-04-22 上午9.39.30.png)

> 1.java源码通过编译器生成字节码文件
>
> 2.字节码文件通过虚拟机的翻译字节码和JIT编译器转化为机器语言，交给操作系统

## JVM的架构模型（P38）

### 两种指令集架构

Java编译器输入的指令流基本上是一种基于**栈的指令集架构**，另一种指令集架构则是基于**寄存器的指令集架构。**

### 字节码文件反编译过程

```java
public static void main(String[] args) {
        int i=2;
  			int j=3;
  			int k=i+j;
}
```

反编译后：

```java
Code:
      stack=2, locals=4, args_size=1
         0: iconst_2//变量2入栈
         1: istore_1
         2: iconst_3//变量3入栈
         3: istore_2
         4: iload_1
         5: iload_2
         6: iadd    //变量2，3出栈相加
         7: istore_3//结果5出栈
         8: return
```

### 总结

**由于跨平台的设计，Java指令都是根据栈来设计的。**不同平台CPU架构不同，所以不能设计为基于寄存器的。

栈（相较寄存器）：跨平台性、指令集小、指令多；执行性能比寄存器差。

## JVM的生命周期（P43）

### 虚拟机的启动

Java虚拟机的启动是通过引导类加载器（bootstrap class loader）创建一个初始类（initial class）来完成的，这个类是由虚拟机的具体实现指定的。

### 虚拟机的执行

- 一个运行中的Java虚拟机有着一个清晰的任务：执行Java程序。

- 程序开始执行时他才运行，程序结束时他才停止。
- 执行一个Java程序时，真真正正在执行的是一个叫做Java虚拟机的进程。

### 虚拟机的退出

有如下几种情况：

- 程序正常执行。
- 程序在执行过程中遇到了异常或错误而终止异常。
- 由于操作系统出现错误导致Java虚拟机进程终止。
- 某线程调用Runtime类或System类的exit方法，或Runtime类的halt方法，并且Java安全管理器也允许这次exit或halt操作。

- 除此之外JNI（Java Native Interface）规范描述了JNI Invocation API来加载或卸载，Java虚拟机时，Java虚拟机的退出情况。

## JVM发展历程

### Sun的Classic VM

![截屏2021-04-22 下午8.36.54](/Users/wangjingyu/mac_study_note/学习笔记/picture/截屏2021-04-22 下午8.36.54.png)

初代Classic VM的Java虚拟机内部只提供解释器，效率低，会反复执行重复代码（如for循环或执行频率较高的代码）；出现JIT编译器后，将反复出现的代码（或执行频率较高的代码）编译成本地机器指令并标记为热点代码放在方法区的CodeCache缓存起来，下次执行代码就不需要逐行翻译。

注意：解释器和JIT（及时编译器）不能协同工作，二者选择一个执行即可。

举例：我想去某地，解释器就类似于步行，不需要等待直接就走，JIT类似于坐公交车，你需要公交车才能走，但是一旦坐上公交车你的速度就快于步行速度。一般采取解释器和JIT交叉使用更快捷。

### Sun的Exact VM

- Exact Memory Management（准确式内存管理）

- 具备现代高性能虚拟机的雏形
  - 采用编译器和解释器混合工作模式
  - 热点检测

- 生命周期较短，最终被HotSpot替代

### Sun的HotSpot VM

JDK8默认VM，也是绝大多数JDK默认的VM

- 名称中的HotSpot指的就是它的热点代码探测技术
  - 通过计数器找到最具编译价值代码
  - 触发即时编译或栈上替换
- 通过编译器与解释器协同工作，在最优化的程序响应时间与最佳执行性能中取得平衡

### BEA的JRockit