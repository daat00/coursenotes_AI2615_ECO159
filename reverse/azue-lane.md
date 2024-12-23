AL 逆向解包：live2d, spine 动态立绘，背景图
====


Live2d 工具：Cubism; Live2dviewerEX

按笔者理解，Live2d 通过 u3d 的插件制作成 prefab 被加入到 u3d 工程里。编译后被 u3d 打包到 APK 资源目录的 AssetBundles 目录下。该目录的二级目录会被 u3d 再次打包（笔者实验中见到了 .ys 文件和无后缀文件），可以使用 AssetStudioModGUI 工具解包复原目录结构。（有文章说 AssetRipper 是一个好用的 u3d 解包工具，我没试过）

关于 AB 文件：Unity移动端的手游 通常都会打ab包热更新资源。StreamingAssets文件夹下的资源会在项目Build发布后 原样复制到文件夹下，不会被unity进行整合处理。ab包用于热更新，所有一般会在这个文件夹下。但是也有资源全在Data文件夹里，没有打ab包的情况，常在小规模的独立游戏出现、

逆向解包的资料很杂，网上有很多创意工坊作者分享的教程。这些资料侧重于在 l2d 编辑器复现游戏表现效果，工作重心偏向美术设计，因而解包能跑就行，过程中的 warning 和 missing files 没啥解释。笔者没做过 u3d 二游开发自然也是不会的，只是记录一下心路历程。

解包 live2d
参考链接
关于Unity2d游戏动态CG和Live2D动画的提取以及逆向思路 - 哔哩哔哩​​​​​​

从零开始，金坷垃都会的碧蓝航线动态壁纸制作教程——以Live2DViewer为例 - 哔哩哔哩

碧蓝航线拆包记录 | 次元客栈

这篇用了 UnityLive2DExtractor。笔者没跑通，也没跑通他的一个 fork UnityCNLive2DExtractor:

解包某款2D手游中的live2d看板娘 - live2d - waifu - 教程 | 花园仓鼠的博客 = = 记录一些个人笔记,踩坑记录等

live2d编译器食用教程 - 教学 - Live2DHub 论坛好东西很多

过程大体分两步：解包复原目录和文件；然后是用编辑器打开（也验证了正确性）

大致猜一下 u3d dev 的流程。在引擎搭好 UI Scene 之后，l2d 的每个动作 motion 类似于播放一个动画；实现 l2d 的动作切换可能要涉及到一些逻辑播放不同 motion。



操作很简单：用 AssetGUIViewer 打开 AB/live2d 目录，识别后导出为 Cubism 格式。在 Console 会有一些 warning，不懂，暂不影响提取 l2d 的文件。得到的目录包含一个 .moc 文件，这是 Cubism 的入口；motion3.json 定义了一个动作，规定每个部件的动画；model3 关联模型和纹理；.physics3 关联物理效果；.cdi3 描述参数。

│   aijier_4.cdi3.json // Omit similar files
│   aijier_4.moc3
│   aijier_4.model3.json
│   aijier_4.physics3.json
├───motions
│       complete.motion3.json
│       effect.motion3.json
│       touch_idle8.motion3.json
│       touch_special.motion3.json
│       wedding.motion3.json
└───textures
        texture_00.png
        texture_01.png
用 cubism-viewer 打开或者在 l2dViewer 打开不做修改会发现，点击画面不会触发动作，仅在列表点击每个动作时播放对应动画。显然是因为 u3d 的逻辑没有被导出（笔者理解 l2dviewer 的点击区域和动画系统是自己实现的非标接口，有待考证）。

为什么会这样？AssetStudioGUI解开之后提取 prefab，除了贴图，还有Animator（动画），AnimationClip（动画片段），MonoBehaviour（挂载脚本），这些都是unity工程内的对象，没有被导出或者导出不完全。CubismSDKForUnity是Live2D的官方工具包，可以在将Live2D工程导入unity时，自动根据那些json文件生成unity工程可以使用的对象和文件: .prefab, .fade, .anim

注：这一段来自[1]，该文写这一段的时候没有导出 motion, physics 等 json，只导出了 moc。笔者怀疑这几个类已被 UL2DExtractor 导出为 json 。动作跳转的逻辑不知道在哪，怀疑没有，需要看一下 u3d-l2d 开发。该作者试过导出 fbx 是空的；moc 文件甚至是用 hexeditor 搜 MOC 头搞出来的。

