# 基于 React 的大文件上传组件的开发详解

## 参考文章

-   [踩坑 Webuploader 视频上传](http://fex.baidu.com/webuploader/)
-   [Plupload](https://www.plupload.com/)
-   [Filereader API](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader)
-   [ArrayBuffer：类型化数组](https://blog.csdn.net/lichwei1983/article/details/43893025)
-   [文件和二进制数据的操作](http://javascript.ruanyifeng.com/htmlapi/file.html)
-   [Blob](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)

实现代码
https://www.jianshu.com/p/0d1421df0e0d

```javascript
import React from "react";
import "./index.css";
import plupload from "plupload";
// import FileUpload from "./FileUpload"; //没有弄完 https://www.jianshu.com/p/0d1421df0e0d

export default class App extends React.Component {
    componentDidMount() {
        var uploader = new plupload.Uploader({
            browse_button: "browse", // this can be an id of a DOM element or the DOM element itself
            url: "upload.php"
        });

        uploader.init();
        uploader.bind("FilesAdded", function(up, files) {
            var html = "";
            plupload.each(files, function(file) {
                html +=
                    '<li id="' +
                    file.id +
                    '">' +
                    file.name +
                    " (" +
                    plupload.formatSize(file.size) +
                    ") <b></b></li>";
            });
            document.getElementById("filelist").innerHTML += html;
        });

        uploader.bind("UploadProgress", function(up, file) {
            document
                .getElementById(file.id)
                .getElementsByTagName("b")[0].innerHTML =
                "<span>" + file.percent + "%</span>";
        });

        uploader.bind("Error", function(up, err) {
            document.getElementById("console").innerHTML +=
                "\nError #" + err.code + ": " + err.message;
        });

        document.getElementById("start-upload").onclick = function() {
            uploader.start();
        };
    }
    render() {
        return (
            <div>
                <ul id="filelist" />
                <br />
                <div id="container">
                    <button id="browse">[Browse...]</button>
                    <button id="start-upload">[Start Upload]</button>
                </div>
                <br />
                <pre id="console" />
                {/* <FileUpload /> */}
                fadsfasdf
                {do {
                    let t = 1;
                    t = t * t + 1;
                    console.log(t);
                }}
            </div>
        );
    }
}
```
