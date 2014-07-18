---
date:2014-7-03
---

- 声明可以这样写

```
/*------------------------------------*\
    $CONTENTS
\*------------------------------------*/
/**
 * CONTENTS............You’re reading it!
 * RESET...............Set our reset defaults
 * FONT-FACE...........Import brand font files
 */
 ```

```
.styleguide-format{
	color:#000;
	background-color:rgba(0,0,0,.5);
	border:1px solid #0f0;
}

.multiple,
.classes,
.get-new-lines{
	display:block;
}
```

一份规划良好的 CSS 应当按照如下排列：

Reset 万物之根源
元素类型 没有 class 的 h1、ul 等
对象以及抽象内容 最一般、最基础的设计模式
子元素 由对象延伸出来的所有拓展及其子元素
修补 针对异常状态


- 我们可以发现，.widget-heading 是 .widget 的子元素，因为前者比后者多缩进了一级。这样通过缩进就可以让开发者在阅读代码时快速获取这样的重要信息。

```
.widget{
    padding:10px;
    border:1px solid #BADA55;
    background-color:#C0FFEE;
    -webkit-border-radius:4px;
       -moz-border-radius:4px;
            border-radius:4px;
}
    .widget-heading{
        font-size:1.5rem;
        line-height:1;
        font-weight:bold;
        color:#BADA55;
        margin-right:-10px;
        margin-left: -10px;
        padding:0.25em;
    }
```


- 文档注释方法

```
/**
 * 这是一个文档块（DocBlock）风格的注释。
 *
 * 这里开始是描述更详细、篇幅更长的注释正文。当然，我们要把行宽控制在 80 字以内。
 *
 * 我们可以在注释中嵌入 HTML 标记，而且这也是个不错的办法：
 *
    <div class=foo>
        <p>Lorem</p>
    </div>
 *
 * 如果是注释内嵌的标记的话，在它前面不加星号，否则会被复制进去。
 */

 ```