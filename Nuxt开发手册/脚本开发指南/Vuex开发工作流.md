# Vuex开发工作流



Nuxt.js会自动识别`store`目录内的js文件，这些文件将被当作Vuex的模块进行注册。

**除了tokne外，所有的vuex数据不应从缓存中加载，都应该在用户第一次进入网站的时候通过接口请求的方式进行重刷。**

Vuex使用规范请参考：[Vuex使用规范](/开发规范/vue开发规范/Vuex使用规范.md)。



 ## 创建一个Vuex模块

在`store/`下创建一个`user.js`文件：

`store/user.js`

```javascript
export const state = () => ({
    info: null,
})

export const actions = {
    async update({ commit }) {
        try {
            const res = await this.$axios.$post('');
            commit('set', res);
        } catch (e) {
            console.error(e);
        }
    }
}

export const mutations = {
    set(state, info) {
        state.info = info;
    },
    out(state) {
        state.info = null;
    }
}

```



使用方式：

```vue
<template>
    <div>
        {{$store.state.user.info}}
    </div>
</template>
<script>
export default {
    data() {
        return {
            info: null
        };
    },
    async mounted() {
        this.info = this.$store.state.user.info;//获取
        await this.$store.commit('user/update');//调用异步函数
        this.$store.dispatch('user/set', this.info);//提交状态更改
    }
}
</script>
```



