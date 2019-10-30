## 强大的文本处理能力
### line-height 的另外一个朋友 font-size
#### font-size 和 vertical-align 的隐秘故事
> line-height 的部分类别属性值是相对于 font-size 计算的，vertical-align 百分比值属性值又是相对于 line-height 计算的，于是，看上去八辈子都搭不上边的 verticalalign 和 font-size 属性背后其实也有有着关联的。
> 例如，下面的 CSS 代码组合：此时，p > img 选择器对应元素的 vertical-align 计算值应该是:16px * 1.5 * -25% = -6px

    p { 
        font-size: 16px; 
        line-height: 1.5; 
    } 
    p > img { 
        ertical-align: -25%; 
    }

> 但是两者又有所不同，很显然，-25%是一个相对计算属性值，如果此时元素的 font-size发生变化，则图片会自动进行垂直位置调整。我们可以看一个无论 font-size 如何变化、后面图标都垂直居中对齐的例子，(手动输入)(http://demo.cssworld.cn/8/1-1.php) 或者扫下面的二维码。可以看到，无论文字字号是大还是小，后面的图标都非常良好地垂直居中对齐，[如图](https://ws1.sinaimg.cn/large/0060ZzrAgy1g8eazi2gosj309y048wel.jpg)

#### 理解 font-size 与 ex、em 和 rem 的关系
> ex 是字符 x 高度，显然和 font-size 关系密切，font-size 值越大，自然 ex 对应的大小也就大，对此本书前面已经有介绍，这里不赘述。
> 之所以叫作 em 完全取决于 M 的字形，毕竟英文26 个字母方方正正的不算多。如果按照这种说法，那方方正正的汉字岂不是每一个字都正好一个 em？没错，确实是这样，尤其作为印刷体的宋体效果最为明显，这种表现在 CSS 中也有非常明显的体现。
> 也就是说，em 就是'中'等汉字的高度！于是，我们对 em 的理解就更加简单了，直接看一个很容易理解错误的题目，在 Chrome 浏览器下，\<h1>元素有如下的默认 CSS：

    h1 { 
        font-size: 2em; 
        -webkit-margin-before: 0.67em;
        -webkit-margin-after: 0.67em;
    }

> 我们可以这样想，假设\<h1>里面有汉字，此时汉字的高度是多少？这个高度就是此时 1em大小。\<h1>元素此时 font-size 是 2em，算一下就是 32px，因此，此时里面汉字的高度应该是 32px，也就是说，此时\<h1>元素的 1em 应该是 32px，于是 margin-before 的像素计算值为 32px×0.67 = 21.44px，和浏览器自己的计算值一样
> 总结如下：在 CSS 中，1em 的计算值等同于当前元素所在的 font-size 计算值，可以将其想象成当前元素中（如果有）汉字的高度。
> 所有相对单位的好处都是一样的，样式表现更具有弹性。例如，理论上，有一个布局，希望小屏时整体缩小，大屏时再弹性扩大，此时就可以让所有元素宽高尺寸等都使用 em，于是，最后只要改变布局祖先元素的 font-size 大小就可以实现整体的弹性变化。
> 这种策略很棒，也确实可行，但是有一个比较麻烦的事情，它和上面<h1>元素计算一样的麻烦，em 是根据当前 font-size 大小计算的，一旦布局中出现标题这种跟基础 font-size大小不一样的场景的时候，标题里面所有元素 em 都要重新计算一遍，甚为麻烦，最终的成品维护成本就比较高了。
> 回到 em 单位。em 实际上更适用于图文内容展示的场景，对此进行弹性布局。例如，\<h1>～\<h6>以及\<p>等与文本内容展示的元素的 margin 都是用 em 作为单位，这样，当用户把浏览器默认字号从“中”设置成“大”或改成“小”的时候，上下间距也能根据字号大小自动调整，使阅读更舒服。
> 再举个适用于 em 的场景，如果我们使用 SVG 矢量图标，建议设置 SVG 宽高如下：这样，无论图标是个大号文字混在一起还是和小号文字混在一起，都能和当前文字大小保持一致，既省时又省力。

    svg { 
        width: 1em; height: 1em; 
    }

#### 理解 font-size 的关键字属性值
> font-size 支持长度值，如 1em，也支持百分比值，如 100%。这两点想必众所周知，但font-size 还支持关键字属性值这一点怕是就有不少人不清楚了。
> font-size 的关键字属性值分以下两类。
> + （1）相对尺寸关键字。指相对于当前元素 font-size 计算，包括：
> + + larger：大一点，是\<big>元素的默认 font-size 属性值
> + + smaller：小一点，是\<small>元素的默认 font-size 属性值。
> + （2）绝对尺寸关键字。与当前元素 font-size 无关，仅受浏览器设置的字号影响。注意这里的措辞，是“浏览器设置”，而非“根元素”，两者是有区别的。
> + + xx-large：好大好大，和<h1>元素计算值一样。
> + + x-large：好大，和<h2>元素计算值一样。
> + + large：大，和<h3>元素计算值近似（“近似”指计算值偏差在 1 像素以内，下同）。
> + + medium：不上不下，是 font-size 的初始值，和\<h4>元素计算值一样。为了解决大家可能有的疑问，这里有必要多说几句。如果 font-size 默认值是 medium，而medium 计算值仅与浏览器设置有关，那为何我们平时元素 font-size 总是受环境影响变来变去呢？这完全是因为 font-size 属性的继承性，实际开发的时候，我们常常会对\<html>或\<body>重置 font-size 大小，于是，受继承性影响，大多数后代元素的 font-size 计算值也变成了 14px，medium这个初始值受继承性影响而被覆盖了。
> + + small：小，和<h5>元素计算值近似。
> + + x-small：好小，和<h6>元素计算值近似。
> + + xx-small：好小好小，无对应的 HTML 元素。
> 如何权衡“易于实现维护”“视觉还原”“可访问性”这三者，我这里有两个珍藏的建议。
> + （1）即使是定宽的传统桌面端网页，也需要做响应式处理，尤其是针对 1200 像素宽度设计的网页，但只需要响应到 800 像素即可，可以保证至少有 1.5 倍的缩放空间，如果做到这一步，那么是否需要响应浏览器的字号设置这一点就可以忽略。
> + （2）如果困各种原因无法做响应式处理，也没有必要全局都使用相对单位，毕竟成本等现实问题摆在那里，其实只需要在图文内容为主的重要局部区域使用可缩放的 font-size 处理即可。例如，小说网站的阅读页、微信公众号文章展示区、私信对话内容区、搜索引擎的落地页、评论区等，都强烈建议摒弃 px 单位，而采用下面的实践策略

#### font-size:0 与文本的隐藏
> 桌面 Chrome 浏览器下有个 12px 的字号限制，就是文字的 font-size 计算值不能小于12px，我猜是因为中文，如宋体，要是小于 12px，就会挤成成一团，略丑，Chrome 看不下去，就直接禁用了。
> 还是回到字号限制的问题。实际上，并不是所有小于12px的font-size都会被当作12px处理，有一个值例外，那就是 0，也就是说，如果 font-size:0 的字号表现就是 0，那么文字会直接被隐藏掉，并且只能是 font-size:0，哪怕设置成 font-size:0.0000001px，都还是会被当作 12px 处理的。

### 字体属性家族的大家长 font-family
> font-family 支持两类属性值，一类是“字体名”，一类是“字体族”

#### 了解衬线字体和无衬线字体
> 但是需要注意的是，serif 和 sans-serif 一定要写在最后，因为在大多数浏览器下，写在 serif 和 sans-serif 后面的所有字体都会被忽略。例如： Chrome 浏览器下，后面的 Microsoft Yahei 字体是不会被渲染的。据我的推测，有可能浏览器认为当前“字体族”已经满足了文本渲染的需要，没必要再往后解析了。

    body { 
    font-family: sans-serif, "Microsoft Yahei"; 
    }

#### 等宽字体的实践价值
> 所谓等宽字体，一般是针对英文字体而言的。据我所知，东亚字体应该都是等宽的，就是每个字符在同等 font-size 下占据的宽度是一样的。但是英文字体就不一定了，我随便写一个单词，就 iMac 吧，大家很明显地发现这个字符 i 要比 M 占据的宽度小。如果这样看着还不够清楚，那我换一种呈现方式，上下两行，上一行 6 个 i，下一行 6 个 M，如下：

    iiiiii 
    MMMMMM  

> 但是，如果是等宽字体（可以让英文字符同等宽度显示的字体就称为“等宽字体”），如Consolas、Monaco、monospace，则宽度表现就不一样了,[如图](https://ws1.sinaimg.cn/large/0060ZzrAgy1g8eyvgzur5j307z03yq2z.jpg)
> 等宽字体在 Web 中有什么用呢？
> + 1．等宽字体与代码呈现-首先等宽字体利于代码呈现
> + 2．[等宽字体与图形呈现案例一则](http://demo.cssworld.cn/8/2-1.php)
> + 3．ch 单位与等宽字体布局.ch 和 em、rem、ex 一样，是 CSS 中和字符相关的相对单位。和 ch 相关的字符是 0，没错，就是阿拉伯数字 0。1ch 表示一个 0 字符的宽度，所以 6 个 0 所占据的宽度就是 6ch。

#### 中文字体和英文名称
> 虽然一些常见中文字体，如宋体、微软雅黑等，直接使用中文名称作为 CSS font-family 的属性值也能
生效，但我们一般都不使用中文名称，而是使用英文名称，主要是为了规避乱码的风险

#### 一些补充说明
> 所有英文名称大小写都经过一定的考量，并不是随便设定的，虽然说 CSS font-family对名称的大小写不怎么敏感，但是根据我的经验，最好至少首字母要大写，否则在使用 CSS unicode-range 的时候可能会遇到一些麻烦

### 字体家族其他成员
#### 貌似粗犷、实则精细无比的 font-weight
> font-weight 表示“字重”，通俗点讲，就是表示文字的粗细程度。
> 对于 font-weight 这个属性，我们平时使用都相当粗犷，无非就是下面这两个 CSS 声明：

    font-weight: normal; 
    font-weight: bold;

> 首先让我们大致了解一下 font-weight 都支持哪些属性值。具体如下：

    /* 平常用的最多的 */ 
    font-weight: normal; 
    font-weight: bold;

    /* 相对于父级元素 */ 
    font-weight: lighter; 
    font-weight: bolder;

    /* 字重的精细控制 */ 
    font-weight: 100; 
    font-weight: 200; 
    font-weight: 300; 
    font-weight: 400; 
    font-weight: 500; 
    font-weight: 600; 
    font-weight: 700; 
    font-weight: 800; 
    font-weight: 900;

> 所有这些属性值，都可以从 font-weight:100 至 font-weight:900 说起。
> + 100：文字很细，细如发丝。
> + 200：文字很轻，轻如鸿毛。
> + 300：文字较轻，轻如飞燕。
> + 400：文字正常，等同 normal。 • 500：文字不粗不细，不轻不重。
> + 600：文字略粗，粗如小腿。
> + 700：文字加粗，等同 bold。 • 800：文字超粗，粗如大腿。
> + 900：文字很重，重如泰山。

> 下面关键问题来了。很多人会发现，font-weight 无论是设置 300、400、500 还是 600，文字的粗细都没有任何变化，只有到 700 的时候才会加粗一下，感觉浏览器好像不支持这些数值，那么搞这么多档位不就是多余的吗？
> 实际上，所有这些数值关键字浏览器都是支持的，之所以没有看到任何粗细的变化，是因为我们的系统里面缺乏对应粗细的字体。尤其我们做桌面端项目时，大部分用户都是使用Windows 系统，而 Windows 系统中的中文字体粗细就一个型号，如“宋体”，或者说“微软雅黑”，因此，最终的效果就是 CSS 层面的“加粗”和“正常尺寸”两种表现
> 也就是说，font-weight 要想真正发挥潜力，问题不在于 CSS 的支持，而在于是否存在对应的字体文件

#### 具有近似姐妹花属性值的 font-style
> font-style 表示文字造型是斜还是正，与 font-weight 相比，其属性值就要少很多，如下：

    font-style: normal; 
    font-style: italic; 
    font-style: oblique;

> 差别在于：italic 是使用当前字体的斜体字体，而 oblique 只是单纯地让文字倾斜。如果当前字体没有对应的斜体字体，则退而求其次，解析为 oblique，也就是单纯形状倾斜

#### 不适合国情的 font-variant
> font-variant 是一个从 IE6 时代就过来的 CSS 属性，对于我们大中华用户，其支持的属性值和作用让我们这些汉字用户觉得有些头疼，实现小体型大写字母，两个属性值要么 normal，要么 small-caps，font-variant:small-caps 就是可以让英文字符表现为小体型大写字母。代码示意如下：

    http://www.<span style="font-variant:small-caps">css-world.com</span>/

> 结果如下：

### font 属性
#### 作为缩写的 font 属性
> 完整语法为：[ [ font-style || font-variant || font-weight ]? font-size [ / line-height ]? 
font-family ],||表示或，?和正则表达式中的?的含义一致，表示 0 个或 1 个
> 仔细观察上面的语法，会发现font-size 和font-family 后面没有问号，也就是说是必需的，是不可以省略的。这和background属性不一样，background 属性虽然也支持缩写，但是并没有需要两个属性值同时存在的限制。
> 因此，如果你的 font 属性缩写无效，检查一下 font-size 和 font-family 这两个属性是否同时存在
> 每次 font 缩写后面都挂一个长长的字体列表，令人很是不悦，有什么小技巧可以避免吗？
> + 方法一：我们可以随便找一个系统根本不存在的字体名占位，如字母 a，或者特殊一点，用笑脸表情☺，然后再设置 font-family:inherit 来重置这个占位字体。例如，我们想把字号和行高合并缩写，就可以这样：

    .font { 
        font: 30px/30px '☺';
        font-family: inherit; 
    }

> + 方法二：利用@font face 规则将我们的字体列表重定义为一个字体，这是兼容性很好、效益很高的一种解决方法，会在 8.5 节详细介绍。

#### 使用关键字值的 font 属性
> font 属性除了缩写用法，还支持关键字属性值，这个怕是很多人都不知道的。其语法如下：font:caption | icon | menu | message-box | small-caption | status-bar
> 如果将 font 属性设置为上面的一个值，就等同于设置 font 为操作系统该部件对应的font，也就是说直接使用系统字体。
> 根据 W3C 官方维基的解释，以及我自己在 Windows 系统下的测试，各个关键字的含义如下。
> + caption：活动窗口标题栏使用的字体。
> + icon：包含图标内容所使用的字体，如所有文件夹名称、文件名称、磁盘名称，甚至浏览器窗口标题所使用的字体。
> + menu：菜单使用的字体，如文件夹菜单。
> + message-box：消息盒里面使用的字体。
> + small-caption：调色板标题所使用的字体。
> + status-bar：窗体状态栏使用的字体。
> 需要注意的是，使用关键字作为属性值的时候必须是独立的，不能添加 font-family 或 者 font-size 之类的，这和 font 属性缩写不是一个路子。
> 通过设置修改 Windows 系统相关控件的默认字体我发现，这次是 Chrome浏览器拖了后腿。caption、icon、message-box 这 3 个关键字在 Windows 系统下的Chrome 浏览器中似乎是无效的，
> 除了 caption、icon、menu、message-box、small-caption 和 status-bar，还有很多其他非标准的关键字，如 button、checkbox、checkbox-group、combo-box、desktop、dialog、document、field、hyperlink、list-menu、menu-item、menubar、outline-tree 、 password 、 pop-up-menu 、 pull-down-menu 、 push-button 、radio-button、radio-group、range、signature、tab、tooltip、window 和workspace。不过，这些关键字浏览器大多不支持，尽管 Firefox 浏览器支持一部分，但是需要添加私有前缀-moz-。例如：font: -moz-button;

#### font 关键字属性值的应用价值