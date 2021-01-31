# FastJson

### 相关文档

[相关文档](https://blog.csdn.net/qq_41589166/article/details/80543084?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.control)

### 关于Json的日期格式转换

在打印日志或其他操作的时候，需要将某个对象转为jsonString，其中日期格式我们希望使用自定义的格式化后的格式，方便查看及后续处理

```java
Test test = new Test();
test.setDate(new Date());
JSON.toJSONStringWithDateFormat(test, "yyyy-MM-dd", SerializerFeature.WriteDateUseDateFormat);
```

```xml
反序列化能够自动识别如下日期格式：
ISO-8601日期格式
yyyy-MM-dd
yyyy-MM-dd HH:mm:ss
yyyy-MM-dd HH:mm:ss.SSS
```

