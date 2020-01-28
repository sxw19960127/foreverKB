# `jQuery`

## 初识jQuery

```
jQuery是一款优秀的JavaScript库,从命名可以看出jQuery最主要的用途是用来做查询(jQuery=js+Query);
正如jQuery官方logo副标题所说(write less,do more);
使用jQuery能让我们对html文档遍历和操作,事件处理,动画以及Ajax变得更加简单;
版本:
	1.x: 兼容IE678,相对其它版本文件比较大;
	2.x: 不兼容IE678,文件比较小;
	3.x: 不兼容IE678,只支持最新浏览器;
```

```html
<style>
  div{
     width: 100px;
     height: 100px;
     border: 1px solid black;
  }
</style>
<script>
  window.onload = function() {
     // 利用原生js查找DOM元素
     var div1 = document.getElementsByTagName('div')[0];
     var div2 = document.getElementsByClassName('box1')[0];
     var div3 = document.getElementById('box2');
     console.log(div1);
     console.log(div2);
     console.log(div3);
  }
  // 利用jQuery查找DOM元素
  $(function() {
     var $div1 = $('div')[0];
     var $div2 = $('.box1')[0];
     var $div3 = $('#box2')[0];
     console.log($div1);
     console.log($div2);
     console.log($div3);
  })
</script>
<body>
   <div></div>
   <div class="box1"></div>
   <div id="box2"></div>
</body>
```

```html
<style>
  div{
     width: 100px;
     height: 100px;
     border: 1px solid black;
  }
</style>
<script>
  window.onload = function() {
     // 利用原生js修改背景颜色
     var div1 = document.getElementsByTagName('div')[0];
     var div2 = document.getElementsByClassName('box1')[0];
     var div3 = document.getElementById('box2');
     // div1.style.backgroundColor = 'red';
     // div2.style.backgroundColor = 'green';
     // div3.style.backgroundColor = 'pink';
  }
  // 利用jQuery修改背景颜色
  $(function() {
     var $div1 = $('div');
     var $div2 = $('.box1');
     var $div3 = $('#box2');
     $div1.css({
        background: 'black'
     })
     $div2.css({
        background: 'green'
     })
     $div3.css({
        background: 'orange'
     })
  })
</script>
<body>
   <div></div>
   <div class="box1"></div>
   <div id="box2"></div>
</body>
```

## jQuery的入口函数

```js
// 1.原生js的固定写法
window.onload = function() {

}
// 2.jQuery的固定写法
$(document).ready(function() {
 alert("hello");
})
```

## jQuery和js加载模式以及两大区别

```html
<script>
  // 1.通过原生js入口函数可以拿到DOM元素
  window.onload = function() {
     var img = document.getElementsByTagName('img')[0];
     console.log(img);
     // 2.通过原生js入口函数可以拿到DOM元素宽高
     var width = window.getComputedStyle(img).width;
     console.log("原生" + width); //原生760px
  }
  // 2.通过jQuery的入口函数可以拿到DOm元素
  $(document).ready(function() {
     var $img = $('img'); 
     // 注意,在获取DOM节点的时候这里需要加上[0];
     // 而在下面获取元素的宽高的时候要把这个[0]删掉才能获取到;
     console.log($img);
     // 2.通过jQuery的入口函数不可以拿到DOm元素的宽高
     var $width = $img.width();
     console.log("jQ" + $width); //jQ 0px
  })

  // 原生js和jQuery入口函数的加载模式不同: 
  // 原生js会等到DOM元素加载完毕,并且图片也加载完毕之后才会执行;
  // jQuery会等到DOM元素加载完毕,但不会等到图片也加载完毕就会执行;
</script>
<body>
   <img src="https://img-bss.csdn.net/1560249226461.png" alt="">
</body>
```

```js
// 原生js如果编写了多个入口函数,后面编写的会覆盖前面编写的
window.onload = function() {
	// alert("sxw1")
}
window.onload = function() {
	// alert("sxw2")
}

// 用jQuery编写多个入口函数,后面编写的不会覆盖前面编写的
$(document).ready(function() {
	alert("sxw3")
})
$(document).ready(function() {
	alert("sxw4")
})
```

## jQuery入口函数的其它写法

```js
// 第一种方式
$(document).ready(function() {
	// alert("sxw1")
})
// 第二种方式
jQuery(document).ready(function() {
	// alert("sxw2")
})
// 第三种方式 推荐
$(function() {
	// alert("sxw3")
})
// 第四种方式
jQuery(function() {
	alert("sxw4")
})
```

## jQuery冲突问题

```
1.放弃$符号的使用权;
2.自定义一个符号代替$;
```

```js
// 1.释放jQuery的使用权
// 注意: 释放操作必须在编写其它jQuery代码之前编写;
// 释放之后不能再使用 $ 符号了,用jQuery代替$
// 2.自定义一个访问符号
var sxw = jQuery.noConflict();
// jQuery.noConflict();
sxw(function() {
	alert('hello')
})
```

## jQuery核心函数

