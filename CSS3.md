# `CSS`引入方式

```
内部样式表; <style>...</style>
外部样式表; <link rel="stylesheet" href="./index.css">
行间样式; <div style="color: red;">sxw</div>
```



# 文档流

```
文档流: 默认情况下,浏览器中页面结构排列方式为从上到下,从左到右;
脱离文档流: 不受文档流排列控制;
三种脱离文档流的方法:
	1.position: absolute; (脱离文档流相对于最近有定位的父级进行定位)
	2.position: fixed; (脱离文档流相对于窗口进行定位)
	3.float: left;
```



# `CSS`属性前缀

```
兼容各个浏览器;
	1.-moz- ,兼容火狐等使用 Mozilla 浏览器引擎;
	2.-webkit- ,兼容safari/谷歌浏览器等Webkit引擎;
	3.-o- ,兼容Opera浏览器引擎;
	4.-ms- ,兼容Internet Explorer浏览器引擎;
```

```css
例子: border: 1px solid #000;
     (-moz-border: 1px solid #000;)
     (-webkit-border: 1px solid #000;)
     (-o-border: 1px solid #000;)
     (-ms-border: 1px solid #000;)
```



# `CSS`选择器

## 属性选择器

```css
input[type = 'text'] type属性值 === ‘text’,元素才生效;
input[type ~= 'text'] type属性可以有多个值,text为其中一个即可生效样式;
input[type ^= 'text'] type属性值以'text'开头的元素生效;
input[type $= 'text'] type属性值以'text'结尾的元素生效;
input[type *= 'text'] type属性值包含'text'的元素生效;
(举一反三: 此处的type也可以是class等;'text'也可以是一个字母'x'等;学会灵活贯通)
```

```html
<style>
    input[type = 'text'] { // input中type值必须严格等于'text'才生效;
        border: 1px solid red;
    }
</style>
<body>
    <form action="#">
        <input type="text"> // 生效
        <input type="password">
    </form>
</body>
```

## 兄弟选择器

```
E+F 找当前元素紧挨着的后一个元素;
E~F 找当前元素后面只要有对应的元素,都生效;
```

## 伪类选择器

```
:not(s) 不含有s选择符的元素;

:first-child 匹配父元素的第一个子元素;
:last-child 匹配父元素的最后一个子元素;
:nth-child(n) 匹配父元素第n个'同类'子元素;
:nth-last-child(n) 匹配父元素倒数第n个'同类'子元素生效;

:first-of-type;
:last-of-type;
:nth-of-type(n); 匹配父元素第n个子元素,若第n个子元素不是同类型,则继续往下找;
:nth-last-of-type;

:empty; 找内容是空的那个元素;

:enable 匹配表单中激活的元素;
:disabled 匹配表单中禁用的元素,无法获得焦点,不能使用了;
             在input标签里添加disabled属性就会禁用输入框;
:checked 匹配表单中被选中的radio(单选按钮)或checkbox(复选框)元素;
:target 只要选中当前元素立即生效样式效果;
::selection 匹配当前选中的元素;
:root 根元素;
```

```html
<style>
    li:not(.first) {
        color: pink;
    }
</style>
<body>
    <ul>
        <li class="first">1</li>
        <li>2</li> //生效
        <li>3</li> //生效
    </ul>
</body>
```

```html
<style>
    li:first-child {
        color: yellow;
    }
</style>
<body>
    <li>0</li>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
    <li>4</li>
</body>
// 0,1生效;
// 若把first改成last则4,3生效;
```

```html
<style>
    li:nth-child(2) {
        color: yellow;
    }
</style>
<body>
    <li>0</li>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
    <li>4</li>
</body>
// 2生效
```

```html
<style>
    li:nth-of-type(2) { //比较灵活
        color: pink;
    }
</style>
<body>
    <li>0</li>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
    <li>4</li>
</body>
// 2,4生效
```

```html
<style>
    li::before {
        content: 'sxwsxw...'; //就算在这里加文本内容对结果也不影响;
    }
    li:empty {
        width: 100px;
        height: 30px;
        border: 1px solid #000;
    }
</style>
<body>
    <li>0</li>
    <ul>
        <li>1</li>
        <li>2</li>
        <li></li> //生效
    </ul>
</body>
```

```css
body{
    user-select: none; // 设置用户不能选中文字
}
```

```css
:root和写html的效果是一样的
:root{

}
html{

}
```

```html
<style>
    .nav a:target{ // :target表示只要选中当前元素立即生效样式效果;
        background: pink;
        color: red;
    }
</style>
<body>
    <div class="nav">
        <a href="#sxw" id="sxw">pxt</a> //通过'#sxw'与id'sxw'相对应;
    </div>
</body>
```

## 伪元素选择器

```
精确到文本内容,给个别元素设置样式:
	:first-letter 设置对象内的第一个字符样式
	:first-line 设置对象内第一行样式
	:before 
	:after 设置对象文本前后进行操作
	::selection 设置对象被选择时的元素样式
```

