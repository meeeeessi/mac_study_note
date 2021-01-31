# SpringSecurity

### 配置全局用户名

```java
#spring:
#  security:
#    user:
#      name: wang
#      password: wang
```

### 自定义设置登录页面不需要认证可以访问

1. 在配置类实现相关配置

```java
protected void configure(HttpSecurity httpSecurity) throws Exception{

    httpSecurity.formLogin() //自定义自己编写的自定义页面
        .loginPage("/login.html") //登录页面设置
        .loginProcessingUrl("/user/login") //设置访问路径
        .defaultSuccessUrl("/test/index").permitAll()//登录成功后跳转的路径
        .and().authorizeRequests()
            .antMatchers("/","/test/hello","/user/login").permitAll() //设置哪些路径可以直接访问，不需要认证
        	//当前登录用户，只有具备admins权限才可以访问这个路径
            //.antMatchers("/test/index").hasAuthority("admins")
            //针对多个权限
            //.antMatchers("/test/index").hasAnyAuthority("admins,manager")
                //.hasRole("sale") 默认将sale添加前缀ROLE_  hasAnyRole()和hasAnyAuthority作用相同
                .antMatchers("/test/index").hasRole("sale")
        .and().csrf().disable();//关闭csrf防护

}
```

2. 创建相关页面，controller

```java

```

### spring security中@PreAuthorize、@PostAuthorize、@PreFilter和@PostFilter四者的区别

[相关文档](https://blog.csdn.net/weixin_39220472/article/details/80873268)