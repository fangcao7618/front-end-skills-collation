# HTML5 新特性

-   [DOM](https://dom.spec.whatwg.org/)-DOM Living Standard
-   [JavaScript 教程](https://wangdoc.com/javascript/)
-   Mutation Observer

    -   [mutation-observers-tutorial](https://dev.opera.com/articles/mutation-observers-tutorial/)

    -   [Mutation Observer API](http://javascript.ruanyifeng.com/dom/mutationobserver.html)
    -   [DOM MutationObserver](https://hacks.mozilla.org/2012/05/dom-mutationobserver-reacting-to-dom-changes-without-killing-browser-performance/)
    -   Mutation events list [DOM 3 级 事件规范:](https://www.w3.org/TR/DOM-Level-3-Events/#events-mutationevents)

    -   Mutation events [DOM 变动事件](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Mutation_events)

        -   DOMAttrModified
            -   在特性被修改之后触发。
            -   IE9+/firefox/opera
        -   DOMAttributeNameChanged
        -   DOMCharacterDataModified
            -   在文本节点的值发生变化时触发
            -   IE9+/chrome/firefox/safari/opera
        -   DOMElementNameChanged
        -   DOMNodeInserted
            -   在一个节点作为子节点被插入到另一个节点中时触发
            -   IE9+/chrome/firefox/safari/opera
        -   DOMNodeInsertedIntoDocument
            -   在一个节点被直接插入文档或通过子树间接插入到文档之后触发
            -   这个事件在 DOMNodeInserted 之后触发
            -   /chrome/safari/opera
        -   DOMNodeRemoved
            -   在节点从其父节点中被移除时触发
            -   IE9+/chrome/firefox/safari/opera
        -   DOMNodeRemovedFromDocument
            -   在一个节点被直接从文档中移除或通过子树间接从文档中移除之前触发
            -   这个事件在 DOMNodeRemoved 之后触发
            -   /chrome/safari/opera
        -   DOMSubtreeModified
            -   在 DOM 结构中发生的任何变化时触发
            -   这个事件在其他任何事件触发后都会触发
            -   IE9+/chrome/firefox/safari

        ```javascript
        element.addEventListener(
            "DOMNodeInserted",
            function(event) {
                // ...
            },
            false
        );
        ```

    ![](./eventflow.svg)
