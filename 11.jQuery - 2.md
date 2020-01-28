# jQuery进阶

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>42-折叠菜单上</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .nav{
            list-style: none;
            width: 300px;
            margin: 100px auto;
            /*border: 1px solid #000;*/
        }
        .nav>li{
            border: 1px solid #000;
            line-height: 35px;
            border-bottom: none;
            text-indent: 2em;
            position: relative;
        }
        .nav>li:last-child{
            border-bottom: 1px solid #000;
            border-bottom-right-radius: 10px;
            border-bottom-left-radius: 10px;
        }
        .nav>li:first-child{
            border-top-right-radius: 10px;
            border-top-left-radius: 10px;
        }
        .nav>li>span{
            background: url("images/arrow_right.png") no-repeat center center;
            display: inline-block;
            width: 32px;
            height: 32px;
            position: absolute;
            right: 10px;
            top: 5px;
        }
        .sub>li{
            list-style: none;
            background: mediumpurple;
            border-bottom: 1px solid white;
        }
        .sub>li:hover{
            background: red;
        }
    </style>
</head>
<body>
<ul class="nav">
    <li>一级菜单<span></span>
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单<span></span></li>
    <li>一级菜单<span></span></li>
    <li>一级菜单<span></span></li>
    <li>一级菜单<span></span></li>
    <li>一级菜单<span></span></li>
    <li>一级菜单<span></span></li>
    <li>一级菜单<span></span></li>
</ul>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>43-折叠菜单下</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .nav{
            list-style: none;
            width: 300px;
            margin: 100px auto;
            /*border: 1px solid #000;*/
        }
        .nav>li{
            border: 1px solid #000;
            line-height: 35px;
            border-bottom: none;
            text-indent: 2em;
            position: relative;
        }
        .nav>li:last-child{
            border-bottom: 1px solid #000;
            border-bottom-right-radius: 10px;
            border-bottom-left-radius: 10px;
        }
        .nav>li:first-child{
            border-top-right-radius: 10px;
            border-top-left-radius: 10px;
        }
        .nav>li>span{
            background: url("images/arrow_right.png") no-repeat center center;
            display: inline-block;
            width: 32px;
            height: 32px;
            position: absolute;
            right: 10px;
            top: 5px;
        }
        .sub{
            display: none;
        }
        .sub>li{
            list-style: none;
            background: mediumpurple;
            border-bottom: 1px solid white;
        }
        .sub>li:hover{
            background: red;
        }
        .nav>.current>span{
            transform: rotate(90deg);
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 1.监听一级菜单的点击事件
            $(".nav>li").click(function () {
                // 1.1拿到二级菜单
                var $sub = $(this).children(".sub");
                // 1.2让二级菜单展开
                $sub.slideDown(1000);
                // 1.3拿到所有非当前的二级菜单
                var otherSub = $(this).siblings().children(".sub");
                // 1.4让所有非当前的二级菜单收起
                otherSub.slideUp(1000);
                // 1.5让被点击的一级菜单箭头旋转
                $(this).addClass("current");
                // 1.6让所有非被点击的一级菜单箭头还原
                $(this).siblings().removeClass("current");
            });
        });
    </script>
</head>
<body>
<ul class="nav">
    <li>一级菜单<span></span>
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单<span></span>
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单<span></span>
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单<span></span>
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单<span></span>
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单<span></span>
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单<span></span>
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单<span></span>
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
</ul>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>44-下拉菜单</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .nav{
            list-style: none;
            width: 300px;
            height: 50px;
            background: red;
            margin: 100px auto;
        }
        .nav>li{
            width: 100px;
            height: 50px;
            line-height: 50px;
            text-align: center;
            float: left;
        }
        .sub{
            list-style: none;
            background: mediumpurple;
            display: none;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            /*
            在jQuery中如果需要执行动画, 建议在执行动画之前先调用stop方法,然后再执行动画
            */
            // 1.监听一级菜单的移入事件
            $(".nav>li").mouseenter(function () {
                // 1.1拿到二级菜单
                var $sub = $(this).children(".sub");
                // 停止当前正在运行的动画：
                $sub.stop();
                // 1.2让二级菜单展开
                $sub.slideDown(1000);
            });
            // 2.监听一级菜单的移出事件
            $(".nav>li").mouseleave(function () {
                // 1.1拿到二级菜单
                var $sub = $(this).children(".sub");
                // 停止当前正在运行的动画：
                $sub.stop();
                // 1.2让二级菜单收起
                $sub.slideUp(1000);
            });

        });
    </script>
</head>
<body>
<ul class="nav">
    <li>一级菜单
        <ul class="sub">
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
            <li>二级菜单</li>
        </ul>
    </li>
    <li>一级菜单</li>
    <li>一级菜单</li>
</ul>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>45-jQuery淡入淡出动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 300px;
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
                $("div").fadeIn(1000, function () {
                    alert("淡入完毕");
                });
            });
            $("button").eq(1).click(function () {
                $("div").fadeOut(1000, function () {
                    alert("淡出完毕");
                });
            });
            $("button").eq(2).click(function () {
                $("div").fadeToggle(1000, function () {
                    alert("切换完毕");
                });
            });
            $("button").eq(3).click(function () {
                $("div").fadeTo(1000, 0.2, function () {
                    alert("淡入完毕");
                })
            });
        });
    </script>
