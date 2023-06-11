## canvas

<p align=center><img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6a776ba27407404eb6ac75ef229ae410~tplv-k3u1fbpfcp-watermark.image?" alt="No4Hj3C0pgUJ0PA$eqI1Xw==.png"  /></p>

> 学习地址：https://www.bilibili.com/video/BV1Ri4y1M7NT

### 前言

Hello，大家好！我是 Atrox🚀，一个正在努力学习的前端攻城狮 🦁。在我的上一篇文章 svg 基础知识入门中，我向大家介绍了有关`SVG`的基础知识。

在`HTML5`之前，人们通常使用`SVG`来在页面上绘制出图形。`SVG`使用`XML`来定义图形，就像使用`HTML`标签和样式定义`div`一样。但是浏览器在做页面渲染时，`Dom`元素是作为矢量图进行渲染的。每一个元素的边距都需要单独处理，浏览器需要将它们全都处理成像素才能输出到屏幕上，计算量十分庞大。当页面上内容非常多，存在大量`DOM`元素的时候，这些内容的渲染速度就会变得很慢。因此对于复杂的图形以及动画的绘制我们也可以使用`Canvas`来实现。

### SVG 与 Canvas

`Canvas`是`HTML`5 中新增的标签，用于在网页上实时生成图像，并且可以操作图像内容。`Canvas`提供的是 `JavaScript` 的`绘图 API`，而不是像`SVG`那样使用`XML`描述绘图，通过 `JavaScript API`直接完成绘制，比起修改`XML`来说要更简便、更直接。

而`Canvas`与`DOM`的区别则是`Canvas`的本质就是一张位图，类似 img 标签，或者一个 div 加了一张背景图（background-image）。所以，`DOM`那种矢量图在渲染中存在的问题换到`Canvas`身上就完全不同了。在渲染`Canvas`时，浏览器只需要在`JavaScript`引擎中执行绘制逻辑，在内存中构建出画布，然后遍历整个画布里所有像素点的颜色，直接输出到屏幕就可以了。**不管`Canvas`里面的元素有多少个，浏览器在渲染阶段也仅需要处理一张画布。**

因此使用`Canvas`在某些场景下可以极大的提升渲染性能。除此之外，在复杂动画、游戏、数据可视化、图片编辑器、实时视频处理等领域，`Canvas`都有优秀的表现。下面让我们一起来学习吧！

### 简介

**Canvas API（画布）** 是在 HTML5 中新增的标签用于在网页实时生成图像，并且可以操作图像内容，基本上它是一个可以用 JavaScript 操作的位图（bitmap）。Canvas 对象表示一个 HTML 画布元素 `canvas`。它没有自己的行为，但是定义了一个 API 支持脚本化客户端绘图操作。它可以用来制作照片集或者制作简单的动画，甚至可以进行实时视频处理和渲染。

### 描述

HTML5`canvas`标签用于绘制图像(通过脚本，通常是 JavaScript)

```html
<canvas id="canvas" width="400" height="400"> 您的浏览器暂不支持canvas </canvas>
```

上面的代码中，我们创建了一个`id`为`canvas`的元素。

`canvas`元素本身并没有绘制能力(它仅仅是图形的容器)，你必须使用脚本来完成实际的绘图任务。因此，标签之中的文字并不会被展示，只有你的浏览器不支持`canvas`时才会展示该内容。

`canvas`标签提供了一个方法**getContext()方法**，可返回一个对象，该对象提供了用于在画布上绘图的方法和属性。

这里需要注意一点，getContext 方法是有一个接收参数，它是绘图上下文的类型，可能的参数有：`2d`,`webgl`,`webgl2`,`bitmaprenderer`。

这里我们只介绍并使用 **getContext("2d")** 对象的属性和方法，它可以用于在画布上绘制文本、线条、矩形、圆形等。

