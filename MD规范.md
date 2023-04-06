# 基础语法

## 标题语法

> 要点：在行首使用N个 `#` 后加空格再写标题，即可构成N级标题

| Markdown语法                       | HTML              |
| ---------------------------------- | ----------------- |
| # 一级标题(或者在下一行使用多个=)  | <h1>一级标题</h1> |
| ## 二级标题(或者在下一行使用多个-) | <h2>二级标题</h2> |
| ### 三级标题                       | <h3>三级标题</h3> |
| #### 四级标题                      | <h4>四级标题</h4> |
| ##### 五级标题                     | <h5>五级标题</h5> |
| ###### 六级标题                    | <h6>六级标题</h6> |

## 段落语法

> 要点：段落结束后使用空白行进行分段，段内容不要使用空格或制表符进行缩进

| Markdown语法                                                 | HTML                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| I really like using Markdown.<br><br>I think I'll use it to format all of my documents from now on. | <p>I really like using Markdown.</p><br /><br><p>I think I'll use it to format all of my documents from now on.<p> |

第一段

第二段

## 换行语法

> 要点：行末输入两个或多个空格再按下回车，即可创建 `<br>` 换行(编辑器好像在回车时自动进行了换行，无需空格)，也可使用HTML方式的 `<br>` 来换行

| Markdown语法                                                 | HTML                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `This is the first line.  <br/>And this is the second line.` | `<p>This is the first line.<br>And this is the second line.</p>` |

试试多空格换行  

## 强调语法

### 粗体：

> 要点：在目标内容的前后分别添加 `**` 或 `__` 即可

| Markdown语法                 | HTML                                      |
| :--------------------------- | :---------------------------------------- |
| `I just love **bold text**.` | `I just love <strong>bold text</strong>.` |
| `I just love __bold text__.` | `I just love <strong>bold text</strong>.` |

**这是粗体**

### 斜体

> 要点：在目标内容的前后分别添加 `*` 或 `_` 即可

| Markdown语法                           | HTML                                          |
| :------------------------------------- | :-------------------------------------------- |
| `Italicized text is the *cat's meow*.` | `Italicized text is the <em>cat's meow</em>.` |
| `Italicized text is the _cat's meow_.` | `Italicized text is the <em>cat's meow</em>.` |

*这是斜体*

### 粗体和斜体

> 要点：在目标内容的前后分别添加 `***` 或 `___` 即可

| Markdown语法                           | HTML                                                       |
| :------------------------------------- | :--------------------------------------------------------- |
| `This text is ***really important***.` | `This text is <strong><em>really important</em></strong>.` |
| `This text is ___really important___.` | `This text is <strong><em>really important</em></strong>.` |
| `This text is __*really important*__.` | `This text is <strong><em>really important</em></strong>.` |
| `This text is **_really important_**.` | `This text is <strong><em>really important</em></strong>.` |

***这是粗斜体***

**注意：为兼容性尽量避免 `___` **

## 引用语法

> 要点：在行首使用一个 `>` 后加空格即可创建引用，可嵌套使用，在要嵌套的行首使用多一个的 `>` 后加空格即可

| Markdown语法                                                 |
| :----------------------------------------------------------- |
| > Dorothy followed her through many of the beautiful rooms in her castle. |
| > Dorothy followed her through many of the beautiful rooms in her castle.<br>><br>> > The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood. |

> 外层引用
> 
> > 内层引用

## 列表语法

### 有序列表

> 要点：在每个列表项前使用数字后加 `.`再加空格即可创建，数字可以不按实际顺序写，但应从1开始

| Markdown语法                                                 | HTML                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `1. First item2. Second item3. Third item4. Fourth item`     | `<ol><li>First item</li><li>Second item</li><li>Third item</li><li>Fourth item</li></ol>` |
| `1. First item1. Second item1. Third item1. Fourth item`     | `<ol><li>First item</li><li>Second item</li><li>Third item</li><li>Fourth item</li></ol>` |
| `1. First item8. Second item3. Third item5. Fourth item`     | `<ol><li>First item</li><li>Second item</li><li>Third item</li><li>Fourth item</li></ol>` |
| `1. First item2. Second item3. Third item  1. Indented item  2. Indented item4. Fourth item` | `<ol><li>First item</li><li>Second item</li><li>Third item<ol><li>Indented item</li><li>Indented item</li></ol></li><li>Fourth item</li></ol>` |

1. 有序的1
6. 有序的6
5. 有序的5

### 无序列表

> 要点：在每个列表项前使用 `-`/`*`/`+` 再加空格即可创建，使用缩进即可嵌套

| Markdown语法                                                 | HTML                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `- First item- Second item- Third item- Fourth item`         | `<ul><li>First item</li><li>Second item</li><li>Third item</li><li>Fourth item</li></ul>` |
| `* First item* Second item* Third item* Fourth item`         | `<ul><li>First item</li><li>Second item</li><li>Third item</li><li>Fourth item</li></ul>` |
| `+ First item+ Second item+ Third item+ Fourth item`         | `<ul><li>First item</li><li>Second item</li><li>Third item</li><li>Fourth item</li></ul>` |
| `- First item- Second item- Third item  - Indented item  - Indented item- Fourth item` | `<ul><li>First item</li><li>Second item</li><li>Third item<ul><li>Indented item</li><li>Indented item</li></ul></li><li>Fourth item</li></ul>` |

- 无序1
- 无序2
  - 无序子1
  - 无序子2

**注意：别混用符号，在列表中使用其他元素需要给该元素添加四空格或制表符的缩进**

