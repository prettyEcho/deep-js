> BY 张建成([prettyEcho@github](https://github.com/prettyEcho))
>
> 除非另行注明，页面上所有内容采用知识共享-署名（[CC BY 2.5 AU](https://creativecommons.org/licenses/by/2.5/au/deed.zh)）协议共享

<p style="color: rgb(253,201,11);" align="center">🐬🐬 欢迎评论和star 🐳🐳</p>

**先说个重要的题外话：[maoyanMovie](https://github.com/prettyEcho/maoyanMovie)是我的一个前后端分离的开源项目，该项目模仿猫眼电影移动端 APP（半模仿吧，我加了一些后端逻辑在里面，嘿嘿），涉及到前端、后端、数据库、部署一系列技术栈，对于前端来说，也算是一个全栈（暂不考虑安卓、iOS、AI）的实践吧。如果有哪位小伙伴感兴趣，给个 star🙏 ➡️ [maoyanMovie](https://github.com/prettyEcho/maoyanMovie)**

好了，还是言归正传吧。

如题，this 就是一个淘气鬼。

对此我想很多小伙伴都不会反对我，对吧？😘😘😘

在工作中我们经常被它搞得晕头转向的，一遇到 this，首先闭目自问:TND 指向谁。明明觉得它指向 a，怎么偏偏就指向了 b。真是无语。😓😓😓

就这种情况，我是不能忍的，小样，竟敢这样戏耍我们玩 JS 的人，看我不整蒙你。

于是决心写这样一篇文章，分析 this 到底是什么，所有 this 指向的场景以及如何使用 this，帮助大家彻彻底底的搞定 this，希望大家多多支持！！！

### 探个究竟

关于 this，我们放在嘴边的话就是：this 指向谁。很少有小伙伴会想到，**this 到底是什么**，**指向指的是什么**，以及**什么时候确定的指向**。

此时，给自己 30 秒钟的时间，想一想自己是否清楚我提的这 3 个问题。

如果觉得自己对这些问题很清楚，我觉得应该为自己小小的高兴一下。

好了，我来回答下。

* 是个啥

      	this是JavaScript里面定义的一个关键字，不能用做标识符。在函数中，是一个特殊对象。

* 指向是啥

      	指向也作“引用”。JS中对象存在堆中，“this指向谁”也就是：this存储谁的地址。也可以简单理解为某个函数名的别名（alias).

* 什么时候确定指向

      	[原来JavaScript内部是这样运行的](https://github.com/prettyEcho/deep-js/issues/1) 中我们讲到，this指向的确定是发生在执行上下文的创建阶段，也就是说在函数调用以前，this到底指向谁，谁也不知道，而且一旦确定，便不可更改。

### 变来变去不累吗

> 《JavaScript 高级程序设计》中说：this 引用的是函数据以执行的环境对象。因此正是由于执行的环境对象飘忽不定，才导致 this 引用的对象“变来变去”。

##### 一、函数调用

* 函数被 obj 所拥有

      	上面我们说到的两点，可以为我们确定this指向提供理论依据：

      	1. 函数执行的时候确定this指向 => this指向确定一定是在函数最终执行的位置
      	2. 	this引用的是函数据以执行的环境对象 => this指向和函数的拥有者有密切关系


    结论：**this指向函数最终调用的拥有者** （记住这句话能帮你解决一半以上的this问题）

    严格模式：

    ```
    // 函数被obj所拥有
    "use strict"

    var a = 10;

    var obj = {
        a: 20,
        fn() {
            console.log( this.a );
        }
    }

    obj.fn(); // 20 this -> obj

    ```

    非严格模式：

    ```
    // 函数被obj所拥有
    var a = 10;

    var obj = {
        a: 20,
        fn() {
            console.log( this.a );
        }
    }

    obj.fn(); // 20 this -> obj
    ```

* 独立调用

      	**1. 严格模式下，不管在哪独立调用this都指向undefined**

      	**2. 非严格模式下，不管在哪独立调用this都指向window**

      	严格模式：

      	```
      	// 独立调用

      	// demo1
      	"use strict"

      	var a = 10;

      	function foo() {
      	    var a = 20;

      	    console.log( this ); // this -> undefined
      	}

      	foo();

      	// demo2
      	"use strict"

      	var a = 10;

      	function fn() {
      	    var a = 20;
      	    console.log( this ); // this -> undefined
      	}

      	function foo() {
      	    var a = 30;
      	    fn();
      	}

      	foo();

      	```

      	非严格模式：

      	```
      	// 独立调用

      	// demo1
      	var a = 10;

      	function foo() {
      	    var a = 20;

      	    console.log( this.a );
      	}

      	foo(); // 10 this -> window

      	// demo2
      	var a = 10;

      	function fn() {
      	    var a = 20;
      	    console.log( this.a );
      	}

      	function foo() {
      	    var a = 30;
      	    fn();
      	}

      	foo(); // 10  this -> window
      	```

##### 二、非函数调用

* 全局对象中的 this

      	[原来JavaScript内部是这样运行的](https://github.com/prettyEcho/deep-js/issues/1) 中我们强调过：**全局对象中this指向自身window**

      	严格模式：

      	```
      	"use strict"

      	var a = 10;
      	console.log( this ); // this -> undefined

      	```
      	非严格模式：

      	```
      	var a = 10;
      	console.log( this.a ); // 10 this -> window

      	```

* 对象属性中的 this

      	**不论在严格还是非严格模式下，对象中属性的this都指向window**

      	严格模式：

      	```
      	"use strict"

      	var a = 10;

      	var obj = {
      	    a: 20,
      	    b: this.a + 10,
      	    fn: function() {
      	        console.log( this.b ); // 20 this -> window
      	    }
      	}

      	obj.fn();
      	```

      	非严格模式：

      	```
      	var a = 10;

      	var obj = {
      	    a: 20,
      	    b: this.a + 10,
      	    fn: function() {
      	        console.log( this.b ); // 20 this -> window
      	    }
      	}

      	obj.fn();
      	```

##### 三、 new 操作符下的 this

**new 把构造函数中的 this 指向新创建的实例对象**（如果不清楚可以去研究下 new 操作符具体都干了什么）
`function People(name) { this.name = name; } People.prototype.getName = function() { return this.name; } var person = new People('echo'); console.log( person.name ); // echo this -> person`

##### 四、特殊情况

* 定时器、计数器下的 this

      	**不论在严格还是非严格模式下，定时器、计数器下的this都指向window**

      	```
      	"use strict" (or none)

      	var a = 10;

      	function foo() {
      	    var a = 20;

      	    setTimeout(function() {
      	        var a = 30;
      	        console.log( this.a ); // 10 this -> window
      	    })
      	}

      	foo();
      	```

* DOM 中的 this

      	**不论在严格还是非严格模式下，DOM事件中的this都指向该DOM元素对象**

      	```
      	// HTML

      	<body>
      	    <div id="test" style="width: 100px; height: 100px; background-color: red;" onclick="console.log( this.id );"></div>
      	</body>

      	// JS

      	"use strict" (or none)

      	var a = 10;
      	var oDiv = document.querySelector('#div');

      	/*
      	oDiv.onclick = function() {
      	    var a = 20;
      	    console.log( this ); // DOM(div)
      	} */

      	/*
      	oDiv.addEventListener('click', function() {
      	    var a = 20;
      	    console.log( this ); // DOM(div)
      	}) */

      	```

##### 五、箭头函数中的 this

箭头函数中的 this 可就神奇了，来来，我们一起看下。准确的说，**箭头中并不存在 this 上下文**（真懒，嘿嘿，和我一样），它懒得自己生成 this 上下文，直接**捕获父级作用域中的 this**。但是懒也有懒的好处，因为在定义的时候我们就能确定箭头函数中的 this（词法作用域），而且也不会受到严格模式、call、apply、bind 的限制。说的什么鬼东西？行，我们代码分析。

```
var a = 10;
var obj = {
	a: 20,
        fn: () => {
	    var a = 30;
	    console.log( this.a );
	}
}

obj.fn();
```

箭头函数 fn 向上寻找 this，首先遇到 obj，发现它不能构成作用域，也不会存在 this，直接跳过，此时遇到全局 window，发现其中的 this，真好，拿过来，那么我们结果自然就是 10.
我们想让结果是 20，怎么办，继续看。

```
var a = 10;
var obj = {
	a: 20,
	fn: function() {
	    (() => {
	        var a = 30;
	        console.log( this.a );
	    })();
	}
}

obj.fn();
```

箭头函数开始向上寻找 this，遇到函数作用域 fn，发现其中有 this 上下文，今天真走运，刚出门就找到了，直接拿过来用，哈哈。所以自然而然箭头函数中的 this 就是函数 fn 中的 this，通过上面的知识，我们知道 fn 中的 this 指向 obj，因此自然而然输出 20。很简单是吧？那么你分析下，下面这题输出什么？

```
var a = 10;
var obj = {
	a: 20,
	fn: function() {
	    (() => {
	        var a = 30;
	        (() => {
	            var a = 40;
	            (() => {
	                console.log( this.a );
	            })()
	        })();
	    })();
	}
}

obj.fn();
```

如果你的结果是 20,那么恭喜你，箭头函数中的 this 问题，对你来说已经是小菜一碟了。

#### 总结

好累，this 指向会出现这么多种情况，谁记得住，别急，所有你能想到的我都为你准备好了。（请叫我雷锋）

<p align="center">
<img src="https://user-images.githubusercontent.com/22290721/38778241-36f852c4-40e9-11e8-9570-47b7166d8a52.png" alt="this指向总结" style="width: 20%">
</p>

### 你皮，任你皮

你不是淘气吗？你不是变来变去吗？

看我不找个绳子绑住你。

于是乎，JavaScript 给了我们三条绳子。

我们认识下它们吧。

##### 一、call、apply

这两条绳子都可以把 this 限制到固定的对象上，只是传递参数的方式不一样。

* call：fn.call(obj, arg1, arg2)
* apply: fn.apply(obj, [arg1, arg2])

继续看代码：

```
var a = 10;

function foo() {
	console.log( this.a );
}

var obj = {
	a: 20
}

foo.call(obj); // 20 this -> obj
foo.apply(obj); // 20 this -> obj
```

这样我们就把 this 硬生生的绑定到 obj 上了。

接下来看下两个函数的区别

```
function sum(a, b) {
    console.log(this.c + a + b);
}

var obj = {
    c: 10
}

sum.call(obj, 20, 30); // 60
sum.apply(obj, [20, 30]); // 60
```

简单说下我们实际应用

* 类数组->数组

```
function foo(a, b){
    let arr = [].slice.call(arguments);
    console.log( arr );
}

foo(1,2);
```

补充 2 种方法：

1.  `Array.from(arguments)`
2.  `[...arguments]`

* 继承

```
var People = function(name) {
    this.name = name;
}

var Programmer = function(name) {
    People.call(this, name)
}

Programmer.prototype.getName = function() {
    console.log( `${this.name} is a programmer!` );
}

var person = new Programmer('echo');

person.getName(); // echo is a programmer!
```

#### 二、bind

bind 方法只是把 this 绑定到固定对象上，不会传递参数。

```
var people = {
	name: 'echo',
	getName() {
		setTimeout(function(){
			console.log(this.name);
		}.bind(this), 1000)
	}
}

peopler.getName(); // echo
```

当然我们也可以通过声明一个临时变量来暂存 this，利用作用域链实现 this 的处理，这种方法很简单，这里不给出例子了。

### 唯一的不足

这篇文章我认为唯一不足的地方就是，我没有涉及到 this 在 Node 环境下的情况。起初我的确把 Node 环境考虑进来了，写着写着发现出现的情况太多了，为了不给大家营造一个十分混乱的局面，我决定把所有关于在 Node 中的情况放弃了。

在这深表抱歉 ！🙏🙏🙏

**🤗 如果你觉得写的还不错，请关注我的 [github](https://github.com/prettyEcho/deep-js) 吧，让我们一起成长。。。**
