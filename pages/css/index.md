# CSS

## 1.link 和@import 的区别

两者都是外部引用 CSS 的方式，它们的区别如下：

- link 是 XHTML 标签，除了加载 CSS 外，还可以定义 RSS 等其他事务；@import 属于 CSS 范畴，只能加载 CSS。
- link 引用 CSS 时，在页面载入时同时加载；@import 需要页面网页完全载入以后加载。
- link 是 XHTML 标签，无兼容问题；@import 是在 CSS2.1 提出的，低版本的浏览器不支持。
- link 支持使用 Javascript 控制 DOM 去改变样式；而@import 不支持。

## 2. display:none 与 visibility:hidden 的区别

- **在渲染上：**
  - display:none 会让元素完全从渲染树中消失，渲染时不会占据任何空间； visibility:hidden 不会让元素从渲染树中消失，渲染的元素还会占据相应的空间，只是内容不可见。
  - display 通常会造成文档的重排，visibility 属性只会造成元素的重绘；
- **在继承上：**
  - display:none 是非继承属性；visibility:hidden 是继承属性，子孙节点消失是由于继承了 hidden，通过设置 visibility:visible 可以让子孙节点显示；

## 3. 对 requestAnimationframe 的理解

- **是什么：**
  它是告诉浏览器你想要执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数来更新这个动画。requestAnimationframe 需要传入一个回调函数作为参数，然后这个回调函数会在浏览器下一次重绘之前执行。
- **原理：**
  requestAnimationframe 是严格跟着系统的刷新率（帧率）来的，如果系统的刷新率是 60Hz，那么他的回调执行的间隔就是 1000/60，如果系统的刷新率是 75Hz，那么他的回调的执行间隔就是 1000/75。

- **使用：**
  window.requestAnimationFrame(callback)
- **取消：**
  使用 cancelAnimationFrame()来取消执行动画，该方法接收一个参数——requestAnimationFrame 默认返回的 id，只需要传入这个 id 就可以取消动画了
- **优点：**
  - **相对节能：**使用 SetTinterval 实现的动画，当页面被隐藏或最小化时，SetTinterval 仍然在后台执行动画任务。而 RequestAnimationFrame 在页面处理未激活的状态下，屏幕的刷新任务会被暂停。
  - **函数节流：**RequestAnimationFrame 可保证每个刷新间隔内，函数只被执行一次。
  - **减少 DOM 操作：**打个比方，你有一个 setInterval，间隔设定 1000/60，并且 setInterval 进行了很多次 dom 操作，那么，回调被执行的时候会触发很多次的计算布局和生成绘制命令，但是，requestAnimationframe 只会集中起来进行一次布局和绘制。

## 4. 盒模型

**盒模型都是由四个部分组成的，分别是 margin、border、padding 和 content**

- **标准模型和 IE 模型的宽高计算方式**
  - 标准盒模型的 width 和 height 属性只包含了 content，
  - IE 盒模型的 width 和 height 属性包含了 border、padding 和 content。
- **box-sizing 的取值**
  - content-box: 标准盒模型（默认值）
  - border-box: IE 盒模型

## 5. translate

## 6. CSS3 新增了哪些特性

- 新增了各种 CSS 选择器
- 圆角 border-radius
- 旋转 transform
- 动画，缩放....

## 7. 什么是物理像素，逻辑像素和像素密度

- **物理像素：**
  物理像素指的是设备的真实像素。比如你的手机分辨率是 1920x1080，意味着你的手机横向有 1920 个像素点。
- **逻辑像素：**
  而逻辑像素可以理解成是 CSS 像素。指的是设备渲染的真实像素，比如，虽然你的手机分辨率支持 1920x1080。但是如果你设置的分辨率是 1280x720，那你的逻辑像素就是 1280x720。
- **像素密度（DPR）：**
  像素密度可以使用公式 PPI = √（横向像素数量 ² + 纵向像素数量 ²） / 屏幕对角线长度（英寸）来计算。
  比如，你的设备分辨率是 1920x1080，大小是 6 英寸，那你的像素密度是 PPI = √（1920² + 1080²）/ 6 ≈ 367.11。~~那 1px 占用的物理像素点是 367/96=3.82 个~~

## 8. 两栏布局实现

- **利用浮动 float：**

```css
.parent {
  height: 100px;
}
.left {
  float: left;
  width: 200px;
}
.right {
  margin-left: 200px;
  width: auto;
}
```

- **利用 flex 布局：**

```css
.parent {
  display: flex;
}
.left {
  width: 200px;
}
.right {
  flex: 1;
}
```

- **利用绝对定位：**

```css
.parent {
  position: relative;
}
.left {
  position: absolute;
}
.right {
  margin-left: 200px;
}
```

## 9. 三栏布局实现

- **利用绝对定位：**

```css
.outer {
  position: relative;
}
.left {
  position: absolute;
}
.right {
  position: absolute;
  right: 0;
}
.center {
  margin-left: 100px; /*看情况设不设*/
  margin-right: 200px; /*看情况设不设*/
}
```

- **利用 flex 布局：**

```css
.outer {
  display: flex;
}

.left {
  width: 100px;
}

.right {
  width: 100px;
}

.center {
  flex: 1;
}
```

