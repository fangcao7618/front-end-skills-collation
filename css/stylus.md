# CSS

-   [stylus](http://stylus-lang.com/)

    -   Stylus 实现迭代生成的 Mixin 实现

    ```css
    iterator(from, to, multiple, unit = 1px, prop, abbpre, abbpost = '')
    for i in (from..to)
      .{abbpre}{i * multiple}{abbpost}
        {prop}: i * multiple * unit
    ```

    使用

    ```css
    // 使用：
    iterator(0, 5, 5, 1px, height, ui-h)
    
    // 生成出：
    .ui-h0 {
        height: 0px;
    }
    .ui-h5 {
        height: 5px;
    }
    .ui-h10 {
        height: 10px;
    }
    .ui-h15 {
        height: 15px;
    }
    .ui-h20 {
        height: 20px;
    }
    .ui-h25 {
        height: 25px;
    }
    ```

[你不知道的 Stylus](https://juejin.im/post/5bbd7a7c6fb9a05cfd27f4c6)
