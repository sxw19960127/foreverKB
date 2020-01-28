# 微信小程序

## 起步

```
起步: 微信小程序官网 -- 最下面 -- 接入流程
注册完之后可在微信小程序官网进行登陆;
在小程序发布流程里面可以添加协作开发者以及体验人员;
获取AppId -- 在登陆个人网页之后 -- 左侧开发选项 -- 开发设置
在微信开发者工具顶部一栏 - 普通编译 - 添加编译模式 - 设置启动页面 - (解决每次都需要手动看效果)
全局js文件初始化 -- App
局部js文件初始化 -- Page
```

```
<block></block>标签,仅作包裹使用,在代码段中不显示结构,可以在对象,数组遍历的时候做包裹使用;
使用遍历时候绑定的key值,对象的话绑定每一项独一无二的属性名,数组的话可以这样 wx:key="*this"
绑定点击事件 bindtap
所有手机屏幕都是750rpx
全局js -- 生命周期回调:
    onLaunch 监听小程序初始化
    onShow 监听小程序启动或切前台
    onHide 监听小程序切后台
```

## `app.json`

```js
// 关于首页底部的设置
"tabBar": {
	"list": [{
		"pagePath": "pages/index/index",
		"text": "首页"
	},
	{
		"pagePath": "pages/index2/index2",
        "text": "我的"
	}]
}
```

## `setData`

```js
// 数据操作的实现需要放到setData中去执行 
click() {
	this.setData({
		movies: this.data.movies.reverse()
	})
}
```

## 条件渲染

```html
<view wx:if="{{ show === 1 }}">123</view>
<view wx:elif="{{ show === 2 }}">234</view>
<view wx:else>345</view>
// 注意上述根据show值进行显示和隐藏是将整个元素删除
<view hidden="true">123</view> // hidden元素的隐藏知识display: none;
```

## 生命周期

## 视图层

## 逻辑层

## 路由

```js
// 给定一个按钮绑定点击事件,在事件函数中
toHome() {
	wx.navigateTo({ // 支持后退,只是在栈的上面放上了新的页面
		url: "../pages/home/home"
	})
}
toHome() {
	wx.redirectTo({ // 不可以后退,是在栈中直接替换掉了之前的页面
		url: "../pages/home/home"
	})
}
// 上面两个跳转只能跳转到非tabBar中的页面,tabBar就是页面底部的切换,而switchTab只能打开tabBar页面
// 回退两个页面:
wx.navigateBack({
	delta: 2 // 回退页面数量设置
})
```

## 页面跳转之间传递的参数

```js
// 父页面在onLoad 
function(options) {
	// 以对象的形式返回
	// 这里的options就是子页面传递过来的参数,子页面怎么传递呢?
	// 通过在跳转的url最后 ?name=sxw 进行传递传递
}
```

## 绑定点击事件

```
使用catchtap绑定的点击事件默认就没有冒泡行为
一般我们都是这么去写: catch:tap / bind:tap
捕获 capture-bind:tap
```

## 原生组件

```
a) swiper 轮播图
b) navigator 页面链接 -- 类似a标签
c) view视图容器
	属性: hover-class 指定按下去的样式类,当hover-class="none"时,没有点击效果;
		 hover-stop-propagation 指定是否阻止本节点的祖先节点出现点击态;
		 hover-start-time 按住后多久出现点击态,单位毫秒;
		 hover-stay-time 手指松开后点击保留时间,单位毫秒;
d) 滚动容器 scroll-view
	属性: 
             scroll-x
             scroll-y
             bindscroll 滚动时候触发的事件,有事件对象 e.detail获取所有信息
             		(this.data.scrollTop = e.detail.scrollTop 来实时监听滚动条滚动的数据)
             scroll-top 设置竖向滚动条位置,结合双段号数据绑定来一起使用
             scroll-left 设置横向滚动条位置 
             bindscrolltoupper 滚动到顶部/左部触发
             bindscrolltolower 滚动到底部/右部触发
             upper-threshold 距离顶部/左部默认50距离触发
             lower-threshold 距离底部/右部默认50距离触发
             scroll-into-view 指定跳转到哪个id对应的区域
             scroll-with-animation=“true" 在我们按钮切换过程中会有过渡效果实现
             enable-back-to-top 快速到顶部
             <scroll-view class="scroll"></scroll-view>
思路: scroll-view标签包裹三个区域,scroll-view固定高度,加上scroll-y后就可以往下拖动了;
```

