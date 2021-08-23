# Markdown and Typora

## 1. Introduction to Markdown

首先，想要论述一个比较重要的点：作为一个软件工程师是否需要能够

`Markdown` 可能是我最喜欢的一个文本编辑语言了。相比`overleaf`的”沉重“感，`Markdown`像个孩子一样活泼但是又不乏他的稳重：虽然很轻量级，但是贯彻了`不花费时间在排版上`的理念。特别是对于软件开发者或者常常要写一系列文档的人员来说简直就是福音。

`Markdown`特别适用于写说明文档以及一些没有那么复杂的格式需求的作品（实际上本书的所有内容都是基于`Markdown`编写并存放在`GitBook`上的）。非常推荐你去尝试它！

## 2. Typora

### 2.1 Why choose Typora?

`Markdown`是很方便不假，但是如果你直接通过`Txt文本编辑器`去编辑`md`文件，绝对又是一件非常痛苦的事情。它看上去像这样：

![open md file as txt](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/image-20210817024221950.png)

这个时候就需要一款好用的本地`Markdown`编辑器了。我试过好几个但是最好用的果然还是`Typora`。其功能很全（包括对各种插件的支持）。

### 2.2 Install Typora

[Markdown官网](https://typora.io/)

在官网下载选择合适的操作系统，都很方便，傻瓜式安装。

> 别忘了软件安装在`development_tools`文件夹下

### 2.3 Try Typora with Markdown

#### 2.3.1 Title

在想要设置为标题的文字前面加#来表示
 一个#是一级标题，##是二级标题，以此类推。支持六级标题。

示例：

```markdown
# 这是一级标题
## 这是二级标题
### 这是三级标题
#### 这是四级标题
##### 这是五级标题
###### 这是六级标题
```

效果如下：

##### 这是五级标题

###### 这是六级标题

#### 2.3.2 Font

* 加粗（快捷键：ctrl+B）

要加粗的文字左右分别用两个*号包起来

* 斜体(快捷键：ctrl+i)

要倾斜的文字左右分别用一个*号包起来

* 删除线（快捷键：Alt+shift+5 ,笑死根本不快捷，不过用到的地方也少）

要加删除线的文字左右分别用两个~~号包起来

示例

```markdown
**example**
*example*`
***example***
~~example~~
```

效果如下：

**example**
 *example*
 ***example***
 ~~example~~

#### 2.3.3 Reference

在引用的文字前加`>`和空格即可,可以多层引用。

示例：

```markdown
>Ref
>>Ref
>>>>>>>>>>Ref
```

效果如下：

> Ref
>
> > Ref
> >
> > > > > > > > > > Ref

#### 2.3.4 Split Line

三个或者三个以上的 - 或者 * 都可以。

示例：

```markdown
---
----
***
*****
```

效果如下：
 可以看到，都是一样的分割线。

------

------

------

------

#### 2.3.5 Images

语法：

```markdown
![图片alt](图片地址 ''图片title'')
图片alt就是显示在图片下面的文字，相当于对图片内容的解释。
图片title是图片的标题，当鼠标移到图片上时显示的内容。title可加可不加
```

示例：

```markdown
![dinotocat](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/dinotocat.png)
```

效果如下：

![dinotocat](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/dinotocat.png)

> markdown格式追求的是简单、多平台统一。那么图片的存储就是一个问题，需要用`图床`，提供统一的外链，这样就不用在不同的平台去处理图片的问题了。这样我们就能做到书写一次，各处使用。
> 图床的操作可在[markdown图床](#ImageBed)查看。

#### 2.3.6 Hyper Link

语法：

```markdown
[超链接名](超链接地址 "超链接title")
title可加可不加
```

示例：

```markdown
[Google](http://google.com)
[Baidu](http://baidu.com)
```

效果如下：

[Google](http://google.com)
[Baidu](http://baidu.com)

#### 2.3.7 List

* 语法：
   无序列表用 - + * 任何一种都可以

```markdown
- 列表内容
+ 列表内容
* 列表内容

注意：- + * 跟内容之间都要有一个空格
```

* 有序列表

  语法： 数字加点

```markdown
1. 列表内容
2. 列表内容
3. 列表内容

注意：序号跟内容之间要有空格
```

* 列表嵌套:*<u>上一级和下一级之间敲三个空格或者一个tab即可</u>*

  下面是示例：

---

- 一级无序列表内容
  - 二级无序列表内容
  - 二级无序列表内容
  - 二级无序列表内容
- 一级无序列表内容
  1. 二级有序列表内容
  2. 二级有序列表内容
  3. 二级有序列表内容

1. 一级有序列表内容
   - 二级无序列表内容
   - 二级无序列表内容
   - 二级无序列表内容
2. 一级有序列表内容
   1. 二级有序列表内容
   2. 二级有序列表内容
   3. 二级有序列表内容

---

#### 2.3.8 Table

语法：

```markdown
表头|表头|表头
---|:--:|---:
内容|内容|内容
内容|内容|内容

第二行分割表头和内容。
- 有一个就行，为了对齐，多加了几个
文字默认居左
-两边加：表示文字居中
-右边加：表示文字居右

```

示例：

```ruby
Language|Popularity|Type
--|:--:|--:
Java|50%|Object Oriented
C|40%|Process Oriented
Python|70%|Script
```

效果如下：

| Language | Popularity |             Type |
| -------- | :--------: | ---------------: |
| Java     |    50%     |  Object Oriented |
| C        |    40%     | Process Oriented |
| Python   |    70%     |           Script |

#### 2.3.9 Code

语法：
 单行代码：代码之间分别用一个反引号包起来

```markdown
    `代码内容`
```

代码块：代码之间分别用三个反引号包起来，且两边的反引号单独占一行

~~~markdown
```
  代码...
  代码...
  代码...
```
~~~

示例：

单行代码

```markdown
`create database hero;`
```

代码块

~~~markdown
```
    function fun(){
         echo "这是一句非常牛逼的代码";
    }
    fun();
```
~~~

效果如下：

单行代码

```
create database hero;
```

代码块

```kotlin
function fun(){
  echo "这是一句非常牛逼的代码";
}
fun();
```

#### 2.3.10 Process Chart

~~~php
```flow
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
&```
~~~

效果如下：
```flow
st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op
&```
```

## 3 Image Bed

我
