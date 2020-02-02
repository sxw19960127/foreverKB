# 1.脚手架初始化项目

```shell
vue create name
```

## 安装依赖

- `Babel`
- `CSS Pre-processors`
- `Sass/SCSS (with dart-sass)`

## 初始化项目

将脚手架搭建出来的项目中不需要的部分进行删除，初始化项目。

```js
// main.js 项目入口模块
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

new Vue({
   el: '#app',
   render: h => h(App)
})
```

```vue
<!-- App.vue 根组件 -->
<template>
  <div>根组件</div>
</template>
```



# 2.项目开发流程

## 一级路由设计

![路由设计](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\路由设计.jpg)

先设计最底下的电影、影院、我的三大路由展示页组件，结构如下：

![底部三个大路由](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\底部三个大路由.jpg)

```shell
路由下载
npm install vue-router --save
```

在`src`下新建`router`文件夹，里面再新建`index.js`路由入口模块：

```js
import VueRouter from 'vue-router'
import Vue from 'vue' 

const Movie = () => import('@/views/Movie') 
const Cinema = () => import('@/views/Cinema')
const Mine = () => import('@/views/Mine')

Vue.use(VueRouter) 

const router = new VueRouter({ // 实例化路由
    routes: [
    	{path: '/movie',component: Movie},
  		{path: '/cinema',component: Cinema},
  		{path: '/mine',component: Mine}
    ],
    mode: 'history'
})

export default router
```

```js
// main.js 项目入口模块

import router from './router' // 1.导入
new Vue({
   el: '#app',
   router, // 2.注册
   render: h => h(App)
})
```

```vue
<!-- 回到App.vue根组件,放置用以展示一级路由的坑 -->

<template>
  <div>
    根组件
    <router-view />  
  </div>
</template>
```

路由设计后通过手动更改`url`以验证路由是否配置成功！



# 3.`git`管理项目

## 创建分支

```shell
git checkout -b dev 创建并且连接到分支dev
```

```shell
git push ck dev
```



# 4.移动端适配

```html
<!-- 不允许用户拖放设置 user-scalable -->
<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no">
```



# 5.配置资源请求路径

```html
<!-- 在index.html中的href属性 <%= BASE_URL %> 和路由中的base字段相对应 -->
<link rel="icon" href="<%= BASE_URL %>favicon.ico"> // 1.
```

```js
const router = new VueRouter({  
    routes: [
    	{path: '/movie',component: Movie},
  		{path: '/cinema',component: Cinema},
  		{path: '/mine',component: Mine}
    ],
    mode: 'history',
    base: process.env.BASE_URL // 2.
})
```



# 6.`public`文件夹

`public`文件夹下，新建一个`css`文件夹，用以存放静态样式文件。

将`iconfont`图标样式和初始化项目样式放进去，并在`index.html`中引入：

```html
<link rel="stylesheet" href="<%= BASE_URL %>css/common.css">
<link rel="stylesheet" href="<%= BASE_URL %>css/iconfont.css">
```

打开浏览器控制台，在`network`中可以发现上述两个`css`已经全部加载完毕！



# 7.公共头部和底部组件

 ```vue
// 1.头部组件

<template>
   <header id="header">
      <h1>mini_cinema</h1>
   </header>
</template>

<script>
export default {
   name: 'Header' // 添加name属性后,方便调试组件
}
</script>

<style lang="scss" scoped>
   #header{
      width: 100%;
      height: 100%;
      color: #fff;
      background-color: #e54847;
      border-bottom: 1px solid #e54847;
      position: relative;
      h1{
         font-size: 18px; 
         font-weight: normal;
         text-align: center;
         line-height: 50px;
      }
   }
</style>
 ```

```vue
// 2.底部组件

<template>
  <footer id="footer">
     <ul>
         <li>
            <i class="iconfont icon-dianying"></i>
            <p>电影</p>
         </li>
         <li>
            <i class="iconfont icon-yingyuan"></i>
            <p>影院</p>
         </li>
         <li>
            <i class="iconfont icon-wode"></i>
            <p>我的</p>
         </li> 
     </ul>
  </footer>
</template>

<script>
export default {

}
</script>

<style lang="scss" scoped>
   #footer{
      width: 100%;
      height: 50px;
      background-color: #fff;
      border-top: 2px solid #ebe8e3;
      position: fixed;
      left: 0;
      bottom: 0; 
      ul{
         text-align: center;
         height: 50px;
         align-items: center;
         display: flex;
         li{
            height: 40px;
            flex: 1;
            .active{
               color: #f03d37;
            }
         }
         i{
            font-size: 20px;
         }
         p{
            font-size: 12px;
            line-height: 18px;
         }
      }
   }
</style>
```

## 在`Movie`页引入上述组件

```vue
<template>
  <div id="main">
    <!-- 3.使用 -->
    <Header />
    <TabBar />
  </div>
</template>

<script>
import Header from '@/components/Header' // 1.引入
import TabBar from '@/components/TabBar'
    
export default {
  name: 'Movie',
  components: {
    Header, // 2.注册
    TabBar
  }
}
</script>
```



# 8.缓存处理

在`App.vue`中，使用`<keep-alive>`将路由展示包裹起来，在进行路由切换时，会有缓存效果。

```vue
<template>
  <keep-alive>
    <router-view />  
  </keep-alive>
</template>
```



# 9.`router-link`路由跳转

```vue
 <router-link tag="li" to="/movie">
    <i class="iconfont icon-dianying"></i>
    <p>电影</p>
 </router-link>
```

## 当前路由的高亮设置

```css
.router-link-active{
   color: #f03d37;
}
```

同样在`cinema`和`mine`页面也引入上述两个组件！



# 10.切换路由时头部标题的切换

在头部组件中，标题是写死的，我们需要在调用头部组件的时候，实现==父子通信==，将标题写成一个动态信息。

```js
// 头部组件中通过props
export default {
   name: 'Header',
   props: {
      title: {
         type: String,
         default: 'mini_cinema'
      }
   }
}
```

回到`cinema`和`mine`组件，将`title`字段值传递出去：

```vue
<template>
  <div id="main">
    <Header title="电影院" /> <!-- 这里为啥不用属性绑定的形式 ? -->
    <TabBar />
  </div>
</template>

<script>
import Header from '@/components/Header'
import TabBar from '@/components/TabBar'
    
export default {
  name: 'Cinema',
  components: {
    Header,
    TabBar
  }
}
</script>
```



# 11.二级路由 

 ![二级路由](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\二级路由.jpg)

在`components`文件夹中，新建4个二级子路由，分别是`City`、`NowPlaying`、`ComingSoon`、`Search`！

## 配置二级路由

```js
const router = new VueRouter({  
    routes: [
        {path: '/movie',component: Movie,
            children: [
                {path: 'city',component: () => import('@/components/City')},
                {path: 'nowPlaying',component: () => import('@/components/NowPlaying')},
                {path: 'comingSoon',component: () => import('@/components/ComingSoon')},
                {path: 'search',component: () => import('@/components/Search')}
            ]
        },
  		{path: '/cinema',component: Cinema},
        {path: '/mine',component: Mine},
        {path: '/*',redirect: '/movie'} // 路由重定向
    ],
    mode: 'history'
})
```

## 将结构写入组件

`Movie`组件：

