---
title: Markdownlint规则
date: 2025-07-11 23:20:00 +0800
categories: [语法规则]
tags: [Markdown]
description: Markdownlint是一个检查Markdown文件中存在的问题的工具，本文对Markdownlint的语法提示进行介绍，借助本文能更好地编写一篇优秀的Markdown文档。
---

## Markdownlint规则

### MD001

- Heading levels should only increment by one level at a time  
标题的级数只能每次按照加一从一级到六级递增，不能跨级

例如：

```makrdown
# Heading 1

### Heading 3

这里的标题级数跳了两级
```

### MD002

- First heading should be a top level heading  
文档的第一个标题必须为一级标题

> MD002目前已弃用，MD041为它的改进版

### MD003

- Heading style  
在整篇文档中要使用统一的标题样式

> 有六种样式：atx 、atx_closed 、consistent 、setext 、setext_with_atx 、setext_with_atx_closed  
> consistent（默认）：整篇文章统一样式  
> atx：在标题内容前加`#`作为标题样式符号  
> atx_closed：在标题内容的前后都加上`#`作为标题样式符号  
> setext：采用`-`或`=`作为标题样式符号，只支持一级和二级标题  
> setext_with_atx / setext_with_atx_closed：允许在setext样式标题（仅支持一级和二级标题）的文档中使用级别在三级及三级以上的atx标题样式

### MD004

- Unordered list style  
整篇文章的无序列表格式要统一

> 有五种样式：asterisk 、 consistent 、 dash 、 plus 、 sublist  
> consistent：一致的格式  
> asterisk：采用`*`  
> dash：采用`-`  
> plus：采用`+`  
> sublist：定义多重列表时使用不同的符号定义

### MD005

- Inconsistent indentation for list items at the same level  
同一级的列表缩进必须一致

### MD006

- Consider starting bulleted lists at the beginning of the line  
一级列表不能被缩进

### MD007

- Unordered list indentation  
无序列表嵌套缩进时默认采用两个空格

> 缩进有以下几种取值：indent 、 start_indent 、 start_indented  
> indent：缩进采用的空格，默认为2  
> start_indent：在start_indented未设置时，可以允许一级列表的缩进空格数跟其他列表不一样  
> start_indented：允许一级列表按照默认值（2）进行缩进，跟MD006相反

### MD009

- Trailing spaces  
行尾最多可以添加两个空格，超过会给出警告，两个空格正好可以用于换行

> br_spaces：换行的空格，默认为2，小于2则为0不换行  
> list_item_empty_lines：允许列表在行尾使用默认的空格数缩进空行，默认关闭  
> strict：允许不必要的换行，默认关闭

### MD010

- Hard tabs  
不能使用tab键缩进，要使用空格

### MD011

- Reversed link syntax  
`(`、`)`、`[`、`]`等内联形式符号的使用错误

例如：

```markdown
(错误的链接语法)[https://www.example.com/]

修复：
[正确的链接语法](https://www.example.com/)
```

### MD012

- Multiple consecutive blank lines  
多个连续的空行，默认是一个  
在代码块中该规则不生效

### MD013

- Line length  
默认行的最大长度，默认为80  
在代码块、标题行中也同样生效

### MD014

- Dollar signs used before commands without showing output  
在代码块中，终端命令前面不需要有美元符号`$`  
如果代码块中既有终端命令，也有命令的输出，则终端命令前可以有美元符号($)，如：

```shell
$ ls
foo bar
$ cat foo
hello world
```

### MD018

- No space after hash on atx style heading  
atx样式标题符号`#`跟标题内容中间要带有一个空格

```markdown
#错误的标题

# 正确的标题
```

### MD019

- Multiple spaces after hash on atx style heading  
atx样式标题符号`#`跟标题内容中间必须只能带有一个空格

### MD020

- No space inside hashes on closed atx style heading  
在closed_atx样式标题中，前后两个`#`符号跟标题内容要用一个空格隔开  

```markdown
#错误标题 1#

##错误标题 2##

# 正确标题 1 #

## 正确标题 2 ##
```

### MD021

- Multiple spaces inside hashes on closed atx style heading  
在closed_atx样式标题中，前后两个`#`符号跟标题内容只能用一个空格隔开

### MD022

- Headings should be surrounded by blank lines  
标题行的上下行必须为空行

### MD023

- Headings must start at the beginning of the line  
标题行不能缩进

### MD024

- Multiple headings with the same content  
标题的内容不能重复

> 参数：  
> allow_different_nesting：只检查同级标题，默认关闭  
> siblings_only：只检查同级标题，默认关闭  
> 两个参数都可以用

### MD025

- Multiple top-level headings in the same document  
一篇文档内只能有一个顶级标题

> 参数：  
> front_matter_title：字符串，指定在文档开头处的front matter中的标题，这个标题将作为整篇文档的最高级标题，如果文档中再次出现最高级标题，将会给出警告，另外，如果不想在front matter中指定标题，就把本参数的值设置为""  
> level：指定文档最高级的标题，默认是1

### MD026

- Trailing punctuation in heading  
标题行的尾部不能有`.,;:!。，；：！`等符号

> 通过参数punctuation去指定

### MD027

- Multiple spaces after blockquote symbol  
引用符号`>`后只能有一个空格

### MD028

- Blank line inside blockquote  
两个区块引用块之间不能只用一个空行进行隔开，或者同一引用区块中不能有空行

```markdown
错误示例：
> This is a blockquote
> which is immediately followed by

> this blockquote. Unfortunately
> In some parsers, these are treated as the same blockquote.

正确示例1：
> This is a blockquote.

And Jimmy also said:

> This too is a blockquote.

正确示例2：
> This is a blockquote.
>
> This is the same blockquote.
```

