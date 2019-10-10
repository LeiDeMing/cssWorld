## 盒尺寸四大家族

### 深入理解 content
#### content 与替换元素
+ 1．什么是替换元素
> 这种通过修改某个属性值呈现的内容就可以被替换的元素就称为“替换元素”
> + 内容的外观不受页面上的 CSS 的影响
> + 有自己的尺寸。在 Web 中，很多替换元素在没有明确尺寸设定的情况下，其默认的尺寸（不包括边框）是 300 像素×150 像素，video、iframe或者canvas等，也有少部分替换元素为 0 像素，如img图片，而表单元素的替换元素的尺寸则和浏览器有关，没有明显的规律。
> + 在很多 CSS 属性上有自己的一套表现规则 
> + 下面提个简单问题：下拉框是不是替换元素？答案：是的。我们可以对照一下“替换元素”的一些特点：首先，内容可替换例如我们设置 multiple属性，下拉直接变成了展开的直选多选模式；其次，基本样式外部 CSS 很难改变；最后，它有自己的尺寸，基线也是下边缘等

+ 2．替换元素的默认 display 值
> + [各个替换元素的默认display属性值](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7hbl4rt4wj30lw09e753.jpg)
> +     <input type="button" value="按钮"><button type="button">按钮</button>，在 Firefox 下，前者的 display 属性默认值是 inline，后者却是 inline-block，很自然会奇怪明明一个模子里出来的，怎么会有这个区别呢？<input>和<button>按钮的区别在什么地方？区别在于两种按钮默认的 white-space 值不一样，前者是 pre，后者是 normal，所表示出来的现象差异就是：当按钮文字足够多的时候，<input>按钮不会自动换行，<button>按钮则会
> + 替换元素的 display 是 inline、block和 inline-block 中的任意一个，其尺寸计算规则都是一样的。

+ 3．替换元素的尺寸计算规则
> 替换元素的尺寸从内而外分为 3 类：固有尺寸、HTML 尺寸和 CSS 尺寸
> + 固有尺寸指的是替换内容原本的尺寸
> + HTML 尺寸这个概念略微抽象，我们不妨将其想象成水煮蛋里面的那一层白色的膜，里面是“固有尺寸”这个蛋黄蛋白，外面是“CSS 尺寸”这个蛋壳。“HTML 尺寸”只能通过HTML 原生属性改变，这些 HTML 原生属性包括img的 width 和 height 属性、input的 size 属性、textarea的 cols 和 rows 属性等

+ 4．替换元素和非替换元素的距离有多远
> + 观点 1：替换元素和非替换元素之间只隔了一个 src 属性
> + 观点 2：替换元素和非替换元素之间只隔了一个 CSS content 属性

+ 5．content 与替换元素关系剖析
> + 我们使用 content 生成的文本是无法选中、无法复制的，好像设置了 userselect:none 声明一般，但是普通元素的文本却可以被轻松选中。同时，content 生成的文本无法被屏幕阅读设备读取，也无法被搜索引擎抓取，因此，千万不要自以为是地把重要的文本信息使用 content 属性生成，因为这对可访问性和 SEO 都很不友好，content 属性只能用来生成一些无关紧要的内容，如装饰性图形或者序号之类；同样，也不要担心原本重要的文字信息会被 content 替换，替换的仅仅是视觉层
> + 不能左右:empty 伪类。:empty 是一个 CSS 选择器，当元素里面无内容的时候进行匹配
> + content 动态生成值无法获取。content 是一个非常强大的 CSS 属性，其中一个强大之处就是计数器效果，可以自动累加数值

#### content 内容生成技术
> 在实际项目中，content 属性几乎都是用在::before/::after 这两个伪元素中，因此，“content 内容生成技术”有时候也称为“::before/::after 伪元素技术”。

### 温和的 padding 属性
#### padding 与元素的尺寸
+ 因为 CSS 中默认的 box-sizing 是 content-box，所以使用 padding 会增加元素的尺寸
+ 很多人可能有这样的误区，认为设置了 box-sizing:border-box，元素尺寸就不会变化。大多数情况下是这样的，但是，如果 padding 值足够大，那么 width 也无能为力了。例如：

    .box { 
        width: 80px; 
        padding: 20px 60px; 
        box-sizing: border-box; 
    } 
