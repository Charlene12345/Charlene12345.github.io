# 二、Layout 处理

## 创建首页组件并配置路由

1、创建 `src/views/home/index.vue`

```html
<template>
  <div class="home-container">首页</div>
</template>

<script>
export default {
  name: 'HomeIndex',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less"></style>

```

2、然后在路由表中

<img src="assets/image-20200421172340709.png" alt="image-20200421172340709" style="zoom:50%;" />

3、登录成功，跳转到首页测试



## 创建 Layout 组件并配置路由

1、创建 `src/views/layout/index.vue`

```html
<template>
  <div class="layout-container">
    <div>顶部导航栏</div>

    <div>侧边导航栏</div>

    <!-- 子路由出口 -->
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'LayoutIndex',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less"></style>
```

2、配置 layout 路由

<img src="assets/image-20200421172555035.png" alt="image-20200421172555035" style="zoom:50%;" />

3、最后测试

## 使用 Container 布局容器 搭建页面结构

> 参考文档：[Container 布局容器](https://element.eleme.cn/#/zh-CN/component/container)

```html
<template>
  <el-container class="layout-container">
    <el-aside
      class="aside"
      width="200px"
    >Aside</el-aside>
    <el-container>
      <el-header class="header">Header</el-header>
      <el-main class="main">
        <!-- 子路由出口 -->
        <router-view />
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  name: 'LayoutIndex',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less">
.layout-container {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
}

.aside {
  background-color: #d3dce6;
}

.header {
  background-color: #b3c0d1;
}

.main {
  background-color: #e9eef3;
}
</style>
```



## 处理侧边栏导航菜单

1、创建 `src/views/layout/components/aside.vue`

```html
<template>
  <!--
    el-menu-item 的 index 不能重复，确保唯一即可
   -->
  <el-menu
    class="nav-menu"
    default-active="/"
    background-color="#002033"
    text-color="#fff"
    active-text-color="#ffd04b"
    router
  >
    <el-menu-item index="/">
      <i class="el-icon-s-home"></i>
      <span slot="title">首页</span>
    </el-menu-item>
    <el-menu-item index="/article">
      <i class="el-icon-document"></i>
      <span slot="title">内容管理</span>
    </el-menu-item>
    <el-menu-item index="/image">
      <i class="iconfont iconimage"></i>
      <span slot="title">素材管理</span>
    </el-menu-item>
    <el-menu-item index="/publish">
      <i class="iconfont iconpublish"></i>
      <span slot="title">发布文章</span>
    </el-menu-item>
    <el-menu-item index="/comment">
      <i class="iconfont iconcomment"></i>
      <span slot="title">评论管理</span>
    </el-menu-item>
    <el-menu-item index="/fans">
      <i class="el-icon-setting"></i>
      <span slot="title">粉丝管理</span>
    </el-menu-item>
    <el-menu-item index="/settings">
      <i class="el-icon-setting"></i>
      <span slot="title">个人设置</span>
    </el-menu-item>
  </el-menu>
</template>

<script>
export default {
  name: 'AppAside',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less">
.nav-menu {
  .iconfont {
    margin-right: 10px;
    padding-left: 5px;
  }
}
</style>

```

2、在 layout 中加载使用侧边栏导航菜单组件

<img src="assets/image-20200421173310913.png" alt="image-20200421173310913" style="zoom:50%;" />



## 处理页面顶栏

1、创建 `src/views/layout/components/header.vue` 组件

```html
<template>
  <div class="header-container">
    <div>
      <i class="el-icon-s-fold"></i>
      <span>江苏传智播客科技教育有限公司</span>
    </div>
    <el-dropdown>
      <div class="avatar-wrap">
        <img class="avatar" src="http://toutiao.meiduo.site/FrvifflobfNNRM9V_ZBTI2ZaTH4n" alt="">
        <span>用户昵称</span>
        <i class="el-icon-arrow-down el-icon--right"></i>
      </div>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item>设置</el-dropdown-item>
        <el-dropdown-item>退出</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </div>
</template>

<script>
export default {
  name: 'AppHeader',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less">
.header-container {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #ccc;
  .avatar-wrap {
    display: flex;
    align-items: center;
    .avatar {
      width: 30px;
      height: 30px;
      border-radius: 50%;
      margin-right: 10px;
    }
  }
}
</style>

```

2、然后在 layout 中加载使用

<img src="assets/image-20200421174653839.png" alt="image-20200421174653839" style="zoom:50%;" />

## 在顶栏中展示当前登录用户

1、在 `api/user.js` 中封装请求方法

```js
// 获取用户信息
export const getUserProfile = () => {
  return request({
    method: 'GET',
    url: '/mp/v1_0/user/profile',
    // 后端要求把需要授权的用户身份放到请求头中
    // axios 可以通过 headers 选项设置请求头
    headers: {
      // 属性名和值都得看接口的要求
      // 属性名：Authorization，接口要求的
      // 属性值：Bearer空格token数据
      Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2MTg5MDkxMjYsInVzZXJfaWQiOjEsInJlZnJlc2giOmZhbHNlLCJ2ZXJpZmllZCI6dHJ1ZX0.EdKErKDqMc3snkYxqt02jSa8t9G44002yWKY3CMOMJg'
    }
  })
}
```

2、在 header 组件中请求获取数据

<img src="assets/image-20200421174841178.png" alt="image-20200421174841178" style="zoom:50%;" />

3、把请求到的数据绑定到模板中

<img src="assets/image-20200421175004383.png" alt="image-20200421175004383" style="zoom:50%;" />

## Token 处理

1、在登录成功以后将用户信息存储到本地存储

<img src="assets/image-20200422120805576.png" alt="image-20200422120805576" style="zoom:50%;" />

> 注意：代码是不折行的
>
> 本地存储只能存字符串，如果需要存储数组或者对象数据，则转为 JSON 格式字符串

2、然后在请求的时候获取本地存储中的 user 数据使用 token

<img src="assets/image-20200422120925785.png" alt="image-20200422120925785" style="zoom:50%;" />

## 使用拦截器统一设置用户 Token

> axios 拦截器官方示例：https://github.com/axios/axios#interceptors

在 request 请求模块中添加如下代码：

```js
// 请求拦截器
request.interceptors.request.use(
  // 任何所有请求会经过这里
  // config 是当前请求相关的配置信息对象
  // config 是可以修改的
  function (config) {
    const user = JSON.parse(window.localStorage.getItem('user'))

    // 如果有登录用户信息，则统一设置 token
    if (user) {
      config.headers.Authorization = `Bearer ${user.token}`
    }

    // 然后我们就可以在允许请求出去之前定制统一业务功能处理
    // 例如：统一的设置 token

    // 当这里 return config 之后请求在会真正的发出去
    return config
  },
  // 请求失败，会经过这里
  function (error) {
    return Promise.reject(error)
  }
)
```

## 处理侧边菜单的展开/收起状态

1、在 layout 组件中声明数据用来控制侧边导航菜单的展开收起状态

<img src="assets/image-20200422121315411.png" alt="image-20200422121315411" style="zoom:50%;" />

2、在 layout 组件中处理图标的点击状态

<img src="assets/image-20200422121352145.png" alt="image-20200422121352145" style="zoom:50%;" />

3、将 layout 组件中的 `isCollapse` 传递给侧边栏组件

<img src="assets/image-20200422121446475.png" alt="image-20200422121446475" style="zoom:50%;" />

> 别忘了把 el-aside 的 width 设置为 auto

4、在 aside 组件中声明接收 props 数据并绑定到导航菜单组件中

<img src="assets/image-20200422121540402.png" alt="image-20200422121540402" style="zoom:50%;" />

## 控制页面访问权限

在我们的项目中，除了登录页面，其它所有页面都需要具有登录状态才能访问。也就是说我们要给这些需要登录才能访问的页面进行统一控制。

通常的做法就是利用[路由的导航守卫]([https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)来统一处理。

所谓的路由拦截器就是一个公共的页面访问门卫，说白了就是所有的页面访问都要经过这里，我们可以在这里执行一共公共的操作，例如校验是否具有登录状态。

> 提示：官方文档叫导航守卫，都是一个意思。



具体做法就是在 `src/router/index.js` 中：

```js
// 路由导航守卫：说白了所有页面的导航都会经过这里
// 守卫页面的导航的
// to：要去的路由信息
// from：来自哪里的路由信息
// next：放行方法
router.beforeEach((to, from, next) => {
  // 如果要访问的页面不是 /login，校验登录状态
  // 如果没有登录，则跳转到登录页面
  // 如果登录了，则允许通过
  // 允许通过
  // next()

  const user = JSON.parse(window.localStorage.getItem('user'))

  // 校验非登录页面的登录状态
  if (to.path !== '/login') {
    if (user) {
      // 已登录，允许通过
      next()
    } else {
      // 没有登录，跳转到登录页面
      next('/login')
    }
  } else {
    // 登录页面，正常允许通过
    next()
  }
})
```

> 关于路由导航守卫更详细的用户请参考官方文档：[https://router.vuejs.org/zh/guide/advanced/navigation-guards.html

## 结合导航守卫实现页面切换顶部进度条

- [nprogress](https://github.com/rstacruz/nprogress)
- 路由前置钩子
- 路由后置钩子

1、安装 nprogress

```bash
# yarn add nprogress
npm i nprogress
```

> 注意：项目中不要乱用包管理工具，要从一而终，不要一会儿这个，一会儿那个的。否则的话会导致一些包被莫名删除。
>
> 提示：如果想要从一个包管理工具切换到另一个包管理工具：
>
> 1、手动删除 node_modules
>
> 2、执行 `npm install` 或者 `yarn install` 或者 `cnpm install` 把所有依赖项重新安装一遍
>
> 3、之后固定使用 npm、yarn、cnpm 来装包
>
> 注意：cnpm 就不建议使用了。

2、在 `main.js` 中引入 `nprogress.css` 样式文件

```js
// 加载 nprogress 中的指定的样式文件
// 注意：加载第三方包中的具体文件不需要写具体路径，直接写包名即可
// 总结就是："包名/具体文件路径"
import "nprogress/nprogress.css";
```

3、在路由的全局前置守卫中，开启进度条

```js
...
+ import NProgress from 'nprogress'


router.beforeEach((to, from, next) => {
  // 开启顶部导航进度条特效
+  NProgress.start()

  // 停止导航
  // 我们可以在一些特殊情况下，停留在当前页面，中断当前导航
  // next(false)

  // next()

  // 1. 如果访问的是登录页面，则直接放行
  if (to.path === '/login') {
    next()

    // 停止代码往后执行
    return
  }

  // 2. 非登录页面，校验登录状态

  // 2.1 获取用户 token
  const token = window.localStorage.getItem('user-token')

  // 2.2 判断是否有 token，有就通过
  if (token) {
    // 导航通过，放行，访问哪里就往哪里走
    next()
  } else {
    // 2.3 没有，就跳转到登录页
    next('/login') // 跳转到指定路由
  }
})
```

4、在路由的全局后置钩子中，关闭进度条特效

```js
router.afterEach((to, from) => {
  // 结束顶部的导航进度条
  NProgress.done();
});
```

最后，回到浏览器中测试访问。

## 用户退出

1、给退出按钮注册点击事件

<img src="assets/image-20200422172829358.png" alt="image-20200422172829358" style="zoom:50%;" />

> 注意：并不是所有的组件在注册事件的时候需要使用 `.native` 修饰符，例如 el-button 组件注册点击事件就不需要，这主要是因为该组件内部处理了。
>
> 什么时候使用 `.native`？首先肯定是在组件上注册事件可能会用到，如果普通方式注册不上，这个时候加 `.native` 修饰符。
>
> 例如你给一个组件注册一个 `input` 事件，如果直接 `@input` 注册无效，那就试一下 `@input.native`。

2、处理函数如下

```js
onLogout () {
  this.$confirm('确认退出吗？', '退出提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning'
  }).then(() => {
    // 把用户的登录状态清除
    window.localStorage.removeItem('user')

    // 跳转到登录页面
    this.$router.push('/login')
  }).catch(() => {
    this.$message({
      type: 'info',
      message: '已取消退出'
    })
  })
}
```

最后，回到浏览器测试。
