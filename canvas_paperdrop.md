# canvas制作矩形沿中心y轴翻转动画

------

在学习canvas一些别人写的动画过程中，遇见一个效果挺有趣的，**五彩纸屑飘落动画特效**。不由得让我思考这是怎么做出来的，图片稍后再贴。看了源码之后，关于它怎么做出来翻转的效果，看起来还是有点难以理解的。所以决定自己先尝试写一个翻转的效果，应该对理解带来一点好处。

以下地址是分别是我写的翻转的效果和源代码：

### [自己写的翻转效果](https://github.com/Boice123/canvas_demo/tree/master/canvas-paperdrop-demo)
### [五彩纸屑飘落动画特效源码](https://github.com/Boice123/canvas_demo/tree/master/canvas-paperdrop)

> 请保留此份 Cmd Markdown 的欢迎稿兼使用说明，如需撰写新稿件，点击顶部工具栏右侧的 <i class="icon-file"></i> **新文稿** 或者使用快捷键 `Ctrl+Alt+N`。

------

## 什么是 Markdown

其实明白了实现翻转的原理，问题就很简单了。首先先做好一些初始化容器、矩形的准备工作。
1.初始化屏幕宽高的最外层canvas容器。
2.根据设定的要绘画的矩形数量，遍历创建对应的小canvas（决定了矩形的宽高、大小、颜色、初始位置），放置到新创建的矩形canvas数组sprites。
3.定义一个周期函数,设定一个动画从开始到结束的持续时间duration，得出动画进行到的周期比例：progress = （动画此时时间戳-动画开始时间戳）- duration。progress必定是在0到1之间的。这个参数跟绘画矩形的翻转效果有决定性效果。

翻转之前
![avatar](https://github.com/Boice123/blog/blob/master/static/img/canvas/%E7%BF%BB%E8%BD%AC%E5%89%8D.png)
翻转之后
![avatar](https://github.com/Boice123/blog/blob/master/static/img/canvas/%20%E5%88%86%E6%9E%90.png)

翻转，不仅矩形的宽度有变化，就连绘画点的位移也变了。如图，黑色框住的区域，就是矩形canvas的范围。首先，画笔需要先移动到下一帧矩形canvas的中心y轴位置。然后调用*drawImage(canvas,x,y,width,height)* API在大画布上绘制这个小矩形。参数canvas就是这个矩形canvas本身，y就是0,height就是矩形canvas的高，至于x和width参数就复杂点了。

宽度从最大值到最小值，再到最大值的变化，让我想起之前学过的cos函数，根据x值的变化，y值会不断的在极小值和极大值之间变化，所以可以用到这个知识点。因此，所以矩形的宽度为**矩形本来宽度 * Math.abs(Math.cos(Math.PI * 2 * progress))**。至于x参数，就为**-矩形本来宽度 * Math.abs(Math.cos(Math.PI * 2 * progress)) / 2**

以上就是我canvas制作矩形沿中心y轴翻转动画的见解，[详细的代码点这里](https://github.com/Boice123/canvas_demo/tree/master/canvas-paperdrop-demo)