+ 则此时的 width 会无效，最终宽度为 120 像素（60px×2），而里面的内容则表现为“首选最小宽度”。上述尺寸表现是对具有块状特性的元素而言的，对于内联元素（不包括图片等替换元素）表现则有些许不同
+ 内联元素的 padding 在垂直方向同样会影响布局，影响视觉表现。
+ CSS 中还有很多其他场景或属性会出现这种不影响其他元素布局而是出现层叠效果的现象。比如，relative 元素的定位、盒阴影 box-shadow 以及 outline 等。这些层叠现象虽然看似类似，但实际上是有区别的。其分为两类：一类是纯视觉层叠，不影响外部尺寸；另一类则会影响外部尺寸。box-shadow 以及 outline 属于前者，而这里的 inline 元素的padding 层叠属于后者
+ 区分的方式很简单，如果父容器 overflow:auto，层叠区域超出父
容器的时候，没有滚动条出现，则是纯视觉的；如果出现滚动条，则会影响尺寸、影响布局
+ 首先，我们可以在不影响当前布局的情况下，优雅地增加链接或按钮的点击区域大小
    
    article a { 
        padding: .25em 0; 
    }

+ 利用内联元素的 padding 实现高度可控的分隔线

    a + a:before {
        content: ""; 
        font-size: 0; 
        padding: 10px 3px 1px; 
        margin-left: 6px; 
        border-left: 1px solid gray; 
    }

    \<a href="">登录\</a>\<a href="">注册\</a>

+ 实际上，对于非替换元素的内联元素，不仅 padding 不会加入行盒高度的计算，margin和 border 也都是如此，都是不计算高度，但实际上在内联盒周围发生了渲染

#### padding 的百分比值
+ padding 百分比值无论是水平方向还是垂直方向均是相对于宽度计算的
+ 网页开发的时候经常会有横贯整个屏幕的头图效果，我们通常的做法是定高，如 200 像素高，屏幕小的时候图片两侧内容隐藏。然而，这种实现有一个问题，就是类似笔记本这样的小屏幕，头图高度过高会导致下面主体内容可能一屏都实现不了，但是，如果我们使用 padding 进行等比例控制，则小屏幕下头图高度天然等比例缩小，没有任何 JavaScript，却依然适配良好！例如：

    .box { 
        padding: 10% 50%; 
        position: relative; 
    } 
    .box > img { 
        position: absolute; 
        width: 100%; height: 100%; 
        left: 0; top: 0; 
    }

+ 上面百分比值是应用在具有块状特性的元素上的，如果是内联元素，会有怎样的表现呢？
> + 同样相对于宽度计算；
> + 默认的高度和宽度细节有差异；
> + padding 会断行。
+ 我们先来看一下内联元素的 padding 断行，代码如下：

    .box { 
        border: 2px dashed #cd0000; 
    } 
    span { 
        padding: 50%; 
        background-color: gray; 
    } 
    \<span>内有文字若干\</span>
+ [图示](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7qtcrosa2j304d05iq2q.jpg)对于内联元素，其 padding 是会断行的，也就是 padding 区域是跟着内联盒模型中的行框盒子走的，上面的例子由于文字比较多，一行显示不了，于是“若干”两字换到了下一行，于是，原本的 padding 区域也跟着一起掉下来了，根据后来居上的层叠规则，“内有”两字自
然就正好被覆盖，于是看不见了；同时，规则的矩形区域因为换行，也变成了五条边；至于宽度和外部容器盒子不一样宽，那是自然的，如果没有任何文字内容，那自然宽度正好和容器一致；现在有“内有文字若干”这 6 个字，实际宽度是容器宽度和这 6 个字宽度的总和，换行后的宽度要想和容器宽度一样，那可真要靠极好的人品了
+ 事情还没完，我们再看一个现象，假如是空的内联元素，里面没有任何文字，仅有一个\<span>标签：\<span>内有文字若干\</span>,此时，我们会发现居然最终背景区域的宽度和高度是不相等的（[见图](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7qte2uymmj304a0500p3.jpg)），这不科学啊！padding:50%相对宽度计算，应该出来是个正方形啊，为何高度要高出一截呢？
+ 原因其实很简单：内联元素的垂直 padding 会让“幽灵空白节点”显现，也就是规范中的“strut”出现。
知道了原因，要解决此问题就简单了。由于内联元素默认的高度完全受 font-size 大小控制，因此，我们只要：

    span { 
        padding: 50%; 
        font-size: 0; 
        background-color: gray; 
    }

