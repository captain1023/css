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

apply（this,arguments(数组等)） call(this,arg1,arg2,arg3)参数列表

今儿听了叮叮的演讲 还是向往阿里

javascript变量可以保存两种类型的值：基本类型和引用类型
  基本类型值在内存中占据固定大小的空间，因此被保存在栈内存中;
  引用类型的值是对象，保存在堆内存中;
  包含引用类型值的变量实际上包含的并不是对象本身，而是一个指向该对象的指针;
  从一个变量向另一个变量复制引用类型的值，复制的其实是指针，因此两个变量最终 都指向同一个对象;
  确定一个值是哪种基本类型可以使用typeof 操作符，而确定一个值是哪种引用类型 可以使用instanceof 操作符。

JavaScript执行环境
执行环境有全局执行环境(也称为全局环境)和函数执行环境之分;
每次进入一个新执行环境，都会创建一个用于搜索变量和函数的作用域链;
函数的局部环境不仅有权访问函数作用域中的变量，而且有权访问其包含(父)环
  境，乃至全局环境;
  变量的执行环境有助于确定应该何时释放内存。
  全局环境只能访问在全局环境中定义的变量和函数，而不能直接访问局部环境中的任
  何数据;


引用类型

一般来说，访问对象属性时使用的都是点表示法，这也是很多面向对象语言中通用的语 法。不过，在JavaScript也可以使用方括号表示法来访问对象的属性。在使用方括号语法 时，应该将要访问的属性以字符串的形式放在方括号中，如下面的例子所示。

```
alert(person["name"]); //"Nicholas" alert(person.name); //"Nicholas"
```
从功能上看，这两种访问对象属性的方法没有任何区别。但方括号语法的主要优点是可以 通过变量来访问属性，例如:
```
var propertyName = "name"; alert(person[propertyName]); //"Nicholas"
```

如果属性名中包含会导致语法错误的字符，或者属性名使用的是关键字或保留字，也可以 使用方括号表示法。例如:
```
person["first name"] = "Nicholas";
```
由于"first name" 中包含一个空格，所以不能使用点表示法来访问它。然而，属性名中 是可以包含非字母非数字的，这时候就可以使用方括号表示法来访问它们。
通常，除非必须使用变量来访问属性，否则我们建议使用点表示法。



5.5.2 函数声明与函数表达式
本节到目前为止，我们一直没有对函数声明和函数表达式加以区别。而实际上，解析器在 向执行环境中加载数据时，对函数声明和函数表达式并非一视同仁。解析器会率先读取函 数声明，并使其在执行任何代码之前可用(可以访问);至于函数表达式，则必须等到解 析器执行到它所在的代码行，才会真正被解释执行。请看下面的例子。
 ```
alert(sum(10,10));
function sum(num1, num2){
    return num1 + num2;
}
```

以上代码完全可以正常运行。因为在代码开始执行之前，解析器就已经通过一个名为函数 声明提升(function declaration hoisting)的过程，读取并将函数声明添加到执行环境中。 对代码求值时，JavaScript引擎在第一遍会声明函数并将它们放到源代码树的顶部。所 以，即使声明函数的代码在调用它的代码后面，JavaScript引擎也能把函数声明提升到顶 部。如果像下面例子所示的，把上面的函数声明改为等价的函数表达式，就会在执行期间 导致错误。
```
alert(sum(10,10));
var sum = function(num1, num2){
    return num1 + num2;
};
```

以上代码之所以会在运行期间产生错误，原因在于函数位于一个初始化语句中，而不是一 个函数声明。换句话说，在执行到函数所在的语句之前，变量sum 中不会保存有对函数的 引用;而且，由于第一行代码就会导致“unexpected identifier”(意外标识符)错误，实际 上也不会执行到下一行。
除了什么时候可以通过变量访问函数这一点区别之外，函数声明与函数表达式的语法其实 是等价的。
也可以同时使用函数声明和函数表达式，例如var sum = function sum(){} 。 不过，这种语法在Safari中会导致错误。



  面向对象的程序设计
  最简单方式就是创建一个Object 的实例，然后再为它添加属性和方法，如下所示。
  ```
    var person = new Object(); person.name = "Nicholas"; person.age = 29;
    person.job = "Software Engineer";
    person.sayName = function(){
      alert(this.name);
    };
  ```

  对象字面量创建对象
```
var person = {
  name: "Nicholas",
  age: 29,
  job: "Software Engineer",
  sayName: function(){
    alert(this.name);
  }
};
```

