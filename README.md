# 简介

YfKitLibrary包含了云帆加速提供的Android直播推流、连麦、短视频、播放器等多种组件。通过不同组件之间的相互配合，可以满足目前市面上绝大多数需求。

# 集成方法：

## 1、选择所需组件
对于不同的使用场景，我们提供了更细致的库的区分，以最优化sdk库的大小。
所有库说明如下：

---

SDK | 简称 | 说明
---|---|---
YfPlayerKit | 播放库 | 播放引擎主体部分，需结合对应ffmpeg使用
YfEncoderKit | 编码库 | 编码（推流)引擎主体部分，需结合对应ffmpeg及mux使用
YfAuthentication | 鉴权库 | 必选，所有库的使用都需要通过鉴权
YfK2Pagent | UDP库 | 可选，可用于推流及播放
YfAR | AR库 | 可选，配合推流实现AR贴纸特效
YfVR | VR库 | 可选，配合播放库播放VR/全景视频
YfFilter | 滤镜（特效）库 | 内置于YfEncoderKit，也可单独使用
YfLinkKit | 连麦库 | 可选，Yf连麦调度库
FFMpeg-decoder | 解码ffmpeg库 | 仅包含解码的ffmpeg库
FFMpeg-decoder-encoder | 编解码ffmpeg库 | 包含解码、编码的ffmpeg库
FFMpeg-decoder-https | https解码ffmpeg库 | 包含https的解码ffmpeg库
FFMpeg-decoder-encoder-https| https编解码ffmpeg库 | 包含https的编码、解码ffmpeg库
MuxerLib-stream| 直播mux库 | 用于直播推流的混流库
MuxerLib-editor| 直播、编辑mux库 | 内置视频编辑、直播推流功能的混流库
MuxerLib-link| 直播、连麦mux库 | 内置回声消除、直播推流的混流库

***其中，播放库是必须自选至少一个ffmpeg库配合使用的，编码推流库则必须自选一个ffmpeg及muxer库配合使用。***

---

场景 | SDK | 可选
---|---|---
播放 | 播放库+鉴权库+解码ffmpeg库| VR/UDP/https解码ffmpeg
推流 | 推流库+鉴权库+编解码ffmpeg库|  VR/UDP/连麦mux库（耳返功能）
推流+播放 | 播放库+推流库+鉴权库+编解码ffmpeg库+直播mux库 |VR/UDP/https编解码ffmpeg/AR
推流+播放+短视频| 播放库+推流库+鉴权库+编解码ffmpeg库+直播编辑mux库 | 	VR/UDP/https编解码ffmpeg/AR
推流+播放+连麦 | 播放库+推流库+鉴权库+编解码ffmpeg库+直播连麦mux库+连麦库 | VR/UDP/https编解码ffmpeg/AR

---

如有除了上述场景外的使用需求，可与我们联系定制化裁剪SDK。

## 2 、集成方式

### a）下载sdk到本地集成
用户可以直接通过github下载库，只需将所需要的module内的lib内容复制到项目对应目录即可。

### b）添加依赖的方式集成
用户可以直接通过在项目添加依赖的方式集成对应的sdk。
该项目使用的仓库是JitPack，集成首先需要在project的gradle.build中添加以下maven路径，代码如下：

```
    allprojects {
		repositories {
			...
			maven { url 'https://www.jitpack.io' }
		}
	}
```

然后在对应的module中的gradle.build中添加所需的库依赖即可，以短视频需求集成为例，所需库组成为**播放库+推流库+鉴权库+编解码ffmpeg库+直播编辑mux库**，并加选AR组件，则示例代码如下：

```
    compile 'com.github.YfCloudKit.YfKitLibrary:FFMpeg-decoder-encoder-https:-SNAPSHOT'
    compile 'com.github.YfCloudKit.YfKitLibrary:MuxerLib-editor:-SNAPSHOT'
    compile 'com.github.YfCloudKit.YfKitLibrary:YfAR:-SNAPSHOT'
    compile 'com.github.YfCloudKit.YfKitLibrary:YfAuthentication:-SNAPSHOT'
    compile 'com.github.YfCloudKit.YfKitLibrary:YfEncoderKit:-SNAPSHOT'
    compile 'com.github.YfCloudKit.YfKitLibrary:YfFilter:-SNAPSHOT'
    compile 'com.github.YfCloudKit.YfKitLibrary:YfMediaPlayer:-SNAPSHOT'
```

其中，-SNAPSHOT代表我们上传至github的最新版本，为了保证稳定，建议使用我们在github上发布的release版，代码示例如下：
```
    compile 'com.github.YfCloudKit.YfKitLibrary:YfMediaPlayer:v1.0.3'
```

每次发布release版本，我们也会同步放出release note，大家可以通过点击Github上的Release标签查看版本日志。

下面是以版本v1.0.3为例，各库的依赖集成路径

--- 

SDK | 路径
---|---
YfPlayerKit | com.github.YfCloudKit.YfKitLibrary:YfMediaPlayer:v1.0.3
YfEncoderKit | com.github.YfCloudKit.YfKitLibrary:YfEncoderKit:v1.0.3
YfAuthentication | com.github.YfCloudKit.YfKitLibrary:YfAuthentication:v1.0.3
YfK2Pagent | com.github.YfCloudKit.YfKitLibrary:YfK2Pagent:v1.0.3
YfAR |  com.github.YfCloudKit.YfKitLibrary:YfAR:v1.0.3
YfVR |  com.github.YfCloudKit.YfKitLibrary:YfVR:v1.0.3
YfFilter | com.github.YfCloudKit.YfKitLibrary:YfFilter:v1.0.3
YfLinkKit | com.github.YfCloudKit.YfKitLibrary:YfLinkKit:v1.0.3
FFMpeg-decoder | com.github.YfCloudKit.YfKitLibrary:FFMpeg-decoder:v1.0.3
FFMpeg-decoder-encoder | com.github.YfCloudKit.YfKitLibrary:FFMpeg-decoder-encoder:v1.0.3
FFMpeg-decoder-https | com.github.YfCloudKit.YfKitLibrary:FFMpeg-decoder-https:v1.0.3
FFMpeg-decoder-encoder-https| com.github.YfCloudKit.YfKitLibrary:FFMpeg-decoder-encoder-https:v1.0.3
MuxerLib-stream| com.github.YfCloudKit.YfKitLibrary:MuxerLib-stream:v1.0.3
MuxerLib-editor|com.github.YfCloudKit.YfKitLibrary:MuxerLib-editor:v1.0.3
MuxerLib-link| com.github.YfCloudKit.YfKitLibrary:MuxerLib-link:v1.0.3