# Vue.js

Vue是一个开源的前端框架

特点:

1. 大量的语法糖简化开发流程
2. 语法与js区别非常大
3. 更新迭代非常快
4. 遵循MVVM架构模式
5. 支持单页面(应用)开发
6. 支持渐进式开发
7. 支持视图组件式开发
8. 支持虚拟DOM(将DOM节点树创建内存中的副本)

**MVVM**

MVVM: Model View ViewModel三部分

(模型-视图-视图模型)

![image-20220617155751495](# Vue.js.assets/image-20220617155751495.png)

各部分的作用:

1. Model代表数据, 数据来自后端或者用户输入
2. View代表网页, 通过网页展示数据
3. ViewModel: 用于绑定网页和数据(双向绑定)

![image-20220617154952517](# Vue.js.assets/image-20220617154952517.png)

如何使用Vue开发网页:

1. 直接在网页中导入Vue.js
2. 使用Vue-cli脚手架开发

```html
<script>
/* 完成：视图  和   数据之间的绑定 */
let app = new Vue({
el: '#app',
data : {msg:"Hello World!"}
});
</script>
<html>
    <body>
        <div id="app">
            {{msg}}
        </div>
    </body>
</html>
```

{{}} : 动态渲染视图,类似于jsp中的占位符

Vue提供了大量的预定义元素属性, 可以实现在html中进行动态编程

重要元素属性:

| 属性                          | 作用                                |
| ----------------------------- | ----------------------------------- |
| v-for="(item, index) in list" | 循环                                |
| v-bind:属性="变量"            | 用于在标签属性中引入动态的变量      |
| :属性="变量"                  | v-bind:属性的简写                   |
| v-model="变量"                | 用于将元素的value和变量进行双向绑定 |
| v-on:事件="函数"              | 将函数绑定事件                      |
| @事件="函数"                  | v-on的简写                          |
| v-if="条件表达式"             | 用于进行条件判断                    |
| v-else                        | 多分支条件语句                      |
| v-else-if                     | 多分支条件语句                      |

Vue中给html元素提供了特殊属性ref, 可以直接通过this.$refs.ref值来获取元素的直接引用, 相当于js中的getDocumentBy

当ref在v-for循环中使用时, 获取的元素集合顺序不一定与html中对应

## 生命周期

Vue的生命周期有4个阶段: 创建	挂载	更新	销毁

Vue提供了8种Hook函数对应4种生命周期之前和之后

Hook函数会在对应的生命周期自动执行

| Hook函数        | 顺序                      |
| --------------- | ------------------------- |
| beforeCreate()  | 在new Vue(构造器)之前执行 |
| created()       | 在构造器后执行            |
| beforeMount()   | DOM挂载之前执行           |
| mounted()       | DOM挂载后执行             |
| beforeUpdate()  | vm(虚拟DOM)更新视图之前   |
| updated()       | vm更新视图后              |
| beforeDestory() | Vue实例被销毁前           |
| destory()       | Vue实例被销毁后           |

其中mounted()等同于js中的onload函数

> 一般使用created()函数用于初始化函数

**this关键字**

vue中方法可以通过this关键字调用vue对象的实例成员: 实例方法和实例属性

> 在js中this关键字默认绑定window对象

## 数据计算

Vue有4种方式对数据进行处理

1. 在html中{{}}用行内js直接运算
2. 调用methods中定义的函数进行计算
3. wath属性
4. computed属性

computed属性和methods都可以定义函数,区别在于

computed定义的函数通过函数名调用不需要加()

methods定义的函数通过函数名()调用

computed提供了属性缓存机制, 只有当其返回值的依赖项发生变化时才会重新计算结果值并缓存起来, 当依赖项没有改变时会直接返回缓存的值

如果使用methods定义的函数返回值作为{{}}展示, 将会不停的计算最新值

## Vue类方法

Vue中提供了大量的api方法提高编程效率

| 方法名                                          | 作用                                                 |
| ----------------------------------------------- | ---------------------------------------------------- |
| Vue.set(对象,"属性","属性值")                   | 为对象添加新的属性和属性值或者修改已有的属性的属性值 |
| Vue.delete(对象,"属性")                         | 将对象的某个属性删除                                 |
| Vue.filter("过滤器名称",function(参数){代码块}) | 定义一个全局过滤器                                   |
| filters:{过滤器名称:function(参数){}}           | 定义一个局部过滤器                                   |
| this.$nextTick(lambda表达式)                    | 在挂载成功后执行对应的代码                           |

过滤器通常用于对数据进行处理(直接修改数据本身)

```js
//调用过滤器
{{name:filter}}
```

## component开发

component:组件开发

组件是Vue最强大的功能之一

使用组件可以将app的界面抽象为组件树并进行组件化开发(封装代码和降低代码耦合性)

组件分为全局组件(可以被任意Vue对象使用)和局部组件(只能被所属的Vue对象使用)

> 全局组件

```html
<!--通过标签调用组件-->
<hello></hello>
<script>
//定义一个全局组件
Vue.component("hello",{
    //组件一般和template组合使用
    template:`
    <div>
    <h1>1</h1>
    <hello02></hello02>
    </div>
    `
});
new Vue({
    //选择器绑定
    el:'#app',
    //定义局部选择器
    components:{
        'hello02':{
            template:`
            <div>
    <h1>2</h1>
    <!--组件嵌套-->
    <hello></hello>
    <hello02></hello02>
    </div>
    `
        }
    }
})
</script>
```

## 库跟框架的概念

库: Librarys, 本质上是提供了一系列的方法来调用

如jQuery是将js的方法进行封装, 使编程的效率更高

框架: 框架包含了各种库, 并且在框架内必须按照框架的特殊语法来写代码

区别: 我们可以在写js时调用库中的方法来提供编程效率, 而使用库时本质上不是在写js代码而是在写特定库的代码

前端目前最流行的三个库:

1. Angular
2. React
3. Vue.js

**Vue构造器**

```js
//Vue构造器
new Vue({属性以及方法})
//component构造器
Vue.component("组件名",{属性以及方法});
```

Vue构造器的属性和方法有:

| 属性或方法 | 值                             | 作用                      |
| ---------- | ------------------------------ | ------------------------- |
| el:        | "元素选择器"                   | 绑定对应的元素作为Vue对象 |
| data(){}   | return{变量:变量值}            | 声明Vue对象成员变量       |
| methods:{} | 方法名: function(参数){方法体} | 声明Vue对象成员方法       |

Vue.component构造器的属性和方法有:

| 属性或方法 | 值                             | 作用             |
| ---------- | ------------------------------ | ---------------- |
| template:  | \`html代码\`                   | 网页组件         |
| data(){}   | return{变量:变量值}            | 声明组件成员变量 |
| methods:{} | 方法名: function(参数){方法体} | 声明组件成员方法 |



**Vue自定义的js必须在绑定的标签后引入, 否则会发生element  not found的错误**

```html
<div id="app">
    
</div>
<script src="自定义的js.js"></script>
```

