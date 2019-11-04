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

#### 不支持的 currentColor 变量
> 实际上，CSS 中很多属性值默认就是 currentColor 的表现

#### 不支持的 rgba 颜色和 hsla 颜色
> 十六进制颜色指的是长得像#000000 或#000 这样的颜色，我们在 CSS 中用得最频繁的就是这种格式的颜色。为什么呢？因为字符数目最少，书写更快，渲染性能更高
> rgb 颜色实际上和十六进制颜色是近亲，都归属于 rgb 颜色，只是进制有差异。rgb 格式从我入行起浏览器就支持了，除了支持数值颜色，如 rgb(255, 0, 51)，还支持百分比 rgb颜色，如 rgb(100%, 0%, 20%)
> rgb 数值格式只能是整数，不能是小数，否则，包括各 IE 以及 Chrome 在内的浏览器都会无视它
> hsl 颜色是 CSS3 才出现的颜色表现格式，IE9+浏览器才支持。和 rgb 分别表示 red、green、blue 一样，hsl 颜色的 3 个字母也有自己的含义。其中，h 表示 hue，是色调的意思，取值 0～360，大致按照数值红、橙、黄、绿、青、蓝、紫变化节奏；s 表示 saturation（饱和度），用 0～100%表示，值越大饱和度越高，颜色越亮，通常我们评价某设计“亮瞎我们的眼”，就是“这个设计颜色饱和度太高”的另一种说法；l 表示 lightness（亮度），也用 0～100%表示，值越高颜色越亮，100%就是白色，50%则是正常亮度，0%就是黑色

#### 支持却鸡肋的系统颜色
> （1）系统颜色本身有颜色，我们的模块是可以即时预览的，双击 html 模块文件就可以，没有任何其他依赖。
> （2）系统颜色名称都比较高冷，非常适合作为变量，替换时不会发生冲突。于是，当我们选取某一颜色后，只要把所有 CSS 中的系统颜色变量替换成选中的色值，任意组装的模块的即时配色的效果就实现了

### CSS 世界的 background 很单调
> CSS 世界中的 background 大部分有意思的内容都是在 CSS3 新世界中才出现的，如Multiple backgrounds（多背景）、background-size（背景尺寸）、background-origin（背景初始定位盒子）、background-clip（背景剪切盒子）等
> 当我们使用 background 属性的时候，实际上使用的是一系列 background 相关属性的集合，包括：
> + background-image: none
> + background-position: 0% 0%
> + background-repeat: repeat
> + background-attachment: scroll
> + background-color: transparent

> 如果是 IE9+浏览器，则还包括
> + background-size: auto auto
> + background-origin: padding-box
> + background-clip: border-box

#### 隐藏元素的 background-image 到底加不加载
> 根据我的测试，一个元素如果 display 计算值为 none，在 IE 浏览器下（IE8～IE11，更高版本不确定）依然会发送图片请求，Firefox 浏览器不会，至于 Chrome 和 Safari 浏览器则似乎更加智能一点：如果隐藏元素同时又设置了 background-image，则图片依然会去加载；如果是父元素的 display 计算值为 none，则背景图不会请求，此时浏览器或许放心地认为这个背景图暂时是不会使用的。

#### 与众不同的 background-position 百分比计算方式
> \<position>值支持 1～4 个值，可以是具体数值，也可以是百分比值，还可以是 left、top、right、center 和 bottom 等关键字。[图](https://ws1.sinaimg.cn/large/0060ZzrAgy1g8kpd7ba7yj30hy0bbadk.jpg)
> \<position>值的百分比值有着特殊的计算公式：

    positionX = (容器的宽度 - 图片的宽度) * percentX; 
    positionY = (容器的高度 - 图片的高度) * percentY;

#### background-repeat 与渲染性能
> background-repeat 支持 repeat、repeat-x、repeat-y 这几个值
> 然后有些人为了追求极致，就把 alpha.png 做成了 1 像素×1 像素大小，确实图片尺寸小了那么一点点，但是遮罩背景出现的时候会有明显的卡顿，体验非常不好。究其原因，就是平铺图片尺寸太小，平铺次数太多，渲染太吃力，其实我们大可把背景图保存成 100 像素×100 像素大小，这样一来，图片尺寸并没有大多少，但是渲染性能却有明显提升。

#### 外强中干的 background-attachment:fixed
> 在 CSS 世界中，background-attachment 支持 scroll 和 fixed两个属性值，其中 scroll 是默认值，就是平常使用背景图的效果表现；fixed 表示背景相对于当前文档视区定位，也就是页面再怎么滚动背景图片位置依旧纹丝不动，稳若泰山
> 见书288~289

#### background-color 背景色永远是最低的

#### 利用多背景的属性 hack 小技巧