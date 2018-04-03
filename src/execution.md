<p style="color: rgb(253,201,11);" align="center">🐬🐬 欢迎评论和star 🐳🐳</p>

天气渐渐转暖了，树渐渐露出了枝芽，小河也欢快的向前流着，感觉大地充满了生命力，真的好开心 😄😄😄

附上美图一张
<p style="text-align: center">
<img src="../img/spring.jpeg" alt="AST" style="width: 20%">
</p>

小伙伴们，我们也出来活动活动筋骨，迎接我们2018年的春天。

今天我们说说JS执行流程，现在我们先暂且不考虑异步的情况。

装备好了吗，go...

## 编译阶段 

#### 词法分析（Lexing）
> 这个过程会将由字符组成的字符串分解成(对编程语言来说)有意义的代码块，这些代 码块被称为词法单元(token)。

简单举个例子：c = b - a 转换为
- NAME "c"
- EQUALS
- NAME "a"
- MINUS
- NAME "b"
- SEMICOLON

#### 语法分析（Parsing）
> 这个过程是将词法单元流(数组)转换成一个由元素逐级嵌套所组成的代表了程序语法 结构的树。这个树被称为“抽象语法树”(Abstract Syntax Tree，AST)。

AST大概是下面的样子：

<img src="./img/Parsing.jpg" alt="AST">

#### 生成可执行代码

> 将 AST 转换为可执行代码的过程称被称为代码生成。

## 执行阶段 

接下来，我们以一个简单例子进行分析。

```
var a = 2;

function bar() {
    var b = 2;

    function foo() {
        var c = 2;
    }

    foo();
}

bar();
```

**1. JS引擎创建一个全局对象（Global Object)**

这个对象全局只存在一份，它的属性在任何地方都可以访问，它的存在伴随着应用程序的整个生命周期。全局对象在创建时，将Math,String,Date,document 等常用的JS对象作为其属性。由于这个全局对象不能通过名字直接访问，因此还有另外一个属性window,并将window指向了自身，这样就可以通过window访问这个全局对象了。用伪代码模拟全局对象的大体结构如下：

```
//创建一个全局对象
var globalObject = {
    Math:{},
    String:{},
    Date:{},
    document:{}, //DOM操作
    ...
    window:this //让window属性指向了自身
}
```

**2. JS引擎会创建一个执行环境栈（Execution Context Stack)**

* 栈
提到栈，小伙伴们都知道，是一种类似羽毛球筒存储羽毛球的数据结构，采用**先进后出，后进先出**的特点。

<p style="text-align: center">
<img src="./img/data-instructure.jpg" alt="AST" style="width: 20%">
</p>

上图中的羽毛球1一定是先放入栈中，然后是羽毛球2，以此类推，而出栈时，一定是羽毛球5先拿出来，然后是羽毛球4，以此类推，这种方式和栈存取数据的方式如出一辙。

* 堆
堆数据类型类似与书架。书虽然也整齐的存放在书架上，但是我们只要知道书的名字，我们就可以很方便的取出我们想要的书。

好了好了，扯远了。我们接着往下说，在这只需知道执行环境栈是怎样存取数据的就行。

**3. 创建全局执行上下文（Execution Context）**

到这你可能会问，上下文是个啥玩意？

是啊，上下文是个什么鬼啊？

上下文不是玩意，也不是什么鬼。

执行上下文可以理解为当前代码的**执行环境**。JS所有代码都会在自己的上下文环境下运行。

说到上下文，你可能会有这样的疑惑：上下文不就是作用域吗？