</head>
<body>
<button>淡入</button>
<button>淡出</button>
<button>切换</button>
<button>淡入到</button>
<div></div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>46-弹窗广告</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .ad{
            position: fixed;
            right: 0;
            bottom: 0;
            display: none;
        }
        .ad>span{
            display: inline-block;
            width: 30px;
            height: 30px;
            position: absolute;
            top: 0;
            right: 0;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 1.监听span的点击事件
            $("span").click(function () {
                $(".ad").remove();
            });

            // 2.执行广告动画
            /*
            $(".ad").slideDown(1000, function () {
                $(".ad").fadeOut(1000, function () {
                    $(".ad").fadeIn(1000);
                });
            });
            */
            $(".ad").stop().slideDown(1000).fadeOut(1000).fadeIn(1000);

        });
    </script>
</head>
<body>
<div class="ad">
    <img src="images/ad-pic.png" alt="">
    <span></span>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>47-jQuery自定义动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
            margin-top: 10px;
            background: red;
        }
        .two{
            background: blue;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 编写jQuery相关代码
            $("button").eq(0).click(function () {
                /*
                $(".one").animate({
                    width: 500
                }, 1000, function () {
                    alert("自定义动画执行完毕");
                });
                */
                $(".one").animate({
                    marginLeft: 500
                }, 5000, function () {
                    // alert("自定义动画执行完毕");
                });
                /*
                第一个参数: 接收一个对象, 可以在对象中修改属性
                第二个参数: 指定动画时长
                第三个参数: 指定动画节奏, 默认就是swing
                第四个参数: 动画执行完毕之后的回调函数
                */
                $(".two").animate({
                    marginLeft: 500
                }, 5000, "linear", function () {
                    // alert("自定义动画执行完毕");
                });
            })
            $("button").eq(1).click(function () {
                $(".one").animate({
                    width: "+=100"
                }, 1000, function () {
                    alert("自定义动画执行完毕");
                });
            });
            $("button").eq(2).click(function () {
                $(".one").animate({
                    // width: "hide"
                    width: "toggle"
                }, 1000, function () {
                    alert("自定义动画执行完毕");
                });
            })
        });
    </script>
</head>
<body>
<button>操作属性</button>
<button>累加属性</button>
<button>关键字</button>
<div class="one"></div>
<div class="two"></div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>48-jQuery的stop和delay方法</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .one{
            width: 100px;
            height: 100px;
            background: red;
        }
        .two{
            width: 500px;
            height: 10px;
            background: blue;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 编写jQuery相关代码
            $("button").eq(0).click(function () {
                /*
                在jQuery的{}中可以同时修改多个属性, 多个属性的动画也会同时执行
                */
                /*
                $(".one").animate({
                    width: 500
                    // height: 500
                }, 1000);

                $(".one").animate({
                    height: 500
                }, 1000);
                */
                /*
                delay方法的作用就是用于告诉系统延迟时长
                */
                /*
                $(".one").animate({
                    width: 500
                    // height: 500
                }, 1000).delay(2000).animate({
                    height: 500
                }, 1000);
                */
                $(".one").animate({
                    width: 500
                }, 1000);
                $(".one").animate({
                    height: 500
                }, 1000);

                $(".one").animate({
                    width: 100
                }, 1000);
                $(".one").animate({
                    height: 100
                }, 1000);
            });
            $("button").eq(1).click(function () {
                // 立即停止当前动画, 继续执行后续的动画
                // $("div").stop();
                // $("div").stop(false);
                // $("div").stop(false, false);

                // 立即停止当前和后续所有的动画
                // $("div").stop(true);
                // $("div").stop(true, false);

                // 立即完成当前的, 继续执行后续动画
                // $("div").stop(false, true);

                // 立即完成当前的, 并且停止后续所有的
                $("div").stop(true, true);
            });
        });
    </script>
</head>
<body>
<button>开始动画</button>
<button>停止动画</button>
<div class="one" style="display: none"></div>
<div class="two"></div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>49-图标特效</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            list-style: none;
            width: 400px;
            height: 250px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        ul>li{
            width: 100px;
            height: 50px;
            margin-top: 50px;
            text-align: center;
            float: left;
            overflow: hidden;
        }
        ul>li>span{
            display: inline-block;
            width: 24px;
            height: 24px;
            background: url("images/bg.png") no-repeat 0 0;
            position: relative;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 1.遍历所有的li
            $("li").each(function (index, ele) {
                // 1.1生成新的图片位置
                var $url = "url(\"images/bg.png\") no-repeat 0 "+(index * -24)+"px"
                // 1.2设置新的图片位置
                $(this).children("span").css("background", $url);
            });
            
            // 2.监听li移入事件
            $("li").mouseenter(function () {
                // 2.1将图标往上移动
                $(this).children("span").animate({
                    top: -50
                }, 1000, function () {
                    // 2.2将图片往下移动
                    $(this).css("top", "50px");
                    // 2.3将图片复位
                    $(this).animate({
                        top: 0
                    }, 1000);
                });
            });
        });
    </script>
