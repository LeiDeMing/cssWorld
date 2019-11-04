## 元素的显示与隐藏
> 使用 CSS 让元素不可见的方法很多，剪裁、定位到屏幕外、明度变化等都是可以的。虽然它们都是肉眼不可见，但背后却在多个维度上都有差别
> 如果希望元素不可见，同时不占据空间，辅助设备无法访问，同时不渲染，可以使用\<script>标签隐藏。例如：

    <script type="text/html">
        <img src="1.jpg"> 
    </script>

> 此时，图片 1.jpg 是不会有请求的。\<script>标签是不支持嵌套的，因此，如果希望在\<script>标签中再放置其他不渲染的模板内容，可以试试使用\<textarea>元素。例如：

    <script type="text/html"> 
        <img src="1.jpg"> 
        <textarea style="display:none;"> 
        <img src="2.jpg"> 
        </textarea> 
    </script>

> 图片 2.jpg 也是不会有请求的。另外，\<script>标签隐藏内容获取使用 script.innerHTML，\<textarea>使用textarea.value。

> 如果希望元素不可见，同时不占据空间，辅助设备无法访问，但资源有加载，DOM 可访问，则可以直接使用 display:none 隐藏。

> 如果希望元素不可见，同时不占据空间，辅助设备无法访问，但显隐的时候可以有transition 淡入淡出效果，则可以使用：

    .hidden {
        position: absolute; 
        visibility: hidden; 
    }

> 如果希望元素不可见，不能点击，辅助设备无法访问，但占据空间保留，则可以使用visibility:hidden 隐藏。例如：

    .vh {
        visibility: hidden; 
    }

> 如果希望元素不可见，不能点击，不占据空间，但键盘可访问，则可以使用 clip 剪裁隐藏。例如：

    .clip {
        position: absolute; 
        clip: rect(0 0 0 0); 
    } 
    .out { 
        position: relative; 
        left: -999em; 
    }

> 如果希望元素不可见，不能点击，但占据空间，且键盘可访问，则可以试试 relative隐藏。例如，如果条件允许，也就是和层叠上下文之间存在设置了背景色的父元素，则也可以使用更友好的 z-index 负值隐藏。例如：

    .lower {
        position: relative; 
        z-index: -1;
    }

> 如果希望元素不可见，但可以点击，而且不占据空间，则可以使用透明度。例如：

    .opacity {
        position: absolute; 
        opacity: 0; 
        filter: Alpha(opacity=0); 
    }

> 如果单纯希望元素看不见，但位置保留，依然可以点可以选，则直接让透明度为 0。例如：

    .opacity {
        opacity: 0; 
        filter: Alpha(opacity=0); 
    }

### display 与元素的显隐
> 对一个元素而言，如果 display 计算值是 none 则该元素以及所有后代元素都隐藏，如果是其他 display 计算值则显示。
> 在 Firefox 浏览器下，display:none 的元素的 background-image 图片是不加载的，包括父元素 display:none 也是如此；如果是 Chrome 和 Safari 浏览器，则要分情况，若父元素 display:none，图片不加载，若本身背景图所在元素隐藏，则图片依旧会去加载；对 IE浏览器而言，无论怎样都会请求图片资源
> 另外，如果不是 background-image 图片，而是\<img>元素，则设置 display:none 在所有浏览器下依旧都会请求图片资源。
> HTML 中有很多标签和属性天然 display:none，如\<style>、\<script>和 HTML5中的\<dialog>元素（如果浏览器支持）。如果这些标签在\<body>元素中，设置 display: lock 是可以让内联 CSS 和 JavaScript 代码直接在页面中显示的。例如：,页面上就会出现.l { float: left; }的文本信息；如果再设置 contenteditable= "true"，在有些浏览器下（如 Chrome），甚至可以直接实时编辑预览页面的样式。

    <style style="display:block;"> 
        .l { float: left; } 
    </style>

> 还有一些属性也是天然 display:none 的。例如，hidden 类型的\<input>输入框：

    <input type="hidden" name="id" value="1">

> HTML5 中新增了 hidden 这个布尔属性，可以让元素天生 display:none 隐藏。IE11 以及其他现代浏览器都支持它，因此，如果要兼容桌面端，需要如下 CSS 设置：

    [hidden] { 
        display: none; 
    }

> display:none 显隐控制并不会影响 CSS3 animation 动画的实现，但是会影响 CSS3 transition 过渡效果执行，因此 transition 往往和 visibility 属性走得比较近。

### visibility 与元素的显隐
#### 不仅仅是保留空间这么简单
> 有一些人简单地认为 display:none 和 visibility:hidden 两个隐藏的区别就在于：display:none 隐藏后的元素不占据任何空间，而 visibility:hidden 隐藏的元素空间依旧保留。实际上并没有这么简单，visibility 是一个非常有故事的属性。
> + 1．visibility 的继承性
> + visibility 与 CSS 计数器
> + 3．visibility 与 transitio(这是为什么呢？因为 CSS3 transition 支持的 CSS 属性中有 visibility，但是并没有 display。)由于 transition 可以延时执行，因此，和 visibility 配合可以使用纯 CSS 实现 hover延时显示效果，由此提升我们的交互体验
> transition 隐藏除了和 transition 友好外，与 display:none 相比，其在 JavaScript也更加友好。存在这样的场景：我们需要对隐藏元素进行尺寸和位置的获取，以便对交互布局进行精准定位。此时，建议使用 visibility 隐藏,原因是，我们可以准确计算出元素的尺寸和位置，如果使用的是 display:none，则无论是尺寸还是位置都会是0，计算就会不准确