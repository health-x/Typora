# 一、JS回顾

JS中的数据类型包括如下：

- [`Number`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)（数字）

- [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)（字符串）

- [`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Boolean)（布尔）

- [`Symbol`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)（符号）（ES2015 新增）

- `Object`

  （对象）

  - [`Function`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function)（函数）
  - [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)（数组）
  - [`Date`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date)（日期）
  - [`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)（正则表达式）

- [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/null)（空）

- [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)（未定义）

### 1.1、Number（数字）

```shell
Math

# 第一个参数为字符串或数字，第二个参数
parseInt(param1,param2);

# 用以解析浮点数字符串,只应用于解析出十进制数字
parseInt(param1,param2);

# 判断一个变量是否为 NaN
isNaN()

# 判断一个变量是否是一个有穷数
isFinite(1/0); // false
isFinite(Infinity); // false
isFinite(-Infinity); // false
isFinite(NaN); // false

// 如果是纯数值类型的检测，则返回 false：
Number.isFinite("0"); // false
```



### 1.2、String（字符串）

```shell
# 
"abc".length()
```





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

#### 子传父（方式三）

```vue
父组件
<template>
  <div>
      //绑定自定义事件receiveThing
      <Student ref="health"></Student>
  </div>
</template>
<script>
//引入组件
import Student from './components/Student.vue'
    export default {
        name:'App',
        components:{Student},
        methods:{
            //编写自定义事件内容
            receiveStudent(name){
                console.log(name);
            }
        },
        //挂载完毕函数
        mounted(){
            //student的组件实例对象('当health被触发时'，执行的回调函数)
            this.$refs.student.$on('health',this.receiveStudent)
        }
    }
</script>

子组件
<template>
  <div>
    <h2>学生姓名：{{ name }}</h2>
    <button @click="sendStudent">发送学生信息给父组件</button>
  </div>
</template>

<script>
export default {
  name: "Student",
  data() {return{name: "张三"}},
  methods:{
    sendStudent(){
      //触发Student组件实例上的receiveThing组件事件
      this.$emit('receiveThing',this.name)
    }
  }
};
</script>
```





## 2.5 props属性

#### **父传子（常用）**

```vue
<!--父组件-->
<MyItem v-for="todo in todos" :key="todo.id" :todo="todo"/>


<!--子组件-->
<template>
  <div>
    <input type="checkbox" :checked="todo.done"/>
    <span>{{todo.title}}</span>
  </div>
</template>
<script>
  export default {
    name: "MyItem",
    props:['todo'],
  };
</script>
```

#### **子传父（方式一）**

```vue
<!--父组件-->
<MyHeader :addTodoObj="addTodoObj"></MyHeader>	//传给子组件一个函数
<script>
export default {
  name: "App",
  methods:{
    //该函数接收一个参数
    addTodoObj(todoObj){
      console.log(todoObj)
    },
  },
};
</script>


<!--子组件-->
<script>
export default {
  name: "MyHeader",
  props:["addTodoObj"],	//接收父组件传的函数
  methods:{
    addTodo(){
      const todoObj = 111;
      //调用父组件传的函数，把值传递给父组件
      this.addTodoObj(todoObj)
    }
  }
};
</script>


```





## 2.6 $emit

#### 子传父（方式二）

```vue
父组件
<template>
  <div>
      //绑定自定义事件receiveThing
      <Student v-on:receiveThing="receiveStudent"></Student>
  </div>
</template>
<script>
//引入组件
import Student from './components/Student.vue'
    export default {
        name:'App',
        components:{Student},
        methods:{
            //编写自定义事件内容
            receiveStudent(name){
                console.log(name);
            }
        }
    }
</script>

子组件
<template>
  <div>
    <h2>学生姓名：{{ name }}</h2>
    <button @click="sendStudent">发送学生信息给父组件</button>
  </div>
</template>

<script>
export default {
  name: "Student",
  data() {return{name: "张三"}},
  methods:{
    sendStudent(){
      //触发Student组件实例上的receiveThing组件事件
      this.$emit('receiveThing',this.name)
    }
  }
};
</script>
```