</head>
<body>
<ul>
    <li><span></span><p>百度</p></li>
    <li><span></span><p>百度</p></li>
    <li><span></span><p>百度</p></li>
    <li><span></span><p>百度</p></li>
    <li><span></span><p>百度</p></li>
    <li><span></span><p>百度</p></li>
    <li><span></span><p>百度</p></li>
    <li><span></span><p>百度</p></li>
</ul>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>50-无限循环滚动</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 600px;
            height: 161px;
            border: 1px solid #000;
            margin: 100px auto;
            overflow: hidden;
        }
        ul{
            list-style: none;
            width: 1800px;
            height: 161px;
            background: #000;
        }
        ul>li{
            float: left;
        }
    </style>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 0.定义变量保存偏移位
            var offset = 0;
            // 1.让图片滚动起来
            var timer;
            function autoPlay(){
                timer = setInterval(function () {
                    offset += -10;
                    if(offset <= -1200){
                        offset = 0;
                    }
                    $("ul").css("marginLeft", offset);
                }, 50);
            }
            autoPlay();

           // 2.监听li的移入和移出事件
            $("li").hover(function () {
                // 停止滚动
                clearInterval(timer);
                // 给非当前选中添加蒙版
                $(this).siblings().fadeTo(100, 0.5);
                // 去除当前选中的蒙版
                $(this).fadeTo(100, 1);
            }, function () {
                // 继续滚动
                autoPlay();
                // 去除所有的蒙版
                $("li").fadeTo(100, 1);
            });
        });
    </script>
</head>
<body>
<div>
    <ul>
        <li><img src="images/a.jpg" alt=""></li>
        <li><img src="images/b.jpg" alt=""></li>
        <li><img src="images/c.jpg" alt=""></li>
        <li><img src="images/d.jpg" alt=""></li>
        <li><img src="images/a.jpg" alt=""></li>
        <li><img src="images/b.jpg" alt=""></li>
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
    <title>51-jQuery添加节点相关方法</title>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            /*
            内部插入
            append(content|fn)
            appendTo(content)
            会将元素添加到指定元素内部的最后

            prepend(content|fn)
            prependTo(content)
            会将元素添加到指定元素内部的最前面


            外部插入
            after(content|fn)
            会将元素添加到指定元素外部的后面

            before(content|fn)
            会将元素添加到指定元素外部的前面

            insertAfter(content)
            insertBefore(content)
            */
            $("button").click(function () {
                // 1.创建一个节点
                var $li = $("<li>新增的li</li>");
                // 2.添加节点
                $("ul").append($li);
                $("ul").prepend($li);

                // $li.appendTo("ul");
                // $li.prependTo("ul");


                // $("ul").after($li);
                // $("ul").before($li);
                // $li.insertAfter("ul");
            });
        });
    </script>
</head>
<body>
<button>添加节点</button>
<ul>
    <li>我是第1个li</li>
    <li>我是第2个li</li>
    <li>我是第3个li</li>
</ul>
<div>我是div</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>52-jQuery删除节点相关方法</title>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            /*
            删除
            remove([expr])
            删除指定元素
            empty()
            删除指定元素的内容和子元素, 指定元素自身不会被删除
            detach([expr])
            */
            $("button").click(function () {
                // $("div").remove();
                // $("div").empty();
                // $("li").remove(".item");

                // 利用remove删除之后再重新添加,原有的事件无法响应
                // var $div = $("div").remove();
                // 利用detach删除之后再重新添加,原有事件可以响应
                var $div = $("div").detach();
                // console.log($div);
                $("body").append($div);
            });
            $("div").click(function () {
                alert("div被点击了");
            });
        });
    </script>
</head>
<body>
<button>删除节点</button>
<ul>
    <li class="item">我是第1个li</li>
    <li>我是第2个li</li>
    <li class="item">我是第3个li</li>
    <li>我是第4个li</li>
    <li class="item">我是第5个li</li>
</ul>
<div>我是div
    <p>我是段落</p>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>53-jQuery替换节点相关方法</title>
    <script src="../jQuery基础/js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            /*
            替换
            replaceWith(content|fn)
            replaceAll(selector)
            替换所有匹配的元素为指定的元素
            */
            $("button").click(function () {
                // 1.新建一个元素
                var $h6 = $("<h6>我是标题6</h6>");
                // 2.替换元素
                // $("h1").replaceWith($h6);
                $h6.replaceAll("h1");
            });
        });
    </script>
</head>
<body>
<button>替换节点</button>
<h1>我是标题1</h1>
<h1>我是标题1</h1>
<p>我是段落</p>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>54-jQuery复制节点相关方法</title>
    <script src="js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // clone([Even[,deepEven]])
            /*
            如果传入false就是浅复制, 如果传入true就是深复制
            浅复制只会复制元素, 不会复制元素的事件
            深复制会复制元素, 而且还会复制元素的事件
            */
            $("button").eq(0).click(function () {
                // 1.浅复制一个元素
                var $li = $("li:first").clone(false);
                // 2.将复制的元素添加到ul中
                $("ul").append($li);
            });
            $("button").eq(1).click(function () {
                // 1.深复制一个元素
                var $li = $("li:first").clone(true);
                // 2.将复制的元素添加到ul中
                $("ul").append($li);
            });

            $("li").click(function () {
                alert($(this).html());
            });
        });
    </script>
