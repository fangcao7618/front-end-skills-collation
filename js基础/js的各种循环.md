```javascript
function myFunction() {
    for (var i in arguments) {
        console.log(arguments[i]);
    }
}
var params = [10, 15];
myFunction(5, ...params, 20, ...[25]);
```