JavaScript有两种属性：数据属性和访问器属性
1.数据属性
1. 数据属性
数据属性包含一个数据值的位置。在这个位置可以读取和写入值。数据属性有4个描述其 行为的特性。
[[Configurable]] :表示能否通过delete 删除属性从而重新定义属性，能否修改 属性的特性，或者能否把属性修改为访问器属性。像前面例子中那样直接在对象上定 义的属性，它们的这个特性默认值为true 。
[[Enumerable]] :表示能否通过for-in 循环返回属性。像前面例子中那样直接在 对象上定义的属性，它们的这个特性默认值为true 。
[[Writable]] :表示能否修改属性的值。像前面例子中那样直接在对象上定义的属 性，它们的这个特性默认值为true 。
[[Value]] :包含这个属性的数据值。读取属性值的时候，从这个位置读;写入属 性值的时候，把新值保存在这个位置。这个特性的默认值为undefined 。

对于像前面例子中那样直接在对象上定义的属性，它们的[[Configurable]] 、[[Enumerable]] 和[[Writable]] 特性都被设置为true ，而[[Value]] 特性被设置 为指定的值。


修改数据属性需要使用 Object.defineProperty() 方法
这 个方法接收三个参数:属性所在的对象、属性的名字和一个描述符对象
其中，描述符 (descriptor)对象的属性必须是:configurable 、enumerable 、writable 和value 。设置其中的一或多个值，可以修改对应的特性值。

```
var person = {}; Object.defineProperty(person, "name", {
    writable: false,
    value: "Nicholas"
});
alert(person.name); //"Nicholas"
person.name = "Greg";
alert(person.name); //"Nicholas"
```


```
var person = {}; Object.defineProperty(person, "name", {
    configurable: false,
value: "Nicholas" });
alert(person.name); //"Nicholas" delete person.name; alert(person.name); //"Nicholas"
```

访问器属性
访问器属性不包含数据值;它们包含一对儿getter和setter函数(不过，这两个函数都不是 必需的)。在读取访问器属性时，会调用getter函数，这个函数负责返回有效的值;在写 入访问器属性时，会调用setter函数并传入新值，这个函数负责决定如何处理数据。访问 器属性有如下4个特性。
[[Configurable]] :表示能否通过delete 删除属性从而重新定义属性，能否修改 属性的特性，或者能否把属性修改为数据属性。对于直接在对象上定义的属性，这个 特性的默认值为true 。
[[Enumerable]] :表示能否通过for-in 循环返回属性。对于直接在对象上定义的 属性，这个特性的默认值为true 。
[[Get]] :在读取属性时调用的函数。默认值为undefined 。
[[Set]] :在写入属性时调用的函数。默认值为undefined 。
访问器属性不能直接定义，必须使用Object.defineProperty() 来定义。


```
var book = {
    _year: 2004,  
   edition: 1
 };
Object.defineProperty(book, "year", {
   get: function(){
     return this._year; },
    set: function(newValue){
      if (newValue > 2004) {
        this._year = newValue; this.edition += newValue - 2004;
        }
  }
});
book.year = 2005;
alert(book.edition); //2


```
以上代码创建了一个book 对象，并给它定义两个默认的属性:year 和edition 。year 前面的下划线是一种常用的记号，用于表示只能通过对象方法访问的属性。而访 问器属性year 则包含一个getter函数和一个setter函数。getter函数返回_year 的值，setter 函数通过计算来确定正确的版本。因此，把year 属性修改为2005会导致_year 变成 2005，而edition 变为2。这是使用访问器属性的常见方式，即设置一个属性的值会导致 其他属性发生变化。


定义多个属性
由于为对象定义多个属性的可能性很大，ECMAScript 5又定义了一 个Object.defineProperties() 方法。利用这个方法可以通过描述符一次定义多个属 性。这个方法接收两个对象参数:第一个对象是要添加和修改其属性的对象，第二个对象 的属性与第一个对象中要添加或修改的属性一一对应。例如:
```
var book = {};
Object.defineProperties(book, { _year: {
         value: 2004
     },
     edition: {
         value: 1
},
     year: {
         get: function(){
              return this._year; },
         set: function(newValue){
             if (newValue > 2004) {
               this._year = newValue;
               this.edition += newValue - 2004;
               }
   }
 }
});

```

创建对象

虽然Object 构造函数或对象字面量都可以用来创建单个对象，但这些方式有个明显的缺 点:使用同一个接口创建很多对象，会产生大量的重复代码。为解决这个问题，人们开始 使用工厂模式的一种变体。