#### 标签元素内置的 padding
+ 1. ol/ul 列表内置 padding-left，但是单位是 px 不是 em
+ 2. 很多表单元素都内置 padding
> + 所有浏览器\<input>/\<textarea>输入框内置 padding
> + 所有浏览器\<button>按钮内置 padding
> + 部分浏览器\<select>下拉内置 padding，如 Firefox、IE8 及以上版本浏览器可以设置padding
> + 所有浏览器\<radio>/\<chexkbox>单复选框无内置 padding
> + \<button>按钮元素的 padding 最难控制

#### padding 与图形绘制
> padding 属性和 background-clip 属性配合，可以在有限的标签下实现一些 CSS 图形绘制效果
> 例 1：不使用伪元素，仅一层标签实现大队长的“三道杠”分类图标效果

    .icon-menu { 
        display: inline-block; 
        width: 140px; height: 10px; 
        padding: 35px 0; 
        border-top: 10px solid; 
        border-bottom: 10px solid; 
        background-color: currentColor; 
        background-clip: content-box; 
    }

> 例 2：不使用伪元素，仅一层标签实现双层圆点效果。此效果在移动端也比较常见

    .icon-dot { 
        display: inline-block; 
        width: 100px; height: 100px; 
        padding: 10px; 
        border: 10px solid; 
        border-radius: 50%; 
        background-color: currentColor; 
        background-clip: content-box; 
    }

### 激进的 margin 属性
#### margin 与元素尺寸以及相关布局
+ 1. 元素尺寸的相关概念
> + 元素尺寸:包括 padding和 border，也就是元素的 border box 的尺寸。在原生的 DOM API 中写作 offsetWidth和 offsetHeight，所以，有时候也成为“元素偏移尺寸”。
> + 元素内部尺寸:包括 padding 但不包括 border，也就是元素的 padding box 的尺寸。在原生的 DOM API 中写作 clientWidth 和 clientHeight，所以，有时候也称为“元素可视尺寸”。
> + 元素外部尺寸:不仅包括 padding 和 border，还包括 margin，也就是元素的 margin box 的尺寸。没有相对应的原生的 DOM API
> + “外部尺寸”有个很不一样的特性，就是尺寸的大小有可能是负数，没错，负尺寸。这和我们现实世界对尺寸的认知明显冲突了，因为现实世界没有什么物体的尺寸是负的。所以，我总是把“外部尺寸”理解为“元素占据的空间尺寸”，把概念从“尺寸”转换到 “空间”，这时候就容易理解多了。

+ 2. margin 与元素的内部尺寸
> + 只有元素是“充分利用可用空间”状态的时候，margin 才可以改变元素的可视尺寸,比方说，如下 CSS：
    
    .father { 
        width: 300px; 
        margin: 0 -20px; 
    }

> + 此时元素宽度还是 300 像素，尺寸无变化。因为只要宽度设定，margin 就无法改变元素尺寸，这和 padding 是不一样的
> + 但是，如果是下面这样的 HTML 和 CSS：
    
    <div class="father"> 
    <div class="son"></div> 
    </div> 
    .father { width: 300px; } 
    .son { margin: 0 -20px; }

> + 则.son 元素的宽度就是 340 像素了，尺寸通过负值设置变大了，因为此时的宽度表现是“充分利用可用空间”
> + CSS 世界默认的流方向是水平方向，因此，对于普通流体元素，margin 只能改变元素水平方向尺寸；但是，对于具有拉伸特性的绝对定位元素，则水平或垂直方向都可以，因为此时的尺寸表现符合“充分利用可用空间”
> + 我们还可以利用 margin 改变元素尺寸的特性来实现两端对齐布局效果,我们可以通过给父容器添加 margin 属性，增加容器的可用宽度来实现。

    ul { 
        margin-right: -20px; 
    } 
    ul > li { 
        float: left; 
        width: 100px; 
        margin-right: 20px; 
    }

