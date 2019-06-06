# 提高 vue.js 中大型数据的性能

原文 TIPs：https://vuedose.tips/tips 英文版一共 22 篇

原文：https://vuedose.tips/tips/improve-performance-on-large-lists-in-vue-js/

翻译文：

VueDose 的所有的文章都非常的简洁，我相信人们在这种格式下更容易找到有用的东西。所以，让我们直奔主题。

通常我们需要获取对象数据，比如用户，项目，文章，等等等等·····

有时，我们甚至不需要修改它们，只是为了展示它们或在（a.k.a. Vuex）中存贮它们的全局状态。那么获取这个数据的简单代码如下：

```javascript
export default {
    data: () => ({
        users: {}
    }),
    async created() {
        const users = await axios.get("/api/users");
        this.users = users;
    }
};
```

Vue 会自动循环数组的每个对象，并对每个一级属性进行响应。

对于大型数组对象来说这是一个昂贵的做法。是的，有时候你可以把这些数据分页，但是，其他人就能从前端拿到你整个数据。
谷歌地图标记通常就是这种情况，事实上它们是一个巨大的对象。

所以，在这些情况下，如果能够阻止 Vue 对这个数据的反应机制，我们可以获得一些性能上的提升。我们可以在添加到组件之前使用 Object.freeze 来处理数据实现这一点：

```javascript
export default {
    data: () => ({
        users: {}
    }),
    async created() {
        const users = await axios.get("/api/users");
        this.users = Object.freeze(users);
    }
};
```

这个也同样适用于 Vuex：

```javascript
const mutations = {
    setUsers(state, users) {
        state.users = Object.freeze(users);
    }
};
```

顺便说一下，如果你想要修改数组，你可以创建一个新数组来实现。列如，在原有上添加数据项，你可以这样做：

```javascript
state.users = Object.freeze([...state.users, user]);
```

你想知道性能提升多少？我们会在下一篇文章看到它，所以，请继续关注。

今天就到这里了！希望你会喜欢这第一篇文章（鬼脸）。
