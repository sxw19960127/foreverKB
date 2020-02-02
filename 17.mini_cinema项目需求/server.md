```
普通会员 iu 1996
管理员 admin 123
```

# 1.服务端

## 全局安装`express`脚手架

```shell
npm install express-generator -g
```

## 快速创建项目

```shell
express -e miniserver
```

```shell
cd miniserver 
npm install // 下载脚手架所依赖的包
```

```shell
修改package.json文件:
start后面修改为 nodemon 去实时监听项目
```

```
通过 localhost:3000 访问项目
```

## 数据库：下载`mongoose`

```shell
npm install mongoose --save
```

[下载网址](https://www.mongodbmanager.com/download.php) 选 4.8.1.2 版本

## 使用数据库

首先在项目文件夹中新建一个`db`文件夹，用以存储数据库实例。

```shell
// 全局配置安装数据库
cmd -> 
mongod --dbpath=D:\projects\miniserver\db (后面跟上项目文件的db文件夹地址,就是上述所创建的)
```

这样就成功开启数据库了。

在项目中新建文件夹`untils`，里面新建`config.js`

```js
var mongoose = require('mongoose');

var Mongoose = {
	url : 'mongodb://localhost:27017/miaomiao', // 连接数据库的url
	connect(){ // 连接数据库的方法
		mongoose.connect(this.url , { useNewUrlParser: true, useUnifiedTopology: true }, (err)=>{
			if(err){
				console.log('数据库连接失败');
				return;
			}
			console.log('数据库连接成功');
		});
	}
};

module.exports = {
	Mongoose
};
```

回到`app.js`项目入口模块：

```js
var { Mongoose } = require('./untils/config.js') // 引入
Mongoose.connect(); // 注册,尽量放代码后面一点
```

## 创建数据库

可以使用命令行，也可以使用可视化工具（之前已经下载了）。

创建一个空数据库`Create New Database`

重启项目

![数据库](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\数据库.jpg)

## 后端接口一览

- 登录
- 注册 （短信发送验证码：阿里大鱼、云片）
- 退出
- 验证
- 获取当前用户信息（用户当前是什么状态）
- 找回密码

## 邮箱配置和发送验证码接口

在项目文件夹中新建文件夹`models`（里面新建`users.js`文件）、`controllers`（里面新建`users.js`文件）

回到`routes`文件夹，在`users.js`文件里面开始创建路由规则：

```js
routes > users.js

var express = require('express');
var usersController = require('../controllers/users.js');
var router = express.Router();

/* GET users listing. */
router.get('/', function(req, res, next) {
  res.send('respond with a resource');
});

router.post('/login' , usersController.login );
router.post('/register' , usersController.register );
router.get('/verify' , usersController.verify );
router.get('/logout' , usersController.logout );
router.get('/getUser' , usersController.getUser );
router.post('/findPassword' , usersController.findPassword );

module.exports = router;
```

回到`controllers`中的`users.js`文件：控制器接口

```js
var login = async (req,res,next)=>{

};

var register = async (req,res,next)=>{

};

var verify = async (req,res,next)=>{

};

var logout = async (req,res,next)=>{

};

var getUser = async (req,res,next)=>{

};

var findPassword = async (req,res,next)=>{

};

module.exports = {
	login,
	register,
	verify,
	logout,
	getUser,
	findPassword
};
```

回到`app.js`文件中，修改请求的路径：

```js
app.use('/api2/users', usersRouter); // 从api2开始请求
```

### 先做验证接口

```js
先进行测试一下

var verify = async (req,res,next)=>{ 
	res.send({
		msg: '测试',
		status: 0
	})
};
```

![需求接口设计](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\需求接口设计.jpg)

### 邮箱验证

`nodemailer`模块

需要开启`Smtp`简单邮件传输协议，[官网](https://nodemailer.com/about/)

来到QQ邮箱，设置`smtp`：

我们先来理一下思路：也就是说要有两个qq邮箱，验证需要返回给用户的用户邮箱以及发送给用户的邮箱

设置QQ邮箱：

登录QQ邮箱 > 设置 > 账户 >

![开启邮箱密钥](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\开启邮箱密钥.jpg)

使用`nodemailer`模块：

```shell
npm install nodemailer --save
```

回到`untils`文件夹中的`config.js`，完成邮箱的配置：

```js
var mongoose = require('mongoose');
var nodemailer = require('nodemailer');

var Mongoose = {
	url : 'mongodb://localhost:27017/miaomiao', // 连接数据库的url
	connect(){ // 连接数据库的方法
		mongoose.connect(this.url , { useNewUrlParser: true, useUnifiedTopology: true }, (err)=>{
			if(err){
				console.log('数据库连接失败');
				return;
			}
			console.log('数据库连接成功');
		});
	}
};

var Email = {
	config : {
		host: "smtp.qq.com",
	    port: 587,
	    auth: {
	      user: '1183476700@qq.com', // 邮箱发件人
	      pass: 'gafmrugznbsbihbi' // 密钥
	    }
	},
	get transporter(){
		return nodemailer.createTransport(this.config);
	},
	get verify(){ // 生成验证码
		return Math.random().toString().substring(2,6);
	},
	get time(){
		return Date.now();
	}
};

module.exports = {
	Mongoose,
	Email
};
```

回到`controllers`中的`users.js`：

```js
var { Email } = require('../untils/config.js'); // 引入邮箱依赖

var verify = async (req,res,next)=>{
	// res.send({
	// 	msg: '测试',
	// 	status: 0
	// })

	var email = req.query.email;
    
	// 需要发送字段及信息
	var mailOptions = {
	    from: '测试 1183476700@qq.com', // 发件人
	    to: email, // 发送给谁
	    subject: '这是一个测试邮件', // 邮箱主题
	    text: '验证码：' + Email.verify // 邮箱文件主题内容
	}

	Email.transporter.sendMail(mailOptions,(err)=>{
		if(err){
			res.send({
				msg : '验证码发送失败',
				status : -1
			});
		}
		else{
			res.send({
				msg : '验证码发送成功',
				status : 0
			});
		}
	});
}; 

module.exports = {
	login,
	register,
	verify,
	logout,
	getUser,
	findPassword
};
```

![验证码发送成功](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\验证码发送成功.jpg)

## 数据持久化与用户注册接口

下载`session`实现数据持久化

```shell
npm install express-session --save
```

去`app.js`中去引入下载好的中间介：

```js
var session = require('express-session')

app.use(session({
  secret: '$#%$%$%', // 加密
  name : 'sessionId',
  resave: false,
  saveUninitialized: false,
  cookie: {
	maxAge : 1000*60*60 // session的过期时间
  }
}));
```

原理：通过在客户端保存一个`cookie`，当我们去请求地址的时候可以把`cookie`拿过来，然后在服务端做一个验证的机制。 

回到`controller.js`中的`users.js`：

```js
var verify = async (req,res,next)=>{
	// res.send({
	// 	msg: '测试',
	// 	status: 0
	// })
	var email = req.query.email;
	var verify = Email.verify;
    
    // 不知道为啥,加上就报错
	// req.session.verify = verify; // 持久化验证
	// req.session.email = email; // 持久化邮箱

	// 需要发送字段及信息
	var mailOptions = {
	    from: '测试 1183476700@qq.com', // 发件人
	    to: email, // 发送给谁
	    subject: '这是一个测试邮件', // 邮箱主题
	    text: '验证码：' + verify // 邮箱文件主题内容
	}

	Email.transporter.sendMail(mailOptions,(err)=>{
		if(err){
			res.send({
				msg : '验证码发送失败',
				status : -1
			});
		}
		else{
			res.send({
				msg : '验证码发送成功',
				status : 0
			});
		}
	});
}; 
```

为了更好的缓存，可以引入`redis`数据库，来优化我们的项目缓存。

## 注册

```js
var register = async (req,res,next)=>{
    
	var { username , password , email , verify } = req.body;
	
    // 不知道为啥,加上下述代码就报错,说email为undefined
	// if( email !== req.session.email || verify !== req.session.verify ){
	// 	res.send({
	// 		msg : '验证码错误',
	// 		status : -1
	// 	});
	// 	return;
	// }

	// if( (Email.time - req.session.time)/1000 > 60 ){
	// 	res.send({
	// 		msg : '验证码已过期',
	// 		status : -3
	// 	});
	// 	return;
	// }
	
	var result = await UserModel.save({
		username,
		password,
		// password : setCrypto(password),
		email
	});

	if(result){
		res.send({
			msg : '注册成功',
			status : 0
		});
	}
	else{
		res.send({
			msg : '注册失败',
			status : -2
		});
	}
};
```

## 完善数据层，启用数据库

回到`models`中的`users.js`文件：

```js
var mongoose = require('mongoose');

mongoose.set('useCreateIndex',true);

var UserSchema = new mongoose.Schema({
	username : { type : String , required : true , index : { unique : true } },
	password : { type : String , required : true },
	email : { type : String , required : true , index : { unique : true } },
	date : { type : Date , default : Date.now() }
});

var UserModel = mongoose.model('user' , UserSchema);
UserModel.createIndexes();

var save = (data)=>{
	var user = new UserModel(data);
	return user.save()
		   .then(()=>{
		   		return true;
		   })
		   .catch(()=>{
		   		return false;
		   });
};

module.exports = {
	save
};
```

回到`controllers`中的`users.js`：

```js
var UserModel = require('../models/users.js');
```

![数据库数据保存成功](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\数据库数据保存成功.jpg)

## 登录接口设计

回到`models`文件夹下的`users.js`，添加一个函数`findLogin`：

```js
var findLogin = (data)=>{
	return UserModel.findOne(data);
}
module.exports = {
	save,
	findLogin
};
```

回到`controllers`中的`users.js`文件：

```js
var login = async (req,res,next)=>{
	var { username , password } = req.body;
    
	var result = await UserModel.findLogin({
		username,
		password
	});
	if(result) {
		res.send({
			msg: '登录成功',
			status: 0
		})
	}
	else{
		res.send({
			msg : '登录失败',
			status : -1
		});
	}
};
```

测试一下，根据数据库中已经保存了的一条数据：

![登录的接口](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\登录的接口.jpg)

## 获取用户和退出接口

在登录的时候转存一下`username`：

```js
if(result) {
    req.session.username = username; // 转存一下
    res.send({
        msg: '登录成功',
        status: 0
    })
}
```

退出接口设计：

```js
var logout = async (req,res,next)=>{
	req.session.username = '';
	res.send({
		msg : '退出成功',
		status : 0
	});
};
```

获取用户状态接口设计：

```js
var getUser = async (req,res,next)=>{
	
	if(req.session.username){
		res.send({
			msg : '获取用户信息成功',
			status : 0,
			data : {
				username : req.session.username,
				// isAdmin : req.session.isAdmin,
				// userHead : req.session.userHead
			}
		});
	}
	else{
		res.send({
			msg : '获取用户信息失败',
			status : -1
		});
	}
};
```

测试接口（查看登录状态）：

![查看登录状态](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\查看登录状态.jpg)

测试接口：（退出功能）

![退出接口](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\退出接口.jpg)

## 修改密码

```js
var findPassword = async (req,res,next)=>{
	var { email , password , verify } = req.body;
	
	if( email === req.session.email && verify === req.session.verify ){
		var result = await UserModel.updatePassword(email , setCrypto(password));
		if(result){
			res.send({
				msg : '修改密码成功',
				status : 0
			});
		}
		else{
			res.send({
				msg : '修改密码失败',
				status : -1
			});
		}
	}
	else{
		res.send({
			msg : '验证码失败',
			status : -1
		});
	}
};
```

测试：

![修改密码](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\修改密码.jpg)

至此，所有的数据接口都已经实现完成了。



# 2.实现前台功能

来到`Mine`文件夹下的`index.vue`文件中：

在`Mine`文件夹下新建`center.vue`文件：

```html
<template>
  <div>
     个人中心
  </div>
</template>

<script>
export default {
   name: 'center'
}
</script>

<style>

</style>
```

去到`components`文件夹下新建一个`Register`文件夹，里面新建一个`index.vue`文件：

```html
<template>
  <div>注册页</div>
</template>

<script>
export default {
   name: 'register'
}
</script>

<style>

</style>
```

去到`components`文件夹下新建一个`FindPassword`文件夹，里面新建一个`index.vue`文件：

```html
<template>
  <div>修改密码</div>
</template>

<script>
export default {
   name: 'findPassWord'
}
</script>

<style>

</style>
```

## 配置路由

```js
{
    path: '/mine',
    component: Mine,
    children: [
        {
            path: 'center',
            component: () => import('@/views/Mine/center.vue')
        },
        {
            path: 'login',
            component: () => import('@/components/Login')
        },
        {
            path: 'register',
            component: () => import('@/components/Register')
        },
        {
            path: 'center',
            component: () => import('@/components/FindPassword')
        },
        {
            path: '/mine', // 重定向到center页面
            redirect: 'center'
        }
    ]
}
```

回到`Mine`文件夹下的`index.vue`文件中，将路由展示替换为组件：

```html
<template>
  <div id="main">
    <!-- 使用组件 -->
    <Header title="mine" />
    <div id="content">
      <router-view /> <!-- 路由展示区域 -->
      <!-- <Login /> -->
    </div>
    <TabBar />
  </div>
</template>
```

## 实现登录状态组件

回到`Login`文件夹中的`index.vue`文件中：

```js
export default {
   name: 'Login',
   data() {
      return {
         username: '', // 定义用户的用户名和密码为空
         password: ''
      }
   }
} 
```

```html
<!-- 使用v-model开启双向数据绑定 -->
<input class="login_text" v-model="username" type="text" placeHolder="账户名" >
<input class="login_text" v-model="password" type="password" placeHolder="密码" >
```

```html
<!-- 给登录按钮添加点击事件 -->
<input type="submit" value="登录" @touchstart="handleToLogin">
```

去`vue.config.js`中去代理一下我们的接口，因为我们要进行数据请求了。

```js
// 反向代理我们的接口
proxy: {
    '/api2': { // 代理本地的接口要放在比较上面一点
        target: 'http://localhost:3000',
        changeOrigin: true
    },
    '/api': {
       target: 'http://39.97.33.178',
       changeOrigin: true
    }
}    
```

重启服务，让反向代理生效。

```js
// 给登录按钮点击事件发送axios请求
methods: {
  handleToLogin() { 
     this.axios.post('/api2/users/login', {
        username: this.username,
        password: this.password
     }).then((res) => {
        console.log(res)
     })
  }
}
```

我们切换到`/login`地址来进行测试一下我们的反向代理是否成功！

![登录测试](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\登录测试.jpg)

当我们换成正确的账号和密码之后就会显示成功登录了。

![登录成功](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\登录成功.jpg)

好了，登录成功之后要给用户一个提示信息，我们之前有做过一个`MessageBox`文件，我们可以使用那个！

```js
import { messageBox } from '@/components/JS' // 引入弹窗

methods: {
  handleToLogin() {
     this.axios.post('/api2/users/login', {
        username: this.username,
        password: this.password
     }).then((res) => {
        // console.log(res)
        var status = res.data.status
        if(status === 0) {
           messageBox({
              title: '登录',
              content: '登录成功',
              ok: '确定'
           })
        }else {
           messageBox({
              title: '登录',
              content: '登录失败',
              ok: '确定'
           })
        }
     })
  }
}
```

优化弹窗，加上`v-if`条件判断，美化界面样式：

```js
优化1：在 JS > MessageBox > index.vue中

<span v-if="cancel" @touchstart="handleCancel">{{ cancel }}</span>
<span v-if="ok" @touchstart="handleOk">{{ ok }}</span>
```

```js
优化2：来到 JS > index.js 中

// 给我们定制的模板一些默认参数
var default1 = {
  title: '',
  content: '',
  cancel: '',
  ok: '',
  handleCancel: null,
  handleOk: null
}
var MyComponent = Vue.extend(MessageBox)

将上述代码放到后续return 代码块中,原因类似vue组件中为什么data是一个方法并且要返回一个对象是一样的。
```

![当我们输入错误的账号密码的时候](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\当我们输入错误的账号密码的时候.jpg)

当我们登录成功之后，要使用`this.$router.push`跳转到个人中心页面：

```js
// 注意两个点: 1.this指向问题;2.$router.push使用
methods: {
  handleToLogin() {
     this.axios.post('/api2/users/login', {
        username: this.username,
        password: this.password
     }).then((res) => {
        var status = res.data.status
        var This = this // 保存一下this指向
        if(status === 0) {
           messageBox({
              title: '登录',
              content: '登录成功',
              ok: '确定',
              handleOk() {
                 This.$router.push('/mine/center')
              }
           })
        }else {
           messageBox({
              title: '登录',
              content: '登录失败',
              ok: '确定'
           })
        }
     })
  }
}
```

当我们输入正确的用户名和密码之后就会跳转到用户中心页面。图片我就不放了，自行补脑。

以上，登录页面就已经完成了。

## 用户中心页面

我们先来做一下用户中心的退出功能。

来到`Mine` > `center.vue`中，点击退出按钮，对应事件发起`axios`请求：

```html
<template>
  <div>
     个人中心：<a href="javascript:;" @touchstart="handleToLogin">退出</a>
  </div>
</template>

<script>
export default {
   name: 'center',
   methods: {
      handleToLogin() {
         this.axios.get('/api2/users/logout').then(res => {
            var status = res.data.status
            if(status === 0) {
               this.$router.push('/mine/login')
            }
         })
      }
   }
}
</script>

<style>

</style>
```

以上就是退出页面。

## 使用路由守卫去监听用户状态

处理当用户没有登录的时候点击我的的时候，是会跳转到注册页面的；但是当用户注册完成之后，从别的页面跳转到我的页面的时候，是直接显示`center`页面的，你有没有想到应该怎么去处理？

因为我们仅仅是组件之间的状态判断，所以我们使用组件内的路由守卫。

来到个人中心`center.vue`页面，添加前置守卫：

```js
// 需要注意两点:
// 1.为什么还要进一步导入aixos,之前我们不是已经将其挂载到Vue原型链上去了么？这是因为路由守卫中明确提出我们在路由内部是拿不到this指向的，所以我们需要再次引入axios，已确保我们此时拿到的this是正确的
// 2.路由守卫的使用

import axios from 'axios'
export default {
   name: 'center',
   methods: {
      handleToLogin() {
         this.axios.get('/api2/users/logout').then(res => {
            var status = res.data.status
            if(status === 0) {
               this.$router.push('/mine/login')
            }
         })
      }
   },
   beforeRouteEnter(to, from, next) {
      axios.get('/api2/users/getUser').then(res => {
         var status = res.data.status
         if(status === 0) {
            next()
         }else {
            next('/mine/login')
         }
      })
   }
}
```

接着需要获取到当前登录用户，并且把数据渲染到页面上。

```html
<template>
  <div>
     <div>个人中心：</div>
     <div>当前用户：<a href="javascript:;" @touchstart="handleToLogin">退出</a></div>
  </div>
</template>
```

因为获取到的用户信息可能不仅仅在一个页面上会使用到，所以我们将其存到`vuex`中：

在`stores`中创建一个文件夹`user`，里面再新建一个`index.vue`文件：

```js
const state = {
   name: ''
};

const actions = {

};

const mutations = {
   USER_NAME(state,payload) {
      state.name = payload.name
   }
}

export default {
   namespaced: true,
   state,
   actions,
   mutations
}
```

去`stores`文件夹中的入口状态管理`index.js`中去注册一下。

```js
import user from './user' // 1.引入
modules: {
  city,
  user // 2.注册
}
```

然后回到我们的个人中心`center`页面进行个人状态管理就可以了：

```js
beforeRouteEnter(to, from, next) {
  axios.get('/api2/users/getUser').then(res => {
     var status = res.data.status
     if(status === 0) {
        next(vm => { // 官网说可以通过 vm 访问到组件实例
           vm.$store.commit('user/USER_NAME', { name: res.data.data.username }) // 第一个参数就是名字，第二个参数就是接口中写好的返回的数据
        })
     }else {
        next('/mine/login')
     }
  })
}
```

通过上述代码的设置就成功把我们的数据存到状态管理里面了。

![数据存储](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\数据存储.jpg)

以上我们就可以发现在`vue-devtools`里面，我们可以发现数据从无到有的变化过程了。

那么我们就可以在个人中心页面进行用户信息的渲染了：

```html
<div>当前用户：{{ $store.state.user.name }} <a href="javascript:;" @touchstart="handleToLogin">退出</a></div>
```

细节优化：当我们退出的时候，在`center`页面置空用户名

```js
methods: {
  handleToLogin() {
     this.axios.get('/api2/users/logout').then(res => {
        var status = res.data.status
        if(status === 0) {
           this.$store.commit('user/USER_NAME', {name: ''}) // 1.当我们退出的时候,将用户名置空
           this.$router.push('/mine/login')
        }
     })
  }
},
```

![退出置空数据](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\退出置空数据.jpg)

以上就是所有登录以及用户状态的信息。

## 注册和找回密码

来到`Login`文件夹下的`index.vue`文件中：

```html
<div class="login_link">
	<router-link to="/mine/register">立即注册</router-link>
	<router-link to="/mine/findPassword">找回密码</router-link>
</div>
```

来到`Register`文件夹下的`index.vue`文件，来完成注册页的开发：确认密码部分

```html
<template>
  <div class="register_body">
    <div class="register_email">
      邮箱：<input v-model="email" class="register_text" type="text"> <button>发送验证码</button>
    </div>
    <div>
      用户名：<input v-model="username" class="register_text" type="text">
    </div>
    <div>
      密码：<input v-model="password" class="register_text" type="password">
    </div>
    <div>
      确认密码：<input class="register_text" type="password"> <!-- 此功能不开发 -->
    </div>
    <div>
      验证码：<input v-model="verify" class="register_text" type="text">
    </div>
    <div class="register_btn">
      <button>注册</button>
    </div>
    <div class="register_link">
      <router-link to="/mine/login">立即登录</router-link>
      <router-link to="/mine/findPassword">找回密码</router-link>
    </div>
  </div>
</template>

<script>
export default {
   name: 'register',
   data() { // 双向数据绑定
     return {
       email: '',
       username: '',
       password: '',
       verify: ''
     }
   }
}
</script>

<style lang="scss" scoped>
   .register_body{  
      .register_email{
        position: relative;
        button{
          position: absolute;
          right: 10px;
          top: 10px;
          height: 30px;
        }
      }
      width: 100%;
      .register_text{ 
         width: 100%; 
         height: 40px; 
         border: none; 
         border-bottom: 1px solid #ccc; 
         margin-bottom: 5px; 
         outline: none; 
         text-indent: 10px;
      }
      .register_btn{ 
         height: 50px; 
         margin: 10px;
         button{
            width: 100%; 
            height: 100%; 
            background-color: #e54847; 
            border-radius: 3px; 
            border: none; 
            display: block; 
            color: white; 
            font-size: 20px;
         }
      }
      .register_link{ 
         display: flex; 
         justify-content: space-between;
         a{ 
            text-decoration: none; 
            margin: 0 5px; 
            font-size: 12px; 
            color:#e54847;
         }
      }
   }
</style>
```

给注册按钮绑定点击事件：

```html
<div class="register_btn">
	<button @touchstart="handleToRegister">注册</button>
</div>

<button @touchstart="handleToVerify">发送验证码</button>
```

**验证码部分**：

```js
import { messageBox } from "@/components/JS"; // 导入信息提示弹窗

handleToVerify() {
  this.axios.get("/api2/users/verify?email=" + this.email).then(res => {
    var status = res.data.status;
    if (status === 0) {
      // 验证码发送成功,需要导入messageBox组件来做一个提示用户作用
      messageBox({
        title: "验证码",
        content: "验证码发送成功",
        ok: "确定"
      });
    } else {
      messageBox({
        title: '验证码',
        content: '验证码发送失败',
        ok: '确定'
      });
    }
  });
},
```

![验证码部分](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\验证码部分.jpg)

**注册**：

```js
handleToRegister() {
  this.axios
    .post("/api2/users/register", {
      email: this.email,
      username: this.username,
      password: this.password,
      verify: this.verify
    })
    .then(res => {
      var status = res.data.status;
      var This = this // 1.为路由跳转做准备
      if (status === 0) {
        messageBox({
          title: "注册",
          content: "用户注册成功",
          ok: "确定",
          handleOk() {
            This.$router.push('/mine/login') // 2.当注册成功之后就往注册页跳转
          }
        });
      } else {
        messageBox({
          title: "注册",
          content: res.data.msg + "，请重新注册", // 3.把失败的原因显示出来给用户
          ok: "确定"
        });
      }
    });
}
```

**修改密码**：

来到`FindPassword`文件夹中的`index.vue`文件：

```html
<template>
  <div class="password_body">
    <div class="password_email">
      邮箱：<input class="password_text" type="text"> <button>发送验证码</button>
    </div>
    <div>
      新密码：<input class="password_text" type="password">
    </div>
    <div>
      验证码：<input class="password_text" type="text">
    </div>
    <div class="password_btn">
      <button>修改</button>
    </div>
    <div class="password_link">
      <router-link to="/mine/login">立即登录</router-link>
      <router-link to="/mine/register">立即注册</router-link>
    </div>
  </div>
</template>

<script>
export default {
   name: 'findPassWord'
}
</script>

<style lang="scss" scoped>
.password_body {
  .password_email {
    position: relative;
    button {
      position: absolute;
      right: 10px;
      top: 10px;
      height: 30px;
    }
  }
  width: 100%;
  .password_text {
    width: 100%;
    height: 40px;
    border: none;
    border-bottom: 1px solid #ccc;
    margin-bottom: 5px;
    outline: none;
    text-indent: 10px;
  }
  .password_btn {
    height: 50px;
    margin: 10px;
    button {
      width: 100%;
      height: 100%;
      background-color: #e54847;
      border-radius: 3px;
      border: none;
      display: block;
      color: white;
      font-size: 20px;
    }
  }
  .password_link {
    display: flex;
    justify-content: space-between;
    a {
      text-decoration: none;
      margin: 0 5px;
      font-size: 12px;
      color: #e54847;
    }
  }
}
</style>
```

以上便是基本的样式文件。

添加点击事件：

```html
<button @touchstart="handleToVerify">发送验证码</button>
<button @touchstart="handleToPassword">修改</button>
```

```js
// 定义数据
data() {
    return {
      email: '',
      password: '',
      verify: ''
    }
}

// 双向数据绑定到view上
<input class="password_text" v-model="email" type="text" />
<input class="password_text" v-model="password" type="password" />
<input class="password_text" v-model="verify" type="text" />
```

**找回密码**：

```js
// 功能
// 发起验证码和注册是一样的接口,所以可以共用代码
// 修改密码和注册密码是通用的
handleToVerify() {
  // 验证码功能
  this.axios.get("/api2/users/verify?email=" + this.email).then(res => {
    var status = res.data.status;
    if (status === 0) {
      // 验证码发送成功,需要导入messageBox组件来做一个提示用户作用
      messageBox({
        title: "验证码",
        content: "验证码发送成功",
        ok: "确定"
      });
    } else {
      messageBox({
        title: "验证码",
        content: "验证码发送失败",
        ok: "确定"
      });
    }
  });
}
```

**修改密码**：

```js
import { messageBox } from "@/components/JS"; // 导入信息提示弹窗

handleToPassword() {
  this.axios.post('/api2/users/findPassword', {
    email: this.email,
    password: this,password,
    verify: this.verify
  }).then(res => {
    var status = res.data.status;
    var This = this
    if(status === 0) {
      messageBox({
        title: '修改',
        content: '修改密码成功',
        ok: '确定',
        handleOk() {
          This.$router.push('/mine/login');
        }
      })
    }else {
      messageBox({
        title: '修改',
        content: '修改密码失败',
        ok: '确定'
      })
    }
  })
}
```

![修改密码1](C:\Users\lenovo\Desktop\5天背诵\17.mini_cinema项目需求\img_server\修改密码1.jpg)

至此所有的页面组件都完成了。
