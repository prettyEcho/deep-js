> BY 张建成([prettyEcho@github](https://github.com/prettyEcho))
> 
>  除非另行注明，页面上所有内容采用知识共享-署名（[CC BY 2.5 AU](https://creativecommons.org/licenses/by/2.5/au/deed.zh)）协议共享

<p style="color: rgb(253,201,11);" align="center">🐬🐬 欢迎评论和star 🐳🐳</p>

写这篇文章时的心情是十分忐忑的💢，因为对于我们今天的主角：**闭包**，很多小伙伴都写过关于它的文章，相信大家也读过不少，那些文章到底有没有把JS中这个近似神话的东西讲清楚，说实话，真的有，但为数不多。

写这篇文章的初衷：**让所有看到这篇文章的小伙伴都彻彻底底的理解闭包 => 提高JS水平 => 能够写出更高质量的JS代码。**

开文之所以说心情是忐忑的，就是怕达不到我写该文的初衷，但是我有信心同时我也会努力的完成我的目标。如行文中有丝毫误人子弟的陈述，欢迎大家指正，在这感激不尽。 🙏🙏🙏

我们开始吧：

相信众多JS的lovers都听说过这句话：**闭包很重要但是很难理解**。

我起初也是这么觉得，但是当我努力学习了JS的一些深层的原理以后我倒觉得闭包并不是那么不好理解，反倒是让我感觉出一种很美。当我彻底理解闭包的那一刹那，心中油然产生一种愉悦感，就像**"酒酣尚醉，花未全开"**那种美景一样。

### 拨开闭包神秘的面纱

我们先看一个闭包的例子：

```
function foo() {
    let a = 2;

    function bar() {
        console.log( a );
    }

    return bar;
}

let baz = foo();

baz();
```

大家肯定都写过类似的代码，相信很多小伙伴也知道这段代码应用了闭包，but, Why does the closure be generated and Where is closure?

来，我们慢慢分析：

首先必须知道闭包是什么，才能分析出闭包为什么产生和闭包到底在哪？

**当一个函数能够记住并访问到其所在的词法作用域及作用域链，特别强调是在其定义的作用域外进行的访问，此时该函数和其上层执行上下文共同构成闭包。**

需要明确的几点：

1. 闭包一定是函数对象（[wintercn大大的闭包考证](https://github.com/wintercn/blog/issues/3)）
2. 闭包和词法作用域，作用域链，垃圾回收机制息息相关
3. 当函数一定是在其定义的作用域外进行的访问时，才产生闭包
4. 闭包是由该函数和其上层执行上下文共同构成（这点稍后我会说明）

闭包是什么，我们说清楚了，下面我们看下闭包是如何产生的。

接下来，我默认你已经读过我之前的两篇文章 [原来JavaScript内部是这样运行的](https://github.com/prettyEcho/deep-js/issues/1) 和 [彻底搞懂JavaScript作用域](https://github.com/prettyEcho/deep-js/issues/3) , 建议先进行阅读理解JS执行机制和作用域等相关知识，再理解闭包，否则可能会理解的不透彻。

现在我假设JS引擎执行到这行代码

`let baz = foo();`

此时，JS的作用域气泡是这样的：

<p align="center">
	<img src="https://user-images.githubusercontent.com/22290721/38412730-97a3fd6e-39bc-11e8-9a53-208d71ca98eb.png" alt="closure" width="60%">
</p>


这个时候foo函数已经执行完，JS的垃圾回收机制应该会自动将其标记为"离开环境",等待回收机制下次执行，将其内存进行释放（标记清除）。

但是，**我们仔细看图中粉色的箭头，我们将bar的引用指向baz，正是这种引用赋值，阻止了垃圾回收机制将foo进行回收，从而导致bar的整条作用域链都被保存下来**。

接下来，**`baz()`执行，bar进入执行栈，闭包（foo）形成**，此时bar中依旧可以访问到其父作用域气泡中的变量a。

这样说可能不是很清晰，接下来我们借助chrome的调试工具看下闭包产生的过程。

当JS引擎执行到这行代码`let baz = foo();`时：

<p align="center">
	<img src="https://user-images.githubusercontent.com/22290721/38487982-9301fd1a-3c14-11e8-8238-57321d84eb05.jpg" alt="closure" width="80%">
</p>

图中所示，`let baz = foo();`已经执行完，即将执行`baz();`，此时Call Stack中只有全局上下文。

接下来`baz();`执行：

<p align="center">
	<img src="https://user-images.githubusercontent.com/22290721/38488086-e0d4349a-3c14-11e8-84af-ea05e7546a43.jpg" alt="closure" width="80%">
</p>

我们可以看到，此时bar进入Call Stack中，并且Closure(foo)形成。


针对上面我提到的几点进行下说明：
 
1. 上述第二点（闭包和词法作用域，作用域链，垃圾回收机制息息相关）大家应该都清楚了
2. 上述第三点，当函数baz执行时，闭包才生成
3. 上述第四点，闭包是foo，并不是bar，很多书（《you dont know JavaScript》《JavaScript高级程序设计》）中，都强调保存下来的引用，即上例中的bar是闭包，而chrome认为被保存下来的封闭空间foo是闭包，针对这点我赞同chrome的判断（仅为自己的理解，如有不同意见，欢迎来讨论）

### 闭包的艺术性

我相信这个世界上最美的事物往往就存在我们身边，通常它并不是那么神秘，那么不可见，只是我们缺少了一双发现美的眼睛。

生活中，我们抽出一段时间放慢脚步，细细品味我们所过的每一分每一秒，会收获到生活给我们的另一层乐趣。

闭包也一样，它不是很神秘，反而是在我们的程序中随处可见，当我们静下心来，品味闭包的味道，发现它散发出一种艺术的美，朴实、精巧又不失优雅。

<p align="center">
	<img src="https://user-images.githubusercontent.com/22290721/38449808-25cd072c-3a47-11e8-9fce-d122cdd788fe.jpg" alt="closure" width="60%">
</p>

细想，在我们作用域气泡模型中，作用域链让我们的内部bar气泡能够"看到"外面的世界，而闭包则让我们的外部作用域能够"关注到"内部的情况成为可能。可见，只要我们愿意，**内心世界和外面世界是可以相通的**。

### 闭包的应用的注意事项

闭包，在JS中绝对是一个高贵的存在，它让很多不可能实现的代码成为可能，但是物虽好，也要合理使用，不然不但不能达到我们想要的效果，有的时候可能还会适得其反。

* 内存泄漏（Memory Leak)

	JavaScript分配给Web浏览器的可用内存数量通常比分配给桌面应用程序的少，这样做主要是防止JavaScript的网页耗尽全部系统内存而导致系统崩溃。
	
	因此，要想使页面具有更好的性能，就必须确保页面占用最少的内存资源，也就是说，我们应该保证执行代码只保存有用的数据，一旦数据不再有用，我们就应该让垃圾回收机制对其进行回收，释放内存。
	
	我们现在都知道了闭包阻止了垃圾回收机制对变量进行回收，因此变量会永远存在内存中，即使当变量不再被使用时，这样会造成内存泄漏，会严重影响页面的性能。因此当变量对象不再适用时，我们要将其释放。
	
	我们拿上面代码举例：
	
	```
	function foo() {
	    let a = 2;
	
	    function bar() {
	        console.log( a );
	    }
	
	    return bar;
	}
	
	let baz = foo();
	
	baz(); //baz指向的对象会永远存在堆内存中
	
	baz = null; //如果baz不再使用，将其指向的对象释放
	
	```
	
	关于内存泄漏，推荐 [阮一峰老师博客](http://www.ruanyifeng.com/blog/2017/04/memory-leak.html)。
	
### 闭包的应用

1. 模块

	一个模块应该具有私有属性、私有方法和公有属性、公有方法。

	而闭包能很好的将模块的公有属性、方法暴露出来。
	
	```
	var myModule = (function (window, undefined) {
		let name = "echo";
		
		function getName() {
			return name;
		}
		
		return {
			name,
			getName
		}
	})(window);
	
	console.log( myModule.name ); // echo
	console.log( myModule.getName() ); // echo
	```
	
	"return"关键字将对象引用导出赋值给myModule，从而应用到闭包。

2. 延时器(setTimeout)、计数器(setInterval)

	这里简单写一个常见的关于闭包的面试题。
	
	```
	for( var i = 0; i < 5; i++ ) {
		setTimeout(() => {
			console.log( i );
		}, 1000 * i)
	}
	```
	
	答案大家都知道：**每秒钟输出一个5，一共输出5次**。
	
	那么如何做到**每秒钟输出一个数，以此为0，1，2，3，4**呢？
	
	我们这里只介绍闭包的解决方法，其他类似块作用域等等的解决方法，我们这里不讨论。
	
	```
	for( var i = 0; i < 5; i++ ) {
		((j) => {
			setTimeout(() => {
				console.log( j );
			}, 1000 * j)
		})(i)	
	}
	```
	
	"setTimeout"方法里应用了闭包，使其内部能够记住每次循环所在的词法作用域和作用域链。
	
	由于setTimeout中的回调函数会在当前任务队列的尾部进行执行，因此上面第一个例子中每次循环中的setTimeout回调函数记住的i的值是for循环作用域中的值，此时都是5，而第二个例子记住的i的数为setTimeout的父级作用域自执行函数中的j的值，依次为0，1，2，3，4。	

3. 监听器

	```
	var oDiv = document.querySeletor("#div");
	
	oDiv.addEventListener('click', function() {
		console.log( oDiv.id );
	})
	```

=- 关于闭包，我觉得我说清楚了，你看清楚了吗？留言告诉我吧 -=

**🤗 如果你觉得写的还不是很烂，请关注我的 [github](https://github.com/prettyEcho/deep-js) 吧，让我们一起成长。。。**     