```js
//1.找到画布对象
let divcanvas = document.getElementById("divcanvas");

//2.获取上下文对象
let ctx = divcanvas.getContext("2d");

//3.绘制元素
ctx.rect(50, 50, 300, 300); //绘制一个矩形，它的起始坐标点是(50,50)大小是300

//4.填充元素
ctx.fillStyle = "aqua"; //设置填充样式
ctx.fill(); //填充元素

//5.路径描边
ctx.lineWidth = 15;
ctx.strokeStyle = "salmon";
ctx.stroke();
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230313154222985-971065162.png"  /></p>

如图所示，我们创建了一个宽高为 400 的`canvas`元素，并在其中绘制了一个宽高为 300 的矩形，并填充了描边的样式。

**canvas 仅仅只是一个画布标签，要绘制内容，必须用 js 绘制。**

### Canvas 三要素

`canvas`元素的三要素分别是`id`,`width`,`height`。

- id：作为唯一标识
- width：画布内容宽度的像素大小(与 style 的宽度和高度是有区别的)
- height：画布内容高度的像素大小

`canvas` 有 默认的 宽度(300px) 和 高度(150px)。如果不在 `canvas` 上设置宽高，那 `canvas` 元素的默认宽度是`300px`，默认高度是`150px`。需要注意的是，`width`,`height`这两个属性只需传入数值，不需要传入单位（比如 px 等）。需要注意的是，**不能通过 CSS 设置画布的宽高使用 css 设置 canvas 的宽高**，否则会出现**内容被拉伸**的后果。

下面我就来介绍`canvas`常见的几种属性与方法，帮助大家快速入门。

### Canvas 常用绘制属性与方法

#### 1. 路径

首先我们来介绍`canvas`中与路径相关的方法，路径的使用可以说是比较基础且普遍的，能够实现各种基础的图形。

|                                        方法                                         |                          描述                          |
| :---------------------------------------------------------------------------------: | :----------------------------------------------------: |
|             [fill()](https://www.w3school.com.cn/jsref/canvas_fill.asp)             |                    填充当前绘图路径                    |
|           [stroke()](https://www.w3school.com.cn/jsref/canvas_stroke.asp)           |                    绘制已定义的路径                    |
|        [beginPath()](https://www.w3school.com.cn/jsref/canvas_beginpath.asp)        |              起始一条路径或者重置当前路径              |
|           [moveTo()](https://www.w3school.com.cn/jsref/canvas_moveto.asp)           |               把路径移动到画布中的制定点               |
|        [closePath()](https://www.w3school.com.cn/jsref/canvas_closepath.asp)        |               创建从当前点到起始点的路径               |
|           [lineTo()](https://www.w3school.com.cn/jsref/canvas_lineto.asp)           | 添加一个新点，然后在画布中创建从改点到最后指定点的线条 |
|             [clip()](https://www.w3school.com.cn/jsref/canvas_clip.asp)             |           从原始画布剪切任意形状和尺寸的区域           |
| [quadraticCurveTo()](https://www.w3school.com.cn/jsref/canvas_quadraticcurveto.asp) |                   创建二次贝塞尔曲线                   |
|    [bezierCurveTo()](https://www.w3school.com.cn/jsref/canvas_beziercurveto.asp)    |                   创建三次贝塞尔曲线                   |
|              [arc()](https://www.w3school.com.cn/jsref/canvas_arc.asp)              |           创建弧、曲线，用于创建圆形或部分圆           |
|            [arcTo()](https://www.w3school.com.cn/jsref/canvas_arcto.asp)            |                创建两切线之间的弧、曲线                |
|    [isPointInPath()](https://www.w3school.com.cn/jsref/canvas_ispointinpath.asp)    |  如果指定的点位于当前路径中则返回 true 否则返回 false  |

**示例**

```html
<body>
  <canvas id="canvas" width="600" height="600"></canvas>
  <script>
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    //设置开始路径
    ctx.beginPath();
    // 设置绘制的起始点
    ctx.moveTo(50, 50);
    // 设置经过某个位置
    ctx.lineTo(50, 300);
    ctx.lineTo(300, 100);
    ctx.lineTo(300, 250);
    ctx.closePath(); //设置结束路径
    ctx.stroke(); // 绘制路径
  </script>
</body>
```

上面的代码实现了如下图所示的一个效果，如果想要填充一些样式与效果我们就需要使用`fill()`方法了。

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230315210451815-1329970753.png"  /></p>

```js
// 在上一部分代码中补充
ctx.closePath(); //设置结束路径
ctx.fillStyle = "rgb(36,172,242)"; //设置填充样式
ctx.fill(); // 填充路径
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230315210618183-1338530459.png"  /></p>

这里我们简单的设置绘制样式并使用`fill()`方法进行了填充就实现了这样的效果，其余设置样式的方式我们会在下一章节介绍。而`clip()`方法是用来从原始画布中剪切任意形状和尺寸，一旦剪切了某个区域，则所有之后的绘图都会被限制在被剪切的区域内（不能访问画布上的其他区域）。您也可以在使用 `clip()` 方法前通过使用 `save()` 方法对当前画布区域进行保存，并在以后的任意时间对其进行恢复（通过 `restore()` 方法）。以下代码就实现了在画布上剪切一个矩形后，后续对画布的操作正在这个矩形内生效，最后的效果就是只填充了一个矩形。