## 2.7 插槽

#### 2.7.1 默认插槽

```vue
父组件
<子组件名><h1>1111111</h1></子组件名>

子组件
<div>
    <slot></slot>	//此位置会显示 <h1>1111111</h1>的内容
</div>
```



#### 2.7.2 具名插槽

```vue
父组件
<子组件名>
    <h1 slot="s1">1111111</h1>
    <h1 slot="s2">2222222</h1>
    <h1 slot="s2">3333333</h1>
    如果用的是 template 包裹的内容 可以用<template v-slot:s1></template>形式
</子组件名>

子组件
<div>
    <slot name="s1"></slot>	//此位置会显示 <h1>1111111</h1>的内容
    <slot name="s2"></slot>	//此位置会显示 <h1>2222222</h1> 和 <h1 slot="s2">3333333</h1>的内容
</div>
```





# 三、脚手架创建项目

官方文档：https://cli.vuejs.org/zh/guide/

## 3.1 使用步骤

默认vscode、nodejs已经安装好了

```shell
# 1.全局安装@vue/cli 可以帮助我们快速构建Vue项目
npm install -g @vue/cli

# 2.切换到要创建项目的目录，使用命令创建项目
vue create 项目名	//选择vue3

# 3.启动项目（切记要切换到项目目录下再执行命令）
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















 



















# 小工具

npm i nanoid	uuid简化版



## axios使用步骤

### 一、普通html项目

引入axios库

```html
<!-- axios库 -->
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

发送请求

```js
//post请求（带参数）
axios.post("http://47.100.81.153:10001/user/add", {
    username: that.registerForm.username,
    password: that.registerForm.password,
    sex: that.registerForm.sex,
    birth: that.registerForm.birth,
    phone: that.registerForm.phone,
}).then(function (response) {
    console.log(response);
}, function (err) {
    that.$message.error('操作失败！');
})

//get请求（不带参数）
axios.get("http://47.100.81.153:10001/user/add")
    .then(function (response) {
    	//请求成功
    	console.log(response);
	}, function (err) {
    	that.$message.error('操作失败！');
})
```



### 二、脚手架项目

```vue
<script>
    import axios from 'axios'	//引入axios
    export default {
        name:'App',
        methods: {
            getStudent(){
                //执行axios请求
                axios.get('url').then(function (response) {
    				//请求成功
    				console.log(response.data);
				}, function (err) {
    				that.$message.error('操作失败！');
            	}
        }
    }
    
</script>
```



### 三、解决跨域

1. cros （后端做）
2. jsonp（前后端一起，只能解决get请求）
3. 代理服务器（ nginx 或 vue-cli ）

**代理服务器跨域（vue-cli）**

​	在根目录创建 vue.config.js 文件

```js

module.exports = {
    
    //方式一：开启代理服务器（只能配置一个代理，不能灵活控制到底走不走代理）
    //需要把原来的请求路径从http://localhost:10001改为http://localhost:8080
	devServer: {
        //把前端传来的8080请求转发给10001
    	proxy: 'http://localhost:4000'
  	}
    
    
    //方式二：开启代理服务器
    //需要把原来的请求路径从http://localhost:10001改为http://localhost:8080/health
	devServer: {
    	proxy: {
    	//请求前缀(发请求时端口号后面有它才会来找代理服务器转发请求)
    	'/health': {
    		//请求地址
    		target: 'http://localhost:10001',
    		//匹配所有以/health开头的路径，并把它改为/health改为''
    		pathRewrite:{'^/health':''}
    		//用于支持websocket
    		ws: true,
             //用于控制请求头中的host值，默认为true。请求回复后端时false：我来自8080，true：我来自跟你一样10001
    		changeOrigin: true
		},
         //配置多个代理
    	'/health2': {
            target: 'http://localhost:10002'
      }
    }
  }
}
```







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









