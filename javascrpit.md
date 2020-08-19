##Javascript


函数调用模式 : 当一个函数并非一个对象的属性的时候，那么它就是被当作一个函数调用的。通过函数模式调用，此时的this被绑定到全局对象。



```    
var myObject = {
       value: 0,
      increment: function (inc) {
      this.value += typeof inc === 'number' ? inc : 1;
      }
};





// 给 myObject 增加一个 double 方法。
myObject.double = function ( ) {
  var that = this; //解决方法(此时的this指向的是my object这个对象)
  var helper = function ( ) {
    that.value = add(that.value, that.value);
  };
  helper( ); //以函数的形式调用 helper。
};


```



构造器调用模式：
javascript是基于原型继承的语言。这意味着对象可以直接从其他对象继承属性。
如果在一个函数前面带上new来调用，那么背地里将会创建一个连接到该函数的prototype成员的新对象，同时this会被绑定到那个新对象上。


```
// 创建一个名为 Quo 的构造器函数。它构造一个带有 status 属性的对象。

var Quo = function(string){
  this.status = string;
}

// 给 Quo 的所有实例提供一个名为 get_status 的公共方法。
Quo.prototype.get_status = function(){
  return this.status;
};

var myQuo = new Quo("confused")
doucument.writeln(myQuo.get_status());//输出结果“confused”


```

按照约定，它们保存在以大写格式命名的变量里。



apply调用模式
因为JavaScript是一门函数式的面向对象编程语言，所以函数可以拥有方法。

apply方法让我们构建一个参数数组传递给调用函数。它也允许我们选择this的值。 apply方法接收两个参数，第1个是要绑定给this的值，第2个就是一个参数数组。

```
// 构造一个包含两个数字的数组，并将它们相加。
var array = [3, 4];
var sum = add.apply(null, array); // sum 值为7。

// 构造一个包含 status 成员的对象。
var statusObject = {
  status: 'A-OK'
};

// statusObject 并没有继承自 Quo.prototype，但我们可以在 statusObject 上调
// 用get_status 方法，尽管 statusObject 并没有一个名为 get_status 的方法。

var status = Quo.prototype.get_status.apply(statusObject);
// status 值为 'A-OK'。

```


##参数 Argument

当函数被调用时，会得到一个“免费”配送的参数，那就是arguments数组。函数可以通过此参数访问所有它被调用时传递给它的参数列表，包括那些没有被分配给函数声明时 定义的形式参数的多余参数。这使得编写一个无须指定参数个数的函数成为可能


```
// 构造一个将大量的值相加的函数。

var sum = function ( ) {
var i, sum = 0;
for (i = 0; i < arguments.length; i += 1) {
    sum += arguments[i];
}
return sum;
};
document.writeln(sum(4, 8, 15, 16, 23, 42)); // 108


```
因为语言的一个设计错误，arguments并不是一个真正的数组。它只是一个“类似数组 (array-like)”的对象。arguments拥有一个length属性，但它没有任何数组的方法。我们将 在本章结尾看到这个设计错误导致的后果。



返回 return
当一个函数被调用时，它从第一个语句开始执行，并在遇到关闭函数体的}时结束。 然后函数把控制权交还给调用该函数的程序。
return语句可用来使函数提前返回。当return被执行时，函数立即返回而不再执行余下 的语句。
一个函数总是会返回一个值。如果没有指定返回值，则返回undefined。
如果函数调用时在前面加上了new前缀，且返回值不是一个对象，则返回this(该新 对象)。


##回调函数(callbacks)

函数使得对不连续事件的处理变得更容易。例如，假定有这么一个序列，由用户交互 行为触发，向服务器发送请求，最终显示服务器的响应。最自然的写法可能会是这样的:
  request = prepare_the_request( );
  response = send_request_synchronously(request); display(response);
这种方式的问题在于，网络上的同步请求会导致客户端进入假死状态。如果网络传输 或服务器很慢，响应会慢到让人不可接受。
更好的方式是发起异步请求，提供一个当服务器的响应到达时随即触发的回调函数。 异步函数立即返回，这样客户端就不会被阻塞。
  request = prepare_the_request( ); send_request_asynchronously(request, function (response) {
            display(response);
         });
我们传递一个函数作为参数给send_request_asynchronously函数，一旦接收到响应，它 就会被调用。


##模块Module
我们可以使用函数和闭包来构造模块。模块是一个提供接口却隐藏状态与实现的函数 或对象
https://blog.csdn.net/ying422/article/details/45152967


##柯里化 curry （多个函数降级处理）
https://juejin.im/post/6844903603266650125

##记忆 memoization


函数可以将先前操作的结果记录在某个对象里，从而避免无谓的重复运算。这种优化 被称为记忆 (7) (memoization )。

 ```
 实现原理：生成一个闭包记忆数组，储存结果可以隐藏在闭包中
    var fibonacci = function ( ) {
       var memo = [0, 1];
       var fib = function (n) {
          var result = memo[n];
          if (typeof result !== 'number') {
result = fib(n - 1) + fib(n - 2);
             memo[n] = result;
          }
          return result;
       };
       return fib;
    }( );



//现在，我们可以使用memoizer函数来定义fibonacci函数，提供其初始的memo数组和 formula函数:


var fibonacci = memoizer([0, 1], function (recur, n) {
   return recur (n - 1) + recur (n - 2);
});




 ```


##继承
Javascript是一门基于原型的语言，这意味着对象直接从其他对象继承。
####对象说明符
在这种情况下，如果我们在编写构造器时让它接受一个简单的对象说明符，可能会更 加友好。那个对象包含了将要构建的对象规格说明。所以，与其这样写:
```
    var myObject = maker(f, l, m, c, s);

    //改良后
    var myObject = maker({
       first: f,
       middle: m,
       last: l,
       state: s,
       city: c
       });

```

##正则表达式


END
这本书的基本部分看完了 剩下的糟粕的没有看但占据了很大但篇幅
接下来打算看看express框架并且实战



promise的then方法传入的是一个callback函数的参数，
1.callback函数为匿名函数的时候，call函数内部的this指向window，需要对函数bind this
2.callback函数为箭头函数的时候，callback函数的this会指向他的上层




bind绑定
bind绑定是软绑定 new的优先级比bind高
bind() 方法创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。
bind可以在function后直接调用

apply（this,arguments(数组等)） call(this,arg1,arg2,arg3)参数李贝奥
