## 3D LUT调色：单反如何实现电影级调色。


很多朋友非常喜欢电影的色调，希望数码照片也能按电影的方法去调色，以获得更大动态范围的调色处理。 要实现照片的电影级调色处理，简单来说就是把Raw转化成LOG模式，再加载3D LUT文件即可。

这种调色方式触摸了工业级别调色的门槛。毫无疑问，有些高质量的 lut，兼顾了美学品质，无需你多高明色偏手段，短时间就能获得媲美胶片般的色调。

首先，我们先说下思路。单反相机实现电影级调色的思路很简单：单反拍摄 raw 格式，raw 底片在 ACR 或者 LR 中转换为相机 DCP 校准文件。转换之后导入 PS 中利用颜色查找表进行叠加 3d lut 风格文件进行调色。

**输出RAW ->转换 LOG->叠加LUT**。差不多就是这么个玩法。

既然玩法提了，但咱们不能知其然不知其所以然。所以前期一些知识必要铺垫的还是要铺垫。这里涉及到电影工业化流程。就稍微提一下这些东西。大家心里面有个印象。

导览：

*   **什么是LOG格式**

*   **什么是3D LUT**

*   **如何应用单反相机转换出来的LOG格式**

*   **如何在PS用颜色查找表制作3D LUT文件** 

*   **如何在PS中利用3D LUT进行花式调色** 

*   **关于3D LUT预览**

*   **如何自己反编译相机LOG文件（伪）**

*   **关于3D LUT的调色**

*   **什么是LOG格式**

Log一词来源于单词logarithm，也就是对数，它本质是一条平滑的S曲线，这个log算法编码具有“全局动态范围”的特点。摄像机能把捕获的影像信息素材变“灰”变“平”。虽然看起来灰蒙蒙的。但记录和容纳更多的亮部跟暗部细节。通常胶片有十二档宽容度，而数码视频只有五档。在九十年代，柯达公司搞出一个cineon系统。胶转数的方式，用曲线形式压缩这个数码视频信号，压缩到适当的体积大小。就是靠这个曲线。它到底压缩了什么，其实就是压缩了对比度。数码log编码是被压缩对比度的。这根曲线压低了高光，提亮的暗部。尽可能保留中间调的细节。所以log格式视频信号看起来，灰蒙蒙的。

在这个压缩了大量的视频信号数据的基础上，LOG格式素材匹配和加载相应规范流程的LUT才能正确显示和进行电影级风格化调色。从而获得胶片般的效果。当然，随着技术在进步，现在的摄像机宽容度越来越大，LOG色彩空间已经变得非常广。专业级电影调色能呈现出的色彩也越来越丰富。

而log格式最早可以追溯到胶片时代，1980年由亨特（Hurter）和崔菲（Driffield）发明出H&D曲线。这是一种基于光密度法的胶片成像原理。而现代数码则起源于柯达。在1990年的时候柯达就已经搞出一套基于数字计算机的胶片扫描系统。这个很吊的系统叫Cineon System。由扫描仪、工作站软件和记录仪三个部分组成。靠这个系统，柯达公司获取了大量的胶片上的光学信息。靠这套玩意，柯达公司可是赚了不少大钱。

当然，log格式演变到现在。每个厂商都有自己的Log算法。Arri称为LogC，SONY有s-log,s-log3，佳能的则是Clog。值得注意的是不同的算法，所能记录的最亮值也不同，比如Arri的LogC能达到3500%，而佳能的Clog只能达到800%。毕竟德国Arri研究胶片都将近100多年的历史。底蕴深厚。厂商们还会给自己的log做适合lut文件，比如著名的rec 709。其实这就是Technical Lut的一种。在前期拍摄监看和后期调色时会经常用到这套lut。

至于为什么log格式为什么看起来很灰。这里涉及到两个概念方便理解，第一个是**场景对应图像**（Scene referred images）第二个是**输出对应图像**（Output referred images）。大部分影像信号被数码机器感光元件产生之后都是场景对应图像。然后经过机内加载处理或者监视器，后期校色，打印处理输出，得到输出对应图像。

打一个比方。数码摄影拍摄保存为raw数字底片格式。然后进入ACR或者LR进行后期。处理过后得到的正片便是输出对应图像。所以为什么厂商们要出对应log文件的lut进行加载处理。方便现场调色，监看等等。再提一次rce709 lut便是这样产生的。

为毛现在流行拍摄LOG格式呢，进行log拍摄可以保留更多的曝光，存储的色彩空间，宽容度等图像信息更加丰富。更加方便进入后期创造。现在调色师行业可是香饽饽。调色预算一点也不低级，甚至一步电影，调色预算就达上百万美元。所有log模式拍摄都需要后期调色，给导演或者调色师更多创作空间的余地。创作出绚丽多彩的影像。

