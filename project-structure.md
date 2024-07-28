# 项目文件结构
在上一章节，我们用 hzengine-cli 创建了一个模板项目，现在是时候介绍项目文件夹的结构了。

用 VS Code 打开我们刚才创建的项目文件夹，可以看到如下图所示的目录结构

（此处有一张项目文件夹结构的图片）

其中，assets文件夹用于存放ZeppOS小程序的资源文件，assets/raw/icon.png是小程序图标。

而assets/raw/project是HZ-Engine的游戏资源根文件夹（以下简称project文件夹）。
这个文件夹里面保存了游戏脚本、立绘、cg 等游戏的核心资源，可以说，这里几乎是HZ-Engine项目的基地。

让我们看看project文件夹包含了哪些内容

hz_package.json 游戏的描述文件，里面保存了游戏名字、制作者信息和版本等。

image 图片资源文件夹，保存了角色立绘、CG、背景图等图片。

audio 音频资源文件夹，保存人物语音、背景音乐、音效等音频资源。

gui 图形界面资源文件夹，保存按钮、文本框背景等可交互元素的图片资源

script 游戏脚本文件夹，保存以.hzs结尾的HZ-Engine Script文件，用于描述剧本和演出。

animation 动画描述文件，保存用于转场、变换的动画描述文件



一个HZ-Engine项目同时也是ZeppOS小程序项目，hzengine-core以npm包的形式应用到项目中。你可以直接使用官方的zeus-cli、模拟器等构建和调试HZ-Engine游戏项目。