+ 3. margin 与元素的外部尺寸
> 对于普通块状元素，在默认的水平流下，margin 只能改变左右方向的内部尺寸，垂直方向则无法改变。如果我们使用 writing-mode 改变流向为垂直流，则水平方向内部尺寸无法改变，垂直方向可以改变。这是由 margin:auto 的计算规则决定的
> + 如果容器可以滚动，在 IE 和 Firefox 浏览器下是会忽略 padding-bottom 值的，Chrome 等浏览器则不会。也就是说，在 IE 和 Firefox 浏览器下,底部没有 50 像素的 padding-bottom 间隙，

    <div style="height:100px; padding:50px 0;">     
        <img src="0.jpg" height="300"> 
    </div>

> + 此兼容性差异的本质区别在于：Chrome 浏览器是子元素超过 content box 尺寸触发滚动条显示，
而 IE 和 Firefox 浏览器是超过 padding box 尺寸触发滚动条显示

> + 但是，我们可以借助 margin 的外部尺寸特性来实现底部留白，代码如下：记住了，只能使用子元素的 margin-bottom 来实现滚动容器的底部留白。

    <div style="height:200px;"> 
        <img height="300" style="margin:50px 0;"> 
    </div>

> + 下面再举一个利用 margin 外部尺寸实现等高布局的经典案例。此布局多出现在分栏有背景色或者中间有分隔线的布局中，有可能左侧栏内容多，也有可能右侧栏内容多，但无论内容多少，两栏背景色都和容器一样高
> + 由于height:100%需要在父级设定具体高度值时才有效，因此我们需要使用其他技巧来实现。方法其实很多，例如使用 display: 
table-cell 布局，左右两栏作为单元格处理，或者使用 border 边框来模拟，再或者使用我们这里的 margin 负值实现，核心 CSS 代码如下：

    .column-box { 
        overflow: hidden; 
    } 
    .column-left, 
    .column-right { 
        margin-bottom: -9999px; 
        padding-bottom: 9999px; 
    }   

> + 垂直方向 margin 无法改变元素的内部尺寸，但却能改变外部尺寸，这里我们设置了margin-bottom:-9999px 意味着元素的外部尺寸在垂直方向上小了 9999px。默认情况下，垂直方向块级元素上下距离是 0，一旦 margin-bottom:-9999px 就意味着后面所有元素和上面元素的空间距离变成了-9999px，也就是后面元素都往上移动了 9999px。此时，通过神来一笔padding-bottom:9999px 增加元素高度，这正负一抵消，对布局层并无影响，但却带来了我们需要的东西—视觉层多了 9999px 高度的可使用的背景色。但是，9999px 太大了，所以需要配合父级 overflow:hidden 把多出来的色块背景隐藏掉，于是实现了视觉上的等高布局效果。
> + 本例中，padding-bottom:9999px 也可以用 border-bottom: 9999px solid transparent 代替，不过 IE7 以上浏览器才支持。

#### margin 的百分比值
> 和 padding 属性一样，margin 的百分比值无论是水平方向还是垂直方向都是相对于宽度计算的。

#### 正确看待 CSS 世界里的 margin 合并
+ 1. 什么是 margin 合并
> + 块级元素的上外边距（margin-top）与下外边距（margin-bottom）有时会合并为单个外边距，这样的现象称为“margin 合并”。从此定义上，我们可以捕获两点重要的信息
> + > （1）块级元素，但不包括浮动和绝对定位元素，尽管浮动和绝对定位可以让元素块状化。
> + > （2）只发生在垂直方向，需要注意的是，这种说法在不考虑 writing-mode 的情况下才是正确的，严格来讲，应该是只发生在和当前文档流方向的相垂直的方向上。由于默认文档流是水平流，因此发生 margin 合并的就是垂直方向。
+ 2. margin 合并的 3 种场景
> + （1）相邻兄弟元素 margin 合并。这是 margin 合并中最常见、最基本的，例如：则第一行和第二行之间的间距还是 1em，因为第一行的 margin-bottom 和第二行的margin-top 合并在一起了，并非上下相加。

    p { margin: 1em 0; } 
    <p>第一行</p>
    <p>第二行</p>

