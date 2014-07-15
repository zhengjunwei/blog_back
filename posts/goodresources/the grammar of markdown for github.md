```

标题

支持六级标题

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题 ######
段落

空行分段
一个回车不分段，本行续上行。
行尾两个空格保持换行
段落缩进

邮件体段落缩进：

> 第一级段落缩进。
>
> > 第二级段落缩进。
>
> 返回一级段落缩进。
代码块

四个空格缩进是代码块

    $ printf "Hello, world.\n"
列表

无序列表

* 星号、减号、加号开始列表。

  - 列表层级和缩进有关。

    + 和具体符号无关。

* 返回一级列表。
有序列表

1. 数字和点开始有序列表。

   1. 注意子列表的缩进位置。

      1. 三级列表。
      1. 编号会自动更正。

   1. 二级列表，编号自动更正为2。

2. 返回一级列表。
列表段落

1. 列表项可以折行，
   对齐则自动续行。

2. 列表项可包含多个段落。

    空行开始的新段落必须缩进四个空格，
    段落才属于列表项。

3. 列表中的代码块要缩进8个空格。

        $ printf "Hello, world.\n"
分割线

三条或更多短线（或星号、下划线）\
显示为分隔线。

---
粗体和斜体

这些都是 **粗体** 或 __粗体__ ，
这写都是 *斜体* 或 _斜体_ 。
删除线

~~删除线~~ 效果
下划线

<u>下划线</u> 效果
上标、下标

- Water: H<sub>2</sub>O
- E = mc<sup>2</sup>
等宽字体

行内反引号嵌入代码，如: `git status` 。
链接

访问 [Google](http://google.com "Search")
- 访问 [GitHub][1]
- 访问 [WorldHello][]

 [1]: http://github.com "Git host"
 [worldhello]: http://www.worldhello.net
内部跳转

<a name="md-anchor" id="md-anchor"></a>

跳转至 [文内链接](#md-anchor) 。
图片

![GitHub](/images/github.png "Logo")

GitHub Logo: ![GitHub][logo]

[logo]: /images/github.png "Logo"
混用HTML

<div style="background:#bbb;">
  HTML块中不能混用 **标记语法**
</div>

```