```html
<script>
  // $(); 就是调用jQuery的核心函数
  // 核心函数可以接收的参数
  // 1.接收一个函数
  $(function() {
     alert("sxw1")
     // 2.接收一个字符串
     //    2.1接收一个字符串选择器
     //       返回一个jQuery对象,对象保存了找到的DOM元素
     var box1 = $(".box1");
     var box2 = $("#box2");
     console.log(box1);
     console.log(box2);
     //    2.2接收一个字符串代码片段
     //       返回一个jQuery对象,对象保存了创建的DOM元素
     var $p = $("<p>我是p</p>");
     console.log($p);
     box1.append($p);
     // 3.接收一个DOM元素
     // 会被包装成一个jQuery对象返回给我们
     var span = document.getElementsByTagName('span')[0];
     console.log(span);
     var $span = $(span);
     console.log($span);
  })
</script>
<body>
   <div class="box1"></div>
   <div id="box2"></div>
   <span>我是一个span</span>
</body>
```

## jQuery对象

```html
<script>
  $(function() {
     // 1.什么是jQuery对象?
     //   jQuery对象是一个伪数组
     // 2.什么是伪数组?
     //   有0到length-1的属性,并且有length属性
     var $div = $('div');
     console.log($div);
  })
</script>
<body>
   <div>div1</div>
   <div>div2</div>
   <div>div3</div>
</body>
```

## jQuery静态方法

```js
// 1.定义一个类
function AClass() {

}
// 2.给这个类添加一个静态方法
// 直接添加给类的就是静态方法
AClass.staticMethod = function() {
	alert('staticMethod');
}
// 静态方法通过类名调用
AClass.staticMethod();

// 3.给这个类添加一个实例方法
AClass.prototype.instanceMethod = function() {
	alert('instanceMethod')
}
// 实例方法通过类的实例调用
// 创建一个实例(创建一个对象)
var a = new AClass();
// 通过实例调用实例方法
a.instanceMethod();
```

### 静态方法 forEach

```js
var arr = [1, 3, 5, 7];
// 第一个参数: 遍历到的元素
// 第二个参数: 当前遍历到的元素索引值
// 注意:原生的forEach方法只能遍历数组,不能遍历伪数组
// 参数先传value
arr.forEach(function(value, index) {
 // console.log(index, value);
})
// 伪数组
var obj = {0:1, 1:3, 2:5, 3:7, 4:9, length:5} //不能用forEach遍历

// 1.利用jQuery的Each静态方法遍历数组
// 参数先传index
// jQuery是可以遍历伪数组的
$.each(arr,function(index, value) {
 // console.log(index, value);
})
$.each(obj,function(index, value) {
 console.log(index, value);
})
```

```js
var arr = [1, 3, 5, 7];
var obj = {0:1, 1:3, 2:5, 3:7, 4:9, length:5} 
// 1.利用原生js的map方法遍历
// 第一个参数,当前遍历到的元素
// 第二个参数,当前遍历到的索引
// 第三个参数,当前被遍历的数组
// 注意,和原生的forEach一样,不能遍历伪数组
arr.map(function(value, index, array) {
 // console.log(index, value, array);
})

// 第一个参数,要遍历的数组
// 第二个参数,每次遍历所执行的回调函数
// 回调函数的参数
// 第一个参数,遍历到的元素
// 第二个参数,遍历到的索引
// 注意点,和forEach静态方法一样,map静态方法也可以遍历伪数组
$.map(arr, function(value, index) {
 // console.log(index, value)
})
$.map(obj, function(value, index) {
 console.log(index, value)
})

// jQuery中的each静态方法和map静态方法的区别:
// each静态方法默认的返回值是遍历的数组,不支持在回调函数中对遍历的数组进行处理
// map静态方法默认的返回值是一个空数组,可以在回调函数中通过return对遍历的数组进行处理
```

```js
// $.trim(); 去除字符串两端的空格
// 参数: 需要去除空格的字符串
// 返回值: 去除空格之后的字符串
var str = "   sxw   ";
console.log("---" + str + "---");
var res = $.trim(str);
console.log("---" + res + "---");

// $.isWindow(); 判断传入的是不是window对象
// 返回值 true/false
var arr = [1,2,3];
var fn = function() {};
var w = window;
var res = $.isWindow(arr);
console.log(res);

// $.isArray(); 判断传入的数组是否是真数组
// 返回值 true/false

// $.isFunction(); 判断传入的数组是否是函数
// 注意:jQuery本质上是一个函数 
// 返回值 true/false
```

## .holdReady();的使用

```html
<script>
      // $.holdReady(true); 作用,暂停/延迟ready执行,暂停出口函数的执行
      $.holdReady(true);
      $(document).ready(function() {
         alert('ready');
      })
   </script>
</head>
<body>
   <button>按钮</button>
   <script>
      var btn = document.getElementsByTagName('button')[0];
      btn.onclick = function() {
         alert('hello')
         $.holdReady(false);
      }
   </script>
```

## jQuery内容选择器

