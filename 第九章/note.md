## 元素的装饰与美化
### CSS 世界的 color 很单调
#### 少得可怜的颜色关键字
> CSS1 的时候只支持 16 个基本颜色关键字
> CSS2 的时候，显然应该要新增一些颜色关键字。或许很多人满怀希望，总以为会有什么惊喜，结果果然很惊喜：居然只加入了一个颜色—橙色 orange
> 到了 CSS3 的时候，以为又会是“雷声大，雨点小”，结果这次却出人意料，一下子增加了 100 多个颜色关键字，甚至连 mediumturquoise 这样的复杂得看不懂也记不住的关键字都出现了。这些扩展的 CSS 颜色关键字是有专门的名称的，称为“X11 颜色名”，这里的“X11”不是 11 区的意思，而是指用来显示位图的 X Window System，常见于类 UNIX 计算机系统上
> 到了 CSS4 的时候，以为又会有什么惊人之举，结果又仅增加了一个颜色关键字—rebeccapurple
> 在本书中，CSS 世界指 CSS 2.1，CSS 新世界指 CSS3，CSS 新新世界指 CSS4，因此，CSS世界中支持的颜色关键字其实少得可怜，总共就[17 个](https://ws1.sinaimg.cn/large/0060ZzrAgy1g8kotqgoajj30u013o4ps.jpg)

#### 不支持的 transparent 关键字
> transparent 关键字是一个很有意思的关键字，background-color:transparent包括 IE6 浏览器都支持，border-color:transparent 从 IE7 浏览器开始支持，但是 color: transparent 却从 IE9 浏览器才开始支持。