# APE标记

## 备注

**翻译自：**

+ <https://wiki.hydrogenaud.io/index.php?title=APEv2_specification>
+ <https://wiki.hydrogenaud.io/index.php?title=APE_Tags_Header>
+ <https://wiki.hydrogenaud.io/index.php?title=Ape_Tags_Flags>
+ <https://wiki.hydrogenaud.io/index.php?title=APE_Tag_Item>
+ <https://wiki.hydrogenaud.io/index.php?title=APE_key>
+ <https://wiki.hydrogenaud.io/index.php?title=APE_Item_Value>
+ <https://wiki.hydrogenaud.io/index.php?title=APE_date>
+ <https://wiki.hydrogenaud.io/index.php?title=APE_time_stamp>

**译者非音乐或数字多媒体行业专业人士，组织此文档仅用于软件开发，部分机翻错误已纠正，若有专业术语翻译错误或不合理的情况，请提交拉取请求或issue。**

**译者不对本文作任何保证，包括但不限于行文描述准确性的保证，读者应自行评判准确性并对可能因参考本文造成的错误承担一切责任。**

## APE标记概述

APEV2的结构如下

|名称|大小|
|-|-|
|[标签头](#标签头)|32B|
|[项目1](#项目)|nB|
|[项目2](#项目)|nB|
|...|...|
|[项目n](#项目)|nB|
|[标签尾](#标签头)|32B|

APEV1没有标签头。强烈建议以APEV2取代APEV1。

APE标签项目应按大小升序排列。在流式传输时，可以去掉部分APE标记，以减少标题之间出现脱落的危险。这不是必须的，但强烈建议这样做。实际上，项目应按重要性/字节排序，但这并不可行。只有在添加了不太重要的小项目，又不想重写整个标签时，才可以打破这一规则。位于文件末尾的APEv1标签必须至少有一个标签尾，APEv1标签不能用于文件的开头(与APEv2标签不同)。当位于MP3文件末尾时，APE标记应放在最后一帧之后，ID3v1标记(如果有)之前。

APE标签使用小端序。

## 标签头

|名称|大小|备注|
|-|-|-|
|起始标记|8B|`{'A','P','E','T','A','G','E','X'}`|
|版本号|4B|`1000` = 版本1.000；`2000` = 版本2.000|
|标签大小|4B|因需与APEV1兼容，此字段指示的大小不包含标签头|
|项目数|4B||
|[标志](#标志)|4B||
|保留|8B|必须为`0`|

## 项目

|名称|大小|备注|
|-|-|-|
|值长度|4B|值的长度，以字节为单位|
|[标志](#标志)|4B||
|[键](#键)|nB|包含ASCII可打印字符|
|键结束符|`1`B|`0x00`|
|[值](#值)|`m`B||

## 标志

|位|含义|
|-|-|
|31|`0` = 不含标签头，`1` = 含标签头|
|30|`0` = 不含标签尾，`1` = 含标签尾|
|29|在标签头中设置此位|
|28 - 3|未用，必须为`0`|
|2 - 1|`0` = 值为UTF-8编码的字符串，`1` = 值为二进制数据，`2` = 值为外部存储信息的链接(链接也为UTF-8编码)，`3` = 保留|
|0|`0` = 标签可读可写，`1` = 标签只读|

APEV1不使用标志，所有的位必须为`0`。

> 下面是允许的链接形式：
>
> http://host/directory/filename.ext
>
> ftp://host/directory/filename.ext
>
> filename.ext
>
> /directory/filename.ext
>
> DRIVE:/directory/filename.ext

## 键

键的长度为2(包括)至255(包括)个字符，范围从0x20(空格)至0x7E(浪纹线)。

典型的键长度应为2-16个字符，使用以下字符：
空格(0x20)、斜线(0x2F)、数字(0x30-0x39)、字母(0x41-0x5A、0x61-0x7A)。

不允许使用这些键名： `ID3`、`TAG`、`OggS`和`MP+`。

当前定义了以下键：

|键名|含义|建议的值格式|
|-|-|-|
|Title|标题|UTF-8字符串|
|Subtitle|副标题|UTF-8字符串|
|Artist|艺术家|UTF-8字符串(或列表)|
|Album|专辑|UTF-8字符串|
|Debut album|首发专辑|UTF-8字符串|
|Publisher|唱片公司或出版商|UTF-8字符串|
|Conductor|指挥者|UTF-8字符串|
|Track|音轨号<br>音轨号/总音轨数|整数<br>整数/整数|
|Composer|作曲、编曲|UTF-8字符串(或列表)|
|Comment|备注|UTF-8字符串(或列表)或链接(或列表)|
|Copyright|版权方|UTF-8字符串(或列表)|
|Publicationright|出版权持有人|UTF-8字符串(或列表)|
|File|文件位置|链接(或列表)|
|EAN/UPC|EAN-13或UPC(欧洲物品编码-13或通用产品代码)条形码|12或13位整数|
|ISBN|带校验码的ISBN(国际统一书号)|9位整数+1位校验码(0-9或X)|
|Catalog|目录编号|有时在EAN或UPC中，常为字母后跟数字|
|LC|标签代码|`"LC"`+4或5位整数|
|Year|发布年份|日期|
|Record Date|录音日期|日期|
|Record Location|录音地点|UTF-8字符串(或列表)|
|Genre|流派|UTF-8字符串(或列表)|
|Media|来源媒体<br>来源媒体编号/总媒体数<br>来源时间|媒体<br>媒体 整数/整数<br>媒体 时间|
|Index|供快速查找用的索引|索引时间|
|Related|相关信息链接|链接(或列表)|
|ISRC|国际标准录音制品编码|ASCII字符串|
|Abstract|摘要|链接|
|Language|语言|UTF-8字符串(或列表)|
|Bibliography|书目或文献|链接|
|Introplay|前奏特色部分|索引时间范围|
|Dummy|占位符|`0x00`字节序列|

**以下内容为译者添加。**

|键名|含义|建议的值格式|
|-|-|-|
|Cover Art (*)|封面|UTF-8编码的字符串描述 + `0x00` + 图片数据。其中*为图片类型，如`"Cover Art (Front)"`或`"Cover Art (Back)"`|
|Performer|表演者/合作/客串|UTF-8字符串(或列表)|
|**(foobar2000)** <br> REMIXED BY <br> **(Mp3tag)** <br> MIXARTIST|重混音者|UTF-8字符串(或列表)|
|**(foobar2000)** <br> ORIGINAL ARTIST <br> **(Mp3tag)** <br> ORIGARTIST <br> **(Winamp)** <br> Orig. Artist| 原艺术家|UTF-8字符串(或列表)|
|**(foobar2000/Winamp)** ALBUM ARTIST <br> **(Mp3tag)** <br> ALBUMARTIST|专辑艺术家|UTF-8字符串(或列表)|
|Musicbrainz_albumtype|专辑类型|UTF-8字符串(或列表)|
|**(foobar2000)** <br> ARTISTSORTORDER <br> **(Mp3tag)** <br> ARTISTSORT|艺术家排序替代|UTF-8字符串|
|**(foobar2000)** <br> TITLESORTORDER <br> **(Mp3tag)** <br> TITLESORT|标题排序替代|UTF-8字符串|
|**(foobar2000)** <br> ALBUMARTISTSORTORDER <br> **(Mp3tag)** <br> ALBUMARTISTSORT|专辑艺术家排序替代|UTF-8字符串|
|**(foobar2000)** <br> ALBUMSORTORDER <br> **(Mp3tag)** <br> ALBUMSORT|专辑排序替代|UTF-8字符串|
|TRACKNUMBER|音轨号 **(有些应用程序也用作音轨数，但这不是标准用法)**|整数|
|TOTALTRACKS|总音轨数|整数|
|**(foobar2000)** <br> SET SUBTITLE <br> **(Mp3tag)** <br> SETSUBTITLE|集副标题(光盘副标题)|UTF-8字符串|
|Releasecountry|发行国家|UTF-8字符串(或列表)|
|**(foobar2000)** <br> ORIGINAL RELEASE DATE <br> **(Mp3tag)** <br> ORIGYEAR|原始发布时间|日期|
|Mood|情绪|UTF-8字符串(或列表)|
|ENCODEDBY <br> ENCODED-BY <br> ENCODED BY|编码者|UTF-8字符串(或列表)|
|Mediatype|来源媒体|UTF-8字符串|
|Www <br> Contact <br> **(Winamp)** <br> Url|网站|链接|
|**(foobar2000)** <br> ARTIST WEBPAGE URL <br> **(Mp3tag)** <br> WWWARTIST|艺术家网站|链接|
|**(foobar2000)** <br> PUBLISHER URL <br> **(Mp3tag)** <br> WWWPUBLISHER|发布者网站|链接|
|**(foobar2000)** <br> FILE WEBPAGE URL <br> **(Mp3tag)** <br> WWWAUDIOFILE|下载地址 **(应当使用`"File"`)**|链接|
|**(foobar2000)** <br> COPYRIGHT URL <br> **(Mp3tag)** <br> WWWCOPYRIGHT|版权说明网站|链接|
|**(foobar2000)** <br> PAYMENT URL <br> **(Mp3tag)** <br> WWWPAYMENT|付款链接|链接|
|**(foobar2000)** <br> UNSYNCED LYRICS <br> **(Mp3tag)** <br> UNSYNCEDLYRICS|不同步歌词|UTF-8字符串|

## 值

项值是由键分配的值，可以是二进制数据或UTF-8字符串。

可以使用二进制数据，但不推荐。UTF-8字符串不以0结尾。

虽然内容是自由的，但强烈建议使用下列格式：

|类型|说明|
|-|-|
|整数数字|数字序列，可使用`"-"`字符作为前缀|
|浮点数|带中间点的数字序列，可使用`"-"`字符作为前缀。可以添加指数，以`"e"`或`"E"`开头，后面跟一个整数。|
|日期|日期和时间格式用于处理与播放无关的时间点。通常用于较长的时间段(1-500年)，精度较低(1天-1年)。 <br>它使用ISO-8601作为时间格式，以克服含糊不清的国家格式。<br>格式为"大端"文本格式，就像普通阿拉伯数字一样。左侧数字的权重高于右侧数字。<br>因此，`YYYY-MM-DD hh:mm:ss`的含义如下：`YYYY`表示年，`MM`表示月，`DD`表示日，`hh`表示午夜后的小时数，`mm`表示分钟数，`ss`表示秒数。<br>年为四位数，其他项目为两位数。<br>年、月、日用连字符分隔，时、分、秒用冒号分隔。可以去掉右边的数字，以降低表示精度。<br>另外，要处理一个特殊的星期，格式是`YYYY-Www`，其中`YYYY`是年，`ww`是周。第1周是新年的第一个星期三，周的取值为1-53。<br>下面是示例：<br>1999<br>1999-08<br>1999-08-11<br>1999-W34<br>1999-08-11 12:34<br>1999-08-11 12:34:56|
|时间段|日期...日期|
|字符串|不包含字符`0`、`254`和`255`的任意数据。UTF-8编码。|
|条目列表|条目不像C/C++那样以`0`结尾。如果有零字符，则表示在键下存储多个条目，条目之间用零字符分隔。|
|时间戳|时间戳用于处理编码音频中的某些内容。时间是相对于音频开始的，距离通常较小(几分钟到一两个小时)，精确度可以从1秒到1个采样点。<br>采样精确寻址至少需要4位数(fs<=10kHz)或5位数(fs<=100kHz)。写入的项目必须四舍五入，重新计算采样后再四舍五入。<br>小时和分钟可在秒前使用两个或一个冒号编码。但也可以只写秒。可以在句号(`.`)后添加部分秒。<br>下面是示例：<br>312<br>5:12<br>4:72<br>312.33334<br>5:12.33334<br>312.3<br>3621.4<br>1:00:21.4|
|歌词|[时间戳]文本 换行符<br>[时间戳，表演者]文本 换行符<br>[时间戳,表演者1,表演者2]文本 换行符|
|媒体|CD、DVD、VHS、广播...|
|标题编号|2位或3位非负整数|
|网络链接|如：http://host/directory/filename <br> ftp://host/directory/filename <br> http://host:port/directory/filename <br> ftp://host:port/directory/filename <br> http://user:password@host/directory/filename <br> ftp://user:password@host/directory/filename <br> http://user:password@host:port/directory/filename <br> ftp://user:password@host:port/directory/filename <br>|
|本地链接|如：file:/directory/file <br> file:./relativedirectory/filename <br> file:./filename <br>|
|其他IP链接|协议:标识符|
|电子邮件地址|mailto:用户名@主机名|
|二进制数据|任意数据|
