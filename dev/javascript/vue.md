Vue
===

## 指令
创建应用实例
```js
app = Vue.createApp({
  data() {
    return {
      message: 'Hello Vue!'
    }
  }
}).mount('#app')
```
App - 根组件

```html
v-bind: id="value"
v-on/ @click="handler"
v-model + v-bind

<!-- modifiers -->
v-on:submit.prevent="handler"
```

```js
export default {
  data() {
    return {}
  },
  methods: {
    func(){},
  },
  computed:{
    someAttr(){} // cached, readonly
  }
  watch
}
```

## 数据访问
this.$refs, $data

props

## 渲染组件
```html
<component :is="currentTabComponent"></component>
```

## 依赖注入
解决 props 沿途过长

# 渲染顺序问题（刷新）
`utils/request`: export default service AJAX. 若 log out, 触发 store resetToken 清空 token, 但是 service 里面的 token 仍然存在, 会导致刷新页面后, 依然会发送请求, 但是请求的 token 为空, 

`asyncRoute` 先被过滤一遍，发生在后端之后