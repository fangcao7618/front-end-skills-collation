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

> 扩展运算符(…)

```javascript
var myArray = [5, 10, 50];
Math.max(myArray); // Error: NaN
Math.max.apply(Math, myArray); //50
Math.max.apply({}, myArray); //50

Math.max(...myArray); //50
Math.max.call(...myArray);
```
