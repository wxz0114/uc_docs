U3内核扩展接口定义
=============

##目录
* [1 编写目的](#1)
* [1.1 背景](#1.1)
* [2 功能扩展简表](#2)
* [3 功能扩展](#3)
* [3.1 屏幕方向](#3.1)
* [3.1.1.规范状态](#3.1.1.规范状态)
* [3.1.2.定义](#3.1.2.定义)
* [3.1.3.示例](#3.1.3.示例)
* [3.1.4.Meta标签](#3.1.4.Meta标签)
* [3.1.5.操作设计](#3.1.5.操作设计)
* [3.1.6.版本变更历史](#3.1.6.版本变更历史)
* [3.2.全屏控制](#3.2.全屏控制)
* [3.2.1.规范状态](#3.2.1.规范状态)
* [3.2.2.定义](#3.2.2.定义)
* [3.2.3.示例](#3.2.3.示例)
* [3.2.4.Meta标签](#3.2.4.Meta标签)
* [3.2.5.操作设计](#3.2.5.操作设计)
* [3.2.6.版本变更历史](#3.2.6.版本变更历史)
* [3.3.手势开关](#3.3.手势开关)
* [3.3.1.规范状态](#3.3.1.规范状态)
* [3.3.2.定义](#3.3.2.定义)
* [3.3.3.示例](#3.3.3.示例)
* [3.3.4.Meta标签](#3.3.4.Meta标签)
* [3.3.5.操作设计](#3.3.5.操作设计)
* [4.排版扩展](#4.排版扩展)
* [4.1.xhtml适应屏幕排版](#4.1.xhtml适应屏幕排版)
* [4.1.1.定义](#4.1.1.定义)
* [4.1.2.示例](#4.1.2.示例)
* [4.1.3.Meta标签](#4.1.3.Meta标签)
* [4.1.4.操作设计](#4.1.4.操作设计)
* [4.2.排版模式](#4.2.排版模式)
* [4.2.1.定义](#4.2.1.定义)
* [4.2.2.标签](#4.2.2.标签)
* [4.2.3.示例](#4.2.3.示例)
* [4.2.4.说明](#4.2.4.说明)
* [4.3.夜间模式](#4.3.夜间模式)
* [4.3.1.定义](#4.3.1.定义)
* [4.3.2.标签](#4.3.2.标签)
* [4.3.3.示例](#4.3.3.示例)
* [4.3.4.说明](#4.3.4.说明)
* [4.4.强制图片显示](#4.4.强制图片显示)
* [4.4.1.定义](#4.4.1.定义)
* [4.4.2.标签](#4.4.2.标签)
* [4.4.3.示例](#4.4.3.示例)
* [4.4.4.说明](#4.4.4.说明)
* [4.5.急速控制](#4.5.急速控制)
* [5.其他](#5.其他)
* [5.1.发送到桌面](#5.1.发送到桌面)
* [5.2.应用模式](#5.2.应用模式)
* [5.2.1.定义](#5.2.1.定义)
* [5.2.2.标签](#5.2.2.标签)
* [5.2.3.示例](#5.2.3.示例)
* [5.2.4.说明](#5.2.4.说明)
* [6.浏览器版本识别](#6.浏览器版本识别)

<br>
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
<br>
<br>
##4.排版扩展
###4.1.xhtml适应屏幕排版
####4.1.1.定义
适应屏幕排版是UC浏览器的一大特色，为广大用户所喜爱，可以有效的避免出现左右滚动条，减少用户的操作次数。但对于xhtml，在小屏幕上的缩放是通过放大缩小显示区域来实现的，使得无法通过适应屏幕来提供更好的视觉效果。<br>
UC浏览器在标准排版效果实现的基础上，提供适应屏幕的排版方式，可以使得xhtml页面在屏幕中进行缩放操作时保持不出现左右滚动条。<br>
要使用这个功能，只需要在viewport中使用uc-fitscreen进行设置即可，默认值为uc-fitscreen=no，即不启用此功能，此时浏览器的缩放行为与标准一致。当设置为uc-fitscreen=yes，则当进行缩放操作时，仅放大图片和文字等页面元素，但不放大屏幕宽度，从而避免了左右滚动条的产生。<br>
####4.1.2.示例
```javascript
<meta name="viewport" content="width=device-width, uc-fitscreen=yes"/>
<meta name="viewport" content="initial-scale=1.0, uc-fitscreen=yes"/>
```
####4.1.3.Meta标签
```javascript
<meta name="viewport" content="uc-fitscreen=yes"/>
```
请根据页面要求配合viewport的其他属性一同使用。
####4.1.4.操作设计
本扩展仅限制了缩放时的有效可视区域为屏幕宽度，并没有对排版方式及规则进行调整，因此当放大达到最大时，可能会在一些表格行中出现少量字体重叠。这时可以通过设定maximum-scale避免太大的放大比例来规避。<br>
基于设计的考虑，当一个页面内出现多个viewport定义时，后出现的viewport将覆盖前面的定义，因此uc-fitscreen这个属性应该和页面的其他viewport属性一起定义使用，而不能单独使用。例如：<br>
```javascript
<!DOCTYPE html '-//WAPFORUM//DTD XHTML Mobile 1.0//EN' 'http://www.wapforum.org/DTD/xhtml-mobile10.dtd'>
<meta name="viewport" content="width=device-width,uc-fitscreen=yes"/>
```
或
```javascript
<!DOCTYPE html '-//WAPFORUM//DTD XHTML Mobile 1.0//EN' 'http://www.wapforum.org/DTD/xhtml-mobile10.dtd'>
<meta name="viewport" content="initial-scale=1.0,uc-fitscreen=yes"/>
```
###4.2.排版模式
####4.2.1.定义
Uc浏览器提供两种排版模式，分别是适屏模式及标准模式，其中适屏模式简化了一些页面的处理，使得页面内容更适合进行页面阅读、节省流量及响应更快，而标准模式则能按照标准规范对页面进行排版及渲染。通过新定义的标签及js api接口，可以让网页设计者执行决定采用何种排版方式向用户展现页面。
####4.2.2.标签
Meta标签
```javascript
<meta name="layoutmode" content="fitscreen/standard" />
```
Js API接口
* 定义当前排版模式的属性值
```javascript
interface layout{
readonly attribute DOMString layoutmode;
};
```
* 定义了
Layoutmode的取值为fitscreen(对应适应屏幕)或standard(对应标准模式)<br>
Onlayoutchange函数将在用户通过菜单切换排版方式时被调用，对每个窗口通知其网页排版发生了变化。<br>
当时用meta标签定义了该页面的排版方式后，该页面的排版将不再受用户的菜单排版方式选择影响。<br>
####4.2.3.示例
* 设置页面排版为标准模式
```javascript
<html>
 <meta name="layoutmode" content="standard"/>
 <body>
  本页面将以标准模式进行排版，用户菜单中的缩放/适屏选择无效
 </body>
</html>
```
* 检测排版模式切换，提示用户
```javascript
<html>
 <meta name="layoutmode" content="fitscreen"/>
 <body>
  <script type="text/javascript">
Function handlelayout(newmode)
{
  confirm("当前页面排版固定为适屏模式，切换到标准模式请点击xxx链接");
 }
    Layout.onlayoutchange = handlelayout;
  </script>
 </body>
</html>
```
####4.2.4.说明
当不设置layoutmode时，排版方式按用户配置的浏览器选项决定，且可随用户菜单变化而更改。<br>
Meta设置对当前页面生效，包括当前页面下属的iframe/frame，但不影响前进后退的页面<br>
非当前窗口的事件回调函数将实时产生<br>
页面内的Frame/iframe中的排版meta属性不生效。<br>
排版切换的事件将通知到页面中的frame/iframe<br>
###4.3.夜间模式
####4.3.1.定义
夜间模式是UC浏览器的特色功能，帮助用户在低亮度或黑暗情况下更舒适的进行页面浏览。由于基于网页的应用愈加复杂，由浏览器实现的单一夜间模式不一定能够适应所有情况(例如游戏应用)，因此UC浏览器允许网页设计者对其设计的页面禁用浏览器的夜间模式，自行设计更适合用户使用的夜间模式。
####4.3.2.Meta标签：
```javascript
<meta name="nightmode" content="enable/disable"/>
```
Content=disable时的含义：<br>
* 禁止页面使用uc浏览器自定义的夜间模式，进入夜间模式时的表现同日间模式
Content=enable时的含义：
* 允许页面使用uc浏览器自定义的夜间模式，用于取消<meta name="nightmode" content="disable"/>时的效果
Js接口：
* 定义夜间模式的事件处理函数
* 定义夜间模式的属性值
```javascript
interface layout{
readonly attribute DOMString displaymode;
    Function onnightmodechange(DOMString newmode);
};
```
displaymode是一个以页面为单位的变量，取值为day/night，分别对应日间模式及夜间模式，与整个浏览器的夜间模式取值相同。<br>
在浏览器进入或退出夜间模式时，向所有窗口发出该事件，并调用onnightmodechange函数，传入参数为将要变更的displayMode值：<br>
####4.3.3.示例
* 禁止进入夜间模式
```javascript
<!DOCTYPE html>
<html>
  <meta name="nightmode" content="disable"/>
  <body>
	   <p>这个页面不会进入夜间模式。
	<body>
</html>
```
* 提示用户进入夜间模式
```javascript
<!DOCTYPE html>
<html>
  <body>
  <p>当前的夜间模式：<div id="isNight"><script>document.write(layout.displaymode)</script></div>
  <script language="text/javascript">
function handlenightmode(newmode)
{
  ret_val = true;
  //当前模式为off，即正在转换为夜间模式
  if (newmode == "night"){
   alert("进入了夜间模式");
	}
  document.getElementById("isNight").innerHTML= layout.displaymode;
}
layout.onnightmodechange = handlenightmode;
}
    
  </script>
  </body>
</html>
```
* 自定义夜间模式
```javascript
<!DOCTYPE html>
<html>
	<meta name="nightmode" content="disable"/>
  <body>
  <p>当前的夜间模式：<div id="isNight"><script>document.write(layout.nightmode)</script></div>
  <script language="text/javascript">
function handlenightmode(newmode)
{
  ret_val = false;
  //从日间模式切换到夜间模式
  if (layout.nightmode == "night")
  {
   alert("正在使用页面自定义的夜间模式");
   //切换到页面自定义的夜间模式样式表
  }
  else //从夜间模式切换到日间模式
  {
    //取消页面自定义的夜间模式样式表
  }
 }
 layout.onnightmodechange = handlenightmode;       
 document.getElementById("isNight").innerHTML= layout.nightmode;
}    
  </script>
  </body>
</html>
```
####4.3.4.说明
页面内的Frame/iframe中的夜间模式的meta不生效。<br>
用户的夜间模式切换事件将通知到页面内的frame/iframe<br>
###4.4.强制图片显示
####4.4.1.定义
为了节省流量及加快速度，UC为用户提供了无图模式，在实际使用中存在页面中的图片是不可缺少的，例如验证码，地图等。通过强制图片显示的功能可以保证图片显示不受用户的设置影响。<br>
此功能分为两部分：整页强制图片显示及单个图片强制显示<br>
整页强制图片显示，通过meta标签通知浏览器该页内所有图片均需加载，用户设置的无图选项失效。<br>
单个图片强制显示，为img标签增加一个属性，该属性强制对应图片对象进行加载，忽视用户设置的无图选项，但不影响页面内的其他图片对象处理。<br>
####4.4.2.标签
Meta标签：
```javascript
<meta name="imagemode" content="force"/>
```
Img标签：
```javascript
<img src="..." show="force">
```
####4.4.3.示例
* 整页强制图片显示
```javascript
<html>
  <meta name="imagemode" content="force"/>
  <body>
  <img src="sample1.jpg"/>
  <img src="sample2.jpg"/>
  <img src="sample3.jpg"/>
  <p>以上图片不会受无图模式影响，强制显示
  </body>
</html>
```
* 单幅图片强制显示
```javascript
<html>
 <body>
  <img src="sample1.jpg"/>
  <p>这个图片在无图模式下会不显示
  <img src="sample2.jpg" show="force"/>
  <p>这个图片在无图模式下也会显示
  <img src="sample3.jpg" id=testimg/>
  <a href="javascript:document.getElementById("testimg").show="force";">设置强制显示</a>
  <p>这个图片在点击按钮后会强制显示
 </body>
</html>
```
####4.4.4.说明
在应用模式下，自动强制页面进入整页图片强制显示<br>
整页图片强制显示仅对当前页面生效，对页面内的iframe/frame不生效，也不影响前进后退的页面<br>
####4.4.5.极速模式控制
<br>
<br>
##5.其他
###5.1.发送到桌面
对于以下的标签，在用户通过添加书签->发至桌面快捷方式时，联网获取对应的图片获取并在桌面中使用此图片显示（兼容了safari手机浏览器的定义）
```javascript
   <link rel="apple-touch-icon-precomposed" sizes=”57x57” href="images/icon.png" />
   <link rel="apple-touch-icon" sizes=”72x72” href="images/icon.png" />
```
在用户触发“发至桌面”的菜单操作后，即触发联网获取size最大的图片。在未获取到图片前，可以先显示默认图片；获取完成后再将获取到的图片更新到桌面。如果获取不到或者没有此特殊标签，仍然使用默认的桌面书签图标。
###5.2.应用模式
####5.2.1.定义
应用模式是为方便web应用及游戏开发者设置的综合开关，通过meta标签进行指示打开，当进入应用模式时，浏览器将自动调整以下参数：
| 参数  |  状态 |
| ---------- | -----------|
| 全屏   |  生效，可通过meta或js api调用退出全屏  |
| 长按菜单   |  失效，可通过js api调用重新生效 |
|   浏览器默认手势 |  失效，可通过js api调用重新生效 |
|  排版模式  | 标准模式，可通过meta或js api调用设置其他排版模式  |
|   强制图片显示 | 失效  |
|  夜间模式  | 失效，可通过meta或js api调用启用夜间模式  |
####5.2.2.标签
通过meta标签可调用应用模式
```javascript
<meta name="browsermode" content="application"/>
```
####5.2.3.示例
* 进入应用模式
```javascript
<html>
 <meta name="browsermode" content="application"/>
 <body>
  本页面设置了应用模式，默认将全屏，禁止长按菜单，禁止手势，标准排版，强制图片显示。
 </body>
</html>
```
* 进入应用模式，打开长按菜单，禁止夜间模式
```javascript
<html>
 <meta name="browsermode" content="application"/>
 <meta name="nightmode" content="disable"/>
 <script language="text/javascript">
   navigator.control.longpressMenu(true);
 </script>
 <body>
   本页面设置了应用模式，但禁止了夜间模式及允许长按菜单
 </body>
</html>
```
####5.2.4.说明
<br>
<br>
##6.浏览器版本识别
UC浏览器的识别方式和其他浏览器识别方式一致，可以通过navigator.appVersion变量识别是否UC浏览器及对应的版本。请参考以下例程
```javascript
<html>
<body>
		<script type="text/javascript">
			document.write(navigator.appVersion);
			if (navigator.appVersion.indexOf("UC")!= -1)
			{
			  document.write("this is uc browser");
			}
		</script>
</body>
</html>
```
当使用UC浏览器（默认UA）时，页面显示：
```javascript
5.0 (Linux; U; Android 2.3.3; zh-cn; samsung Build/GINGERBREAD) UC AppleWebKit/534.31 (KHTML,like Gecko) Mobile Safari/534.31 UC/8.6.0.174
This is uc browser
```
子frame中所有meta不生效<br>
事件传递到子frame中
