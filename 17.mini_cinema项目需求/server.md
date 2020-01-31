# 服务端

## 全局安装`express`脚手架

```shell
npm install express-generator -g
```

## 快速构建项目

```shell
express -e miniserver
```

```shell
cd miniserver // 进项目
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



# 实现前台的功能

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











































