</head>
<body>
<button>浅复制节点</button>
<button>深复制节点</button>
<ul>
    <li>我是第1个li</li>
    <li>我是第2个li</li>
    <li>我是第3个li</li>
    <li>我是第4个li</li>
    <li>我是第5个li</li>
</ul>
</body>
</html>
```

## jQ类库

```
jQ 类库产生的原因:
	1.js兼容问题;
	2.随着js、css、Ajax等技术的不断进步,越来越多的开发者将一个又一个丰富多彩的程序功能进行封装,供其他开发者调用这些封装好的程序组件;
```

## 官网

```
中文官网: jQuery123.com
英文官网: jQuery.com
```

## 特征

```
1.轻量级;
2.强大的选择器;
3.出色的DOM操作封装;
4.可靠的事件处理机制;
5.出色的浏览器兼容性;
```

## 当文档加载完成之后调用函数

```
js:
    window.onload = function() {
        alert("林素素");
    }
jQ:
    $(document).ready(function() {
        console.log('林素素');
    })
// 把函数的句柄作为参数来使用
    function myFun() {
        console.log('lss');
    }
    $(document).ready(myFun);
```

```
jQuery对象就是通过jQuery包装DOM对象后产生的对象;
约定: 如果获取的是jQuery对象,那么要在变量前面加上$符;
var $name --> jQuery对象
var name --> DOM对象
```

```
<div>林素素</div>
console.log($("div").html()); //获取文本信息

对于一个原生DOM对象,只需用$()把DOM对象包装起来,就可以获得一个jQuery对象(jQuery对象就是通过jQuery包装DOM对象后产生的对象)。
例如: $(document.getElementById("msg")).html(); 这样转换之后就能调用jQuery的方法了;
```

```
两种办法把jQ对象转换成DOM对象:
1.通过索引获取dom对象:
	$("div")[1].innerHTML //调用js里的innerHTML属性,传参是索引,从0开始
2.通过get方法传递一个索引获取dom对象;
	$("div").get(1).innerHTML //传参是索引
```

```
//应用于所有的div
$("div").css({"width":"30px","height":"30px","border":"1px solid black"});
```

```
通配符选择器
$("body *").css("border","1px solid black"); //body下的所有标签
子选择器
$(".red > p").css("color","red"); //.red下面的子元素生效,注意不是后代都生效
```

```js
<bug 代码块>
定时器实现运动效果:
js:
<div class="demo"></div>
<button>move</button>
var oDome = document.getElementsByClassName('demo')[0],
	oButton = document.getElementsByTagName('button')[0],
	timer;
oButton.onclick = function() {
    timer = setInterval(function() {
        var l = oDome.offsetLeft + 10;
        if(l > 300) {
            oDome.style.left = '300px';
            clearInterval(timer);
        }else {
            oDome.style.lleft = l + 'px';
        }
    }, 10)
}
jQ:
$('button').click(function() {
    $('.demo').animate(left: 300);
})
```

```
DOM操作复杂: DOM元素的获取 给DOM元素添加属性 DOM元素的运动效果;
兼容问题: 浏览器宽高 事件event获取源 设置监听事件;
jQuery是一个高效,精简并且功能丰富的JavaScript工具库;
$('.wrapper').click(function() {
    $(this).html('hello world').css({width: 100,height: 100,backgroundColor: 'pink'})
})
```

```
jQuery
1.语法简单,开发高效;
2.文件够轻,短小精悍;
3.插件丰富,拓展性强;
4.浏览器支持性高;
```

```
CDN服务器上引用在线jQ资源
var oLi = document.getElementsByTagName('li')[0];
console.log( $(oLi) ); 可以填原生dom对象;

只要有索引,jQ就会把她放到自己的类数组里面:
	$({0: 'a', 1: 'b', length: 2})
	$(['a','b'])
jQ参数可以放原生dom对象,css选择器,函数,有索引值的数组类数组,空值(返回空的jQ对象)等;

$(function() {
    console.log( $('li').text() ); //在dom解析完成之后,jQ会自动把函数执行
})
$(document).ready(function() {
    console.log( $('li').text() );
})
```

```
$('#dom').text();
{
    0: dom,
    length: 1
}.text();
```

```
处理作用域问题:1.命名空间;2.闭包;
jQ无new操作:
<div id="dom"></div>
(function() {
    window.jQuery = window.$ = jQuery; //把内部的jQ暴露到全局下
    function jQuery(id) {
        return new jQuery.prototype.init(id);
    }
    var init = jQuery.prototype.init = function(id) {
        var dom = document.getElementById(id.slice(1)); //出去下面参数里的$
        this[0] = dom;
        this.length = 1;
        return this;
    }
    jQuery.prototype.text = function() {
        console.log('text');
        return this; //返回对象实现链式操作
    }
    jQuery.prototype.css = function() {
        console.log('css');
        return this;
    }
    init.prototype = jQuery.prototype; //把init的原型指向jQ的原型	
})();
console.log( $('#dom').text() ); //成功
---
jQ链式操作:
$('#dom').css('color','red').text();
```

```
jQuery Attribute
.text(); 获取匹配元素集合中所有的'文本内容',或设置匹配元素集合中每个元素文本内容为指定的内容;
.html(); 获取集合中第一个匹配元素的HTML内容,或设置每一个匹配元素的html内容;
.val(); 获取匹配元素集合中第一个元素的当前值或设置匹配元素集合中每个元素的值;
.prop(); 获取匹配元素集合中第一个元素的属性值或设置每一个匹配元素的一个或多个属性;
.removeProp(); 为集合中匹配的元素删除一个属性;
.attr(); 获取匹配元素集合中的第一个元素的属性值或设置每一个匹配元素的一个或多个属性;
.removeAttr(); 为匹配的元素集合中的每个元素中移除一个属性;
.addClass(); 为每个匹配的元素添加指定的样式类名; 
.removeClass(); 移除集合中每个匹配元素上一个,多个或全部样式;
.hasClass(); 确定任何一个匹配元素是否有被分配给定的类;
.toggleClass(); 在匹配的元素集合中的每个元素上添加或删除一个或多个样式类,取决于这个样式类是否存在或值切换属性,即:如果存在(不存在)就删除(添加)一个类;
```

```
$('#btn').click(function() {
    $('p').html('一年三班<b>流川枫</b>');
})