```html
<body>
  <canvas id="canvas" width="600" height="600"></canvas>

  <script>
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    ctx.rect(70, 150, 50, 50);
    ctx.stroke();
    ctx.clip();
    // 一旦剪切了某个区域，则所有之后的绘图都会被限制在被剪切的区域内（不能访问画布上的其他区域）。

    ctx.beginPath(); // 设置开始路径
    ctx.moveTo(50, 50); // 设置绘制的起始点

    // 设置经过某个位置
    ctx.lineTo(50, 300);
    ctx.lineTo(300, 100);
    ctx.lineTo(300, 250);

    ctx.closePath(); // 设置结束路径
    ctx.fillStyle = "rgb(36,172,242)"; //设置填充样式
    ctx.fill(); // 填充路径

    ctx.stroke(); // 绘制路径
  </script>
</body>
```

如图所示：只有坐标为起始位置(70,50)，宽高为 50 的矩形区域被绘制了。

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230316203143146-77873582.png"  /></p>

剩余的方法我们就不一一介绍了，使用这些方法可以创造出更丰富的图形。

```html
<body>
  <canvas id="canvas" width="600" height="600"></canvas>

  <script>
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    ctx.beginPath(); // 设置开始路径
    ctx.moveTo(50, 50); // 设置绘制的起始点
    ctx.quadraticCurveTo(20, 100, 200, 20); // 创建二次贝塞尔曲线
    ctx.bezierCurveTo(20, 100, 200, 100, 200, 150); // 创建三次贝塞尔曲线
    ctx.arc(100, 75, 50, 0, 2 * Math.PI); // 绘制圆形
    ctx.arcTo(150, 20, 150, 70, 50);

    // 设置经过某个位置
    ctx.lineTo(50, 300);
    ctx.lineTo(300, 100);
    ctx.lineTo(300, 250);

    ctx.closePath(); // 设置结束路径
    ctx.fillStyle = "rgb(36,172,242)"; //设置填充样式
    ctx.fill(); // 填充路径

    ctx.stroke(); // 绘制路径
  </script>
</body>
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230316204329837-1751824776.png"  /></p>

---

#### 2. 颜色、样式和阴影

绘制图形时最简单的需求无外乎改变颜色、样式、阴影等效果了，下面依据这两张表来介绍这些属性与方法的基础用法(以绘制填充矩形元素为例)。

| 属性                                                                       | 描述                                       |
| -------------------------------------------------------------------------- | ------------------------------------------ |
| [fillStyle](https://www.w3school.com.cn/jsref/canvas_fillstyle.asp)        | 设置或返回用于填充绘画的颜色、渐变或模式。 |
| [strokeStyle](https://www.w3school.com.cn/jsref/canvas_strokestyle.asp)    | 设置或返回用于笔触的颜色、渐变或模式。     |
| [shadowColor](https://www.w3school.com.cn/jsref/canvas_shadowcolor.asp)    | 设置或返回用于阴影的颜色。                 |
| [shadowBlur](https://www.w3school.com.cn/jsref/canvas_shadowblur.asp)      | 设置或返回用于阴影的模糊级别。             |
| [shadowOffseX](https://www.w3school.com.cn/jsref/canvas_shadowoffsetx.asp) | 设置或返回阴影与形状的水平距离。           |
| [shadowOffseY](https://www.w3school.com.cn/jsref/canvas_shadowoffsety.asp) | 设置或返回阴影与形状的垂直距离。           |

通过以下代码：

```html
<canvas id="canvas" width="400" height="400"> </canvas>
```

```js
//1.找到画布对象
let canvas = document.getElementById("canvas");

//2.上下文对象
let ctx = canvas.getContext("2d");

//3.绘制路径
ctx.rect(50, 50, 200, 100);

//4.填充元素
ctx.fillStyle = "skyblue";
ctx.fill();
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230313164818269-9601861.png"  /></p>

我们就生成了一个天蓝色背景的矩形元素，在此基础上加入以下代码：

```js
ctx.lineWidth = 3;
ctx.strokeStyle = "salmon";
ctx.stroke();
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230313165454194-784235081.png"  /></p>

我们就可以得到带有一个描边样式的矩形了。之后我们就可以为矩形设置阴影，完成我们想要的效果了。

```js
ctx.shadowBlur = 10;
ctx.shadowColor = "gold";
ctx.shadowOffsetX = 5;
ctx.shadowOffsetY = 5;
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230313170520765-633889087.png"  /></p>

如图所示，利用这些属性与方法，就可以如同设置 css 一般，绘制各种各样的元素了。除了设置简单颜色以及边框填充外，我们还可以使用以下方法创建具有渐变效果的样式来填充(具体使用方式见链接)。