> + （2）父级和第一个/最后一个子元素。我们直接看例子，在默认状态下，下面 3 种设置是等效的：在实际开发的时候，给我们带来麻烦的多半就是这里的父子 margin 合并。

    <div class="father"> 
        <div class="son" style="margin-top:80px;"></div> 
    </div> 
    <div class="father" style="margin-top:80px;"> 
        <div class="son"></div> 
    </div> 
    <div class="father" style="margin-top:80px;"> 
        <div class="son" style="margin-top:80px;"></div> 
    </div>
> 对于 margin-top 合并，可以进行如下操作（满足一个条件即可）：
> + 父元素设置为[块状格式化上下](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)文元素；
> + 父元素设置 border-top 值；
> + 父元素设置 padding-top 值；
> + 父元素和第一个子元素之间添加内联元素进行分隔。

> 对于 margin-bottom 合并，可以进行如下操作（满足一个条件即可）：
> + 父元素设置为[块状格式化上下](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)元素；
> + 父元素设置 border-bottom 值；
> + 父元素设置 padding-bottom 值；
> + 父元素和最后一个子元素之间添加内联元素进行分隔；
> + 父元素设置 height、min-height 或 max-height。

> + （3）空块级元素的 margin 合并。例如，下面 CSS 和 HTML 代码：

    .father { overflow: hidden; } 
    .son { margin: 1em 0; } 
    <div class="father"> 
    <div class="son"></div> 
    </div>

> + > 比方说，我们一开始的“相邻兄弟元素 margin 合并”，其实，就算兄弟不相邻，也是可以发生合并的，前提是中间插手的也是个会合并的家伙。比方说：

    p { margin: 1em 0; } 
    <p>第一行</p>
    <div></div>
    <p>第二行</p>

> + > 此时第一行和第二行之间的距离还是 1em，中间看上去隔了一个\<div>元素，但对最终效果却没有任何影响。如果非要细究，则实际上这里发生了 3 次 margin 合并，\<div>和第一行\<p>的 margin-bottom 合并，然后和第二行\<p>的 margin-top 合并，这两次合并是相邻兄弟合并。由于自身是空\<div>，于是前两次合并的 margin-bottom 和 margin-top 再次合并，这次合并是空块级元素合并，于是最终间距还是 1em。
> + > 如果有人不希望空\<div>元素有 margin 合并，可以进行如下操作：
> + > + 设置垂直方向的 border；
> + > + 设置垂直方向的 padding；
> + > + 里面添加内联元素（直接 Space 键空格是没用的）；
> + > + 设置 height 或者 min-height。

+ 3. margin 合并的计算规则
> 我把 margin 合并的计算规则总结为“正正取大值”“正负值相加”“负负最负值”3 句话
> + （1）正正取大值。如果是相邻兄弟合并：此时.a 和.b 两个\<div>之间的间距是 50px，取大的那个值。
    
    .a { margin-bottom: 50px; } 
    .b { margin-top: 20px; } 
    <div class="a"></a> 
    <div class="b"></a>
> + > 如果是父子合并：此时.father 元素等同于设置了 margin-top:50px，取大的那个值。

    .father { margin-top: 20px; } 
    .son { margin-top: 50px; } 
    <div class="father">
        <div class="son"></div> 
    </div>

> + > 如果是自身合并：则此时.a 元素的外部尺寸是 50px，取大的那个值。

    .a { 
        margin-top: 20px; 
        margin-bottom: 50px; 
    } 
    <div class="a"></div>

> + （2）正负值相加。如果是相邻兄弟合并：此时.a 和.b 两个<div>之间的间距是 30px，是-20px+50px 的计算值。

    .a { margin-bottom: 50px; } 
    .b { margin-top: -20px; } 
    <div class="a"></a> 
    <div class="b"></a>

