# 由 requestAnimationFrame 谈浏览器渲染优化.md

requestAnimationFrame 这个 API，可能很多人都听过，但并没有真正用过。MDN 上的解释是：

> window.requestAnimationFrame() 方法告诉浏览器您希望执行动画，并请求浏览器调用指定的函数在下一次重绘之前更新动画。该方法将在重绘之前调用的回调作为参数。

在 three.js 里，requestAnimationFrame 主要用在渲染器 renderer 里，作为优化动画的解决方案。当然在 js animation 中也需要用到 requestAnimationFrame。在谈此之前，我们就 three.js 的应用场景，来简单介绍一下动画的相关概念。

> 动画的本质是利用了人眼的视觉暂留特性，快速地变换画面，从而产生物体在运动的假象。而对于 Three.js 程序而言，动画的实现也是通过在每秒中多次重绘画面实现的。

为了衡量画面切换速度，引入了每秒帧数 FPS(Frames Per Second)的概念，是指每秒画 面重绘的次数。(这也是在游戏中经常遇到的 FPS，打过 lol 的都知道，一般 FPS 越高，画面会越流畅)FPS 越大，则动画效果越平滑，当 FPS 小于 20 时，一般就能明显感受到 画面的卡滞现象。

当然，当 FPS 足够大(比如达到 60)，再增加帧数 人眼也不会感受到明显的变化，反而相应地就要消耗更多资源(比如电影的胶片就需要更 长了，或是电脑刷新画面需要消耗计算资源等等)

`setInterval` 与`setTimeout`
一般做动画而言，我们第一想到的就是使用`setInterval`或者`setTimeout`来实现，如下：

```javascript
function animate() {
    // ...
}
setInterval(animate, 200);
```

or

```javascript
function animate() {
    setTimeout(animate, 200);
}
animate();
```

由于大部分屏幕刷新的频率是 60HZ,所以我们要做的就是尽量去让帧率达到 60fps。

```javascript
//1000ms/60 = 16.7ms，故约等于17
setInterval(animate, 17);
```

然后，setInterval 与 setTimeout 所设定的时间，并不一定按照间隔来执行。由于浏览器是单线程的缘故，事件都是按异步队列执行，如果执行 setTimeout/setInterval 时，有大量的异步事件在等待执行，浏览器线程只能让其等待，这样 delay 肯定时大于所设置的时间。

出现的问题：

1. 浏览器依然在执行一些不必要的动画，或者异步事件，尽管 chrome 会对 setInterval 以及 setTimout 在 1fps 做节流处理，但其他浏览器并没有
2. setTimeout 只会在浏览器想要更新的时候更新，而不会考虑计算机是否能够更新，这就意味着当你在重绘整个屏幕的时候，浏览器不得不重绘动画，此时当你的动画帧率跟屏幕重绘得帧率不同步时，于是会耗费更多的电量，这就意味着高 CPU 使用率。
3. 另一个要考虑的是多个元素立即发生的运动。一个解决方法是将所有动画逻辑放到同一个间隔以此来解决可能的动画调用，即使特定元素可能不需要当前帧的任何动画

## requestAnimationFrame

为了解决上述问题 requestAnimationFrame 产生了

```javascript
function animate() {
    requestAnimationFrame(animate);
}
animate();
//requestAnimationFrame(animate, element)   //可以定义当前节点
```

`requestAnimationFrame`的帧率取决于你的浏览器以及计算机，但一般来说都是`60fps`。`requestAnimationFrame`关键的就是他只是请求浏览器在下一次可以获得的机会去展示一帧画面，而不是在一个已经规划好的间隔。也就是说浏览器能够根据页面加载，元素显示，电池的状态来选择`requestAnimationFrame`的性能。

另外一个`requestAnimationFrame`的优点是它能够将所有的动画都放到一个浏览器重绘周期里去做，这样能保存你的 CPU 的循环次数，让你的设备存活时间更长。

当然在用`requestAnimationFrame`设置动画后，当页面出现新的 tab 后，动画也会停止，从而减少计算机的开销。

下面是`requestAnimationFrame`的`polyfill`

