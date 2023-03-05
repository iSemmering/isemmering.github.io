## 实现深拷贝
借助工具库，如lodash的_.cloneDeep()

利用JSON.stringify()将对象转成JSON字符串，再用JSON.parse()把字符串解析成对象1。但是这种方法不能处理函数和正则

递归遍历对象或数组，直到里面都是基本数据类型，然后再去复制

```javascript
function deepClone(obj) {
  // 判断对象是否为基本类型
  if (typeof obj !== "object" || obj === null) {
    return obj;
  }
  // 判断对象是否为数组
  if (Array.isArray(obj)) {
    return obj.map((item) => deepClone(item));
  }
  // 处理对象
  const newObj = {};
  Object.keys(obj).forEach((key) => {
    newObj[key] = deepClone(obj[key]);
  });
  return newObj;
}

```

## 实现 promise
创建一个 Promise 构造函数，它包含两个参数：resolve 和 reject。

在构造函数内部，创建一个状态变量（state），它有三种可能的值：pending、fulfilled 和 rejected。

创建一个 value 变量，用于存储 Promise 的值。如果状态为 fulfilled，则 value 包含 Promise 的值；如果状态为 rejected，则 value 包含 Promise 失败的原因。

创建一个 then 方法，它接受两个函数作为参数：onFulfilled 和 onRejected。当 Promise 的状态为 fulfilled 时，调用 onFulfilled 函数；当 Promise 的状态为 rejected 时，调用 onRejected 函数。

实现 Promise 的 then 方法，它应该返回一个新的 Promise，以支持链式调用。

在 then 方法内部，检查当前 Promise 的状态。如果状态为 pending，将 onFulfilled 和 onRejected 函数添加到 Promise 的回调队列中。如果状态为 fulfilled，则调用 onFulfilled 函数并将结果传递给新的 Promise。如果状态为 rejected，则调用 onRejected 函数并将原因传递给新的 Promise。

实现 Promise 的 catch 方法，它接受一个函数作为参数，用于处理 Promise 的错误。catch 方法实际上是 then 方法的一个简写，只处理 Promise 失败的情况。

实现 Promise 的 resolve 和 reject 静态方法，它们可以将任何值转换为 Promise 对象，resolve 方法返回一个 fulfilled 状态的 Promise，而 reject 方法返回一个 rejected 状态的 Promise。


## 实现继承
https://www.yuque.com/semmering/fknusv/skg3qe

## 实现文件上传

```javascript
<form method="POST" action="https://your.domain.com/upload"></form>
<input type="file" value="请选择文件" />
var form = new FormData();
var fileObj = document.getElementById("file").files[0]
form.append("file", fileObj); //第一个参数是后台读取的请求key值
最后xhr.send(form)
```

## ajax 的取消请求

## call/apply/bind 实现

**call**

判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。

判断传入上下文对象是否存在，如果不存在，则设置为 window 。

处理传入的参数，截取第一个参数后的所有参数。

将函数作为上下文对象的一个属性。

使用上下文对象来调用这个方法，并保存返回结果。

删除刚才新增的属性。

返回结果。

```javascript
Function.prototype.myCall = function (context) {
  // 判断调用对象
  if (typeof this !== "function") {
    console.error("type error");
  }
  // 获取参数
  let args = [...arguments].slice(1),
    result = null;
  // 判断 context 是否传入，如果未传入则设置为 window
  context = context || window;
  // 将调用函数设为对象的方法
  context.fn = this;
  // 调用函数
  result = context.fn(...args);
  // 将属性删除
  delete context.fn;
  return result;
};
```

**apply**

判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。

判断传入上下文对象是否存在，如果不存在，则设置为 window 。

将函数作为上下文对象的一个属性。

判断参数值是否传入

使用上下文对象来调用这个方法，并保存返回结果。

删除刚才新增的属性

返回结果

```javascript
Function.prototype.myApply = function (context) {
  // 判断调用对象是否为函数
  if (typeof this !== "function") {
    throw new TypeError("Error");
  }
  let result = null;
  // 判断 context 是否存在，如果未传入则为 window
  context = context || window;
  // 将函数设为对象的方法
  context.fn = this;
  // 调用方法
  if (arguments[1]) {
    result = context.fn(...arguments[1]);
  } else {
    result = context.fn();
  }
  // 将属性删除
  delete context.fn;
  return result;
};
```

**bind**
判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。

保存当前函数的引用，获取其余传入参数值。

创建一个函数返回

函数内部使用 apply 来绑定函数调用，需要判断函数作为构造函数的情况，这个时候需要传入当前函数的 this 给 apply 调用，其余情况都传入指定的上下文对象。

```javascript
Function.prototype.myBind = function (context) {
  // 判断调用对象是否为函数
  if (typeof this !== "function") {
    throw new TypeError("Error");
  }
  // 获取参数
  var args = [...arguments].slice(1),
    fn = this;
  return function Fn() {
    // 根据调用方式，传入不同绑定值
    return fn.apply(
      this instanceof Fn ? this : context,
      args.concat(...arguments)
    );
  };
};
```

## new 实现

当我们知道了原型对象，原型，原型链的关系以后，其实就知道 new 关键字的内部实现了。
总结下来，就四步，

1. 创建一个空对象

2. 让空对象继承 constructor 的属性

3. 执行构造函数，生成实例对象

4. 判断参数类型按情况返回实例

```javascript
function MyNew() {
  const obj = new Object();
  // 由于传递进来的参数会是很多个，所以我们先把构造函数放在第一个参数传进来，这样的话，shift返回的将是这个构造函数
  const theConstructor = [].shift.call(arguments);
  // 先把构造函数从参数中提取出来再说

  // 把构造函数的prototype赋值给对象的__proto__，因为js中就是这样继承的
  obj.__proto__ = theConstructor.prototype;

  // 上面两步我们只做了基本工作，现在要做的工作是把传递进来的参数用构造函数实例化对象。
  const result = theConstructor.apply(obj, arguments);

  // new关键字如果return的是一个引用类型的话，就会返回引用类型，如果是基本类型，会返回this
  return typeof result === "object" ? result : obj;
}
function Play(name, age) {
  (this.name = name), (this.age = age);
}
const b = MyNew(Play, "das", 12);
console.log(b); // Play {name: 'das', age: 12}
```