> + > 如果是父子合并：此时.father 元素等同于设置了 margin-top:30px，是-20px+50px 的计算值。

    .father { margin-top: -20px; } 
    .son { margin-top: 50px; } 
    <div class="father"> 
        <div class="son"></div> 
    </div>

> + > 如果是自身合并：则此时.a 元素的外部尺寸是 30px，是-20px+50px 的计算值。

    .a { 
        margin-top: -20px; 
        margin-bottom: 50px; 
    } 
    <div class="a"></div>

> + （3）负负最负值。如果是相邻兄弟合并：此时.a 和.b 两个<div>之间的间距是-50px，取绝对负值最大的值。

    .a { margin-bottom: -50px; } 
    .b { margin-top: -20px; } 
    <div class="a"></a> 
    <div class="b"></a>

> + > 如果是父子合并：此时.father 元素等同于设置了 margin-top:-50px，取绝对负值最大的值。

    .father { margin-top: -20px; } 
    .son { margin-top: -50px; }
    <div class="father"> 
        <div class="son"></div> 
    </div>

> + > 如果是自身合并：则此时.a 元素的外部尺寸是-50px，取绝对负值最大的值。

    .a { 
        margin-top: -20px; 
        margin-bottom: -50px; 
    } 
    <div class="a"></div>

+ 4. margin 合并的意义
> “margin-top 合并 bug。”这种说法是大有问题的，“margin-top 合并”这种特性是故意这么设计的，在实际内容呈现的时候是有着重要意义的，根本就不是 bug！不要遇到出乎自己意料或者自己无法理解的现象就称其为 bug。
> + 对于兄弟元素的 margin 合并其作用和 em 类似，都是让图文信息的排版更加舒服自然。假如说没有 margin 合并这种说法，那么连续段落或列表之类首尾项间距会和其他兄弟标签成 1:2 关系；文章标题距离顶部会很近，而和下面的文章详情内容距离又会很开，就会造成内容上下间距不一致的情况
> + 父子 margin 合并的意义在于：现在有如下一段 HTML：\<div style="margin-top:20px;">\</div>,现在要在上面这段 HTML 的外面再嵌套一层\<div>元素，假如说现在没有父子 margin合并，那这层新嵌套的\<div>岂不阻断了原本的兄弟 margin 合并？很有可能间距就会变大，妥妥地影响了原来的布局，这显然就违背了\<div>的设计作用了。所以才有了父子 margin 合并，外面再嵌套一层\<div>元素就跟没嵌套一样，表现为 margin-top:20px 就好像是设置在最外面的\<div>元素上一样。
> + 自身 margin 合并的意义在于可以避免不小心遗落或者生成的空标签影响排版和布局。例如：

    <p>第一行</p>
    <p></p>
    <p></p>
    <p></p>
    <p></p>
    <p>第二行</p>
> + > 其和下面这段 HTML 最终视觉效果是一模一样的：

    <p>第一行</p>
    <p>第二行</p>

#### 深入理解 CSS 中的 margin:auto
> 首先，我们需要知道下面这些事实。
> margin 属性的auto 计算就是为块级元素左中右对齐而设计的，和内联元素使用 text-align 控制左中右对齐正好遥相呼应！
> 另外，对于替换元素，如果我们设置 display:block，则 margin:auto 的计算规则同
样适合
> + 有时候元素就算没有设置 width 或 height，也会自动填充。例如：\<div>\</div>,此\<div>宽度就会自动填满容器。
> + 有时候元素就算没有设置 width 或 height，也会自动填充对应的方位。例如：此时\<div>宽度就会自动填满包含块容器。
    div { 
        position: absolute; 
        left: 0; right: 0; 
    }

> margin:auto 的填充规则如下。
> + 如果一侧定值，一侧 auto，则 auto 为剩余空间大小。
> + 如果两侧均是 auto，则平分剩余空间。
> 由于 CSS 世界中 margin 的初始值大小是 0，因此，上面的例子如果margin-right 缺失，实现的效果正好是块级元素的右对齐效果。也就是：

    .son { 
        width: 200px; 
        margin-left: auto; 
    }
