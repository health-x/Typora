# 一、简单vue入门

## 1. 小示例

参考：https://cn.vuejs.org/v2/guide/

```html
1.引入vue包
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>

2.编写html体
<div id="app">
  {{ message }}
</div>

3.编写js
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})

```

## 2. el挂载点

- el：用来设置vue实例挂载(管理)的元素
- Vue实例的作用范围：el选项命中的元素及内部的后代元素
- 可以使用其它选择器，但建议使用id选择器
- 不能挂载 html 和 body 标签

## 3. 指令

```js
v-text=""		//设置标签的内容，会替换标签的全部内容
{{}}		//插值设置内容，可替换指定内容
v-html=""		//设置标签的内容，会解析html格式并替换标签的全部内容
v-on:click="方法"  @click="方法名"		//为元素绑定事件（单击事件，双击事件等等）。
v-show="表达式"						//根据表达式真假，判断元素显示和隐藏(style设置为none)
v-if="表达式"						//根据表达式真假，判断元素显示和隐藏(从dom树中移除)
v-bind:属性名="xx"  :属性名="xxx"			//为元素绑定属性 如:src="xxx"
v-for="(item,index) in arr"
v-model							//双向数据绑定（表单元素）
```



# 二、基础

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。

## 2.1 安装

2.1.1 使用script引入

2.1.2 使用cdn引入



## 2.2 Vue模板语法：

#### 插值语法：

- 功能：用于解析标签体内容

- 举例：{{xxx}}，xxx是js表达式，且可以直接读取到data中的所有属性。

#### 指令语法：

- 功能：用于解析标签（包括：标签属性、标签体内容、绑定事件......）

- 举例：**v-bind**:href="xxx" 或简写为 :href="xxx"，xxx同样要写js表达式，且可以是data中所有属性。

Tips:

```shell
use v-bind:href="url" to instead of href="url"	# v-bind:	可以动态绑定各种属性


```



## 2.3 数据绑定

v-model只能应用在表单类/输入类元素上，如input，radio，checkbox等，都有value的元素



## 2.4 ref属性

- 被用来给元素或子组件注册引用信息（id的替代者）
- 应用在html标签上获取的是真实的dom元素，应用在组件标签上是组件的实例对象

# 三、脚手架

官方文档：https://cli.vuejs.org/zh/guide/

## 3.1 使用步骤

```shell
# 1.全局安装@vue/cli
npm install -g @vue/cli

# 2.切换到要创建项目的目录，使用命令创建项目
vue create 项目名

# 3.启动项目
npm run serve
```

注意：

1. 如果出现下载缓慢请配置 npm 淘宝镜像：npm config set registry https://registry.npm.taobao.org
2. Vue 脚手架隐藏了所有webpack相关的配置，若想看具体的webpack配置，请执行：vue inspect > output.js



## 3.2 项目结构

### 配置文件

| 文件名            | 功能作用                                     |
| ----------------- | -------------------------------------------- |
| .gitignore        | git的忽略文件，设置不被git管理的文件和文件夹 |
| babel.config.js   | babel的控制文件                              |
| package-lock.json | 包的说明书                                   |
| package.json      | ...（scripts下的 lint 作用语法检查）         |















 



























# 工程化

```shell
npm init -y		# 生成package.json
npm install jquery -S	# 引入jquery依赖	-S：--save表示将依赖放进dependencies中，全局可用

npm install webpack@5.42.1 webpack-cli@4.7.2 -D	# 安装webpack	-D：--save-dev表示将依赖放在开发环境下devDependencies
```

### 1.  webpack在项目中的安装配置

```shell
# 1.安装webpack	-D：--save-dev表示将依赖放在开发环境下devDependencies
npm install webpack@5.42.1 webpack-cli@4.7.2 -D

# 2.在项目根目录，创建webpack.config.js的文件，并初始化如下配置
module.exports = {
    //webpack运行的模式（开发环境development和 生产环境production）
    mode: 'development'
}

# 3.在package.json的scripts节点下，新增dev脚本，内容如下：
"scripts": {
    "dev": "webpack"	//script节点下的脚本，可以通过 npm run 执行，例如：npm run dev（前面dev可自定义命名)
}
```















# ES6模块化

ES6 模块化规范是浏览器端与服务器端通用的模块化开发规范。它的出现极大的降低了前端开发者的模块化学 习成本，开发者不需再额外学习 AMD、CMD 或 CommonJS 等模块化规范。

ES6 模块化规范中定义： 

- 每个 js 文件都是一个独立的模块
- 导入其它模块成员使用 import 关键字
- 向外共享模块成员使用 export 关键字

### ES6 的模块化主要包含 3 种用法： 

**1.默认导出(每个模块只允许一次)与默认导入**

- 共享/导出自己的变量和方法：export  default  { 变量，方法......}
- 导入01.js中向外共享的成员：import 别名 from ./01.js

**2.按需导出与按需导入**

- 按需导出的语法： export 按需导出的成员（按需导入可以和默认导入一起使用）
- 按需导入的语法： import { s1,s2 as ss2,say } from './xxx.js'（导入时可以用as重命名）

**3.直接导入并执行模块中的代码**

- 直接：import './xxx.js'，就会执行后xxx.js中的方法

### 宏任务微任务

执行顺序：同步任务-->微任务-->宏任务









