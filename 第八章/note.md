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
> 目前，非常多网站的通用 font-family 直接就是：html { font-family: 'Microsoft YaHei'; }
> 知道问题在哪里吗？这样一设置，就意味着所有操作系统下的所有浏览器都要使用“微软雅黑”字体。假如说用户的 iMac 或者 macbook 因为某些原因安装了“微软雅黑”字体，那岂不是这些系统原本更加漂亮的中文字体就不能使用了？
> 于是，人们就会有这样的需求：希望非 Windows 系统下不要使用“微软雅黑”字体，而是使用其系统字体。怎么处理呢？一种方法是可以试试使用非标准的-apple-system 等关键字字体，具体方法如下：html { font-family: -apple-system, BlinkMacSystemFont, 'Microsoft YaHei'; }
> 顺便多说两句，实际上还真有标准的系统字体关键字，叫作 system-ui，使用示例如下：html { font-family: system-ui; }
> 压轴的总在最后，显然还有个更好的方法就是使用这里的 font 关键字，这是标准属性，10 年前浏览器就支持了，可以放心使用,CSS 代码如下（三选一即可）：

    html { font: menu; } 
    body { font-size: 16px; }

    html { font: small-caption; } 
    body { font-size: 16px; } 

    html { font: status-bar; } 
    body { font-size: 16px; }

### 真正了解@font face 规则
#### @font face 的本质是变量
> 虽然说 CSS3 新世界中才出现真正意义上的变量 var，但实际上，在 CSS 世界中已经出现了本质上就是变量的东西，@font face 规则就是其中之一。@font face 本质上就是一个定义字体或字体集的变量，这个变量不仅仅是简单地自定义字体，还包括字体重命名、默认字体样式设置等。
> @font face 规则支持的 CSS 属性有 font-family、src、font-style、font-weigh、unicode-range、font-variant、font-stretch 和 font-feature-settings。例如：

    @font-face { 
        font-family: 'example'; 
        src: url(example.ttf); 
        font-style: normal; 
        font-weight: normal; 
        unicode-range: U+0025-00FF; 
        font-variant: small-caps; 
        font-stretch: expanded; 
        font-feature-settings："liga1" on; 
    }

> 属性还是挺多的，而且有些属性估计是他认识你，你不认识他。但是从实用角度来讲，有些属性其实可以不用去深究，比如 font-variant、font-stretch 和 font-featuresettings 这 3 个属性。为什么呢？因为按照我的经验，这 3 个属性给我感觉更像是专为英文设计的，所以如果不是有业务需要，可以先放一放。再加上后两个是 CSS3 新属性，本书就不做进一步介绍了,好，现在，是不是感觉压力一下子小了很多？我们需要在意的可以自定义的属性就只剩下下面这些：

    @font-face { 
        font-family: 'example'; 
        src: url(example.ttf); 
        font-style: normal; 
        font-weight: normal; 
        unicode-range: U+0025-00FF; 
    }

##### 1．font-family
> 这里的 font-family 可以看成是一个字体变量，名称可以非常随意，如直接用一个美元符号'$'。例如：

    @font-face { 
        font-family: '$';
        src: url(example.ttf); 
    }

> 虽然说自己变量名可以很随意，但是有一类名称不能随便设置，就是原本系统就有的字体名称。例如，如果使用下面的代码从此“微软雅黑”字体就变成了这里 example.ttf 对应的字体了。

    @font-face { 
        font-family: 'Microsoft Yahei'; 
        src: url(example.ttf); 
    }

