# Vorbis注释

Vorbis注释是一种基础元数据格式，最初是为Ogg Vorbis创建的。此后，其他Xiph.Org编解码器(包括Theora、Speex和FLAC)的Ogg封装规范也采用了该格式。

Vorbis注释的用例如下

> ......就像有人在 CDR 底部快速记录一样。它应该是用来记忆光盘和向他人解释光盘的一点信息；一个简短、切题的文字注释，不要求只有几个字，但也不会超过一小段。

Vorbis注释通常用于提供作品名称和版权持有者等基本信息。因此，其范围与MP3文件中使用的ID3标签类似。便携式Ogg Vorbis播放器以及流媒体、编辑和播放软件广泛支持Vorbis注释。

尽管Vorbis注释的语法已得到很好的规范，但使用中的字段名却有各种约定。本页面的目的是编纂最佳实践，并收集有关Vorbis注释字段名标准化的建议。

Vorbis注释通常作为编解码器数据流中的第二个数据包进行编码。当Vorbis注释包含在Ogg Theora文件的第一个(即Theora)数据流中时，它们会被认为覆盖了多路复用组中的所有数据流。

Vorbis注释是使用Xiph.Org编解码器存储元数据的最简单、支持最广泛的机制。有关其他现有和建议的机制，请参阅元数据。

## 备注

**本节为译者添加。**

**翻译自：**

+ <https://wiki.xiph.org/index.php/VorbisComment>

**译者非音乐或数字多媒体行业专业人士，组织此文档仅用于软件开发，部分机翻错误已纠正，若有专业术语翻译错误或不合理的情况，请提交拉取请求或issue。**

**译者不对本文作任何保证，包括但不限于行文描述准确性的保证，读者应自行评判准确性并对可能因参考本文造成的错误承担一切责任。**

# 目录

