# react中使用canvas播放视频
```javascript react
import React from "react";
export const draw = (canvas, url) => {
    if (!canvas) {
        return;
    }
    if (canvas) {
        //获取canvas上下文
        let ctx = canvas.getContext("2d");

        //创建video标签，并且设置相关属性
        let video = document.createElement("video");

        video.preload = true;
        video.autoplay = true;
        video.src = url;
        //document.body.appendChild(video);

        //监听video的play事件，一旦开始，就把video逐帧绘制到canvas上
        video.addEventListener(
            "play",
            () => {
                let play = () => {
                    ctx.drawImage(video, 0, 0);
                    requestAnimationFrame(play);
                };

                play();
            },
            false
        );

        //暂停/播放视频
        canvas.addEventListener(
            "click",
            () => {
                if (!video.paused) {
                    video.pause();
                } else {
                    video.play();
                }
            },
            false
        );
    }
};
export default class App extends React.Component {
    componentDidMount() {
        if (this.canvas) {
            draw(this.canvas, require("./b_MuvpZ1v73IfC1556365516.mp4"));
        }
        if (this.canvas2) {
            draw(this.canvas2, require("./b_MuvpZ1v73IfC1556365516.mp4"));
        }
        if (this.canvas3) {
            draw(this.canvas3, require("./b_MuvpZ1v73IfC1556365516.mp4"));
        }
    }
    render() {
        return (
            <div style={{ width: "100%", height: "auto" }} id={"scrollBox"}>
                <canvas
                    width="1280"
                    height="720"
                    ref={node => (this.canvas = node)}
                    style={{
                        width: "100%",
                        height: "auto"
                    }}
                />
                <canvas
                    width="1280"
                    height="720"
                    ref={node => (this.canvas2 = node)}
                    style={{
                        width: "100%",
                        height: "auto"
                    }}
                />
                <canvas
                    width="1280"
                    height="720"
                    ref={node => (this.canvas3 = node)}
                    style={{
                        width: "100%",
                        height: "auto"
                    }}
                />
            </div>
        );
    }
}
```