## 自定义组件

```
json文件
{
	"component": true
}
js文件
Component({
	
})
```

## 使用自定义组件

```js
在需要使用的页面的json文件中
{
	"usingComponents": {
		"app-header": "/components/header/header" //然后再wxml里面就可以使用<app-header>标签
	}
}
自定义组件中最好都使用class选择器
```

## 页面向组件传值

```js
<app-header title="welcome"></app-header>
// 在自定义组件中:
Component({
	data: {},
    properties: {
    	title: String
    },
    methods: {
    	tapTitle() { //点击事件传值
    		console.log(this.properties.title)
    		this.triggerEvent("myEvent", {
    			title: this.properties.title
    		}) 
    		// 这里可以设置在页面中绑定属性值,当点击tapTitle时,页面会触发bindmyEvent事件,
    		// 然后回到页面的js文件中去定义bindmyEvent事件对应操作,实现相互影响，,
    		// 子组件向页面(父组件传值)
    	}
    }
})
```

## slot使用

```html
在父级
<app-h title="home">
	<text>我是副标题</text>
</app-h>
在组件中直接使用<slot></slot>去接收就行了
```

## 第三方组件的使用

```js
vant weapp
在github上下载到本地,将dist文件夹拷贝到自己的项目根目录中
在页面中引入组件库
{
	"usingComponents": {
		"vant-button": "../../dist/button/index" // 引用button按钮组件
	}
}
还可以使用npm去下载,但是存在版本问题需注意
```

## 项目

```
自己做了一个仿bilibili微信小程序的demo
做项目的流程和有关注意点:
1.初始化全局样式;
    page,view,image,swiper,swiper-item,navigator,video{
        box-sizing: border-sizing;
    }
2.初始化全局js文件,通过敲击APP实现快速创建;
3.单个页面中,整理wxml,wxss,js 还原一个干净的页面结构,其中js初始化通过敲击page快速创建;
4.修改首页的页面名称: index文件中的index.json
    {
        "navigationBarTitleText": "bilibili"
    }
5.将头部封装成一个可以复用的组件:
	a.创建组件: 回到根目录下新建一个components文件夹,在此文件夹中新建一个文件myTitle,
	然后选中文件夹,右键新建Components文件夹,名称为myTitle;
		{
	  		"component": true //json文件里配置
		}
	b.声明组件: 在需要使用的地方文件中的json下
        {
            "usingComponents": {
                "myTitle": "../../components/myTitle/myTitle"
            }
        }
	c.使用组件 <myTitle></myTitle>
6.关于首页导航实现需求的思路:
	数据是通过请求拿回来的,我们先在data中定义数组用以存放所有请求拿回来的数据,然后通过请求成功回调
函数将数据存放;在结构页通过循环遍历出数组,拿到每个元素{{ item.text }},至于如何实现点击元素实现选
中效果,我们先定义一个激活的类,通过绑定点击事件,在js中的处理函数,通过处理函数中的事件源以及setData
实现动态改变属性状态时对应的事件源点击的索引传递给一个参数,即现在这个参数就是实时保存的我们点击的
对象的索引了,回到结构目录中,通过 data-index="{{index}}" 用以保存循环出的每一项对应的索引,最后将
两个索引进行匹配,配对上了便激活类;
拿数据流程:
	1.js中的data里面定变量;videoList: []
	2.定函数
	getVideosList() {
	    let _this = this;
	    wx.request({
	      url: 'https://easy-mock.com/mock/5c1dfd98e8bfa547414a5278/bili/videosList',
	      success(res) {
	        console.log(res);
	        if (res.data.code === 0) { // ===0表示成功拿到数据
	          _this.setData({
	            videoList: res.data.data.videoList
	          })
	        }
	      }
	    })
	  }
	3.在onLoad生命周期函数中执行一下函数,当页面加载的时候会自动监听函数;
	  onLoad: function (options) {
	    this.getVideosList();
	  },
源码:
GitHub: https://github.com/sxw19960127/keep/tree/master/bilibili
```

