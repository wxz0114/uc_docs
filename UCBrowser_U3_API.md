U3内核扩展接口定义
=============

##目录
* [1 编写目的](#1.编写目的)
* [1.1 背景](#1.1 背景)

###1.编写目的
本文对基于U3内核进行的各种扩展进行描述及说明。本文的读者是所有使用U3进行开发的前端开发人员及进行扩展实现的研发人员。
####1.1.背景
随着手机游戏，以及手机应用的迅速发展，这些应用对手机浏览器提出了方方面面的新需求，这些新需求不一定被现有的协议规范所定义，因此需要通过扩展U3内核的功能来为这些应用提供支持。本文档汇集了所有U3内核增加的独有的或尚未被规范定义的扩展功能，方便使用U3内核进行开发的前端开发人员使用。
<br>
<br>
###2.功能扩展简表
| 扩展功能分类  |功能细项|Meta支持|Js支持|补充说明|
| ---------- | -----------|-----------|-----------|-----------|
| 全屏   |    |    8.6|    未来版本|   |
| 屏幕方向   |    |    8.6|   未来版本|   |
| 手势开关   |longpressMenu, gesture, flipbutton, menu|    8.6|   未来版本|   |
| 适屏排版   |    |    8.5patch|   无|   |
| 发送桌面图标   |    |    8.6|   无|   |
| 浏览器识别   |    |    8.6|   8.2|   |
<br>
<br>
###3.功能扩展
####3.1.屏幕方向(screen orientation)
#####3.1.1.规范状态
http://www.w3.org/TR/screen-orientation/
#####3.1.2.定义
提供对屏幕方向特性的判断，事件及操纵，包括：<br>
* 1. 定义screen对象，可读取当前屏幕的方向<br>
* 2. 通过screen对象可对屏幕方向进行设置<br>
* 3. 定义屏幕方向改变时的事件处理函数<br>
```javascript
interface screen {
readonly attribute DOMString orientation;
    boolean lockOrientation (DOMtring orientation);
		 void    unlockOrientation ();
    attribute Function onorientationchange;
};
```
#####3.1.3.示例
```javascript
<!DOCTYPE html>
<html>
  <script>
    function show() {
      alert("Screen orientation state is " + screen.orientation);
    }
    screen.addEventListener("orientationchange", show, false);
    show();
    screen.lockOrientation("portrait");
  </script>
</html>
```
#####3.1.4.Meta标签
为了简化调用，可以通过meta对屏幕方向进行设定，效果与使用js调用相关接口是一致的。<br>
```javascript
<meta name="screen-orientation" content="portrait/landscape">
```
等同于<br>
```javascript
screen.lockOrientation("portrait/landscape");
```
#####3.1.5.操作设计
Tab切换：动画按当前也方向，切换后页面按该页面<br>
前进后退：动画按当前页方向，切换后页面按该页面方向进行方向切换显示<br>
系统旋屏设置：本功能仅改变一次屏幕设置，并不永久生效。如果用户通过浏览器->设置->转屏设置对屏幕进行设置，则屏幕依据设置变更进行变化，并相应触发屏幕方向事件。
#####3.1.6.版本变更历史
####3.2.全屏控制(full screen)
#####3.2.1.规范状态
http://dvcs.w3.org/hg/fullscreen/raw-file/tip/Overview.html
#####3.2.2.定义
提供对全屏的操纵，包括：<br>
* 1. 定义在element对象上的进入全屏(requestFullscreen)函数<br>
* 2. 定义在document对象上的退出全屏(exitFullscreen)函数<br>
* 3. 定义在document对象上读取全屏状态及当前全屏元素<br>
* 4. 定义全屏切换事件
```javascript
partial interface Element{
  void requestFullscreen();
}

partial interface Document {
  readonly attribute boolean fullscreenEnabled;
	Readonly attribute Element fullscreenElement;
  void exitFullscreen();
}
```
#####3.2.3.示例
```javascript
<!DOCTYPE html>
<html>
  <script language="text/javascript">
    var docElm = document.documentElement;
    if (docElm.requestFullscreen) {
      docElm.requestFullscreen();
}
If (docElm.fullscreenEnabled){
  docElem.exitFullscreen();
}
  </script>
</html>
```
#####3.2.4.Meta标签
```javascript
<meta name="full-screen" content="yes">
```
功能等同于
```javascript
Var docElm = document.documentElement;
docElem.requestFullscreen();
```
#####3.2.5.操作设计
全屏特性目前定义的交互及操作特性与应用全屏是一致的
#####3.2.6.版本变更历史
* 8.8变更
去除8.6/8.7版本中定义的全屏功能的meta标签包括的禁止手势，禁止悬浮框，禁止长按菜单的设计。<br>
去除了苹果特有的meta支持
###3.3.手势开关
####3.3.1.规范状态
暂无
####3.3.2.定义
U3为手机浏览器定义了一些特有的操作方式，这些操作方式包括：
* 手势：单指前进后退，双指操作(新增/删除/切换tab)
* 长按菜单
* 全屏时的悬浮按钮(暂未提供)
* 翻页按钮
* 物理菜单键
提供对手势的控制功能，包括：<br>
1. 定义浏览器控制对象：navigator.control
2. 为navigator.control提供手势、悬浮按钮等操作
```javascript
Interface control{
  Boolean gesture(boolean enable/disable);
  Boolean longpressMenu(boolean enable/disable);
  Boolean flipbutton()
  Boolean menu(boolean enable/disable);
}
```
####3.3.3.示例
####3.3.4.Meta标签
暂不提供
####3.3.5.操作设计
所有设置仅对当前页面有效，也不影响前进后退列表中的网页。<br>
手势禁止时，在当前页面中手势事件应传递给网页进行处理。通过以下方式达到本页面时会导致无法继续使用该操作：<br>
1. 使用双指切换从另一个tab切换到被禁止手势事件的当前页面<br>
2. 使用单指前进后退时遇到被禁止手势事件的当前页面<br>
