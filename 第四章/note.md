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