```html
<style>
    input[class ^= 'k'] + p:first-letter {
        color: pink;
    }
</style>
<form action="#">
    <input type="text" class="kobe input first">
    <p>舒小伟</p> // 舒字变pink
    <p>sxw</p>
    <input type="text" class="pxt">
    <p>kobe</p>
</form>
```

```html
<style>
    p{
        display: inline-block;
        width: 138px;
    }
    input[class ^= 'n'] + p:first-line{ // input中class以'n'开头的紧挨着的p标签中第一行文字变红
        color: red;
    }
</style>
<body>
    <form action="#">
        <input type="text" class="name">
        <p>你在我心中是最美,每一个动作都让人沉醉,你的坏,你的好,都是我眼中最美的诶为,你在我心中是..</p>
        <input type="text">
        <p>我又回来了！</p>
    </form>
</body>
```

```html
before/after是伪元素,可以给后面添加一个内容元素;
<style>
    input[class ^= 'k'] + p:before { //在舒小伟前面加上了 sxw
        content: 'sxw'
    }
</style>
<body>
    <form action="#">
        <input type="text" class="kobe input first">
        <p>舒小伟</p>
        <p>一年三班</p>
        <input type="text" class="pxt">
        <p>流川枫</p>
    </form>
</body>
```



# `border-radius`

```css
border-radius: 50%;	圆
border-radius: 10px;
border-radius: 10px 10px 10px 10px;(左上右上 右下左下)顺时针
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px;
    border-bottom-left-radius: 10px;
```



# `box-shadow`

```css
box-shadow 盒子阴影
box-shadow: 10px 10px 3px 10px rgba(0, 0, 0, 0.5) inset;
box-shadow: [x向左挪动] [y向右挪动] [模糊半径] [阴影拓展半径] [阴影颜色] [投影方式];
																  inset 向内投影;
text-shadow 文字阴影
text-shadow: x y [模糊半径] [阴影颜色];
```



# `rgba`和`opacity`

```
在给父级设置opacity时,子元素默认继承opacity;
但是rgba不会出现这种情况;
```



# 线性渐变

```css
linear-gradient 线性渐变:
background: linear-gradient(direction, color[percent], color[percent], ...);
                            direction: 渐变方向;
                                写方向: to bottom/to bottom right...;
                                写角度: 0deg/45deg;
                                        color: 渐变颜色;
                                        percent: 百分比;
background: linear-gradient(to top right, yellow, red);
background: linear-gradient(90deg, yellow 50%, green 90%, red);
从90度位置开始渐变,前百分之50是黄色,百分之50到百分之90是绿色,百分之90到百分之100是红色;
```



# 径向渐变

```css
radial-gradient 径向渐变:
background: radial-gradient(shape r/(a,b) at position color[percent], color[percent]);
    						shape: circle/ellipse椭圆;
    							  r/(a,b): 半径/(长短轴)
    									  at position: 中心点位置
									像素值/百分比(50%, 50%)/top left/默认情况下另一个值center;
```



# `background`属性

```css
新增background值:
background-origin 改变指定绘制背景图像时的起点
background-origin: border-box|padding-box|content-box;
	border-box 从左上角开始引入图片,其他位置进行平铺;
	padding-box 从padding的位置开始引入图片,其他位置进行平铺;(默认状态)
	content-box 从content区域引入图片,其他位置进行平铺;
* * *
background-clip 指定背景显示的范围,裁剪
background-clip: border-box(默认)|padding-box|content-box;
* * *
background-size 指定背景中图像的尺寸
background-size: auto|length|percentage|cover|contain
contain: 让盒子至少保留一张完整背景图片
* * *
让一张图片充满body:
body{
    background-image: url('./ck.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    background-position: 0 50%;
	//水平方向不动,垂直方向移动到原来的百分之50进行调整
}
.box{
    width: 200px;
    height: 200px;
    padding: 50px;
    border: 50px solid rgba(189, 178, 234, 0.5);
    background-image: url('sxw.jpg');
    background-origin: border-box;
    background-repeat: no-repeat;
    background-size: 100% 100%;
}
```



# `border`属性

```css
新增border值:
给border添加背景图片
border-image: url number style;
    url: 图片地址;
    number: 图片剪裁的值;
    style: 图片添加的方式;
    例如: 花边效果; border: 10px solid rgba(0, 0, 0, 0.5);
                  border-width: 10px;
                  border-style: solid;
                  border-color: rgba(0, 0, 0, 0.5);
```



# 画一个三角形

```css
.box{
    width: 0px;
    height: 0px;
    border: 50px solid transparent;
    border-width: 50px;
    border-style: solid;
    border-top-color: rgba(189, 178, 234, 0.8);
}
```



# 给border添加背景图片