+ [1.推荐字段名](#1推荐字段名)
+ [2.建议的字段名](#2建议的字段名)
  + [2.1.封面](#21封面)
    + [2.1.1.元数据块图片](#211元数据块图片)
      + [2.1.1.1.一般编码解码](#2111一般编码解码)
      + [2.1.1.2.图块处理](#2112图块处理)
      + [2.1.1.3.链接图像](#2113链接图像)
      + [2.1.1.4.图片尺寸字段](#2114图片尺寸字段)
      + [2.1.1.5.多个图块](#2115多个图块)
      + [2.1.1.6.播放测试](#2116播放测试)
    + [2.1.2.非官方`COVERART`字段(已废弃)](#212非官方coverart字段已废弃)
      + [2.1.2.1.转换为`METADATA_BLOCK_PICTURE`](#2121转换为metadata_block_picture)
  + [2.2.章节扩展](#22章节扩展)
  + [2.3.日期和时间](#23日期和时间)
    + [2.3.1.ISO建议](#231iso建议)
  + [2.4.编码器](#24编码器)
    + [2.4.1.建议：在`ENCODER`值中包含URL](#241建议在encoder值中包含url)
    + [2.4.2.建议：`ENCODED_BY`](#242建议encoded_by)
  + [2.5.改进许可证数据](#25改进许可证数据)
    + [2.5.1.新的`RIGHTS`字段名建议](#251新的rights字段名建议)
    + [2.5.2.改进现有字段的建议](#252改进现有字段的建议)
  + [2.6.地理位置字段](#26地理位置字段)
  + [2.7.重放增益](#27重放增益)
  + [2.8.Tantalos资源ID](#28tantalos资源id)
+ [3.其他(非建议字段名称)](#3其他非建议字段名称)
  + [3.1.VCLT播放列表格式](#31vclt播放列表格式)
+ [4.实现](#4实现)
+ [5.版权](#5版权)

# 1.推荐字段名

当前的[Vorbis注释建议](http://xiph.org/vorbis/doc/v-comment.html)包含一组建议的注释字段名。

# 2.建议的字段名

关于额外字段名的一些建议：

+ [Ogg Vorbis注释字段建议](http://age.hobba.nl/audio/mirroredpages/ogg-tagging.html)
+ [扩展Ogg Vorbis注释的建议](https://web.archive.org/web/20120429102447/http://reallylongword.org/vorbiscomment/)
+ [字段名称](https://wiki.xiph.org/Field_names)

注释本应是自由格式的，但为了实现互操作性，为特定应用定义标签集并为机器解析提供一些指导是有帮助的。请注意，为了实现机器解析，某些字段名称必须是非自由格式的。

## 2.1.封面

### 2.1.1.元数据块图片

[二进制FLAC图片结构](http://flac.sourceforge.net/format.html#`METADATA_BLOCK_PICTURE`)经过Base64编码后，被放置在一个标签名为``METADATA_BLOCK_PICTURE``的Vorbis注释中。这是在Vorbis注释中嵌入封面图片的首选和推荐方式。它有以下优点：

+ 由于FLAC和MP3也使用相同(或类似)的结构，因此开发人员可以轻松使用
+ 封面既可以链接到流中，也可以嵌入到流中
+ 支持常见的图片文件格式(jpg和png)
+ 可包含说明，并提供图片类型(封面、封底、……)和图片MIME类型
+ Base64编码数据在UTF-8和有效UTF-8字符串下不变，因此遵守注释数据的规则

**解释或编写图片块的实现应注意以下细节：**

#### 2.1.1.1.一般编码/解码

+ 解码图片块失败不应妨碍文件的播放(无法处理注释头所需的特别大的数据包是播放器实现的另一个问题)
+ Base64编码的使用与[RFC4648](http://www.faqs.org/rfcs/rfc4648.html)第4节相同。我们注意到不允许换行，而且需要使用填充字符('=')
+ 添加图片块的应用程序应告知用户某些应用程序或硬件可能不支持图片块，并应提供移除图片块的方法(对于能够添加图片块的应用程序来说，移除图片块是轻而易举的)

#### 2.1.1.2.图块处理

+ 未编码格式为FLAC图块格式。字段按FLAC中的大端顺序存储，图片数据按相关标准存储
+ 图片数据应以PNG或JPEG格式存储，或单独链接。建议阅读器同时支持PNG和JPEG格式
+ MIME字符串的允许值为`"image/"`、`"image/png"`、`"image/jpeg"`、`"-->"`(链接指示符)和`""`(长度为0)。空MIME字符串表示类型为`"image/"`
+ ID3v2.4.0附加图片帧(APIC帧)中出现的字段与ID3v2.4.0 格式中的解释相同，但有以下例外(沿用FLAC格式)：
  + 描述字段为UTF-8(编码时不含ID3v2的"初始编码"字节)
  + 字符串字段不以空值结束：而是使用其前面的长度字段

#### 2.1.1.3.链接图像

处理图片块的应用程序可选择是否支持链接图片。表示链接图片时应遵守以下规则：

+ 图片数据是一个完整的URL，表示要使用的图片，允许使用相对URL(注意相对URL不以协议指定符开头，而是使用与正在处理的文件相同的协议进行检索)。
+ 链接采用ISO-8859-1编码
+ 应用程序可通过`file://`协议检索链接的图像
+ 如果应用程序希望通过远程协议检索图像，则必须获得用户批准
+ 链接目标可能不可用：支持链接图像的应用程序应能从容应对，并可向用户报告不可用的情况
+ 链接文件的类型不限于JPEG和JFIF，应用程序可以支持其他格式
+ 如果应用程序不支持链接图像、目标不可用、不允许或格式未知，则应跳过图片块
+ 应用程序可向用户提供链接，这在不支持链接或链接类型不支持的情况下特别有用

#### 2.1.1.4.图片尺寸字段

+ 高度、宽度、色深和颜色数字段纯粹用于提供信息。应用程序不得将其用于解码目的，但可以将其显示给用户，并可以用其决定是否跳过图块(例如，在多个图块中选择最合适的一个)
+ 编写图块的应用程序必须正确设置这些字段，或者将它们全部设置为零

#### 2.1.1.5.多个图块

+ 多个图片块可以作为单独的`METADATA_BLOCK_PICTURE`注释包含
+ 在一个Vorbis数据流中，图片类型(APIC类型)1和2(**译者注** 即封面和封底)只能各有一个
+ 图块顺序对某些类型非常重要，应用程序在读写Vorbis注释标头时应保留注释顺序。区块顺序可用于确定图片呈现给用户的顺序。

#### 2.1.1.6.播放测试

将图片嵌入文件可能会破坏现有播放器(尤其是硬件播放器，软件播放器可以很容易地更新)的播放。解决方法是在标签中链接图片。此外，还应以某种方式告知用户，嵌入图片可能会导致问题(如上所述)。

为了测试是否存在播放问题，[此处](http://www.audioranger.com/coverart_mk.ogg)和[此处](http://www.audioranger.com/coverart_im.ogg)提供了测试文件。请下载其中一个测试文件(或两个都下载)，在软件和硬件播放器上测试播放效果，并在维基上报告结果。

**测试过的的软件播放器：**

+ Audacious 1.5.1：没有问题
+ foobar2000：没有问题
+ Gnome：内置预览播放：没有问题
+ MediaMonkey: 没有问题
+ Media Player Classic (unicode build) 6.4.9.1：没有问题
+ MPlayer 1.1：没有问题
+ RoarAudio：没有问题(服务器和客户端)
+ Rythmbox 0.11.6：没有问题
+ Totem 2.24.3：没有问题
+ VLC 0.9.4/0.9.6：无法播放
  + 已向 VLC 发送补丁以修复此问题 - 应在1.0.0中发布
+ WinAmp: 没有问题
+ Windows Media Player 11：没有问题
+ XMMS 1.2.11：没有问题
+ XMPlay 3.4.2：没有问题
+ Nero ShowTime：没有问题
+ Songbird 1.8.0：没有问题，能够显示和编辑嵌入的图片

**测试过的硬件播放器：**

+ 罗技Squeezebox：截至2009年1月支持(服务器版本7.3.3)
+ Sandisk Sansa Fuze(固件01.01.22)：尝试播放演示文件时挂起 - 不得不重置播放器
  + 注："Fuze"可播放嵌入了"Easytag"图片的Ogg Vorbis文件
+ Cowon iAudio U3(固件1.29，4GB)：没有问题
+ Cowon D2：没有问题(最新固件：2.59，8GB版本)
+ iRiver E100：没有问题(最新固件：1.16 G_U，8GB版本)
+ iRiver T20：无法播放(固件：1.71，1GB 版本)
+ Samsung YP-R1：没有问题(最新固件：3.07，16GB版本)

**测试过的标签编辑器：**

+ Easytag 2.1.6：可打开文件编辑普通标签字段
+ MP3Tag 2.42e：可打开文件编辑普通标签字段
+ MP3Tag 2.47b：可显示和编辑嵌入的图片

**测试过的其他软件：**

+ Total Recorder：能根据规范处理封面

### 2.1.2.非官方`COVERART`字段(已废弃)

还有一个名为`COVERART`的非官方、不受支持的注释字段。它包括一个二进制图片数据(通常是JPEG文件，但也可能是其他文件格式)的Base64编码字符串。缺点是

+ 不提供其他信息，如封面艺术描述或其类型(封面、封底等)、
+ 不能链接封面设计
+ 由于许多标签编辑器不支持`COVERART`字段，Base64字符串在这些编辑器中显示为纯文本
+ 由于Vorbis注释头较大，可能会影响硬件播放器的播放效果

非官方的`COVERART`字段支持的软件有[AudioShell](http://www.softpointer.com/AudioShell.htm) - 读/写，以及[Total Recorder](http://www.totalrecorder.com/) - 只读。

#### 2.1.2.1.转换为`METADATA_BLOCK_PICTURE`

旧的`COVERART`标记应转换为新的`METADATA_BLOCK_PICTURE`标记(其规格见上文)。这种转换很简单，建议按以下方式进行：

+ 解码`COVERART`标记。程序可以检查嵌入图片的签名，以确定它是否是允许的类型。可以将不允许的类型无损转换为允许的类型。
+ 用二进制图片数据填写FLAC块。如果图片的MIME类型未知或无法确定，可以使用MIME类型`"image/"`代替。提供图片尺寸、色深等信息是可选项(见上文说明)。
+ 如果没有其他信息，则应使用图片类型"其他"。应用程序可能希望允许用户选择默认类型或指定使用的类型。
+ 对新图片块进行编码，从注释中移除`COVERART`标记并添加`METADATA_BLOCK_PICTURE`条目。
+ 如果要转换多个标记，`METADATA_BLOCK_PICTURE`标记的顺序应与它们要替换的`COVERART`标记的顺序相同。

## 2.2.章节扩展

这些标记用于包含导航点，例如

```
CHAPTER001=00:00:00.000
CHAPTER001NAME=Chapter 1
CHAPTER001URL=http://...
```

更多详情，请参阅[章节扩展](https://wiki.xiph.org/Chapter_Extension)页面。

## 2.3.日期和时间

目标是指定一种描述日期和/或时间的标准格式。

### 2.3.1.ISO建议

任何描述日期的字段的日期格式都必须遵循ISO 8601标准：`YYYY-MM-DD`，可简化为`YYYY-MM`或`YYYY`。

我们建议使用DATE标签已经有一段时间了，建议对规范进行修订，纳入这一信息，以便于加工。

除轨道持续时间外，任何字段的时间格式都必须以`"T"`开头，并以时区结尾。有日期和无日期的模式：

```
yyyy-mm-ddthh:mm:ss+ts
```

```
THH:MM+TZ
```

## 2.4.编码器

目的是确定编码器软件的属性。将来可以使用该值来确定哪些文件可以通过使用较新版本重新编码而得到改进。

> **评论**：从一开始就存在于规范中的供应商字符串缺少了什么？据我所知，所有libvorbis和编码器调整都在这里记录了编码器版本。

不使用供应商字符串的理由：

+ 供应商字符串通常用于存储底层编解码器库的名称和版本。
+ `ENCODER`的目的是存储用户可见应用程序的名称，例如ffmpeg2theora。
+ 存储调用应用程序的名称和版本对调试很有用。
+ libvorbis API不允许应用程序覆盖供应商字符串。

### 2.4.1.建议：在`ENCODER`值中包含URL

编码器字段名称必须是提供编码器软件名称和版本的唯一URL。如果没有同时提供名称和版本的唯一URL地址，则可以用空格分隔指定版本号。例如

```
ENCODER=http://flac.sourceforge.net/ 1.2.1
```

请注意，ffmpeg2theora使用`ENCODER`，但不包含URL。*由Rillian于2007年9月17日添加*

### 2.4.2.建议：`ENCODED_BY`

我也见过`ENCODED_BY`。*由Rillian于2007年9月17日添加*

`ENCODED_BY`通常是指进行编码的人。由于故意或意外向第三方传播会产生法律问题，因此这不应成为建议的一部分。基本上，不应该包含编码者的名字，以保护编码者的自尊和可能的法律起诉。*由Aleksandersen于2007年9月20日添加*

## 2.5.改进许可证数据

我们的目标是提供一种方法来宣布许可证和版权信息(基本上是澄清"分发权(如果有的话)和所有权")。

[规范文档](http://xiph.org/vorbis/doc/v-comment.html)描述了`LICENSE`和`COPYRIGHT`字段。但对于这些字段是否应为机器可读字段还不够明确。

我们应考虑与知识共享组织(Creative Commons)合作，在知识共享组织和Xiph维基上提供互补和相互链接的信息。请参考知识共享维基中的Ogg页面。

### 2.5.1.新的`RIGHTS`字段名建议

一项建议是用`RIGHTS`代替`COPYRIGHT`和LICENSE字段名。`RIGHTS`必须是人类可读的版权声明。示例：

```
RIGHTS=Copyright © Recording Company Inc. All distribution rights reserved.
```

但这不是机器可读的。添加两个补充字段名就可以解决这个问题：`RIGHTS-DATE`——描述版权日期；`RIGHTS-URI`——提供链接到许可证的方法。软件代理可以假定多首歌曲使用相同的RURI，例如Creative Commons的情况。完整示例：

```
RIGHTS=Copyright © 2019 Recording Company Inc. All distribution rights reserved.
RIGHTS-DATE=2019-04
RIGHTS-URI=http://somewhere.com/license.xhtml
```

鼓励多媒体管理和播放等软件使用`RIGHTS-URI`将权利声明作为链接短语显示。

无需显示`RIGHTS-DATE`，因为国际版权协议要求在人可读版本中显示`RIGHTS-DATE`。`RIGHTS-DATE`可用于确定版权作品何时属于公有领域及相关事宜。(披头士乐队原录音室录音(非混音版)的版权即将到期。因此，音乐管理和文件共享软件确实需要`RIGHTS-DATE`这样的机制！)

为了保持机器可读性，每个`RIGHTS`字段名称最多只能有一个实例。当然，所有字段仍然是可选的。

都柏林核心元数据倡议使用`RIGHTS`来描述许可和版权事项。网络馈送格式Atom 1.0在其规范中实施了`RIGHTS`元素。

> **评论** 三元组`RIGHTS`、`RIGHTS-DATE`、`RIGHTS-URI`是结构化元数据的一个例子。Vorbis注释本身就是非结构化的，这一点应该得到尊重。结构化元数据属于不同的数据流，如XML(使用[XMLEmbedding](https://wiki.xiph.org/Metadata#XMLEmbedding))。

### 2.5.2.改进现有字段的建议

与上面的`DATE`标签类似，我们一般建议在`LICENSE`字段中包含一个唯一标识许可证的URL，以便机器识别许可证。这与知识共享维基中的建议一致。由于`COPYRIGHT`字段与上述建议的`RIGHTS`标签一样，都是人类可读的版权声明，因此有些人会在该字段中包含许可URL。因此，如果在`LICENSE`标记(如果有的话)中找不到URL，应用软件应使用`COPYRIGHT`标记(如果有的话)中的URL。用于验证、署名、再授权等的联系信息可以从`COPYRIGHT`字段中获取，但知识共享组织也建议使用单独的`CONTACT`标签来处理这些信息。这是合理的，因此我们建议将其包括在内。

## 2.6.地理位置字段

现有的`LOCATION`字段用于记录/创建媒体文件时的人类可读位置。

根据WGS84编制的地理坐标也很有用，尤其是可以用机器解析的形式。商定的格式与此地理微格式类似：

```
GEO_LOCATION=纬度;经度[;高度]
```

其中，每个值都是一个定点十进制数，用C语言格式化，弧度用句点(.)表示。数值之间用";"分隔，空格不重要。海拔高度为可选项。

纬度(latitude)是媒体记录或制作地点的地理纬度，根据WGS84以十进制度表示(赤道为0，南纬为负值)(C double)。

经度(longitude)是媒体记录或制作地点的地理经度，按WGS84标准十进制表示(格林威治/英国本初子午线为零，西经为负值)。(C double)。

海拔高度(elevation)是媒体录制或制作地点的地理海拔高度，根据WGS84，单位为米(0表示平均海平面)(C double)。

## 2.7.重放增益

`REPLAYGAIN_*`字段实现了存储音轨相对音量调整的重放增益(Replay Gain)建议，可用于"修复"声音小或大的Vorbis或FLAC流。这组标记可由机器解析，其形式如下：

```
REPLAYGAIN_TRACK_GAIN=-7.03 dB
REPLAYGAIN_TRACK_PEAK=1.21822226
REPLAYGAIN_ALBUM_GAIN=-6.37 dB
REPLAYGAIN_ALBUM_PEAK=1.21822226
```

有关重放增益以及如何计算不同值的详细信息，请参阅<http://www.replaygain.org/>。

## 2.8.Tantalos资源ID

Tantalos是一种在网络范围(通常为"局域网"，但也可以更大，如"公司网络")内自动定位和访问内容的协议。它使用UUID来标识资源。这些UUID有两组： 使用轨道元数据生成的ID和以其他方式生成的ID(例如随机ID，参见UUID规范)。后一组UUID可能需要与音轨一起传递。VCLT播放列表格式(见下文)使用`HASH={UUID}id...`和十六进制横杠格式(`HASH={UUID}e278173d-4d6d-4c66-95ec-4ec85eedc7d1`)。*--[Ph3-der-loewe](https://wiki.xiph.org/User:Ph3-der-loewe)太平洋标准时间2011年12月13日 02:05*

# 3.其他(非建议字段名称)

## 3.1.VCLT播放列表格式

VCLT播放列表格式使用了一些看起来像Vorbis注释的键，但它们现在不是、将来也不会变成建议使用的名称(HASH 除外)。其中包括STREAMURL、FILENAME、FILEURL、LENGTH、HASH(见上文)、SIGNALINFO、AUDIOINFO和OFFSET。有关这些键的更多信息，请访问<https://bts.keep-cool.org/wiki/Specs/VCLT。>

# 4.实现

+ OggImporter - 将Ogg Vorbis和Ogg FLAC文件导入Spotlight
+ vorbiscomment – 命令行工具，是[VorbisTools](https://wiki.xiph.org/VorbisTools)的一部分
+ oggz-comment - 命令行工具，是[Oggz](https://wiki.xiph.org/Oggz)工具的一部分。它可以为多轨文件和视频文件添加注释

# 5.版权

**Content is available under CC BY 3.0 and revised BSD unless otherwise noted.**