# Shiro

[shiro+thymeleaf整合](https://blog.csdn.net/qq_34579313/article/details/82024058)

- shiro整合thymeleaf时关于static文件夹下的资源被拦截

  ```java
      map.put("/static/**", "anon");
      map.put("/*","authc");//authc请求这个资源需要认证和授权
  //   /**会拦截包括springboot模板的内容，/*对静态依然有效
  ```

  