```css
.box{
    width: 300px;
    height: 300px;
    padding: 50px;
    border-width: 50px;
    border-style: solid;
    background-image: url('ck.jpg');
    border-image: url('./border.png') 27 repeat;
    // 27表示一个小四边形的高度是27px,会把裁剪出来的图片放在四个顶点上;
    // repeat表示除了四个顶点外的其他边上图形呈现的形式,repeat是从中间向两边重复平铺,平铺到哪算哪,超出的地方都裁掉;
    // 默认是stretch平铺:拉伸平铺,一个图像他会拉伸到撑满四个边;
    // round表示保留完整的图片进行重复;
}
```



# 文字属性

```css
1.text-overflow: clip | ellipsis | ellipsis-word
	clip: 不显示省略标记(...),而是简单裁切;
	ellipsis: 当对象文本溢出时显示省略标记(...),省略标记插入的位置是最后一个字符;
	(上面两个属性使用的时候要加上overflow: hidden;)
2.white-space: nowrap;
	文本不会换行,直到遇到<br>标签为止;
3.单行打点:
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;
4.多行打点:
    -webkit-line-clamp: 2; //2行打点
    text-overflow: ellipsis; //处理文字溢出的方式
    display: -webkit-box; //box是新的盒模型的兼容写法
    -webkit-box-orient: vertical; //设置子元素排列的方式
    overflow: hidden;
    (多行打点兼容性不好,可用js操作)
文本换行:
word-wrap: normal|break-word;
	normal: 连续文本换行;
	break-word: 内容将在边界内换行(强制换行);
文字字体:
@font-face{
    font-family: '写字体库名称';
    src: url('写字体库地址'); //兼容IE
    src: //兼容性写法
    	url('./ShadowsIntoLight.eot?#iefix')format('embedded-opentype'),
    	url('./ShadowsIntoLight.woff')format('woff'),
    	url('./ShadowsIntoLight,ttf')format('truetype'),
    	url('./ShadowsIntoLight.svg')format('svg');
    					//format是告诉浏览器我这是什么类型的文件
}
字体库地址(外网,需要翻墙下载):
http://fonts.google.com/?selection.family=Shadows+Into+Light
```

```html
<style>
    a{
        display: inline-block;
        width: 100px;
        border: 1px solid #000;
        /* 单行打点 */
        text-overflow: ellipsis;
        overflow: hidden; /*带上*/
        white-space: nowrap; /*强制不换行*/
    }
</style>
<body>
    <a href="#">http://www.baidu.com</a>
    <a href="#">百度一下你就知道</a>
</body>
```

```html
<style>
    a{
        display: inline-block;
        width: 100px;
        border: 1px solid #000;
    }
</style>
<body>
    <a class="dot">百度一下你就知道百度一下你就知道</a>
    <script>
        var oA = document.getElementsByClassName('dot')[0];
        function dot(dom, num) { 
            var str = dom.innerHTML; //获取字符串
            var len = str.length; //获取字符串长度
            if(len > num) {
                dom.innerHTML = str.substr(0, num) + '...'; 
                // .substr 裁剪,从第0位开始裁剪到第num位,后面拼接上...并返回;
            }
        };
        dot(oA, 8); //8位自动截断打点
    </script>
</body>
```

```html
改变字体:
<style>
    a{
        display: inline-block;
        width: 100px;
        font-family: 'zitiku';
    }
    @font-face{
        font-family: 'zitiku';
        src: url(./font_style/fonts2/Dancing_Script/DancingScript-Bold.ttf)
    }
</style>
<body>
    <a class="dot">sxw</a>
</body>
```



# 盒模型

```
显示方式: box-sizing: content-box(怪异模式) | border-box(标准模式,默认);
作用: 改变盒模型的解析方式,解决两种模式下盒子宽度标准定义不同造成的问题;
可拖拽地控制盒子的大小:
	resize: none|horizontal|vertical|both; (水平/垂直/水平垂直均可)
	(使用时必须结合属性 overflow: auto;)
定义轮廓: outline: outline-width outline-style outline-color;
```

```html
<style>
    input:focus{
        outline: 3px solid red;
    }
</style>
<body>
    <input type="text">
</body>
```



# columns 多列布局

