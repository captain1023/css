##执行环境以及作用域
每个函数都有自己的执行环境。当执行流进入一个函数当时候，函数当环境就会被推入一个环境栈中。而在函数执行之后，栈将其环境弹出，把控制权返回 之前的执行环境。

4.2.2 没有块级作用域
  1.声明变量
    使用var声明的变量会自动被添加到最接近的环境中。在函数内部，最接近的环境就是函数的局部环境。在with中，最接近的环境是函数环境。如果初始化变量没有使用var，则变量会被自动添加到全局环境。

## 引用类型
  5.1 Object类型
    创建object实例的方式有两种
    5.1.1 使用new操作符 var person = new Object();
    5.1.2 使用对象字面量表示法 var person = { name:"aaa",age:"11"};
    5.2.3 通过"."访问对象object中的属性（不支持变量形式） 通过"[]"访问的object中属性，支持变量
  5.2 Array类型
    var colors = new Array();
    var colors = new Array(20);                 创建包含20项的数组
    var colors = new Array("red","blue","green")
    字面量创建方法
    var colors = ["red","blue","green"]
    vard names = []
    数组中的length不只是“只读的”，因此可以通过设置这个属性，从数组的末尾移除项或向数组中添加新项。（被删除和新扩张的项是undefined）
    使用length在数组末尾添加新项：colors[colors.length] = "black"
    5.2.1
    检测数组:单全局环境（就一个标签页）使用(value instanceof Array)即可 通用Array.isArray(value)
    数组模拟栈(pop(),push(),LIFO)
    数组模拟队列(shift(),push(),FIFO)
    数组排序 var values = [0,1,2,3] values.sort(compare)
    function compare(v1,v2){
      //
    }
    数组剪裁: slice(begin,end)
             slice如果只传入begin则会从begin开始复制到结尾
             slice如果传入end，则会复制到end-1
             slice如果传入负数，则用数组长度加上该数字确定位置，如果end比begin要小，返回空数组

             splice()
             删除：splice(0,2)删除前两项
             插入:splice(2,0,"red","green")从当前数组到位置2开始插入"red" "green"
             替换:splice(2,1,"red,"green")删除当前数组位置2并插入"red" "green"
    数组位置方法：indexOf(value,index) lastindexOf(value,index)
                一个从前往后着 一个从后往前找
                value=要查找到项, index=从哪里开始查找
    数组迭代方法：
              every(fucntion(item,value,array),this(默认是本身))
                若每一项都返回true则返回true
              filter()
                给每一项运行给定函数，返回该函数会返回true的项组成的数组
              forEach()
                对每一项运行给定函数，没有返回值
              map()
                对每一项运行给定函数，返回每次调用结果组成对数组
              some()
              若有一项返回true则返回ture
    "TODO:跳过了DATE和Regex"
    5.5 Function()
    函数实际上是对象。每个函数都是Function类型的实例，而且都与其他引用类型一样具有属性和方法。由于函数       
    是对象，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定
    声明方法：
    function sum(num1,num2)           函数声明语法
    var sum = function(num1,num2){}   函数表达式
    var sum = new Function(num1,num2,"return num1+num2")//构造函数 不推荐

####函数声明和函数表达式
      函数声明会有函数声明提升的过程(即在全局环境中会预先解析函数名)
       ```
         alert(sum(10,10))
         fucntion sum (num1,num2){
           //可以运行
           //
         }
       ```
    5.5.3作为值的函数
    函数可以作为值来使用。也就是说，不仅可以像传递参数一样把一个函数传递给另一个函数，而且可以讲一个函数作为另一个函数的结果返回

    ```
      function createComparisonFunction(propertyName){
        return function(obj1,obj2){
          var value1 = obj1[propertyName];
          var value2 = obj2[propertyName];
          if(value1<value2) return -1;
          if(value1>value2) return 1;
          if(value1== value2) return 0;
        }
      }

      var data = [{name:"Zachary",age:28},{name:"Nicholas",age:29}];

      data.sort(createComparisonFunction("name"))//注:sort(function(））

    ```
    5.5.4函数内部属性(arguments this)
    argument:主要功能是为了保存传入的参数 和callee指针(指向拥有这个argument的函数)
    this:this应用的是函数数据执行的环境对象

    函数的属性和方法:
    length:表示函数希望接收的命名参数的个数
    apply(this,参数数组):
    call(this,参数列表):

###基本包装类型 Boolean Number String
```
  var s1 = "some text"
  var s2 = s1.substring(2)
```
s1包含一个字符串，字符串当然是基本类型值。而下一行调用了s1的substring()方法，并将返回的结果保存在了s2中。我们知道，基本类型值不是对象，能完成此类操作后台自动完成了一系列处理。
1.创建String类型实例
2.在实例上调用指定方法
3.销毁实例

```
var s1 = "some text";
s1.color = "red";
alert(s1.color); //undefined 在第二行执行完对象在第三行之前就销毁了
```

##Javascript 面向对象的程序设计


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






promise的then方法传入的是一个callback函数的参数，
1.callback函数为匿名函数的时候，call函数内部的this指向window，需要对函数bind this
2.callback函数为箭头函数的时候，callback函数的this会指向他的上层




###bind绑定
bind绑定是软绑定 new的优先级比bind高
bind() 方法创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。
bind可以在function后直接调用

apply（this,arguments(数组等)） call(this,arg1,arg2,arg3)参数列表


javascript变量可以保存两种类型的值：基本类型和引用类型
  基本类型值在内存中占据固定大小的空间，因此被保存在栈内存中;
  引用类型的值是对象，保存在堆内存中;
  包含引用类型值的变量实际上包含的并不是对象本身，而是一个指向该对象的指针;
  从一个变量向另一个变量复制引用类型的值，复制的其实是指针，因此两个变量最终 都指向同一个对象;
  确定一个值是哪种基本类型可以使用typeof 操作符，而确定一个值是哪种引用类型 可以使用instanceof 操作符。






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

##创建对象

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


###继承总结：
1.原型链实现继承 缺点：引用类型属性共享

```
function SuperType(){
  this.colors = ["red","blue","green"]
}

fucntion SubType(){
}
//继承了superType subType中有实例自己的属性和prototype中的属性
//问题：数据共享
SubType.prototype = new SuperType();

```
2.为解决引用类型共享问题引入：借用构造函数---在子类对构造函数中给父类绑定this
                          解决了属性共享问题和传递参数问题
                          构造函数问题：方法都在构造函数中定义，函数无法复用
                          每个对象都包含同样都函数但不是同一个函数 浪费内存
```
function SuperType(name){
  this.colors = ["red","blue","green"]
  this.name = name;
}

function subType(name){
  //继承SuperType并传递name属性
  SuperType.call(this,name)
  //实例属性
  this.age = 29
}


var instance = new SubType();

```

3.为了借用构造函数问题和属性共享问题引入 组合继承 ---- 构造函数继承私有属性，原型链继承方法和公共属性

```
fucntion SuperType(name){
  this.name = name;
  this.colors = ["red","blue","green"]
}

SuperType.prototype.sayName = function(){
  alert(this.name)
}


function SubType(name,age){
  //继承属性
  SuperType.call(this,name)
  //子类属性
  this.age = age
}
//继承方法
subType.prototype = new SuperType();
SubType.prototype.sayAge = function(){
  alert(this.age)
}
```

4.原型式继承（基于已有但对象来继承）
```
function object(o){   //Javascript5实现了object.create
    function F(){}
    F.prototype = o;
    return new F();
}
var person={
    name:"test"
    friends:["1","2","3"]

}
var person1 = object(person)    //用相同的prptotype
var person2 = object(person)


```
5.寄生式继承 （基于原有对象并加入子对象的私有属性）
```
function createAnother(original){
  var clone = object(original)    //创建新对象
  clone.sayHi=function(){         //增强对象
    alert("Hi")
  }
  return clone                   //返回对象
}
问题：做不到函数复用
```

6.寄生组合式继承：为了弥补组合继承调用两次父类构造函数的问题
          思路： 不必为了指定的子类型的原型而调用超类型的构造函数，我们所需要的无非是一个超类型原型的一个副本。本质上，就是使用寄生式继承来继承超类型的原型，然后在将结果给指定子类型的原型。
          借用构造器继承属性
          手动实现原型链继承

```
function inheritPrototype(subType,superType){
  var prototype = object(superType);
  prototype.constructor = subType
  subType.prototype=prototype
}

fucntion SuperType(name){
  this.name = name;
  this.colors = ["red","blue","green"]
}

SuperType.prototype.sayName = function(){
  alert(this.name)
}

function SubType(name,age){
  //继承属性
  SuperType.call(this,name)
  //子类属性
  this.age = age
}

inheritPrototype(subType,superType)

subType.prototype.sayAge= function(){
  alert(this.age)
}
```













## 7 函数表达式
7.1 递归
    1.通过argument.callee() 实现递归可以避免递归函数名变化造成的错误

7.2 闭包
    概念:闭包是指有权访问另一个函数作用域中的变量函数。
    常见创建方式:在一个函数内创建另一个函数.

```
  function A (args){
    var var1 = 0;
    var var2 = 0;
    return B(){
        //
    }
  }

//此处function A的活动对象有 args, var1,var2,this

var tmp = A(args)
在执行完这个function A 函数后 var1 和var2仍然存在内存中
因为tmp(返回的匿名函数)仍旧在引用var1 var2

```


7.2.1 闭包和变量
          闭包的副作用：闭包只能取得包含函数中任何变量的最后一个值。
          闭包所保存的是整个变量对象，而不是某个特殊的变量。
```
function creatFunctions(){
  var result = new Array();

  for(var i = 0 ; i <10;i++){
    result[i] = function(){
      //闭包
      return i;
    };
  }
  return result;
}
//返回的数组里面都是10
闭包内引用的外部活动对象i 而不是值。
//测试结果返回的是包含10个function的数组
```


匿名函数的执行环境具有全局性
this的指向:
        1.fuction调用指向当前的执行环境
        2.对象方法调用指向调用对象
        3.


```
var name = "The Window";
var person = {
    name : "Alan",
    sayOne:function(){
      alert(this.name)                    //Alan
    }

    sayTwo:function(){
      //这里的this指向person 对象
      return function(){

        alert(this.name)                  //The Window
      }
    }
};
  person.sayOne() //Alan  
  person.sayTwo() //The Window
  sayOne通过对象方法调用指向person对象
  sayTwo通过对象方法调用指向person对象，但SayTwo返回一个匿名函数，此匿名函数执行时当前环境是全局环境，故此this指向window
```

闭包会引用包含函数的整个活动对象
可以利用闭包模仿块级作用域 和私有变量
```
(function(){
    //块级作用域，声明的变量出了函数会被销毁
  })();//立刻执行


//好处：减少闭包占用的内存问题，没有指向匿名函数的引用。只要函数执行完毕，就可以立即销毁其作用域链
```

私有变量：利用构造函数来生成私有变量
```
function MyObject(){
  //私有变量和私有函数
  var privateVariable = 10;
   function privateFunction(){
     return false;
    }
//特权方法
this.publicMethod = function (){
         privateVariable++;
         return privateFunction();
     };
}

var obj1 = new MyObject();
alert(ojb1.privateVariable) //undefined
原因在new对象的时候没有把privateVarible绑定到对象上，只绑定了特权方法
故此实现私有变量
缺点：只能使用构造函数模式，每个对象的方法和属性都是重新生成(构造函数创建对象的问题)
```

静态私有变量
```
(function(){

  var privateVarible = 10;
  function privateVarible(){
    return false;
  }

  //构造函数
  //没有使用函数声明而是使用了函数表达式
  //函数声明只能创建局部函数，函数表达式会在js运行时确定，并在赋值完后才能调用
  //MyObject 也没有使用var关键词 初始化未经声明的变量，总是会创建一个全局变量
  //严格模式下报错
  MyObject = function(){};

  //公有方法
  MyObject.prototype.publicMethod = function(){
    privateVariable++;
    return privateFunction();
  }


})();


```

模块模式 ---为单例创建私有变量和方法

```
var singleton = function(){

    var privateVarible = 10;

    function privateFunction(){
      return false;
    }
    return {
          publicProperty:true,
          publicMethod:function(){
            privateVarible++;
            return privateFunction();
          }
    }
}()''

//立刻执行并返回字面量对象
```

### BOM

####窗口总结
  1.window是全局对象
  2.如果页面中包含框架(frame),则每个框架都拥有自己的Window对象并保存在frames中
    可以荣国window.frames[0]/window.frames["topFrame"]/window.frames["leftFrame"]
  3.窗口位置
    取得窗口在屏幕上的位置:windows.screenLeft/windows.screenRight
    移动窗口位置：
    window.moveTo(0,9)  //移动到屏幕左上角
    windows.moveBy(0,100)//向下移动100像素
  4.导航和打开窗口
    var wroxWin = window.open(URL,窗口目标，特性字符串)


    返回一个新窗口的引用
    ```
      window.open("http://www.wrox.com/", topFrame");
      //等同于< a href="http://www.wrox.com" target="topFrame"></a>

      如果有一个叫topFrame的窗口或者框架，就会在该窗口或框架加载这个URL
      否则会创建一个新的窗口并将其命名为topFrame
    ```

      4.1 弹出窗口
        如果给window.open()传递的第二个参数并不存在，那么会根据第三个参数传入的字符串穿件一个新的窗口或者标签页（）。如果没有传入则全部默认
        ```
          window.open("http://www.wrox.com/","wroxWindow",
         "height=400,width=400,top=10,left=10,resizable=yes");
         可以设置多个属性，字符串中用","隔开
        ```

      5.关闭窗口
      关闭窗口 wroxWin.close()
      浏览器的主窗口在不经过用户允许是不可以关闭自己，但弹出窗口可以关闭自己

      6.新窗口的原始窗口
        新创建的window对象有一个openner属性，其中保存着打开它的原始窗口对象。

      7.窗口间的通信
        有些浏览器(IE8,Chrome)会在独立的进程中运行每个标签页。当一个标签页打开另一个标签页时，如果两个window对象之间需要彼此通信，那么新标签页就不能运行在独立的进程中。在Chrome中，将新创建的标签页的opener设置为null，即表示在单独的进程中运行新标签页。
      8.弹出窗口被窗口屏蔽程序屏蔽后返回null


#### 间歇调用和超时调用(setTimeout(),setInterval())
      1.宏任务、微任务、event-loop
        1.1会首先执行同步代码，同步代码中检查微任务，下一个宏任务
        1.2当前宏任务的微任务没有结束则不会调用下一个宏任务
        1.3 new promise((resolve,reject)=>{
            resolve()
            console.log("111")//这里是同步代码
          }).then(()=>{
              //这里才是异步代码 微任务
            })
        1.4 event-loop会定期检查当前宏任务还有没有微任务，如果没有则进入下一个宏任务
      2.setTimeout(),setInterval()会返回一个调用ID 根据这个ID可以取消调用
        clearTimtout(),clearInterval()

####系统对话框
    1.alert()
    2.confirm()如果点ok返回true,点cancel返回false
    3.prompt() 提示框
    ```
      var result = prompt("what is your name?","");
      if (result !== null){
        alert("Welcome,"+result)
      }
    ```

####location对象
    1.window.location和document.location引用的同一个对象
    location包含hash,host,hostname,href,pathname,port,protocol,search等属性
    2.使用locaton.assign("http://www.wrox.com")可以立刻打开新的URL
    3.改变URL都会以新URL重新加载并保存在历史记录中(可以通过浏览器后退)
    4.可以使用location.replace(http://www.wrox.com)禁用, replace函数不会生成历史记录
    5.location.reload()//如果服务器没有改变则从缓存中加载
      location.reload(true)//从服务器加载

#### navigator对象
    1.有很多属性用来检测浏览器信息
#### screen对象
    1.用来储存显示器的信息
#### history对象
    1.history.go()//参数可以是1,-1,URL(跳转到最近的)
    2.history.back() history.forward()
    3.history有length属性，可以用于确定用户是否一开始就打开了你的页面
#### DOM
    1.nodeName 元素的标签名
    2.DOM可以看作树状图，每个节点都有childNodes属性(类array对象)且动态更新


###事件





###JSON




##Ajax and Comet
