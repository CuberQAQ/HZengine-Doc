# 快速入门

{% hint style="info" %}
注意：本文档中 HZengine Script 的内容非最终版本，将随着设计完善而增加、修改甚至删除部分语法特性。不要瞎移植一通然后又得重新改了捏\~
{% endhint %}

## HZengine Script 简介

HZengine Script 是为了方便创作者开发视觉小说而生的专用脚本语言，允许用简单、一目了然的语法描述剧本、控制游戏流程。

由于 HZengine Script 高度借鉴了 Ren'Py 的语法，快速入门章节也会仿照 Ren'Py 相关内容的结构编写。后续的具体介绍会明显区分出二者的底层实现原理和使用细节。

HZengine Script 的剧本文件扩展名为 .hzs，一个项目中可以包含多个剧本文件。

## 一个简单的游戏

```renpy
*start

"仓科明日香" "嗨！教练，今天也可以去训练吗？"

"我" "那当然了！"

"我不会和她说，和美少女一起训练什么的，怎么会不可以呢"

"我" "走，我们去社团吧！"
```

这是个简单的不能再简单的游戏，不包含任何图像和音频，仅仅表达了两个人之间的对话和内心独白。但让我们就从这开始HZengine之旅吧！

第一行是一个 **label 语句** ，用于标记脚本中的某些位置，并为其命名。start 是一个特殊的标签名，正常情况下，HZengine 将从 start 标签处开始运行剧本。

剩下行都是 **say 语句** ，用于展示人物对话或旁白。say语句有两种格式，一种只包含一个字符串（由两个英文双引号包裹的文字），表示人物的内心想法或旁白等；另一种由两个字符串组成，常用于人物对话，第一个字符串表示人物名字，第二个字符串表示角色说的话。

当字符串中的文本包含英文双引号时，需要用反斜杠作为转义符，例如：

```renpy
"张团长" "嘿，同志。你有没听说过一句话，叫做\"故乡的sakura开了\"？"
```

接下来，我们将逐步学习如何添加角色定义、图片、音频和分支选项等，直至能够创作出一些基础的剧本！Let's GO\~

## 角色(character)

在上面的示例中，存在一个问题：每当你需要让角色对话时，都得输入该角色的名字。对于常出现的人物，我们可以通过使用 **character** 语句提前定义好，接下来需要该角色说话时，你可以直接使用它的短名。

```renpy
*start
character e "爱希利亚"
character m "我"

"岛上的阳光如此温和，让人忍不住出门转转..."
"咦，地上怎么有个东西..."
e "救命！"
m "起猛了，你怎么躺在地上"
e "...这还用问，当然是摔倒了啊！"
```

第二行和第三行使用 **character 语句** 定义了角色。第二行中定义一个短名为"k"，长名为"爱希利亚"的角色。

后面的三行中，我们使用定义的角色对象展示了人物对话。

## 图像(image)

图像是视觉小说的核心之一，是时候学习如何显示人物立绘和背景图了。

```renpy
*start
character k "爱希利亚"
character m "我"
scene bg village morning
"过了一会，我们来到了山腰上的一座寺庙，传闻这里曾有精灵出没"
"据山上的老人说，精灵是这座小岛的守护神，是和平与幸福的象征"
show elysia surprised
e "咦...！快看这是什么"
"我朝她手指的方向看去，那是颗散发着绿色荧光的石头"
m "这个，莫非是传说中的精灵符石？！"
show elysia smile
e "嘿嘿，让我看看！"
"说完，她伸手准备去触碰那颗神奇的石头"
"???" "慢着！"
```

这段剧本出现了两个新的语句，分别是 **scene 语句** 和 **show 语句** 。第四行的scene语句显示了一个新的背景图像；第七和十一行的show语句显示了一个人物立绘。

在HZengine中，每个图像都有一个名称，该名称包含一个 tag (图像标签，和 label 脚本标签不同)，以及一个以上的可选属性(attribute)，例如：

* 第四行的 scene 语句中，tag 标签是"bg"，属性是"village"和"morning"。按照习惯，背景图片应使用"bg"作为其 tag 标签。
* 第七行的 show 语句中， tag 标签是"elysia"，属性是"surprised"。
* 第十一行的 show 语句中， tag 标签是"elysia"，属性是"smile"。

具有相同 tag 的图像不会同时显示。当一个图像被显示出来时，它会替换掉与之具有相同 tag 的图像。

HZengine 会在游戏项目的 image 目录下搜索图像文件。HZengine 支持大部分常见图像文件，如 PNG、JPEG、WEBP 等，但请注意它们对透明像素的支持（PNG有透明通道而JPEG没有）。

图像文件的命名十分重要，HZengine 会使用去掉扩展名，并将英文字母强制转换成小写后的文件名作为图像名，例如