### MD029

- Ordered list item prefix  
有序列表的前缀序号格式必须只用1或者从1开始的加1递增数字

### MD030

- Spaces after list markers  
列表（有序、无序）的前缀符号和文字之间用1个空格隔开  
在列表嵌套或者同一列表项中有多个段落时，无序列表缩进两个空格，有序列表缩进3个空格

### MD031

- Fenced code blocks should be surrounded by blank lines  
代码块的前后都要有空行跟其他块隔开

### MD032

- Lists should be surrounded by blank lines  
列表的前后都要有空行跟其他块隔开

### MD033

- Inline HTML  
在Markdown文档中不能使用HTML的元素

### MD034

- Bare URL used  
链接要用尖括号括住

```markdown
错误：
For more information, see https://www.example.com/.

正确：
For more information, see <https://www.example.com/>.
```

如果不想将一个Url识别成链接，将它变成代码块，例`https://www.example.com/`

### MD035

- Horizontal rule style  
创建水平线时整篇文档要统一，要和文档中第一次创建水平线使用的符号一致  

### MD036

- Emphasis used instead of a heading  
不能使用强调来替代标题

### MD037

- Spaces inside emphasis markers  
强调标记`_`或`*`与被强调文本之间不能有空格

### MD038

- Spaces inside code span elements  
使用反引号括住的内容与反引号不能带有空格

```markdown
`some text `

` some text`

正确使用：
`some text`
```

如果想在反引号嵌入到代码块中，在反引号和代码块标识符之间加入空格

例：

```markdown
`` `backticks` ``
```

效果：
`` `backticks` ``

### MD039

- Spaces inside link text  
链接名和包围它的中括号之间不能有空格，但链接名中间可以有空格

```markdown
错误：
[ a link ](https://www.example.com/)

正确：
[a link](https://www.example.com/)
```

### MD040

- Fenced code blocks should have a language specified  
采用反引号括起来的代码块需要定义一种语言，例如plain、shell

`````markdown
```text
Plain text in a code block
```
`````

### MD041

- First line in a file should be a top-level heading  
文档的第一个非空行应该为顶级标题，默认为一级标题

> 参数：  
> level：顶级标题，默认为1  
> front_matter_title：字符串，指定在文档开头处的front matter中的标题，这个标题将作为整篇文档的最高级标题

### MD042

- No empty links  
链接的地址不能为空

```markdown
[空链接]()

[有效链接](https://example.com/)

[空的段落](#)

[有效的段落](#fragment)
```

### MD043

- Required heading structure  
要求标题遵循一定的结构，默认是没有规定的结构

### MD044

- Proper names should have the correct capitalization  
指定一些名称，会检查它是否有正确的大写

> 参数：  
> names：文档会检查这个列表里的名称是否有正确的大小写  
> code_blocks：包含代码块，默认开启  
> html_elements：包含html元素，默认开启

### MD045

- Images should have alternate text (alt text)  
图片链接必须包含描述文本（alt text）

### MD046

- Code block style  
整篇文档采用一致的代码格式

> 参数：consistent、fenced 、indented
> consistent：统一样式
> fenced：采用反引号
> indented：采用缩进

### MD047

- Files should end with a single newline character  
文件的结尾必须是一个换行符或者空行

### MD048

- Code fence style  
代码块使用的符号要配置的符号一致

> 参数：backtick 、consistent 、tilde
> consistent：统一样式
> backtick：反引号
> tilde：波浪号

### MD049

- Emphasis style should be consistent  
斜体文本使用的符号要配置的符号一致

> 参数：asterisk 、consistent 、underscore
> consistent：统一样式
> asterisk：采用星号`*`
> underscore：采用下划线`_`

### MD050

- Strong style should be consistent
加粗文本使用的符号要配置的符号一致

> 参数：asterisk 、consistent 、underscore
> consistent：统一样式
> asterisk：采用星号`**`
> underscore：采用下划线`__`

### MD051

- Link fragments should be valid  
当链接指向的段落跟文档中的任何段落都不匹配时会出现这个错误  

```markdown
错误：
# Title

[Link](#fragment)

正确：
# Title

[Link](#title)
```

### MD052

- Reference links and images should use a label that is defined  
Markdown中的链接和图片可以在使用时提供链接目的地或图片来源，也可以在其他地方定义它，并使用标签来参考。参考格式便于保持段落文本的无杂乱性，也便于在多个地方重复使用同一个URL.

```markdown
Full: [text][label]
Collapsed: [label][]
Shortcut: [label]

Full: ![text][image]
Collapsed: ![image][]
Shortcut: ![image]

[label]: https://example.com/label
[image]: https://example.com/image
```

效果：

Full: [text][label]  
Collapsed: [label][]  
Shortcut: [label]  

Full: ![text][image]  
Collapsed: ![image][]  
Shortcut: ![image]  

[label]: https://example.com/label
[image]: https://example.com/image

### MD053

- Link and image reference definitions should be needed  
Markdown中的链接和图片可以在使用时提供链接目的地或图片来源，也可以在其他地方定义它，并使用标签来参考。参考格式便于保持段落文本不受干扰，也便于在多个地方重复使用同一个URL。

    因为链接和图像参考的定义与它们的使用地点是分开的，所以有两种情况下定义是不必要的：  
        1. 如果一个标签没有被文件中的任何链接或图像所引用，该定义就没有使用，可以被删除。  
        2. 如果一个标签在一个文件中被多次定义，则使用第一个定义，其他的可以删除。

### 参考

[markdownlint文档](https://github.com/DavidAnson/markdownlint/tree/main/doc)

[markdown常见错误速查](https://blog.csdn.net/longtype/article/details/103582331)
