# Vue 中路由(VueRouter)

## 1. 简介

!>**路由**：根据请求的路径按照一定的路由规则进行请求的转发从而帮助我们实现统一请求的管理

> [vue-router 中文文档](https://router.vuejs.org/zh/)

## 2. 作用

用来在`vue`中实现**组件之间的动态切换**

## 3. 使用路由

### 3.1. 引入路由

```js
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script> //vue 路由js
```

### 3.2. 创建组件对象

```js
//声明组件模板
const login = {
  template: "<h1>登录</h1>",
};

const register = {
  template: "<h1>注册</h1>",
};
```

### 3.3. 定义路由对象的规则

```js
//创建路由对象
const router = new VueRouter({
  routes: [
    { path: "/login", component: login }, //path: 路由的路径  component:路径对应的组件
    { path: "/register", component: register },
  ],
});
```

### 3.4. 将路由对象注册到 vue 实例

```js
const app = new Vue({
  el: "#app",
  data: {
    username: "千仔",
  },
  methods: {},
  router: router, //设置路由对象
});
```

### 3.5. 在页面中显示路由的组件

```js
<!--显示路由的组件-->
<router-view></router-view>
```

### 3.6. 根据连接切换路由

```js
<a href="#/login">点我登录</a>
<a href="#/register">点我注册</a>
```

?>`#/login`在切换路由路径时候，前面需要添加`#`

![使用路由](<media/Vue中路由(VueRouter).assets/使用路由.gif>)

##  router-link使用

**作用**：用来替换我们在切换路由时使用a标签切换路由

**好处**：就是可以自动给路由路径加入#不需要手动加入

```js
    <router-link to="/login" tag="button">我要登录</router-link>
    <router-link to="/register" tag="button">点我注册</router-link>
```

> [!tip]
>
> 1. `router-link` 用来替换使用a标签实现路由切换 好处是不需要书写`#`号直接书写路由路径
> 2. `router-link` **to属性**用来书写路由路径   **tag属性**:用来将`router-link`渲染成指定的标签

## 默认路由

