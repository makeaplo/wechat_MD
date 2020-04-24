

有的小朋友看到AE两个字就莫名感到一阵恐惧，听到表达式更是直接被劝退。但其实AE和表达式真没那么难，下面我将通过几个场景和案例和大家分享几个常用的表达式，你只要跟着我左手右手一个慢动作，就能很快的学会啦，它一定能大大提升你的工作效率。

![害怕](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190921131722.png)

## 一闪一闪亮晶晶

天上闪烁的星星往往会带给我们许多值得珍藏的回忆，而当我们想在AE中创建这样的夜空场景，星星往往又是最麻烦的那个讨厌鬼。如果你不会表达式只能一帧帧的调整，费时费力效果还不好，这时候当然要请出表达式啦。

![星星](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/1.jpg)


### 我最摇摆

我们都知道，星星并不会老老实实的在原地，它会在一定区域内运动，所以应该是给星星的**位置属性**设置动画，让它自由摇摆。

我们快速创建一个星星图形，然后打开他的位置属性（快捷键P，position的缩写）。这时，**按住**键盘上的「Alt」键，同时点击位置属性前的小码表，就可以激活表达式。

这里我们选择的是wiggle表达式，wiggle的中文是摇摆的意思，所以我们可以在代码框中输入：

wiggle(5,10)

![表达式](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/2.jpg)


这个表达式比较简单，它的基本形式为：wiggle(抖动频率,抖动幅度)，简单点说表达式里第一个数值控制摇摆的速度，第二个数值控制摇摆的范围。

在这个例子中，如果你想让你的星星移动的更快，只需要调整这个表达式前面的数值；如果你想让它移动的范围更大，调整后面的数值就可以啦。

怎么样？是不是简单的操作就能获得一个摇摆的星星呀！

![wiggle](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/1.gif)



### 我最闪亮

其实星星只摇摆还是不够的，它除了会摇摆，还会闪烁，就像儿歌里唱的“一闪一闪亮晶晶”一样，所以我们还要为星星制作闪烁的效果。

![闪烁](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/2.gif)

首先我们确定为星星的**不透明度属性**进行操作，但是一点点打帧实在太麻烦，于是我们又需要借助表达式的功能了。

有聪明的小朋友已经想到可以用上面的wiggle表达式来实现这个功能，但是wiggle表达式的规律不可控，而我更希望我的星星能像呼吸一样有规律的闪烁，所以我们需要借助另一个表达式来实现它。

在实际动手之前呢，我们可能需要先复习一点中学数学知识，我们知道正、余弦（sin、cos）函数是随着周期变换的，好像能和星星的闪烁规律相匹配。

![函数](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/函数.png)

同时，正、余弦函数的取值范围在[-1,1]，如果我们把这个函数扩大100倍，即将其取值范围变成[-100,100]，似乎刚好又能服务于不透明度属性。

由于不透明度属性不存在负值，所以上述函数的取值范围变成了[0，100]，且仍然保留了周期性的变化规律。

于是，我们给不透明度写上如下表达式：

Math.sin(time*6)*100

![表达式](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190921134725.png)

那有的小朋友就要问了，上面的表达式中的time又是什么呢？这里我为大家一一讲解这个表达式的含义。

首先我们把上述表达式拆成 Math.sin(**time\*6**)*100 的形式

Math.sin()是数学中的正弦函数表达式，其中Math表示**AE内置的表达式**中的数学表达式组，sin表示正弦函数，两者用「.」连接，表示后者属于前者，即可以理解为**Math表达式组**中的sin表达式。

这种从属关系是AE表达式中的基本逻辑，我们再举个例子，比如「transform.position.speed」表示的就是**变换**中的**位置属性**的**速度**，这个表达式可以与模糊效果配合，制作运动模糊。

![运动模糊](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190921140432.png)

time是时间表达式，即当前时间轴的秒数，我们把这个变量填入正弦表达式中，就可以让正弦函数的取值波动起来，但由于Math.sin(time)的变化率太慢，所以我们给time的数值乘6，以加快正弦函数的变换。

最后我们把正弦函数的取值放大100倍，同时由于不透明度属性的限制，使得最终不透明度值在[0,100]中有规律的变换。



## 伪3D效果

有的时候呢，我们希望有一些简单的3D动画（如各类节目的转场动画）来丰富画面，这类动画通常很短但有不错的效果，按普通思路我们需要使用C4D或者MAX这样的软件，经历建模、上材质等复杂步骤才能完成。

![伪3D](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/zhuanc.gif)


其实我们可以换种思路来处理这个问题，这里我们需要先引入这样一个例子：

生活中，当桌子上只有一张A4纸的时候，你就会觉得这张纸没什么立体感可言，但我们拿出一塌纸放在桌子上时，你能明显感觉到立体。

![](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/zhi.jpg)


参考这个例子，我们就可以使用图层**层叠**的方法，让一张平面图片表现出立体感，具体做法是让每一张图片与上一张图片在Z轴上差1个像素就可以了。

但是说起来简单，如果你不会表达式，这个过程其实不比建模简单，下面就为大家介绍具体的步骤：


首先我们打开图片的3D属性，并激活其位置属性的表达式，填写如下代码：


transform.position+[0,0,index]


![层叠](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190921144546.png)


这时好像并没有什么变化，不过问题不大，我们先来了解一下表达式的含义。

