# POM中各种依赖

> **注意版本问题**

#### mybatisplus

```java
<dependency>
			<groupId>com.baomidou</groupId>
			<artifactId>mybatis-plus-boot-starter</artifactId>
			<version>3.3.2</version>
</dependency>
```

#### druid

```java
<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.1.10</version>
		</dependency>
```

#### lombok

```java
<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
```

#### mysql

```java
<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>
```

### Jackson

```java
<!-- 导入Jackson：提供一些功能将转换成Java对象匹配JSON结构，反之亦然 -->
    
    <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.5</version>
    </dependency>

    <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.9.5</version>
    </dependency>

    <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-annotations</artifactId>
    <version>2.9.5</version>
    </dependency>
```

### Thymeleaf

```java
<!-- ThymeLeaf 依赖 -->
    <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
```

### Shiro

```java
<!--引入shiro整合springboot依赖-->
		<dependency>
			<groupId>org.apache.shiro</groupId>
			<artifactId>shiro-spring-boot-starter</artifactId>
			<version>1.5.3</version>
		</dependency>
```

### Thymeleaf整合Shiro

```java
		<dependency>
			<groupId>com.github.theborakompanioni</groupId>
			<artifactId>thymeleaf-extras-shiro</artifactId>
			<version>2.0.0</version>
		</dependency>
```