老铁，我肯定的告诉你，**上下文不是作用域**。的确，在JS里，这还真是个很难区分的东东。不过现在我还不能马上道出他们的区别，因为作用域的知识，我们还没有涉及，👉[彻底搞懂JavaScript作用域](https://github.com/prettyEcho/deep-js/issues/2)，通过这篇文章，你将彻彻底底了解关于作用域的一切。

那在JS中会有几种执行环境呢？

大概有3种：
* 全局环境：JavaScript代码运行起来会首先进入该环境
* 函数环境：当函数被调用执行时，会进入当前函数中执行代码
* eval、with（不建议使用，可忽略）

因此在一个JavaScript程序中，必定会产生多个执行上下文。

go on...

**4. 全局上下文推入执行环境栈底**

**5. 代码开始从上往下执行，这里我们暂且不谈标识符处理，当代码执行到bar(),生成bar执行上下文，推入栈中**

**6. 代码执行到foo(),生成foo执行上下文，推入栈中**

**7. foo()执行完，foo执行上下文出栈**

**8. bar()执行完，bar执行上下文出栈**

**9. 全局上下文执行上下文出栈**

我们用图走一下js执行流程，是这样的：

<p style="text-align: center">
<img src="./img/flow.jpg" alt="AST" style="width: 80%">
</p>

小伙伴们，现在是不是对JS执行流程有了一个整体认识，下面我们来说点更有意思的。

## 上下文执行细节

我们先看整体了解下

<p style="text-align: center">
<img src="./img/context-detail.png" alt="AST" style="width: 80%">
</p>

### 创建阶段

#### 1. 创建变量对象（Variable Object)

创建变量对象，依次经历了以下几个步骤

1. 建立arguments对象。检测当前上下文参数，建立该对对象下的属性及属性值。（这里提一下，函数的参数是按值传递，我知道你是知道的）
2. 检测关键词function函数声明。检测当前上下文中的函数声明，并挂载到变量对象上，其值是函数对象的引用。
3. 检测var变量声明。检测当前上下文中的var声明，并赋值为undefined;如遇到同名var声明的变量，则会默认覆盖;如遇到同名函数声明，则默认忽略，这也就体现了函数声明的优先级要高于var声明。谁的大哥还是得分清的，哈哈。。。

##### 变量提升

看到这，我觉得你对变量提升具体是什么以及如何实现的应该了解的一清二楚了。

是不是呢？

我们来一道题测试下

```
function foo() {
    console.log(a);
    console.log(baz);

    var a = 'inner';
    var baz = 1;

    function baz() {}
}

foo();
```

第一处是undefined，第二处是[Function: baz]，是不是很简单？

下面我用代码简单模拟下上面的过程

```
function foo() {
    function baz() {}

    var a = undefined;

    console.log(a);
    a = 'inner';

    console.log(baz);
}

foo();

```

变量对象大概是这样的

```
 VO(foo) = {
     arguments: {},
     baz: <foo reference>,  // 表示foo的地址引用
     a: undefined
 }
```

#### 2. 确定作用域链

作用域链是由当前作用域与上层一系列父级作用域组成，作用域的头部永远是当前作用域，尾部永远是全局作用域。作用域链保证了当前上下文对其有权访问的变量的有序访问。

我们先简单了解下，详细的我们会在[彻底搞懂JavaScript作用域](https://github.com/prettyEcho/deep-js/issues/2)中谈到。

```
var a = 1;
function foo() {
    function baz() {
        console.log( a ); 
    }

    baz();
}

foo(); // 1
```

上面的对于我们来说很简单，是吧？没错这就是作用域链的应用。

我们简单模拟下

```
EC(foo) = {
    VO(foo): {...}, //省略
    ScopeChain: [VO(foo), window],
    this: 
}

EC(baz) = {
    VO(baz): {...}, //省略
    ScopeChain: [VO(baz), VO(foo), window],
    this: 
}
```

#### 3. 确定this指向

谈到this，大家是不是感到很兴奋，平时写代码时，被这家户整的晕头转向的，这回我们终于可以揭开this的神秘面纱了，搞清楚它在JS到底是怎样的存在，不过客官别着急，我们这里先不介绍this，因为关于this的内容太多了，我们得慢慢去品味它，这里先记住，**this是在执行上下文创建阶段确定的**。

this传送门👇👇👇

[this真是一个淘气鬼](https://github.com/prettyEcho/deep-js/issues/3)

#### 全局上下文

全局上下文有些特殊，其变量对象永远是window，this永远指向window（在浏览器中，Node中不是）。

即

```
EC(global) = {
    VO: window,
    ScopeChain: {},
    this: window
}
```

### 执行阶段

在执行阶段变量对象(Variable Object)变为活动对象(Active Object)。
VO => AO

> 这样，如果再面试的时候被问到变量对象和活动对象有什么区别，就又可以自如的应答了，他们其实都是同一个对象，只是处于执行上下文的不同生命周期。不过只有处于函数调用栈栈顶的执行上下文中的变量对象，才会变成活动对象。

执行阶段JS引擎会进行**变量赋值**、**函数引用**、**执行其他代码**，执行顺序取决于代码的位置。

我们就聊到这吧。

喝杯茶，

休息一下。