```html
	<style>
      div{
         width: 50px;
         height: 50px;
         border: 1px solid black;
         margin-top: 5px;
      }
   </style>
   <script>
      $(function() {
         // :empty 找到既没有文本也没有子元素的指定元素
         var $div = $('div:empty');
         // console.log($div)

         // :parent 找到有文本或有子元素的指定元素
         var $div = $('div:parent');
         // console.log($div)

         // :contains(text) 找到所有div,里面包含2的指定元素
         var $div = $("div:contains('2')");
         // console.log($div)

         // :has(selector) 找到包含指定子元素的指定元素
         var $div = $("div:has('span')");
         console.log($div)
      })
   </script>
</head>
<body>
   <div></div>
   <div>1212</div>
   <div><span>1</span></div>
   <div>
      <p></p>
   </div>
</body>
```

```
// 1.什么是属性？
//    对象身上保存的变量就是属性
// 2.如何操作属性？
//    赋值 ---> 对象.属性名称 = 值;
//       取值 --- > 对象.属性名称;
//    赋值 ---> 对象["属性名称"] = 值
//       取值 --- > 对象["属性名称"]
// 3.什么是属性节点？
//    <span name="sxw"></span>
//       在编写html代码时,在html标签中添加的属性就是属性节点
//       在浏览器中找到span这个DOM元素之后,展开看到的都是属性
//       在attributes属性中保持的所有的内容都是属性节点
// 4.如何操作属性节点？
// DOM元素 .setAttribute("属性名称", "值");
// DOM元素 .getAttribute("属性名称");
// 5.属性和属性节点的区别?
// 任何对象都有属性,但是只有DOM对象才有属性节点
```

```html
<script>
  $(function() {
     // attr(name|pro|key, val|fn)
     // 作用: 获取或者设置属性节点的值
     // 可以传递一个参数,也可以传递两个参数
     // 传递一个参数,表示获取属性节点的值; 
     //       无论找到多少个元素,都会返回第一个元素指定的属性节点的值
     // console.log($("span").attr("class"));

     // 传递两个参数,表示设置属性节点的值
     //       找到多少个元素,就会设置上多少个元素
     //       如果设置的属性节点不存在,那么系统会自动新增
     // console.log($("span").attr("class","box"))
     // $("span").attr("abc","123");

     // removeAttr(name) 删除属性节点
     // 会删除所有找到元素指定的属性节点
     $('span').removeAttr("class name"); //删除所有span标签里面的class和name属性节点
  })
</script>
   <span class="span1" name="sxw"></span>
   <span class="span2" name="zjl"></span>
```

```html
<script>
      $(function() {
         // prop方法和attr方法一致
         // 找到所有span标签,中的eq(0) === 第1个,设置上demo值为666
         $("span").eq(0).prop("demo","666");
         $("span").eq(1).prop("demo","777");

         // removeProp方法和removeAttr方法一致
         $("span").removeProp('demo');

         //注意: prop方法不仅能够操控属性,还能够操控属性节点
         console.log($('span').prop('class'));
         $("span").prop("class", "box");

         console.log($('input').prop('checked')); //true / false
         console.log($('input').attr('checked')); //checked / undefined

         // 那么在什么时候使用prop,什么时候使用attr？
         // 在操作属性节点的时候,具有true/false两个属性的属性节点,如 checked selected disabled使用ropr()
         // 其它情况使用attr()
      })
   </script>
</head>
<body>
   <span class="span1" name="sxw"></span>
   <span class="span2" name="zjl"></span>
   <input type="text" checked>
</body>
```

## 练习

```html
<script>
      $(function() {
         // 1.给按钮添加点击事件
         var btn = document.getElementsByTagName('button')[0];
         btn.onclick = function() {
            // 2.获取输入框输入的内容
            var input = document.getElementsByTagName('input')[0];
            var text = input.value;
            // 3.修改img的attr属性节点的值
            $('img').attr("src",text);
            // $('img').prop("src",text);
         }
      })
   </script>
</head>
<body>
   <input type="text">
   <button>切换图片</button><br>
   <img src="https://img-bss.csdn.net/1560249226461.png" alt="">
</body>
```

```html
<style>
      .class1{
         width: 100px;
         height: 100px;
         background: red;
      }
      .class2{
         border: 100px solid green;
      }
   </style>
   <script>
      $(function() {
         var btns = document.getElementsByTagName('button');
         btns[0].onclick = function() {
            $('div').addClass("class1 class2") //添加一个类
         }
         btns[1].onclick = function() {
            $('div').removeClass("class1 class2") //删除一个类
         }
         btns[2].onclick = function() {
            $('div').toggleClass('class1 class2') //切换一个类
         }
      })
   </script>
</head>
<body>
   <button>添加类</button>
   <button>删除类</button>
   <button>切换类</button>
   <div></div>
</body>
```