```css
columns: column-width|column-count;
	column-width: 设置每列的宽度(不小于);
	column-count: 固定列数;
	column-gap: 列宽,默认会继承font-size的值;
	column-span: none|all; (标题横跨所有列);
	column-rule: column-rule-width column-rule-style column-style-color; 列边框样式
    column-rule: 1px dashed #ccc; (线不占宽度)
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>demo</title>
        <style>
            .wrapper{
                border: 4px double #000;
                column-width: 300px; //每列宽度
                width: 900px;
                column-gap: 0; //列宽
                column-rule: 1px dashed #666; //列与列之间的分割线,不占宽度;
            }
            h1{
                text-align: center;
                column-span: all; //标题横跨所有列
            }
        </style>
    </head>
    <body>
        <div class="wrapper">
            <h1>出师表</h1>
            <p>&nbsp;&nbsp;&nbsp;&nbsp;先帝创业未半而中道崩殂，今天下三分，益州疲弊，此诚危急存亡之秋也。然侍卫之臣不懈于内，忠志之士忘身于外者，盖追先帝之殊遇，欲报之于陛下也。诚宜开张圣听，以光先帝遗德，恢弘志士之气，不宜妄自菲薄，引喻失义，以塞忠谏之路也。宫中府中，俱为一体。</p>
            <p>&nbsp;&nbsp;&nbsp;&nbsp;亲贤臣，远小人，此先汉所以兴隆也；亲小人，远贤臣，此后汉所以倾颓也。先帝在时，每与臣论此事，未尝不叹息痛恨于桓、灵也。侍中、尚书、长史、参军，此悉贞良死节之臣，愿陛下亲之信之，则汉室之隆，可计日而待也。臣本布衣，躬耕于南阳，苟全性命于乱世。</p>
            <p>&nbsp;&nbsp;&nbsp;&nbsp;今南方已定，兵甲已足，当奖率三军，北定中原，庶竭驽钝，攘除奸凶，兴复汉室，还于旧都。此臣所以报先帝而忠陛下之职分也。至于斟酌损益，进尽忠言，则攸之、祎、允之任也。</p>
            <p>&nbsp;&nbsp;&nbsp;&nbsp;愿陛下托臣以讨贼兴复之效，不效，则治臣之罪，以告先帝之灵。若无兴德之言，则责攸之、祎、允等之慢，以彰其咎；陛下亦宜自谋，以咨诹善道，察纳雅言，深追先帝遗诏，臣不胜受恩感激。今当远离，临表涕零，不知所言。</p>
        </div>
    </body>
</html>
```



# 瀑布流效果

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>demo</title>
        <style>
            body, ul, li{
                margin: 0;
                padding: 0;
                list-style: none;
            }
            ul{
                /* border: 1px solid #000; */
                /* 给父级设置多列布局columns属性,子元素就会实现多列布局,注意子元素必须都是块元素才能使用多列布局 */
                column-width: 300px; //每列宽度
                margin: 30px 50px; 
            }
            li{
                display: inline-block; /*设置图片一个个排开*/
                width: 300px; /*li宽度是300px*/
                border: 1px solid #ccc;
                padding: 10px; 
                box-sizing: border-box; /*按照IE6混杂模式盒模型属性实现所设置的border和padding同时成立*/
                border-radius: 10px; 
                margin: 10px 5px; 
                /*li上线链接的太紧密了,设置一下margin值,上下margin都设置成10px会导致margin塌陷的问题,最后上下只有10px,而不是20px*/
                /* 注意: 如果margin设置上下10px,左右的5px我们设置为0,然后通过column-gap: 10px;设置列宽,就会出现一个bug,因为是行级块元素,就会出现布局混乱,解决办法是:再次通过改变margin左右的值为5px,即为10px,让出一个10px,来解决 */
            }
            img{
                width: 100%;
            }
        </style>
    </head>
    <body>
        <ul>
            <li><img src="./video/1.jpg" alt=""></li>
            <li><img src="./video/2.jpg" alt=""></li>
            <li><img src="./video/3.jpg" alt=""></li>
            <li><img src="./video/4.jpg" alt=""></li>
            <li><img src="./video/5.jpg" alt=""></li>
            <li><img src="./video/6.jpg" alt=""></li>
            <li><img src="./video/7.jpg" alt=""></li>
            <li><img src="./video/8.jpg" alt=""></li>
            <li><img src="./video/9.png" alt=""></li>
        </ul>
    </body>
</html>
```



# Flex弹性盒子

```
定义: flexbox,只能同时实现一个方向的页面布局,它给flexbox的子元素提供了强大的空间分布和对齐能力;
弹性盒子: 1.flex容器;2.flex元素;
display: flex | inline-flex;
    flex: 把父元素解析成块元素;
    inline-flex: 把父元素解析成行级块元素;
注意: 只要设置了弹性盒子 display: flex|inline-flex 属性,下面这些属性就都没用了;
(columns属性在伸缩容器上没效果,同时float,clear和vertical-align属性在伸缩项目上面也没有效果)
```

```
flexbox的两根轴线:
	1.主轴和交叉轴;
	2.主轴方向可以水平从左向右/从右向左/垂直从上到下/从下到上,由flex-direction决定;
	3.在主轴上的排列方式: (123 )( 123)( 123 )(1 2 3)等;
