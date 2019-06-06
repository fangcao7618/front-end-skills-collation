# [servless](https://serverless.com/)

git 地址：https://github.com/serverless/serverless

-   servless 让前端不关心 node 运维，轻松扩容，解决 api 层所有痛点
-   基础运维被云计算打败，servicemesh 更是将基础服务进一步服务化，如果根据流量动态实例化容器，pe 还有必要么？现在数据库单表 60 亿很正常，crud 的 rpc 服务可以量产，异构数据汇总越来越简单，流式处理更是极致。唯有端上，被产品蹂躏。
-   js 跨端开发是必然。无论 rn，nativescript，小程序，还是 flutter，都是极好的尝试，投入产出比极高
-   前端技术栈趋于稳定，接下来就是降低难度，最终服务化，上云。

servless 云服务解决方案，让前端受益最大。应用只会无限增多，前端未来不愁，必然是下一个风口。

关于微前端，有不同的风格，例如，我们可以使用 iframe 来组成最终的视图，或者使用 [Edge Side Include 或 Client Side Include](https://gustafnk.github.io/microservice-websites/)，甚至使用预渲染策略，如 [Open Components](https://opencomponents.github.io/) 或者 [Interface Framework](https://jobs.zalando.com/tech/blog/front-end-micro-services/) 并将结果缓存在 CDN 级别上。
另一种方法是使用一个协调器([orchestrator](https://medium.com/dazn-tech/orchestrating-micro-frontends-a5d2674cbf33))，它正在为 SPA，单个 HTML 页面或 SSR 应用程序提供服务，协调器可以处于边缘，起点或客户端，协调器的一个例子可以是[Single-SPA](https://single-spa.js.org/)。