```html
<style>
      div{
         width: 100px;
         height: 100px;
         border: 1px solid black;
      }
   </style>
   <script>
      $(function() {
         var btns = document.getElementsByTagName('button');
         btns[0].onclick = function() {
            // 和原生innerHTML一样
            $('div').html("<p>我是段落<span>我是span</span></p>")
         }
         btns[1].onclick = function() {
            // 和原生innerHTML一样
            console.log($('div').html())
         }
         btns[2].onclick = function() {
            // 和原生的innerText一样
            $('div').text("hahaha");
         }
         btns[3].onclick = function() {
            console.log($('div').text())
         }
         btns[4].onclick = function() {
            $('input').val('请输入内容')
         }
         btns[5].onclick = function() {
            console.log($('input').val())
         }
      })
   </script>
</head>
<body>
   <button>设置html</button>
   <button>获取html</button>
   <button>设置text</button>
   <button>获取text</button>
   <button>设置value</button>
   <button>获取value</button>
   <div></div>
   <input type="text">
</body>
```

```html
<script>
      $(function() {
         // 逐个设置css
         // $('div').css('width', "100px")
         // $('div').css('height', "100px")
         // $('div').css('background', "red")

         // 链式设置css
         // $('div').css('width', "100px").css('height', "100px").css('background', "pink")

         // 批量设置css <推荐>
         $('div').css({
            width: "100px",
            height: "100px",
            border: "1px solid black"
         })

         // 获取css样式值
         console.log($('div').css('width'));
      })
   </script>
</head>
<body>
   <div></div>
```

```html
<style>
      *{
         margin: 0;
         padding: 0;
      }
      .father{
         width: 200px;
         height: 200px;
         background: red;
         border: 50px solid #000;
         margin-left: 50px;
         position: relative;
      }
      .son{
         width: 100px;
         height: 100px;
         background: blue;
         position: absolute;
         top: 50px;
         left: 50px;
      }
   </style>
   <script>
      $(function() {
         var btns = document.getElementsByTagName('button');
         btns[0].onclick = function() {
            // 获取元素的宽度
            // console.log($('.father').width())
            // 获取元素距离窗口的偏移量
            // console.log($('.son').offset().left)
            // position方法只能获取,不能设置
            console.log($(".son").position().left)
         }
         btns[1].onclick = function() {
            // 设置元素的宽度
            $('.father').width("500px")
            // 设置元素距离窗口的偏移量
            $('.son').offset({
               left: 10
            })
         }
      })
   </script>
</head>
<body>
   <div class="father">
      <div class="son"></div>
   </div>
   <button>获取</button>
   <button>设置</button>
</body>
```

```html
<style>
      *{
         margin: 0;
         padding: 0;
      }
      .scroll{
         width: 100px;
         height: 200px;
         border: 1px solid black;
         overflow: auto;
      }
   </style>
   <script>
      $(function() {
         var btns = document.getElementsByTagName('button');
         btns[0].onclick = function() {
            // 获取滚动的偏移位
            // console.log($('.scroll').scrollTop())
            // 获取网页的偏移位,兼容写法,如下:
            console.log($('html').scrollTop() + $('body').scrollTop())
         }
         btns[1].onclick = function() {
            // 设置滚动的偏移位
            // $('.scroll').scrollTop(300)
            // 设置网页的滚动偏移位,兼容写法,如下:
            $('html,body').scrollTop(9)
         }
      })
   </script>
</head>
<body>
   <div class="scroll">我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div我是div</div>
   <button>获取</button>
   <button>设置</button>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
   <br>
</body>
```

```html
<script>
      $(function() {
         // jQuery里面有两种绑定事件方式
         // 给元素重复添加事件不会影响效果的实现
         $('button').click(function() { //编码效率高,部分事件jQ没有实现,所以不能全部添加
            alert('我按了')
         })
         $('button').mouseenter(function() { //编码效率高,部分事件jQ没有实现,所以不能全部添加
            alert('我来了')
         })
         $('button').mouseleave(function() { //编码效率高,部分事件jQ没有实现,所以不能全部添加
            alert('我走了')
         })

         // $('button').on('click', function() { 编码效率低,所有js事件都可以添加
         //    alert('我按了2')
         // })
      })
   </script>
</head>
<body>
   <button>按钮</button>
```

```html
<script>
      $(function() {
         function test1() {
            alert('sxw1')
         }
         $('button').click(test1);

         // off方法不传参数,会移除所有事件
         $('button').off();
         $('button').off('click'); // 传一个参数表示把该类型的所有事件移除
         $('button').off('click', test1); //传两个参数移除指定类型click类型的指定事件test1事件
      })
   </script>
</head>
<body>
   <button>按钮</button>
```

```html
<style>
      *{
         margin: 0;
         padding: 0;
      }
      .father{
         width: 200px;
         height: 200px;
         background: red;
      }
      .son{
         width: 100px;
         height: 100px;
         background: blue;
      }
   </style>
   <script>
      $(function() {
         // 事件冒泡,事件从里往外进行触发,点击儿子,父亲也会跟随着触发事件
         $('.son').click(function(e) {
            alert('son')
            // return false; //方法1: 阻止事件冒泡
            e.stopPropagation(); //方法2
         })
         $('.father').click(function() {
            alert('father')
         })

         $("a").click(function(e) {
            alert('弹出注册框')
            // return false; 方法1: 阻止默认行为
            e.preventDefault();
         })
      })
   </script>
</head>
<body>
   <div class="father">
      <div class="son"></div>
   </div>
   <a href="http://www.baidu.com">百度</a>
   <form action="http://www.taobao.com">
      <input type="text">
      <input type="submit">
   </form>
</body>
```

