# jQuery

## 应用场景

### 1.动态给表格添加数据

**1.1相关文档**

[相关文档](https://www.cnblogs.com/picaso/archive/2012/10/08/2715564.html)

### 2.获取当前点击元素的内容<This的用法>

**2.1相关文档**

[相关文档](https://blog.csdn.net/qq_36769100/article/details/79173323)

```javascript
alert($(this).parent("td").siblings("input").eq(0).val());
```