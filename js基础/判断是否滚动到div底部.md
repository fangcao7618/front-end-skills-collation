# 判断是否滚动到 div 底部

-   clientHeight 可见高度
-   offsetHeight 可见高度＋边框
-   scrollHeight 内容高度
-   scrollTop 滚动高度

判断是否滚动到底部：

```javascript
clientHeight + scrollTop === scrollHeight;
```

判断是否距底部 100 像素以内：

```javascript
scrollHeight - clientHeight - scrollTop < 100;
```