log模式与后期调色对比。（来源拍电影论坛李宗泰）

log

<noscript><img src="https://pic4.zhimg.com/v2-5a018b12181c46c00d25bad94313256b_b.jpg" data-rawwidth="750" data-rawheight="422" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic4.zhimg.com/v2-5a018b12181c46c00d25bad94313256b_r.jpg"></noscript>

<span></span>

后期调色

<noscript><img src="https://pic4.zhimg.com/v2-d631b19323f3dd937250ebae86260af7_b.jpg" data-rawwidth="750" data-rawheight="422" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic4.zhimg.com/v2-d631b19323f3dd937250ebae86260af7_r.jpg"></noscript>

<span></span>

*   **什么是3D LUT**

LUT是Look Up Table的缩写，意为“查找表”。打开这个lut文件，会发现里面一堆数值。这是一堆原始RGB数值。而这些RGB数值的输入值将会转化成新的输出值。其本质就是把一种颜色的效果转化为另一种颜色效果。或者是灰度值转化为另一种灰度值。

当然lut也是分种类的。有1D LUT跟3D LUT的分别。1D LUT只能控制gamma值、RGB平衡（灰阶）和白场（white point）而3D LUT能以全立体色彩空间的控制方式影响色相、饱和度、亮度等。简单描述来说3D LUT可以影响到颜色，而1D LUT只能影响亮度值。

被降成一维的1d lut

<noscript><img src="https://pic3.zhimg.com/v2-b1cea91f33cf3818eda2a0b7966eae76_b.jpg" data-rawwidth="300" data-rawheight="300" class="content_image" width="300"></noscript>

<span></span>

1D LUT变动某个颜色输入值只会影响到该颜色的输出值，RBG的数据之间是互相独立的。

升维3D LUT

<noscript><img src="https://pic4.zhimg.com/v2-fb964f14da0269c709bb26e6eb80621f_b.jpg" data-rawwidth="330" data-rawheight="330" class="content_image" width="330"></noscript>

<span></span>

3D LUT变动某个颜色值，都会对三个颜色值造成影响，也就是说任何一个颜色的改变都会对其他颜色做出改变。

而我们发现，大多数网络流传的基本是3D LUT预设，基本都是3D LUT。因为除了亮度之外还要调节色彩的话，就只能选择3D LUT。

**关于LUT的应用。**

Lut从用途上可以分为三种。

1，校准（calibrtion Lut），主要用于色彩管理中硬件和显示设备校准。比如现场监视器，调色平台监视器等等。这种LUT能够确保经过校准的显示器可以显示尽可能准确的图像。

2，技术（Technical Lut），多用于不同色彩空间不同特性曲线下的转换，从Log映射到Rec709即属于此种类型。简单来说现在的一些数码摄影机RED Epic, Alexa等这些拍出图像为的log格式，看起来很灰很平，在现场工作环境用看这种灰的片子难以把握方向，这个时候后套用LUT 档，套入REC709这些还原接近人眼的颜色。直接在摄影机外接的监视器看到的画面色彩有没有校正或者是不是自己想要的颜色。每个厂商对自己的摄影机提供相对应发挥性能的LUT。

3，风格（Creative Lut／Looks Lut），为实现某种特定风格而制作的lut，摄影指导在前期拍摄中制作并可现场预览的Lut。大多数是第三方提供的摄影风格化LUT。也有一些后期厂商为摄影机的需求提供的LUT。比如达芬奇自身为适应不同的摄影机需求而主动嵌入软件的LUT。这些文件既有cube类型的，也有DXP类型的。从某种角度，也可以成为调色LUT。

现在网上大肆所买的XXX千种胶片LUT预设，就是这样类型的风格lut。这种预设简单粗暴。套用婚礼，低成本广告，庆典之类。经常拿来忽悠低端客户。毕竟低成本的东西，就靠忽悠来钱，多弄几套预设给客户，然后说着是加班加点做好不同的风格。您喜欢那种我们就用哪种。哇，对方很感动。乐呵呵的就掏钱了。

电影工业级调色流程对规格掌控要求非常严格。限定规范是为了更好的效果。所以无论是摄影机输出，监看。后期调色。都需要有套入LUT文件整理规范。

当然，我们在这说的是如何用单反实现电影级色调。所以我们就直接简化流程。借用3D LUT预设从而模拟达到电影色调。

**如何应用单反相机转换出来的LOG格式**

下载：VisionLOG文件。

