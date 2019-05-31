# js 的各种 es5 转 es6 解析

## 循环参数（扩展运算符(…)的使用）

```javascript
function myFunction() {
    for (var i in arguments) {
        console.log(arguments[i]);
    }
}
var params = [10, 15];
myFunction(5, ...params, 20, ...[25]);
```

扩展操作符的另一个优点是，它可以很容易地与构造函数一起使用：

```javascript
new Date(...[2016, 5, 6]); // Mon Jun 06 2016 00:00:00 GMT-0700 (Pacific Daylight Time)
```

当然，我们可以在 ECMAScript 5 中重写上面的代码，但我们需要使用复杂的模式来避免类型错误：

```javascript
new Date.apply(null, [2016, 4, 24]); // TypeError: Date.apply is not a constructor
new (Function.prototype.bind.apply(Date, [null].concat([2016, 5, 6])))(); // Mon Jun 06 2016 00:00:00 GMT-0700 (Pacific Daylight Time)
```

浏览器支持详情[点我查看](https://kangax.github.io/compat-table/es6/)感谢 kangax，已经 fork 到[compat-table](https://github.com/fangcao7618/compat-table)or[caniuse](https://caniuse.com/#feat=classlist)。

## Rest 参数

rest 参数的语法与扩展运算符一样，但是它不是展开一个数组到参数中，相反它是将参数收拢为一个数组：

```javascript
function myFunction(...options) {
    return options;
}
myFunction("a", "b", "c"); //["a", "b", "c"]
```

当创建一个参数可变的函数（即接受的参数数量是有变化的函数）时，rest 参数是非常有用的。得益于数组的好处，reset 参数可以容易地替换掉 arguments 对象（我们将在本教程后面解释）。思考这个使用 ECMAScript 5 写的函数：

```javascript
function checkSubstring(string) {
    console.log("string", string);
    console.log("arguments", arguments);

    for (var i = 1; i < arguments.length; i++) {
        console.log(`arguments[${i}]`, arguments[i]);
        if (string.indexOf(arguments[i]) === -1) {
            return false;
        }
    }
    return true;
}
checkSubstring("this is a string", "is", "this");
// string this is a string
// arguments
//Arguments(3) ["this is a string", "is", "this", callee: ƒ, Symbol(Symbol.iterator): ƒ]
//arguments[1] is
//arguments[2] this
//true
```

这个函数检查一个字符串是否包含一些子字符串。这个函数的第一个问题是，我们必须得看函数的内部才知道它需要多个参数。第二个问题是循环迭代必须从索引 1 而不是 0 开始，因为 arguments[0]指向第一个参数。如果我们以后决定在 string 之前或者之后添加另一个参数，我们可能会忘记更新 for 循环的逻辑处理。使用 rest 参数，我们就很容易地避免这些问题：
