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

## 4. router-link 使用

**作用**：用来替换我们在切换路由时使用 a 标签切换路由

**好处**：就是可以自动给路由路径加入#不需要手动加入

```js
    <router-link to="/login" tag="button">我要登录</router-link>
    <router-link to="/register" tag="button">点我注册</router-link>
```

> [!tip]
>
> 1. `router-link` 用来替换使用 a 标签实现路由切换 好处是不需要书写`#`号直接书写路由路径
> 2. `router-link` **to 属性**用来书写路由路径 **tag 属性**:用来将`router-link`渲染成指定的标签

## 5. 默认路由

**作用**：用来在第一次进入界面是显示一个默认的组件

```js
//创建路由对象
const router = new VueRouter({
  routes: [
    // {path: "/", component: login},   不太推荐
    { path: "/", redirect: "login" }, //推荐使用
    { path: "/login", component: login },
    { path: "/resgiter", component: register },
  ],
});
```

?>`redirect`:：用来当访问的是默认路由 "/" 时 跳转到指定的路由展示

## 6. 路由中参数传递

### 6.1. 传统方式

#### 6.1.1. 通过?号形式拼接参数

```js
<router-link to="/login?id=21&name=zhangsan">登录</router-link>
```

#### 6.1.2. 组件中获取参数

```js
const login = {
  template: "<h1>用户登录</h1>",
  data() {
    return {};
  },
  methods: {},
  created() {
    console.log(
      "=============>" +
        this.$route.query.id +
        "======>" +
        this.$route.query.name
    );
  },
};
```

> **传统方式**都是放在`query`里面的

### 6.2. restful 方式

#### 6.2.1. 通过使用路径方式传递参数

```js
<router-link to="/register/24/张三">我要注册</router-link>;
var router = new VueRouter({
  routes: [
    { path: "/register/:id/:name", component: register }, //定义路径中获取对应参数
  ],
});
```

#### 6.2.2. 组件中获取参数

```js
const register = {
  template: "<h1>用户注册{{ $route.params.name }}</h1>",
  created() {
    console.log(
      "注册组件中id:   " + this.$route.params.id + this.$route.params.name
    );
  },
};
```

> **restful 方式**都是放在`params`里面的

## 7. 嵌套路由

### 7.1. 声明最外层和内层路由

```js
<template id="product">
  <div>
    <h1>商品管理</h1>

    <router-link to="/product/add">商品添加</router-link>
    <router-link to="/product/edit">商品编辑</router-link>

    <router-view></router-view>
  </div>
</template>;

//声明组件模板
const product = {
  template: "#product",
};

const add = {
  template: "<h4>商品添加</h4>",
};

const edit = {
  template: "<h4>商品编辑</h4>",
};
```

### 7.2. 创建路由对象含有嵌套路由

```js
const router = new VueRouter({
  routes: [
    {
      path: "/product",
      component: product,
      children: [
        { path: "/add", component: add },
        { path: "/edit", component: edit },
      ],
    },
  ],
});
```

### 7.3. 注册路由对象

```js
const app = new Vue({
  el: "#app",
  data: {},
  methods: {},
  components: {},
  router,
});
```

### 7.4. 测试路由

```js
      <router-link to="/product">商品管理</router-link>
      <router-view></router-view>
```