| 方法                                                                                        | 描述                                  |
| ------------------------------------------------------------------------------------------- | ------------------------------------- |
| [createLinearGradient()](https://www.w3school.com.cn/jsref/canvas_createlineargradient.asp) | 创建线性渐变(用在画布内容上)          |
| [createPattern()](https://www.w3school.com.cn/jsref/canvas_createradialgradient.asp)        | 在指定方向上重复指定的元素            |
| [createRadiaGradient()](https://www.w3school.com.cn/jsref/canvas_createradialgradient.asp)  | 创建放射状/环形的渐变(用在画布内容上) |
| [addColorStop()](https://www.w3school.com.cn/jsref/canvas_addcolorstop.asp)                 | 规定渐变对象中的颜色和停止位置        |

在掌握了`Canvas`填充元素的基础用法后，下面我们就可以绘一些基础的图形了。

---

#### 3. 线条样式

绘制线条时，首先需要设置开始路径`beginPath()`,然后设置绘制线条的起始点`moveTo(x, y)`，途经点`lineTo(x, y)`,最后结束线条绘制`closePath()`。可以设置以下的属性来丰富线条的样式。

|                                 属性                                  |                   描述                   |
| :-------------------------------------------------------------------: | :--------------------------------------: |
|    [lineCap](https://www.w3school.com.cn/jsref/canvas_linecap.asp)    |       设置或返回线条结束端点的样式       |
|   [lineJoin](https://www.w3school.com.cn/jsref/canvas_linejoin.asp)   | 设置或返回两条线相交时，所创建的拐角类型 |
|  [lineWidth](https://www.w3school.com.cn/jsref/canvas_linewidth.asp)  |          设置或返回当前线条宽度          |
| [miterLimit](https://www.w3school.com.cn/jsref/canvas_miterlimit.asp) |          设置或返回最大斜接长度          |

```js
let canvas1 = document.getElementById("canvas1");
let ctx1 = canvas1.getContext("2d");

//设置开始路径
ctx1.beginPath();
// 设置绘制的起始点
ctx1.moveTo(50, 50);
// 设置经过某个位置
ctx1.lineTo(50, 300);
ctx1.lineTo(300, 100);
ctx1.lineTo(300, 260);
//设置结束路径
ctx1.closePath();

// 绘制路径
ctx1.lineCap = "round"; // 设置起始路径的线段边缘为圆角 默认butt
ctx1.lineJoin = "round"; // 设置或返回两条线相交时，所创建的拐角类型 默认miter
ctx1.miterLimit = 2;
ctx1.strokeStyle = "rgb(36,172,242)";
ctx1.lineWidth = 10;
ctx1.stroke();
```

效果如下图所示：

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230313174327984-721109739.png"  /></p>

---

#### 4.矩形

|                                  属性                                   |             描述             |
| :---------------------------------------------------------------------: | :--------------------------: |
|       [rect()](https://www.w3school.com.cn/jsref/canvas_rect.asp)       |           创建矩形           |
|   [fillRect()](https://www.w3school.com.cn/jsref/canvas_fillrect.asp)   |      绘制“被填充”的矩形      |
| [strokeRect()](https://www.w3school.com.cn/jsref/canvas_strokerect.asp) |       绘制矩形(无填充)       |
|  [clearRect()](https://www.w3school.com.cn/jsref/canvas_clearrect.asp)  | 在给定的矩形内清除指定的像素 |

绘制矩形以及改变矩形的样式都比较简单我们可以在一张图中就演示这些方法的使用。

```html
<body>
  <canvas id="canvas" width="600" height="600"></canvas>

  <script>
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    ctx.rect(20, 20, 150, 100);
    ctx.stroke(); //黑色实线区域
    ctx.fillStyle = "#f2928f";
    ctx.fillRect(20, 20, 150, 100); // 浅粉色填充区域
    ctx.strokeStyle = "#2a54b6";
    ctx.strokeRect(70, 70, 150, 100); // 蓝色实线区域
    ctx.clearRect(20, 20, 50, 50); // 黑色区域中的空白区域
  </script>
</body>
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230316210119483-1767417895.png"  /></p>

---

#### 5. 转换

|                                    方法                                     |                      描述                      |
| :-------------------------------------------------------------------------: | :--------------------------------------------: |
|        [scale()](https://www.w3school.com.cn/jsref/canvas_scale.asp)        |            缩放当前绘图至更大或更小            |
|       [rotate()](https://www.w3school.com.cn/jsref/canvas_rotate.asp)       |                  旋转当前绘图                  |
|    [translate()](https://www.w3school.com.cn/jsref/canvas_translate.asp)    |           重新映射画布上的(0,0)位置            |
|    [transform()](https://www.w3school.com.cn/jsref/canvas_transform.asp)    |               替换绘图的当前矩阵               |
| [srtTransform()](https://www.w3school.com.cn/jsref/canvas_settransform.asp) | 将当前转换重置为单位矩阵，然后运行 transform() |

1. **Canvas scale() 方法**

`scale()` 方法缩放当前绘图，更大或更小，使用方式类似于 css 中 scale，可以设置缩放当前绘图的宽度、高度 (1=100%, 0.5=50%, 2=200%, 依次类推)。

```html
<body>
  <canvas id="canvas" width="400" height="400"></canvas>

  <script type="text/javascript">
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    ctx.strokeRect(5, 5, 25, 15); // 黑色实线区域部分
    ctx.scale(2, 2);
    ctx.fillStyle = "#f2928f";
    ctx.fillRect(5, 5, 25, 15); // 粉色填充区域
    ctx.scale(2, 2);
    ctx.fillStyle = "#409eff"; // 蓝色填充区域
    ctx.fillRect(5, 5, 25, 15);
  </script>
</body>
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230318170650602-49623755.png"  /></p>

注释：**如果您对绘图进行缩放，所有之后的绘图也会被缩放。** 定位也会被缩放。如果您写 scale(2,2)，那么绘图将定位于距离画布左上角两倍远的位置。因此在同一位置绘制大小相同的矩形后，左上角坐标会逐渐偏移，最终呈现如图所示的效果。

2. **Canvas rotate() 方法**

rotate() 方法旋转当前的绘图，可以设置旋转的角度，以弧度计算。注意：这里旋转的是整张画布而不是单独的某个图形。

```html
<body>
  <canvas id="canvas" width="400" height="400"></canvas>

  <script type="text/javascript">
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    ctx.fillStyle = "#409eff";
    ctx.fillRect(50, 20, 150, 50);
    ctx.rotate((20 * Math.PI) / 180);
    ctx.fillRect(50, 50, 150, 50);
  </script>
</body>
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230318173451490-1548866476.png"  /></p>

3. **Canvas translate() 方法**

translate() 方法重新映射画布上的 (0,0) 位置。

注释：当您在 translate() 之后调用诸如 fillRect() 之类的方法时，值会添加到 x 和 y 坐标值上。

```html
<body>
  <canvas id="canvas" width="400" height="400"> 这里是一个canvas </canvas>

  <script type="text/javascript">
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    ctx.fillStyle = "#409eff";
    ctx.fillRect(10, 10, 100, 50);
    ctx.translate(70, 70);
    ctx.fillRect(10, 10, 100, 50);
  </script>
</body>
```

在我们将矩形的坐标偏移 70 后，它会加上原有矩形坐标值，因此新矩形的左上角坐标就是`(80,80)`。

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230318174100890-1907794499.png"  /></p>

4. **Canvas transform() 方法**

`transform()` 允许您缩放、旋转、移动并倾斜当前的环境。transform() 方法的行为相对于由 rotate()、scale()、translate() 或 transform() 完成的其他变换。例如：如果您已经将绘图设置为放到两倍，则 transform() 方法会把绘图放大两倍，您的绘图最终将放大四倍。

```html
<body>
  <canvas id="canvas" width="400" height="200"> 这里是一个canvas </canvas>

  <script type="text/javascript">
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    ctx.fillStyle = "yellow";
    ctx.fillRect(0, 0, 250, 100);

    ctx.transform(1, 0.5, -0.5, 1, 30, 10);
    ctx.fillStyle = "red";
    ctx.fillRect(0, 0, 250, 100);

    ctx.transform(1, 0.5, -0.5, 1, 30, 10);
    ctx.fillStyle = "blue";
    ctx.fillRect(0, 0, 250, 100);
  </script>
</body>
```

绘制一个矩形；通过 `transform()` 添加一个新的变换矩阵，再次绘制矩形；添加一个新的变换矩阵，然后再次绘制矩形。请注意，每当您调用 `transform()` 时，它都会在前一个变换矩阵上构建,最终展示入下的效果：

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202303/2332774-20230318175914052-1158920416.png"  /></p>

---

#### 6. 文本

想要在画布上设置文字，就可以使用`文本`相关的 API 了。大多数文本属性使用的语法与 CSS font 属性相同，而绘制文字的方法与绘制其它图形的方式大同小异这里我们就不做一一介绍了。

|                                   属性                                    |                  描述                  |
| :-----------------------------------------------------------------------: | :------------------------------------: |
|         [font](https://www.w3school.com.cn/jsref/canvas_font.asp)         |    设置或返回文本内容的当前字体属性    |
|    [textAlgin](https://www.w3school.com.cn/jsref/canvas_textalign.asp)    |    设置或返回文本内容的当前对齐方式    |
| [textBaseline](https://www.w3school.com.cn/jsref/canvas_textbaseline.asp) | 设置或返回绘制文本时使用的当前文本基线 |

|                                   方法                                    |            描述            |
| :-----------------------------------------------------------------------: | :------------------------: |
|    [fillText()](https://www.w3school.com.cn/jsref/canvas_filltext.asp)    | 在画布上绘制“被填充的”文本 |
|  [strokeText()](https://www.w3school.com.cn/jsref/canvas_stroketext.asp)  |  在画布上绘制文本(无填充)  |
| [measureText()](https://www.w3school.com.cn/jsref/canvas_measuretext.asp) | 返回包含指定文本宽度的对象 |

`文本`元素最常见的使用就是来绘制移动弹幕了，下面我们就来实现一个简单的弹幕效果。

```html
<body>
  <canvas id="canvas" width="600" height="600"></canvas>

  <script>
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    //设置字体样式
    ctx.font = "50px 微软雅黑";

    // 设置阴影
    ctx.shadowBlur = 20;
    ctx.shadowColor = "rgba(0,0,0,1)";
    ctx.shadowOffsetX = 10;
    ctx.shadowOffsetY = 0;

    let x = 600;
    setInterval(() => {
      //清空画布
      ctx.clearRect(0, 0, 600, 600);
      x -= 10;
      if (x < -100) x = 600;
      //填充字体内容
      ctx.fillText("HelloWorld", x, 100);

      //设置描边内容
      ctx.strokeText("代码改变世界", x, 200);
    }, 16);
  </script>
</body>
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202302/2332774-20230227205648940-647508217.gif"  /></p>

#### 7. 图像绘制

`drawImage()` 方法在画布上绘制图像、画布或视频。也能够绘制图像的某些部分，以及/或者增加或减少图像的尺寸。`drawImage()`的应用多种多样这里我们只介绍其基础应用，它可以配合其它属性与方法，完成更炫酷的效果。

|                                 属性                                  |             描述             |
| :-------------------------------------------------------------------: | :--------------------------: |
| [drawImage()](https://www.w3school.com.cn/jsref/canvas_drawimage.asp) | 向画布上绘制图像、画布或视频 |

```html
<body>
  <canvas id="canvas" width="400" height="400"></canvas>
  <div style="display: flex">
    <canvas id="canvasVideo" width="400" height="400"></canvas>
    <video
      id="video"
      width="400"
      height="400"
      src="./component.mp4"
      controls
    ></video>
  </div>

  <script>
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    // 绘制图像
    // ctx.drawImage(图片对象,X位置,Y位置)
    // ctx.drawImage(图片对象,X位置,Y位置,宽度,高度)
    // ctx.drawImage(图片对象,图片的裁剪位置x,图片裁剪的y位置,裁剪的宽度,裁剪的高度,X位置,Y位置,宽度,高度)
    let img = new Image(); //可以一次加载多个图片
    img.src = "./canvas.png";
    img.onload = function () {
      // 图片载入后再绘制
      ctx.drawImage(img, 0, 0, 100, 100, 50, 50, 300, 300);
      ctx.fillText("waterMark", 300, 350); //设置水印
    };
    let img2 = new Image(); //可以一次加载多个图片
    img2.src = "./CanvasLogo.png";
    img2.onload = function () {
      // 图片载入后再绘制
      ctx.drawImage(img2, 50, 50, 150, 50);
    };

    let canvasVideo = document.getElementById("canvasVideo");
    let interId;
    let ctxVideo = canvasVideo.getContext("2d");
    let video = document.getElementById("video");
    video.onplay = function () {
      interId = setInterval(() => {
        ctxVideo.clearRect(0, 0, 500, 500);
        ctxVideo.fillRect(0, 0, 350, 500);
        ctxVideo.drawImage(video, 0, 160, 350, 60);
        ctxVideo.font = "20px";
        ctxVideo.strokeStyle = "#999";
        ctxVideo.strokeText("水印", 10, 180);
      }, 16);
    };
    video.onpause = function () {
      clearInterval(interId);
    };
  </script>
</body>
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202306/2332774-20230606172522512-379734933.png"  /></p>

如上下图所示我们使用`drawImage()`方法绘制了一个嵌套的图形以及视频元素。

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202306/2332774-20230606172736068-1657463351.gif"  /></p>

---

需要注意的是，在 Canvas 中，宽度和高度定义了 HTML 元素可以画在其中的区域的大小。这些属性在 width 和 height 属性中设置，通常使用像素（px）表示。Canvas 的实际显示大小由 CSS 控制，通过将 width 和 height 属性指定为相同或不同的值来实现。与之不同，CSS style 中的宽度和高度属性定义了元素应该有多大，以便用于定位和布局目的，以及指定应该向用户显示的内容区域的大小。在绝大多数情况下，这两者的值应该相等，但在少数情况下可能需要进行微调来实现最佳效果。简而言之，Canvas 的宽高设置会影响到内部的图形，而 style 中的宽高设置只会影响画板的尺寸。

#### 8.像素操作

实际上我们用`canvas`创建的元素都是由一个个像素点组成的，只要拿到对应的值再搭配相关的方法，对于`canvas`元素的可操作性就大大增加了。

|                                  属性                                   |                        描述                         |
| :---------------------------------------------------------------------: | :-------------------------------------------------: |
|  [width](https://www.w3school.com.cn/jsref/canvas_imagedata_width.asp)  |              返回 ImageData 对象的宽度              |
| [height](https://www.w3school.com.cn/jsref/canvas_imagedata_height.asp) |              返回 ImageData 对象的高度              |
|   [data](https://www.w3school.com.cn/jsref/canvas_imagedata_data.asp)   | 返回一个对象，其包含指定的 ImageData 对象的图像数据 |

|                                       方法                                        |                           描述                            |
| :-------------------------------------------------------------------------------: | :-------------------------------------------------------: |
| [createImageData()](https://www.w3school.com.cn/jsref/canvas_createimagedata.asp) |              创建新的、空白的 ImageData 对象              |
|    [getImageData()](https://www.w3school.com.cn/jsref/canvas_getimagedata.asp)    | 返回 ImageData 对象，该对象为画布上指定的矩形复制像素数据 |
|    [putImageData()](https://www.w3school.com.cn/jsref/canvas_putimagedata.asp)    |       把图像数据(从指定的 ImageData 对象)放回画布上       |

```html
<body>
  <canvas id="canvas" width="170" height="115"></canvas>

  <canvas id="canvas2" width="200" height="200"></canvas>

  <script>
    /* 
    width="200" height="200" 内容的分辨率
    style="width: 300px; height: 300px" 元素的宽高
    */
    let canvas = document.getElementById("canvas");
    let ctx = canvas.getContext("2d");

    let canvas2 = document.getElementById("canvas2");
    let ctx2 = canvas2.getContext("2d");

    let img = new Image();
    img.src = "./canvas.png";
    img.onload = function () {
      ctx.drawImage(img, 0, 0);
      for (let j = 0; j < 3; j++) {
        let imgData = ctx.getImageData(0, j, 100, 100);
        console.log("图像数据：", imgData);
        let imgData2 = ctx2.createImageData(28, 28);
        console.log("图像数据2：", imgData2);

        for (let i = 0; i < 300 * 4; i++) {
          imgData2.data[i] = imgData.data[i];
        }
        ctx2.putImageData(imgData, 0, j * 28, 0, 0, 28, 28);
      }
    };
  </script>
</body>
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202306/2332774-20230606180039308-1043591609.png"  /></p>

在这段代码中，我们首先用一张图片填充一个`canvas`元素，再获取到它的图像数据填充到另一个`canvas元素上`，实现了如下图所示的效果。而获取到的图像信息也如上图所示，是由像素点组成的数组，每四个为一组代表了颜色的`rgba`值，正是这些颜色值组成了像素点，最后再组成了我们的图像。

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202306/2332774-20230606175645831-545379565.png"  /></p>

#### 9. 合成

|                                  属性                                   |                  描述                  |
| :---------------------------------------------------------------------: | :------------------------------------: |
| [globalAlpha](https://www.w3school.com.cn/jsref/canvas_globalalpha.asp) | 设置或返回绘图当前的 alpha 或者透明度  |
|                       [globaCompositeOperation]()                       | 设置或返回新图像如何绘制到已有的图像上 |

`globalAlpha()`方法主要用来设置绘图的透明度，使用比较简单，效果如下图所示。

```js
var c = document.getElementById("myCanvas");
var ctx = c.getContext("2d");
ctx.fillStyle = "red";
ctx.fillRect(20, 20, 75, 50);
// 调节透明度
ctx.globalAlpha = 0.2;
ctx.fillStyle = "blue";
ctx.fillRect(50, 50, 75, 50);
ctx.fillStyle = "green";
ctx.fillRect(80, 80, 75, 50);
```

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202306/2332774-20230607162121805-616973134.png"  /></p>

> 下面我们着重介绍`globaCompositeOperation()`方法的使用。

**定义和用法**
globaCompositeOperation 属性设置或返回如何将一个源(新的)图像绘制到目标(已有的)图像上。因此通过多个绘图元素的叠加以及交互，我们就能实现更加复杂精巧的图像动画了。

- 源图像 = 您打算放置到画布上的绘图
- 目标图像 = 您已经放置在画布上的绘图

|      方法       |                 source-over                  |
| :-------------: | :------------------------------------------: |
| javaScript 语法 | context.globalCompositeOperation="source-in" |

**属性值**

|        值        |                                       描述                                       |
| :--------------: | :------------------------------------------------------------------------------: |
|   source-over    |                           默认，在目标图像上显示源图像                           |
|   source-atop    |           在目标图像顶部显示源图像，源图像位于目标之外的部分是不可见的           |
|    source-in     |  在目标图像中显示源图像，只有在目标图像之内的源图像部分会显示，目标图像是透明的  |
|    source-out    | 在目标图像之外显示源图像 ，只有目标图像之外的源图像部分会显示，目标图像是透明的  |
| destination-over |                              在源图像上显示目标图像                              |
| destination-atop |        在源图像顶部显示目标图像，目标图像位于源图像之外的部分是不可见的。        |
|  destination-in  |  在源图像中显示目标图像，只有源图像之内的目标图像部分会被显示，源图像是透明的。  |
| destination-out  | 在源图像之外显示目标图像，只有源图像之外的目标图像部分会被显示，源图像是透明的。 |
|     lighter      |                               显示源图像+目标图像                                |
|       copy       |                             显示源图像，忽略目标图像                             |
|       xor        |                      使用异或操作对源图像与目标图像进行组合                      |

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      #ggk {
        width: 400px;
        height: 100px;
        position: relative;
      }
      #ggk .jp {
        width: 400px;
        height: 100px;
        position: absolute;
        user-select: none;
        text-align: center;
        left: 0;
        top: 0;
      }
      #ggk #canvas {
        width: 400px;
        height: 100px;
        position: absolute;
        left: 0;
        top: 0;
      }
    </style>
  </head>
  <body>
    <div id="ggk" class="ggk">
      <div id="jp" class="jp">谢谢惠顾</div>
    </div>
    <script>
      const ggk = document.getElementById("ggk");
      const canvas = document.createElement("canvas");
      canvas.width = 400;
      canvas.height = 100;
      canvas.id = "canvas";
      document.getElementById("ggk").append(canvas);

      const context = canvas.getContext("2d");
      context.fillStyle = "darkgrey";
      context.fillRect(0, 0, 400, 100);
      context.fillStyle = "#fff";
      context.font = "20px 宋体";
      context.fillText("刮刮卡", 180, 40);

      let isDraw = false;
      canvas.onmousedown = function () {
        isDraw = true;
      };

      //移动的时候绘制圆形，将源图像的目标内容给清除掉
      canvas.onmousemove = function (e) {
        if (!isDraw) return;
        let x = e.pageX - ggk.offsetLeft;
        let y = e.pageY - ggk.offsetTop;
        context.globalCompositeOperation = "destination-out";
        context.arc(x, y, 20, 2 * Math.PI, false);
        context.fill();
      };

      canvas.onmouseup = function () {
        isDraw = false;
      };

      let jangp = [
        { context: "一等奖：1000", p: 0.1 },
        { context: "二等奖：500", p: 0.4 },
        { context: "三等奖：100", p: 0.5 },
      ];
      let randomNum = Math.random();
      let jpDom = document.getElementById("jp");
      if (randomNum < jangp[0].pageX) {
        jpDom.innerHTML = jangp[0].context;
      } else if (randomNum < jangp[1].p + jangp[0].p) {
        jpDom.innerHTML = jangp[1].context;
      } else if (randomNum < jangp[1].p + jangp[2].p) {
        jpDom.innerHTML = jangp[2].context;
      }
    </script>
  </body>
</html>
```

使用`globaCompositeOperation()`实现一个简单的刮刮卡效果如下。

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202306/2332774-20230607162808674-1355694989.gif"  /></p>

#### 其它

还有一些其它非常有用的方法需要我们在实践中大量练习使用。

|     方法      |                                                                        描述                                                                         |
| :-----------: | :-------------------------------------------------------------------------------------------------------------------------------------------------: |
|    save()     |                                                                 保存当前环境的状态                                                                  |
|   restore()   |                                                           返回之前保存的路径状态和属性。                                                            |
| createEvent() |                          createEvent() 允许我们在 Canvas 上创建自定义事件，以便在将来事件触发时执行特定的 JavaScript 代码                           |
| getContext()  |                               getContext()是 Canvas 对象的一个属性，通过该属性可以获得一个用于绘制的上下文(context)。                               |
|  toDataURL()  | toDataURL() 是 HTMLCanvasElement 接口的方法之一，它可以将 Canvas 对象转换成一个 base64 编码的图片地址，以便我们在网页上显示或者将其保存为图片文件。 |

> 使用**canvas** 绘制一个钟表

<p align=center><img src="https://img2023.cnblogs.com/blog/2332774/202306/2332774-20230607164708640-1522803690.gif"  /></p>

### 总结

总结一下，在渲染模式上，Canvas 站在了 DOM 的对面，浏览器对其内容一无所知，一切渲染的权利回到了开发者的手上，这个改变带来了显著的性能优势。此外，我们可以使用 Canvas 绘制种类更为丰富的 UI 元素，如线形、特殊图形等，通过画法逻辑，还可以实现更加精准的 UI 界面渲染，解决了浏览器差异造成的样式误差，让更多应用场景可以顺利迁移到 Web 平台上来。
