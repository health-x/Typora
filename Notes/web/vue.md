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
v-on:click="方法"/@click="方法名"		//为元素绑定事件（单击事件，双击事件等等）。
v-show="表达式"						//根据表达式真假，判断元素显示和隐藏(style设置为none)
v-if="表达式"						//根据表达式真假，判断元素显示和隐藏(从dom树中移除)
v-bind:属性名="xx"/:属性名="xxx"			//为元素绑定属性 如:src="xxx"
v-for="(item,index) in arr"
v-model							//双向数据绑定（表单元素）
```



# 二、工程化

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