```html
<style>
      *{
         margin: 0;
         padding: 0;
      }
      .father{
         width: 200px;
         height: 200px;
         background: red;
      }
      .son{
         width: 100px;
         height: 100px;
         background: blue;
      }
   </style>
   <script>
      $(function() {
         $('.son').click(function(e) {
            alert('son')
         })
         $('.father').click(function() {
            alert('father')
         })
         // 自动触发事件
         // $('.father').trigger("click"); 触发完点击事件后会自动触发事件冒泡
         // $('.father').triggerHandler('click') 触发完点击事件后不会触发事件冒泡

         $("input[type='submit']").click(function() {
            // alert("submit")
         })
         // $("input[type='submit']").trigger("click"); 自动触发完事件之后依旧会触发默认事件
         $("input[type='submit']").triggerHandler("click") // 自动触发完事件之后不会触发默认行为

         // a标签比较特殊,按如下的形式书写刷新的时候自动触发完事件之后不会跳转页面,想要跳转页面需要将内容部分用span标签包裹起来
         // $('a').click(function() {
         //    alert('sxw')
         // })
         // $('a').trigger("click");
         
         $('span').click(function() {
            alert('sxw')
         })
         $('span').trigger("click");
      })
   </script>
</head>
<body>
   <div class="father">
      <div class="son"></div>
   </div>
   <a href="http://www.baidu.com"><span>百度</span></a>
   <form action="http://www.taobao.com">
      <input type="text">
      <input type="submit">
   </form>
</body>
```

```html
<style>
      *{
         margin: 0;
         padding: 0;
      }
      .father{
         width: 200px;
         height: 200px;
         background: red;
      }
      .son{
         width: 100px;
         height: 100px;
         background: blue;
      }
   </style>
   <script>
      $(function() {
         // 自定义事件,使用trigger
         // 事件必须使用on进行绑定,并且必须使用trigger进行触发
         $('.son').on("sxw",function() {
            alert('sxw')
         })
         $('.son').trigger("sxw")
      })
   </script>
</head>
<body>
   <div class="father">
      <div class="son"></div>
   </div>
   <a href="http://www.baidu.com"><span>百度</span></a>
   <form action="http://www.taobao.com">
      <input type="text">
      <input type="submit">
   </form>
```

```html
<style>
      *{
         margin: 0;
         padding: 0;
      }
      .father{
         width: 200px;
         height: 200px;
         background: red;
      }
      .son{
         width: 100px;
         height: 100px;
         background: blue;
      }
   </style>
   <script>
      $(function() {
         // 事件的命名空间,click.sxw 用以区分是谁编写了这部分代码
         // 想要让事件的命名空间生效的话必须满足两点:
         // 事件是通过on来绑定的
         // 通过trigger来触发
         $('.son').on("click.sxw",function() {
            alert('click1')
         })
         $('.son').on("click.kobe",function() {
            alert('click2')
         })
         $('.son').trigger('click.sxw')
      })
   </script>
</head>
<body>
   <div class="father">
      <div class="son"></div>
   </div>
   <a href="http://www.baidu.com"><span>百度</span></a>
   <form action="http://www.taobao.com">
      <input type="text">
      <input type="submit">
   </form>
```

```
注意: 
利用trigger触发子元素带命名空间的事件,那么父元素带有同样命名空间的事件也会被触发,而父元素没有命名空间的事件是不会被触发的;
利用trigger触发子元素不带命名空间的事件,那么子元素所有相同类型的事件和父元素所有相同类型的事件都会被触发
```

```html
<script>
      $(function() {
         $('button').click(function() {
            $('ul').append('<li>我是新增的li</li>')
         })
         //在jQ中,通过核心函数找到的元素不止一个,那么在添加事件的时候,jQ会遍历所有找到的元素,给所有找到的元素添加事件
         // 使用下述方式会导致新增的li是无法获取的,此时我们需要使用事件委托
         // $('ul>li').click(function() {
         //    console.log($(this).html()) 
         // })

         // 事件委托
         // 将li的click事件委托给ul取监听,因为当页面加载完成之后jQ回调函数是无法监听的
         // 在入口函数执行之前就有的元素 ul来监听动态添加的元素
         $('ul').delegate('li','click',function() {
            console.log($(this).html())
         })
      })
   </script>
</head>
<body>
   <ul>
      <li>我是第1个</li>
      <li>我是第2个</li>
      <li>我是第3个</li>
   </ul>
   <button>新增一个li</button>
</body>
```

## 事件委托

