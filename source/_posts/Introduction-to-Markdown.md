title: Introduction to Markdown
date: 2015-09-25 01:00:45
tags: Markdown
---

> Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档，然后转换成格式丰富的HTML页面。    —— [维基百科](https://zh.wikipedia.org/wiki/Markdown)

<!-- more -->

## 轻量级语言

- Markdown是一种轻量级标记语言，它以纯文本形式(易读、易写、易更改)编写文档，并最终以HTML格式发布。

- Markdown也可以理解为将以Markdown语言编写的语言转换成HTML内容的工具，最初是一个perl脚本Markdown.pl。

## Markdown使用
如果不算**扩展**，Markdown的语法绝对**简单**到让你爱不释手。

废话太多，下面正文，Markdown语法主要分为如下几大部分：
**标题**，**段落**，**区块引用**，**代码区块**，**强调**，**列表**，**分割线**，**链接**，**图片**，**反斜杠 `\`**，**符号'`'**。

#### 1 标题 
两种形式：  
1）使用`=`和`-`标记一级和二级标题。
> 一级标题   
> `=========`   
> 二级标题    
> `---------`
  
效果：
> 一级标题   
> =========   
> 二级标题
> ---------  

2）使用`#`，可表示1-6级标题。
> \# 一级标题   
> \## 二级标题   
> \### 三级标题   
> \#### 四级标题   
> \##### 五级标题   
> \###### 六级标题    

效果：
> # 一级标题   
> ## 二级标题   
> ### 三级标题   
> #### 四级标题   
> ##### 五级标题   
> ###### 六级标题 

#### 2 段落
段落的前后要有空行，所谓的空行是指没有文字内容。若想在段内强制换行的方式是使用**两个以上**空格加上回车（引用中换行省略回车）。

#### 3 区块引用
在段落的每行或者只在第一行使用符号`>`,还可使用多个嵌套引用，如：
> \> 区块引用  
> \>> 嵌套引用  

效果：
> 区块引用  
>> 嵌套引用 

#### 4 代码区块
代码区块的建立是在每行加上4个空格或者一个制表符（如同写代码一样）。如    
普通段落：

void main()    
{    
    printf("Hello, Markdown.");    
}    

代码区块：

    void main()
    {
        printf("Hello, Markdown.");
    }

**注意**:需要和普通段落之间存在空行。

#### 5 强调
在强调内容两侧分别加上`*`或者`_`，如：
> \*斜体\*，\_斜体\_    
> \*\*粗体\*\*，\_\_粗体\_\_

效果：
> *斜体*，_斜体_    
> **粗体**，__粗体__

#### 6 列表
使用`·`、`+`、或`-`标记无序列表，如：
> \-（+\*） 第一项
> \-（+\*） 第二项
> \- （+\*）第三项

**注意**：标记后面最少有一个_空格_或_制表符_。若不在引用区块中，必须和前方段落之间存在空行。

效果：
> + 第一项
> + 第二项
> + 第三项

有序列表的标记方式是将上述的符号换成数字,并辅以`.`，如：
> 1 . 第一项   
> 2 . 第二项    
> 3 . 第三项    

效果：
> 1. 第一项
> 2. 第二项
> 3. 第三项

#### 7 分割线
分割线最常使用就是三个或以上`*`，还可以使用`-`和`_`。

#### 8 链接
链接可以由两种形式生成：**行内式**和**参考式**。    
**行内式**：
> \[younghz的Markdown库\]\(https:://github.com/younghz/Markdown "Markdown"\)。

效果：
> [younghz的Markdown库](https:://github.com/younghz/Markdown "Markdown")。

**参考式**：
> \[younghz的Markdown库1\]\[1\]    
> \[younghz的Markdown库2\]\[2\]    
> \[1\]:https:://github.com/younghz/Markdown "Markdown"    
> \[2\]:https:://github.com/younghz/Markdown "Markdown"    

效果：
> [younghz的Markdown库1][1]    
> [younghz的Markdown库2][2]

[1]: https:://github.com/younghz/Markdown "Markdown"
[2]: https:://github.com/younghz/Markdown "Markdown"

**注意**：上述的`[1]:https:://github.com/younghz/Markdown "Markdown"`不出现在区块中。

#### 9 图片
添加图片的形式和链接相似，只需在链接的基础上前方加一个`！`。
#### 10 反斜杠`\` 
相当于**反转义**作用。使符号成为普通符号。
#### 11 符号'`' 
起到标记作用。如：
>\`ctrl+a\`

效果：
>`ctrl+a`    

## Markdown 使用者
- GitHub
- Stack Overflow
- Reddit
- 简书
- Marxico, which is based on [dillinger](http://dillinger.io/) and [stackedit](https://stackedit.io/editor)

## Reference

[Markdown 介绍](https://github.com/younghz/Markdown)
[Markdown: The Ins and Outs](http://code.tutsplus.com/tutorials/markdown-the-ins-and-outs--net-25482)
[Node Webkit (NW.js) tutorial: creating a Markdown editor](http://www.codeproject.com/Articles/996558/Node-Webkit-NW-js-tutorial-creating-a-Markdown-edi)
[一个 Markdown 编辑器的实现](http://egrcc.github.io/2015/04/12/mango-tutorial/)
[如何写一个实时解析器](http://www.zhihu.com/question/24747506)