##### 2．src
> src 表示引入的字体资源可以是系统字体，也可以是外链字体。如果是使用系统安装字体，则使用 local()功能符；如果是使用外链字体，则使用 url()功能符。由于 local()功能符IE9 及其以上版本浏览器才支持，非常实用，而本书目标浏览器包含 IE8 浏览器，因此不做展开，有兴趣的读者可以参考[博客文章](http://www.zhangxinxu.com/ wordpress/?p=6063)
> 目前在业界，凡是使用自定义字体的，差不多都是下面这种格式：

    @font-face { 
        font-family: ICON; 
        src: url('icon.eot') format('eot'); 
        src: url('icon.eot?#iefix') format('embedded-opentype'), 
            url('icon.woff2') format("woff2") 
            url('icon.woff') format("woff"), 
            url('icon.ttf') format("typetrue"), 
            url('icon.svg#icon') format('svg'); 
        font-weight: normal; 
        font-style: normal; 
    }

> 上面这段 CSS 代码一共出现了 5 种字体格式，分别是.eot、.woff2、.woff、.ttf 和.svg。
> + svg 格式是为了兼容 iOS 4.1 及其之前的版本，考虑到现如今 iOS 的版本数已经翻了一番，所以 svg 格式的兼容代码大可舍弃。
> + eot 格式是 IE 私有的。注意，目前所有版本的 IE 浏览器都支持 eot 格式，并不是只有 IE6～IE8 支持。只是，IE6～IE8 仅支持 eot 这一种字体格式。
> + woff 是 web open font format 几个词的首字母简写，是专门为 Web 开发而设计的字体格式，显然是优先使用的字体格式，其字体尺寸更小，加载更快。Android 4.4 开始全面支持。
> + woff2 是比 woff 尺寸更小的字体，小得非常明显。因此，Web 开发第一首选字体就是 woff2，只是此字体目前仅 Chrome 和 Firefox 支持得比较好。• ttf 格式作为系统安装字体比较多，Web 开发也能用，就是尺寸大了点儿，优点在于老版本 Android 也支持。
> 但是，如果我们仔细看上面的代码就会发现，在IE 浏览器下，使用的永远是 eot 格式的字体（因为排在最前），而 woff 格式字体从 IE9 开始就支持了，浏览器好的特性都没用上啊！但是，我们又不能简单地把 woff 格式提前，否则会影响低版本 IE 浏览器的字体显示。怎么办呢？有一个小技巧如下

    @font-face { 
        font-family: ICON; 
        src: url('icon.eot') format('eot'); 
        src: local('☺'),
            url('icon.woff2') format("woff2") 
            rl('icon.woff') format("woff"), 
            url('icon.ttf') format("typetrue"); 
    }

> 由于 IE6～IE8 不认识功能符，于是下面一个 src 被完美避开了。此时，IE9 浏览器就可以正大光明地享用 woff 字体格式了！
> font-weight:normal 和 font-style:normal 是不是多余的？我的回答是，如果你没有同字体名的多字体设置，则它就是多余的，至少我在常规项目中删掉这两行 CSS 没有出现任何异常
> 最后的问题是：format()功能符有什么作用，可不可以省略？我的回答是最好不要省略。format()功能符的作用是让浏览器提前知道字体的格式，以决定是否需要加载这个字体，而不是加载完了之后再自动判断。举个例子，下面的 CSS 代码在 Chrome 浏览器下就会同时加载eot 和 ttf 两种格式字体：

    @font-face { 
        font-family: ICON; 
        src: url('icon.eot'), 
        url('icon.ttf'); 
    }

> 浏览器对文件格式的判断不是基于后缀名，下面这种写法只会加载 ttf 这一种格式字体，因为浏览器提前知道了文件格式是自己无法识别的：

    @font-face { 
        font-family: ICON; 
        src: url('icon.eot') format("embedded-opentype"), 
        url('icon.ttf'); 
    }

> 于是，综合上面的全部知识会发现，业界常用的这套东西，其实可以优化成下面这样：

    @font-face { 
        font-family: ICON; 
        src: url('icon.eot'); 
        src: local('☺'),
            url('icon.woff2') format("woff2"), 
            url('icon.woff') format("woff"), 
            url('icon.ttf'); 
    }

##### 3．font-style
> @font face 规则中的 font-style 和 font-weight 类似，都是用来设置对应字体样式或字重下该使用什么字体。因为有些字体可能会有专门的斜体字体，注意这个斜体字体并不是让文字的形状倾斜，而是专门设计的倾斜的字体，所以很多细节会跟物理上的请求不一样。于是，我在 CSS 代码中使用 font-style:italic 的时候，就会调用这个对应字体，如下意：

##### 4．font-weight
> font-weight 和 font-style 类似，只不过它定义了不同字重、使用不同字体
> font-weight 就被委以重任来实现“响应式图标”，而 IE7 和 IE8 浏览器都是支持这个的
> 所谓“响应式图标”，指的是字号较大时图标字体细节更丰富，字号较小时图标字体更简单的响应式处理,[demo](ttp://demo.cssworld.cn/8/5-1.php)

##### 5．unicode-range
> unicode-range 的作用是可以让特定的字符或者特定范围的字符使用指定的字体。

#### @font face 与字体图标技术
> 从面向未来的角度讲，字体图标技术的使用会越来越边缘化，因为和 SVG 图标技术相比，其唯一的优势就是兼容一些老的 IE 浏览器。等再过几年，IE8 等浏览器彻底被淘汰了，我们就
> 见书257
> 当我们使用工具生成图标字体的时候，无论是在线工具还是本地工具，其中间的媒介都是 SVG 图标，但是并不是所有的 SVG 图标都是可以的，根据我的经验，最好满足下面3 点。
> + （1）纯路径，纯矢量，不要有 base64 内联图形。
> + （2）使用填充而非描边，也尽量避免使用一些高级的路径填充覆盖技巧。
> + （3）宽高尺寸最好都大于 200，因为字体生成的时候，坐标值会四舍五入，SVG 尺寸过小
会导致坐标取值偏差较大，使最终的图标不够精致。
> 有人可能会问：我可不可以把映射字符直接写在页面中，而不是放在:before 伪元素中？也就是不需要下面的 CSS 代码：

    .icon-microphone:before { 
        content: '\1f3a4' 
    }

> 而是直接用：

    <i class="icon">&#x1f3a4;</i>

> 从技术实现的角度来讲这是完全可以的，而且不支持伪元素的 IE7 等浏览器都支持这样做。但是在实际开发的时候，我并不建议这么做，有两点原因：一是不好维护，如果以后字符映射关系改变，而图标 HTML 是散布在各个页面中的，那么我们的改动就会很麻烦；二是从语义角度考虑，图标字符往往是不包含任何意义的，应该没有必要让搜索引擎知道，也无须让辅助设备读取，而伪元素恰好有这样的功能，如果内联在 HTML 中，则反而成了一种干扰。

### 文本的控制
#### text-indent 与内联元素缩进
> 顾名思义，text-indent 就是对文本进行缩进控制,但是这种缩进对内容要求比较高，如果段落掺杂英文、数字或者图片等内容，缩进反而可能会给人以参差不齐的感觉，加上现代 Web 形式更加多样，text-indent 在实际项目中的应用已经脱离了它原本的设计初衷。
> 很多人喜欢设置一个很大的 text-indent 负值，如 text-indent:-9999em，我是不建议这么做的。首先，这样做在某些设备下有潜在的性能风险，体现在滚屏的时候会发生卡顿；其次，对于一些智能设备的屏幕阅读软件，如 VoiceOver，如果内容缩进在屏幕之外，它是不会读取的，这样就降低了页面的无障碍访问能力,另外，text-indent 负值缩进在部分浏览器下会影响元素的 outline 区域，通常需要再设置 overflow:hidden。
> text-indent 的百分比值是相对于当前元素的“包含块”计算的，而不是当前元素。由于 text-indent 最终作用的是当前元素里的内联盒子，因此很容易让人误以为 text-indent的百分比值是相对于当前元素宽度计算的。
> 眼见为实，[demo](http://demo.cssworld.cn/8/6-1.php)。结果text-indent: -120px 缩进在盒子之外，而 text-indent:-100%早已缩进到天涯海角，连影子都看不到
> 在正常情况下，确实可以隐藏文字，而且无论是一行还是多行都可以非常兼容地隐藏，因为一般情况下“包含块”的宽度都会大于子元素的宽度，所以我们是看不出问题来的。但是一旦“包含块”的宽度反而小了，那么这段代码就会出现样式问题。我举个简单的例子：

    <div style="position:absolute;"> 
        <p class="hide-text" style="position:absolute;">坚挺</p> 
    </div>

> 此时“坚挺”两个字绝对是纹丝不动，绝对定位具有包裹性，而子元素也是绝对定位的，hide-text 所在元素的“包含块”的宽度就是 0，此时 text-indent:100%计算值也是 0，文本缩进隐藏彻底失败
> 与文本控制相关的 CSS 属性支持百分比值的并不多，例如，letter-spacing、word-spacing 和 text-shadow 等都不支持,例如，word-spacing 新草案增加了百分比值并且已有浏览器开始支持
> text-indent 百分比值还有没有实际价值呢？从理论上讲，我们可以使用 textindent 实现宽度已知内联元素的居中效果，[例如](http://demo.cssworld.cn/8/6-2.php)
> 另外，text-indent 还有一个比较生僻的应用。在 Chrome 浏览器下，如果\<img>标签没有设置 src 属性，则会出现一个灰色的线框,根据研究我发现，此灰色边框是预留给 alt 属性值用的，是内联水平元素，因此可以使用text-indent 属性隐藏之

    img { 
        text-indent: -400px; 
        overflow: hidden; 
    }

> 小知识。
> + （1）text-indent 仅对第一行内联盒子内容有效。
> + （2）非替换元素以外的 display 计算值为 inline 的内联元素设置 text-indent 值无效，如果计算值是 inline-block/inline-table 则会生效。因此，如果父级块状元素设置了 text-indent 属性值，子 inline-block/inline-table 需要设置 text-indent:0重置。
> + （3）\<input>标签按钮 text-indent 值无效。
> + （4）\<button>标签按钮 text-indent 值有效，但是存在兼容性差异，IE 浏览器理解为单标签，百分比值按照容器计算，而 Chrome 和 Firefox 浏览器标签内还有其他 Shadow DOM 元素，因此百分比值是按照自身的尺寸计算的。
> + （5）\<input>和\<textarea>输入框的 text-indent 在低版本 IE 浏览器下有兼容问题。

#### letter-spacing
> letter-spacing 可以用来控制字符之间的间距，这里说的“字符”包括英文字母、汉字以及空格等。
> 具有以下一些特性
> + （1）继承性。
> + （2）默认值是 normal 而不是 0。虽然说正常情况下，normal 的计算值就是 0，但两者
还是有差别的，在有些场景下，letter-spacing 会调整 normal 的计算值以实现更好的版
面布局。
> + （3）支持负值，且值足够大的时候，会让字符形成重叠，甚至反向排列（非 IE 浏览器）。
> + （4）和 text-indent 属性一样，无论值多大或多小，第一行一定会保留至少一个字符。letter-spacing 还有一个非常有意思的特性就是，在默认的左对齐情况下，无论值如何设置，第一个字符的位置一定是纹丝不动的。
> + （5）支持小数值，即使 0.1px 也是支持的，但并不总能看到效果，这与屏幕的密度有关。对普通的桌面显示器，设备像素比是 1，最小渲染单位是 1px，因此，需要至少连续 10 个字符，才能看到 0.1px 产生的 1px 间距变化，如果是 9 个字符，则不会有效果，这很可能会让人误以为 letter-spacing 不支持非常小的数值，实际上是支持的。
> + （6）暂不支持百分比值。
> 由于 letter-spacing 负值的字体重叠特性，我们还可以利用该属性实现一些文本动效，核心 CSS 代码如下：[demo](http://demo.cssworld.cn/8/6-4.php)

    .title { 
        animation: textIn 1s both; 
    } 
    @keyframes textIn { 
        0% { 
            letter-spacing: -200px; 
        } 
        100% { 
            letter-spacing: 0; 
        } 
    }

#### word-spacing 与单词间距
> word-spacing 和 letter-spacing 名称类似，其特性也有很多共通之处：
> + （1）都具有继承性。
> + （2）默认值都是 normal 而不是 0。通常情况下，两者表现并无差异。
> + （3）都支持负值，都可以让字符重叠，但是对于 inline-block 和 inline-table 元素却存在兼容性差异，Chrome 浏览器下可以重叠，IE 和 Firefox 浏览器下则再大的负值也不会重叠，因此不适合使用 word-spacing 来清除空白间隙。
> + （4）都支持小数值，如 word-spacing:0.5px。 
> + （5）在目前的 CSS2.1 规范中，并不支持百分比值，但新的草案中新增了对百分值的支持，是根据相对于字符的“步进宽度”（advance width）计算的。这属于新世界内容，本书不做介绍。
> + （6）间隔算法都会受到 text-align:justify 两端对齐的影响。
> 当然也有差异。letter-spacing 作用于所有字符，但 word-spacing 仅作用于空格字符。注意，是作用在“空格”上，而不是字面意义上的“单词”
> 另外，在命名上，word-spacing 之所以称为word-spacing 而不是 blank-spacing 之类的，主要原因是此属性当初主要为英文类排版设计，而英文单词和单词之间是以空格分隔的，要想控制单词之间的间距，自然就向“空格”开刀了。
> 两个按钮距离自动分开。在 IE6 时代，这确实是个非常好的方法，但如今下面的做法可能要更合适些：

    button + button { 
        margin-left: 20px; 
    }

#### 了解 word-break 和 word-wrap 的区别
> word-break几个关键字值的含义具体解释如下。
> + normal：使用默认的换行规则。
> + break-all：允许任意非 CJK（Chinese/Japanese/Korean）文本间的单词断行。
> + keep-all：不允许 CJK 文本中的单词换行，只能在半角空格或连字符处换行。非 CJK
文本的行为实际上和 normal 一致

> word-wrap几个关键字值的含义具体解释如下。
> + normal：就是大家平常见得最多的正常的换行规则。
> + break-word：一行单词中实在没有其他靠谱的换行点的时候换行。
> word-wrap 属性其实也是很有故事的，它之前由于和 word-break 长得太像，难免会让人记不住或搞混，于是在 CSS3 规范里，这个属性的名称被修改了，叫作 overflow-wrap
> 下面回到重点：word-break:break-all 和 word-wrap:break-word。首先，两者长相神似，都有 word，都有 break，位置都还一样，一个有两个 break，一个有两个 word；其次，两者的功能作用也类似，这两个声明都能使连续英文字符换行，那么它们的区别到底是什么？[对比效果](http://demo.cssworld.cn/8/6-5.php)

#### white-space 与换行和空格的控制
##### white-space 的处理模型
> white-space 属性声明了如何处理元素内的空白字符，这类空白字符包括 Space（空格）键、Enter（回车）键、Tab（制表符）键产生的空白。因此，white-space 可以决定图文内容是否在一行显示（回车空格是否生效），是否显示大段连续空白（空格是否生效）等,其属性值包括下面这些。[如图](https://ws1.sinaimg.cn/large/0060ZzrAgy1g8ifipqn1uj30oa06wjs9.jpg)
> + normal：合并空白字符和换行符。
> + pre：空白字符不合并，并且内容只在有换行符的地方换行。
> + nowrap：该值和 normal 一样会合并空白字符，但不允许文本环绕。
> + pre-wrap：空白字符不合并，并且内容只在有换行符的地方换行，同时允许文本环绕。
> + pre-line：合并空白字符，但只在有换行符的地方换行，允许文本环绕。

##### white-space 与最大可用宽度
>  white-space 设置为 nowrap 的时候，元素的宽度此时表现为“最大可用宽度”，换行符和一些空格全部合并，文本一行显示。
> 在实际 Web 开发的时候，white-space 的应用非常广泛，下面举几个例子。
> + （1）“包含块”尺寸过小处理。绝对定位以及 inline-block 元素都具有包裹性，当文本内容宽度超过包含块宽度的时候，就会发生文本环绕现象。还记不记得之前遇到过的“一柱擎天”的例子,可以对其使用 white-space:nowrap 声明让其如预期的那样一行显示。
> + （2）单行文字溢出点点点效果。text-overflow:ellipsis 文字内容超出打点效果离不开 white-space:nowrap 声明。
> + （3）水平列表切换效果。水平列表切换是网页中常见的交互效果，如果列表的数目是不固定的，使用 white-space:nowrap 使列表一行显示会是个非常不错的处理。
> + [演示页面](http://demo.cssworld.cn/8/6-6.php)

#### text-align 与元素对齐
> 就最终的渲染表现来看，Chrome 等浏览器应该对文本内容进行了算法区分，对 CJK 文本使用了 letter-spacing 间隔算法，而对非 CJK 文本使用的是 word-spacing 间隔算法，但 IE 浏览器则就一个 word-spacing 间隔算法。于是就会出现明明左右 padding 大小设置一样，结果右侧空白明显更大的尴尬问题
> 不过，好在 IE 有一个私有的 CSS 属性 text-justify（目前也写入规范草案了）可以实现中文两端对齐的。于是，通过使用下面的 CSS 代码组合就可以实现全部浏览器都兼容的中文两端对齐效果,[http://demo.cssworld.cn/8/6-7.php](http://demo.cssworld.cn/8/6-7.php)

    .justify { 
        text-align: justify; 
        text-justify: inter-ideograph; 
    }

> text-align:justify 除了实现文本的两端对齐，还可以实现容错性更强的两端对齐布局效果