- **圣杯布局：**

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>圣杯布局</title>
    <style>
      #bd {
        padding: 0 200px 0 180px;
        height: 100px;
      }
      #center {
        float: left;
        width: 100%;
        height: 500px;
        background: yellow;
      }
      #left {
        float: left;
        position: relative;
        width: 180px;
        height: 500px;
        margin-left: -100%;
        background: #00cc99;

        left: -180px;
      }
      #right {
        float: left;
        position: relative;
        width: 200px;
        height: 500px;
        margin-left: -200px;
        background: #ff0000;

        right: -200px;
      }
    </style>
  </head>
  <body>
    <div id="bd">
      <div id="center">center</div>
      <div id="left">left</div>
      <div id="right">right</div>
    </div>
  </body>
</html>
```

- **双飞翼布局：**

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>双飞翼布局</title>
    <style>
      #center {
        float: left;
        width: 100%; /*左栏上去到第一行*/
        height: 200px;
        background: blue;
      }
      #left {
        float: left;
        width: 180px;
        height: 200px;
        margin-left: -100%; /*left这个时候在center后面，而且是新的一行。-100%相当于把left拉回原位*/
        background: yellow;
      }
      #right {
        float: left;
        width: 200px;
        height: 200px;
        margin-left: -200px; /*等于宽度*/
        background: tomato;
      }

      /*给内部div添加margin，把内容放到中间栏，其实整个背景还是100%*/
      #inside {
        margin: 0 200px 0 180px;
        height: 100px;
      }
    </style>
  </head>
  <body>
    <div id="center">
      <div id="inside">center</div>
    </div>
    <div id="left">left</div>
    <div id="right">right</div>
  </body>
</html>
```

## 10. 水平垂直居中方案

- **利用绝对定位：**

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: -50px; /* 自身 height 的一半 */
  margin-left: -50px; /* 自身 width 的一半 */
}
```

- **还有一种绝对定位：**

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}
```

- **利用 flex：**

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

## 11. flex 布局

**1. flex: 1**

- flex:1 等于 flex:1 1 0%
- flex:1 是 flex-grow， flex-shrink， flex-basis 的缩写
  - flex-grow：定义项目的放大比例，默认为 0。表示如果存在剩余空间，也不放大
  - flex-shrink：定义了项目的缩小比例，默认为 1。表示如果空间不足，该项目将缩小
  - flex-basis：给上面两个属性分配多余空间之前, 计算项目是否有多余空间, 默认值为 auto, 即项目本身的大小；设置为 auto 的时候，会根据盒子内容的多少自动撑开盒子。
- flex: auto 等于 flex:1 1 auto。
- flex: 0 等于 flex:0 1 0。

## 12. BFC

- **开启 BFC 的方式：**
  - 设置浮动
  - absolute/fixed
  - inline-block
  - 将 overflow 设置为 hidden 是副作用最小的开启 BFC 的方式

## 13. position 定位

- relative:生成相对定位的元素，相对于其正常位置进行定位
- absolute:生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位

## 14. sticky 布局

- 父元素设置了 overflow:hidden 等属性，可能会影响到 sticky 布局的效果。
- 父元素设置了 transform、filter 等属性，这些属性会使得元素进入了合成层，导致 sticky 布局失效。
- 元素的高度超出了可视区域，超出部分将被裁剪，导致 sticky 布局失效。
- 元素所在的容器没有设置高度，如果没有设置高度，元素会一直保持 sticky 状态，直到滚动到页面底部。
- 如果使用了相对定位或绝对定位，需要确保定位值的正确性，否则可能会导致 sticky 布局失效。
- 如果父元素的高度小于元素的高度，元素的底部将无法出现在父元素的底部，而是粘在父元素的顶部，导致 sticky 布局失效。

## 15. 如何解决 1px 问题

- window.devicePixelRatio 是设备上物理像素和设备独立像素(device-independent pixels (dips))的比例。
- window.devicePixelRatio = 设备的物理像素 / CSS 像素。
**1. 直接写 0.5px：**

  (1) `<div id="container" data-device={{window.devicePixelRatio}}></div>`

  (2) `#container[data-device="2"] {border:0.5px solid #333}`

**2. viewport**

  `<meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">`
  用 js 这样实现：

  ```javascript
  const scale = 1 / window.devicePixelRatio;
  // 这里 metaEl 指的是 meta 标签对应的 Dom
  metaEl.setAttribute(
    "content",
    `width=device-width,user-scalable=no,initial-scale=${scale},maximum-scale=${scale},minimum-scale=${scale}`
  );
  ```
  **缺点：**
   一些不需要被缩小的内容，比如文字、图片等，也被缩小了

## 16. 开启 GPU 加速

- **transform：** translate3d() rotate3d() scale3d()。如果不需要 3d 变换：可以使用 transform: translateZ(0)
- **opacity：** 设置 opacity 透明度小于 1。因为浏览器会将该元素放入一个单独的合成层中，这个合成层会在 GPU 上进行渲染，然后提高页面的渲染性能。
- **will-change：**这个我没有用过，只是作为一个了解。从字面意思来看，就是告诉浏览器，我将要“变形”了。通俗点说的话，will-change 属性可以让浏览器提前准备好需要进行变换的元素，并且让浏览器在变换发生之前就为该元素创建一个单独的合成层。但是我觉得不能一直在一个 dom 上使用这个属性，不然的话，就算你的动画不执行或者执行完毕了，浏览器还是会为他创建单独的合成层。应该要动画结束后给他释放掉这个属性。

## 17. 圣杯布局和双飞翼布局

- 圣杯布局缺点：
  - 可能出现重叠问题：当左侧和右侧列的宽度超出中央列时，可能会出现重叠问题，需要使用负边距来解决
- 双飞翼布局缺点：
  - 多写了一层 dom
