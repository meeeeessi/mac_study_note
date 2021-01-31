# Thymeleaf

## yml相关配置

```java
  ###ThymeLeaf配置
  thymeleaf:
    #模板的模式，支持 HTML, XML TEXT JAVASCRIPT
    mode: HTML5
    #编码 可不用配置
    encoding: UTF-8
    #内容类别,可不用配置
    content-type: text/html
    #开发配置为false,避免修改模板还要重启服务器
    cache: false
    #配置模板路径，默认是templates，可以不用配置
    prefix: classpath:/templates/
    suffix: .html
```

## thymeleaf th:if表达式语法

**1.比较运算符**

gt：great than（大于）>
ge：great equal（大于等于）>=
eq：equal（等于）==
lt：less than（小于）<
le：less equal（小于等于）<=
ne：not equal（不等于）!=

**2.网页应用**

```html
<td class="td-status" th:if="${product.status} eq 1">
	<span class="label label-success radius">已上架</span>
</td>
```

```html
<a th:if="${product.status} ne 2 " class="ml-5"  href="javascript:;" title="发布秒杀">
	<i class="Hui-iconfont">&#xe603;</i>
</a>
```

