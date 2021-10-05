## 1. Introduction to Markdown

首先，想要论述一个比较重要的点：

>  作为一个软件工程师是否需要写得一手好文档？

在我浅显的目光所及内，写`说明文档`应该是任何一个软件开发人员都无法逃避的必备技能之一。因此我希望你能够从一开始就练习书写说明文档，或者说尝试通过文字让并没有参与开发的工程师一眼看懂你的项目的功能。

而对于书写文档，`Markdown` 可能是我最喜欢的一个文本编辑语言了。相比`overleaf`的”沉重“感，`Markdown`像个孩子一样活泼但是又不乏他的稳重：虽然很轻量级，但是贯彻了`不花费时间在排版上`的理念。特别是对于软件开发者或者常常要写一系列文档的人员来说简直就是梦中情人般的存在。

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

### 3.1 Why is Image Bed necessary?

在我第一次尝试编写markdown的时候，插入图片的方式是：

* 从本地复制图片或者按住`win` +`shift`+`S`截图后粘贴。

* 然后发现将其上传到网页之后，无法显示图片

* 究其原因：我粘贴的图片的地址是`C://desktop//***.jpg`,但是我上传的时候只上传了我项目的文件夹，并没有上传这张照片。所以在网页预览的时候就会出现`Can not find file`这样的错误

* 这时候有两个解决方案：

  1. 在项目中新建一个`pic`文件夹，将所有图片复制一份到此文件夹下，然后将`markdown` 中的地址从[绝对地址改为相对地址](https://www.zhihu.com/question/49820208https://www.zhihu.com/question/49820208)。
  2. 将所有图片传到一个托管服务器上，然后通过`url`直链访问该图片。

  实际上1和2都可以，但是单纯从使用体验上来说方案2更加方便。而方案二中的`托管服务器`便是我们这里所说的图床ImageBed。

### 3.2 Install PicGo

我们需要使用PicGo插件集成到Typora，即可实现本地文章编写，支持剪贴板直接粘贴图片，完成文章编写之后使用typora自带的一键上传服务，一次性上传所有图片。

#### 3.2.1 Create Image Bed at GitHub

* 创建一个新的repo，唯一关键的点是选择项目为  `Public` ，其他可以根据自己的需求选择。

![Create New Image Bed at GitHub](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108231637531.png)

* 在GitHub中生成一个Token给PicGo

  * 首先点击个人头像，选择`setting`

  * 然后在左侧找到`development_setting`→进入后再选择`Personal access token`→进入界面后选择`generate new token` 

    > 这里token的有效期选择多久更具自己的需求来即可。

    ![Copy Token](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108231637782.png)

    复制这个token（注意这个token只可见一次，及时保存，如果一不小心手滑也没关系只要重新create一个新的token即可）


#### 3.2.2 Download PicGo

- 下载PicGO，可以进入GitHub的[PicGo官网下载](https://github.com/Molunerfinn/PicGo/releases)，Mac下载后缀名`.dmg`,Window下载后缀名`.exe`。

![PicGo Website](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108231637883.png)

* 将其安装到任何你想要安装的地方

#### 3.2.3 Set PicGO

- 点击左边图床设计，选择GitHub图床，具体配置如下
- 设定仓库名，填写：**GitHub名/库名**
- 分支，**默认填master**
- 设定Token，**刚才保存的token令牌**
- 指定存储路径，**默认填img/**
- 点击确定和设为默认图床

![Set PicGo](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108231638357.png)

- 进入PicGo设置，打开时间戳重命名

![Turn on Timestamp Rename](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108231659289.png)

### 3.3 Set Typora

- 打开Typora
- `File` → `Preference`→`Image`
- 进行图片中的设置

![Set Figure](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108231634195.png)

- 所有的设置都已经完成就点击***验证图片上传选项***

![Upload Check](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108231636355.png)

- 然后进入GitHub里看一下有图片就代表成功了

* 如果觉得写一次传一次太麻烦，可以将`插入图片`应用规则 `上传图片`，即可实现插入图片然后自动上传其至GitHub，同时替换url，非常方便。

![Advanced](https://raw.githubusercontent.com/gggdttt/ImageBeds/master/img/202108231644553.png)