```

```css
Flex布局:
以下6个属性设置在'容器'上:
	flex-direction
	flex-wrap
	flex-flow (此属性是上面两个属性的简写形式)
	justify-content (项目在主轴上的对齐方式) 
			(flex-start/flex-end/center/space-between/space-around)
	align-items (项目在交叉轴上如何对齐)
			(flex-start/flex-end/center/baseline/stretch默认值)
			baseline 项目的第一行文字的基线对齐
			stretch: 如果项目未设置高度或设为auto,将占满整个容器的高度;
	align-content (定义了多根轴线的对齐方式)
			(flex-start/flex-end/center/space-between/space-around/stretch)
----------------------------------------------------------------------------
以下6个属性设置在'项目'上:
	order
	flex-grow (放大,默认为0)
	flex-shrink (缩小,默认为1)
	flex-basis (分配多余空间,默认auto)
	flex (上面三个属性的简写)
	align-self (允许单个项目与其他项目不一样的对齐方法)
```

```css
1.实现元素的垂直水平居中方式?
a.设置定位,margin为自身的的一半,top: 50%;left: 50%;position: absolute;
.wrapper{
    width: 500px;
    height: 500px;
    border: 1px solid #ccc;
    position: relative;
}
.wrapper .box {
    position: absolute;  
    top:50%;                
    left: 50%;
    margin: -50px -50px;   
    width: 100px;
    height: 100px;
    background-color: blueviolet;
}
-----------------------------------------------
b.transform: translate(-50%,-50%);
c.flex布局 justify-center: center;align-items: center;
.wrapper {
   width: 500px;
   height: 500px;
   border: 1px solid #ccc;
   display: flex;
   justify-content: center;
   align-items: center;
}
.wrapper .box {
   width: 100px;
   height: 100px;
   background-color: blueviolet;
}
--------------------------------------------------
d.margin:auto;position:absolute;left:0;top:0;right:0;bottom:0;
.wrapper {
    width: 500px;
    height: 500px;
    position: relative;
    border: 1px solid #ccc;
}
.wrapper .box {
    position: absolute;
    left: 0;
    right: 0;
    top:0;
    bottom: 0;
    margin: auto;
    width: 100px;
    height: 100px;
    background-color: blueviolet;
}
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>demo</title>
        <style>
            .wrapper{
                display: flex; 
                width: 600px;
                height: 300px;
                border: 1px solid #000;
            }
            .item{
            	// 当子元素宽度加起来刚好等于父元素宽度时,弹性盒子是不会被换行产生的空隙影响页面布局的;
            	// 当子元素宽度累加之后大于父级总宽度,弹性盒子仍然会显示父级的宽度,子元素的宽度会等比被压缩再呈现出来;
                width: 100px;
                height: 200px;
                text-align: center;
                color: #fff;
            }
            .item:first-child{
                background: rgb(200, 70, 90);
            }
            .item:nth-child(2) {
                background: orange;
            }
            .item:last-child{
                background: rgb(88, 8, 88);
            }
        </style>
    </head>
    <body>
        <div class="wrapper">
            <div class="item">1</div>
            <div class="item">2</div>
            <div class="item">3</div>
        </div>
    </body>
</html>
```

```
flex容器中的所有flex元素都有初始默认效果:
主轴水平从左向右,元素排列成一行: flex-direction属性的初始值是row;
元素从左边主轴的起始线开始: justify-content:flex-start;
默认不拉伸(flex-grow:0),但是会压缩(flex-shrink:1)不换行(flex-wrap:nowrap);
不设置高度时flex元素充满flex容器(align-items:strstch),即元素被拉伸来填充交叉轴部分;
```

## flex容器属性

```css
1.flex-direction 设置flex容器主轴方向:
    row: (默认)水平从左往右 ---> (123 )
    row-reverse: 水平从右往左 ---> ( 321)
    column: 垂直从上到下
    column-reverse: 垂直从下到上
2.flex-wrap 控制flex容器是单线还是多线,以及新线的堆叠方向:
    nowrap: 单行,不换行
    wrap: 多行,允许换行
3.justify-content 项目在主轴上的对齐方式:
    flex-start: 默认(123 )
    flex-end: ( 123)
    center: ( 123 )
    space-beteew: (1 2 3)
    space-around: 每个项目两侧的距离相等( 1  2  3 ) 注意:1和2之间的空隙是两个空格
4.align-items 项目在交叉轴上的对齐方式:
    flex-start: 顶部对齐
    flex-end: 底部对齐
    center: 居中对齐
    baseline: 依照flex元素'第一行'文字为基准对齐,没有设置baseline元素的会去找设置了的第一行文字对齐
    stretch: flex元素未设置高度时,高度自动充满flex容器高度
```

## flex元素属性

```css
flex元素属性:
1.flex-basis: length; 定义main-size,没有设置时会继承父元素的宽和高即主轴方向上的尺寸;
2.flex-grow: number; 拉伸比例默认为0,给子元素分别设置1,就会等比把剩余的空间均分;
3.flex-shrink: number; 压缩比例默认为1,如果超过的话会按照1:1:1比例进行压缩;
4.align-self 单个项目在cross轴上的对齐方式
    flex-start: 与cross-start齐平
    flex-end: 与cross-end齐平
    center: 居中
    baseline: 与第一行文字齐平
    stretch: 未设置高度时,该元素高度为flex容器高度
