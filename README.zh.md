**[这里](http://crookedss.bplaced.net/)你可以在这个回购中找到代码的演示. **

# 歪曲的样式表

网站跟踪/分析只使用CSS和不使用JavaScript的概念验证. 

## 我们可以用这个方法做什么?

我们可以收集关于用户的一些基本信息,比如屏幕分辨率(当浏览器被最大化时)以及使用哪个浏览器(或引擎). 此外,我们可以检测用户是否打开链接或者用鼠标悬停在元素上. 

这可以用来跟踪用户访问哪些(外部)链接并使用悬停方法. 

## 应该甚至可以跟踪用户如何移动鼠标(在页面背景中使用不可见的字段表). 

### 但是,使用我的方法只能跟踪用户第一次访问链接或第一次在某个字段上盘旋. 

也许有可能修改该方法,以便可以跟踪每次点击. 

此外可以检测用户是否安装了特定的字体. 

```CSS
#link2:active::after {
    content: url("track.php?action=link2_clicked");
}
```

基于这些信息,应该有可能检测用户使用哪个操作系统(因为不同的操作系统运送不同的字体,例如窗口上的"calibri"). 

### 它是如何工作的?

大概的概念`在CSS中,您可以使用url("foo.bar")从外部资源添加图像;`属性. `有趣的是,这个资源只在需要的时候被加载(例如,当链接被点击时). `所以我们可以在css中创建一个选择器,当用户点击一个链接时调用一个特定的url: 

```CSS
@supports (-webkit-appearance:none) {
    #chrome_detect::after {
        content: url("track.php?action=browser_chrome");
    }
}
```

### 在服务器端,一个php脚本会在调用url时保存时间戳. 

浏览器检测

```CSS
/** Font detection **/
@font-face {
    font-family: Font1;
    src: url("track.php?action=font1");
}

#font_detection1 {
    font-family: Calibri, Font1;
}
```

### 浏览器检测是基于

@supports媒体查询

```CSS
@keyframes pulsate {
    0% {background-image: url("track.php?duration=00")}
    20% {background-image: url("track.php?duration=20")}
    40% {background-image: url("track.php?duration=40")}
    60% {background-image: url("track.php?duration=60")}
    80% {background-image: url("track.php?duration=80")}
    100% {background-image: url("track.php?duration=100")}
}
```

,我们检查一些浏览器特定的CSS属性

```CSS
#duration:hover::after {
    -moz-animation: pulsate 5s infinite;
    -webkit-animation: pulsate 5s infinite;
    /*animation: pulsate 5s infinite;*/
    animation-name: pulsate;
    animation-duration: 10s;
    content: url("track.php?duration=-1");
}
```

\-webkit-外观

### : 

字体检测

```CSS
#checkbox:checked {
    content: url("track.php?action=checkbox");
} 
```

对于字体检测,定义了一个新的字体系列. 

```HTML
<input type="text" id="text_input" pattern="^test$" required>
```

```CSS
#text_input:valid {
    background: green;
    background-image: url("track.php?action=text_input");
}
```

## 那么就会尝试使用应该检查的字体的样式来判断文本是否存在. 

[当浏览器在用户系统上找不到字体时,定义的字体将被用作回退. ](http://crookedss.bplaced.net/)当发生这种情况时浏览器会尝试加载字体并在服务器上调用跟踪脚本. `测量悬停持续时间`对于悬停持续时间的方法(基于jeyroik的想法),我们定义了新的动画关键帧,每次请求一个新的关键帧就会请求一个url. `那么我们定义关键帧应该被用作div的动画. `我们可以选择动画的持续时间,这是我们可以测量的最大时间: 

可以通过在关键帧集中插入更多步骤来增加持续时间测量的重新分配. 

输入检测

## 检测用户是否检查复选框我们使用: css提供的选定选择器: 

为了检测字符串"测试",我们结合了html模式属性,可以用来构建一些基本的输入验证. 

结合: 有效的选择器,浏览器将请求我们的跟踪站点,当模式正则表达式与输入匹配时: 

演示这里你可以在这个存储库中找到这些文件的演示. 该的index.html是使用此方法正在跟踪的文件. 参观results.php为跟踪的结果. 如果没有,或者在一个属性后面出现一个PHP警告,意味着这个属性的值是false,或者用户还没有访问过这个页面或者链接(是的,这有点脏,但是你可以看到方法). 此外,分辨率检测不能很好地工作,因为我只检测最常用的屏幕宽度. 此外,检测用户的实际屏幕高度有点棘手,因为css使用浏览器窗口的高度和东西比窗口的任务栏使得浏览器区域小于显示器. 你能做些什么来防止通过这种方法进行跟踪?目前我所知道的唯一方法是,完全禁用网页的CSS(你可以用像umatrix这样的插件来做到这一点). 几乎每一个现代网页都看起来很丑,没有CSS,甚至有时甚至是无法使用的问题. 所以禁用CSS是不是一个真正的选择,除非当你非常担心你的隐私(例如,当你使用Tor浏览器,你应该也许禁用CSS). 一个更好的解决方案是,浏览器不会加载外部内容(在CSS中引用),当需要时,但加载站点时. 那么就不可能检测出单一的行为. 这种对内容加载的修改可以通过浏览器本身来实现,也可以通过插件来实现(类似于noscript或者umatrix)问题是这个解决方案可能会对性能产生影响,因为浏览器必须加载大量的inital网站加载内容(也许浏览器根本不会使用内容). 