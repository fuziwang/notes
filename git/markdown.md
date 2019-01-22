# Markdown

Markdown 是一种轻量级的「标记语言」，Markdown的优点很多，目前被越来越多的写作爱好者喜爱，语法非常简单，常用的标记符号也不超过十个！

使用Markdown，专注的是你的文字内容而不是排版样式，可以使用它轻松的导出HTML、PDF文档。自我认为，使用它关注一些简单语法实现对应的文本就可以，不必要使用它实现特别负责的功能（流程图、甘特图），如果你需实现复杂功能，专业的图形界面工具会更加方便。

## Markdown使用工具

本人推荐使用[Typora](https://www.typora.io/)。Typora是“所见即所得”的编辑器。你敲入两个井号，加一个空格，再敲入你的标题内容，最后回车，那么标题内容就会被渲染为二级标题的形式。

![typora](https://img-blog.csdnimg.cn/20190120164616615.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Z1eml3YW5n,size_16,color_FFFFFF,t_70)

## Markdown 语法的简要规则

### 标题

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

标题是每篇文章都需要也是最常用的格式，在 Markdown 中，如果一段文字被定义为标题，只要在这段文字前加 `#` 号即可。

`# 一级标题`

`## 二级标题`

`### 三级标题`

以此类推，总共六级标题，建议在井号后加一个空格，这是最标准的 Markdown 语法。

### 列表

```markdown
+ list1
+ list2
+ list3
***
1. list1
2. list2
3. list3
```

熟悉 HTML 的同学肯定知道有序列表与无序列表的区别，在 Markdown 下，列表的显示只需要在文字前加上 `-` 或 `*` 或 `+` 即可变为无序列表，有序列表则直接在文字前加 `1. 2. 3.` 符号要和文字之间加上一个字符的空格。

### 引用

```markdown
> 这是一个引用
```

如果你需要引用一小段别处的句子，那么就要用引用的格式。

`> 例如这样`

只需要在文本前加入 `>` 这种尖括号（大于号）即可

### 图片与链接

插入链接与插入图片的语法很像，区别在一个 `!`号

```markdown
相对路径 ![](images/01.png)
绝对路径 ![](images/https://github.com/fuziwang)
```

### 粗体与斜体

Markdown 的粗体和斜体也非常简单，用两个 `*` 包含一段文本就是粗体的语法，用一个 `*` 包含一段文本就是斜体的语法。

```markdown
例如：这里是**粗体** *这里是斜体*
```

### 表格

表格是我觉得 Markdown 比较累人的地方，例子如下：

```
	| Tables        | Are           | Cool  |
	| ------------- |:-------------:| -----:|
	| col 3 is      | right-aligned | $1600 |
	| col 2 is      | centered      |   $12 |
	| zebra stripes | are neat      |    $1 |

```

这种语法生成的表格如下：

| Tables        | Are           | Cool  |
| ------------- | ------------- | ----- |
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      | $12   |
| zebra stripes | are neat      | $1    |

### 代码框

如果你是个程序猿，需要在文章里优雅的引用代码框，在 Markdown 下实现也非常简单，只需要用两个 ` 把中间的代码包裹起来，如 ``code``。图例：

```markdown
​```JavaScript
	
	var a = 12;
	console.log(a);
```

使用 `tab` 键即可缩进。

### 分割线

分割线的语法只需要另起一行，连续输入三个星号 `***` 即可。

### 自动生成目录

```markdown
[TOC]
自动生成目录
```