```html
<style>
        *{
            margin: 0;
            padding: 0;
        }
        html,body{
            width: 100%;
            height: 100%;
        }
        .mask{
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            position: fixed;
            top: 0;
            left: 0;
        }
        .login{
            width: 522px;
            height: 290px;
            margin: 100px auto;
            position: relative;
        }
        .login>span{
            width: 50px;
            height: 50px;
            /*background: red;*/
            position: absolute;
            top: 0;
            right: 0;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 编写jQuery相关代码
            $("a").click(function () {
                var $mask = $("<div class=\"mask\">\n" +
                    "    <div class=\"login\">\n" +
                    "        <img src=\"images/login.png\" alt=\"\">\n" +
                    "        <span></span>\n" +
                    "    </div>\n" +
                    "</div>");
                // 添加蒙版
                $("body").append($mask);
                $("body").delegate(".login>span", "click", function () {
                    // 移除蒙版
                    $mask.remove();
                });
                return false;
            });
        });
    </script>
</head>
<body>
<!--<div class="mask">-->
    <!--<div class="login">-->
        <!--<images src="images/login.png" alt="">-->
        <!--<span></span>-->
    <!--</div>-->
<!--</div>-->
<a href="http://www.baidu.com">点击登录</a>
<div>我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我是段落我</div>
</body>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>33-jQuery移入移出事件</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .father{
            width: 200px;
            height: 200px;
            background: red;
        }
        .son{
            width: 100px;
            height: 100px;
            background: blue;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 编写jQuery相关代码
            /*
            mouseover/mouseout事件, 子元素被移入移出也会触发父元素的事件
            */
            /*
            $(".father").mouseover(function () {
                console.log("father被移入了");
            });
            $(".father").mouseout(function () {
                console.log("father被移出了");
            });
            */

            /*
            mouseenter/mouseleave事件, 子元素被移入移出不会触发父元素的事件
            推荐大家使用
            */
            /*
            $(".father").mouseenter(function () {
                console.log("father被移入了");
            });
            $(".father").mouseleave(function () {
                console.log("father被移出了");
            });
            */

            /*
            $(".father").hover(function () {
                console.log("father被移入了");
            },function () {
                console.log("father被移出了");
            });
            */

            $(".father").hover(function () {
                console.log("father被移入移出了");
            });
        });
    </script>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>34-电影排行榜上</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 300px;
            height: 450px;
            margin: 50px auto;
            border: 1px solid #000;
        }
        .box>h1{
            font-size: 20px;
            line-height: 35px;
            color: deeppink;
            padding-left: 10px;
            border-bottom: 1px dashed #ccc;
        }
        ul>li{
            list-style: none;
            padding: 5px 10px;
            border: 1px dashed #ccc;
        }
        ul>li:nth-child(-n+3) span{
            background: deeppink;
        }
        ul>li>span{
            display: inline-block;
            width: 20px;
            height: 20px;
            background: #ccc;
            text-align: center;
            line-height: 20px;
            margin-right: 10px;
        }
        .content{
            overflow: hidden;
            margin-top: 5px;
        }
        .content>img{
            width: 80px;
            height: 120px;
            float: left;
        }
        .content>p{
            width: 180px;
            height: 120px;
            float: right;
            font-size: 12px;
            line-height: 20px;
        }
    </style>
</head>
<body>
<div class="box">
    <h1>电影排行榜</h1>
    <ul>
        <li><span>1</span>电影名称
            <div class="content">
                <img src="images/zl.jpg" alt="">
                <p>简介：故事发生在非洲附近的大海上，主人公冷锋（吴京 饰）遭遇人生滑铁卢，被“开除军籍”，本想漂泊一生的他，正当他打算这么做的时候，一场突如其来的意外打破了他的计划，突然被卷入了一场</p>
            </div>
        </li>
        <li><span>2</span>电影名称</li>
        <li><span>3</span>电影名称</li>
        <li><span>4</span>电影名称</li>
        <li><span>5</span>电影名称</li>
        <li><span>6</span>电影名称</li>
    </ul>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>35-电影排行榜下</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 300px;
            height: 450px;
            margin: 50px auto;
            border: 1px solid #000;
        }
        .box>h1{
            font-size: 20px;
            line-height: 35px;
            color: deeppink;
            padding-left: 10px;
            border-bottom: 1px dashed #ccc;
        }
        ul>li{
            list-style: none;
            padding: 5px 10px;
            border: 1px dashed #ccc;
        }
        ul>li:nth-child(-n+3) span{
            background: deeppink;
        }
        ul>li>span{
            display: inline-block;
            width: 20px;
            height: 20px;
            background: #ccc;
            text-align: center;
            line-height: 20px;
            margin-right: 10px;
        }
        .content{
            overflow: hidden;
            margin-top: 5px;
            display: none;
        }
        .content>img{
            width: 80px;
            height: 120px;
            float: left;
        }
        .content>p{
            width: 180px;
            height: 120px;
            float: right;
            font-size: 12px;
            line-height: 20px;
        }
        .current .content{
            display: block;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 编写jQuery相关代码
            /*
            // 1.监听li的移入事件
            $("li").mouseenter(function () {
                $(this).addClass("current");
            });
            // 2.监听li的移出事件
            $("li").mouseleave(function () {
                $(this).removeClass("current");
            });
            */
            $("li").hover(function () {
                $(this).addClass("current");
            }, function () {
                $(this).removeClass("current");
            });
        });
    </script>
</head>
<body>
<div class="box">
    <h1>电影排行榜</h1>
    <ul>
        <li><span>1</span>电影名称
            <div class="content">
                <img src="images/zl.jpg" alt="">
                <p>简介：故事发生在非洲附近的大海上，主人公冷锋（吴京 饰）遭遇人生滑铁卢，被“开除军籍”，本想漂泊一生的他，正当他打算这么做的时候，一场突如其来的意外打破了他的计划，突然被卷入了一场</p>
            </div>
        </li>
        <li><span>2</span>电影名称
            <div class="content">
                <img src="images/zl.jpg" alt="">
                <p>简介：故事发生在非洲附近的大海上，主人公冷锋（吴京 饰）遭遇人生滑铁卢，被“开除军籍”，本想漂泊一生的他，正当他打算这么做的时候，一场突如其来的意外打破了他的计划，突然被卷入了一场</p>
            </div>
        </li>
        <li><span>3</span>电影名称
            <div class="content">
                <img src="images/zl.jpg" alt="">
                <p>简介：故事发生在非洲附近的大海上，主人公冷锋（吴京 饰）遭遇人生滑铁卢，被“开除军籍”，本想漂泊一生的他，正当他打算这么做的时候，一场突如其来的意外打破了他的计划，突然被卷入了一场</p>
            </div>
        </li>
        <li><span>4</span>电影名称
            <div class="content">
                <img src="images/zl.jpg" alt="">
                <p>简介：故事发生在非洲附近的大海上，主人公冷锋（吴京 饰）遭遇人生滑铁卢，被“开除军籍”，本想漂泊一生的他，正当他打算这么做的时候，一场突如其来的意外打破了他的计划，突然被卷入了一场</p>
            </div>
        </li>
        <li><span>5</span>电影名称
            <div class="content">
                <img src="images/zl.jpg" alt="">
                <p>简介：故事发生在非洲附近的大海上，主人公冷锋（吴京 饰）遭遇人生滑铁卢，被“开除军籍”，本想漂泊一生的他，正当他打算这么做的时候，一场突如其来的意外打破了他的计划，突然被卷入了一场</p>
            </div>
        </li>
        <li><span>6</span>电影名称
            <div class="content">
                <img src="images/zl.jpg" alt="">
                <p>简介：故事发生在非洲附近的大海上，主人公冷锋（吴京 饰）遭遇人生滑铁卢，被“开除军籍”，本想漂泊一生的他，正当他打算这么做的时候，一场突如其来的意外打破了他的计划，突然被卷入了一场</p>
            </div>
        </li>
    </ul>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>36-TAB选项卡上</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 440px;
            height: 298px;
            border: 1px solid #000;
            margin: 50px auto;
        }
        .nav>li{
            list-style: none;
            width: 110px;
            height: 50px;
            background: orange;
            text-align: center;
            line-height: 50px;
            float: left;
        }
        .nav>.current{
            background: #ccc;
        }
        .content>li{
            list-style: none;
            display: none;
        }
        .content>.show{
            display: block;
        }
    </style>
</head>
<body>
<div class="box">
    <ul class="nav">
        <li class="current">H5+C3</li>
        <li>jQuery</li>
        <li>C语言</li>
        <li>Go语言</li>
    </ul>
    <ul class="content">
        <li class="show"><img src="images/11.jpg" alt=""></li>
        <li><img src="images/12.jpg" alt=""></li>
        <li><img src="images/13.jpg" alt=""></li>
        <li><img src="images/14.jpg" alt=""></li>
    </ul>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>37-TAB选项卡下</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 440px;
            height: 298px;
            border: 1px solid #000;
            margin: 50px auto;
        }
        .nav>li{
            list-style: none;
            width: 110px;
            height: 50px;
            background: orange;
            text-align: center;
            line-height: 50px;
            float: left;
        }
        .nav>.current{
            background: #ccc;
        }
        .content>li{
            list-style: none;
            display: none;
        }
        .content>.show{
            display: block;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 1.监听选项卡的移入事件
            $(".nav>li").mouseenter(function () {
                // 1.1修改被移入选项卡的背景颜色
                $(this).addClass("current");
                // 1.2获取当前移入选项卡的索引
                var index = $(this).index();
                // 1.3根据索引找到对应的图片
                var $li = $(".content>li").eq(index);
                // 1.4显示找到的图片
                $li.addClass("show");

            });
            // 1.监听选项卡的移出事件
            $(".nav>li").mouseleave(function () {
                // 1.1还原选项卡的背景颜色
                $(this).removeClass("current");
                // 1.2获取当前移出选项卡的索引
                var index = $(this).index();
                // 1.3根据索引找到对应的图片
                var $li = $(".content>li").eq(index);
                // 1.4隐藏对应的图片
                $li.removeClass("show");
            });

        });
    </script>
</head>
<body>
<div class="box">
    <ul class="nav">
        <li class="current">H5+C3</li>
        <li>jQuery</li>
        <li>C语言</li>
        <li>Go语言</li>
    </ul>
    <ul class="content">
        <li class="show"><img src="images/11.jpg" alt=""></li>
        <li><img src="images/12.jpg" alt=""></li>
        <li><img src="images/13.jpg" alt=""></li>
        <li><img src="images/14.jpg" alt=""></li>
    </ul>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>38-TAB选项卡终极</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 440px;
            height: 298px;
            border: 1px solid #000;
            margin: 50px auto;
        }
        .nav>li{
            list-style: none;
            width: 110px;
            height: 50px;
            background: orange;
            text-align: center;
            line-height: 50px;
            float: left;
        }
        .nav>.current{
            background: #ccc;
        }
        .content>li{
            list-style: none;
            display: none;
        }
        .content>.show{
            display: block;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            /*
            // 1.监听选项卡的移入事件
            $(".nav>li").mouseenter(function () {
                // 1.1修改被移入选项卡的背景颜色
                $(this).addClass("current");
                // 1.2获取当前移入选项卡的索引
                var index = $(this).index();
                // 1.3根据索引找到对应的图片
                var $li = $(".content>li").eq(index);
                // 1.4显示找到的图片
                $li.addClass("show");
            });
            // 1.监听选项卡的移出事件
            $(".nav>li").mouseleave(function () {
                // 1.1还原选项卡的背景颜色
                $(this).removeClass("current");
                // 1.2获取当前移出选项卡的索引
                var index = $(this).index();
                // 1.3根据索引找到对应的图片
                var $li = $(".content>li").eq(index);
                // 1.4隐藏对应的图片
                $li.removeClass("show");
            });
            */
            // 1.监听选项卡的移入事件
            $(".nav>li").mouseenter(function () {
                // 1.1修改被移入选项卡的背景颜色
                $(this).addClass("current");
                // 1.2还原其它兄弟选项卡的背景颜色
                $(this).siblings().removeClass("current");
                // 1.3获取当前移出选项卡的索引
                var index = $(this).index();
                // 1.4根据索引找到对应的图片
                var $li = $(".content>li").eq(index);
                // 1.5隐藏非当前的其它图片
                $li.siblings().removeClass("show");
                // 1.6显示对应的图片
                $li.addClass("show");
            });
        });
    </script>
</head>
<body>
<div class="box">
    <ul class="nav">
        <li class="current">H5+C3</li>
        <li>jQuery</li>
        <li>C语言</li>
        <li>Go语言</li>
    </ul>
    <ul class="content">
        <li class="show"><img src="images/11.jpg" alt=""></li>
        <li><img src="images/12.jpg" alt=""></li>
        <li><img src="images/13.jpg" alt=""></li>
        <li><img src="images/14.jpg" alt=""></li>
    </ul>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>39-jQuery显示和隐藏动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            background: red;
            display: none;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 编写jQuery相关代码
            $("button").eq(0).click(function () {
                // $("div").css("display", "block");
                // 注意: 这里的时间是毫秒
                $("div").show(1000, function () {
                    // 作用: 动画执行完毕之后调用
                    alert("显示动画执行完毕");
                });
            });
            $("button").eq(1).click(function () {
                // $("div").css("display", "none");
                $("div").hide(1000, function () {
                    alert("隐藏动画执行完毕");
                });
            });
            $("button").eq(2).click(function () {
                $("div").toggle(1000, function () {
                    alert("切换动画执行完毕");
                });
            });
        });
    </script>
</head>
<body>
<button>显示</button>
<button>隐藏</button>
<button>切换</button>
<div></div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>40-对联广告</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .left{
            float: left;
            position: fixed;
            left: 0;
            top: 200px;
        }
        .right{
            float: right;
            position: fixed;
            right: 0;
            top: 200px;
        }
        img{
            display: none;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 1.监听网页的滚动
            $(window).scroll(function () {
                // 1.1获取网页滚动的偏移位
                var offset = $("html,body").scrollTop();
                // 1.2判断网页是否滚动到了指定的位置
                if(offset >= 500){
                    // 1.3显示广告
                    $("img").show(1000);
                }else{
                    // 1.4隐藏广告
                    $("img").hide(1000);
                }
            });

        });
    </script>
</head>
<body>
<img src="images/left_ad.png" class="left">
<img src="images/right_ad.png" class="right">
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>41-jQuery展开和收起动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 300px;
            background: red;
            display: none;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 编写jQuery相关代码
        $("button").eq(0).click(function () {
            $("div").slideDown(1000, function () {
                alert("展开完毕");
            });
        });
        $("button").eq(1).click(function () {
            $("div").slideUp(1000, function () {
                alert("收起完毕");
            });
        });
        $("button").eq(2).click(function () {
            $("div").slideToggle(1000, function () {
                alert("收起完毕");
            });
        });
        });
    </script>
</head>
<body>
<button>展开</button>
<button>收起</button>
<button>切换</button>
<div></div>
</body>
</html>
```