```



# 常见页面布局

```
静态布局;
流式布局;
自适应布局;
响应式布局;
```



# Media媒体查询

```css
媒体类型: 媒体查询中指定的媒体类型匹配展示文档所使用的设备类型;
媒体特性: 媒体特性表达式(0或多个)最终会被解析为true或false;
link元素中的css媒体查询:
    <link rel="stylesheet" href="demo.css" media="screen and (max-width: 600px)">
样式表中的css媒体查询:
    在彩色屏幕下并且最大宽度值不能超过600px,.demo里面的css样式生效
@media screen and (max-width: 600px) { //screen媒体类型,(max-width:600px)媒体特性
    .demo{
        background: pink;
        color: deeppink;
    }
}
@media screen and (min-width: 600px) and (max-width: 800px) {
    .demo{ //(600-800px)宽度之间生效
        background: green;
        color: yellow;
    }
}
```

## 媒体查询的逻辑操作符

```css
1.and操作符
用于合并多个媒体属性或合并媒体属性与媒体类型
@media screen and (min-width: 500px) and (max-width: 800px)
2.逗号分隔列表
逗号分隔效果等同于or逻辑操作符,只要任意一个媒体查询返回真,样式就是有效的
@media (max-width: 300px), screen and (orientation: landscape)
3.not操作符
在媒体查询为假时返回真
@media not screen and (min-width: 500px) and (max-width: 800px)
4.only操作符
防止老旧的浏览器不支持带媒体属性的查询而应用到给定的样式
@media only screen and (min-width: 500px) and (max-width: 800px)
```

```html
<style>
    .wrapper{
        display: flex;
        border: 1px solid #000;
    }
    .left, .right{
        width: 200px;
        height: 200px;
        background: orange;
    }
    .center{/*宽度由内容撑开,高度由内容决定*/
        background: pink; 
        flex-grow: 1;
        width: 100px; /*定宽实现三列布局*/
    }
</style>
<body>
    <div class="wrapper">
        <div class="left">我是left</div>
        <div class="center">我是center我是center我是center我是center我是center我是center我是center</div>
        <div class="right">我是right</div>
    </div>
</body>
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>三列布局</title>
        <style>
            body, ul, li{
                margin: 0;
                padding: 0;
                list-style: none;
            }
            .nav{
                display: none;
                background: #ccc;
                justify-content: flex-end;
            }
            @media screen and (max-width: 800px) { // <= 800px 生效
                .nav{
                    display: flex;
                }
                .left{
                    display: none;
                }
            }
            .nav li{
                display: inline-block;
                border-left: 1px solid rgba(0, 0, 0, 0.2);
                padding: 5px 30px;
                margin: 10px 0;
                text-align: center;
            }
            .wrapper{
                display: flex;
                border: 1px solid #ccc;
                min-width: 600px; //界面小于600px出现横向滚动条
            }
            .left{
                width: 200px;
                height: 200px;
                background: orange;
            }
            .left ul{
                display: flex;
                flex-direction: column;
                height: 200px;
            }
            .left li{
                display: inline-block;
                width: 100%;
                height: 30px;
                line-height: 30px;
                text-align: center;
                flex-grow: 1;
                background: #999;
                margin-bottom: 1px;
            }
            .center{
                flex-grow: 1;
                width: 100px;
                background: pink;
            }
            .right{
                width: 200px;
                height: 200px;
            }
        </style>
    </head>
    <body>
        <nav class="nav">
            <ul>
                <li>1</li>
                <li>2</li>
                <li>3</li>
                <li>4</li>
                <li>5</li>
            </ul>
        </nav>
        <div class="wrapper">
            <div class="left">
                <ul>
                    <li>1</li>
                    <li>2</li>
                    <li>3</li>
                    <li>4</li>
                    <li>5</li>
                </ul>
            </div>
            <div class="center">我是center我是center我是center我是center我是center我是center</div>
            <div class="right">我是right</div>
        </div>
    </body>
</html>
```



# 移动端布局

```
百分比布局;
flex布局;
单位rem(根据html所设置的值为1个单位,可以解决移动端等比缩放布局的问题);
单位em(根据紧挨着的父级的大小值为1单位);
单位vw/vh(把屏幕分成100份,1份就是1vw/1vh,推荐);
```



# js动态获取浏览器屏幕宽度

```js
window.onload = function() {
  	var w = document.documentElement.clientWidth;
    document.documentElement.style.fontSize = w / 10 + 'px';
};
```



# 综合解决方案

```
方案一: 手淘解决方案flexbile
    1.根据屏幕大小动态改变html和fontSize,达到等比缩放问题;
    2.给body设置fontsize,字体大小可以直接继承body的font-size;
    3.给html标签添加data-dpr属性,可以通过查找该属性,给不同dpr元素设置个性化属性;
    [data-dpr='2']div{
        background: green;
    }