解压后得到一个文件夹，把这个文件夹拷贝到：

Mac OS:HD:/Users/yourusername/Library/ApplicationSupport/Adobe/CameraRaw/CameraProfiles

Windows:C:/Users/yourusername/AppData/Roaming/Adobe/CameraRaw/CameraProfiles

<noscript><img src="https://pic1.zhimg.com/v2-07865fdbc1bacee89e510d2e484f58fc_b.jpg" data-rawwidth="750" data-rawheight="714" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-07865fdbc1bacee89e510d2e484f58fc_r.jpg"></noscript>

<span></span>

支持的一些主流机型。

下载：3D LUT预设文件。

放入photoshop中的3DLUT目录。

X:\Adebo\Adobe Photoshop CC 2015.5\Presets\3DLUTs

<noscript><img src="https://pic3.zhimg.com/v2-b1e77d8ba11854c161c119be6f257046_b.jpg" data-rawwidth="500" data-rawheight="140" class="origin_image zh-lightbox-thumb" width="500" data-original="https://pic3.zhimg.com/v2-b1e77d8ba11854c161c119be6f257046_r.jpg"></noscript>

<span></span>

接着，随便调入一张RAW文件，尼康或者佳能拍摄的。在以下相机校准中选择LOG模式。当然，如果VisionLOG没有自己手上的相机型号的文件，那么就很遗憾了，ACR不能识别了。但没关系，我们后面可以自己反编译一个LOG相机文件。

<noscript><img src="https://pic4.zhimg.com/v2-d9eb06e72fa6bb376f9c42ca9e51fc1f_b.jpg" data-rawwidth="750" data-rawheight="450" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic4.zhimg.com/v2-d9eb06e72fa6bb376f9c42ca9e51fc1f_r.jpg"></noscript>

<span></span>

就这样，直接把片子怼灰了。怼平了。当然，你也可以继续在基本面板里压高光，提亮阴影。恢复大量的细节。

<noscript><img src="https://pic3.zhimg.com/v2-bc6e04a8c3303a728155f09da270d71e_b.jpg" data-rawwidth="750" data-rawheight="450" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic3.zhimg.com/v2-bc6e04a8c3303a728155f09da270d71e_r.jpg"></noscript>

<span></span>

然后，我们按住SHIFT键点击打开图像，愉快的进入photoshop。（按住SHIFT进入photoshop图像会变智能对象模式，不满意的随时可以调用camera raw进行修改操作）

接着在photoshop中打开颜色查找表。图层-新建调整图层-颜色查找

<noscript><img src="https://pic2.zhimg.com/v2-471e60fc1af070f62db95e78aeeac649_b.jpg" data-rawwidth="343" data-rawheight="750" class="content_image" width="343"></noscript>

<span></span>

<noscript><img src="https://pic2.zhimg.com/v2-397aa8e8dd662f69840cf604e7e09985_b.jpg" data-rawwidth="593" data-rawheight="385" class="origin_image zh-lightbox-thumb" width="593" data-original="https://pic2.zhimg.com/v2-397aa8e8dd662f69840cf604e7e09985_r.jpg"></noscript>

<span></span>

点击载入3D LUT，愉快的选择各种LUT进行叠加。

<noscript><img src="https://pic1.zhimg.com/v2-59814a23f0e71cf694b71f789a5c6d48_b.jpg" data-rawwidth="750" data-rawheight="403" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-59814a23f0e71cf694b71f789a5c6d48_r.jpg"></noscript>

<span></span>

需要注意的是。

**颜色查找层可以叠加图层。**

**可以调不透明度进行影响。**

**可以图层混合模式进行影响。**（滤色，叠加，柔光等）

如果一上来就是直接怼预设。那么你会惊讶的发现，咦这跟美图秀秀没区别嘛。这滤镜加的666。所以，建议以上方式进行调节。

。

调完之后，既然是电影级调色，我们是不是应该进行那啥。加黑边。

再加点什么字啥的。一个LOWBEE级别的MV就完成了。实际可控性还是很多的。

<noscript><img src="https://pic1.zhimg.com/v2-a09c4d957e859a9a1255daf199705228_b.jpg" data-rawwidth="750" data-rawheight="500" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-a09c4d957e859a9a1255daf199705228_r.jpg"></noscript>

<span></span>

*   **如何在PS用颜色查找表制作3D LUT文件**

在photoshop中使用调整图层建立了调色方案。那么怎么保存为3D LUT文件呢。2014版以后的photoshop解决这个问题。

