# 距当前时刻 fromNow

```javascript
// 根据正负判断是之前还是之后
function toText(val, before, after) {
    return Math.abs(val) + (val > 0 ? before : after);
}

function fromNow(date, format) {
    let ms = Date.now() - date.getTime();

    // 误差修正
    if (ms > 0) ms += 1000;
    else ms -= 1000;

    const minute = parseInt(ms / 1000 / 60);
    const hour = parseInt(minute / 60);
    const day = parseInt(hour / 24);
    const month = parseInt(day / 30);
    const year = parseInt(day / 365);

    if (year) return toText(year, "年前", "年后");
    else if (month) return toText(month, "个月前", "个月后");
    else if (day) return toText(day, "天前", "天后");
    else if (hour) return toText(hour, "小时前", "小时后");
    else if (minute) return toText(minute, "分钟前", "分钟后");
    else return ms > 0 ? "刚刚" : "不到一分钟之后";
}
```
