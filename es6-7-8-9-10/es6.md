# [ES6](http://es6.ruanyifeng.com/)--(https://github.com/lukehoban/es6features#readme)

[es6 最新提案](http://es6.ruanyifeng.com/#docs/proposals)

> JavaScript（ES6）方法名（函数名）支持表达式运算！

```javascript
//ES6中的 函数名也支持表达式运算！

let aa = "method";
let bb = "Test";

let Person = class {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    [aa + bb]() {
        console.log(`名字为：${this.name}`);
    }
    [aa]() {
        console.log(`名字为：${this.name}`);
    }
};

let person = new Person("kirin", 18);

person[aa] === person.method && console.log("true");
person[aa + bb] === person.methodTest && console.log("true");

console.log([aa + bb]); //输出结果为：数组：["methodTest"]
```

> [do { .. } 表达式](http://es6.ruanyifeng.com/#docs/proposals#do-%E8%A1%A8%E8%BE%BE%E5%BC%8F)<br> > [throw 表达式](http://es6.ruanyifeng.com/#docs/proposals#throw-%E8%A1%A8%E8%BE%BE%E5%BC%8F)<br>
> 链判断运算符<br>
> 函数的部分执行<br>
> 管道运算符<br>
> 数值分隔符<br>
> BigInt 数据类型<br>
> Math.signbit()<br>
> 双冒号运算符<br>
> ECMAScript 6 中的默认参数

-   扩展运算符(…)

    ```javascript
    var myArray = [5, 10, 50];
    Math.max(myArray); // Error: NaN
    Math.max.apply(Math, myArray); //50
    Math.max.apply({}, myArray); //50

    Math.max(...myArray); //50
    Math.max.call(...myArray);
    ```

-   ECMAScript 6 中的默认参数

    ```javascript
    function foo(param1, param2) {
        if (param1 === undefined) {
            param1 = 10;
        }
        if (param2 === undefined) {
            param2 = 10;
        }
        console.log(param1, param2);
    }
    比;
    function foo(param1, param2) {
        param1 = param1 || 10;
        param2 = param2 || 10;
        console.log(param1, param2);
    }
    安全;
    ```

    这个安全又简写;
    正如你所看到的，调用函数时，省略参数会触发默认值，但是传入 0 和 null 并不会触发默认值。

    ```javascript
    function foo(a = 10, b = 10) {
        console.log(a, b);
    }

    function getParam() {
        alert("getParam was called");
        return 3;
    }
    function multiply(param1, param2 = getParam()) {
        return param1 * param2;
    }
    multiply(2, 5); // 10
    multiply(2); // 6 (also displays an alert dialog)
    ```

    注意，函数 getParam 仅仅在第二个参数缺省时才会被调用。因此当我们传入两个参数调用函数 multiply()时，alert 并不会显示。
    默认参数另一个有趣的特性是，在函数声明中我们可以引用其他参数和变量：

    ```javascript
    function myFunction(a = 10, b = a) {
        console.log("a = " + a + "; b = " + b);
    }
    myFunction(); // a=10; b=10
    myFunction(22); // a=22; b=22
    myFunction(2, 4); // a=2; b=4
    ```

    你甚至可以在函数声明中执行操作：

    ```javascript
    function myFunction(a, b = ++a, c = a * b) {
        console.log(a, b, c);
    }
    myFunction(5);
    //6 6 36
    ```

    <b style="color:red">注意，与其它语言不同，JavaScript 是在调用函数时计算默认值。</b>

    ```javascript
    function add(value, array = []) {
        array.push(value);
        return array;
    }
    add(5); // [5]
    add(6); // [6], not [5, 6]
    ```

-   解构

    ```javascript
    function initiateTransfer(options) {
        var protocol = options.protocol,
            port = options.port,
            delay = options.delay,
            retries = options.retries,
            timeout = options.timeout,
            log = options.log;
        // code to initiate transfer
    }
    options = {
        protocol: "http",
        port: 800,
        delay: 150,
        retries: 10,
        timeout: 500,
        log: true
    };
    initiateTransfer(options);
    ```

    这种模式 JavaScript 的开发者经常使用，并且效果非常好，但是我们必须得看函数的内部才知道它需要哪些参数。使用解构参数，在函数声明时，我们可以清晰地表明需要哪些参数：

    ```javascript
    function initiateTransfer({
        protocol,
        port,
        delay,
        retries,
        timeout,
        log
    }) {
        // code to initiate transfer
    }
    var options = {
        protocol: "http",
        port: 800,
        delay: 150,
        retries: 10,
        timeout: 500,
        log: true
    };
    initiateTransfer(options);
    ```

    在这个函数中，我们使用了对象解构模式替代了配置对象。这使得我们的函数不仅更简洁，而且易读性更强。
    我们可以结合使用解构参数和常规参数：

    ```javascript
    function initiateTransfer(
        param1,
        { protocol, port, delay, retries, timeout, log }
    ) {
        // code to initiate transfer
        console.log(protocol, port, delay, retries, timeout, log);
    }
    initiateTransfer("some value", options); //undefined undefined undefined undefined undefined undefined
    ```

    在这个函数中，一个空对象作为默认值赋值给了解构参数。现在，如果这个函数不带任何参数被调用，并不会报错。

    我们也可以给每个解构参数赋默认值：

    ```javascript
    function initiateTransfer({
        protocol = "http",
        port = 800,
        delay = 150,
        retries = 10,
        timeout = 500,
        log = true
    }) {
        // code to initiate transfer
        console.log(protocol, port, delay, retries, timeout, log);
    }
    initiateTransfer({}); //http 800 150 10 500 true
    ```

-   值参数

    从技术上讲，JavaScript 只能通过值传递。当我们传递一个值参数给函数时，该值会在函数的作用域内创建一个副本。因此，对值的任何更改只会在函数内部有所体现。思考这个示例：

    ```javascript
    var a = 5;
    function increment(a) {
        a = ++a;
        console.log(a);
    }
    increment(a); // 6
    console.log(a); // 5
    ```

    在这里，修改函数内部的参数对原始值没有影响。因此，在函数外部变量被记录时，打印的值仍然是 5。

-   引用参数

    在 JavaScript 中，一切都是通过值传递的，但是当我们传递一个对象（包含数组）变量时，“值”是对象的引用，更改变量引用对象的属性会更改底层对象。

    ```javascript
    function foo(param) {
        param.bar = "new value";
    }
    let obj = {
        bar: "value"
    };
    console.log(obj.bar); // value
    foo(obj);
    console.log(obj.bar); // new value
    ```

    正如你所看到的，函数内部的对象属性被修改，但修改后的值在函数外部是可见的。

    当我们传递一个非基本值时，如数组或对象，在背后就会创建一个变量并指向内存中的原始对象的位置。然后这个变量传递给函数，并且修改它就会影响到原始对象。

-   类型检查和缺省或额外的参数
    -   实参的数量少于形参缺少的参数将等于 undefined。
    -   实参的数量大于形参额外的参数将被忽略，但是可以通过特殊数组比如变量 arguments 来检索
-   必要参数

    如果函数调用时缺省一个参数，那么这个参数将被设置为 undefined。我们可以利用这种行为，如果省略参数抛出一个错误：

    ```javascript
    function foo(mandatory, optional) {
        if (mandatory === undefined) {
            throw new Error("Missing parameter: mandatory");
        }
    }
    foo();
    ```

    在 ECMAScript 6 中，我们可以更进一步，使用默认参数来设置必要参数：

    ```javascript
    function throwError() {
        throw new Error("Missing parameter");
    }
    function foo(param1 = throwError(), param2 = throwError()) {
        // do something
    }
    foo(10, 20); // ok
    foo(10); // Error: missing parameter
    ```

-   参数对象

    ```javascript
    function checkParams(param1) {
        console.log(arguments); //数组对象
        console.log(param1); // 2
        console.log(arguments[0], arguments[1]); // 2 3
        console.log(param1 + arguments[0]); // 2 + 2
    }
    checkParams(2, 3);
    ```

    这个函数预计只接受一个参数。当我们使用两个参数调用它时，第一个参数通过参数名称 param1 或者参数对象 arguments[0]在函数中是可以访问的，但是第二个参数只能通过 arguments[1]来访问。此外注意，arguments 对象可以和参数名称一起使用。

    arguments 对象包含传递给函数每个参数的入口，并第一个入口的索引从 0 开始。如果我们想在上面的示例中访问更多的参数，我们会写 arguments[2]和 arguments[3]等。

    我们甚至可以完全跳过设置命名参数，仅仅使用 arguments 对象：

    ```javascript
    function checkParams() {
        console.log(arguments[1], arguments[0], arguments[2]);
    }
    checkParams(2, 4, 6); // 4 2 6
    ```

    事实上，命名参数是为了方便，而不是必要的。类似地，rest 参数也可以用来体现所传递的参数：

    ```javascript
    function checkParams(...params) {
        console.log(params[1], params[0], params[2]); // 4 2 6
        console.log(arguments[1], arguments[0], arguments[2]); // 4 2 6
    }
    checkParams(2, 4, 6);
    ```

    arguments 对象是一个数组对象，但是它缺少数组的方法，比如 slice()和 foreach()。为了在 arguments 对象上使用数组的方法，该对象首先需要转换为一个真正的数组：

    ```javascript
    function sort() {
        var a = Array.prototype.slice.call(arguments);
        return a.sort();
    }
    sort(40, 20, 50, 30); // [20, 30, 40, 50]
    ```

    ECMAScript 6 有一个更简单的方法。Array.from()是 ECMAScript 6 中新增的，它可以把一个数组对象创建成一个数组：

    ```javascript
    function sort() {
        var a = Array.from(arguments);
        return a.sort();
    }
    sort(40, 20, 50, 30); // [20, 30, 40, 50]
    ```

-   length 属性

    虽然参数对象不是数组，但是它拥有 length 属性，这可以用来检查传入函数中的参数数量：

    ```javascript
    function countArguments() {
        console.log(arguments.length);
    }
    countArguments(); // 0
    countArguments(10, null, "string"); // 3
    ```

    rest 参数是数组，所以他们有一个 length 属性。在 ECMAScript 中 6 中，前面的代码可以使用 rest 参数这么来改写：

    ```javascript
    function foo(...params) {
        if (params.length < 2) {
            throw new Error("This function expects at least two arguments");
        } else if (params.length === 2) {
            // do something
        }
    }
    ```

-   callee 和 caller 属性

    callee 属性是指当前正在运行的函数，caller 是指调用了当前函数的执行函数。在 ECMAScript 5 严格模式下，这些属性已被废弃，如果尝试访问这些属性会导致类型错误。

    arguments.callee 属性在递归函数（递归函数是指有规律的自己调用自己的函数）中是有用的，尤其是当函数名称不可用（即匿名函数）时。因为匿名函数没有名称，只能通过 arguments.callee 属性调用自己。

    ```javascript
    var result = (function(n) {
    if (n <= 1) {
        return 1;
    } else {
        return n * arguments.callee(n - 1);
    }
    ```

-   严格模式和非严格模式中的参数对象

    在 ECMAScript 5 非严格模式下，arguments 对象有一个不同寻常特性：它可以与相应的命名参数的值保持同步。
    思考下面的代码：

    ```javascript
    function foo(param) {
        console.log(param === arguments[0]); // true
        arguments[0] = 500;
        console.log(param === arguments[0]); // true
        return param;
    }
    foo(200); // 500
    ```

    在这个函数中，一个新值赋给了 arguments[0]。因为 arguments 的值始终和相应的命名参数保持同步，改变 arguments[0]的值也将改变 param 的值。事实上，他们像两个不同名称的相同变量。在 ECMAScript 5 严格模式下，这个令人困惑的 arguments 对象的行为已被删除：

    ```javascript
    "use strict";
    function foo(param) {
        console.log(param === arguments[0]); // true
        arguments[0] = 500;
        console.log(param === arguments[0]); // false
        console.log(param, "===", arguments[0]);
        return param;
    }
    foo(200); // 200
    ```

    此时，改变 arguments[0]并不会影响 param,并且输出结果和预期的一样。该函数的输出结果在 ECMAScript 6 中和 ECMAScript 5 严格模式下是相同的，但是记住，当函数声明中使用默认值时，arguments 对象并不受影响：

    ```javascript
    function foo(param1, param2 = 10, param3 = 20) {
        console.log(
            "param1 === arguments[0]",
            param1,
            arguments[0],
            param1 === arguments[0]
        ); // true
        console.log(
            "param2 === arguments[1]",
            param2,
            arguments[1],
            param2 === arguments[1]
        ); // true
        console.log(
            "param3 === arguments[2]",
            param3,
            arguments[2],
            param3 === arguments[2]
        ); // false
        console.log("arguments[2]", arguments[2]); // undefined
        console.log("param3", param3); // 20
    }
    foo("string1", "string2");
    ```

    在这个函数中，即使 param3 有默认值，它也和 arguments[2]不相等，因为只有两个参数传递给了函数。换句话说，设置默认值并不影响 arguments 对象。
