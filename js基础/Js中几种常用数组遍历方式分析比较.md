# [循坏对比](https://dailc.github.io/jsfoundation-perfanalysis/html/performanceAnalysis/demo_performanceAnalysis_jsarrayGoThrough.html)
每次计算次数：10次<br>
一共执行循环：10次

| 测试代码                        |                                                           |                                                          性能分析 |
| ------------------------------- | --------------------------------------------------------- | ----------------------------------------------------------------: |
| for循环执行代码第一种方式       | ```for(j = 0; j < arr.length; j++) {}```                  |              总耗时:11ms 平均每秒执行:9090.91次 执行平均差:89.09% |
| for循环执行代码第一种方式       | ```for(j = 0,len=arr.length; j < len; j++) {} ```         |                               总耗时:10ms 平均每秒执行:10000.00次 |
| for循环执行代码第三种方式       | ```for(j = 0; arr[j]!=null; j++) {}```                    |                总耗时:23ms平均每秒执行:4347.83次执行平均差:50.43% |
| for in循环执行代码              | ```for(j in arr) {}   ```                                 |                总耗时:790ms平均每秒执行:126.58次执行平均差:14.43% |
| for each循环执行代码            | ```arr.forEach(function(e){ });  ```                      |                 总耗时:156ms平均每秒执行:641.03次执行平均差:8.97% |
| for each循环执行代码,第二种方式 | ```Array.prototype.forEach.call(arr,function(el){  });``` |                 总耗时:144ms平均每秒执行:694.44次执行平均差:8.33% |
| for map循环执行代码             | ```arr.map(function(n){  });   ```                        |                 总耗时:217ms平均每秒执行:460.83次执行平均差:8.76% |
| for of 遍历（需要ES6支持）      | ```for(let value of arr) {}) ```                          | 这种方式是es6里面用到的，性能要好于forin，但仍然比不上普通for循环 |