innerHTML --对应--> html

innerHTML 获取当前dom元素下所有的内容
innerText 只获取dom元素下文本内容

<select name="list" id="list">
	<option value="cute">可爱</option>
	<option value="tender" selected>温柔</option>
	<option value="no-reason">毫无理由</option>
</select>
<button id="btn">btn</button>
$('#btn').click(function() {
    $('select').val(['no-reason']);
})

<div id="test" lecturer="sxw"></div>
$('div').attr('id') -->
$('div').prop('id') --> 两个都可以获取到
$('div').attr('lecturer') --> 可以获取到
$('div').prop('lecturer') --> 自定义的属性获取不到
```

```
属性:
1.固有属性,特性: id class title href src alt type value
2.自定义属性,新增属性:
	attr万能属性获取 -->对应js中--> attribute
	prop只能获取固有属性,特性 -->对应js中--> 通过.的方式操作对象

$('div').attr('id','test');
$('div').prop('id','test');

$('div').attr({
    id: 'test',
    lecturer: 'sxw'
});
$('div').prop({
    id: 'test',
    lecturer: 'sxw' //设置不上
});

取值:
$('div').attr('id');

$('div').attr('class'); //如果没有class属性,返回undefined;
$('div').prop('class'); //同理,返回空值

attr获取未设置的属性,均返回undefined;
prop获取未设置的特性,返回空;
```

```
复选框demo
ul{
    margin: 0;
    padding: 0;
    width: 200px;
}
li{
    list-style: none;
    background: #eee;
}
.title{
    background: #ccc;
    border-radius: 4px;
    font-size: 20px;
    margin: 0;
}
<ul>
	<h3>选出你喜欢的明星？</h3>
    <ul class="sm">
        <li class="title">
            <input type="checkbox">
            <span>小奶狗</span>
        </li>
        <li>
            <input type="checkbox">
            <span>蔡徐坤</span>
        </li>
        <li>
            <input type="checkbox">
            <span>刘昊然</span>
        </li>
        <li>
            <input type="checkbox">
            <span>TF boys</span>
        </li>
    </ul>
</ul>
$('.title input').change(function() {
    console.log($('.title input').prop('checked')); //只能使用prop,返回值是true/false
    if($('.title input').prop('checked')) {
        $('ul input').prop('checked',true);
    }else {
        $('ul input').prop('checked',false);
    }
})
```

```
移除
$('ul').removeAttr('class');
添加
$('ul').attr('bbb','bbb');

removeProp只能删除prop();设置上的自定义属性;

$('ul').prop('aaa','aaa'); //我们设置上了但是并没有对应到html里面的dom,但是确实是存在的
console.log($('ul').prop('aaa')); //可以打印出来
$('ul').removeProp('aaa');
console.log($('ul').prop('aaa')); //可以清除,但是当我们把aaa换做id,发现我们清除不掉;

$('div').addClass('text').removeClass('text'); //添加class和移除class

console.log($('div').text('div'));

$('div').addClass(function(index, className) {
    returm 'test';
});

$('li').addClass(function(index) {
    if(index % 2 == 0) {
        return 'active test'; //即设置active属性,又设置test属性,属性之间空格分开
    }
});
```

```html
小demo
body, ul, li, p{
    padding: 0;
    margin: 0;
}
li{
    list-style: none;
}
.item{
    width: 300px;
    margin: 10px;
    background: #ededed;
    border-radius: 10px;
}
p{
    padding: 10px 0;
    margin: 0 10px;
}
span{
    float: right;
}
button{
    margin: 10px;
    padding: 10px;
    border-color: #23a1ec;
    color: #23a1ec;
    border-radius: 10px;
    outline: none; //按钮外边框的颜色取消掉
    background: #fff;
}
.active{
	color: #fff;
	background: #23a1ec;
}
<ul>
	<li class="item">
		<p>
			枸杞
			<span>￥300</span>
		</p>
	</li>
	<li class="item">
		<p>
			可乐
			<span>￥200</span>
		</p>
	</li>
	<li class="item">
		<p>
			红枣
			<span>￥100</span>
		</p>
	</li>
	<li class="item">
		<p>
			菊花
			<span>￥100</span>
		</p>
	</li>
	<li class="item">
		<p>
			啤酒
			<span>￥300</span>
		</p>
	</li>
	<li class="item">
		<p>
			咖啡
			<span>￥100</span>
		</p>
	</li>
