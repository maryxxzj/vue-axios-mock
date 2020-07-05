# vue-axios-mock 简单项目

 ## 介绍
  vue-cli 本地开发使用axios+mock数据
  
 **技术栈**：
 * Vue
 * Vue-router 
 * webpack
 * axios
 * mock 
 
 **关键步骤**：
 1. nodeJS安装
 2.  vue-cli 安装模板及初始化项目
 3. 安装淘宝镜像和依赖并启动项目
 4.  配置mock.js
 5.  引用axios

## 配置mock.js
 mockjs安装  `cnpm install mockjs --save-dev`
 
为了只在开发环境使用mock.js，打包到生产环境时自动不使用mock.js，做以下配置：

01 **config目录下dev.env.js**
```
'use strict'
const merge = require('webpack-merge')
const prodEnv = require('./prod.env')

module.exports = merge(prodEnv, {
  NODE_ENV: '"development"',
  Mock: true
})
```
02 **config目录下prod.env.js**
```
'use strict'

module.exports = {
  NODE_ENV: '"production"',
  Mock: false
}
```
03 src目录下main.js
```
process.env.Mock && require('./mock/mock.js')
```
04 项目根目录下创建mock文件夹，新建mock.js的文件 

05 在mock.js文件中编写响应数据
~~~
// 引入mockjs
const Mock = require('mockjs')
// 获取 mock.Random 对象
const Random = Mock.Random
// mock一组数据
const produceNewsData = function () {
  let articles = []
  for (let i = 0; i < 50; i++) {
    let newArticleObject = { 
      author_name: Random.cname(), // Random.cname() 随机生成一个中文姓名
    }
    articles.push(newArticleObject)
  }
  return articles 
}
// 拦截ajax请求，配置mock的数据
Mock.mock('/api/test', 'get', produceNewsData)
~~~
## 引用axios
 axios安装  `cnpm install axios --save-dev`
 
安装其他插件时，可以直接在 `main.js `中引入并 Vue.use()，但 **axios 不能 use，只能每个需要发送请求的组件中即时引入**
为了解决这个问题，有两种开发思路：① 在引入 axios 之后，修改原型链。 ②结合 Vuex，封装一个 aciton。 
###  改写原型链
01 在 main.js 中引入 axios  `import axios from 'axios'`

这时如果在其它的组件中，是无法使用 axios 命令的。所以将 axios 改写为 Vue 的原型属性

02 将 axios 改写为 Vue 的原型属性  ` Vue.prototype.$http= axios`

**在 main.js 中添加了上面两行代码之后，就能直接在组件的 methods 中使用 $http命令**。

03 在HelloWorld.vue的methods 中使用 $http命令，使用mock数据
```
mounted: function () {
    this.getTest();
},
methods: {
    getTest() {
        var _this = this;
        _this.$http.get('/api/test').then(function (res) {
            console.log(res.data); 
        });
    }
} 
``` 

## Build Setup

``` bash
# install registry 安装淘宝镜像
npm install -g cnpm --registry=https://registry.npm.taobao.org

# install dependencies 安装依赖
cnpm install

# serve with hot reload at localhost:8080
npm run dev 运行项目

# build for production with minification
npm run build 编译

# build for production and view the bundle analyzer report
npm run build --report  查看编译报告
```