6.2.1 工厂模式
```
function createPerson(name, age, job){ var o = new Object();
  o.name = name;
  o.age = age;
  o.job = job;
  o.sayName = function(){
  alert(this.name);
  };
  return o;
}
var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
```
函数createPerson() 能够根据接受的参数来构建一个包含所有必要信息的Person 对
象。可以无数次地调用这个函数，而每次它都会返回一个包含三个属性一个方法的对象。
工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题(即怎样知 道一个对象的类型)。

6.2.2 构造函数模式
JavaScript中的构造函数可以用来创建特定类型的对象。像object和array这样的原生构造函数，在运行时会自动出现在执行环境中。此外，也可以创建自定义的构造函数，从而定义自定义对象类型的属性和方法。

```
function Person(name, age, job){
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = function(){
    alert(this.name); };
  }
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

```
在这个例子中，Person() 函数取代了createPerson() 函数。我们注意到，Person()
中的代码除了与createPerson() 中相同的部分外，还存在以下不同之处:
没有显式地创建对象;
直接将属性和方法赋给了this 对象;
没有return 语句。

要创建Person的新实例，必须使用new操作符。以这种方式调用构造函数实际上会经历以上4个步骤：
1.创建一个新对象
2.将构造函数的作用域赋给新对象（因此this就指向了这个新对象）
(var person1 = new Person("Nicholas", 29, "Software Engineer");
  person是一个引用类型（指针 指向堆中的对象） 存在栈里面 Person是一个对象 存放在堆中
)
3.执行构造函数中的代码（为这个新对象添加属性）
4.返回新对象

1. 将构造函数当作函数
构造函数与其他函数的唯一区别，就在于调用它们的方式不同。不过，构造函数毕竟也是 函数，不存在定义构造函数的特殊语法。任何函数，只要通过new 操作符来调用，那它就 可以作为构造函数;而任何函数，如果不通过new 操作符来调用，那它跟普通函数也不会 有什么两样。例如，前面例子中定义的Person() 函数可以通过下列任何一种方式来调 用。

```
// 当作构造函数使用
var person = new Person("Nicholas", 29, "Software Engineer"); person.sayName(); //"Nicholas"
// 作为普通函数调用
Person("Greg", 27, "Doctor"); // 添加到window window.sayName(); //"Greg"
// 在另一个对象的作用域中调用
var o = new Object();
Person.call(o, "Kristen", 25, "Nurse"); （将Person绑定到o上）
o.sayName(); //"Kristen"

```

构造函数创建对象的问题

构造函数模式虽然好用，但也并非没有缺点。使用构造函数的主要问题，就是每个方法都 要在每个实例上重新创建一遍。在前面的例子中，person1 和person2 都有一个名 为sayName() 的方法，但那两个方法不是同一个Function 的实例。不要忘了—— ECMAScript中的函数是对象，因此每定义一个函数，也就是实例化了一个对象。

```
function Person(name, age, job){
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayName = sayName;
}

function sayName(){
   alert(this.name);
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

```
在这个例子中，我们把sayName() 函数的定义转移到了构造函数外部。而在构造函数内 部，我们将sayName 属性设置成等于全局的sayName 函数。这样一来，由于sayName 包 含的是一个指向函数的指针，因此person1 和person2 对象就共享了在全局作用域中定 义的同一个sayName() 函数。这样做确实解决了两个函数做同一件事的问题，可是新问 题又来了:在全局作用域中定义的函数实际上只能被某个对象调用，这让全局作用域有点 名不副实。而更让人无法接受的是:如果对象需要定义很多方法，那么就要定义很多个全 局函数，于是我们这个自定义的引用类型就丝毫没有封装性可言了。好在，这些问题可以 通过使用原型模式来解决。


6.2.3原型模式

我们创建的每个函数都有一个prototype (原型)属性，这个属性是一个指针，指向一 个对象，而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。如果按 照字面意思来理解，那么prototype 就是通过调用构造函数而创建的那个对象实例的原 型对象。使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。换句话 说，不必在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到原型对象 中，如下面的例子所示。


```
function Person(){
}
Person.prototype.name = "Nicholas"; Person.prototype.age = 29; Person.prototype.job = "Software Engineer"; Person.prototype.sayName = function(){
  alert(this.name);
};
var person1 = new Person(); person1.sayName(); //"Nicholas"
var person2 = new Person(); person2.sayName(); //"Nicholas"
alert(person1.sayName == person2.sayName); //true

```

原型模式的问题：原型中不能放引用类型（数组）（会被共享）



6.2.4 组合使用构造函数模式和原型模式

