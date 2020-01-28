# `Vuex`

把需要多个组件共享的变量全部存储在一个对象中，然后放在顶层的`Vue`实例里，让其它组件可以使用。

多个组件就可以共享这个对象中的所有变量属性了。

用户的登录状态、用户名称、头像、地理位置信息、商品的收藏、购物车中的物品等。

```shell
npm install vuex --save
```

![Vuex状态管理](C:\Users\lenovo\Desktop\5天背诵\img\vue\Vuex状态管理.jpg)

![Vuex状态管理图示](C:\Users\lenovo\Desktop\5天背诵\img\vue\Vuex状态管理图示.jpg)

![Vuex基础使用](C:\Users\lenovo\Desktop\5天背诵\img\vue\Vuex基础使用.jpg)

- `State`：保存状态、共享状态的地方，单一状态树，单一数据源
  - 只创建一个`Store`对象进行数据的存储
- `Getters`：类似组件中的计算属性
- `Mutation`：
- `Action`：处理异步操作
- `Module`：划分不同的模块，并做数据的保存



## `Getters`

基本使用：

![getters基本使用](C:\Users\lenovo\Desktop\5天背诵\img\vue\getters基本使用.jpg)

需求：拿到数据对象中年龄小于20岁的成员

![vuex处理](C:\Users\lenovo\Desktop\5天背诵\img\vue\vuex处理.jpg)

需求：统计出上述满足条件的个数

![第二个参数](C:\Users\lenovo\Desktop\5天背诵\img\vue\第二个参数.jpg)

需求：用户来进行输入年龄，然后我们根据传入的数值进行筛选出复合条件的结果

![getter](C:\Users\lenovo\Desktop\5天背诵\img\vue\getter.jpg)



## `Mutation`

### 状态更新

`store`状态更新的唯一方式：提交`Mutation`。

`Mutation`主要包括两部分：

- 字符串的事件类型`type`
- 一个回调函数，该回调函数的第一个参数就是`state`

需求：我们需要给变化的方法指定传递的参数

![传递参数](C:\Users\lenovo\Desktop\5天背诵\img\vue\传递参数.jpg)

需求：点击按钮，像数组中传递一个对象

![传递对象](C:\Users\lenovo\Desktop\5天背诵\img\vue\传递对象.jpg)

提交风格：

![另一种风格](C:\Users\lenovo\Desktop\5天背诵\img\vue\另一种风格.jpg)

### 响应规则

- 提前在`store`中初始化好所需的属性
- 当给`state`中的对象添加新属性时，使用下面的方式

```js
// 方式1:
Vue.set(obj, 'newProp', 123)
```

```js
// 方式2:
用新对象给旧对象重新赋值
```

![vuex响应式原理](C:\Users\lenovo\Desktop\5天背诵\img\vue\vuex响应式原理.jpg)

但是，如果我们通过`state.info['address'] = '...'`的方式给`state`中添加新值，是没有响应式的。

![响应式2](C:\Users\lenovo\Desktop\5天背诵\img\vue\响应式2.jpg)

解决：我们需要使用`Vue.set()`方法去实现新添加功能。

![实现](C:\Users\lenovo\Desktop\5天背诵\img\vue\实现.jpg)

同样我们想要进行删除的话，使用`Vue.delete()`方法进行删除操作。

![删除](C:\Users\lenovo\Desktop\5天背诵\img\vue\删除.jpg)

### 同步函数

通常情况下，`Vuex`要求我们`Mutation`中的方法必须是同步方法，主要的原因是当我们使用`devtools`的时候，`devtools`可以帮助我们捕获`mutation`的快照，但是如果是异步操作的话，那么`devtools`将不能很好的追踪这个操作是什么时候完成的了。



## `Action`异步操作

![异步操作](C:\Users\lenovo\Desktop\5天背诵\img\vue\异步操作.jpg)

==注意==：处理异步操作还是不能够绕过`Mutation`的，业务处理依旧位于`Mutation`中，单纯的将异步处理操作放在`Actions`里面，当然了，按钮点击触发的事件里面应该使用`dispatch`进行处理操作。