```vue
<template>
  <div id="main">
    <!-- 头部 -->
    <Header />
      
    <!-- 二级路由切换部分 -->
    <div id="content">
      <div class="movie_menu">
        <div class="city_name">
          <span>大连</span>
          <i class="iconfont icon-lower-triangle"></i>
        </div>
        <div class="hot_swtich">
          <div class="hot_item active">正在热映</div>
          <div class="hot_item">即将上映</div>
        </div>
        <div class="search_entry">
          <i class="iconfont icon-sousuo"></i>
        </div>
      </div>
       
      <!-- 二级路由展示区 -->
      <keep-alive>
        <router-view></router-view>
      </keep-alive>
    </div>
 
    <!-- 底部 -->
    <TabBar />
  </div>
</template>
```

## 二级路由内容结构

4个二级页面，这四个页面位于`movie`一级路由下，分别是：

- `City`
- `NowPlaying`
- `ComingSoon`
- `Search`

  ![二级路由切换](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\二级路由切换.jpg)

```vue
<!-- 使用 router-link 属性 -->

<div id="content">
  <div class="movie_menu">
    <router-link tag="div" to="/movie/city" class="city_name">
      <span>大连</span>
      <i class="iconfont icon-lower-triangle"></i>
    </router-link>
    <div class="hot_swtich">
      <router-link tag="div" to="/movie/nowPlaying" class="hot_item">正在热映</router-link>
      <router-link tag="div" to="/movie/comingSoon" class="hot_item">即将上映</router-link>
    </div>
    <router-link tag="div" to="/movie/search" class="search_entry">
      <i class="iconfont icon-sousuo"></i>
    </router-link>
  </div>

  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</div>
```

## 二级路由高亮 

```css
.router-link-active{
  color: #ef4238;
  border-bottom: 2px solid #ef4238;
}
```

## 重定向二级路由

```js
const router = new VueRouter({  
    routes: [
        {path: '/movie',component: Movie,
            children: [
                {path: 'city',component: () => import('@/components/City')},
                {path: 'nowPlaying',component: () => import('@/components/NowPlaying')},
                {path: 'comingSoon',component: () => import('@/components/ComingSoon')},
                {path: 'search',component: () => import('@/components/Search')},
                {path: '/movie',redirect: '/movie/nowPlaying'} // 二级路由重定向
            ]
        },
  		{path: '/cinema',component: Cinema},
        {path: '/mine',component: Mine},
        {path: '/*',redirect: '/movie'}
    ],
    mode: 'history'
})
```



# 12.影院和我的一级路由内容

![影院](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\影院.jpg)

创建两个组件`CiList`（插入到一级路由组件`Cinema`中间部位的）和`Login`（用来插入到一级路由组件`Mine`中的中间部位的）。

回到`Cinema`一级路由组件，完善结构。

同样完成`Login.vue`组件，然后回到`Mine`一级路由组件页面，里面原本是有两个组件了已经，分别是头部组件和底部组件，所以我们将新完成的组件`Login.vue`组件导入到`Mine`组件里，注册，使用。那么这个`Mine`页面组件就完成了。

至此，基本上的组件页面拆分就已经完成了。



# 13.`git`管理阶段代码

```shell
git status // 查看代码的修改情况
git add .
git commit -m ""  
git checkout dev // 切换到开发分支
git merge createComponents --no-ff // 参数-no-ff方便后期我们查看日志
git log // 打印日志

git push ck dev 
git branch // 查看当前分支数
git branch -d createComponents // 删除分支
```

 

# 14.请求真实数据

```shell
git checkout -b setData // 创建新分支
```

## 接口文档

```
正在热映
http://39.97.33.178/api/movieOnInfoList?cityId=10
即将上映
http://39.97.33.178/api/movieComingList?cityId=10
搜索
http://39.97.33.178/api/searchList?cityId=10&kw=a
城市
http://39.97.33.178/api/cityList
电影详情
http://39.97.33.178/api/detailmovie?movieId=345808
影院
http://39.97.33.178/api/cinemaList?cityId=10
城市定位
http://39.97.33.178/api/getLocation
```

## 反向代理解决跨域

新建`vue.config.js`文件：

```js
module.exports = {
    devServer: {
        proxy: {
            '/api': {
                target: 'http://39.97.33.178',
                changeOrigin: true
            }
        }
    }
}
```

重启项目，下载`axios`

```shell
npm install axios --save
```

## `city`二级路由页获取数据

先在入口函数模块`main.js`中，引入`axios`：

```js
// vue是面向对象编程,vue是一个类,我们可以对其添加一些方法
import axios from 'axios'
Vue.prototype.axios = axios // 完成添加后,可以通过this.axios调用
```

在`mounted`生命周期中发起`axios`：

```js
// City页面
export default {
   name: 'City',
   data() {
      return {
         // 将请求到的数据保存在这里,以做到响应式
         cityList: [],
         hotList: []
      }
   },
   mounted() {
      // 发起网络请求
      this.axios.get('/api/cityList').then(res => {
         // console.log(res)
         let msg = res.data.msg
         if(msg === 'ok') {
            // 获取到所有数据
            let cities = res.data.data.cities 
            
            // [ { index: 'A',list: [{ name: '安徽',id: 1 },{ name: '埃及',id: 2 } ]} ] 将数据整理成这种结构
            // 封装函数,将原始数据传递进去,以达到格式化效果
            var { cityList,hotList } = this.formatCityList(cities) // 对象的解构赋值
            
            this.cityList = cityList; // 将数据保存在自己data上
            this.hotList = hotList;
         }
      })
   },
   methods: {
      formatCityList(cities) {
         let cityList = [] // 最后整理完成的数据
         let hotList = [] // 筛选出id为1的热门城市

         // 整理热门城市,一层判断就完成了
         for(var i = 0;i < cities.length;i ++) {
            if(cities[i].isHot === 1) {
               hotList.push(cities[i])
            }
         }
		 
         // 整理全部城市,并按首字母顺序进行排序
         for(var i = 0;i < cities.length;i ++) {
            // 截取第一个英文字母并转换为大写形式
            var firstLetter = cities[i].py.substring(0,1).toUpperCase()
            
            if(toCom(firstLetter)) { // 是新添加的index
               cityList.push({ index: firstLetter,list: [{ nm: cities[i].nm, id: cities[i].id }]})
            }else { // 累加到已有index索引中list数组里
               for(var j = 0;j < cityList.length;j ++) {
                  if(cityList[j].index === firstLetter) {
                     cityList[j].list.push({ nm: cities[i].nm, id: cities[i].id })
                  }
               }
            }
         }
          
		 // 仅判断是否有相同数据存在
         function toCom(firstLetter) {
            for(var i = 0;i < cityList.length;i ++) {
               if(cityList[i].index === firstLetter) {
                  return false
               }
            }
            return true
         }

         // console.log(cityList)
		
         // 将整理完成的数据按字母顺序进行整理排列
         cityList.sort((n1,n2) => {
            if(n1.index > n2.index) {
               return 1
            }else if(n1.index < n2.index) {
               return -1
            }else {
               return 0
            }
         })

         // console.log(cityList)
         // console.log(hotList)
          
         // 将最终数据return出去
         return {
            cityList,
            hotList
         }
      }
   }
}
```

![城市数据](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\城市数据.jpg)

## 渲染数据

