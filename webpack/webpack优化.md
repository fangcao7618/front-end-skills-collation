-   [Topics](https://github.com/topics) 浏览 Github 上最常用的主题
-   [GitHub Explore](https://github.com/github/explore)

# webpack 插件

-   [Webpack](https://github.com/topics/webpack) Webpack 是一个 bundler，它接受具有依赖关系的模块并创建静态资产。它旨在简化和增强开发和用户体验。
-   [webpack-deep-scope-analysis-plugin](https://github.com/vincentdchan/webpack-deep-scope-analysis-plugin) 这个插件能够大大提高 webpack tree-shaking 的效率
-   [Webpack Deep Scope Analysis Demo](https://diverse.space/webpack-deep-scope-demo/) Webpack Deep Scope Analysis Plugin 插件优化过的例子

## tree-shaking 目前的缺陷

tree-shaking 作为 rollup 的一个杀手级特性，能够利用 ES6 的静态引入规范，减少包的体积，避免不必要的代码引入，webpack2 也很快引入了这个特性，但是目前，webpack 只能做比较简单的解决方案，比如：

![](./1659e5ceb0c7eca3)

这个例子中，webpack 会寻找引入变量的引用，当发现没有对 isNumber 的引用时，就会去除 isNumber 的代码。这其实不太实用，毕竟在现在的`vscode`中，`没有引用的变量在ide中都会灰显提示`，一般不会犯这种 import 某个模块却不用的错误了。

如果是接下来这种引入方式呢，我写了一个 demo 如下:
![](./1659e5ceb0d8806a)
这个例子非常简单，如果用图来表示是这样
![](./1659e5ceb0a04d93)

在 index.js 中引入了 func.js 中的 func2，并没有引入 func1，但是 func1 引入了 lodash。webpack 检查的时候发现 func.js 中的确用到了 lodash，所以不会把 lodash 去掉。实际上，我们根本没用到它。

webpack-deep-scope-analysis-plugin 就可以解决这种判断。

## 插件效果

---

<b style="color:red">引入前</b>
![](./1659e5ceb0beb7be)
<b style="color:red">引入后</b>
![](./1659e5ceb0b6a8f4)

85.8kb -> 不到 1kb

当然，我这里是标题党了，因为这里直接把一个 lodash 库给去掉了，所以变化才这么惊人。但是即使在实际项目中，我们也能轻易用一个插件减少大量的不必要的引入。

# 原理

那么这个插件是怎么去解决这个问题的呢？这里根据原作者在 Medium 上写的文章，简单介绍一下他的做法。

webpack 的原理，其实就是遍历所有的模块，把它们打包成一个文件，在这个过程中，它就知道哪些 export 的模块有被使用到。那我们同样也可以遍历所有的 scope（作用域），简化没有用到的 scope，最后只留下我们需要的。

![](./1659e5ceb08e21cb)

上图中，func5 层层引用 fun4 fun3 fun2 fun1，最后解析出来其实只使用了 deepEqual 模块。

什么是 scope 呢，其实 scope 在各个语言中都有存在，在 Wikipedia 中是作为计算机术语，有更详细的解释，我觉得可以翻译为作用域或者上下文，在 ECMAScript 中，有以下明确的定义：

```javascript
// module scope start

// Block

{ // <- scope start
} // <- scope end

// Class

class Foo { // <- scope start

} // <- scope end

// If else

if (true) { // <- scope start

} /* <- scope end */ else { // <- scope start

} // <- scope end

// For

for (;;) { // <- scope start
} // <- scope end

// Catch

try {

} catch (e) { // <- scope start

} // <- scope end

// Function

function() { // <- scope start
} // <- scope end

// Scope

switch() { // <- scope start
} // <- scope end

// module scope end
```

在 ES6 中，module 是一种根作用域，只有 function 和 class 才能作为子作用域被导出，所以我们解析的时候，不会把所有的 scope 都作为节点算进去。

我们提到的这个 webpack 插件，正是内置了这样一个 scope 分析器，它能够从入口文件中分析出 scope 的引用关系，最后排除掉所有没有用到的模块。

当然，这个插件也并不是自己做了所有的事情，它也是依赖于了前人的工作。 escope 是一个分析 ES 中 scope 的工具，插件作者将它改成了 ts 版本集成到了插件中，并且利用了 webpack 暴露的接口，可以解析出来的模块的 AST 树，基于这个 AST 就可以交给 escope 分析出 scope 的引用关系。

# 一些边际用例

凡事不能完美，这个插件也有一些情况会导致判断失误

-   情况一：重复赋值变量

    ```javascript
    import { isNull } from "lodash-es";

    var fun = 1;

    fun = function scope(...args) {
        return isNull(...args);
    };

    export { fun };
    ```

    这个例子中 fun 变量一开始被赋值为数字，然后被赋值成一个函数，但是 scope 分析器会直接跳过这个变量，不把它当作一个单独的 scope。

-   情况二：纯函数

        ```javascript
        // copy from rambda/es/allPass.js
        import _curry1 from "./internal/_curry1";
        import curryN from "./curryN";
        import max from "./max";
        import pluck from "./pluck";

        var allPass = /*#__PURE__*/ _curry1(function allPass(preds) {
            return curryN(reduce(max, 0, pluck("length", preds)), function() {
                var idx = 0;
                var len = preds.length;
                while (idx < len) {
                    if (!preds[idx].apply(this, arguments)) {
                        return false;
                    }
                    idx += 1;
                }
                return true;
            });
        });
        export default allPass;
        ```

    在这个例子中，`import allPass` 会导致`\_curry1` 的运行，因此它不会被当作一个单独的 `scope`，因为它可能会有一些“副作用”，比如改变某个全部变量，对全局造成影响。

    所以作者给了个方案，可以在这个函数前加/_#**PURE**_/，这样就会把这个函数视为无副作用的纯函数，如果我们没有 `import allPass`，它引用的其他模块都会被去除。

# 实际使用的过程中应该注意什么

-   必须使用 ES6 的 import/export 模块机制
    -   其实整个深作用域分析都是基于 ES6 模块完成的，也就是说深作用域分析无法分析 CommonJS 和 AMD 等等模块规范。这个时候，就要求项目中引用的模块都遵循 ES6 的规范，比如使用 lodash-es 代替 lodash。另外就是要注意 babel-loader 和 TypeScript 的设置，是否会把代码转换到 ES5 语法，导致深作用域分析失效。
-   学会使用 PURE 注释
    -   由于 JS 语法的复杂程度，webpack 没有打算给 JS 实现数据流分析，所以插件是无法知道一个函数调用是否具有副作用的。所以对于一些导出模块，如果是纯的函数调用，则需要加上 /_@**PURE**_/ 注释来表明这个函数是 pure 的，这是 Uglify 使用的方法。当然也可以使用相关的 babel 插件进行批量添加。

# 最佳实践

首先，要用到`tree-shaking`，必然要保证引用的模块都是 ES6 规范的。这也是为什么我在前面的 demo 中，引入的是`lodash-es`而不是`lodash`。

在项目中，注意要把 babel 设置`module: false`，避免 babel 将模块转为 CommonJS 规范。引入的模块包，也必须是符合 ES6 规范，并且在最新的 webpack 中加了一条限制，即在 package.json 中定义`sideEffect: false`，这也是为了避免出现`import xxx`导致模块内部的一些函数执行后影响全局环境，却被去除掉的情况。

-   文章出处
    -   github 项目地址：https://github.com/vincentdchan/webpack-deep-scope-analysis-plugin
    -   原文出处:https://diverse.space/2018/05/better-tree-shaking-with-scope-analysis/

作者：腾讯 IVWEB 团队
链接：https://juejin.im/post/5b8ce49df265da438151b468
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
