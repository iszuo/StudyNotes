# Vue常用参数

| 参数           | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| data           | 该函数返回组件实例的 data 对象。                             |
| computed       | 计算属性将被混入到组件实例中。                               |
| methods        | methods将被混入到组件实例中。                                |
| watch          | 一个对象，键是要侦听的响应式 property。                      |
| template       | 一个字符串模板，用作 component 实例的标记。                  |
| component      | 声明一组可用于组件实例中的组件。                             |
| props          | 一个用于从父组件接收数据的数组或对象。                       |
| provide/inject | 这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。 |

# vue钩子函数

## 生命周期钩子函数

| 参数化          | 说明                                                    |
| --------------- | ------------------------------------------------------- |
| beforeCreate()  | 实例创建前触发                                          |
| created()       | 实例创建完成，                                          |
| beforeMount()   | 模板渲染前，可以访问数据，模板编译完成，虚拟DOM已经存在 |
| mounted()       | 模板渲染完成，可以拿到DOM节点和数据                     |
| beforeUpdate()  | 更新前                                                  |
| updated()       | 更新完成                                                |
| activated()     | 激活前                                                  |
| deactivated()   | 激活后                                                  |
| beforeDestroy() | 销毁前                                                  |
| destroyed()     | 销毁后                                                  |

## 自定义指令**directives**的钩子函数

| 参数化           | 说明                                         |
| ---------------- | -------------------------------------------- |
| bind             | 绑定指令到元素上，只执行一次                 |
| inserted         | 定了指令的元素插入到页面中展示时调用，很常用 |
| update           | 所有组件节点更新时调用                       |
| componentUpdated | 指令所在的节点及其子节点全部更新完成后调用   |
| unbind           | 解除指令和元素的绑定，只执行一次             |

## 路由导航 / 路由守卫 钩子函数

### 1、全局守卫

1. 前置：router.beforeEach（(to,from,next)=>{ }）
2. 后置：router.afterEach（(to,from)=>{ }）

### 2、路由独享守卫

- beforeEnter:（to,from,next)=>{  }

### 3、导航守卫

1. beforeRouteEnter（to,from,next）{  }
2. beforeRouteLeave（to,from,next）{ }

# 过滤器

## 语法

- {{ data | filter1(参数) | filter2(参数) }}

## 自定义过滤器

- 全局过滤器
- 使用全局方法**Vue.filter(过滤器ID，过滤器函数)，**过滤器写在window.onload=function(){ new Vue...}的外面。

```js
//自定义全局过滤器，将小于10的数字十位补0
Vue.filter('date',data=>{
	let d = new Date(data);
	return d.getFullYear()+'-'+(d.getMonth()+1)+'-'+d.getDate()+' '+d.getHours()+':'+d.getMinutes()+':'+d.getSeconds()
})
```

- 局部过滤器
- 使用全局方法**filters:{过滤器ID:(过滤器参数1，过滤器参数2...)=>{函数回调}}，**过滤器写在new Vue里面。

```js
//自定义局部过滤器，将数字保留3位小数
filters:{
  number:(data,n)=>{
    //console.log(data,n)
    return data.toFixed(n)
  }
}
```

# 指令

## 内置指令

1. v-for： 遍历列表，对象，整数（从1开始） (是否设置key值，动态双向绑定)

2. v-cloak    页面渲染完成后消失；如果不适用的话，每次刷新页面会出现闪的一下子；就好比王者英雄出了闪现

3. v-bind： 动态绑定 缩写 

   ```vue
   <div id="app">
       <h1>使用v-bind  设置style属性</h1>
       <p v-bind:style="s1">段落1</p>
       <p v-bind:style="s2">段落2</p>
       <p v-bind:style="[s2,s3]">段落3</p>
   </div>
   <script>
       var vm = new Vue({
           el:"#app",
           data:{
               s1:"color:red;border:5px solid green;width:50px;",
               s2:{
                   color:'red',
                   border: '5px solid gray',
                   width:'50px'
               },
               s3:{
                   padding:"10px"
               }
           }
       })
   </script>
   ```

   ![image-20220518151130616](https://s2.loli.net/2022/05/18/dU54K7q8mGvWVDT.png)

4. v-model: 一些表单元素的双向绑定

5. v-html  innerHTML输出

6. v-text  innerText输出

7. v-once  只渲染一次

8. **v-on 事件处理（敲黑板！划重点）**缩写 @

9. v-pre  跳出渲染

10. v-if   条件判断，当然还有配套的v-else、v-else-if

## 自定义指令

- 使用方式：先注册后使用

1. 符合注册指令：全局，局部(只可以在绑定的视图中使用)

   ```vue
   // 全局注册
   Vue.directive(id,[definition])
   // 局部注册 v-focus
   new Vue({
       el:"#app",
       directives: {
           focus: {
           }
       }
   })
   
   ```

2. 使用指令：与Vue提供的内置指令使用方式一样。

   ```vue
   <div v-mydirective></div>
   // v-mydirective为自己定义的指令
   
   ```

   
