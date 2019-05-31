# 在 create-react-app 中启用装饰器语法

> 方式一：暴露 create-react-app 的配置

暴露出 create-react-app 的所有配置

运行命令：

```
$ npm run eject
```
项目中就会展示出各种配置文件。

在 `babel` 中添加 `plugins` 配置

在 `package.json` 文件中找到 `babel` 的配置，添加如下代码即可：

`package.json`:

```javascript
"babel": {
    "presets": [
      "react-app"
    ],
+    "plugins": [
+        [
+            "@babel/plugin-proposal-decorators",
+            { "legacy": true }
+        ]
+    ]
}
```
重新运行项目，即可正常使用装饰器语法。

>`create-react-app` 脚手架中已经安装了 `@babel/plugin-proposal-decorators` 插件，如果是自己配置的脚手架，请先安装插件：`npm install @babel/plugin-proposal-decorators --save-dev`

>方式二：直接在项目的 node-modules 中添加配置

打开项目的 node_modules 文件夹，找到 babel-preset-react-app 目录。打开目录下 create.js 文件。找到 plugins 属性配置的地方，修改如下配置即可：

```javascript
-isTypeScriptEnabled && [
-    require('@babel/plugin-proposal-decorators').default,
-    false,
-],

+[
+    require('@babel/plugin-proposal-decorators').default,
+    { legacy: true },
+],
```
>不建议使用方式二，因为一旦需要重新安装 node_modules， 就需要再去 babel-preset-react-app 里面添加一次配置。

# 总结
上面两种方式使用了之后，均可在项目中正常使用装饰器语法，但是使用装饰器时。可能还是会出现红线报错提示，此时在 VSCode 的配置文件中（Visual Studio Code左下角的设置按钮(或者文件>首选项>设置)）添加如下配置即可：
`setting.json`
```json
"javascript.implicitProjectConfig.experimentalDecorators": true,
```

建议使用第一种方式，虽然可能比较麻烦，需要暴露出所有的配置。
但是第二种方式，如果只是自己进行一些小的 Demo 测试还好。不然的话，一旦需要重新安装 node_modules，就需要再重新去 babel-preset-react-app 里面添加一次配置。