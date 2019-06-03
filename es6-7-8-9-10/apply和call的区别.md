# `apply` 和 `call` 的区别

ECMAScript 规范给所有函数都定义了 `call` 与 `apply` 两个方法，它们的应用非常广泛，它们的作用也是一模一样，只是传参的形式有区别而已。

## [apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)( )

apply 方法传入两个参数：一个是作为函数上下文的对象，另外一个是作为函数参数所组成的数组。

```javascript
var obj = {
    name: "linxin"
};

function func(firstName, lastName) {
    console.log(firstName + " " + this.name + " " + lastName);
}

func.apply(obj, ["A", "B"]); // A linxin B
```

可以看到，obj 是作为函数上下文的对象，函数 func 中 this 指向了 obj 这个对象。参数 A 和 B 是放在数组中传入 func 函数，分别对应 func 参数的列表元素。

## [call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)( )

call 方法第一个参数也是作为函数上下文的对象，但是后面传入的是一个`参数列表`，而不是单个数组。

```javascript
var obj = {
    name: "linxin"
};

function func(firstName, lastName) {
    console.log(firstName + " " + this.name + " " + lastName);
}

func.call(obj, "C", "D"); // C linxin D
```

对比 apply 我们可以看到区别，C 和 D 是作为单独的参数传给 func 函数，而不是放到`数组`中。

# 经验

对于什么时候该用什么方法，其实不用纠结。如果你的参数本来就存在一个数组中，那自然就用 apply，如果参数比较散乱相互之间没什么关联，就用 call。

## apply 和 call 的用法

---

## 1.改变 this 指向

```javascript
var obj = {
    name: "linxin"
};

function func() {
    console.log(this.name);
}

func.call(obj); // linxin
```

我们知道，call 方法的第一个参数是作为函数上下文的对象，这里把 obj 作为参数传给了 func，此时函数里的 this 便指向了 obj 对象。此处 func 函数里其实相当于

```javascript
function func() {
    console.log(obj.name);
}
```

## 2.借用别的对象的方法

```javascript
var Person1 = function() {
    this.name = "linxin";
};
var Person2 = function() {
    this.getname = function() {
        console.log(this.name);
    };
    Person1.call(this);
};
var person = new Person2();
person.getname(); // linxin
```

从上面我们看到，Person2 实例化出来的对象 person 通过 getname 方法拿到了 Person1 中的 name。因为在 Person2 中，Person1.call(this) 的作用就是使用 Person1 对象代替 this 对象，那么 Person2 就有了 Person1 中的所有属性和方法了，相当于 Person2 继承了 Person1 的属性和方法。

## 3.调用函数

apply、call 方法都会使函数立即执行，因此它们也可以用来调用函数。

```javascript
function func() {
    console.log("linxin");
}
func.call(); // linxin
```

# [apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 和 [call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) 的不同点：

-   apply：func.apply(thisArg[, argsArray])
-   call：func.apply(thisArg, arg1, arg2, ...)

    apply 接受可包含多个参数的 数组 （或 array-like 对象），而 call 接受的是可包含若干个参数的 参数列表。

# [apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 和 [call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) 的共同点：

apply ( 同 call )调用一个函数，第一个参数指定 this 值。

thisArg:

-   可选。
-   为函数运行时的使用的 this 值，但使用的 this 值并不一定是该函数执行时的真正的 this 值。
-   在非严格模式下，null 和 undefined 会自动替换成全局对象（浏览器里就是 window ），同时值为原始值（数字、字符串、布尔值）的 this 指向该原始值的包装对象。

# [call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) 和 [bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) 的区别

在 EcmaScript5 中扩展了叫 bind 的方法，在低版本的 IE 中不兼容。它和 call 很相似，接受的参数有两部分，第一个参数是是作为函数上下文的对象，第二部分参数是个列表，可以接受多个参数。
它们之间的区别有以下两点。

## 1.bind 发返回值是函数

```javascript
var obj = {
    name: "linxin"
};

function func() {
    console.log(this.name);
}

var func1 = func.bind(obj);
func1();
```

`func.call(obj)`和`func.bind(obj)()`结果一样
bind 方法不会立即执行，而是返回一个改变了上下文 this 后的函数。而原函数 func 中的 this 并没有被改变，依旧指向全局对象 window。

## 2.参数的使用

```javascript
function func(a, b, c) {
    console.log(a, b, c);
}
func.call(null, "linxin"); //linxin undefined undefined
func.bind(null, "linxin")("A", "B", "C"); //linxin A B
func.bind(null, "linxin")("B", "C"); //linxin B C
```

call 是把第二个及以后的参数作为 func 方法的实参传进去，而 func1 方法的实参实则是在 bind 中参数的基础上再往后排。

在低版本浏览器没有 bind 方法，我们也可以自己实现一个。

```javascript
if (!Function.prototype.bind) {
    Function.prototype.bind = function() {
        var self = this, // 保存原函数
            context = [].shift.call(arguments), // 保存需要绑定的this上下文
            args = [].slice.call(arguments); // 剩余的参数转为数组
        return function() {
            // 返回一个新函数
            self.apply(context, [].concat.call(args, [].slice.call(arguments)));
        };
    };
}
```