## 代码语法

> 要点：使用(\`)引起来即可，可用四个空格或一个制表符加在目标内容的开头构成代码块，也可在目标内容的开始和结束处(需要另起一行)加上 ` ``` ` 或者 `~~~` 构建代码块，并且在开始处的 `~~~` 后添加指定语言即可突出显示。

| Markdown                                  | HTML                                             |
| ----------------------------------------- | ------------------------------------------------ |
| At the command prompt, type `nano`.       | `At the command prompt, type <code>nano</code>.` |
| ```python<br>print("Hello Python")<br>``` | -                                                |

这是句中的 `代码` 。

	这是
	制表符控制的
	代码块

## 分隔线语法

> 要点：在单独一行上使用三个或多个 `*`/`-`/`_` 且不可包含其他内容，为了兼容最好在分隔线的前后均添加空白行

| Markdown         |
| ---------------- |
| `*************`  |
| `--------------` |
| `______________` |

----------------------------------

## 链接语法

### 链接基本语法

> 要点：将链接用 `()` 包裹起来，并在其前使用 `[]` 包裹想显示的内容，链接title(鼠标在内容上显示出的内容)可加在()中链接加空格的后边并以 `""` 引起来即可，即 `[显示](http://地址 "这是说明")` 
>
> 另：用 `<>` 将链接或 Email 包裹起来也可以达到可点击地址的效果，其显示内容即本身

|                                        |                                                         |
| -------------------------------------- | ------------------------------------------------------- |
| 这是[链接](http://地址 "虚构的地址哦") | 这是<a href="http://地址" title="虚构的地址哦">链接</a> |

这是 [标准链接](https://标准 "标准哦") 和 <https://尖括号链接> 

**注意：想强调时将整个链接语法前后添加 `*` 即可，想表示成代码则应在 `[]` 中为内容加上 ` **

### 引用型链接语法

> 要点：关联两处内容，一处为与文本保持内联的部分，如引用描述，另一处为存在于其他位置的被引用部分
>
> > 在引用处：使用两组 `[]` ，第一组内为显示的文本，第二组为要指向的内容的标签
> >
> > 在被引用处：使用 `[]` 包裹标签，并在其后紧跟冒号和空格，用 `<>` 包裹链接，可用 `"`/`'`/`()` 引起标题置于尖括号链接后的空格之后

**注意：链接中的空格尽量用 `%20` 表示，即转码后的链接**

这是 [引用][引用1]

[引用1]: <http://exam%20ple.link> "引用的标题"

## 图片语法

> 要点：语法与链接类似，但需在 `[]` 前使用 `!` 

| Markdown                    | HTML                                            |
| --------------------------- | ----------------------------------------------- |
| ![图片](image.png "图片哦") | <img src="图片" alt="image.png" title="图片哦"> |

**注意：想使图片带链接，只需将图片语法放在链接语法的 `[]` 中即可**

[![图片](image.png "图片哦")](http://图片链接地址 "链接地址哦")

## 字符转义语法

> 要点：使用反斜杠 `\` 后加目标字符即可

**注意：HTML文件中的 `<` 和 `&` 需要使用 `&lt;` 和 `&amp;` ，一般Markdown的行内HTML可以自动转义**

这是著作权符号：&copy; ，用 `&copy;` 来写

---------------------------------

其他的用法可直接使用HTML中的标准来编写，但某些Markdown应用程序只支持部分标签。

尽量不要使用制表符或空格对 HTML 标签做缩进，否则将影响格式。

在 HTML 块级标签内不能使用 Markdown 语法。

--------------------------------------

# 扩展语法

***不一定能用***

## 表格语法

> 要点：
> 1. 行中的每列用 `|` 分隔，在下一行用三个或多个 `-` (同样要用 `|` 分隔)即可将上方一行作为表头，每行的 `|` 不必对齐。
> 1. 表中内容对齐可用 `:` ，左对齐在内容左边使用，右对齐同理，居中对齐则需在内容两边同时使用

**注意：表内容不会格式化HTML标签，在表内容中使用代码语法可取消格式化**

**注意：使用HTML字符代码 `&#124;` 即可在表中显示 `|` 字符**

## 脚注语法

> 要点：
>
> 使用脚注处：使用 `[^` 和 `]` 包裹编号(可以是数字或单词)
>
> 脚注处：在脚注内容前使用对应的标识( `[^` 和 `]` 包裹编号)后紧跟冒号及空格
>

要用脚注啦[^1]

[^1]: 脚注内容

## [标题编号](#标题编号1)

> 要点：使用 `{#` 和 `}` 包裹ID，要使其带有链接可使用带 `#` 的链接

| Markdown                      | HTML                                     |
| ----------------------------- | ---------------------------------------- |
| `[Heading IDs](#heading-ids)` | `<a href="#heading-ids">Heading IDs</a>` |

从[这](#标题编号1)可以到标题编号哦

## 定义列表

> 要点：上一行的描述之后在下一行内容的行首添加冒号和空格

_注：Typora貌似不适用_

## 删除线

> 要点：在目标内容的前后分别添加 `~~` 即可

~~我选她！~~ 我全都要！！

## 任务列表(勾选框)

> 要点：在目标内容前添加  `- [ ]` (未选中的)，选中的应修改为 `- [x]` 。

- [x] 已经做了的
- [ ] 没做的

## 表情

> 一般使用复制粘贴就行，Markdown会自动转换，手动则需要在表情名称前后添加 `:` 和 `:` 。

:joy: