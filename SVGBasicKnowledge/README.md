# SVG

## 一、概述

svg 是一种基于 XMl 语法的图像格式，全称是可缩放矢量图(Scalable Vector Graphics)。

其他格式都是基于像素处理的，SVG 则是属于对图像形状描述，所以本质上是文本文件，体积较小，且不管放大多少倍都不会失真。

SVG 文件可以直接插入网页，成为 DOM 的一部分，然后用 JavaScript 和 Css 操作。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title></title>
  </head>

  <body>
</style></defs><path d="M615.6 123.6h165.5L512 589.7 242.9 123.6H63.5L512 900.4l448.5-776.9z" fill="#41B883" p-id="3143"></path><path d="M781.1 123.6H615.6L512 303 408.4 123.6H242.9L512 589.7z" fill="#34495E" p-id="3144"></path></svg>
  </body>
</html>
```

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220715100100217-1292470199.png)

上面是 SVG 代码直接插入网页的例子

SVG 代码也可以写在一个单独文件中，然后用`img`、`object`、`iframe`、`embed`等标签插入网页。

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220715100131826-2129767195.png)

```html
<img src="cicle.svg"></img>
<object id="object" data="circle.svg" type="image/svg+xml"></object>
<embed id="embed" src="icon.svg" type="image/svg+xml"></embed>
<iframe id="iframe" src="icon.svg"></iframe>
```

CSS 也可以使用 SVG 文件

```css
.logo {
  background: url(icon.svg);
}
```

SVG 文件还可以转为 BASE64 编码，然后作为 Data URL 写入网页

```html
<img src="data:images/svg+xml;base64,[data]"></img>
```

## 二、语法

### svg 标签

SVG 代码都放在顶层标签<svg>之中，下面是一个例子

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220715105544677-1431839013.png)

```xml
<svg width="100%" height="100%">
  <circle id="mycircle" cx="50" cy="50" r="50" />
</svg>
```

- `svg`的 width 属性和 height 属性，制定了 SVG 图像在 HTML 元素中所占据的宽度和高度
- 除了相对单位，也可以采用绝对单位（单位：像素）
- 如果不指定这两个属性，SVG 图像默认的大小是 300 像素（宽）\* 150 像素（高）

如果只想展示 SVG 图像的一部分，就要指定 viewBox 属性

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220715113051413-2049040177.png)

```html
<svg width="100" height="100" viewBox="50 50 50 50">
  <circle id="mycircle" cx="50" cy="50" r="50" fill="orange" />
</svg>
```

viewBox 属性的值有四个数字，分别是左上角的横坐标和纵坐标，视口的高度和宽度。上面代码中，SVG 图像是 100 像素宽 \* 100 像素高，viewBox 属性指定视口从（50，50）这个点开始，所以实际看到的是右下角的四分之一圆。

注意，视口必须适配所在的空间。上面代码中，视口的大小是（50，50），由于 SVG 图像的大小是（100，100），所以视口回去适配 SV 的大小，即放大了四倍。

如果不指定 width 属性和 height 属性，只指定 viewBox 属性，则相当于只给到定 SVG 图像的长宽比。这时，SVG 图像的默认大小等于所在的 HTML 元素的大小。

### circle 标签

`circle`标签代表圆形

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220715113133149-1115096657.png)

```html
<svg width="200" height="200">
  <circle id="mycircle" cx="100" cy="100" r="50" fill="blue" />
  <circle id="mycircle" cx="170" cy="170" r="5" class="pink" />
</svg>
```

上面的代码定义了三个圆。`circle`标签的 cx,cy,r 属性分别为横坐标、纵坐标和半径，单位为像素。坐标都是相对于`svg`画布的左上角

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220715113413311-1324736496.png)

class 属性用来指定对应的 css 类

```css
.red {
  fill: red;
}
.fancy {
  fill: none;
  stroke: black;
  stroke-width: 3px;
}
```

SVG 的 CSS 属性与网页元素有所不同

- fill:填充色
- stroke:描边色
- stroke:边框宽度

### line 标签

`link`标签用于绘制直线

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220715113749873-1815287645.gif)

```html
<svg width="400" height="400">
  <line
    x1="50"
    y1="50"
    x2="350"
    y2="350"
    style="stroke: blue;stroke-width:3px;"
  ></line>
</svg>
```

```css
.line {
  stroke: blue;
  stroke-width: 3px;
  transition: all 1s ease-in-out;
  /* transform: rotate(25deg); */
  transform-origin: center;
  animation: transfer 2s linear infinite;
}
.line:hover {
  stroke: tan;
}
@keyframes transfer {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
```

上面的代码中，`link`标签的 x1 属性和 y1 属性，表示线段起点的横坐标与纵坐标；x2 和 y2 属性，表示线段终点的横坐标与纵坐标；style 属性表示线段的样式。

也可以在类名中同样加入过度属性与动画，效果同普通 html 元素

### polyline 标签

<polyline> 标签用于绘制一个折线

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220715113853442-666811910.png)

```html
<svg width="400" height="400">
  <polyline
    points="90,60 180,280 360,360"
    style="stroke: green;stroke-width:3px;fill:none"
  ></polyline>
</svg>
```

`polyline`的 points 属性指定了每个端点的坐标，横坐标与纵坐标之间与逗号分隔，点与点之间用空格分隔。

### rect 标签

<rect> 标签用于绘制矩形

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220718192434293-364030044.png)

```html
<svg width="300" height="180">
  <rect
    x="50"
    y="50"
    height="200"
    width="200"
    style="stroke:#70d5dd;fill:#dd524b"
  ></rect>
</svg>
```

`rect`的 x 属性和 y 属性，指定了矩形左上角端点的横坐标和纵坐标；width 属性和 height 属性指定了矩形的宽度和高度（单位像素）

### ellipse 标签

<ellipse> 标签用于绘制椭圆

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220718193005665-389385309.png)

```html
<svg width="300" height="180">
  <ellipse
    cx="150"
    cy="60"
    rx="40"
    ry="20"
    style="stroke:#092d2e;fill:#271cc3"
  ></ellipse>
</svg>
```

`ellipse`的 cx 和 cy 属性，指定了椭圆中心的横坐标和纵坐标（单位像素）；rx 属性和 ry 属性，指定了椭圆横向轴和纵向轴的半径（单位像素）

### polygon 标签

<polygon> 标签用于绘制多边形

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220718195843075-261725640.png)

```html
<svg width="300" height="300">
  <polygon
    fill="green"
    stroke="orange"
    stroke-width="1"
    points="0,0 100,0 100,100 0,100 0,0"
  ></polygon>
</svg>
```

`polygon`的 points 属性指定了每个端点的坐标，横坐标与纵坐标之间用逗号分隔，点与点之间用空格分隔。

### path 标签

<path> 标签用于绘制路径

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220718200558648-263568237.png)

```html
<svg width="300" height="300">
  <path
    d="
  M 18,3
  L 46,3
  L 46,40
  L 61,40
  L 32,68
  L 3,40
  L 18,40
  Z
  "
  ></path>
</svg>
```

`path` 的 d 属性表示绘制顺序，它的值是一个长字符串，每个字母表示一个绘制动作，后面跟着坐标。

- M：移动到(moveto)
- L：画直线到(ineto)
- Z：闭合路径

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220720195051363-1396622258.png)

### text 标签

<text> 标签用于绘制文本

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220719203003231-2077036231.png)

```html
<svg
  width="500"
  height="500"
  style="
        stroke: #ff0000;
        stroke-width: 2px;
        fill: none;
        font-size: 50px;
        text-shadow: 0 0 20px #333;
      "
>
  <text x="50" y="50">Hello World</text>
</svg>
```

`text`的 x 属性和 y 属性，表示文本区块基线(baseline)起点的横坐标和纵坐标。文字的样式可以用 class 或 style 属性指定。

**改变字体颜色需要使用`fill`属性，而不是 color 属性。**

### use 标签

<use>标签用于复制一个形状

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220720200013677-343906115.png)

```html
<svg width="600" height="600">
  <circle
    id="circle1"
    cx="200"
    cy="200"
    r="50"
    fill="orange"
    stroke="blue"
    stroke-width="2px"
  ></circle>
  <use href="#circle1" x="100" y="100"></use>
</svg>
```

`use`标签的 href 属性指定所复制的节点，x 属性和 y 属性是相对于指定节点左上角的坐标。另外还可以指定 width 和 height 坐标

### g 标签

<g>标签用于将多个形状做成一个(group)，方便复用。

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220720200943041-261815660.png)

```html
<svg width="1200" height="1200">
  <g id="miqi">
    <circle cx="100" cy="100" r="50"></circle>
    <circle cx="500" cy="100" r="50"></circle>
    <circle cx="300" cy="300" r="100"></circle>
  </g>
  <use href="#miqi" x="500" y="0"></use>
</svg>
```

### defs 标签

<defs>标签用于自定义形状，它内部的代码不会显示，仅供引用

![](https://img2022.cnblogs.com/blog/2332774/202207/2332774-20220721204526564-1214816201.png)

```html
<svg width="1200" height="1200">
  <defs>
    <g id="miqi">
      <circle cx="100" cy="100" r="50"></circle>
      <circle cx="500" cy="100" r="50"></circle>
      <circle cx="300" cy="300" r="100"></circle>
    </g>
  </defs>
  <use href="#miqi" x="500" y="0"></use>
</svg>
```

`defs`标签的元素内容无法查看且不能通过 css 控制它的样式

### pattern 标签

<pattern>标签用于自定义一个形状，该形状可以被引用来平铺一个区域

![](https://img2022.cnblogs.com/blog/2332774/202208/2332774-20220822203758051-553559676.png)

```html
<svg width="500" height="500">
  <defs>
    <pattern
      id="dots"
      x="0"
      y="0"
      width="100"
      height="100"
      patternUnits="userSpaceOnUse"
    >
      <circle fill="red" cx="50" cy="50" r="35"></circle>
    </pattern>
  </defs>
  <rect x="0" y="0" width="100%" height="100%" fill="url(#dots)"></rect>
</svg>
```

上面代码中，<pattern>标签将一个圆形定义为 dots 模式。patternUnits="userSpaceOnUse"表示<pattern>的宽度和长度最实际的像素值。然后，指定这个模式下区填充下面的矩形

### image 标签

<image>标签用于插入图片文件

![](https://img2022.cnblogs.com/blog/2332774/202208/2332774-20220822205035960-18969926.png)

```html
<svg viewBox="0 0 1000 1000" width="1000" height="1000">
  <image xlink:href="svg-file/0.png" width="50%" height="50%"></image>
</svg>
```

上面的代码中，<image>的 xlink:href 属性表示图像的来源

### animate 标签

<animate>标签用于产生动画效果

![](https://img2022.cnblogs.com/blog/2332774/202208/2332774-20220822205940243-372578551.gif)

```html
<svg width="1000" height="1000">
  <rect x="0" y="0" width="100" height="50" fill="orange">
    <animate
      attributeName="x"
      from="0"
      to="500"
      dur="2s"
      repeatCount="indefinite"
    ></animate>
    <animate
      attributeName="y"
      from="0"
      to="500"
      dur="3s"
      repeatCount="indefinite"
    ></animate>
  </rect>
</svg>
```

上面的代码中，矩形会不断移动，产生动画效果

animate 的属性含义如下

- attributeName：发生动画效果属性名
- from：单词动画的初始值
- to：单词动画结束值
- dur：单词动画持续时间
- repeatCount：动画的循环模式

可以在多个属性上定义动画

![](https://img2022.cnblogs.com/blog/2332774/202208/2332774-20220823092409108-1608726643.gif)

```html
<animate
  attributeName="x"
  from="0"
  to="500"
  dur="2s"
  repeatCount="indefinite"
></animate>
<animate
  attributeName="y"
  from="0"
  to="500"
  dur="3s"
  repeatCount="indefinite"
></animate>
```

### animateTransform 标签

![](https://img2022.cnblogs.com/blog/2332774/202208/2332774-20220823092033966-1583317903.gif)


```html
<svg width="1000" height="1000">
  <rect x="0" y="0" width="100" height="50" fill="blue">
    <animateTransform
      attributeName="transform"
      type="rotate"
      begin="0"
      dur="2s"
      form="0 200 200"
      to="360 400 400"
      repeatCount="indefinite"
    ></animateTransform>
  </rect>
</svg>
```

上面的代码中，<animateTransform>效果为旋转(rotate)，这时from和to属性有三个数字，第一个数字是角度值，第二个值和第三个值是旋转中心的坐标。`from="0 200 200"`表示开始时。角度为`0`，围绕`(200,200)`开始旋转；`to="360 400 400"`表示结束时，角度为`360`，围绕`(400,400)`旋转。

<animate>标签对 css 属性不起作用，如果需要变形就要使用<animateTransform>标签

## 三、JavaScript操作SVG

### 1.操作DOM

如果SVG代码直接写在HTML网页中，它就成为网页DOM的一部分，可以直接用DOM操作。

首先需要创建一个SVG元素

```html
<div class="enlarge-svg">
      <svg width="500" height="500">
        <rect x="50" y="50" width="100" height="100" class="rect"></rect>
      </svg>
    </div>
    <button>放大SVG</button>
```
其次，可以对其添加一些样式

```css
 .enlarge-svg{
        width: 500px;
        border: 1px saddlebrown dashed;
      }
```

最后就可以使用用Javascript操作SVG

```JavaScript
let btn = document.querySelector("button");
      btn.onclick = function () {
        let svgRect = document.querySelector("rect");
        console.log([svgRect]);
        svgRect.setAttribute("width", 200);
        svgRect.setAttribute("height", 200);
        svgRect.setAttribute("fill", "orange");
        svgRect.setAttribute("x", 150);
        svgRect.style.transition = "all 1s ease-in-out";
        let speed = 20;
        let location = 20;
        setInterval(() => {
          location += speed;
          svgRect.setAttribute("x", location);
        }, 100);
      };
```
以上代码最终的效果如下图所示，点击按钮，矩形的svg元素会被放大并且移动。总的来说JavaScript操作SVG就和JavaScript操作HTML的普通元素的过程以及方法是类似的，主要是对其特有的样式属性等需要做一个了解，并且在实际操作中时刻观察它的变化情况。

![](https://img2022.cnblogs.com/blog/2332774/202208/2332774-20220827142808288-1804090676.gif)



### 2.获取 SVG DOM

使用`object` `embed` 标签可以获取 SVG DOM

```JavaScript
var svgObject = document.getElementById('object').contentDocument
var svgIframe = document.getElementById('iframe').contentDocument
var svgEmbed = document.getElementById('embed').getSVGDocument()
```

## 四、使用SVG绘制图形
    
下面，我们使用SVG来绘制两个图形，用来帮助我们熟悉svg的相关属性以及如何使用javaScript来操纵svg元素。
    
### 1.环形进度条
    
![GIF 2022-10-8 20-58-36.gif](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7e4181711755464a9719ce607a8434d0~tplv-k3u1fbpfcp-watermark.image?)
    
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>环形进度条</title>
    <style>
      .text {
        text-anchor: middle;
      }
      body {
        text-align: center;
      }
    </style>
  </head>
  <body>
    <svg height="700" width="700">
      <!-- 设置底色的圆环 -->
      <circle
        cx="350"
        cy="350"
        r="300"
        fill="none"
        stroke="grey"
        stroke-width="40"
        stroke-linecap="round"
      ></circle>
      <!-- 设置进度条 -->
      <circle
        class="progess"
        transform="rotate(-90,350,350)"
        cx="350"
        cy="350"
        r="300"
        fill="none"
        stroke="red"
        stroke-width="40"
        stroke-linecap="round"
        stroke-dasharry="0,10000"
      ></circle>
      <!-- stroke-dasharry:一个表示长度，一个表示间距 -->
      <!-- 设置文本 -->
      <text class="text" x="350" y="350" font-size="200" fill="red">0</text>
    </svg>

    <script>
      //获取进度条circle对象
      let progressDom = document.querySelector(".progess");
      //获取文本对象
      let textDom = document.querySelector(".text");
      //获取svg圆形环的总长，通过获取半径长度获取总长
      function rotateCircle(persent) {
        let circleLength = Math.floor(
          2 * Math.PI * parseFloat(progressDom.getAttribute("r"))
        );
        console.log(circleLength);
        //按照百分比算出进度环的长度
        let value = (persent * circleLength) / 100;
        //red:rgb(255,0,0)
        //blue:rgb(0,191,255)
        let red = 255 + parseInt(((0 - 255) / 100) * persent);
        let green = 0 + parseInt(((191 - 0) / 100) * persent);
        let blue = 0 + parseInt(((255 - 0) / 100) * persent);
        //设置stroke-dasharray和路径的颜色
        progressDom.setAttribute("stroke-dasharray", value + ",10000");
        progressDom.setAttribute("stroke", `rgb(${red},${green},${blue})`);
        //设置文本内容颜色
        textDom.innerHTML = persent + "%";
        textDom.setAttribute("fill", `rgb(${red},${green},${blue})`);
      }
      //30毫秒变化执行
      let num = 0;
      setInterval(() => {
        num++;
        if (num > 100) {
          num = 0;
        }
        rotateCircle(num);
      }, 30);
    </script>
  </body>
</html>
```
    
### 2.条形统计图

![N$tLm8h$25EXiUAXE1yF1Q==.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b0c6c17f6fc42119638b04adf68e503~tplv-k3u1fbpfcp-watermark.image?)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .coordinate {
        stroke: #999;
        stroke-width: 2;
      }
      .scale {
        stroke: orangered;
        stroke-width: 1;
      }
    </style>
  </head>
  <body>
    <!-- 
        1.获取数据
        2.创建SVG
        3.创建坐标
        4.绘制座标文字
        5.依据数据绘制图形
    -->
    <svg width="1000" height="700">
      <g id="coordinate">
        <!-- 坐标 -->
        <line class="coordinate" x1="50" y1="600" x2="950" y2="600"></line>
        <path d="M 925,590 L 950,600 L 925,610"></path>
        <text x="920" y="630">时间</text>
        <line class="coordinate" x1="100" y1="650" x2="100" y2="50"></line>
        <path d="M 90,75 L 100,50 L 110,75"></path>
        <text x="50" y="70">订单</text>
      </g>
      <!-- 刻度 -->
      <g id="scalex"></g>
      <g id="scaley"></g>
      <g id="list"></g>
    </svg>
    <script>
      let data = [
        {
          time: '8月21日',
          order: 12000,
        },
        {
          time: '8月22日',
          order: 13000,
        },
        {
          time: '8月23日',
          order: 11000,
        },
        {
          time: '8月24日',
          order: 14000,
        },
        {
          time: '8月25日',
          order: 15000,
        },
        {
          time: '8月26日',
          order: 12000,
        },
        {
          time: '8月27日',
          order: 11000,
        },
      ];
      let scalex = document.getElementById("scalex");
      let scaley = document.getElementById("scaley");

      let listLength = parseInt(700 / data.length);
      let yLength = parseInt(450 / 15);

      let listDom = document.getElementById("list");

      for (let i = 1; i <= data.length; i++) {
        renderScale(i);
      }
      for (let j = 1; j <= 15; j++) {
        let lineDom = document.createElement("line");
        lineDom.className = "scale";
        scaley.innerHTML =
          scaley.innerHTML +
          `<line class="scale" x1="100" y1=${600 - yLength * j} x2="120" y2=${
            600 - yLength * j
          } ></line>` +
          `<text x="50" y=${600 - yLength * j}>${1000 * j}</text>`;
      }
      function renderScale(i) {
        let lineDom = document.createElement("line");
        lineDom.className = "scale";
        lineDom.setAttribute("x1", i * listLength + 150);
        lineDom.setAttribute("y1", 600);
        lineDom.setAttribute("x2", i * listLength + 150);
        lineDom.setAttribute("y2", 580);
        scalex.innerHTML =
          scalex.innerHTML +
          lineDom.outerHTML +
          `<text x="${70 + listLength * i}" y="620">${
            data[i - 1].time
          }</text>`;

        let rgbColor = `rgb(${parseInt(Math.random() * 255)},
         ${parseInt(Math.random() * 255)} ,
         ${parseInt(Math.random() * 255)} 
        )`;
        console.log(rgbColor);

        listDom.innerHTML =
          listDom.innerHTML +
          `<rect x="${90 + listLength * i}" y="${
            600 - (data[i - 1].order / 1000) * yLength
          }" width="20" height="${
            (data[i - 1].order / 1000) * yLength
          }" fill="${rgbColor}""></rect>`;
      }
    </script>
  </body>
</html>

```

## 五、总结
              
这篇文章介绍了svg矢量图标的基础知识及语法，也简单介绍了svg矢量图标的绘制以及如何使用JavaScript操作svg。当然，如果你想要炫酷的svg图标，那么你可能需要专业的工具来帮你完成这项任务😀。
              
我是Atrox🚀，一个正在努力学习的前端攻城狮🦁，期待你的关注，一起学习，共同进步💪💪💪。