```js
// 需要进行参数的传递的话
// 按钮点击事件的写法
updateInfo() {
  this.$store.dispatch('aUpdateInfo','我是传递过去的参数')
}

// actions里面的写法,payload是用于接收的参数
actions: {
   aUpdateInfo(context,payload) {
      setTimeout(() => {
         console.log(payload)
         context.commit('updateInfo')
      }, 1000)
   }
}
```

接着上面的栗子：传递的参数是一个对象形式，里面还有方法。

```js
updateInfo() {
  this.$store.dispatch('aUpdateInfo',{ // 将传递过去的数据以对象的形式传递过去
    msg: '我是传递过去的参数',
    success: () => {
      console.log('里面数据已经更新完成了...')
    }
  })
}

actions: {
   // context上下文
   aUpdateInfo(context,payload) {
      setTimeout(() => {
         console.log(payload) // 打印接收到的对象
         context.commit('updateInfo') // 改变数据
         console.log(payload.msg) // 显示数据已经更新完成
         payload.success() // 执行传递过来的对象里面的函数
      }, 1000)
   }
}
```

![实现方式2](C:\Users\lenovo\Desktop\5天背诵\img\vue\实现方式2.jpg)



## `Modules`

`Vuex`使用单一状态树，那么也意味着很多状态都会交给`Vuex`来管理。

当应用变得非常复杂的时候，`store`对象就有可能变得非常臃肿。

所以`Vuex`允许我们将`store`分割成模块`Modules`，而每个模块都有自己的`state`、`mutations`、`actions`、`getters`等。

![Module](C:\Users\lenovo\Desktop\5天背诵\img\vue\Module.jpg)

传递参数：

![mutation2](C:\Users\lenovo\Desktop\5天背诵\img\vue\mutation2.jpg)

![modules](C:\Users\lenovo\Desktop\5天背诵\img\vue\modules.jpg)

![getters3](C:\Users\lenovo\Desktop\5天背诵\img\vue\getters3.jpg)

演示：

![actions](C:\Users\lenovo\Desktop\5天背诵\img\vue\actions.jpg)



## 将`Vuex`中的代码进行目录拆分

![代码拆分](C:\Users\lenovo\Desktop\5天背诵\img\vue\代码拆分.jpg)

![目录抽分](C:\Users\lenovo\Desktop\5天背诵\img\vue\目录抽分.jpg)



# `axios`

浏览器的安全性限制，不允许`ajax`访问协议不同，域名不同，端口号不同的数据接口。

`Vue`发送网络请求的方式：

- 传统`ajax`基于`XMLHttpRequest(XHR)`
- `jQuery-Ajax`
- `Vue-resource`
- `axios`

`axios`功能特点：

- 在浏览器中发送`XMLHttpRequests`请求
- 在`node`中发送`http`请求
- 支持`Promise API`
- 拦截请求和响应
- 转换请求和响应数据

![jsonp](C:\Users\lenovo\Desktop\5天背诵\img\vue\jsonp.jpg)

通过动态创建`script`标签的形式，把`script`标签的`src`属性，指向数据接口地址。

因为`script`标签不存在跨域限制，这种数据获取方式称作`JSONP`，只支持`Get`请求。

![jsonp封装](C:\Users\lenovo\Desktop\5天背诵\img\vue\jsonp封装.jpg)

```shell
npm install axios --save
```

![axios](C:\Users\lenovo\Desktop\5天背诵\img\vue\axios.jpg)

## 发送并发请求

```js
axios.all([axios({}), axios({})]).then()

axios.all([axios({
  url: 'http://123.207.32.32:8000/home/multidata'
}),axios({
  url: 'http://123.207.32.32:8000/home/multidata',
  params: {
    type: 'sell',
    page: 3
  }
})]).then(result => {
        console.log(result)
     })
```

