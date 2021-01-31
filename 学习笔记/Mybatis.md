# Mybatis/Mybatis-plus

## 关于Mybatis执行流程

[Mybatis的执行流程](https://blog.csdn.net/qq_38270106/article/details/93398694)

## 注解

### 1.设置实体类主键自增

```java
@TableId(value = "",type = IdType.AUTO) //mp  如果实体类主键和数据库对应表中主键名一致，无需value，反之需要value来进行指定,type：指定主键策略
```

### 2.为什么要用@Param

[相关文档](https://blog.csdn.net/sinat_33010325/article/details/84261662)

当调用接口：

```java
public List<User> selectUserInIDs(List<Integer> ids,String name);
```

userMapper.xml的书写应该为：

```xml
<select id="selectUserInIDs" resultType="User">
		select * from user where id in 
		<foreach collection="param1" item="item" open="(" separator="," close=")">
			#{item}
		</foreach>
		and name = #{param2}
</select>
```

mybatis会自动将多个不同类型的参数改成param1，param2...

或者采用Mybatis的Annotation（@Param）：

```java
public List<User> selectUserInIDs(@Param("ids")List<Integer> ids,@Param("user")User user);
```

```xml
<select id="selectUserInIDs" resultType="User">
		select * from user 
		<if test="null != ids">
		where id in 
		<foreach collection="ids" item="item" open="(" separator="," close=")">
			#{item}
		</foreach>
		and name = #{user.name}
		</if>
</select>
```



### 3.匹配实体类与数据库表名

```java
@TableName(value = "tbl_employee") //mp	如果实体类类名和数据库对应表表名一致，无需value，反之需要value来进行指定
```

### 4.匹配实体类属性与表中字段

```java
@TableField(value = "last_name") //mp	如果实体类属性名和数据库对应表表中字段一致，无需value，反之需要value来进行指定
@TableField(exist = false) //mp 在执行插入操作时，如果表中某一属性没有与之对应的字段，可以在属性上添加该注解，false表示忽略该属性
```

## yml配置（mp）

```yml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mp?characterEncoding=utf-8&serverTimezone=GMT%2B8
    username: root
    password: root
mybatis-plus:
  configuration:
    map-underscore-to-camel-case: true    //mp 驼峰命名（可以不用写，mp自动配置）
  global-config:
    id-type: 0      //全局配置实体类主键自增
    table-prefix: tbl_    //全局配置添加实体类前缀
```

## BaseMapper中各别方法（mp）

```java
@Mapper
public interface UserMapper extends BaseMapper<Users> {
}
```

### 1.更新

- updateById/updateAllColumnById

  调用updateById这个方法，会自动执行非空判断;updateAllColumnById不会自动执行非空判断，如果修改时某属性未set到实体类中，修改后数据库该表中对应属性为null

### 2.查询

- selectById通过id查询

- selectOne通过多个条件查询一个结果

```java
@Test
void queryOne(){
    Employee employee1=new Employee();
    employee1.setId(7);
    employee1.setAge(17);
    Employee employee = employeeMapper.selectOne(employee1);
    System.out.println(employee);
}
```

```java
Employee(id=7, lastName=ww, email=ww@163.com, gender=0, age=17, salary=null)
```

- queryBatchIds通过多个id进行查询

```java
void queryBatchIds(){
    List<Integer> ids=new ArrayList<>();
    ids.add(1);
    ids.add(2);
    ids.add(3);
    ids.add(5);

    List<Employee> employees = employeeMapper.selectBatchIds(ids);

    for (Employee employee:
         employees) {
        System.out.println(employee);
    }

}
```

```java
Employee(id=1, lastName=Tom, email=111@163.com, gender=1, age=22, salary=null)
Employee(id=2, lastName=Mary, email=Mary@163.com, gender=0, age=23, salary=null)
Employee(id=3, lastName=Jerry, email=Jerry@163.com, gender=1, age=20, salary=null)
Employee(id=5, lastName=zs, email=12111@163.com, gender=1, age=18, salary=null)
```

- selectByMap通过多个条件查询一或多条数据

```java
@Test
void queryBatch(){

    Map<String,Object> map=new HashMap<>();
    map.put("last_name","ww");
    map.put("age",17);

    List<Employee> employees = employeeMapper.selectByMap(map);

    for (Employee employee:
         employees) {
        System.out.println(employee);
    }

}
```

```java
Employee(id=7, lastName=ww, email=ww@163.com, gender=0, age=17, salary=null)
Employee(id=9, lastName=ww, email=ww@163.com, gender=0, age=17, salary=null)
Employee(id=10, lastName=ww, email=ww@163.com, gender=0, age=17, salary=null)
Employee(id=11, lastName=ww, email=ww@163.com, gender=0, age=17, salary=null)
Employee(id=12, lastName=ww, email=ww@163.com, gender=0, age=17, salary=null)
Employee(id=13, lastName=ww, email=ww@163.com, gender=0, age=17, salary=null)
Employee(id=14, lastName=ww, email=ww@163.com, gender=0, age=17, salary=null)
Employee(id=15, lastName=ww, email=ww@163.com, gender=0, age=17, salary=null)
```

- 分页查询

```java
@Test
void pageQuery(){
    List<Employee> employees = employeeMapper.selectPage(new Page<>(3, 2), null);

    for (Employee employee:
         employees) {
        System.out.println(employee);
    }

}
```

```java
Employee(id=5, lastName=zs, email=12111@163.com, gender=1, age=18, salary=null)
Employee(id=6, lastName=ls, email=ls@163.com, gender=0, age=17, salary=null)
```

### 3.删除

- deleteById根据id删除

- deleteByMap根据条件删除

  ```java
  void testCommonDelete(){
      Map<String, Object> map = new HashMap<>();
      map.put("last_name","ww");
      map.put("email","ww@163.com");
      Integer integer = employeeMapper.deleteByMap(map);
      System.out.println(integer);
  }
  ```

- testDeleteBatchById根据一或多个id进行批量删除

  ```JAVA
  @Test
  void testDeleteBatchById(){
      List<Integer> integers=new ArrayList<>();
      integers.add(3);
      integers.add(5);
      employeeMapper.deleteBatchIds(integers);
  }
  ```

![image-20201127150913188](C:\Users\14064\AppData\Roaming\Typora\typora-user-images\image-20201127150913188.png)

## 项目中遇到的错误

- [x] validationquery not set

> druid包和mysql驱动包与mysql版本都不兼容,mysql版本是8.0.11，驱动要用8.0.11，druid版本要用1.1.10



## MyBatis mapper.xml

### [trim标签的使用](https://blog.csdn.net/wt_better/article/details/80992014)

### [resultMap标签用法总结](https://www.cnblogs.com/kenhome/p/7764398.html)

### [Mybatis中#{}和${}的区别](https://blog.csdn.net/siwuxie095/article/details/79190856)

### [Mybatis (ParameterType) 如何传递多个不同类型的参数](https://blog.csdn.net/jcmj123456/article/details/108818507)

### [MyBatis查询结果resultType返回值类型详细介绍](https://www.cnblogs.com/sea520/p/11743155.html)

### mybatis映射文件中不能使用">""<""&"问题

[相关文档](https://blog.csdn.net/denan_kong/article/details/62226617)

原因是 XML 文档中放置了一个类似 “<” 字符，那么这个文档会产生一个错误，这是因为解析器会把它解释为新元素的开始。因此你不能这样写：

```xml
<if test="ageStart = null and ageEnd != null ">
  and (o.patient_age <= #{ageEnd,jdbcType=TIMESTAMP} )
</if>
```

正确的写法是这样：

```xml
<if test="ageStart = null and ageEnd != null ">
   and (o.patient_age <![CDATA[ <= ]]>  #{ageEnd,jdbcType=TIMESTAMP} )
</if>
```

### 