```javascript
(function() {
    var lastTime = 0;
    var vendors = ["ms", "moz", "webkit", "o"];
    for (var x = 0; x < vendors.length && !window.requestAnimationFrame; ++x) {
        window.requestAnimationFrame =
            window[vendors[x] + "RequestAnimationFrame"];
        window.cancelAnimationFrame =
            window[vendors[x] + "CancelAnimationFrame"] ||
            window[vendors[x] + "CancelRequestAnimationFrame"];
    }

    if (!window.requestAnimationFrame)
        window.requestAnimationFrame = function(callback, element) {
            var currTime = new Date().getTime();
            var timeToCall = Math.max(0, 16 - (currTime - lastTime));
            var id = window.setTimeout(function() {
                callback(currTime + timeToCall);
            }, timeToCall);
            lastTime = currTime + timeToCall;
            return id;
        };

    if (!window.cancelAnimationFrame)
        window.cancelAnimationFrame = function(id) {
            clearTimeout(id);
        };
})();
```

通过以上 polyfill 可知，requestAnimationFrame 可以用 setTimeout 来改写，但注意到所有的 callback 的执行时间都控制在 16ms 以内，这也就说明了，requestAnimationFrame 每次的执行都是在页面刷新频率以内的。

当然，我们也可以自己来控制帧率

```javascript
var fps = 15;
function animate() {
    setTimeout(function() {
        requestAnimationFrame(animate);
    }, 1000 / fps);
}
```

当然还有更复杂的办法

```javascript
var time;
function animate() {
    requestAnimationFrame(animate);
    var now = new Date().getTime(),
        dt = now - (time || now);

    time = now;

    // 比如更新x的位置:
    this.x += 10 * dt;
    // 每毫秒增加10个单位
}
```

那么我们如何在实际开发中用 requestAnimationFrame 来优化呢？我们假设有这样的应用场景，如果页面要加载上千甚至上万张图片(或者说是 li)，我们模拟的就是成千上万个 dom。 分析：出现卡顿感的主要原因是每次循环都会修改 DOM 结构，考虑上万张图片，用户不会立即看到，所以我们可以缩短循环次数，并且减少 DOM 操作来进行优化。

-   减少操作 DOM，我们可以使用[DocumentFragment 英文版](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment)、[DocumentFragment 中文版](https://developer.mozilla.org/zh-CN/docs/Web/API/DocumentFragment)
-   减少循环时间，使用分治的思想，把 30000 个 li 分批次插入到页面中，每次插入的时机是在页面重新渲染之前

下面是完整的代码示例：(在这里用背景颜色代替图片)

```html
<ul id="js-list"></ul>
```

```css
ul#js-list {
    padding: 0px;
    display: flex;
    flex-wrap: wrap;
}
li {
    list-style: none;
    text-align: center;
    line-height: 50px;
    width: 50px;
    height: 50px;
    border: 1px solid #000;
}
```

```javascript
(() => {
    const ndContainer = document.getElementById("js-list");
    if (!ndContainer) {
        return;
    }

    const total = 30000;
    const batchSize = 4; // 每批插入的节点次数，越大越卡
    const batchCount = total / batchSize; // 需要批量处理多少次
    let batchDone = 0; // 已经完成的批处理个数

    function getRandomColor() {
        return "#" + Math.floor(Math.random() * 16777215).toString(16);
    }

    function appendItems() {
        const fragment = document.createDocumentFragment();
        for (let i = 0; i < batchSize; i++) {
            const ndItem = document.createElement("li");
            ndItem.innerText = batchDone * batchSize + i + 1;
            ndItem.style.backgroundColor = getRandomColor();
            fragment.appendChild(ndItem);
        }

        // 每次批处理只修改 1 次 DOM
        ndContainer.appendChild(fragment);

        batchDone += 1;
        doBatchAppend();
    }

    function doBatchAppend() {
        if (batchDone < batchCount) {
            window.requestAnimationFrame(appendItems);
        }
    }

    // kickoff
    doBatchAppend();

    ndContainer.addEventListener("click", function(e) {
        const target = e.target;
        if (target.tagName === "LI") {
            alert(target.innerHTML);
        }
    });
})();
```

以上感谢王仕军老师提供的思想跟方法和 PerkinJ 的文章。

实现的效果如图[DEMO](http://localhost:3000/#/RequestAnimationFrame)

> 其实 requestAnimationFrame 也运用到了 react fiber 跟 angular 中，本文在这里不做详细讲解，后期会针对 React Fiber 与 requestAnimayonFrame 再做一次深入探究

## 参考文档

-   [requestAnimationFrame](http://creativejs.com/resources/requestanimationframe/)
-   [How JavaScript Timers Work](https://johnresig.com/blog/how-javascript-timers-work/)
