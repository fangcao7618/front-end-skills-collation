
# React Canvas：高性能渲染 React 组
```javascript
import React from "react";
import { Surface, Image, Text } from "react-canvas";

var MyComponent = React.createClass({
    render: function() {
        var surfaceWidth = window.innerWidth;
        var surfaceHeight = window.innerHeight;
        var imageStyle = this.getImageStyle();
        var textStyle = this.getTextStyle();

        return (
            <Surface
                width={surfaceWidth}
                height={surfaceHeight}
                left={0}
                top={0}
            >
                <Image style={imageStyle} src="..." />
                <Text style={textStyle}>Here is some text below an image.</Text>
            </Surface>
        );
    },

    getImageHeight: function() {
        return Math.round(window.innerHeight / 2);
    },

    getImageStyle: function() {
        return {
            top: 0,
            left: 0,
            width: window.innerWidth,
            height: this.getImageHeight()
        };
    },

    getTextStyle: function() {
        return {
            top: this.getImageHeight() + 10,
            left: 0,
            width: window.innerWidth,
            height: 20,
            lineHeight: 20,
            fontSize: 12
        };
    }
});
export default class App extends React.Component {
    render() {
        return (
            <div>
                <MyComponent />
                <div>fadsfasd</div>
            </div>
        );
    }
}

```