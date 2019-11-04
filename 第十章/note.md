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