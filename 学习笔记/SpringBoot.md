# SpringBoot

## 注释

### 1.@RequestParam

通过前端标签中的id使得函数操作的参数与输入的内容一一对应，但是使用了@RequestParam注解使得我们可以对参数进行自定义命名。

[相关文档](https://blog.csdn.net/weixin_41902922/article/details/107939455)

[相关文档2](https://movie.blog.csdn.net/article/details/84195043?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.control)

```java
@RequestMapping("/login")
    public void login(@RequestParam("email") String e,@RequestParam("password") String p){
        System.out.println(e);
        System.out.println(p);
    }
```

### 2.@RequestParam和@RequestBody的区别

**1.问题描述**

   由于项目是前后端分离，因此后台使用的是spring boot，做成微服务，只暴露接口。接口设计风格为restful的风格，在get请求下，后台接收参数的注解为RequestBody时会报错；在 post请求下，后台接收参数的注解为RequestParam时也会报错。

**2.问题原因**

   由于spring的RequestParam注解接收的参数是来自于requestHeader中，即请求头，也就是在url中，格式为xxx?username=123&password=456，而RequestBody注解接收的参数则是来自于requestBody中，即请求体中。

**3.解决方法**

   因此综上所述，如果为get请求时，后台接收参数的注解应该为RequestParam，如果为post请求时，则后台接收参数的注解就是为RequestBody。附上两个例子，截图如下：

**4.get请求**

![20180116102918320](/Users/wangjingyu/Library/Containers/com.tencent.xinWeChat/Data/Library/Application Support/com.tencent.xinWeChat/2.0b4.0.9/ea253dc0bbb565323d2c9a90af066684/Message/MessageTemp/2be3e0331b86f4ac24ee18e82178b335/File/wang/笔记/picture/20180116102918320.png)

**5.post请求**

![20180116102959715](/Users/wangjingyu/Library/Containers/com.tencent.xinWeChat/Data/Library/Application Support/com.tencent.xinWeChat/2.0b4.0.9/ea253dc0bbb565323d2c9a90af066684/Message/MessageTemp/2be3e0331b86f4ac24ee18e82178b335/File/wang/笔记/picture/20180116102959715.png)

[springboot服务端接口获取URL请求参数的几种方法](https://www.cnblogs.com/zhanglijun/p/9403483.html)

### 3.spring mvc 中redirect带参数跳转方法

![微信图片_20201222130135_WPS图片](E:\git_workspace\笔记\picture\微信图片_20201222130135_WPS图片.png)

使用RedirectAttributes的addAttribute方法传递参数会跟随在URL后面，如上代码即为http:/index.action?test=51gjie

### 4.@PathVariable

```java
@GetMapping("/countByMethod/{method}")
public AjaxResult selectEleTargetByMethod(@PathVariable String method) {
    return AjaxResult.success(eleTargetService.selectEleTargetByMethod(method));
}
```

### 5.Spring @Scheduled注解

**5.1.相关文档**

[相关文档](https://blog.csdn.net/jack_bob/article/details/78786740)

[相关文档2](https://blog.csdn.net/m0_37179470/article/details/81271607)

## Springboot集成WebSocket

### 1.相关文档

[1.1相关文档](https://zhengkai.blog.csdn.net/article/details/80275084?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.control)

### 2.WebSocket @ServerEndpoint注解说明

[2.1相关文档](https://www.cnblogs.com/zhuyeshen/p/12169949.html)

### 3.将接收信息转化为JSON形式

**3.1引入pom相关依赖**

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.47</version>
</dependency>
```

[3.2关于使用fastjson相关文档](https://www.runoob.com/w3cnote/fastjson-intro.html)

[3.3fastjson各种用法](https://blog.csdn.net/qq_41589166/article/details/80543084?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.control)

### 4.Springboot+websocket+定时器实现消息推送

**4.1.相关文档**

[1.1易懂篇](https://blog.csdn.net/u013565163/article/details/80659828?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3.control)

[1.2高深篇](https://blog.csdn.net/qq_35498538/article/details/82461695)

### 5.遇到的坑！

5.1<font color='red'>The remote endpoint was in state [TEXT_FULL_WRITING] which is an invalid sta</font>

[解决相关文档](https://blog.csdn.net/wy_xing/article/details/82744559)

