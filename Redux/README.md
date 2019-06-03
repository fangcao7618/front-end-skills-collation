# [Redux](https://redux.js.org/)

-   [react-redux](./react-redux.md)
-   [Redux Starter Kit](./ReduxStarterKit.md)

[开始学习的视频](https://egghead.io/courses/getting-started-with-redux)

-   [Redux 入门教程 #1 课程介绍「05:29」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-1-ke-cheng-jie-shao)
-   [Redux 入门教程 #2 为什么需要 Redux「09:49」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-2-wei-shen-me-xu-yao-redux)
-   [Redux 入门教程 #3 什么是 Redux「08:59」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-3-shen-me-shi-redux)
-   [Redux 入门教程 #4 创建页面「04:19」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-4-chuang-jian-ye-mian)
-   [Redux 入门教程 #5 单独使用 Redux「11:22」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-5-dan-du-shi-yong-redux)
-   [Redux 入门教程 #6 使用 react-redux「Pro」「10:50」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-5-dan-du-shi-yong-redux)
-   [Redux 入门教程 #7 mapStateToProps 和 combineReducers「Pro」「05:33」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-7-mapstatetoprops-he-combinereducers)
-   [Redux 入门教程 #8 dispatch 和 action「Pro」「05:40」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-8-dispatch-he-action)
-   [Redux 入门教程 #9 mapDispatchToProps「Pro」「03:42」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-9-mapdispatchtoprops)
-   [Redux 入门教程 #10 bindActionCreators「Pro」「03:47」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-10-bindactioncreators)
-   [Redux 入门教程 #11 装饰器函数 @connect「Pro」「09:16」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-11-zhuang-shi-qi-han-shu-connect)
-   [Redux 入门教程 #1 中间件「08:54」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-12-zhong-jian-jian)
-   [Redux 入门教程 #1redux-logger「Pro」「02:09」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-13-redux)
-   [Redux 入门教程 #1 异步和 redux-thunk「Pro」「08:04」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-14-yi-bu-redux-thunk)
-   [Redux 入门教程 #1redux-thunk 实践发送 ajax 请求 part 1「08:38」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-15-redux-thunk-shi-jian-fa-song-ajax-qing-qiu-part)
-   [Redux 入门教程 #1redux-thunk 实践发送 ajax 请求 part 2「Pro」「07:22」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-16-redux-thunk-shi-jian-fa-song-ajax-qing-qiu-part)
-   [Redux 入门教程 #1 异步与 promise「Pro」「09:51」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-17-yi-bu-yu-promise)
-   [Redux 入门教程 #1 调试工具 Redux DevTools「Pro」「03:11」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-18-tiao-shi-gong-ju-redux-devtools)
-   [Redux 入门教程 #1configureStore「Pro」「07:31」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-19-configurestore)
-   [Redux 入门教程 #20 配置热模块加载 hmr（完结）「Pro」「03:52」](https://www.qiuzhi99.com/movies/redux-ru-men-jiao-cheng-20-pei-zhi-re-mo-kuai-jia-zai-hmr-wan-jie)

git 地址：(https://github.com/reduxjs/redux)，灵感来源于 flux
(state, action) => state redux 灵感来自 flux，和 flux 很想但是因为依赖纯函数，所以不需要事件触发 redux 建议你永远不要突然改变你的数据；

用于 react（https://react-redux.js.org/）

<!-- ## [Usage with React](https://redux.js.org/basics/usage-with-react)

## [Redux 官网](https://github.com/reduxjs/redux) -->

## 解析 react redux

> redux 原理

-   原理说明
-   createStore 方法创建 store
-   combineReducers 方法合并多个 reducer
-   pplyMiddleware 中间件创建 store
-   bindActionCreator 创建可直接 dispatch 的 action

`核心概念`每一个改变都是一个 action

```javascript
var object = {
    todos: [
        {
            text: "Eat food",
            completed: true
        },
        {
            text: "Exercise",
            completed: false
        }
    ],
    visibilityFilter: "SHOW_COMPLETED"
};
```

分发的 action 其实是返回的一系列对象

```javascript
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }
```

合并后的 reducer

```javascript
function visibilityFilter(state = 'SHOW_ALL', action) {
  if (action.type === 'SET_VISIBILITY_FILTER') {
    return action.filter
  } else {
    return state
  }
}
​
function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([{ text: action.text, completed: false }])
    case 'TOGGLE_TODO':
      return state.map(
        (todo, index) =>
          action.index === index
            ? { text: todo.text, completed: !todo.completed }
            : todo
      )
    default:
      return state
  }
}

function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  }
}

```

`三个核心概念`

单一来源 ，一个程序只有一个 store

```javascript
console.log(store.getState())
​
/* Prints
{
  visibilityFilter: 'SHOW_ALL',
  todos: [
    {
      text: 'Consider using Redux',
      completed: true,
    },
    {
      text: 'Keep all state in a single tree',
      completed: false
    }
  ]
}
*/
```

状态是只读的

唯一改变状态的方法是触发 action，这样可以是状态的改变容易被监控

```javascript
store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1
})
​
store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
})
```

改变由纯函数完成

Reducers 是一个纯函数，传入一个先前的 state 和 action，然后返回一个新的 state

## [connected-react-router](https://github.com/supasate/connected-react-router)
