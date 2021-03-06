# 怎么处理Log/HLG素材


不少的胖友为了拍出更加专业、宽容度更大的画面，逐渐用HLG/Log替代了Rec.709，虽然增加了后期的成本，却可以榨干自己机器的全部性能，毕竟猪肉都30元/斤了，该省的还得省啊！

可不少胖友拍摄回HLG/Log画面后却犯了难，这丑到爆的灰片，不管套上什么Lut好像都看起来怪怪的，最后只好默默继续拍Rec.709的片子，但是宽容度又远不及Log/HLG。

![log画面](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190907121738.png)

所以！我到底该怎么办呢？

## 调色小白到底该何去何从

首先要告诉大家的是，现在绝大数的Lut是针对Rec.709标准制作的，所以直接把这样的Lut套在Log/HLG的素材上，无法调出好看的颜色。

如果很不巧，你对调色毫无了解，那我推荐你先从自己相机的官方网站上下载**还原Lut**，它能让你拍摄的灰片瞬间变得光彩夺目，暗淡的生活仿佛在这一刻重获了生机！

![还原](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190907122612.png)

不过下载的时候一定要注意还原Lut的使用对象，比如你拍摄的S-log3的素材，你就需要在索尼官网上找到对应机型的S-log3还原Rec.709的Lut，如果用S-log2的Lut，想必结果也不会让你满意。

![索尼Lut](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190907122430.png)

当你把颜色成功还原成Rec.709后，你只需要套上你心意的Lut，并做一些简单的曲线和输出强度的调整，就能达到不错的效果


## 我有接触过达芬奇，该如何操作

首先恭喜你超过了66.66%的读者，当然，如果你的相机没有提供官方还原Lut，也可以按这个方法操作，这里我选择使用达芬奇来演示，Pr/Fcpx的操作方法也是大同小异啦。

首先我们还原影片的对比度，这一步主要是确定影片的影调，我们可以通过点击画面，选取影片中最黑，最亮以及中间调的部分。

![曲线](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190907125601.png)


在曲线模块中我们可以看到刚才在画面上点击的三个点，我们可以以这三个键为基准，确定画面层次关系。

![层次](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190907130356.png)

这里需要告诉大家的是，鼠标左键可以添加控制点，右键点击控制点可以取消，同时不要把画面的对比度拉的太多，保证波形图上下都还有空间。

然后我们进入「RGB混合器」把红色、绿色、蓝色输出的对应颜色都拉满

![颜色](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190907133926.png)

此时我们看到影片的颜色已经恢复，如果此时颜色过于浓烈，可以回到「色轮」调整影片的饱和度，当然你也可以调整白平衡等参数。

这步完成后，我们使用快捷键「Alt+S」新建一个调整节点，对影片进行锐化和降噪处理，至此我们大致完成了一级调色。

![第二步](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190907134439.png)

如果有需要，我们可以用不同的平行节点，结合限定器，色相曲线等工具去调整画面中各部分的颜色。

完成以上步骤后，你就可以套用网上琳琅满目的Lut完成二级调色，并把符合自己调性的Lut收藏下来。

![完成](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190907135707.png)

## 特殊的HLG

严格意义来说，HLG格式的视频并不是灰片,它是由 BBC 和 NHK 共同开发的高动态范围标准，所以你在索尼官网上找不到HLG还原Rec.709的Lut，因为他本来就是正常的呀。


上面的话术可能不太像人话，我们把它换成人话来说就是，如果你有支持HDR的显示器可以正常显示出HLG的视频，你看起来画面发灰还不是因为你穷买不起HDR显示器。（雾）

这里需要插一句题外话，我们在拍摄HLG视频的时候，一定要选择2020色域。

但是我们要知道的是，绝大多数的观众都是使用709色域的显示器，我们只做影片也要考虑观众的体验，所以所以大家还是要把它转换成709色域的视频再选择发布，新版本的Pr已经支持2020宽色域，FCPX官方也发布了具体的[教程](https://support.apple.com/zh-cn/HT208229)


![FCPX](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190907140149.png)


## 小结

我们一般所说的调色其实大致分为两步：校色与风格化。一级调色主要注重的是矫正，我们需要在这里大致确定影片的影调和色彩关系，这一步是调色中较为重要，也容易被大家忽略的一步；做好了一级调色之后，我们可以选择Lut这种快捷的工作流，对画面进行风格化，而收藏Lut的意义决不在于多，它只是你个人风格的体现。

最后要告诉大家的是，千万不要把Lut这种工作流神话了，拍摄时多注重置景和灯光，带来的收益远比套Lut来的好。好的作品需要精心打磨，希望各位都能做出自己满意的作品