```html
<div class="city_body">
  <div class="city_list">
     <!-- <div class="city_hot">
        <h2>热门城市</h2>
        <ul class="clearfix">
           <li>上海</li>
        </ul>
     </div>
     <div class="city_sort">
        <div>
           <h2>A</h2>
           <ul>
              <li>阿拉善盟</li>
              <li>鞍山</li>
              <li>安庆</li>
              <li>安阳</li>
           </ul>
        </div>
     </div> -->
     <div class="city_hot">
        <h2>热门城市</h2>
        <ul class="clearfix">
           <li v-for="item in hotList" :key="item.id">{{ item.nm }}</li>
        </ul>
     </div>
     <div class="city_sort">
        <div v-for="item in cityList" :key="item.index">
           <h2>{{ item.index }}</h2>
           <ul>
              <li v-for="itemList in item.list" :key="itemList.id">{{ itemList.nm }}</li>
           </ul>
        </div>
     </div>
  </div>
  <div class="city_index">
     <ul>
        <!-- <li>A</li>
        <li>B</li>
        <li>C</li>
        <li>D</li>
        <li>E</li> -->
        <li v-for="item in cityList" :key="item.index">{{ item.index }}</li>
     </ul>
  </div> 
</div>
```

## 点击右侧字母改变滚动条位置

![城市渲染完毕图](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\城市渲染完毕图.jpg)

移动端触摸事件`touchstart`事件！

```vue
<div class="city_index">
    <ul>
        <!-- <li>A</li>
        <li>B</li>
        <li>C</li>
        <li>D</li>
        <li>E</li> -->
        // 将每一项的索引传递给事件handleToIndex
        <li v-for="(item,index) in cityList" :key="item.index" @touchstart="handleToIndex(index)">{{ item.index }}</li>
    </ul>
</div> 
methods: {
	handleToIndex(index) {
		
	}
}
```

```html
 <div class="city_sort" ref="city_sort"> // 我们在最外层的div上添加一个ref属性
    <div v-for="item in cityList" :key="item.index">
       <h2>{{ item.index }}</h2> // 这里是展示左侧 A B C D...的地方
       <ul>
          <li v-for="itemList in item.list" :key="itemList.id">{{ itemList.nm }}</li>
       </ul>
    </div>
 </div>
methods: {
	handleToIndex(index) {
		var h2 = this.$refs.city_sort.getElementsByTagName('h2') 
		// 找到h2的DOM元素的父元素,并将对应点击的目标元素距离顶部距离赋给父元素
		this.$refs.city_sort.parentNode.scrollTop = h2[index].offsetTop  ****
	}
}
```



# 15.正在热映和即将上映页面数据

```
正在热映
http://39.97.33.178/api/movieOnInfoList?cityId=10
```

```js
export default {
   name: 'NowPlaying',
   data() {
      return {
         movieList: []
      }
   },
   mounted() {
      this.axios.get('/api/movieOnInfoList?cityId=476').then((res) => {
         var msg = res.data.msg;
         if(msg === 'ok') {
            this.movieList = res.data.data.movieList;
         }
      })
   }
}
```

## 过滤器设置图片大小

```js
// 入口模块main.js中,过滤url,replace其w.h,替换为arg

Vue.filter('setWH',(url,arg) => {
    return url.replace(/w\.h/,arg)
})
```

```html
 <li v-for="item in movieList" :key="item.id">
    <div class="pic_show">
       <img :src="item.img | setWH('128.180')" alt=""> // 1.宽高
    </div>
    <div class="info_list">
       <h2>{{ item.nm }} <img v-if="item.version" src="@/assets/maxs.png" alt=""></h2> 
       <p>观众评 <span class="grade">{{ item.sc }}</span></p>
       <p>主演：{{ item.star }}</p>
       <p>{{ item.showInfo }}</p>
    </div>
    <div class="btn_mall">
       购票
    </div>
 </li>
```

以上就是正在热映真实数据渲染，即将到来页面与正在热映差不多。



# 16.搜索页面数据渲染

数据的渲染是当我们在输入框中输入内容之后，才检索出数据的，所以数据请求不是和之前一样在`mouted`里。

- 给`input`输入框添加双向数据绑定指令，用以获取到输入的值

如何根据我们输入的值去获取数据，而且`axios`是异步进行获取数据的，所以`computed`计算属性不能使用，应该使用`watch`去监听。

```js
export default {
   name: 'Search',
   data() {
      return {
         message: '',
         moviesList: []
      }
   },
   watch: {
      // 我们需要实时去监听message字段的变化
      message(newValue) {
         this.axios.get('/api/searchList?cityId=10&kw=' + newValue).then(res => {
            var msg = res.data.msg;
            var movies = res.data.data.movies;
            if(msg && movies) {
               // 符合搜索要求的数据赋值给moviesList进行保存
               this.moviesList = res.data.data.movies.list;
            }
         })
      } 
   }
}
```

```html
<input type="text" v-model="message">

<li v-for="item in moviesList" :key="item.id">
   <div class="img">
      <img :src="item.img | setWH('128.180')">
   </div>
   <div class="info"> 
      <p>
         <span>{{ item.nm }}</span>
         <span>{{ item.sc }}</span>
      </p>
      <p>{{ item.enm }}</p>
      <p>{{ item.cat }}</p>
      <p>{{ item.rt }}</p>
   </div>
</li>
```

## 函数防抖

当我们在上述输入框中频繁地输入内容时，`watch`就会触发多次。

我们所希望的是，当快速输入内容的时候，不发起请求，当我们输入内容完毕之后，才发起请求。

- 方法1，使用`clearTimeout()`和`setTimeout()`每次发起请求的时候都会清除上次发起请求，达到效果
- 方法2，使用`axios`终止多次请求   ***

```js
methods: {
  // 1
  cancelRequest() {
     if(typeof this.source === 'function') {
        this.source('终止请求')
     }
  }
},
watch: {
  message(newValue) {
     // 2
     var that = this;
     // 3
     this.cancelRequest();
     this.axios.get('/api/searchList?cityId=10&kw=' + newValue,{ // 4.再添加一个参数
        cancelToken: new this.axios.CancelToken(function(c) {
           that.source = c;
        })
     }).then(res => {
        var msg = res.data.msg;
        var movies = res.data.data.movies;
        if(msg && movies) {
           this.moviesList = res.data.data.movies.list;
        }
     }).catch(err => { // 5
        if(this.axios.isCancel(err)) {
           console.log('Rquest canceled', err.message); // 请求如果被取消,这里是返回取消的message
        }else {
           // handle error
           console.log(err);
        }
     })
  } 
}
```

![函数防抖](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\函数防抖.jpg)



# 17.影院页数据渲染

接口没有

```html
<li v-for="item in cinemaList" :key="item.id">
    <div>
       <span>{{ item.nm }})</span>
       <span class="q">
          <span class="price">{{ item.sellPrice }}</span> 元起
       </span>
    </div>
    <div class="address">
       <span>{{ item.addr }}</span>
       <span>{{ item.distance }}</span>
    </div>
    <div class="card">
       // 遍历对象也可以使用v-for指令,其中num遍历的是对象的属性名,key遍历的是对象的属性值
       // 调用局部过滤器,将 {{key | formateCard}} key交给formateCard去处理
       <div v-for="(num,key) in item.tag" v-if="num === 1" :key="key">{{ key | formatCard }}</div>
    </div>
 </li>
```

