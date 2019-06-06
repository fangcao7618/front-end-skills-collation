# 测量 vue 应用运行时的性能

原文 TIPs：https://vuedose.tips/tips 英文版一共 22 篇

原文：https://vuedose.tips/tips/measure-runtime-performance-in-vue-js-apps/

翻译文：

在上一篇文章中，我们讨论了如何提高大型数据的性能。但是我们还没有测量它提高了多少。

我们可以使用 Chrome DevTools 的性能选项来实现这一点。但是为了获取准确数据，我们必须在 Vue 上激活性能模式。

我们可以在`main.js`或者插件中设置全局变量，代码如下：

```javascript
Vue.config.performance = true;
```

如果你设置了正确的 NODE_ENV 环境变量，那么可以使用非生产环境做判断。

```javascript
const isDev = process.env.NODE_ENV !== "production";
Vue.config.performance = isDev;
```

这将在 Vue 内部激活标记组件性能的[User Timing API](https://developer.mozilla.org/en-US/docs/Web/API/User_Timing_API)。

上一篇文章内容，我已经在[codesandbox](https://0ql846q66w.codesandbox.io/)上创建了代码。打开 Chrome DevTools 里的 performance 选项并且点击重新加载按钮。
![](./16b204335288bdb9)
这将记录页面加载性能。同时，感谢你在`main.js`中的`Vue.config.performance`设置，这个设置会使你在统计资料能够看到`User Timing`部分。
![](./16b20552b4cb68a8)
在那里，你会发现 3 个指标：

-   Init：创建组件实例需要的时间
-   Render：创建 VDom 结构需要的时间
-   Patch：把 VDom 应用到实际 Dom 的时间

回到上一篇文章好奇（性能提高了多少）的地方，结果是：正常的组件需要 417 毫秒初始化：
![](./16b2067596133a76)
而使用`Object.freeze`阻止了默认反应则只需要`3.9`毫秒：
![](./16b20fc59bec4df6)

当然，每次运行的结果都会有小的变化，但是，仍然有非常巨大的性能差别。由于在创建组件的时候会有默认反应的问题，你可以通过 Init（初始化指标）看到阻止了默认反应和没有阻止的差异。

就是这样！