* "bg village morning.jpg" -> `bg village morning`
* "elysia surprised.png" -> `elysia surprised`
* "elysia smile.png" -> `elysia smile`

**hide 语句** ，可以用于隐藏已经显示出的图像，例如

```renpy
*leave
e "我先走啦！有时间再联系"
hide elysia
m "再见！"
"望着她的背影，我心想，下次见面估计是几年后了吧"
```

在 hide 语句后指定了要隐藏的图像的 tag 标签"elysia"。

## 音乐和音效

相当多的视觉小说拥有背景音或角色配音等，在 HZengine 中允许使用 **play 语句** 播放音频，例如

```renpy
play music "audio/快乐的一天.mp3"
```

play 语句的第二个词 `music` 指定了音频的播放通道，内置的通道还有 `sound` 和 `voice` (由于 Zepp OS 设备暂未支持多个音频同时播放，暂时只支持 `sound` 一个通道)。后面的字符串指定了音频文件的位置。HZengine 支持大部分常见音频格式，如 mp3, ogg, wav 等。

我们可以用 **queue 语句** 在不打断正在播放的音频的前提下，向一个通道的播放队列增加新的音频，例如

```renpy
queue music "audio/离别的滋味.mp3"
```

使用 **stop 语句** 可以停止播放音频，并将该通道的播放队列清空，例如

```renpy
stop music
```

注意：Zepp OS 目前仅支持 `sound` 一个内置通道。该通道默认不会循环播放，除非设置了通道的播放模式。

## pause 语句

使用 **pause 语句** 可以暂停 HZengine Script 的执行，直到玩家点击屏幕。

```renpy
pause
```

可以在 pause 后加一个数字，则游戏只会暂停对应的秒数

```renpy
pause 3.0
```

## 结束游戏

使用 **return 语句** 可以使游戏结束（这是暂时的解释，通过后续的学习，你会了解到它实际的工作原理），无需关心其他事，不过结束游戏之前，别忘了显示一些东西来告诉玩家游戏已经结束，比如结局名或者编号

```renpy
"恭喜你，达成了假结局A"
return
```



通过以上的内容，我们能创建出一个动态小说(kinetic novel)，即不包含分支的一种视觉小说形式。接下来的部分，我们将探索如何设置一些分支、好感度等功能。

## label 和 jump 语句

**label 语句** 用于在剧本文件中标记脚本点位置，由星号(\*)加上标签名组成，如

```renpy
*start
```

start 是一个特殊的 label，因为 HZengine 将会从 start 标签开始运行剧本。

**jump 语句** 可以在剧本执行过程中，跳转到某个 label 处，并继续执行，例如

```renpy
*middle
"哈哈，这里是middle标签哦"
jump after

*start
"你好，这里是start标签，也是整个剧本文件运行的开始位置。"
jump middle

*after
"这里是after标签"
```

在这个例子中，我们完成了一个简单的带标签跳转的小剧本，运行后，显示顺序应该是这样的

* ```
  你好，这里是start标签，也是整个剧本文件运行的开始位置。
  ```
* ```
  哈哈，这里是middle标签哦
  ```
* ```
  这里是after标签
  ```

与 Ren'Py 不同的是，当一个标签内的语句执行完后，并不会结束游戏，而是继续往下执行，直到剧本文件结尾，例如

```renpy
*start
"你好，我是start标签下的语句"

*nice
"这里是nice标签下的语句哦"
```

在这里，start 标签下的语句不含 `jump nice` 之类的语句，但剧本仍会向下执行，因此这两句话都会在游戏过程中展示。

## menu 和 branch 语句

使用 **menu 语句** 和 **branch 语句** 可以给玩家提供一个分支选项：

<pre class="language-renpy"><code class="lang-renpy">e "欢迎来到魔女食堂！你想吃些什么呢"
menu
    branch "巧克力蛋糕"
        e "啊，请稍等，蛋糕马上出炉啦"
    end branch
    branch "水果燕麦片"
        e "咦，貌似没有这种东西了"
<strong>    end branch
</strong>    branch "我要吃你"
        e "Oh my god, are you hentai?"
    end branch
end menu
</code></pre>

<pre class="language-renpy"><code class="lang-renpy">*start
character i "伊雷娜"
i "欢迎来到魔女食堂！你想吃些什么呢"
menu
<strong>@"巧克力蛋糕"
</strong>    i "啊，请稍等，蛋糕马上出炉啦"
@"水果燕麦片"
    i "咦，貌似没有这种东西了"
    jump @bye
@"我要吃你"
    i "Oh my god, are you hentai?"
    jump @bye
@bye "我都不要"
    i "这里可能没有你想要的东西呢..."
    i "欢迎下次光临~"
end menu
</code></pre>