```html
<script>
   import axios from "axios"
   export default{
      data() {
         return {
            email: ''
         }
      }
   },
   created() {
      axios.get(第一个参数是获取数据的地址,第二个参数是请求头配置项(选填))
            .then( data => {
                  // 处理数据
                  // console.log(data)
                  const arr = [];
                  for(let key in data.data) {
                     let myData = data.data[key];
                     myData.id = key;
                     arr.push(myData);
                  }
                  this.email = arr[i].email
            }, err => {

            })
      }
</script>
```



## 全局配置

将请求的根路径以及请求期望时间设置为全局配置。

```js
// 使用全局axios和对应的配置进行网络请求
axios.defaults.baseURL = 'http://123.207.32.32:8000' // 全局配置根路径
axios.defaults.timeout = 5000 // 全局配置超时时间

axios.all([axios({
  url: '/home/multidata'
}),axios({
  url: '/home/multidata',
  params: {
    type: 'sell',
    page: 3
  }
})]).then(result => {
        console.log(result)
     })
```

常见配置选项：

- 请求地址：`url: '/user'`
- 请求类型：`method: 'get'`
- 请求根路径：`baseURL: 'http://www.mt.com/api`
- 请求前的数据处理：`transformResponse: [function(data)]`
- 自定义请求头：`headers: {'x-Requested-With':'XMLHttpRequest'}`
- `URL`查询对象：`params: {id: 12}`

- 查询对象序列化函数：`paramsSerializer: function(params) {}`
- `request body`：`data: {key: 'aa'}`
- 超时设置：`timeout: 1000`

## `axios`的实例和模块封装

```js
// 当我们有不同的服务器来提供不同的url地址的时候,我们可以通过下面这种实例化的形式,进行网络请求
const instance1 = axios.create({
  baseURL: 'http://123.207.32.32:8000',
  timeout: 5000
})

instance1({
  url: '/home/multidata'
}).then(res => {
  console.log(res);
})
instance1({
  url: '/home/multidata',
  params: {
    type: 'pop',
    page: 1
  }
}).then(res => {
  console.log(res);
})
```

![请求网络数据](C:\Users\lenovo\Desktop\5天背诵\img\vue\请求网络数据.jpg)

以上发起网络请求的方式不好，因为其对发起网络请求的框架依赖性太强。

![网络封装](C:\Users\lenovo\Desktop\5天背诵\img\vue\网络封装.jpg)

使用`Promise`的方式进行封装：

![使用Promise的方式](C:\Users\lenovo\Desktop\5天背诵\img\vue\使用Promise的方式.jpg)

再次封装：

==注意==：`instance(config)`本身就是返回一个`Promise`。

![Promise再次封装](C:\Users\lenovo\Desktop\5天背诵\img\vue\Promise再次封装.jpg)

```js
function test(a,b) {
    a(‘传递的参数’)
}

test(function(res){console.log(res) // 拿到传递的参数},function(){})
// 将两个函数传递给函数test中的形参a和b,a形参内部的指针指向的是函数function(){},而函数test中执行的代码是a自执行,就相当于function(){}函数自执行了
```



## 拦截器

提供了4种拦截。

![拦截器](C:\Users\lenovo\Desktop\5天背诵\img\vue\拦截器.jpg)

拦截器的使用：

![拦截器2](C:\Users\lenovo\Desktop\5天背诵\img\vue\拦截器2.jpg)



# `Vue`底层原理

[Vue双向绑定](http://www.cnblogs.com/kidney/p/6052935.html)

实例上的三个属性：

- `$el`：该`Vue`实例所控制的代码段
- `_data`：我们传进去的`data`
- `$refs`：获得所控制代码块的`DOM`节点

数据绑定的实现：

```js
Object.definedProperty(vm, 'title', { get(){},set(){}} )
```

`Vue`底层为我们做了三件事情：

- 将`new`出来的对象上的所有属性和方法中的`this`指向全部都指向当前的`Vue`实例，保证了`this`指向一致性
- 将属性和方法全部都挂载到`Vue`对象上，通过`this`获取到
- 通过`set`函数监听数据