```js
export default {
   name: 'CiList',
   data() {
      return {
         cinemaList: []
      }
   },
   mounted() {
      this.axios.get('/api/cinemaList?cityId=3').then(res => {
         var msg = res.data.msg;
         if(msg === 'ok') {
            this.cinemaList = res.data.data.cinemas;
         }
      })
   },
   filters: { // 局部过滤器
      formatCard(key) {
         var card = [
            { key: 'allowRefund',value: '改签' },
            { key: 'endose',value: '退' },
            { key: 'sell',value: '折扣卡' },
            { key: 'snack',value: '小吃' }
         ];
         for(var i = 0;i < card.length;i ++) {
            if(card[i].key === key) {
               return card[i].value
            }
         }
         return '' // 都没有匹配到,给一个默认值 '空字符串
      }
   }
}
```

## 局部过滤器实现标签颜色变化

```css
// 定义两个样式

.or{color: red;}
.op{color: orange;}
```

```html
<div :class="key | classCard"></div>
```

```js
export default {
   name: 'CiList',
   data() {
      return {
         cinemaList: []
      }
   },
   filters: { // 局部过滤器
      formatCard(key) {
         var card = [
            { key: 'allowRefund',value: '改签' },
            { key: 'endose',value: '退' },
            { key: 'sell',value: '折扣卡' },
            { key: 'snack',value: '小吃' }
         ];
         for(var i = 0;i < card.length;i ++) {
            if(card[i].key === key) {
               return card[i].value
            }
         }
         return '' 
      },
      classCard(key) {
      	 var card = [
            { key: 'allowRefund',value: 'or' },
            { key: 'endose',value: 'op' },
            { key: 'sell',value: 'or' },
            { key: 'snack',value: 'op' }
         ];
         for(var i = 0;i < card.length;i ++) {
            if(card[i].key === key) {
               return card[i].value
            }
         }
         return '' // 都没有匹配到,给一个默认值 '空字符串
      }
   }
}
```



# 18.提交阶段代码到`git`

```shell
git status 
git add .
git commit -m "..."
git checkout dev // 切换到开发分支
git merge setData --no-ff // 将setData合并到开发分支dev上
:q // 需要我们填写注释,不想写的话就退出
git log // 查看日志
^c 退出
git push ck dev 
git branch // 查看分支
git branch -d setData // 删除分支
```



# 19.创建`git`分支

```shell
git checkout -b getCity
```



# 20.移动端的两个问题

## 点击目标跳转到详情页

当我们滑动页面的时候，不触发事件。

当我们点击目标区域时，才触发事件。

### `tap`点击事件

`tap`事件的特点就是当且仅当我们点击的时候触发事件。

`tap`事件在原生`js`中是不存在的，所以我们要想使用必须用`touchstart`、`touchmove`、`touchend`模拟。

当然第三方插件为我们提供了，`zepto`、`vue-touch`、`better-scroll`都为我们解决了这个问题。

还有一个问题就是我们的页面在进行上下滚动的时候，非常的生硬，一点都不流畅。

同样的第三方插件也为我们提供了：`iscroll`、`swiper`、`better-scroll`。

所以我们决定使用`better-scroll`去使用在我们的项目中。

## `better-scroll`

```shell
npm install better-scroll --save
```

 要求我们上下滚动的内容区域一定要长于外层父级的容器区域，并且一定要保证上下滚动的区域全部加载完毕之后，再去调用`better=scroll`。

回到`Nowplaying`页面：

```js
import BScroll from 'better-scroll'
```

```js
// 1.引入better-scroll组件
import BScroll from 'better-scroll'
export default {
   name: 'NowPlaying',
   data() {
      return {
         movieList: []
      }
   },
   mounted() {
      this.axios.get('/api/movieOnInfoList?cityId=476').then((res) => {
         var msg = res.data.msg;
         if(msg === 'ok') {
            this.movieList = res.data.data.movieList;
            // 2.$nextTick()方法能够保证我们的数据完全被渲染到页面上,并且页面已经完整呈现出来了
            this.$nextTick(() => {
               // 3.在$nextTick()里面new BScroll方法就能成功
               new BScroll( this.$refs.movie_body, {})
            })
         }
      })
   }
}
```

```html
<!-- 4.给父级容器起一个ref属性,用以找到他 -->
<div class="movie_body" ref="movie_body"></div>
```

---

关于点击跳转到详情页：

```html
<!-- 1.给目标区域,添加tap事件 -->
<div class="pic_show" @tap="handleToDetail"></div>
```

```js
new BScroll( this.$refs.movie_body, {
  // 2.在better-scroll中的第二个参数中进行配置,开启tap功能
  tap: true
})
```

```js
methods: {
  // 3.测试
  handleToDetail() {
     console.log(111)
  }
}
```

---

### 下拉刷新

通过配置`probeType`属性实现滑动截流效果。

```js
this.$nextTick(() => {
   var scroll = new BScroll( this.$refs.movie_body, {
      tap: true,
      probeType: 1 // 当值 = 1时,滚动的时候会派发scroll事件且会截流
   })
   scroll.on('scroll', () => {
      console.log('scroll')
   })
})
```

```js
mounted() {
  this.axios.get('/api/movieOnInfoList?cityId=476').then((res) => {
     var msg = res.data.msg;
     if(msg === 'ok') {
        this.movieList = res.data.data.movieList;
        this.$nextTick(() => {
           var scroll = new BScroll( this.$refs.movie_body, {
              tap: true,
              probeType: 1 // = 1时,滚动的时候会派发scroll事件,会截流
           })
	
           // 当拖拽开始时候触发的事件, 参数pos用以检测当前上下拖拽的位置变化
           scroll.on('scroll', (pos) => {
              if(pos.y > 30) {
                 this.pullDownMessage = '正在更新中'
              }
              // console.log('scroll')
           })
           
           // scroll touchEnd事件,当拖拽结束之后触发的事件方法
           scroll.on('touchEnd', (pos) => {
              if(pos.y > 30) {
                 this.axios.get('/api/movieOnInfoList?cityId=10').then(res => {
                    var msg = res.data.msg;
                    if(msg === 'ok') {
                       this.pullDownMessage = '更新成功';
                       // 让文字先出来,让数据延迟1秒再显示出来
                       setTimeout(() => {
                          this.movieList = res.data.data.movieList;
                          this.pullDownMessage = ''
                       }, 1000)
                    }
                 })
              }
              // console.log('touchend')
           })
        })
     }
  })
},
```

```js
data() {
  return {
     movieList: [],
     pullDownMessage: '' // 添加一个下拉拖拽的信息
  }
},
```

```html
<li>{{ pullDownMessage }}</li>
```

## 封装`better-scroll`

```vue
// better-scroll 将其注册为全局组件

<template>
  <div class="wrapper" ref="wrapper">
     <slot></slot>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
   name: 'Scroller',
   mounted() {
      var scroll = new BScroll(this.$refs.wrapper, {
         tap: true,
         probeType: 1
      })
   }
}
</script>

<style lang="scss" scoped>
   .wrapper{
      height: 100%;
   }
</style>
```

```js
// main.js 入口模块中注册成为全局组件scroller

import Scroller from '@/components/Scoller'
Vue.component('Scroller', Scroller);
```

**组件中使用**：

```html
1.使用标签<Scroller>将滚动区域包裹起来
2.注册全局组件完成之后,只需将上下滚动内容部分用<Scroller>标签包裹起来即可
    
<Scroller>
 <ul>
   <!-- <li v-for="(item,index) in 10" :key="item + index">
      <div class="pic_show">
         <img src="@/img/movie_1.jpg" alt=""> 
      </div>
      <div class="info_list">
         <h2>斗破苍穹</h2>
         <p>主演: 萧炎,萧熏儿,药老</p>
         <p>今天33家影院放映600场</p>
      </div>
      <div class="btn_mall">
         购票
      </div>
   </li> -->
   <li class="pullDown">{{ pullDownMessage }}</li>
   <li v-for="item in movieList" :key="item.id">
      <!-- 一.添加tap事件 -->
      <div class="pic_show" @tap="handleToDetail">
         <img :src="item.img | setWH('128.180')" alt=""> 
      </div>
      <div class="info_list">
         <h2>{{ item.nm }} <img v-if="item.version" src="@/assets/maxs.png" alt=""></h2> 
         <p>观众评 <span class="grade">{{ item.sc }}</span></p>
         <p>主演：{{ item.star }}</p>
         <p>{{ item.showInfo }}</p>
      </div>
      <div class="btn_mall">
         购票
      </div>
   </li>
 </ul>
</Scroller>
```

```js
2.注释掉之前的局部引用better-scroll组件方式

// 1.引入better-scroll组件
// import BScroll from 'better-scroll'
export default {
   name: 'NowPlaying',
   data() {
      return {
         movieList: [],
         pullDownMessage: ''
      }
   },
   mounted() {
      this.axios.get('/api/movieOnInfoList?cityId=476').then((res) => {
         var msg = res.data.msg;
         if(msg === 'ok') {
            this.movieList = res.data.data.movieList;
            // 2.$nextTick()方法能够保证我们的数据完全被渲染到页面上,并且页面已经完整呈现出来了
            // this.$nextTick(() => {
            //    // 3.在$nextTick()里面new BScroll方法就能成功
            //    var scroll = new BScroll( this.$refs.movie_body, {
            //       tap: true,
            //       probeType: 1 // = 1时,滚动的时候会派发scroll事件,会截流
            //    })

            //    scroll.on('scroll', (pos) => {
            //       if(pos.y > 30) {
            //          this.pullDownMessage = '正在更新中'
            //       }
            //       // console.log('scroll')
            //    })
            //    scroll.on('touchEnd', (pos) => {
            //       if(pos.y > 30) {
            //          this.axios.get('/api/movieOnInfoList?cityId=10').then(res => {
            //             var msg = res.data.msg;
            //             if(msg === 'ok') {
            //                this.pullDownMessage = '更新成功';
            //                setTimeout(() => {
            //                   this.movieList = res.data.data.movieList;
            //                   this.pullDownMessage = ''
            //                }, 1000)
            //             }
            //          })
            //       }
            //       // console.log('touchend')
            //    })
            // })
         }
      })
   },
   methods: {
      handleToDetail() {
         console.log(111)
      }
   }
}
```

### 关于正在更新和更新完成，使用父子通信实现

```js
// better-scroll 组件中,通过props创建两个属性,定义两个方法,并且将两个方法回调出去给外界

import BScroll from 'better-scroll'
export default {
   name: 'Scroller',
   // 1.创建两个属性
   props: {
      handleToScroll: {
         type: Function,
         default: function() {}
      },
      handleToTouchEnd: {
         type: Function,
         default: function() {}
      }
   },
   mounted() {
      var scroll = new BScroll(this.$refs.wrapper, {
         tap: true,
         probeType: 1
      })

      // 2.调用上述定义的两个方法
      // 将上述两个方法回调出去
      scroll.on('scroll', (pos) => {
         this.handleToScroll(pos)
      });

      scroll.on('touchEnd', (pos) => {
         this.handleToTouchEnd(pos)
      })
   }
}
```

```js
// 在父组件中使用上面回调出来的方法

methods: {
      handleToDetail() {
         console.log(111)
      },
      // 调用封装好的组件中传递出来的函数方法
      handleToScroll(pos) {
         if(pos.y > 30) {
            this.pullDownMessage = '正在更新中'
         }
      },
      handleToTouchEnd(pos) {
         if(pos.y > 30) {
            this.axios.get('/api/movieOnInfoList?cityId=10').then(res => {
               var msg = res.data.msg;
               if(msg === 'ok') {
                  this.pullDownMessage = '更新成功';
                  setTimeout(() => {
                     this.movieList = res.data.data.movieList;
                     this.pullDownMessage = ''
                  }, 1000)
               }
            })
         }
      }
   }
```

```html
// 最后一步,在标签头部绑定属性
<Scroller :handleToScroll = "handleToScroll" :handleToTouchEnd = "handleToTouchEnd"></Scroller>
```

### `better-scroller`中的点击跳转配置

**设置**：

```js
// 在better-scroller组件中

mounted() {
  var scroll = new BScroll(this.$refs.wrapper, {
     tap: true,
     probeType: 1
  })

  // 1.vue是面向对象编程,将scroll赋予全局作用域链
  this.scroll = scroll;
 
  scroll.on('scroll', (pos) => {
     this.handleToScroll(pos)
  });

  scroll.on('touchEnd', (pos) => {
     this.handleToTouchEnd(pos)
  })
},
methods: {
  // 2.定义方法
  toScrollTop(y) { // 3.只在y轴上进行跳转
     this.scroll.scrollTo(0,y)
  }
}
```

**使用**：

```html
<Scroller ref="city_List"></Scroller>
```

```js
handleToIndex(index) {
	var h2 = this.$refs.city_sort.getElementsByTagName('h2')
	// this.$refs.city_sort.parentNode.scrollTop = h2[index].offsetTop;
	// 将一个refs属性加到组件上,那么他就拿到当前的组件对象,然后我们.出对象的方法,记得加上负号
	this.$refs.city_List.toScrollTop(-h2[index].offsetTop); 
}
```



# 21.封装`loading`组件

我们采用的是`CSS3`去做一个`loading`的动画实现，不要使用图片，因为图片本身也是需要发起请求的。

新建一个`loading`组件。