<noscript><img src="https://pic2.zhimg.com/v2-2d4baa09d51ebf7d6232d5d0d4cb95c1_b.jpg" data-rawwidth="750" data-rawheight="680" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic2.zhimg.com/v2-2d4baa09d51ebf7d6232d5d0d4cb95c1_r.jpg"></noscript>

<span></span>

首先打开我们打开photoshop。记住，要**包含自带背景图层**，也就是直接打开图片就行。

<noscript><img src="https://pic2.zhimg.com/v2-3ad8ccadc3121325f765bd2273f472c5_b.jpg" data-rawwidth="315" data-rawheight="67" class="content_image" width="315"></noscript>

<span></span>

<noscript><img src="https://pic2.zhimg.com/v2-cfe7c2a25ef78769e24651dfddc0c951_b.jpg" data-rawwidth="750" data-rawheight="360" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic2.zhimg.com/v2-cfe7c2a25ef78769e24651dfddc0c951_r.jpg"></noscript>

<span></span>

进行一系列的新建**调整图层**进行调色。当然你也可以用3D LUT来调色。记住，只用新建调整图层。

调色完之后。我们开始导出到颜色查找表。

<noscript><img src="https://pic1.zhimg.com/v2-dc3e713802f4fffb804922a1559ca880_b.jpg" data-rawwidth="634" data-rawheight="647" class="origin_image zh-lightbox-thumb" width="634" data-original="https://pic1.zhimg.com/v2-dc3e713802f4fffb804922a1559ca880_r.jpg"></noscript>

<span></span>

<noscript><img src="https://pic4.zhimg.com/v2-4958b1975b9215a7a72751dced3ac33f_b.jpg" data-rawwidth="456" data-rawheight="372" class="origin_image zh-lightbox-thumb" width="456" data-original="https://pic4.zhimg.com/v2-4958b1975b9215a7a72751dced3ac33f_r.jpg"></noscript>

<span></span>

确定保存。存在自己能找到的位置。

然后你会得到一个CUBE格式的文件，这个文件就是3D LUT。我们可以任意加载这个文件。

比如我想加载在自己另一张照片上得到的效果。

<noscript><img src="https://pic3.zhimg.com/v2-13b09f4ff9e2899fe26c6e265454f0a6_b.jpg" data-rawwidth="750" data-rawheight="376" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic3.zhimg.com/v2-13b09f4ff9e2899fe26c6e265454f0a6_r.jpg"></noscript>

<span></span>

直接加载这个LUT，有点过曝泛灰白。那我们施展一个微小的曲线魔法。

怼一下。分分钟好了。

<noscript><img src="https://pic4.zhimg.com/v2-db4e9d943d7108e4fe28a54dc658534b_b.jpg" data-rawwidth="750" data-rawheight="309" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic4.zhimg.com/v2-db4e9d943d7108e4fe28a54dc658534b_r.jpg"></noscript>

<span></span>

**如何在PS中利用3D LUT进行花式调色**

这个问题嘛，其实有很多玩法。比如用亮度蒙版+3D LUT这样的方式进行更加精确的打击。有些摄友已经PO过这个教程。这里我依然还是简单说一下。然后给伸手党一个福利。再开头的下载地址，我已经放了亮度蒙版动作。方便大家。这里就不放插件面板了。面板安装比较复杂一点点。不是每个版本的PS都能适用。所以，直接来个动作快捷一点。

对着图片怼一下这个动作，我们就能获得以下神奇魔法。

<noscript><img src="https://pic1.zhimg.com/v2-589782194eb49284f476d6c6c574063c_b.jpg" data-rawwidth="323" data-rawheight="707" class="content_image" width="323"></noscript>

<span></span>

载入这些选区，我们可以针对任意亮度区域调色。

如何施展这种魔法呢，很简单。在通道面板里，我们按住ctrl键对着亮1通道怼以下鼠标左键。看到蚂蚁线没有，表示获得选区。

<noscript><img src="https://pic1.zhimg.com/v2-3e8744ed224910afb61105f01e289150_b.jpg" data-rawwidth="750" data-rawheight="341" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-3e8744ed224910afb61105f01e289150_r.jpg"></noscript>

<span></span>

得到选区，我们再回到图层面板对着什么曲线调整层，什么色彩平衡调整层，甚至是3D LUT调整层。怼一下。魔法完成。

<noscript><img src="https://pic4.zhimg.com/v2-eb122618e8567d3dcf1abdcdf6ab072f_b.jpg" data-rawwidth="386" data-rawheight="296" class="content_image" width="386"></noscript>

<span></span>

<noscript><img src="https://pic4.zhimg.com/v2-4f94968d6125fa89691a932879dd8c83_b.jpg" data-rawwidth="750" data-rawheight="506" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic4.zhimg.com/v2-4f94968d6125fa89691a932879dd8c83_r.jpg"></noscript>

<span></span>

这样，我们就能对任意区域进行调色，曝光操作了。这也就是所谓的分区调色，分区曝光。常用于风光领域，当然，我们这里这样用也是可以的。

已经有大神出过这样的教程。这里就简单描述一下。然后再是PO出伸手党的福利，可以到前面的下载地址，下载这个动作，加载到PS中即可。

亮度蒙版

<noscript><img src="https://pic3.zhimg.com/v2-db9b85d6242b38ac209bf79cf715898a_b.jpg" data-rawwidth="474" data-rawheight="227" class="origin_image zh-lightbox-thumb" width="474" data-original="https://pic3.zhimg.com/v2-db9b85d6242b38ac209bf79cf715898a_r.jpg"></noscript>

<span></span>

*   **关于3D LUT预览**

为了方便大家更加直观明白3D LUT色调，所以我自己做了一个查找表预览LUT表。把我自己常用的LUT文件置入其中，方便预览效果。下载地址还是在最前面。LUT文件跟PSD预览都打包在里面，喜欢折腾的同学也可以把自己喜欢的LUT置入PSD预览版中的或者更换智能对象中的照片做自己的风格LUT表。

风格LUT表

<noscript><img src="https://pic4.zhimg.com/v2-8a1c91b5fcc12b8345c9ba5b98ff2187_b.jpg" data-rawwidth="549" data-rawheight="282" class="origin_image zh-lightbox-thumb" width="549" data-original="https://pic4.zhimg.com/v2-8a1c91b5fcc12b8345c9ba5b98ff2187_r.jpg"></noscript>

<span></span>

<noscript><img src="https://pic1.zhimg.com/v2-9ece84bcc061a6060b2c46e11efa0120_b.jpg" data-rawwidth="750" data-rawheight="431" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-9ece84bcc061a6060b2c46e11efa0120_r.jpg"></noscript>

<span></span>

当然，接下来还有一种黑科技，也可以很方便的预览LUT文件。

这个黑科技叫3DLut Creator。是一款编辑分析3d lut的软件。正版是要收费的。但还有Demo版试用。我们可以简单的分析或者预览LUT文件。如果要保存编辑的LUT文件，需要购买正版。一般分析或者预览，Demo就足够了。当然，这个软件demo的win版我也打包在最前面的下载地址当中了。尽情下载吧。

<noscript><img src="https://pic3.zhimg.com/v2-55a20fb4e949ba4243cd9e19eee5b40e_b.jpg" data-rawwidth="750" data-rawheight="366" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic3.zhimg.com/v2-55a20fb4e949ba4243cd9e19eee5b40e_r.jpg"></noscript>

<span></span>

我们到底怎么用3DLut Creator预览LUT之后的照片呢。首先我们打开3DLut Creator。然后再PS中打开你需要预览的照片。

<noscript><img src="https://pic1.zhimg.com/v2-83ea37c2aecbf6ceb210624332c57ed8_b.jpg" data-rawwidth="750" data-rawheight="413" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-83ea37c2aecbf6ceb210624332c57ed8_r.jpg"></noscript>

<span></span>

然后在切换回3DLut Creator。点击mask面板，再点一下Imge from ps。

这样就把Photoshop中的照片怼到3DLut Creator里面。

当然，如果不用PS，你也可以直接点击Load Image直接选择需要预览LUT的照片。

这个步骤只是为了关联PS跟3DLut Creator。更好的进行LUT编辑。我们可以实时在PS编辑然后在3DLut Creator中分析。

<noscript><img src="https://pic2.zhimg.com/v2-2ae696f192279eec7a6c43a67a5d2171_b.jpg" data-rawwidth="750" data-rawheight="366" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic2.zhimg.com/v2-2ae696f192279eec7a6c43a67a5d2171_r.jpg"></noscript>

<span></span>

接着我们在External 3d lut 下面选择Input输出这一项

<noscript><img src="https://pic1.zhimg.com/v2-94311182cada760f2e8badbc9c498064_b.jpg" data-rawwidth="390" data-rawheight="190" class="content_image" width="390"></noscript>

<span></span>

再点击 load lut 。调用你需要预览的3D LUT所在的文件夹。这里我直接选择Photoshop中放置3D LUT的文件夹。

<noscript><img src="https://pic1.zhimg.com/v2-3edb2c4845b51baf4e0cff4e7d72614c_b.jpg" data-rawwidth="750" data-rawheight="366" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-3edb2c4845b51baf4e0cff4e7d72614c_r.jpg"></noscript>