编辑 l2d
笔者的理解，每个 motion 其实就是一个 onClickHandler。利用 l2dviewer 的额外功能，可以创建点击区域，并将点击区域绑定到动作。这些新的配置会被保存在项目新创建的 json 里。也可以在 l2dviewer 锁死控制参数，下面的截图用的 cubism



[4] 提到，不同版本的 l2d 命名方式不同（好像主要是大小写），可能导致找不到文件动画播放不了。这部分在 l2dviewer 的文档里有写。该文还提到了 l2d-web 的解决方案。

GitHub - GardenHamster/MotionModifys: 用于批量修改Live2d motion3.json文件中的参数名



注：l2d-dev crack: `github/Live2D_Cubism_crack` by a1640727878

论 Live2D Cubism Crack 补丁的使用方法 - 哔哩哔哩

该项目思路：java 中，用 zip 库删除签名 META-INF，然后用 asm 库动态修改判断地址。打包成 jar，用程序自带的 java 程序跑。

解包 spine
参考链接
关于Unity2d游戏动态CG和Live2D动画的提取以及逆向思路 - 哔哩哔哩

（●´3｀●）やれやれだぜ

如何提取并将碧蓝航线小人导入Spine（以及unity） - 哔哩哔哩

胎教级碧蓝航线立绘提取教程 - 哔哩哔哩

关于 spine
AL 的立绘是 spine，也是被拆成很多块打包的，不像 bg 图可以直接从 loadingbg 打开。

Spine 是一款 2D 动画软件，通过将贴图绑定到骨骼文件中，然后再控制骨骼就可以实现不同的动作效果，相对于 live2d 有着更小的内存开支。通常 Spine 模型都需要以下几种文件：.skel, .png, .atlas，表示骨骼、贴图、关联关系。

Spine 必须用相同版本 Spine 编辑器加载，高低版本都不兼容，不过 l2dviewer 好像有这个功能 (这什么生态?)

操作
第一步很像，把 AB/painting, paintingface(差分), dependencies, char 解包（图片大的一批，笔者电脑 OOM 蓝屏了，按需解包），导出全部资产（拼接完整静止 cg 只需要 Mesh 和 Texture2D）

然后我们有两个子任务：1. 把纹理的散装立绘拼成一个图片；2. 复原 spine（动态立绘）

首先讲拼图片。ALPE(azurlane-doujin/AzurLanePaintingExtract-v1.0) 可以利用 Mesh 和 Texture 复原大约一半的内容。有时候会复原出人物和背景两个部分，有时候会复原出更大块的散装人体。这是因为 ALPE 依赖的 json 清单(ALPL) 是手写的...... 作者燃尽了弃更了，所以 ALPE 只能复原一半。

剩下的一半怎么办？ALPA(Deficuet/AzurLanePaintingAnalysis-Kt) 实现了自动化分析方法，可以从 AB/painting, paintingface, dependencies 分析图片的相对位置，拼完最后一部分。注意，ALPA 的立绘根目录是 ALPE 的结果目录，并且需要把所有 jpg 放到一起。

---

没找到复原 spine 的教程，摸索了一下。

动态立绘在 AB/spinepainting 下，和前几个一样先 dump 出来然后解包，得到的文件结构如下：

├───MonoBehaviour
│       ItemList.json
│       NoDrawingGraphic #228.json
│       NoDrawingGraphic #234.json
│       NoDrawingGraphic.json
│       RawImage #107.json
| ....
│       RawImage #98.json
│       RawImage.json
│       SkeletonGraphic.json
│       tuzuo_3_Atlas.json
│       tuzuo_3_SkeletonData.json
│
├───TextAsset
│       tuzuo_3.atlas
│       tuzuo_3.skel
│
└───Texture2D
        tuzuo_3.png
        tuzuo_32.png
json 全是 u3d 的格式用不了，但是已经有关键的纹理、骨骼和关联文件了。

在 L2dViewer 打开目录，程序会自动导入所有文件（哪个目录其实不重要，因为不从 json 打开不会初始化工程）。新建配置文件，选择 skel, atlas, png 文件即可。

一样的问题是动作组没了，但有一个默认动画。检索了一下字符串应该是 skel 自带了一个默认动作 normal。



​