我们以加号为分界线把它分为前后两个部分。

前部分的transform.position代表是该图片的**位置值**，由于我们打开的图片的3D属性，所以它的取值为[x,y,z]
后半部分[0,0,index]的含义是，给该图片的位置值的X加上0，Y加上0,Z加上标签值，即[x+0,y+0,z+index]。


其中，标签值指的是图层名称前的数字标号，当图层数量增加时，index数值会按**自然数**的方式增加，即实现每复制一张图片，其位置在Z轴上与原图片差1个像素。

![标签](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/index.jpg)


接下来我们就可以使用「Ctrl+D」复制几次该图层，这时你可能感觉仍没有立体感，其原因是我们惯常的角度让Z轴的特征并不明显。

所以我们新建一架摄影机，使用「C」移动一下摄影机，一张平面图片就这样变成了立体的。接着，随手给摄影机的位置K上几个关键帧就能实现一些简单的动画啦。

![伪3D](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/Comp-2.gif)


## 弹性动画

我们在AE中的碰到的弹性动画主要分为两类，第一种是类似刚体的，比如小球从空中落至地面，会上下来回跳动，但不论怎样，其弹性的最大值不会超过设定的最大值；另一类是类似果冻，其弹性的最大值会超过设定值

### 刚体碰撞动画

我们快速创建一个圆形和长方形，用来当球体和地面。

初始位置让小球的一部分在画面之外，四帧后让小球与地面接触。

这时播放动画，你会看到小球生硬的砸在了地面上，你会觉得十分难受，这是因为这样的动画与我们的常识认知不相符而造成的。

![刚体表达式](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190921145556.png)

我们按住键盘上的「Alt」键，点击位置前面的小码表，会进入表达式编辑界面，这时，你可以把下面这段代码粘贴进去看看效果。

e = 0.7;
g = 5000;
nMax = 9;
n = 0;
if (numKeys > 0){
  n = nearestKey(time).index;
  if (key(n).time > time) n--;
}
if (n > 0){
  t = time - key(n).time;
  v = -velocityAtTime(key(n).time - .001)*e;
  vl = length(v);
  if (value instanceof Array){
    vu = (vl > 0) ? normalize(v) : [0,0,0];
  }else{
    vu = (v < 0) ? -1 : 1;
  }
  tCur = 0;
  segDur = 2*vl/g;
  tNext = segDur;
  nb = 1; // number of bounces
  while (tNext < t && nb <= nMax){
    vl *= e;
    segDur *= e;
    tCur = tNext;
    tNext += segDur;
    nb++
  }
  if(nb <= nMax){
    delta = t - tCur;
    value +  vu*delta*(vl - g*delta/2);
  }else{
    value
  }
}else  value



![刚体](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/Comp-3.gif)

这就是AE中表达式的神奇作用，有简单编程基础的朋友可以对上述代码进行个性化的修改，这里暂时不展开介绍。





### 果冻弹性动画


这类动画比较像Q弹Q弹的果冻，比如我们让一个圆的「缩放」属性从0%膨胀到100%，在这个过程中会出现「缩放」超过100%的情况。


我们新建一个圆形，让它最初大小为0%，四帧后调整它的缩放为100%

![果冻表达式](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/20190921150217.png)

此时播放动画，同样感觉较为生硬，我们可以再按住「Alt」键，将以下代码粘贴到表达式区域。


amp = .1; 
freq = 2.0; 
decay = 2.0; 
n = 0; 
if (numKeys > 0){ 
 n = nearestKey(time).index; 
 if (key(n).time > time){n--;} 
  } 
if (n == 0){ t = 0;} 
else{t = time - key(n).time;} 
if (n > 0){ 
 v = velocityAtTime(key(n).time - thisComp.frameDuration/10); 
 value + v*amp*Math.sin(freq*t*2*Math.PI)/Math.exp(decay*t); 
   } 
else{value} 


![果冻](https://makeapp-1258954924.cos.ap-chongqing.myqcloud.com/blog/Comp-4.gif)


其实，很多电视节目的花字就是运用这个表达式制作的，我们可以调整amp（振幅），freq（频率），decay（衰减）这些数值，然后修改部分内容，达到不同的效果，比如：


amp = .06;
freq = 2;
decay = 7;
n = 0;
if (numKeys > 0){
n = nearestKey(time).index;
if (key(n).time > time){
n--;
}
}
if (n == 0){
t = 0;
}else{
t = time - key(n).time;
}
if (n > 0){
v = velocityAtTime(key(n).time - thisComp.frameDuration/10);
M=Math.sin(freq*t*2*Math.PI)/Math.exp(decay*t); 
value + v*amp*M; 
}else{
value;
}


大家能在网上搜索到许多这类表达式，文本就不做过多介绍，大家以实用为主即可，可以尝试性理解其含义。

## 小结

我知道很多人看到Ae就望而却步，看到表达式更是被束缚住了手脚。不过，我希望看到这里的你，真的能静下心来去尝试、去练习、去进步。

没有人生来就会这些东西，而我本科所学的也不是这方面专业，一切的一切都是因为我想做出让人「Wow」的东西，所以我会去学习那些看了让自己发出惊叹的技巧，并不断实践、改良，把它变成属于自己的知识，所以我希望，聪明的你，也能不被眼前的困难所吓退，去学习，去尝试，去进步。



