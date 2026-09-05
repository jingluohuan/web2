# 路由VueRouter

# 路由 VueRouter

## 一、基础理解

**理解：**
一个路由（route）就是一组映射关系（key - value），多个路由需要路由器（router）进行管理。

**前端路由：**
key 是路径，value 是组件。

**什么是路由：**
路由是一种映射关系。

**Vue 中的路由是什么：**
路径 和 组件 的映射关系，根据路径匹配对应组件进行渲染。

---

## 二、基本使用

### 版本对应

- 
vue2 → VueRouter3、Vuex3

- 
vue3 → VueRouter4、Vuex4

---

## 三、VueRouter 的使用（5 + 2）

### 5 个基础步骤（固定）

① 下载

```
yarn add vue-router@3.6.5
# 或
npm i vue-router@3.6.5
```

② 引入

```
import VueRouter from 'vue-router'
```

③ 安装注册

```
Vue.use(VueRouter)
```

④ 创建路由对象

```
const router = new VueRouter()
```

⑤ 注入（建立关联）

```
new Vue({
  render: h => h(App),
  router
}).$mount('#app')
```

---

### 2 个核心步骤

① 写组件 + 配置路由规则

- 
views：页面组件（配合路由）

- 
components：复用组件

② 配置导航 + 路由出口

```

```

---

## 四、单文件路由配置

**目的：** 方便维护

### 步骤

- 
创建 `src/router/index.js`

- 
引入组件

- 
配置路由

- 
导出并在入口文件使用

---

## 五、声明式导航（router-link）

### 基本使用

```
跳转
```

- 
本质是 `<a>`

- 
用 `to` 代替 `href`

- 
不需要 `#`

---

### 高亮类名

类名

说明

router-link-active

模糊匹配

router-link-exact-active

精确匹配

---

### 自定义高亮类名

```
const router = new VueRouter({
  routes: [...],
  linkActiveClass: 'active',
  linkExactActiveClass: 'exact-active'
})
```

---

## 六、几个注意点

- 
路由组件 → views/pages

- 
普通组件 → components

- 
路由切换默认会销毁组件

- 
每个组件都有 `$route`

- 
全局只有一个 `$router`

---

## 七、路由传参

### 1️⃣ query 参数（推荐多参数）

#### 传参

```
跳转
```

```
跳转
```

#### 接收

```
$route.query.id
$route.query.title
```

---

### 2️⃣ params 参数（推荐单参数）

#### 配置路由

```
{
  name:'xiangqing',
  path:'detail/:id?/:title?',
  component:Detail
}
```

#### 传参

```
跳转
```

```
跳转
```

⚠️ 注意：
 使用 params + 对象写法 → 必须用 `name`

#### 接收

```
$route.params.id
$route.params.title
```

---

## 八、props 传参

```
{
  path:'path/:id',
  component:Detail,

  // 写法1
  // props:{ a:900 }

  // 写法2
  // props:true

  // 写法3
  props(route){
    return {
      id:route.query.id,
      title:route.query.title
    }
  }
}
```

---

## 九、多级路由

```
{
  path:'/home',
  component:Home,
  children:[
    { path:'news', component:News },
    { path:'message', component:Message }
  ]
}
```

跳转：

```
News
```

---

## 十、路由重定向

```
{ path: '/', redirect: '/my' }
```

---

## 十一、404 页面

```
{ path: '*', component: NotFound }
```

---

## 十二、路由模式

```
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

### 对比

模式

特点

hash

有 #，兼容性好

history

无 #，需后端支持

---

## 十三、命名路由

```
{
  name:'hello',
  path:'welcome',
  component:Hello
}
```

```
跳转
```

---

## 十四、replace 模式

```
跳转
```

---

## 十五、编程式导航

```
this.$router.push('/path')

this.$router.push({
  path:'/path',
  query:{ id:1 }
})

this.$router.replace({
  name:'xxx',
  params:{ id:1 }
})
```

---

## 十六、浏览器历史控制

```
this.$router.forward()
this.$router.back()
this.$router.go(n)
```

---

## 十七、缓存组件 keep-alive

```

  

```

### 属性

- 
include

- 
exclude

- 
max

---

### 生命周期

- 
activated

- 
deactivated

---

## 十八、路由守卫

### 全局前置守卫

```
router.beforeEach((to,from,next)=>{
  if(to.meta.isAuth){
    if(localStorage.getItem('school') === 'atguigu'){
      next()
    }else{
      alert('暂无权限查看')
    }
  }else{
    next()
  }
})
```

---

### 全局后置守卫

```
router.afterEach((to)=>{
  document.title = to.meta.title || 'vue_test'
})
```

---

### 独享守卫

```
beforeEnter(to,from,next){
  next()
}
```

---

### 组件内守卫

```
beforeRouteEnter(to,from,next){}
beforeRouteLeave(to,from,next){}
```

---

## 十九、路由模式补充

### hash

- 
带 #

- 
不会发送给服务器

### history

- 
更美观

- 
需要后端支持

---

## 二十、history 404 解决方案

```
const express = require('express')
const history = require('connect-history-api-fallback')

const app = express()

app.use(history())
app.use(express.static(__dirname + '/static'))

app.listen(5000)

```

部分内容来自：[路由VueRouter - 慕托小记](https://www.tortb.com/posts/vuerouter)