> 我们垂直方向 margin 无法实现居中了吗？当然是可以的，而且场景还不止一种。
> + 第一种方法是使用 writing-mode 改变文档流的方向：此时.son 就是垂直居中对齐的，但是这也带来另外的问题，就是水平方向无法 auto 居中了。

    .father { 
        height: 200px; 
        writing-mode: vertical-lr; 
    } 
    .son { 
        height: 100px; 
        margin: auto; 
    }

> + 所以，有人会关心有没有让水平垂直同时居中的方法。有，就是这里要提的第二种方法，绝对定位元素的 margin:auto 居中

    .father { 
        width: 300px; height:150px; 
        position: relative; 
    } 
    .son { 
        position: absolute; 
        top: 0; right: 0; bottom: 0; left: 0; 
    }

> + > 此时.son 这个元素的尺寸表现为“格式化宽度和格式化高度”，和\<div>的“正常流宽度”一样，同属于外部尺寸，也就是尺寸自动填充父级元素的可用尺寸，此时我们给.son 设置尺寸,此时宽高被限制，原本应该填充的空间就被空余了出来，这多余的空间就是 margin:auto计算的空间，

    .son { 
        position: absolute; 
        top: 0; right: 0; bottom: 0; left: 0; 
        width: 200px; height: 100px; 
    }

> + > 因此，如果这时候我们再设置一个 margin:auto：那么我们这个.son 元素就水平方向和垂直方向同时居中了。因为 auto 正好把上下左右剩余空间全部等分了，自然就居中！

    .son { 
        position: absolute; 
        top: 0; right: 0; bottom: 0; left: 0; 
        width: 200px; height: 100px; 
        margin: auto; 
    }
> + > 绝对定位下的 margin:auto 居中是我用得最频繁的块级元素垂直居中对齐方式，比 top:50%然后 margin 负一半元素高度的方法要好使得多。

#### margin 无效情形解析
> + （1）display 计算值 inline 的非替换元素的垂直 margin 是无效的，虽然规范提到有渲染，但浏览器表现却未寻得一点踪迹，这和 padding 是有明显区别的。对于内联替换元素，垂直 margin 有效，并且没有 margin 合并的问题，所以图片永远不会发生 margin 合并。
> + （2）表格中的\<tr>和\<td>元素或者设置 display 计算值是 table-cell 或 table-row 的元素的 margin 都是无效的。但是，如果计算值是 table-caption、table 或者 inline-table则没有此问题，可以通过 margin 控制外间距，甚至::first-letter 伪元素也可以解析 margin。
> + （3）margin 合并的时候，更改 margin 值可能是没有效果的。以父子 margin 重叠为例，假设父元素设置有 margin-top:50px，则此时子元素设置 margin-top:30px 就没有任何效果表现，除非大小比 50px 大，或者是负值。
> + （4）绝对定位元素非定位方位的 margin 值“无效”。什么意思呢？很多时候，我们对元素进行绝对定位的时候，只会设置 1～2 个相邻方位。img { top: 10%; left: 30%;}
> + （5）定高容器的子元素的 margin-bottom 或者宽度定死的子元素的 margin-right 的定位“失效”,若想使用 margin 属性改变自身的位置，必须是和当前元素定位方向一样的 margin 属性才可以，否则，margin 只能影响后面的元素或者父元素
> + > 在默认流下，其定位方向是左侧以及上方，此时只有 margin-left和 margin-top 可以影响元素的定位。但是，如果通过一些属性改变了定位方向，如float:right 或者绝对定位元素的 right 右侧定位，则反过来 margin-right 可以影响元素的定位，margin-left 只能影响兄弟元素
> + （6）鞭长莫及导致的 margin 无效。
> + （7）内联特性导致的 margin 无效
> + > 这里的例子也很有代表性。一个容器里面有一个图片，然后这张图片设置 margin-top负值，让图片上偏移。但是，随着我们的负值越来越负，结果达到某一个具体负值的时候，图片不再往上偏移了

