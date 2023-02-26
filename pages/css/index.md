# CSS
## link和@import的区别
两者都是外部引用CSS的方式，它们的区别如下：

- link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；@import属于CSS范畴，只能加载CSS。
- link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。
- link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。
- link支持使用Javascript控制DOM去改变样式；而@import不支持。

## 2. display:none与visibility:hidden的区别
- **在渲染上：**
  - display:none会让元素完全从渲染树中消失，渲染时不会占据任何空间； visibility:hidden不会让元素从渲染树中消失，渲染的元素还会占据相应的空间，只是内容不可见。
  - display 通常会造成文档的重排，visibility属性只会造成元素的重绘；
- **在继承上：**
  - display:none是非继承属性；visibility:hidden是继承属性，子孙节点消失是由于继承了hidden，通过设置visibility:visible可以让子孙节点显示；

## 3. 对requestAnimationframe的理解
## 4. 盒模型
**盒模型都是由四个部分组成的，分别是margin、border、padding和content**
- **标准模型和IE模型的宽高计算方式**
  - 标准盒模型的width和height属性只包含了content，
  - IE盒模型的width和height属性包含了border、padding和content。
- **box-sizing的取值**
  - content-box: 标准盒模型（默认值）
  - border-box: IE盒模型
## 5. translate
## 6. CSS3新增了哪些特性
- 新增了各种CSS选择器
- 圆角 border-radius
- 旋转 transform
- 动画，缩放....
## 7. 什么是物理像素，逻辑像素和像素密度
-  **物理像素：**
  物理像素指的是设备的真实像素。比如你的手机分辨率是1920x1080，意味着你的手机横向有1920个像素点。
- **逻辑像素：**
  而逻辑像素可以理解成是CSS像素。指的是设备渲染的真实像素，比如，虽然你的手机分辨率支持1920x1080。但是如果你设置的分辨率是1280x720，那你的逻辑像素就是1280x720。
- **像素密度（DPR）：**
  像素密度可以使用公式 PPI = √（横向像素数量² + 纵向像素数量²） / 屏幕对角线长度（英寸）来计算。
  比如，你的设备分辨率是1920x1080，大小是6英寸，那你的像素密度是PPI = √（1920² + 1080²）/ 6 ≈ 367.11。~~那1px占用的物理像素点是367/96=3.82个~~


## 8. 两栏布局实现
## 9. 三栏布局实现
## 10. 水平垂直居中方案
## 11. flex布局
## 12. BFC
## 13. position定位
## 14. sticky布局
## 15. 如何解决 1px 问题