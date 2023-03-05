# 浏览器

## 1.数据类型

js 主要有两种数据类型：一种是基本数据类型，一种是引用数据类型

**基本数据类型：**string、Number、Boolean、Null、Undefined 等.... Symbol 也是。

**引用数据类型：** Object、Array、Function

**区别：**

1. 基本数据类型是指简单的数据段，引用数据类型是指可能由多个值构成的对象。

2. 基本数据是按值访问的，也就是说，可以直接操作保存在变量中的值。引用数据是按引用访问的，也就是说，操作的是保存在变量中的地址，而不是实际的数据。
   所以你通过一个变量修改引用类型的值，会影响到所有引用该值的变量。

3. 基本数据类型的赋值是值的拷贝，而引用数据类型的赋值是指针的拷贝，即指向同一块内存空间

4. 基本数据类型的变量在栈内存中存储，而引用数据类型的变量在堆内存中存储。

## 2.数据类型检测的方式有哪些

1. typeof

```javascript
console.log(typeof 2); // number
console.log(typeof true); // boolean
console.log(typeof "str"); // string
console.log(typeof []); // object
console.log(typeof function () {}); // function
console.log(typeof {}); // object
console.log(typeof undefined); // undefined
console.log(typeof null); // object
```

2. instanceof:机制是判断在其原型链中能否找到该类型的原型

```javascript
function myInstanceof(left, right) {
  // 获取对象的原型
  let proto = Object.getPrototypeOf(left);
  // 获取构造函数的 prototype 对象
  let prototype = right.prototype;

  // 判断构造函数的 prototype 对象是否在对象的原型链上
  while (true) {
    if (!proto) return false;
    if (proto === prototype) return true;
    // 如果没有找到，就继续从其原型上找，Object.getPrototypeOf方法用来获取指定对象的原型
    proto = Object.getPrototypeOf(proto);
  }
}
```

3. constructor

```javascript
console.log((2).constructor === Number); // true
console.log(true.constructor === Boolean); // true
console.log("str".constructor === String); // true
console.log([].constructor === Array); // true
console.log(function () {}.constructor === Function); // true
console.log({}.constructor === Object); // true
```

4. Object.prototype.toString.call()

## 3. typeof null 的结果是什么，为什么？

typeof null 的结果是 Object。
在 JavaScript 第一个版本中，所有值都存储在 32 位的单元中，每个单元包含一个小的 类型标签(1-3 bits) 以及当前要存储值的真实数据。类型标签存储在每个单元的低位中，共有五种数据类型：

000: object - 当前存储的数据指向一个对象。

1: int - 当前存储的数据是一个 31 位的有符号整数。

010: double - 当前存储的数据指向一个双精度的浮点数。

100: string - 当前存储的数据指向一个字符串。

110: boolean - 当前存储的数据是布尔值。

如果最低位是 1，则类型标签标志位的长度只有一位；如果最低位是 0，则类型标签标志位的长度占三位，为存储其他四种数据类型提供了额外两个 bit 的长度。
有两种特殊数据类型：

undefined 的值是 (-2)30(一个超出整数范围的数字)；
null 的值是机器码 NULL 指针(null 指针的值全是 0)

那也就是说 null 的类型标签也是 000，和 Object 的类型标签一样，所以会被判定为 Object.

## 4. 为什么 0.1+0.2 ! == 0.3，如何让其相等

**原因**
计算机是通过二进制的方式存储数据的，所以计算机计算 0.1+0.2 的时候，实际上是计算的两个数的二进制的和。0.1 的二进制是 0.0001100110011001100...（1100 循环），0.2 的二进制是：0.00110011001100...（1100 循环）。根据这个原则，0.1 和 0.2 的二进制数相加，再转化为十进制数就是
**解决办法**
ES6 中，提供了 Number.EPSILON(埃普西隆)属性，而它的值就是 2-52，只要判断 0.1+0.2-0.3 是否小于 Number.EPSILON

```javascript
function numberepsilon(arg1, arg2) {
  return Math.abs(arg1 - arg2) < Number.EPSILON;
}

console.log(numberepsilon(0.1 + 0.2, 0.3)); // true
```

## 5.箭头函数和普通函数区别

1. 箭头函数比普通函数更加简洁