### 功勋卓越的 border 属性
> border 属性的默认宽度大小是 medium，也就是 3px
#### 为什么 border-width 不支持百分比值
> + 所谓“边框”，是不会因为设备大就按比例变大的,因此，如果支持百分比值，是不是就意味着设备大了边框也跟着变大？这显然不合“边框”的语义嘛
> + 我们要想到 CSS 世界创造的背景主要是为图文展示服务的，有一张图片，大片区域都是白色的，在白底背景上和文字混在一起，就会有一片奇怪的空白区域，会让人产生没对齐的假象，此时，我们给这张图片套个 1px 灰色边框，区域就明显了，设计的初衷就是为了这么点儿事，没有需要使用百分比值的场景
> + > 于是，综合这两点，造成了 border-width 不支持百分比值。
> + 其实还有很多 CSS 属性，如 outline、box-shadow、text-shadow 等，都是不支持百分比值的，原因也与此类似。
+ border-width 还支持若干关键字，包括 thin、medium（默认值）和 thick，对应的尺寸大小具体如下。
> + thin：薄薄的，等同于 1px。
> + medium（默认值）：薄厚均匀，等同于 3px。
> + thick：厚厚的，等同于 4px。

#### 了解各种 border-style 类型
+ 1．border-style:none
> + 注意，border-style 的默认值是 none

+ 2．border-style:solid

+ 3．border-style:dashed
> + 这虚线颜色区的宽高比以及颜色区和透明区的宽度比例在不同浏览器下是有差异的。例如，在 Chrome 和 Firefox 浏览器下，颜色区的宽高比是 3:1，颜色区和透明区的宽度比例是 1:1，如图 4-59 所示；而 IE 浏览器则是另外的数据，颜色区的宽高比是 2:1，颜色区和透明区的宽度比例也是 2:1

+ 4．border-style:dotted

+ 5．border-style:double
> + 视觉表现为线框—透明线框，根据 border-width 大小不同，其表现规则。[如表 4-2](http://ww1.sinaimg.cn/large/0060ZzrAgy1g7sxezlbqpj30m8073jru.jpg)所示
> + “为什么 border-width的默认值是 media（3px）？”答案就在这里，虽然说实际开发的时候 1px 大小的 solid 类型边框是最常用的，但是却无法让 double 类型边框有合乎语义的表现，因此使用了能表现double 类型的最小边框宽度 3px 作为边框宽度默认值。

+ 6．其他 border-style 类型
> + inset（内凹）、outset（外凸）、groove（沟槽）、ridge（山脊）风格老土过时，且兼容性惨不忍睹（见图 4-67）。因此，它
们没有任何实用价值。但是，它们也不是一无是处，这几个边框类型的出现无形中规范了实线边框的转角连接规则。规范中并没有任何关于边框连接规则的描述，按照一般的套路，浏览器会表现不一，类似虚线之类边框确实如此，但是，对于 solid 类型边框，各个浏览器却像是约定好了，连接表现一致，背后起作用的恰恰是这几个看上去没有任何作用的 border-style 类型。这为 border 图形生成技术的广泛应用打下了坚实的基础。

#### border-color 和 color
> border-color 有一个很重要也很实用的特性，就是“border-color 默认颜色就是color 色值”,具有类似特性的 CSS 属性还有 outline、box-shadow 和 text-shadow 等

#### border 与透明边框技巧
> 虽然 color:transparent 在 IE9 以上版本的浏览器才支持，但是 border-color: transparent 在 IE7 浏览器就开始支持了，于是，我们解决一些棘手问题的思路就更加开阔了。
+ 1．右下方 background 定位的技巧。假设现在有一个宽度不固定的元素，我们需要在距离右边缘 50 像素的位置设置一个背景图片，此时 background 属性就遭遇尴尬了：由于宽度不固定，所以无法通过设定具体数值来实现我们想要的效果，因为 background 是相对左上角定位的，我们的需求是右侧定位。要实现上面的需求，方法挺多。其中一种方法就是使用透明边框，如下 CSS 代码：此时，对 50px 的间距我们使用 transparent 边框表示，这样就可以使用百分比 backgroundposition 定位到我们想要的位置了。因为，默认 background 背景图片是相对于 padding box定位的，也就是说，background-position:100%的位置计算默认是不会把 border-width计算在内的。

    .box { 
        border-right: 50px solid transparent; 
        background-position: 100% 50%; 
    }