方案二: Vw+postcss(插件)(推荐)
    根据设置稿(如宽度750px的设计稿),以px为单位,转换成vw,解决等比缩放问题;
    至于小于等于1px的线,以px为单位不转成vw;
    postcss-write-svg插件主要用来处理移动端1px的解决方案,该插件主要使用的是border-image和background来做1px的相关处理,编译出来是border-image或者background;
```



# Bootstrap

```
1.Bootstrap是最受欢迎的HTML,CSS,JS框架,用于开发响应式布局,移动设备优先的WEB项目;
2.特点:
    a.虽然可以直接使用Bootstrap提供的CSS样式表,但Bootstrap的源码是基于最流行的CSS预处理脚本Less和Sass开发的,你可以采用预编译的CSS文件快速开发,也可以从源码定制自己需要的样式;
    b.你的网站和应用能在Bootstrap的帮助下通过同一份代码快速,有效适配手机,平板,PC设备,这一切都是CSS媒体查询(Media Query)的功劳;
    c.Bootstrap提供了全面,美观的文档,你能在这里找到关于HTML元素,HTML和CSS组件,jQuery插件方面的所有详细文档;
3.快速使用:
	a.全局样式(表格,按钮,辅助类);
	b.组件(按钮组,下拉菜单,字体图标,导航条);
	c.栅格系统(使用了媒体查询实现响应式布局);
	d.插件(模态框,轮播图);
```



# transform形状变换

```css
transform属性向元素应用2D或3D转换;
该属性允许我们对元素进行 移动translate, 缩放scale, 旋转rotate等;
	1.移动: translate
		translateX() 左右
		translateY() 上下
		translateZ() 里外
		translateX(100%) ---> 向右移动自身宽度的尺寸
		transform: translate(100px, 100px);
	2.缩放: scale
		scaleX()
		scaleY()
		scaleZ()
		写一个值表示的是x,y两个方向,值等于1不缩放,大于1放大,小于1缩小;
	3.旋转: rotate
		rotateX(45deg);
		rotateY()
		rotateZ()
		写一个值默认沿z轴旋转;
transform-origin: 改变中心点位置,所有图形变换都是以中心点为标准进行的;
    transform-origin: 50% 50% 0;(默认值)
    X轴方向: left|center|right|length|%
    Y轴方向: top|center|bottom|length|%
    Z轴方向: length
```



# 实现元素水平垂直居中

```css
1.父级设置 position: relative;
2.子元素{
    position: absolute;
    top: 50%;
    margin-top: -高度的一半;  <--效果相同-->  transform: translateY(-50%);
    left: 50%;
    margin-left: -宽度的一半;  <--效果相同-->  transform: translateX(-50%);
}
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>demo</title>
        <style>
            body, ul, li{
                padding: 0;
                margin: 0;
                list-style: none;
            }
            ul{
                position: relative;
                margin: 0 auto;
                width: 300px;
                height: 300px;
                border: 2px solid #000;
                border-radius: 50%;
            }
            li{
                position: absolute;
                top: 0;
                left: 50%;
                margin-left: -1px;
                width: 2px;
                height: 10px;
                background: #000;
                transform-origin: 1px 150px;
            }
            /* ul li:nth-child(1) {
                transform: rotate(30deg);
            } */
        </style>
    </head>
    <body>
        <ul>
            <!-- <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li> -->
        </ul>
        <script>
            var oUl = document.getElementsByTagName('ul')[0];
            var str = '';
            for(var i = 1;i <= 12;i ++) {
                str += '<li style="transform: rotate(' + i * 30 + 'deg)"></li>'
            }
            oUl.innerHTML = str;
        </script>
    </body>
</html>
```



# transition过渡动画

```css
transition: property duration timing-function delay;
	transition-property: 设置过渡效果的CSS属性名称;
	transition-duration: 完成过渡效果需要多少毫秒;
	transition-timing-function: 速度效果的速度曲线;
	transition-delay: 定义过渡效果何时开始;
	transition: all 4s linear; //all所有元素只要最初状态和当前状态有差异了,就会发生过渡;