[loading动画网址](https://www.runoob.com/w3cnote/free-html5-css3-loaders-preloaders.html)

```vue
<template>
  <figure>
    <div class="dot white"></div>
    <div class="dot"></div>
    <div class="dot"></div>
    <div class="dot"></div>
    <div class="dot"></div>
  </figure>
</template>

<script>
export default {
  name: "loading"
};
</script>

<style scoped>
body {
  background: #222;
}
figure {
  position: absolute;
  margin: auto;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  width: 6.25em;
  height: 6.25em;
  animation: rotate 2.4s linear infinite;
}
.white {
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: white;
  animation: flash 2.4s linear infinite;
  opacity: 0;
}
.dot {
  position: absolute;
  margin: auto;
  width: 2.4em;
  height: 2.4em;
  border-radius: 100%;
  transition: all 1s ease;
}
.dot:nth-child(2) {
  top: 0;
  bottom: 0;
  left: 0;
  background: #ff4444;
  animation: dotsY 2.4s linear infinite;
}
.dot:nth-child(3) {
  left: 0;
  right: 0;
  top: 0;
  background: #ffbb33;
  animation: dotsX 2.4s linear infinite;
}
.dot:nth-child(4) {
  top: 0;
  bottom: 0;
  right: 0;
  background: #99cc00;
  animation: dotsY 2.4s linear infinite;
}
.dot:nth-child(5) {
  left: 0;
  right: 0;
  bottom: 0;
  background: #33b5e5;
  animation: dotsX 2.4s linear infinite;
}

@keyframes rotate {
  0% {
    transform: rotate(0);
  }
  10% {
    width: 6.25em;
    height: 6.25em;
  }
  66% {
    width: 2.4em;
    height: 2.4em;
  }
  100% {
    transform: rotate(360deg);
    width: 6.25em;
    height: 6.25em;
  }
}

@keyframes dotsY {
  66% {
    opacity: 0.1;
    width: 2.4em;
  }
  77% {
    opacity: 1;
    width: 0;
  }
}
@keyframes dotsX {
  66% {
    opacity: 0.1;
    height: 2.4em;
  }
  77% {
    opacity: 1;
    height: 0;
  }
}

@keyframes flash {
  33% {
    opacity: 0;
    border-radius: 0%;
  }
  55% {
    opacity: 0.6;
    border-radius: 100%;
  }
  66% {
    opacity: 0;
  }
}
</style>
```

将其注册成为全局组件。

```js
// 来到入口函数模块

import Loading from '@/components/Loading'
Vue.component('Loading', Loading)
```

```vue
// 使用

// 1.
<Loading v-if="flag" />
// 2.
<Scroller v-else>
	...
</Scroller>
<script>
	export default {
        data() {
            return {
                // 3.
                flag: true
            }
        },
        mounted() {
            if(msg === 'ok') {
                this.movieList = res.data.data.movieList;
                // 4.当数据请求成功之后,让flag变为false
                this.flag = false
            }
        }
    }
</script>
```



# 22.城市组件中的状态管理和本地存储

## 本地存储

将请求过来的数据进行本地存储，以实现性能优化的目的。

```js
// 在City组件中

mounted() {
  // 2.本地存储完成之后,我们从本地存储中获取数据,不再发起新的请求
  var cityList = window.localStorage.getItem('cityList');
  var hotList = window.localStorage.getItem('hotList');
  
  // 3.判断一下是否已经将数据全部都存储到本地
  if(cityList && hotList) {
     // 4.使用JSON.parse方法,将其解析回来
     this.cityList = JSON.parse(cityList);
     this.hotList = JSON.parse(hotList);
     this.flag = false // 注意: 改为本地存储之后的loading动画也要改变一下
  }else {
     this.axios.get('/api/cityList').then((res) => {
        let msg = res.data.msg
        if(msg === 'ok') {
           // 0.获取到所有数据
           let cities = res.data.data.cities 
           var { cityList,hotList } = this.formatCityList(cities)
           this.cityList = cityList;
           this.hotList = hotList;
           this.flag = false
		
           // 1.当数据请求过来之后,使用H5的localStorage进行本地存储
           // 第一个参数是key值,第二个参数是要存储的内容
           // 因为本地存储的类型是字符串类型,所以使用stringify方法将其转为字符串类型的数组结构
           window.localStorage.setItem('cityList', JSON.stringify(cityList));
           window.localStorage.setItem('hotList', JSON.stringify(hotList))
        }
     })
  }
}
```

![本地存储](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\本地存储.jpg)

## 状态管理

```js
// 入口模块 main.js

import store from './store' // 1.引入
new Vue({
   store // 2.注册
})
```

```js
import Vue from 'vue'
import Vuex from 'vuex'
import city from './city'

Vue.use(Vuex)

const store = new Vuex.Store({
   state: {

   },
   mutations: {

   },
   actions: {

   },
   getters: {

   },
   modules: {
   	city: city
   }
})

export default store
```

遵循模块化开发思想，新建`city`文件夹，里面新建`index.js`：

```js
const state = {
   nm: '北京',
   id: 1
};

const actions = {

};

const mutations = {
   // 定义一个修改state状态的方法,用大写用以区分
   CITY_INFO(state,payload) {
      state.nm = payload.nm;
      state.id = payload.id;
   }
}

// 对外的接口
export default {
   namespaced: true,
   state,
   actions,
   mutations
}
```

使用：

```vue
// 回到Movie组件下
// 注意一下这里的一连串的点操作
<span>{{ $store.state.city.nm }}</span>
```

![vuex](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\vuex.jpg)

## 修改状态管理中的`nm`和`id`

通过添加点击操作，分别给热门城市和分类中的`li`条件点击事件，并将`nm`和`id`传递过去：

```html
<div class="city_hot">
  <h2>热门城市</h2>
  <ul class="clearfix">
     <li v-for="item in hotList" :key="item.id" @tap="handleToCity(item.nm,item.id)">{{ item.nm }}</li>
  </ul>
</div>
<div class="city_sort" ref="city_sort">
  <div v-for="item in cityList" :key="item.index">
     <h2>{{ item.index }}</h2>
     <ul>
        <li v-for="itemList in item.list" :key="itemList.id" @tap="handleToCity(itemList.nm,itemList.id)">{{ itemList.nm }}</li>
     </ul>
  </div>
</div>
```

```js
methods: {
    // 添加对应的点击事件
	handleToCity(nm,id) {
        // 修改状态管理,不是异步操作,所以可以直接触发commit
		this.$store.commit('city/CITY_INFO', {nm,id})
        // 当我们完成刷新之后，记录下上次的城市定位,还是使用本地路由
        // 当我们存储好之后,什么时候去取它呢?看下面
        window.localStorage.setItem('nowNm', nm);
        window.localStorage.setItem('nowId', id);
        // 当我们修改完毕之后,要让其跳回到正在热映组件
        this.$router.push('/movie/noePlaying');
	}
}
```

```js
// 想一下我们是在哪里将其赋值为每次刷新都是北京的,回到状态管理的City组件中
// 按照下面设置完成之后,我们每次刷新就会从本地存储中去取值了

const state = {
    nm: window.localStorage.getItem('nowNm') || '北京',
    id: window.localStorage.getItem('nowId') || 1
}
```

![vuex状态管理](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\vuex状态管理.jpg)



# 23.联动数据

以上我们已经实现城市的切换了，但是页面中显示的城市信息还是没有相对应的。

## `activated`生命周期

是一个函数，`keep-alive`组件激活的时候调用，该钩子在服务器端渲染期间不被调用。

将`activated`生命周期去替代`mounted`去发起网络请求：

```js
// 将mounted字段改为activated
activated() {
  this.axios.get('/api/movieOnInfoList?cityId=476').then((res) => {
     console.log(1) // 这样在每次进行组件切换的时候,就会重新触发里面的代码
     var msg = res.data.msg;
     if(msg === 'ok') {
        this.movieList = res.data.data.movieList;
        this.flag = false 
     }
  })
}
```

## 需求：当且仅当我们从城市组件切换到正在热映组件的时候，需要重新发起网络请求数据，否则直接走缓存

```js
// 在正在热映组件中的data中定义一个数值
data() {
    return {
        prevCityId: -1 // 是个负数
    }
}
activated() {
    var cityId = this.$store.state.city.id // 1
    // 如果两者id是不相等的,则直接走下面,如果相等,则return出去
    if( this.preCityId === cityId) {return;}
    // 设置加载动画
    this.isLoading = true;
    // 动态拼接id
    this.axios.get('/api/movieOnInfoList?cityId=' + cityId).then(res => {
        var msg = res.data.msg;
        if(msg === 'ok') {
            // 数据请求成功之后进行赋值
            this.preCityId = cityId
        }
    })
}
```

在即将到来组件页面也是同样去修改。

注意在搜索页面中，我们直接获取`Id`进行拼接：

```js
watch() {
    message(newVal) {
        // 1.直接拿到id
        var cityId = this.$store.state.city.id
        // 2.进行Id动态拼接
        this.axios.get('/api/searchList?cityId=' + cityId + '&kw=' + newVal,{
            ...
        })
    }
}
```



# 24.定位

## 弹窗

在`components`文件夹下新建`JS`文件夹，在`JS`文件夹里新建`index.js`文件，新建`MessageBox`文件夹，在`MessageBox`文件夹里新建`index.vue`文件。

```vue
<template>
   <div class="messageBox">
      <h2>定位</h2>
      <p>北京</p>
      <div>
         <span>取消</span>
         <span>切换定位</span>
      </div>
   </div>
</template>

<script>
export default {
   name: 'messageBox'
}
</script>

<style lang="scss" scoped>
   .messageBox{
      width: 200px;
      height: 140px;
      border: 1px solid #ccc;
      border-radius: 4px;
      background-color: white;
      box-shadow: 3px 3px 3px 3px #ccc;
      position: absolute;
      left: 50%;
      top: 50%;
      margin: -60px 0 0 -100px;
      h2{
         text-align: center;
         line-height: 40px;
         font-size: 18px;
      }
      p{
         text-align: center;
         line-height: 40px;
      }
      div{
         position: absolute;
         bottom: 0;
         width: 100%;
         border-top: 1px solid #ccc;
         display: flex;
         // justify-content: space-between;
         span{
            flex: 1;
            text-align: center;
            line-height: 30px;
            border-right: 1px solid #ccc;
         }
         span:nth-child(2) {
            border: none;
         }
      }
   }
</style>
```

回到`Movie`页面进行引用：

```html
<MessageBox />
```

```js
import MessageBox from '@/components/JS/MessageBox' // 引入
export default {
    name: 'Movie',
    components: {
        MessageBox // 注册
    }
}
```

![弹窗组件](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\弹窗组件.jpg)

```js
// 上述的弹窗组件模板代码,如下:

import Vue from 'vue'
import MessageBox from './MessageBox'

// 暴露出一个接口,考虑到接口可能有很多,我们使用了闭包去解决
export var messageBox = (function() {
   // 给我们定制的模板一些默认参数
   // 这里default是保留字,我们再使用的时候还是将其换个名字把
   var default = {
      title: '',
      content: '',
      cancel: '',
      ok: '',
      handleCancel: null,
      handleOk: null
   }

   var MyComponent = Vue.extend(MessageBox)

   // 调用之后返回的函数出去
   // 将配置参数opts传递进去
   return function(opts) {
      // 将外面的配置参数进行循环遍历并且覆盖
      for(var attr in opts) {
         default[attr] = opts[attr];
      }
      
      var vm = new MyComponent({
         el: document.createElement('div'), // 有el字段就会渲染其,而不会再生成template
         data: {
            title: default.title,
            content: default.content,
            cancel: default.cancel,
            ok: default.ok
         },
         methods: {
            handleCancel() {
               default.handleCancel && default.handleCancel.call(this); // 要使得handleCancel拿到vm对象,所以我们使用call改变this指向
                // 点击按钮,删除弹窗
               document.body.removeChild(vm.$el);
            },
            handleOk() {
               default.handleOk && default.handleOk.call(this);
               document.body.removeChild(vm.$el);
            }
         }
      });
      // 添加到的位置
      document.body.appendChild(vm.$el)
   }
})()
```

`MessageBox`中的`index.js`，修改内容如下：

```html
<template>
   <div class="messageBox">
      <!-- 将对应的值填进去 -->
      <h2>{{ title }}</h2>
      <p>{{ content }}</p>
      <div>
         <span @touchstart="handleCancel">{{ cancel }}</span>
         <span @touchstart="handleOk">{{ ok }}</span>
      </div>
   </div>
</template>
```

回到`Movie`页面进行测试一下：

```js
import {messageBox} from '@/components/JS'
mounted() {
    messageBox({
        title: '定位',
        content: '兰溪',
        cancel: '取消',
        ok: '切换定位',
        handleCancel() {
            
        },
        handleOk() {
            
        }
    })
}
```

最后一个问题，我们什么时候出现弹窗，使用`ajax`发起请求获取到数据之后！

```js
// 优化上述mounted中的代码

mounted() {
    setTimeout(() => { // 数据请求回来之后,延迟几秒再弹出城市定位
        this.ajax.get('/api/getLocation').then(res => {
            var msg = res.data.msg;
            if(msg === 'ok') {
                var nm = res.data.data.nm;
                var id = res.data.data.id;
                // 判断一下当前是否已经定位在当前的城市,如果是的话,就不再进行弹窗了
                if(this.$store.state.city.id == id) {return;}
                messageBox({
                    title: '定位',
                    content: nm,
                    cancel: '取消',
                    ok: '切换定位',
                    handleCancel() {
                        // 取消直接关闭就行了,不用加什么操作
						// console.log(1)
                    },
                    handleOk() {
                        // 点击切换城市之后,从缓存中拿取数据
						window.localStorage.setItem('nowNm',nm);
                        window.localStorage.setItem('nowId',id);
                        // 当切换的时候,我们重新加载一下我们的页面
                        window.localtion.reload()
                    }
                })
            }
        })
    }, 3000)
}
```

## 传递阶段代码到`git`

```shell
git status  
git add .
git commit -m "..."
git checkout dev  
git merge getCity --no-ff // 融合
git log // 查看日志
git push ck dev
git branch
git branch -d getCity
git status
```



# 25.详情页

```shell
git checkout -b detailPage // 创建一个新分支
```

我们在`views`文件夹中的`Movies`文件夹中再新建一个文件`detail.vue`。

```vue
<template>
  <div id="detailContainer">
		<!-- <header id="header">
			<i class="iconfont icon-right"></i><h1>影片详情</h1>
		</header> -->

      <!-- 3.使用Header组件去改造 -->
      <Header title="影片详情">
         <i class="iconfont icon-right"></i>
      </Header>
		<div id="content" class="contentDetail">
			<div class="detail_list">
				<div class="detail_list_bg"></div>
				<div class="detail_list_filter"></div>
				<div class="detail_list_content">
					<div class="detail_list_img">
						<img src="images/movie_1.jpg" alt="">
					</div>
					<div class="detail_list_info">
						<h2>无名之辈</h2>
						<p>A Cool Fish</p>
						<p>9.2</p>
						<p>剧情,喜剧,犯罪</p>
						<p>中国大陆 / 108分钟</p>
						<p>2018-11-16大陆上映</p>
					</div>
				</div>
			</div>
			<div class="detail_intro">
				<p>在一座山间小城中，一对低配劫匪、一个落魄的泼皮保安、一个身体残疾却性格彪悍的残毒舌女以及一系列生活在社会不同轨迹上的小人物，在一个貌似平常的日子里，因为一把丢失的老枪和一桩当天发生在城中的乌龙劫案，从而被阴差阳错地拧到一起，发生的一幕幕令人啼笑皆非的荒诞喜剧。</p>
			</div>
			<div class="detail_player swiper-container">
				<ul class="swiper-wrapper">
					<li class="swiper-slide">
						<div>
							<img src="images/person_1.webp" alt="">
						</div>
						<p>陈建斌</p>
						<p>马先勇</p>
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
// 1.引入Header组件
import Header from '@/components/Header'
export default {
   name: 'Detail',
   components: {
      Header // 2.注册Header组件
   }
}
</script>

<style lang="scss" scoped>
   #detailContainer{
      position: absolute;
      left: 0;
      top: 0;
      z-index: 9999;
      width: 100%;
      min-height: 100%;
      background-color: white;
      .contentDetail{ 
         display: block; 
         margin-bottom: 0;
         .detail_list{ 
            height: 200px; 
            width: 100%; 
            position: relative; 
            overflow: hidden;
            .detail_list_bg{ 
               width: 100%; 
               height: 100%; 
               background: url(../images/movie_1.jpg) 0 40%; 
               filter: blur(20px); 
               background-size: cover; 
               position: absolute; 
               left: 0; 
               top: 0;
            }
            .detail_list_filter{ 
               width: 100%; 
               height: 100%; 
               position: absolute;
               background-color: #40454d;
               opacity: .55; 
               position: absolute; 
               left: 0; 
               top: 0; 
               z-index: 1;
            }
            .detail_list_content{ 
               display: flex; 
               width: 100%; 
               height: 100%; 
               position: absolute; 
               left: 0; 
               top: 0; 
               z-index: 2;
               .detail_list_img{ 
                  width: 108px; 
                  height: 150px; 
                  border: 1px solid #f0f2f3; 
                  margin: 20px;
                  img{
                     width: 100%; 
                     height: 100%;
                  }
               }
               .detail_list_info{ 
                  margin-top: 20px;
                  h2{ 
                     font-size: 20px; 
                     color: white; 
                     font-weight: normal; 
                     line-height: 40px;
                  }
                  p{
                     color: white; 
                     line-height: 20px; 
                     font-size: 14px; 
                     color:#ccc;
                  }
               }
            }
         }
      }
   }
</style>
```

改造`Header`组件：

```html
// 加上一个slot标签插槽,就是上述代码中的返回图标<i></i>替换位
<header id="header">
  <slot></slot><h1>{{ title }}</h1>
</header>
```

## 命名视图处理路由问题

![命名视图](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\命名视图.jpg)

我们需要在`Movie`页，点击具体电影，跳出详情页：

```html
<router-view name="detail" />
```

回到路由配置文件中：

```js
{path: '/movie',component: Movie,
    children: [
        {path: 'city',component: () => import('@/components/City')},
        // 路由中这样配
        { 
            path: 'detail', // 此处的path对应路由中的name属性值
            components: { // 注意是components
                detail: () => import('@/views/Movie/detail')
            }
        },
        {path: 'search',component: () => import('@/components/Search')},
        {path: '/movie',redirect: '/movie/nowPlaying'}
    ]
},
```

## 给详情页后退图标添加点击事件

这样貌似不行：

```html
<i class="iconfont icon-right" @touchstart="handleToBack"></i>
methods: {
	handleToBack() {
            this.$router.back()
	}
}
```

回到正在热映组件中：

```html
// 绑定点击事件,并将id传递过去
<div class="pic_show" @tap="handleToDetail(item.id)">
 <img :src="item.img | setWH('128.180')" alt=""> 
</div>
```

```js
methods: {
    handleToDetail(movieId) {
    	console.log(movieId)
    },
}
```

还需要配置一下动态路由：

依旧是上面的路由中，加上`:movieId`即可：

```js
{path: '/movie',component: Movie,
    children: [
        {path: 'city',component: () => import('@/components/City')},
        // 路由中这样配
        { 
            path: 'detail/:movieId', // 此处的path对应路由中的name属性值
            components: { // 注意是components
                detail: () => import('@/views/Movie/detail')
            }
        },
        {path: 'search',component: () => import('@/components/Search')},
        {path: '/movie',redirect: '/movie/nowPlaying'}
    ]
},
```

```js
methods: {
    handleToDetail(movieId) {
        // console.log(movieId)
        this.$router.push('/movie/detail/' + movieId)
    },
}
```

以上便是完成了详情页的跳转和返回操作。

## 最后将传递过来的详情页根据id进行网络请求拿到数据

因为我们使用的是命名视图去实现动态路由，所以使用字段`props`去实现：

```js
{
    path: 'detail/movieId',
    components: {
        detail: () => import('@/views/Movie/detail')
    },
    props: {
        detail: true
    }
}
```

回到详情页，直接使用`props`字段拿到`id`：

```js
export default {
    name: 'Detail',
    props: ['movieId'],
    mounted() {
        console.log(this.movieId)
    }
}
```

## 当我们在详情页点击回退图标的时候，希望有一个动画效果

本来是可以使用`transition`去包裹实现动画的，因为与`ios`向右滑动相冲突，所以我们决定自己写一个动画。

```html
<div id="detailContainer" class="slide-enter-active"></div>
```

```css
.slide-enter-active{
  animation: .3s slideMove;
}
@keyframes slideMove{
  0%{
     transform: translateX(100%);
  }
  100%{
     transform: translate(0);
  }
}
```

`bug`：在我们进行动画过渡的时候，发现过渡过程中页面上的内容消失了。因为页面上只有一个`ul`，而且页面上有两个`router-view`，所以当展示一个的时候，另一个自然就断开连接了。解决办法就是给一个`default`字段：

```js
{
    path: 'detail/movieId',
    components: {
        default: () => import('@/components/NowPlaying'), // 加上这个default字段,设置一下就行了
        detail: () => import('@/views/Movie/detail')
    },
    props: {
        detail: true
    }
}
```

如果有多个页面都是需要使用上述动画的，那么我们就将其再多写一份路由配置一下。

 ## 提交到`git `

```shell
git add .
git commit -m "..."
git checkout dev
git merge detailPage --no-ff
git push ck dev
git branch -d detailPage
```



# 26.当我们第一次加载资源的时候，显示一个整体的加载动画

在`index.html`文件中：

```html
<style>
    #app{
        display: flex;
        height: 100%;
    }
    #app div{
        margin: auto;
    }
</style>
<div id="app">
    <div>
        mini_cinema正在加载中...
    </div>
</div>
```



# 27.打包

## 关于路由访问的起始路径

```js
// 路由中添加一个字段
const router = new VueRouter({
    routes: [
        ...
    ],
    mode: 'history',
    base: 'mini' // 这里添加字段表示以后我们的项目路由都是以mini开头的
})
```

![base路由设置](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img\base路由设置.jpg)

## 关于静态资源的路径

回到`vue.config.js`中，添加`publicPath`字段：

```js
// 反向代理的配置,解决跨域问题

module.exports = {
	publicPath: '/miaomiao', 
    devServer: {
       proxy: {
           '/api': {
               target: 'http://39.97.33.178',
               changeOrigin: true
           }
       }
    }
}
```

## 打包

```shell
npm run build
```