</ul>
$('.item').click(function() {
    if($(this).attr('class') == 'item active') {
        $(this).removeClass('active');
    }else {
        $(this).addClass('active');
    }
})
简化使用hasClass属性进行简化代码:
$('.item').click(function() {
    if( $(this).hasClass('active')) {
        $(this).removeClass('active');
    }else {
        $(this).addClass('active');
    }
})
再进行简化,使用toggleClass(),判断你有没有这个class属性,没有就自动加上,有就自动删除
$('.item').click(function() {
    $(this).toggleClass('active');
})
```

## jQuery CSS

```
css(); 获取匹配元素集合中的第一个元素的样式属性的值或设置每个匹配元素的一个或多个CSS属性;
width(); 为匹配的元素集合中获取第一个元素的当前计算宽度值或给每个匹配的元素设置宽度;
innerWidth(); 为匹配的元素集合中获取第一个元素的当前计算宽度值,包括padding,但是不包括border;
outerWidth(); 获取元素集合中第一个元素的当前计算宽度值,包括padding,border和选择性的margin;
offset(); 在匹配的元素集合中,获取的第一个元素的当前坐标,或设置每一个元素的坐标,坐标相对于文档;
position(); 获取匹配元素中第一个元素的当前坐标,相对于offset parent的坐标,offset parent指离该元素最近的而且被定位过的祖先元素;
scrollLeft(); 获取匹配的元素集合中第一个元素的当前水平滚动条的位置或设置每个匹配元素的水平滚动条位置;
scrollTop(); 获取匹配的元素集合中第一个元素的当前垂直滚动条的位置或设置每个匹配元素的垂直滚动条位置;
```

```html
<div class="one">abc</div>
$('.one').click(function() {
    console.log( $(this).css(['width','background'])); //数组的形式获取多个属性
})
$('.one').click(function() {
    $(this).css('width','100px'); //改变css样式
})
$('.one').click(function() {
    $(this).css({
        width: 200,
        height: 200
    });
})
$('.one').click(function() {
    $(this).css({
        width: '+=100'
    });
})
```

```css
获取css值:
css(string)|css({})

设置css值:
css(string,value)|css({})

$('.one').css('width') //'100px'
$('.one').width() //100 number
$('.one').css('width', 200)
$('.one').width(200)

$('.one').width() //获取content宽度部分
$('.one').innerWidth() //获取content + padding
$('.one').outerWidth() //获取content + padding + border
$('.one').outerWidth(true) //获取content + padding + border + margin

改变innerWidth和outerWidth只会改变content的宽度
```

```html
.wrapper{
    position: relative;
    left: 100px;
    top: 100px;
    width: 300px;
    height: 300px;
    background: #ccc;
}
.one{
    position: absolute;
    left: 100px;
    top: 100px;
    width: 100px;
    height: 100px;
    background: red;
}
<div class="wrapper">
	<div class="one">div</div>
</div>
console.log($('.one').offset()); //208 border默认8px,获取的值相对于文档而言
console.log($('.one').position()); //相对于最近的有定位的父级的位置
---
$('.one').offset({
    left: 50,
    top: 50
});
---
body{
    width: 800px; //出现横向滚动条
}
$(document).scrollTop(); //纵向滚动条
$(document).scrollLeft(); //横向滚动条
$(document).scrollTop(100);
---
制作阅读器:
body{
	margin: 0;
    width: 800px;
}
var newTop;
var timer = setInterval(function() {
    if( $(document).scrollTop() + $(window).height() >= $('body').height() ) {
        clearInterval(timer);
        console.log('clear');
    }else {
        console.log('timer');
        newTop = $(document).scrollTop();
        $(document).scrollTop( newTop + 2);
    }
}, 100)
```

## jQuery DOM Traversing

```
筛选:
:odd 选择索引值为奇数元素,从0开始计数;
:even 选择索引值为偶数元素,从0开始计数;
:first 选择第一个匹配的元素;
:last 选择最后一个匹配的元素
:eq() 在匹配的集合中选择索引值为index的元素;
.prev() 取得一个包含匹配元素集合中每一个元素紧邻的前一个同辈元素的元素集合,选择性筛选的选择器;
.prevAll() 获得集合中每个匹配元素的所有前面的兄弟元素,选择性筛选的选择器;
.next() 取得匹配的元素集合中每一个元素紧邻的后面同辈元素的元素集合;
.nextAll() 获得每个匹配元素集合中所有下面的同辈元素,选择性筛选的选择器;
.siblings() 获得匹配元素集合中每个元素的兄弟元素,可以提供一个可选的选择器;
.filter() 筛选元素集合中匹配表达式或通过传递函数测试的那些元素集合;
:not() 选择所有元素去除不匹配给定的选择器的元素;
.is() 判断当前匹配的元素集合中的元素是否为一个选择器,DOM元素或者jQuery对象,如果这些元素至少一个匹配给定的参数,那么返回true;
.slice() 根据指定的下标范围,过滤匹配的元素集合,并生成一个新的jQuery对象;
.map() 通过一个函数匹配当前集合中的每个元素,产生一个包含新的jQuery对象;
:has() 选择元素其中至少包含指定选择器匹配的一个种元素;
```

```css
查找:
children/find/end/add/andBack
offsetParent/parent/parents/closest
---
<style>
	ul{
        margin: 0;
        width: 200px;
	}
	li{
        list-style: none;
        padding: 10px;
        bacjground: #eee;
        margin: 10px 0;
	}