<span></span>

接下来我们就可以愉快的点击<>按钮进行LUT选择了。虽然没LUT表来的方便，但基本上满足了需求。

<noscript><img src="https://pic1.zhimg.com/v2-e42c587704afc52f2e028644ab33c434_b.jpg" data-rawwidth="750" data-rawheight="391" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-e42c587704afc52f2e028644ab33c434_r.jpg"></noscript>

<span></span>

*   **如何自己反编译相机LOG文件（伪）**

我的相机没有VisionLOG怎么办。

那么我们可以自己做一个，虽然吧。这很不规范。但是既然是屌丝版电影级调色。那么将就一下也无所谓。折腾党们搞起来。嗨起。A威巴迪康萌。

参考来源无忌论坛：[让你的相机也能使用LOG调色 - 数码暗房](http://link.zhihu.com/?target=http%3A//forum.xitek.com/thread-1344041-1-1-1.html)

下面进入实际操作。

首先下载dcptool。这个我也打包了。我都快被自己感动了。下载地址还是在帖子最前面。

下载后，解压得到一个dcptol，然后把dcptool文件夹放到D盘根目录下（就是直接盘根目录，不外包任何文件夹）

然后将要转换样本VisionLOG_5DMkII.dcp放到dcptool中。样本DCP文件可以在VisionLOG文件夹中提取（就是下载地址中的VisionLOG压缩文件）。

接下来我们开始

1，启动命令提示符 （通常是 按win键+R ，win键就是小旗子那个按键）

2，在里面输入 cmd 回车，弹出一个黑黑的命令框。

3，输入**d:**进入D盘，再输入**cd dcptool**进入文件夹

4，反编译成XML

**dcptool -d VisionLOG_5DMkII.dcp VisionLOG_FujiXT1.xml**

回车

也可以命名为你相机的名称，例如：
dcptool -d VisionLOG_5DMkII.dcp VisionLOG_d810.xml

回车

5，走完第四步，我们再dcptool中获得一个叫VisionLOG_FujiXT1的文件。

<noscript><img src="https://pic3.zhimg.com/v2-e232627c132a46404d52203e12039d62_b.jpg" data-rawwidth="640" data-rawheight="199" class="origin_image zh-lightbox-thumb" width="640" data-original="https://pic3.zhimg.com/v2-e232627c132a46404d52203e12039d62_r.jpg"></noscript>

<span></span>

用记事本编辑打开它，一直拉倒最后面。

<noscript><img src="https://pic2.zhimg.com/v2-8518eabfe8d335d39bf94f3692508209_b.jpg" data-rawwidth="750" data-rawheight="513" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic2.zhimg.com/v2-8518eabfe8d335d39bf94f3692508209_r.jpg"></noscript>

<span></span>

找到样本相机型号，然后改为你自己的相机型号。比如你的相机是Nikon D7100，就把Canon EOS 5D Mark II 改成Nikon D7100 ，然后保存即可。(**型号一定要对齐**。少一个字少一个符号都回识别不出。)

怎么确定自己的相机型号呢，如果不知道自己的相机型号你可以在Camera raw里找到自己相机型号，一字不漏敲进去，然后保存。

<noscript><img src="https://pic4.zhimg.com/v2-09ce4a167246698aac24495ea3ae2937_b.jpg" data-rawwidth="361" data-rawheight="111" class="content_image" width="361"></noscript>

<span></span>

6、重新编译为dcp

**dcptool -c VisionLOG_FujiXT1.xml VisionLOG_FujiXT1.dcp**

回车

返回dcptool文件夹，我们可以看到已经获得属于你相机型号的LOG预设的DCP文件。

<noscript><img src="https://pic2.zhimg.com/v2-ac75c375b40ce7e7878459d73a0af961_b.jpg" data-rawwidth="732" data-rawheight="281" class="origin_image zh-lightbox-thumb" width="732" data-original="https://pic2.zhimg.com/v2-ac75c375b40ce7e7878459d73a0af961_r.jpg"></noscript>

<span></span>

然后把这个文件放到哪？前面说过的。我们放到C:/Users/yourusername/AppData/Roaming/Adobe/CameraRaw/CameraProfiles 中的 VisionLOG文件夹即可。

还有一种方法，也可以把别的相机log转为适用于你的相机型号的log格式。

这个软件叫DNG Profile Editor，是啊逗比公司出品的。本文下载地址里面附带了这个软件，这个软件是啊逗比公司为了让摄影师更方便的编辑相机色彩配置文件。

首先，我们先找一张raw格式照片，然后在camera raw 或者LR中，转格式。**转为dng格式**。

<noscript><img src="https://pic2.zhimg.com/v2-daeea347e078704f210b5d40ed419031_b.jpg" data-rawwidth="750" data-rawheight="450" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic2.zhimg.com/v2-daeea347e078704f210b5d40ed419031_r.jpg"></noscript>

<span></span>

然后打开DNG Profile Editor。在file一栏下点击 open dng inage打开我们刚才转换的dng格式照片。

<noscript><img src="https://pic1.zhimg.com/v2-882b5c16f35df396c67af22f28872778_b.jpg" data-rawwidth="750" data-rawheight="404" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-882b5c16f35df396c67af22f28872778_r.jpg"></noscript>

<span></span>

然后再点击base proflie，下拉选择 Choose external profile

<noscript><img src="https://pic3.zhimg.com/v2-e66ef84755dc5be460bac8bc5a25fbda_b.jpg" data-rawwidth="750" data-rawheight="442" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic3.zhimg.com/v2-e66ef84755dc5be460bac8bc5a25fbda_r.jpg"></noscript>

<span></span>

点击Choose external profile后弹出文件夹，选择我们下载的 VisionLOG文件。选择一个dcp文件。我这里选的是5D3的DCP文件。

<noscript><img src="https://pic2.zhimg.com/v2-4e74a5c4e8b277d6b84340078d7619c5_b.jpg" data-rawwidth="750" data-rawheight="435" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic2.zhimg.com/v2-4e74a5c4e8b277d6b84340078d7619c5_r.jpg"></noscript>

<span></span>

OK，选择之后，画面明显变灰。起了作用。

<noscript><img src="https://pic1.zhimg.com/v2-e1cb738a10d4459974b2764357c74638_b.jpg" data-rawwidth="750" data-rawheight="404" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-e1cb738a10d4459974b2764357c74638_r.jpg"></noscript>

<span></span>

然后再file中选择 Export canon EOS 5D Mark II proflie。把DCP文件导出来。

<noscript><img src="https://pic1.zhimg.com/v2-64302679801b1179df2ef5a80828107c_b.jpg" data-rawwidth="677" data-rawheight="580" class="origin_image zh-lightbox-thumb" width="677" data-original="https://pic1.zhimg.com/v2-64302679801b1179df2ef5a80828107c_r.jpg"></noscript>

<span></span>

保存在

Mac OS:HD:/Users/yourusername/Library/ApplicationSupport/Adobe/CameraRaw/CameraProfiles

Windows:C:/Users/yourusername/AppData/Roaming/Adobe/CameraRaw/CameraProfiles

这里还是要记得一点，保存的名字为VisionLOG_XXX（XXX就是你相机型号，不知道自己相机型号可以进入camera raw中查询。这型号是厂商命名型号。一个字不能少。少了就识别不出。）

<noscript><img src="https://pic4.zhimg.com/v2-b69f3021f5a9c30b19c3e4c4132f8d2f_b.jpg" data-rawwidth="750" data-rawheight="482" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic4.zhimg.com/v2-b69f3021f5a9c30b19c3e4c4132f8d2f_r.jpg"></noscript>

<span></span>

duang，这样一怼。我们需要的相机logDCP文件就好了。

反正大体思路就是改个型号的事。你要说不严谨，那也没办法。因为本质来讲。这些VisionLOG本身制作没有说完全走规范流程。不是说相机厂商出的LOG预设，毕竟每个厂商的cmos,曲线，电路都不同。所以只是从理论上把RAW解成灰度。不是说相机厂商做出适用相机而出的log。单反始终是单反。

*   **关于3D LUT的调色**

本来，这个要另外再写一篇的。但考虑到用3d lut作为照片调色总是一个不对劲的事。为什么这么说，很简单。lut的用途不单单是调色。之前说过。lut作为校准，统一不同设备显示相同色彩而作为的规范。或者是现场拍摄临时在片场加载色调观察。调色只是区区其中一种用途。为什么现场需要加载临时LUT？太简单了，制片人投资商来片场看片子，你给老板看灰的飞起的log？分分钟老板要撤资的。所以一般都会外接监视器加载正常还原的lut。来回切换效果。好吧都是我瞎编的。

显然现在各家摄影机厂商，后期软件厂商都会推出的针对不同摄影机型号而出lut，是为了更好发挥摄影机的性能。而应用在单反身上，那么单反也未必能发挥出来起一定优势。当然。现在有些单反确实能应用log模式。比如索尼大法a7s。其slog格式也独占风骚。

好闲话少说，关于3d lut照片调色。怎么才能用的更好。个人理解了一些思路，分享给大家。

**适合于低调，中间调氛围**。如果是高调的片子，lut 色彩偏移再丰富也是白搭。

**70%前期打光 30%后期润色**。尤其是想突出冷暖对比的片子。前期光源非常重要，比如霍比特人中一些看冷暖对比的场景。很经典。看起来很舒服，这些需要依靠前期不同色温的灯光进行打光。作为一个对比的基础。如果整个画面很平，色彩单一。使用LUT并不能做出太大的戏剧效果。

<noscript><img src="https://pic3.zhimg.com/v2-eb6288e6575dd98f37173dd00026ddb6_b.jpg" data-rawwidth="500" data-rawheight="312" class="origin_image zh-lightbox-thumb" width="500" data-original="https://pic3.zhimg.com/v2-eb6288e6575dd98f37173dd00026ddb6_r.jpg"></noscript>

<span></span>

使用一些lut为你的片子**打底**。可以优先选择一些rec 709的lut打底。最近 Vision Color公司出了一组ImpulZ胶片lut。其中 有些709挺支持单反的。这里已经上传了度盘了，下载地址在最前有。

当然，我们可以不用一味的滥用lut图层叠加，换个方式调整也可以。

还是PO个案例吧。

首先在Photoshop中建立一个调整颜色查找图层，这里我选择的是ImpulZ - Canon VisionLOG RAW中的Kodak Elite Color 200_FPE.cube 3d lut预设打底。这个胶片对比，青偏橙。跟当时现场差不多。原片本身就是在暖色路灯情况下拍摄。在人物后面是白炽灯，属于冷光源，所以前期光线充分。底子就差不多了。

但是，看直方图，我们可以看到，暗部跟高光都被压缩了。所以下一步就用曲线，色阶之类的调整照片的亮度值。

<noscript><img src="https://pic2.zhimg.com/v2-8ce426b5dff1de1aa147f9f4c8389e81_b.jpg" data-rawwidth="750" data-rawheight="412" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic2.zhimg.com/v2-8ce426b5dff1de1aa147f9f4c8389e81_r.jpg"></noscript>

<span></span>

根据直方图，我使用曲线调整了是画面的亮度值，画面明显增加了对比。但是一些细节还是没有压出来。不管他，先盖印。

<noscript><img src="https://pic1.zhimg.com/v2-b65fecdbc109baee5de985e51d67aab0_b.jpg" data-rawwidth="750" data-rawheight="423" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic1.zhimg.com/v2-b65fecdbc109baee5de985e51d67aab0_r.jpg"></noscript>

<span></span>

盖印之后，用曲线进入亮度蒙版操作，针对高光区域跟中间调区域压暗。压暗继续变灰，然后这里我在暗部区域加了一个增加对比度lut进行对比。感觉画面还不够暖，直接再高光区域加橙色纯色层蒙版，再提炼橙色色调。

<noscript><img src="https://pic2.zhimg.com/v2-5450074af27c04da07aef8397ff59291_b.jpg" data-rawwidth="750" data-rawheight="396" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic2.zhimg.com/v2-5450074af27c04da07aef8397ff59291_r.jpg"></noscript>

<span></span>

继续加lut，加了两个lut，一个是提亮肤色，发色。一个是在暗部加青。最后在光源方向加一个渐变层。基本调色就差不多了。

<noscript><img src="https://pic4.zhimg.com/v2-9a177fa7de9a3f5c598df64b80e35bc3_b.jpg" data-rawwidth="750" data-rawheight="371" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic4.zhimg.com/v2-9a177fa7de9a3f5c598df64b80e35bc3_r.jpg"></noscript>

<span></span>

然后，盖印，加颗粒加黑边，降噪锐化输出。

<noscript><img src="https://pic3.zhimg.com/v2-18bf7b564eefc72a3f553301cd3a026e_b.jpg" data-rawwidth="750" data-rawheight="500" class="origin_image zh-lightbox-thumb" width="750" data-original="https://pic3.zhimg.com/v2-18bf7b564eefc72a3f553301cd3a026e_r.jpg"></noscript>

<span></span>

到这里就差不多结束了。这里还是要提一下，这种调色方式玩一玩，折腾一下是没问题的。但实际应用仍然不足以应对所有情况。一个摄影师如果只依赖预设，那么一些东西永远无法触及。美学品质这个东西，不是一键就能达成的。

本文所有软件下载地址都在这儿。已更新地址。重新上车。

[<span class="invisible">https://</span><span class="visible">pan.baidu.com/s/1hrFcdo</span><span class="invisible">g</span><span class="ellipsis"></span>](http://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1hrFcdog)