transition-timing-function: linear|ease|ease-in|ease-out|ease-in-out|cubic-bezier(n,n,n,n);
        linear 匀速 
        ease 慢快慢 
        ease-in 慢速开始的过渡 
        ease-out 慢速结束的过渡 
        ease-in-out 慢速开始和结束的过渡 
        cubic-bezier(n,n,n,n); 在cubic-bezier函数中定义自己的值,0~1之间的数值。
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>demo</title>
        <style>
            #demo{
                width: 100px;
                height: 100px;
                background: pink;
                transition: transform 4000ms linear,height 2s linear 2s;;
            }
        </style>
    </head>
    <body>
        <div id="demo"></div>
        <script>
            demo.onclick = function() {
                this.style.transform = 'translateX(600px)';
                this.style.height = '200px';
            };
        </script>
    </body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>demo</title>
        <style>
            #demo{
                width: 100px;
                height: 100px;
                background: pink;
                transition: all 4s linear;;
            }
            #demo.move{
                transform: translateX(600px);
                height: 200px;
            }
        </style>
    </head>
    <body>
        <div id="demo"></div>
        <script>
            demo.onclick = function() { //只用js控制class名称不去改变dom
                this.className = 'move';
            };
        </script>
    </body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>demo</title>
        <style>
            ul{
                display: flex;
                width: 80%;
                margin: 0 auto;
                min-width: 600px;
                font-size: 0;
            }
            li{
                position: relative;
                display: inline-block;
                list-style: none;
                width: 200px;
                margin: 0 10px;
                border: 2px solid pink;
                border-radius: 20px;
                overflow: hidden;
                background: rgba(0, 0, 0, 0.8);
            }
            li img{
                width: 100%;
                border-radius: 20px;
                transform: translateY(100%);
                transition: transform 0.8s ease;
            }
            .cover{
                position: absolute;
                bottom: 0;
                left: 0;
                height: 25px;
            }
            .des{
                font-size: 12px;
                background: linear-gradient(to bottom, transparent, rgba(0, 0, 0, 0.8));
                color: #fff;
                height: 25px;
                padding: 10px;
                transform: translateY(100%);
                transition: transform 0.3s;
            }
            .init img{
                transform: translateY(0);
            }
            ul li:nth-child(1) img{
                transition-delay: 0;
            }
            ul li:nth-child(2) img{
                transition-delay: 0.1s;
            }
            ul li:nth-child(3) img{
                transition-delay: 0.2s;
            }
            ul li:nth-child(4) img{
                transition-delay: 0.3s;
            }
            ul li:nth-child(5) img{
                transition-delay: 0.4s;
            }
            li:hover .des{
                transform: translateY(0);
            }
        </style>
    </head>
    <body>
        <ul>
            <li>
                <img src="./video/10.jpg" alt="">
                <div class="cover">
                    <div class="des">好好学习,天天向上,好好学习,天</div>
                </div>
            </li>
            <li>
                <img src="./video/11.jpg" alt="">
                <div class="cover">
                    <div class="des">好好学习,天天向上,好好学习,天</div>
                </div>
            </li>
            <li>
                <img src="./video/12.jpg" alt="">
                <div class="cover">
                    <div class="des">好好学习,天天向上,好好学习,天</div>
                </div>
            </li>
            <li>
                <img src="./video/13.jpg" alt="">
                <div class="cover">
                    <div class="des">好好学习,天天向上,好好学习,天</div>
                </div>
            </li>
            <li>
                <img src="./video/14.jpg" alt="">
                <div class="cover">
                    <div class="des">好好学习,天天向上,好好学习,天</div>
                </div>
            </li>
        </ul>
        <script>
            var oUl = document.getElementsByTagName('ul')[0];
            window.onload = function() {
                oUl.className = 'init';
            };
        </script>
    </body>
</html>
```



# 3D变换动画

```
perspective
	1.定义3D元素距视图的距离,以像素计;
	2.为元素设置perspective属性时,其子元素会获得透视效果,而不是元素本身;
	3.给父级设置;
transform-style 指定嵌套元素是怎样在三维空间中呈现;
	transform-style: flat|preserve-3d;
    注:设置了transform-style: preserve-3d;的元素,即不能设置overflow:hidden;不能设置防止子元素溢出否则persever-3d失效;
backface-visibility 定义元素背面是否可见;
	backface-visibility: visible|hidden;
perspective-origin 视点得位置
	perspective-origin: x y; 50% 50%(默认状态下)
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>demo</title>
        <style>
            .wrapper{
                perspective: 600px;
                width: 200px;
                height: 200px;
                margin: 100px;
                border: 1px solid #000;
            }
            .box{
                position: relative;
                width: 200px;
                height: 200px;
                /* perspective: 600px; */
                transform-style: preserve-3d; /*保留整个box的3D效果*/
                animation: move 5s linear infinite;
            }
            .item{
                position: absolute;
                top: 0;
                left: 0;
                width: 200px;
                height: 200px;
                line-height: 200px;
                text-align: center;
                background: pink;
                color: #fff;
                font-size: 30px;
            }
            .one{
                background: pink;
                transform: rotateX(45deg);
            }
            .two{
                background: orange;
                transform: rotateX(-45deg);
            }
            @keyframes move{
                0%{
                    transform: rotateX(0);
                }
                100%{
                    transform: rotateX(360deg);
                }
            }
        </style>
    </head>
    <body>
        <div class="wrapper">
            <div class="box">
                <div class="item one">3号</div>
                <div class="item two">24号</div>
            </div>
        </div>
    </body>
</html>
```