</style>
<ul>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
</ul>
$('li:first').css('background','orange');
$('li').first().css('background','orange');
$('li:last').css('background','orange');
$('li:odd').css('background','orange'); //索引值是奇数
$('li:even').css('background','orange');
$('li:eq(2)').css('background','orange'); //找第三个li
$('li').eq(-1).css('background','orange');
$('li:eq(2)').prev().css('background','orange'); //找到第三个li,然后找上一个兄弟元素
$('li:eq(2)').prevAll().css('background','orange'); //第三个元素前面所有的兄弟元素
$('li:eq(2)').next().css('background','orange'); //下一个
$('li:eq(2)').nextAll().css('background','orange'); //下一个所有的元素
$('li').prev().css('background','orange'); //除了第一个下面都生效
$('li').prevAll().css('background','orange'); //除了第五个上面的都生效
$('li:eq(2)').siblings().css('background','orange'); //除了自己以外的所有兄弟元素
$('li').siblings().css('background','orange'); //全部生效
```

```css
<ul>
	<li class="text">1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
	<li>5</li>
</ul>
$('li').filter(function(index, ele) {
    //if(index % 2 == 0) {
    //    return true;
    //}
    //return index % 2 == 0? true : false;
    return index % 2 ==0;
}).css('background', 'orange')
找class名是text的,设置上属性
---
不要class名是text的全部设置上css样式
$('li').not('.text'),css('background','orange');
--
$('li').is('.text'); 这一堆li中class名有没有text的,返回true
---
直接找3和4,取头不取尾
$('li').slice(2,4).css('background','orange'); slice里面只填一个参数的时候就是后面的全取
---
ul{
    margin: 0;
}
li{
    list-style: none;
    width: 200px;
    padding: 10px;
    margin: 10px;
    background: #eee;
}
<ul>
	<li>
		<span class="name">sxw</span>
		<span class="age">18</span>
	</li>
	<li>
		<span class="name">kobe</span>
		<span class="age">10</span>
	</li>
	<li>
		<span class="name">lss</span>
		<span class="age">25</span>
	</li>
</ul>
var arr = [];
遍历li获取名字 each循环遍历dom元素
$('li').each(function(index, ele){
    //ele和this指向的都是li对象
    arr.push($(ele).find('.name').text());
})
console.log(arr.join(','));
---
$('li').map(function(index, ele) {
    if($(ele).find('.age').text() > 15) {
        return ele;
    }
}).css('background','orange');
---
<ul>
	<li>
		<p>Person DAta</p>
	</li>
	<li>
		<span class="name">sxw</span>
		<span class="age">18</span>
	</li>
	<li>
		<span class="name">kobe</span>
		<span class="age">10</span>
	</li>
	<li>
		<span class="name">lss</span>
		<span class="age">25</span>
	</li>
</ul>
$('li').has('p').css('color','red');
---
parent --> 找直接父元素
$('span').parent('li'); //找span的直接父元素是li的dom结构
--
parents --> 找祖先元素
$('span').parents('li'); //找祖先中li的元素
---
closest ---> 找离他最近的筛选元素
$('span').closest('div'); 
$('.item').closest('div'); //从自身开始找
--
offsetParent --> 找离自己最近的有定位的父级
$('.item').offsetParent(); //无参数
---
children() --> 获取子元素,不是后代元素,里面可以填参数
---
find() --> 前面父元素.后面要查找的子元素,即可获取子元素,找后代元素
---
find(); --> 会返回prevObject
$('ul').find('li:eq(3)').css('color','red').prevObject.find('li:eq(2)').css('color','blue'); //我们想回退去找第二个对象并设置上属性,这里preveObject===end();两个替换效果相同
--
第一个li和最后一个li同时设置上颜色
$('li:first').add('li:last').css('background','orange');
--
addBack();
选中第二个li设置样式
$('li:eq(1)').css('clolor','yellow').nextAll().css('color','blue').addBack().click(function() {
    consoel.log($(this).text())
})
```

## 项目

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>demo</title>
        <style>
            .wrapper{
                position: relative;
                height: 25px;
                width: 77px;
                overflow: hidden;
                z-index: 1;
            }
            .wrapper a{
                position: absolute;
                top: 0;
                height: 23px;
                width: 17px;
                border: 1px solid #e5e5e5;
                line-height: 23px;
                text-decoration: none;
                text-align: center;
                background: #f0f0f0;
                color: #444;
            }
            .wrapper .prev{
                left: 0;
            }
            .wrapper .next{
                right: 0;
            }
            .wrapper .text{
                width: 39px;
                height: 15px;
                line-height: 15px;
                border: 1px solid #aaa;
                color: #343434;
                text-align: center;
                padding: 4px 0;
                background-color: #fff;
                background-position: -75px -375px;
                position: absolute;
                z-index: 2;
                left: 18px;
                top: 0;
            }
            .wrapper .min{
                color: #e5e5e5;
            }
        </style>
    </head>
    <body>
        <div class="wrapper">
            <a href="#" class="prev">-</a>
            <input type="text" value="1" class="text">
            <a href="#" class="next">+</a>
        </div>
        <script src="./jquery-3.3.1.js"></script>
        <script>
            var value = 1; 
            function notIn() {
                if(value <= 1) {
                    $('.prev').addClass('min');
                }else if(value >= 5) {
                    $('.next').addClass('min');
                }else {
                    $('.min').removeClass('min');
                }
            }
            notIn();
            function cont(num) {
                value = parseInt( $('.text').val() ) + num; 
                if(value <= 1) {
                    value = 1;
                }else if(value >=5) {
                    value = 5;
                } 
                $('.text').val(value);
            }

            // $('.wrapper').find('.prev').click(function() {
            //     cont(-1);
            //     notIn();

            // }).end().find('.next').click(function() {
            //     cont(1);
            //     notIn();
            // })

            $('.prev').add('.next').click(function () {
                if($(this).hasClass('prev')) {
                    cont(-1);
                }else {
                    cont(1);
                }
                notIn();
            })


        </script>
    </body>
</html>
```

