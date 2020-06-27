# Vue的学习
####	一、方法

> methods:{
>
> ​	name:function(){},//es5写法,
>
>  	name(){}//es6写法
>
> }

####	二、数组

####	三、组件

#####	props传值

+ 在props中使用驼峰的形式，模板中需要使用短横线的形式
+ 字符串形式的模板中没有这个限制

#####	注册

```
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  props:['menuTitle'],//<button-counter menu-title="hello"></button-counter>
  template: '<button v-on:click="count++">You clicked me {{ count }} {{menuTitle}} times.</button>'
})
在html中直接使用 <button-counter></button-counter>

或者直接在components中注册（局部组件）
components: {
        buttontemplate: {
            template: '<div>123123123</div>'
        }
}
```

组件也必须包含一个根元素

#####	组件间的数据交互

1、事件通信

```
var event=new Vue()
```

2、监听事件和销毁

```
event.$on('add-todo',addtodo) //在mounted中监听事件
event.$off('add-todo')
```

3、触发事件

```
event.$emit('add-todo',id)
```

#####	插槽

template可以包裹。

定义slot名字，可以在引用的时候，规定。

作用域槽

> 

#### 四、Webpack打包 Vue项目

> vue init webpack 项目名
>
> 前提需要安装vue/cli，webpack



####	五、Vue-cli 运行

+ 开发模式运行 npm run dev
+ 部署模式运行 npm run build



##	NodeJs

####	expresss

#####	部署项目

> express (项目名)

#####	Post和Get

> Post 获取参数 应该在express模块添加以下代码
>
> app.use(bodyParser.json());
>
> app.use(bodyParser.urlencoded({ extended: true }));