---
title: Markdown书写规范
date: 2025-07-11 23:20:00 +0800
categories: [语法规则]
tags: [Markdown]
description: 参照Markdown相关的书写规范，通过编写统一格式的纯文本文件，得到一篇源文件有较高可读性以及编写性、可维护、语法简单且易于记忆的格式化文档输出。
---

## Markdown规范

- [Markdown规范](#markdown规范)
- [一、文档布局](#一文档布局)
- [二、字符行](#二字符行)
- [三、行尾空格](#三行尾空格)
- [四、标题](#四标题)
- [五、列表](#五列表)
- [六、代码](#六代码)
- [七、链接](#七链接)
- [八、图片](#八图片)
- [九、表格](#九表格)
- [十、优先使用列表而非表格](#十优先使用列表而非表格)
- [十一、多用Markdown，少用Html](#十一多用markdown少用html)

## 一、文档布局

一般情况下，文档采用以下的这种布局格式

```markdown
# 文档标题

简短介绍

[TOC]

## 章节标题

内容

## 参考

* https://link-to-more-info
```

1. `# 文档标题`：第一个标题最好是一级标题，而且最好和文件名相同或者类似。第一个标题通常作用于整个文件。

    文档标题采用以下的格式:

    ```markdown
    Markdown 编写规范
    ==========================
    ```

2. `# 简短介绍`：用1-3局话对文档的主题做一个概述。把自己假设成一个新手，看到介绍后就了解了这篇文档介绍了什么？为什么要去这么做？  

3. `# [TOC]`：如果你使用了目录，可以将目录放在简介的后面。

4. `# 章节标题`：剩下的章节标题级别从二级开始。  

5. `## 参考`：将文档参考的相关链接放在底部，方便找到更详细的内容，或者提供当前页面没有但是其他人需要了解的内容。  

## 二、字符行

尽可能遵从Markdown语法中关于字符行的限制。过长的链接以及表格通常会破坏这些限制。因此通常在长链接前后加入换行符以增加可读性, 让它可以符合字符长度的限制。

- 中英文混合时，应该采用以下规则：

  - 英文和数字使用半角字符
  - 中文文字之间不加空格
  - 中文文字与英文、阿拉伯数字及 @ # $ % ^ & * . ( ) 等符号之间加空格
  - 中文标点之间不加空格
  - 中文标点与前后字符之间不加空格
  - 如果括号内有中文，则使用中文括号
  - 如果括号中的内容全部都是英文，则使用半角英文括号
  - 当半角符号/表示[或者]之意时，与前后的字符之间均不加空格
  - 更多例子参考[中文文案排版指北](https://github.com/sparanoid/chinese-copywriting-guidelines)
  
- 中文符号应该采用以下规则：
  - 用直角引号（「」）代替双引号（“”），输入方法参考[如何输入直角引号](https://www.zhihu.com/question/19755746)
  - 省略号使用「……」，而「。。。」仅用于表示停顿

- 表达方式应当遵循《The Element Style》:
  - 使段落成为文章的单元：一个段落只表达一个主题
  - 通常在每一段落开始要点题，在段落结尾要扣题
  - 使用主动语态
  - 陈述句中使用肯定说法
  - 删除不必要的词
  - 避免连续使用松散的句子
  - 使用相同的结构表达并列的意思
  - 将相关的词放在一起
  - 将强调的词放在句末

## 三、行尾空格

在行尾使用反斜线去代替行尾空格进行换行操作。

例如：

```markdown
这里需要一个换行\
下一行
```

效果：

>这里需要一个换行\
>下一行

同时，对于两个段落，或者两部分的内容，通常使用一个空白行来进行隔开。

章节标题和内容间必须有一个空行。

## 四、标题

### 采用ATX格式的标题

```markdown
## 二级标题
```

使用 Setext 格式的标题，在标题行下添加 `=` 或者 `-` ，这种格式很难维护并且不能很好地去适应标题的语法，同时也很难区分一级标题和二级标题，所以最好尽可能避免地去使用 `=` 或者 `-` 作为标题格式。

### 在标题的周围都加入空行

最好在 `#` 和新行的前面和后面都加入空白行，如果文本太挤的话会使源文件的可读性降低。

```markdown
例1：
...上一部分内容

# 标题

下一部分内容...


例2：
...上一部分内容

# 标题
下一部分内容...
```

两个例子的显示效果是一样的，但可以看到，例1的源文件可读性显然高于例2。

## 五、列表

### 对长列表使用偷懒的有序数字符号

创建有序列表，要在每个列表项前添加数字并紧跟一个英文句点，空一个空格输入标题的内容。对于这种有序列表，Markdown有一种偷懒的方式，即数字不必按数学顺序排列，但是列表序号应当以数字 1 开始。

对于可能会修改的长列表，特别是嵌套的长列表，更推荐使用偷懒的有序数字符号

例：

```markdown
1. Foo.
1. Bar.
    1. Foofoo.
    1. Barbar.
1. Baz.
```

效果：

>1. Foo.
>1. Bar.
>    1. Foofoo.
>    1. Barbar.
>1. Baz.

但是在列表较短并且不太可能改变它的情况下，更推荐使用有序完整的数字符号来增加可读性。

```markdown
1. Foo.
2. Bar.
3. Baz.
```

### 嵌套列表间距

在嵌套列表中，使用4个空格对有序列表和无序列表进行缩进

```markdown
1.  在列表符号后有两个空格
    
2.  再次空两个格

*   星号后有三个空格
    下一行的文本前有4个空格缩进
        1.  在列表符号后有两个空格  
            对于嵌套列表，下一行的文本前有8个空格缩进
        2.  可以看到嵌套的结构非常清晰
*   星号后有三个空格
```

效果：

>1. 在列表符号后有两个空格
>2. 再次空两个格
>
>- 星号后有三个空格  
>   下一行的文本前有4个空格缩进  
>        1. 在列表符号后有两个空格  
>           对于嵌套列表，下一行的文本前有8个空格缩进  
>        2. 可以看到嵌套的结构非常清晰
>- 星号后有三个空格

当没有嵌套列表时，使用4个空格对换行文本的布局进行统一

```markdown
*   Foo
    换行.

1.  两个空格
    采用4个空格缩进
2.  再次采用两个空格
```

效果：

>- Foo
>    换行.
>
>1. 两个空格  
>    采用4个空格缩进
>2. 再次采用两个空格缩进

但是，在短列表，只有简单一行且无嵌套布局的情况下，一个空格的间隔就能满足所有的列表格式。

例：

```markdown
* Foo
* Bar
* Baz

1. Foo
2. Bar
```

## 六、代码

### 单行代码

\`反引号\`内标志的内容，Markdown会进行渲染。主要用于短代码和字段名称：

```markdown
You'll want to run `really_cool_script.sh arg`.

Pay attention to the `foo_bar_whammy` field in that table.
```

效果：

>You'll want to run `really_cool_script.sh arg`.
>
>Pay attention to the `foo_bar_whammy` field in that table.

使用反引号标记抽象意义的文件名, 而不是具体的文件:

```text
Be sure to update your `README.md`!
```

效果：

>Be sure to update your `README.md`!

### 代码块

对于超过一行的代码，使用代码块将它括起来：

```python
def Foo(self, bar):
    self.bar = bar
```

#### 声明语言

最好是声明语言类型，这样语法高亮显示，并且方便编辑器对语言猜测。

#### 有时候使用缩进来代表代码块会更加简洁

四个空格的缩进会被解释成声明一个代码块，采用这种方式会看起来更清晰且可读性高，但是采用这种方式的话就无法声明语言类型。推荐在编写许多短代码片段的时候使用这种方式。

```markdown
You'll need to run:

    bazel run :thing -- --foo

And then:

    bazel run :another_thing -- --bar

And again:

    bazel run :yet_again -- --baz
```

效果：

> You'll need to run:
>
> ```text
> bazel run :thing -- --foo
> ```
>
> And then:
>
> ```text
> bazel run :another_thing -- --bar
> ```
>
> And again:
>
> ```text
> bazel run :yet_again -- --baz
> ```

#### 转义换行符

因为大部分的代码片段都会被粘贴到命令行终端中，所以最好避免使用转义换行符。

在行的末尾加上一个反斜杠会进行换行操作.

````markdown
```shell
bazel run :target -- --flag --foo=longlonglonglonglongvalue \
--bar=anotherlonglonglonglonglonglonglonglonglonglongvalue
```
````

#### 列表中嵌套程序块

如果需要在列表中嵌套一个程序块，缩进程序块可以确保列表不被打断：

````markdown
*   Bullet.

    ```c++
    int foo;
    ```

*   Next bullet.
````

效果：

>- Bullet.
>
>    ```c++
>    int foo;
>    ```
>
>- Next bullet.

同样你也可以通过4个空格来创建一个代码块，只需要在列表所今后添加四个空格即可：

```markdown
*   Bullet.

        int foo;

*   Next bullet.
```

效果：

> - Bullet.
>
> ```text
> int foo;
> ```
>
> - Next bullet.

## 七、链接

太长的链接会使markdown文件可读性变差，同时也破坏了每行80个字符的限制。所以尽可能地去缩短链接的长度。

### 使用含有丰富信息的markdown链接标题

Markdown的链接语法允许你像Html一样，设置一个带有超链接的标题。

但是在读者快速浏览你的文档时，"link" 或者"here" 这种没有信息含量的标题，并不会给读者提供任何信息且浪费空间

例：

```markdown
See the syntax guide for more info: [link](syntax_guide.md).
Or, check out the style guide [here](style_guide.md).
DO NOT DO THIS.
```

效果：

>See the syntax guide for more info: [link](syntax_guide.md).  
>Or, check out the style guide [here](style_guide.md).  
>DO NOT DO THIS.

一般人也看不懂你这个是什么链接。相反，要在链接的括号内包含这个链接的描述

```markdown
See the [syntax guide](syntax_guide.md) for more info.
Or, check out the [style guide](style_guide.md).
```

效果：

>See the [syntax guide](syntax_guide.md) for more info.  
>Or, check out the [style guide](style_guide.md).

## 八、图片

少用图片，并且最好采用截图的方式，多用文本的方式来展现内容，最好只在你觉得只有图片能更好表达你想展示的意思时才采用图片

## 九、表格

表格的写法应该参考 [GFM](https://docs.github.com/en/get-started/writing-on-github)，如下所示

```markdown
First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell

| Left-Aligned  | Center Aligned  | Right Aligned |
| :------------ |:---------------:| -----:|
| col 3 is      | some wordy text | $1600 |
| col 2 is      | centered        |   $12 |
| zebra stripes | are neat        |    $1 |
```

效果：
>First Header  | Second Header
>------------- | -------------
>Content Cell  | Content Cell
>Content Cell  | Content Cell
>
>| Left-Aligned  | Center Aligned  | Right Aligned |
>| :------------ |:---------------:| -----:|
>| col 3 is      | some wordy text | $1600 |
>| col 2 is      | centered        |   $12 |
>| zebra stripes | are neat        |    $1 |

## 十、优先使用列表而非表格

最好只在你的文档里使用小的表格，大且复杂的表格会在源文件里非常难阅读，同时也会在 后期维护的时候十分困难

```markdown
Fruit | Attribute | Notes
--- | --- | --- 
Apple | [Juicy](https://example.com/SomeReallyReallyReallyReallyReallyReallyReallyReallyLongQuery), Firm, Sweet | Apples keep doctors away.
Banana | [Convenient](https://example.com/SomeDifferentReallyReallyReallyReallyReallyReallyReallyReallyLongQuery), Soft, Sweet | Contrary to popular belief, most apes prefer mangoes.

DO NOT DO THIS
```

效果：

>Fruit | Attribute | Notes  
>--- | --- | ---
>Apple | [Juicy](https://example.com/SomeReallyReallyReallyReallyReallyReallyReallyReallyLongQuery), Firm, Sweet | Apples keep doctors away.
>Banana | [Convenient](https://example.com/SomeDifferentReallyReallyReallyReallyReallyReallyReallyReallyLongQuery), Soft, Sweet | Contrary to popular belief, most apes prefer mangoes.

列表和紧凑一点的子标题也可以展示同样的信息，而且更容易进行编辑修改。

```markdown
## Fruits

### Apple

* [Juicy](https://SomeReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyLongURL)
* Firm
* Sweet

Apples keep doctors away.

### Banana

* [Convenient](https://example.com/SomeDifferentReallyReallyReallyReallyReallyReallyReallyReallyLongQuery)
* Soft
* Sweet

Contrary to popular belief, most apes prefer mangoes.
```

效果：

>## Fruits<!-- omit in toc -->
>
>### Apple
>
>- [Juicy](https://SomeReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyReallyLongURL)
>- Firm
>- Sweet
>
>Apples keep doctors away.
>
>### Banana
>
>- [Convenient](https://example.com/>SomeDifferentReallyReallyReallyReallyReallyReallyReallyReallyLongQuery)
>- Soft
>- Sweet
>
>Contrary to popular belief, most apes prefer mangoes.

有时候也会需要一个小的表格

```markdown
Transport | Favored by | Advantages
--- | --- | ---
Swallow | Coconuts | Otherwise unladen
Bicycle | Miss Gulch | Weatherproof
X-34 landspeeder | Whiny farmboys | Cheap since the X-38 came out
```

效果：

>Transport | Favored by | Advantages
>--- | --- | ---
>Swallow | Coconuts | Otherwise unladen
>Bicycle | Miss Gulch | Weatherproof
>X-34 landspeeder | Whiny farmboys | Cheap since the X-38 came out

## 十一、多用Markdown，少用Html

尽可能地多使用markwodn的语法，减少Html语法的入侵。如果Markdown不能实现你想要的, 重新考虑这部分内容你是否需要它。Markwodn几乎能完成除了大型表格之外的所有需求。

任何一点的Html以及Javascript 元素，都会减少文档的可阅读性和可移植性。因为你不确定你所使用的Markwodn工具会将这些元素以源文件的方式显示还是以渲染后的效果进行显示。
