## js中this的指向

首先必须要说的是，**this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁**，**实际上this的最终指向的是那个调用它的对象**（这句话有些问题，后面会解释为什么会有问题，虽然网上大部分的文章都是这样说的，虽然在很多情况下那样去理解不会出什么问题，但是实际上那样理解是不准确的，所以在你理解this的时候会有种琢磨不透的感觉）

eg1：

```ruby
function a() {
	var user = 'pipi'；
    console.log(this.user); // undefined
    console.log(this);// window
};
a();
```

按照我们上面说的this最终指向的是调用它的对象，这里的函数a实际是被Window对象所点出来的，下面的代码就可以证明。

```ruby
function a() {
	var user = 'pipi';
	console.log(this.user); // undefined
	console.log(this); // window
}
window.a();
```

和上面代码一样，其实alert也是window的一个属性，也是window点出来的。

eg2:

```
var o = {
	user: 'pipi',
	fn: function() {
		console.log(this.user); // pipi
	}
}
o.fn();
```

这里的this指向的是对象o，因为你调用这个fn是通过o.fn()执行的，那自然指向就是对象o，这里再次强调一点，**this的指向在函数创建的时候是决定不了的，在调用的时候才能决定，谁调用的就指向谁**，一定要搞清楚这个。