## DOM运动

```
<div class="demo"></div>
<button>move</button>
var oDemo = document.getElementsByClassName('demo')[0],
	oButton = document.getElementsByTagName('button')[0],
	timer;
	oButton.onclick = function() {
        timer = setInterval(function() {
            var l = oDemo.offsetLeft + 10;
            if(l > 300) {
                oDemo.style.left = '300px';
                clearInterval(timer);
            }else {
                oDemo.style.left = l + 'px';
            }
        }, 10)
	}
```

## 引用jQ的方式

```
离线: Download the uncompressed,development jQuery 3.3.1
在线: To see all available files and version,visit https://code.jquery.com ; jQuery 3.x uncompressed , 复制粘贴跳转出来的<script></script>里面的src部分到我们编辑的html里面
```

```
jQuery('#dom') 获取DOM 
text() 获取文本
jQuery('#dom').text() 获取id为dom的标签里面的文本内容
jQuery里面可以放css选择器,原生dom对象,还可以放数组,类数组(有下标索引值),函数(jQ会把函数自动执行了),空值(null undefined),其他(jQ会把我们填的数放到类数组第0位);
```

```js
DOM文档解析之后执行:
$(function() {
    console.log($('li').text())
})
$(document).ready(function() {
    console.log($('li').text())
})
```

```html
<div id="dom">div</div>
(function() {
    function jQuery(id) {
    	var dom = document.getElementsById(id,slice(1));
        console.log(dom);
    }
    window.jQuery = window.$ = jQuery // 暴露出去
})();
$('#dom');
```

```js
jQuery源码是怎样实现无new操作
(function() {
	function jQuery(id) { //通过外面套了一层函数
        return new init(id);
	}
    function init(id) {
    	var dom = document.getElementsById(id,slice(1));
    	this[0] = dom;
    	this.length = 1;
        return this;
    }
    jQuery.prototype.text = function() {
        console.log('text');
    }
    jQuery.prototype.css = function() {
        console.log('css');
    }
    init.prototype = jQuery,prototype; //把jQ原型赋给init函数,那么通过init new出来的对象就能与jQuery公用原型了
    window.jQuery = window.$ = jQuery //暴露出去
})();
console.log( $('#dom') );
```

## jQuery链式操作

```
$('dom').css('color','red').text();
实现,通过return this,保证了连续.调用的时候依旧是jQuery对象
```

## jQuery四点操作

```
1.如何全局使用jQuery;
2.无new操作;
3.init原型;
4.链式操作;
```

```css
jQuery Attribute
text(),
html(), 
val()，
prop(), removeProp()
attr(), removeAttr()
addClass(), removeClass()
hasClass(), toggleClass()
```

```css
$('#btn').click(function() {
    $('p').html('我叫<b>舒小伟</b>');
})
//innerHTML --> html();
innerHTML在jQ里面对应的就是html();
text与html关于取文本内容的区别,text是会取所有内容的,而html只会取第一个元素的内容忽略下面元素的内容;
innerHTML与innerText之间的区别,innerHTML是会返回当前元素里面所有的包括内容以及标签的所有信息,innerText只会返回当前元素里面的文本信息;
//text() 取值赋值均一组
//html() 取值取第一个,赋值赋一组
//val() 取值取第一个,赋值赋一组
```

```css
属性:
1.固有属性(特性): id class title href src alt type value
2.新增属性: 自定义的属性
<div id="test" class="asd" lecturer="sxw"></div>
console.log( $('div').attr('id') )
console.log( $('div').prop('id') );
console.log( $('div').attr('lecturer') )
console.log( $('div').prop('lecturer') ); //获取不了
只要是属性attr都能获取,prop只能获取固有属性;
attr是通过setAttribute实现;取值取一个,赋值赋值一组
prop是通过div.className实现;取值取一个,赋值赋值一组
attr获取为设置的属性,均返回undefined;prop获取未设置的特性,返回空
<div>div</div>
$('div').attr('id','test');
$('div').attr({ //循环遍历每一个div,并且把attr里面的对象循环遍历依次插入到每一个div中
    id : 'test',
    lecturer : 'sxw'
})
```

