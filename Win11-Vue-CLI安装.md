### Vue CLI 安装

> 查看版本

```powershell
#查看Node.js版本，下面两句执行效果一样
node -v
node --version

#查看npm、cnpm版本
npm -v
cnpm -v
```

> 卸载旧版本
> Vue CLI 的包名称由 `vue-cli` 改成了 `@vue/cli`

```powershell
# 如果安装了旧版本的 vue-cli (1.x 或 2.x)，先卸载
# -g 表示全局安装
npm uninstall vue-cli -g
# OR
yarn global remove vue-cli
```

> 安装新版本

```powershell
npm install -g @vue/cli
# OR
yarn global add @vue/cli

#安装完，vue命令就可以用了
vue --version

# 也可以从以下路径查看有没有新增文件
# C:\Users\chill\AppData\Roaming\npm
```

> 升级

```powershell
npm update -g @vue/cli
# 或者
yarn global upgrade --latest @vue/cli
```

### 创建一个项目

```powershell
# 以图形化界面创建和管理项目
vue ui

# 以命令行创建一个新项目
vue create hello-world
```

> Vue CLI >= 3 和旧版使用了相同的 `vue` 命令，所以 Vue CLI 2 (`vue-cli`) 被覆盖了。如果仍然需要使用旧版本的 `vue init` 功能，可以全局安装一个桥接工具

```powershell
npm install -g @vue/cli-init
# `vue init` 的运行效果将会跟 `vue-cli@2.x` 相同
vue init webpack my-project
```

### CLI 服务

> 模块热重载

```powershell
npm run serve
# OR
yarn serve

# 如果你可以使用 npx (最新版的 npm 应该已经自带)，也可以直接这样调用命令：
npx vue-cli-service serve

#用法：vue-cli-service serve [options] [entry]
#选项：
#  --open    在服务器启动时打开浏览器
#  --copy    在服务器启动时将 URL 复制到剪切版
#  --mode    指定环境模式 (默认值：development)
#  --host    指定 host (默认值：0.0.0.0)
#  --port    指定 port (默认值：8080)
#  --https   使用 https (默认值：false)
```

> 构建

```powershell
npx vue-cli-service build

#用法：vue-cli-service build [options] [entry|pattern]
#选项：
#  --mode        指定环境模式 (默认值：production)
#  --dest        指定输出目录 (默认值：dist)
#  --modern      面向现代浏览器带自动回退地构建应用
#  --target      app | lib | wc | wc-async (默认值：app)
#  --name        库或 Web Components 模式下的名字 (默认值：package.json 中的 "name" 字段或入口文件名)
#  --no-clean    在构建项目之前不清除目标目录的内容
#  --report      生成 report.html 以帮助分析包内容
#  --report-json 生成 report.json 以帮助分析包内容
#  --watch       监听文件变化
```

> 审查

```powershell
npx vue-cli-service inspect

#用法：vue-cli-service inspect [options] [...paths]
#选项：
#  --mode    指定环境模式 (默认值：development)
```

> 帮助

```powershell
npx vue-cli-service help
```

### 其他

```
#下载当前项目的所有所需依赖包
npm install
```

### 脚手架-HelloWorld概述

> 脚手架文件结构

```
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── component: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   │── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
├── package-lock.json：包版本控制文件
```

> vue.config.js配置文件

```
1. 使用vue inspect > output.js可以查看到Vue脚手架的默认配置。
2. 使用vue.config.js可以对脚手架进行个性化定制，详情见：https://cli.vuejs.org/zh
```

> src\main.js

```javascript
/* 
	该文件是整个项目的入口文件
*/
//引入Vue
import Vue from 'vue'
//引入App组件，它是所有组件的父组件
import App from './App.vue'
//关闭vue的生产提示
Vue.config.productionTip = false

/* 
	关于不同版本的Vue：
	
		1.vue.js与vue.runtime.xxx.js的区别：
				(1).vue.js是完整版的Vue，包含：核心功能+模板解析器。
				(2).vue.runtime.xxx.js是运行版的Vue，只包含：核心功能；没有模板解析器。

		2.因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用
			render函数接收到的createElement函数去指定具体内容。
*/

//创建Vue实例对象---vm
new Vue({
	el:'#app',
	//render函数完成了这个功能：将App组件放入容器中
  render: h => h(App),
	// render:q=> q('h1','你好啊')

	// template:`<h1>你好啊</h1>`,
	// components:{App},
})
```

> public\index.html

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    
		<!-- 针对IE浏览器的一个特殊配置，含义是让IE浏览器以最高的渲染级别渲染页面 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
		
    <!-- 开启移动端的理想视口 -->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
		
    <!-- 配置页签图标 -->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
		
    <!-- 引入第三方样式 -->
		<link rel="stylesheet" href="<%= BASE_URL %>css/bootstrap.css">
		
    <!-- 配置网页标题 -->
    <title>硅谷系统</title>
  </head>
  <body>
		
    <!-- 当浏览器不支持js时noscript中的元素就会被渲染 -->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
	
    <!-- 容器 -->
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>

```

> src\App.vue

```vue
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
</template>

<script>
//引入组件
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

> \src\components\HelloWorld.vue

```vue
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li><a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a></li>
      <li><a href="https://forum.vuejs.org" target="_blank" rel="noopener">Forum</a></li>
      <li><a href="https://chat.vuejs.org" target="_blank" rel="noopener">Community Chat</a></li>
      <li><a href="https://twitter.com/vuejs" target="_blank" rel="noopener">Twitter</a></li>
      <li><a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a></li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
      <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
      <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
      <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
      <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

```