创建自定义类型的最常见方式，就是组合使用构造函数模式与原型模式。构造函数模式用 于定义实例属性，而原型模式用于定义方法和共享的属性。结果，每个实例都会有自己的 一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存。另外，这 种混成模式还支持向构造函数传递参数;可谓是集两种模式之长。下面的代码重写了前面 的例子。

```
function Person(name, age, job){
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = ["Shelby", "Court"];
}
Person.prototype = {
  constructor : Person,
  sayName : function(){
    alert(this.name);
  }
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
person1.friends.push("Van");
alert(person1.friends); //"Shelby,Count,Van"
alert(person2.friends); //"Shelby,Count"
alert(person1.friends === person2.friends); //false
alert(person1.sayName === person2.sayName); //true

```



6.2.5动态原型模式
有其他OO语言经验的开发人员在看到独立的构造函数和原型时，很可能会感到非常困 惑。动态原型模式正是致力于解决这个问题的一个方案，它把所有信息都封装在了构造函 数中，而通过在构造函数中初始化原型(仅在必要的情况下)，又保持了同时使用构造函 数和原型的优点。换句话说，可以通过检查某个应该存在的方法是否有效，来决定是否需 要初始化原型。来看一个例子。
```
function Person(name, age, job){
//属性
  this.name = name;
  this.age = age;
  this.job = job;
//方法  只有当sayName不存在当时候才会在原型中加入
// 只有初次调用的使用即可，只用判断一个应该存在的方法或者属性即可
if (typeof this.sayName != "function"){
    Person.prototype.sayName = function(){
      alert(this.name);
    };
  }

var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName();
```


6.2.6寄生构造函数模式

这个模式可以在特殊的情况下用来为对象创建构造函数。假设我们想创建一个具有额外方 法的特殊数组。由于不能直接修改Array 构造函数，因此可以使用这个模式。

```
function SpecialArray(){
  //创建数组
  var values = new Array();
  //添加值
  values.push.apply(values, arguments);
  //添加方法
  values.toPipedString = function(){
    return this.join("|");
  };
  //返回数组
       return values;
   }
  //使用这种模式返回的是values 而不是specialArray这个对象
  var colors = new SpecialArray("red", "blue", "green"); alert(colors.toPipedString()); //"red|blue|green"

```


6.2.7 稳妥构造函数模式


谓稳妥对象，指的是没有公共属性，而且其方法也不引用this 的 对象。稳妥对象最适合在一些安全的环境中(这些环境中会禁止使用this 和new )，或 者在防止数据被其他应用程序(如Mashup程序)改动时使用。稳妥构造函数遵循与寄生 构造函数类似的模式，但有两点不同:一是新创建对象的实例方法不引用this ;二是不 使用new 操作符调用构造函数。按照稳妥构造函数的要求，可以将前面的Person 构造函 数重写如下。

```
function Person(name, age, job){
//创建要返回的对象
var o = new Object();
//可以在这里定义私有变量和函数
//添加方法
o.sayName = function(){
        alert(name);
    };
//返回对象
return o;
}
//注意，在以这种模式创建的对象中，除了使用sayName() 方法之外，没有其他办法访问 name 的值。可以像下面使用稳妥的Person 构造函数。

var friend = Person("Nicholas", 29, "Software Engineer");
friend.sayName(); //"Nicholas"
```
6.3继承
通过原型链来实现继承
子类型的prototype设置为父类型的实例
通过原型链实现继承的时，不能使用对象字面量创建原型方法。

```
function SuperType(){ this.property = true;
}
SuperType.prototype.getSuperValue = function(){ return this.property;
};
function SubType(){ this.subproperty = false;
}
//继承了SuperType
SubType.prototype = new SuperType();
//使用字面量添加新方法，会导致上一行代码无效
SubType.prototype = {
        getSubValue : function (){
          return this.subproperty;
        },
        someOtherMethod : function (){
          return false;
      }
};
var instance = new SubType();
alert(instance.getSuperValue());
//error!

```

原型链继承的问题：
1.子类型的原型是父类型的实例，那么父类型的引用类型数据（数组）将被共享
2.创建子类型实例的时候，不能向超类的构造函数中传递参数。

解决方案１：
借用构造函数
这种技术 的基本思想相当简单，即在子类型构造函数的内部调用超类型构造函数。
别忘了，函数只 不过是在特定环境中执行代码的对象，因此通过使用apply() 和call() 方法也可以在 (将来)新创建的对象上执行构造函数
```
function SuperType(){
  this.colors = ["red", "blue", "green"];
}
function SubType(){
//继承了SuperType
  SuperType.call(this);
}
var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
var instance2 = new SubType();
alert(instance2.colors); //"red,blue,green"

```