2. 箭头函数没有自己的 this
   箭头函数不会创建自己的 this， 所以它没有自己的 this，它只会在自己作用域的上一层继承 this。所以箭头函数中 this 的指向在它在定义时已经确定了，之后不会改变。

3. call()、apply()、bind()等方法不能改变箭头函数中 this 的指向

4. 箭头函数不能作为构造函数使用

5. 箭头函数没有自己的 arguments
   箭头函数没有自己的 arguments 对象。在箭头函数中访问 arguments 实际上获得的是它外层函数的 arguments 值。

## 6.原型和原型链

**原型**

什么是原型？我觉得可以这样理解。原型就是雏形。就好比是一个模板，你可以根据这个模板创造出很多实实在在的东西，也就是实例。比如 Array 这个构造函数的 prototype 就可以看做是一个原型，你可以 const a = new Array([1,2,3])。然后就创造出了一个数组。那原型 prototype 有什么用呢？prototype 的作用是，如果你要基于构造函数（假设 Array）创建出一个对象（假设是一个数组）的话，prototype 中存放着你生成的实例可使用的所有属性和方法（比如 forEach, length.....等）

**原型链**

```javascript
function FactoryAnimal(name) {
  this.name = name;
}
FactoryAnimal.prototype.actions = ["shout", "jump"];
const dog = new FactoryAnimal("dog");
const cat = new FactoryAnimal("cat");
console.log(dog.actions); // ['shout', 'jump']
```

说明：
在刚才说的代码中，实例 dog 是被 FactoryAnimal 这个 constructor 创建出来的。但是当我 console.log(dog.actions)的时候，代码没有显示 undefined，而是直接打印了['shout', 'jump']。可是我 console.log(dog)的时候，控制台中 dog 明明没有 actions 属性，这是为什么呢？这是因为，dog 在被实例化的以后，其实在 dog 这个对象中还有一个隐藏的属性**proto**。这个属性控制台中是不会显示的。这个属性其实就是构造函数 FactoryAnimal 中的 prototype 属性。当我们去访问 dog.actions 的时候，js 会先从 dog 这个对象上面去找 actions 这个属性，发现找不到，然后就会去**proto**中找，换句话说，就是去 dog 的原型对象 FactoryAnimal 中的 prototype 属性中去找，发现能找到，就直接返回了，但是，在 FactoryAnimal 的 prototype 中如果还是找不到，就会去 FactoryAnimal 构造函数的**proto**属性中去找，也就是去 FactoryAnimal 构造函数的 constructor----Function 构造函数中的 prototype 里面去找，如果在 Function 的 prototype 中还是找不到，那就去 Function**proto**中去找，如此循序渐进，直到一直找到 Object 中的 prototype 中为止，这里如果还是找不到，那就是真的没有这个属性或者方法了，因为 Object 的 e。而上面说的**proto**和 prototype 的这一层链式关系，就是所谓的原型链，由于**proto**这个属性是隐藏的，所以原型链又叫做隐式原型链。js 对象就是通过原型链来实现原型对象的属性继承的。如果没有了原型链，那一个对象被实例化出来就没有什么意义了。
其实原型对象（constructor），原型（prototype），原型链（**proto**）的关系总结下来就是
js 中我们需要创建对象，所以就需要有原型对象做基石，有了原型对象以后，用早原型对象中用 prototype 属性把实例对象可用的属性给存下来，那存下来以后，怎么让实例对象能用呢，**proto**就起到了继承的作用

## 7.闭包

闭包是指一个函数能够访问并使用其定义范围内的变量，即使这些变量在函数外部也可以被访问。通常，当一个函数返回另一个函数时，返回的函数可以访问它被创建的环境中的变量，这个被访问的环境可以理解成是闭包。

举个通俗点的例子：

你在 JavaScript 中，每当函数被调用时，都会创建一个新的执行环境。在这个执行环境中，会有一个变量对象，其中存储了该函数的参数、局部变量等信息。当函数执行完毕后，这个执行环境就会被销毁，其中的变量也会随之消失。

但是，当一个函数返回另一个函数时，内部函数会持有对外部函数作用域的引用，即使外部函数执行完毕后，内部函数仍然可以访问外部函数中的变量。这就是闭包的概念。

闭包可以用于创建私有变量、封装函数、缓存数据等场景。

## 8.作用域

**作用域**

