## script

### 1. 代码分割
程序中js代码是顺序执行的，但是却靠`<script>`进行分块的，每一块的执行代码是独立的，出现错误不会影响到其他块内的代码执行，但是变量用var定义时，变量会存在window下，因此可以通用，例如：

```
<script>
    var i = 0;
    console.log(a);  //报错
</script>
<script>
    console.log(i);	 // 0
</script>
```

### 2. 属性

* async: 并行下载js文件，只对外部脚本有效，不能保证脚本的执行顺序
* defer: 延迟执行js文件，待浏览器遇到`</html>`，再解析js，提升加载速度，只对外部脚本有效。`defer='defer'`

### 3. noscript
* 使用`<noscript>`元素可以指定在不支持脚本的浏览器中显示的替代内容。但在启用了脚本的情况下，浏览器不会显示`<noscript>`元素中的任何内容。

## doctype
* 文档模式：包含混杂模式和标准模式。
* 当html未声明`<!DOCTYPE>`时，会默认采取混杂模式。