大白话可以理解成在程序中变量可作用的区域。它决定了就是在代码某个部分中定义的变量是否可以被访问到。

（1）全局作用域

最外层函数和最外层函数外面定义的变量拥有全局作用域
所有未定义直接赋值的变量自动声明为全局作用域
所有 window 对象的属性拥有全局作用域
全局作用域有很大的弊端，过多的全局作用域变量会污染全局命名空间，容易引起命名冲突。

（2）函数作用域

函数作用域声明在函数内部的变零，一般只有固定的代码片段可以访问到
作用域是分层的，内层作用域可以访问外层作用域，反之不行

## 9. 执行上下文

首先我先说一下最常见的两种执行上下文

（1）全局执行上下文

你可以把任何不在函数内部的都是全局执行上下文，它首先会创建一个全局的 window 对象，并且设置 this 的值等于这个全局对象，一个程序中只有一个全局执行上下文。

（2）函数执行上下文

当一个函数被调用时，就会为该函数创建一个新的执行上下文，函数的上下文可以有任意多个。

简单来说执行上下文就是指：
在执行一点 JS 代码之前，需要先解析代码。解析的时候会先创建一个全局执行上下文环境，先把代码中即将执行的变量、函数声明都拿出来，变量先赋值为 undefined，函数先声明好可使用。这一步执行完了，才开始正式的执行程序。
在一个函数执行之前，也会创建一个函数执行上下文环境，跟全局执行上下文类似，不过函数执行上下文会多出 this、arguments 和函数的参数。

## 10.this
对this对象的理解
this 是执行上下文中的一个属性，它指向最后一次调用这个方法的对象。在实际开发中，this 的指向可以通过四种调用模式来判断。

第一种是函数调用模式，当一个函数不是一个对象的属性时，直接作为函数来调用时，this 指向全局对象。

第二种是方法调用模式，如果一个函数作为一个对象的方法来调用时，this 指向这个对象。

第三种是构造器调用模式，如果一个函数用 new 调用时，函数执行前会新创建一个对象，this 指向这个新创建的对象。

第四种是 apply 、 call 和 bind 调用模式，这三个方法都可以显示的指定调用函数的 this 指向。其中 apply 方法接收两个参数：一个是 this 绑定的对象，一个是参数数组。call 方法接收的参数，第一个是 this 绑定的对象，后面的其余参数是传入函数执行的参数。也就是说，在使用 call() 方法时，传递给函数的参数必须逐个列举出来。bind 方法通过传入一个对象，返回一个 this 绑定了传入对象的新函数。这个函数的 this 指向除了使用 new 时会被改变，其他情况下都不会改变。

这四种方式，使用构造器调用模式的优先级最高，然后是 apply、call 和 bind 调用模式，然后是方法调用模式，然后是函数调用模式。


## 11. call/bind/apply
apply 接受两个参数，第一个参数指定了函数体内 this 对象的指向，第二个参数为一个带下标的集合，这个集合可以为数组，也可以为类数组，apply 方法把这个集合中的元素作为参数传递给被调用的函数。

call 传入的参数数量不固定，跟 apply 相同的是，第一个参数也是代表函数体内的 this 指向，从第二个参数开始往后，每个参数被依次传入函数。

bind返回绑定this之后的函数，便于稍后调用；apply、call则是立即执行

## 12. EventLoop
因为js是单线程的，所以代码的正常执行可能会存在阻塞的情况。而事件轮询就是一种可以让js运行时不阻塞的机制。

在js执行的时候，线程只有一个，但是任务队列是会有多个的。任务队列又会分为宏任务和微任务，常见的宏任务的话，有setTimeout， script整体代码，微任务的话最常见的就是promise.then。

有这么多类型的这个任务事件是因为这个任务队列中事件循环的顺序，决定了js代码的执行顺序。它大概的顺序是先执行宏任务代码，让全局的上下文进入到函数调用栈里面去，然后等到你栈队列里面的任务执行完了，就会看看你刚才的任务中是否有微任务，有的话，就执行，执行完了，再执行下一个宏任务，以此循环，大概就是这个流程

## 13. 创建对象的方式
https://www.yuque.com/semmering/fknusv/dd36ie#q1sec

## 14. 实现继承的方式
https://www.yuque.com/semmering/fknusv/dd36ie#q1sec