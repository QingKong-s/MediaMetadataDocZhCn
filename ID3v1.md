# ID3v1

ID3标签技术说明V1.1(1997年12月12日)，作者迈克尔·迈克塞勒(Michael Mutschler) <amiga2@info2.rus.uni-stuttgart.de>，因篇幅和清晰度原因已作编辑。

## 备注

**本节为译者添加。**

**请注意，此文件基于ID3v2.2文档"附录A - ID3标签技术说明V1.1"翻译并添加内容而来，而非文件的原始版本，若要获取原始版本，请访问<http://www.id3.org>。**

**原文链接：<https://id3.org/id3v2-00>**

**若有其他非ID3官网来源，将在其出现的小节列出。**

**译者非专业音乐人士，组织此文档仅用于软件开发，部分机翻错误已纠正，若有专业术语翻译错误或不合理的情况，请提交拉取请求或issue。**

# ID3v1结构

信息存储在MP3的最后128个字节中，有以下字段，此处给出的偏移为0-127。

|字段|长度|偏移|说明|
|-|-|-|-|
|`"TAG"`|3|0-2|如果该字段包含字符串"TAG"，则标签有效。此字段必须大写！|
|标题|30|3-32||
|艺术家|30|33-62||
|专辑|30|63-92||
|年代|4|93-96||
|备注|30|97-126||
|流派|1|127|包含预定义列表中流派的索引，该字节被视为无符号字节。索引从0开始。|

字符串字段包含ASCII数据，以ISO-Latin-1编码。小于字段长度的字符串以零字节填充。

# ID3v1.1改进

在ID3v1.1中，Michael Mutschler修改了注释字段的规范，以实现音轨编号。注释字段的新格式是一个28个字符的字符串，后面是一个强制性空(`$00`)字符和以无符号字节大小整数形式存储的原始专辑曲目编号。在第29个字节不是空字符或第30个字符为空字符的情况下，音轨编号将被视为未定义。

# ID3v1扩展标签

**本节为译者添加，内容来自：<https://zh.wikipedia.org/wiki/ID3>**

扩展标签是位于ID3v1标签前的额外资料区块，其将标题、艺术家与专辑字段各自扩展到60个字节长度，提供可自由输入文字的流派字段、一字节大小的曲速(值为0到5)以及MP3文件的起始与结束时间。如果以上的区域都没有被使用，那么这个资料区块就会自动被省略。

有些支持ID3v1的程序也可以读到扩展标签，不过写入时可能会在扩展区块中留下过旧的值。扩展区块并非官方标准，而且只有少数程序支持，不包含XMMS或Winamp。扩展标签有时也被称作"补强标签"(enhanced tag)。

扩展标签总长227个字节，并且位于ID3v1标签之前。

|字段|长度|偏移|说明|
|-|-|-|-|
|`"TAG+"`|4|0||
|标题|60|4||
|艺术家|60|64||
|专辑|60|124||
|曲速|1|184|`0` 未设置，`1` 慢板，`2` 中板，`3` 快板，`4` 极快|
|流派|30|185|流派名称字符串，可自由输入|
|开始时间|6|215|音乐开始时间，格式为`mmm:ss`|
|结束时间|6|221|音乐结束时间，格式为`mmm:ss`|

# 流派列表

**本节中文名称与备注为译者添加，仅供参考。**

**本节部分内容来自：**

+ <https://zh.wikipedia.org>
+ <https://www.chosic.com/music-genre-finder/>
+ <https://rateyourmusic.com/genres/>

ID3v1中定义了以下流派：

|值|中文名称|英文名称|备注|
|-|-|-|-|
|0|蓝调|Blues|起源于19世纪末，非裔美国人在美国南方腹地及美国各地。蓝调融合了黑人灵歌、工作歌曲、现场大声叫喊及回应、欢呼、吟唱和押韵成简单的民谣。|
|1|经典摇滚|Classic Rock|通常指上世纪60年代到80年代创作的摇滚音乐。这一流派的特点包括强烈的吉他演奏、标志性的主唱以及具有深厚音乐性和情感表达的歌词。|
|2|乡村|Country|起源于美国南部与阿帕拉契山区，融合了传统民谣音乐、凯尔特音乐、福音音乐、蓝调及美国民间音乐。|
|3|舞曲|Dance|用于跳舞时的伴奏，它可能是一首歌曲或其中一部分，或是为了加强节奏感而重新编曲。|
|4|迪斯科|Disco|源于美国黑人民间舞蹈和爵士舞，并在1970年代后风行世界(特别是欧洲)的舞曲。混合了放克、爵士乐、摇滚乐以及拉丁美洲音乐的节奏特色。|
|5|放克|Funk|起源于1960年代中期至晚期，非洲裔美国人音乐家将灵魂乐、灵魂爵士乐和节奏蓝调融合成一种有节奏的、适合跳舞的音乐新形式。|
|6|油渍摇滚|Grunge|又称垃圾摇滚、颓废摇滚、西雅图之声，一种隶属于另类摇滚的音乐流派与亚文化类别，起源于1980年代中期美国西岸的华盛顿州，特别是西雅图一带，是一种将朋克与重金属混合的音乐风格。|
|7|嘻哈|Hip-Hop|是1970年代源自纽约市南布朗克斯与哈林区的非洲裔及拉丁裔青年之间的一种边缘性次文化，继而发展壮大成为新兴艺术型态，并席卷全球。Hip-Hop包含说唱、DJ、地板霹雳舞及涂鸦四大要素。|
|8|爵士乐|Jazz|于19世纪末期至20世纪初期起源于美国新奥尔良的非裔美国人的社区，植根于蓝调和拉格泰姆音乐以及欧洲的军乐，并且由其发展而来。|
|9|金属乐|Metal|又称重金属，兴起于1960年代末期的英国和美国。它发源于蓝调摇滚、迷幻摇滚，再从更猛烈的硬式摇滚中改良演变而来。是一种浑厚、音量大的音乐，特色是强而有力的嗓音、大量高度失真的电吉他、加长而有技术性的吉他独奏、密集快速的鼓点。|
|10|新世纪|New Age|是一种在1970年代出现的音乐形式，最早用于帮助冥思及洁浄心灵，但许多后期的创作者已不再抱有这种出发点。它并非单指一个类别，而是一个范畴，一切不同以往，象征时代更替诠释精神内涵的改良音乐都可归于此内，所以被命名为New Age，即新世纪音乐。|
|11|老歌|Oldies|主要指20世纪50年代到70年代的流行音乐和摇滚音乐。这一类别的音乐通常带有怀旧的意味，并常常在老歌电台播放。|
|12|其他|Other||
|13|流行乐|Pop|流行音乐的一个分支音乐类型。"Pop"一开始是"流行音乐"(Popular Music)的缩写，也是其同义词，不过大约在1954年以后逐渐被用来指称狭义的特定音乐类型。流行乐歌曲特指歌词内容主要以现代潮流为主，例如叙述爱情或生活等，乐器用现代流行的乐器为主。|
|14|节奏蓝调|R&B|一种美国非裔艺术家首先采用，并融合了爵士乐、福音音乐和蓝调音乐的音乐形式。|
|15|说唱|Rap|一种带有节奏与押韵的歌唱技巧及音乐风格。是构成美国1970年代兴起的嘻哈文化中的主要元素之一，也叫作emceeing(司众)，最早是美国黑人在社区街头表达政治和社会思想的途径之一，其起源与口语诗有关，并且大量运用暗语、俗语、俚语等民间口语。|
|16|雷鬼|Reggae|1960年代源自于牙买加的一种音乐类型，也是牙买加或牙买加侨民的流行乐。|
|17|摇滚乐|Rock|是20世纪50年代初起源于"摇滚"(Rock and roll)音乐中的一种广泛音乐类型，音乐格调吸收了非洲裔美国人的布鲁斯和节奏布鲁斯以及乡村音乐的音乐风格，也吸收了一些其他的音乐流派如电布鲁斯、民间音乐、爵士乐和古典音乐的风格。集中聚焦在电吉他，与电贝斯和鼓组及一个或多个歌手的组合，融合了流行乐和摇滚乐的组成。|
|18|高科技舞曲|Techno|又称泰克诺，一种电子舞曲，发源于1980年代中期到晚期的美国密歇根州底特律。乐风成形始于两种音乐的融合：来自白人的欧陆电音子音乐与电声爵士等美国黑人音乐；除此之外，与美国晚期资本主义社会相关，充满未来感的科幻主题也对它影响深远|
|19|工业摇滚|Industrial|融合工业音乐与朋克摇滚的一种音乐类型，通常使用电吉他、鼓和贝斯等乐器，并搭配白噪声、电子音乐设备(合成器、sequencers、取样器和鼓机)。吉他大量使用失真效果或其他的效果器。贝斯和鼓可能会在现场演唱使用，或是由电子音乐器材和电脑代替。工业摇滚常常结合机械和工业音效。|
|20|另类音乐|Alternative||
|21|斯卡|Ska|发源自牙买加 ，本是该地的传统乐风。经过输入及改进后，于1960年代早期，成为美国流行音乐乐坛的一环，也为美国当地拉丁美洲流行音乐的重要一部分。经过改良后，该曲风采用的跳跃式吉他弹法和时而间歇的节奏诠释蓝调乐曲，之后在融入灵魂歌曲后，更成为美国主流音乐乐风。|
|22|死亡金属|Death Metal|是重金属音乐中的一种极限金属乐派。以早期的"黑金属"与"鞭击金属"为背景开始演化而成(受鞭击金属中的Slayer乐团影响)。特点是充斥电吉他快速的反复、几无旋律的和弦、速击狂踩的双大鼓、主唱咬牙不清的低吟狂吼(死腔)、复杂的编曲和多样化的变拍。歌词以死亡仇恨为主。|
|23|恶作剧|Pranks|一种通过使用恶作剧电话和幽默短剧来创作喜剧内容的音乐流派。这类音乐通常以离谱的人物和情节为特色，旨在让听众捧腹大笑。它通常由喜剧演员或喜剧音乐家表演，他们具有即兴表演和机智敏捷的天赋。恶作剧音乐的一些流行例子包括恶作剧电话、隐藏摄像机恶作剧和喜剧小品。|
|24|原声音乐|Soundtrack|又称影视原声音乐大碟、原声带，多指电影、戏剧、动画或电子游戏之配乐。|
|25|欧洲泰克诺|Euro-Techno|一种电子音乐流派，起源于欧洲，特别是在20世纪80年代末和90年代初。这一流派融合了欧洲的电子舞曲元素和美国的Techno音乐风格，形成了独特的声音特征。Euro-Techno以其快节奏、合成器驱动的旋律和强烈的节拍著称，通常用于夜店和舞池。|
|26|氛围音乐|Ambient|又称气氛音乐、环境音乐，是在传统音乐结构或韵律之上更着重于色调和氛围的一种音乐类型，拥有一种"环境的"、"视觉的"， 或者"不被人察觉的"特质，其与流行、商业化的音乐比起来大多更加冗长。|
|27|痴哈|Trip-Hop|是一种缓拍电子音乐流派，起源于20世纪90年代的英国，尤其是英国布里斯托。Trip hop是trip(痴幻状态)和hip-hop(嘻哈)的词语结合，衍生自后迷幻浩室|
|28|人声|Vocal|指的是以人声为主要表达手段的音乐或歌曲。这种音乐风格强调歌手的声音技巧和表现力，可以涵盖各种音乐流派和风格。|
|29|爵士+放克|Jazz+Funk|是一种融合了爵士乐和放克音乐元素的音乐风格。这种音乐结合了爵士乐的即兴演奏和复杂和声与放克音乐的强劲节奏和律动，创造出一种既有深度又有活力的音乐形式。|
|30|融合爵士乐|Fusion|是混合了两种以上风格的爵士乐，最常见的就是与摇滚乐混合的摇滚爵士乐。|
|31|出神音乐|Trance|是一种电子音乐的风格，通常节奏在128至140BPM之间，4/4拍，注重旋律、和声来营造长时间的出神氛围的一种音乐形式。|
|32|古典音乐|Classical|亦称西洋古典音乐，指根植于西方书面记录传统(包括宗教音乐和世俗音乐)的艺术音乐，包含了从大约11世纪直至当代的广大时间范围。|
|33|器乐|Instrumental|指的是主要由乐器演奏而不包含人声的音乐。这种音乐形式可以涵盖多种不同的风格和流派，从古典音乐到现代流行音乐，再到爵士乐和电子音乐等。|
|34|酸性音乐|Acid|一种起源于20世纪80年代中期的电子舞曲风格，特别是与Acid House音乐紧密相关。这种风格以其独特的声音和节奏，尤其是使用了TB-303低音合成器的酸性音效而闻名。|
|35|浩室音乐|House|是一种电子音乐风格。最早的浩室音乐源自于美国1980年代初期到中期。House一字是出自于芝加哥当代知名的舞厅"仓库"(Warehouse)，绝大部分的浩室是由鼓声器所组成4/4拍子，伴随着厚实的低音声部。在这层基础之上再加入各种电子乐器制造出来的声音和音乐取样，比如爵士乐、蓝调或流行电音。|
|36|游戏|Game|专门为视频游戏创作的音乐。|
|37|声音剪辑|Sound Clip||
|38|福音音乐|Gospel|是基督教音乐的一种流派。福音音乐的创作、表演、意义甚至定义都因文化和社会背景而异。福音音乐的组成和表演有很多目的，包括审美愉悦，宗教或仪式的目的，并作为市场的娱乐产品。|
|39|噪音|Noise|在音乐语境中大量运用噪音表现的音乐流派。|
|40|另类摇滚|AlternRock|又称替代摇滚，源于1980年代的地下独立音乐，并在1990年代到2000年代开始流行。另类摇滚则有别于主流摇滚乐，主要体现在独特的吉他表现形式，颠覆性的歌词和一种轻蔑漠不关心的态度。|
|41|贝斯|Bass|20世纪80年代以来出现的几种电子舞曲音乐和嘻哈音乐，侧重于突出的低音鼓和/或低音线声音。|
|42|灵魂乐|Soul|又称骚灵音乐，是1950年代发源自美国的一种结合节奏蓝调和福音音乐的音乐流派。紧扣节奏、拍掌、即兴形体动作，是其重要特征。此外，独唱与伴唱之间的交流对唱、特别紧绷的发声，也是其主要特色。|
|43|朋克|Punk|是摇滚乐的分支音乐类型，出现于1970年代中期。朋克摇滚起源于1960年代的车库摇滚。这些歌曲具有硬朗的旋律和歌唱风格，精简的乐器以及反建制原则的歌词。朋克奉行DIY道德，许多乐队会自己制作唱片，并通过独立的唱片公司进行发行。|
|44|空间音乐|Space|是新时代音乐的一个子流派，被描述为"宁静、催眠、动人"。它源自环境音乐，与休闲音乐、轻松聆听音乐和电梯音乐相关。|
|45|冥想|Meditative|一种旨在帮助听众进入放松和冥想状态的音乐风格。这种音乐通常具有平静、和谐和缓慢的特点，常用于冥想、瑜伽、放松和睡眠等活动。|
|46|流行器乐|Instrumental Pop|结合了流行音乐的元素和器乐演奏。它的特点是流行音乐的旋律和结构，但没有人声或歌词。|
|47|器乐摇滚|Instrumental Rock|主要由摇滚音乐的元素和器乐演奏结合而成。与一般的摇滚乐不同，器乐摇滚没有人声或歌词，完全依赖于乐器来传达情感和表现力。|
|48|民族特色音乐|Ethnic|也称为世界音乐，是一种涵盖全球各地传统和民间音乐风格的广泛类别。这类音乐通常反映了特定文化或民族的音乐传统和特色。民族特色音乐包括不同地区、国家和文化的独特音乐形式，往往使用传统乐器和地方语言演唱。|
|49|哥特音乐|Gothic|一种深受哥特亚文化影响的音乐流派，起源于20世纪70年代末和80年代初的英国，作为后朋克(post-punk)音乐的一个分支发展起来。Gothic音乐具有黑暗、神秘和忧郁的氛围，歌词和音乐风格通常探讨深刻的情感、悲剧和超自然主题。|
|50|暗波|Darkwave|起源于20世纪70年代末的新浪潮和后朋克运动。其创作大多基于小调音调和内省歌词，被认为具有黑暗、浪漫和凄凉的特点，带有悲伤的基调。|
|51|工业泰克诺|Techno-Industrial|结合了Techno和Industrial的元素。音乐风格通常被描述为具有机械感、冷酷、充满节奏感和重击感。|
|52|电子乐|Electronic|亦简称电声、电音，是使用电子乐器以及电子音乐技术来制作的音乐。|
|53|流行民谣|Pop-Folk|是起源于巴尔干半岛各种流行音乐类型的总称，是巴尔干半岛流行音乐和迪斯科音乐的融合。流行民谣于60年代初开始发展，其特点是融合了流行音乐、民间音乐和舞曲等风格，而在歌词方面，多数带有色情意味。|
|54|欧陆舞曲|Eurodance|是在80年代后期到90年代中期流行于欧洲、大洋洲及南美洲的舞曲。其特色有：女声或男声、简单的歌词、若有饶舌部分多为男声、重且快速的节拍(约100到175BPM)、合成器的即兴重复段落等。歌唱部分常伴有无实际意义的歌词|
|55|梦幻|Dream|特别是指Dream Pop(梦幻流行)，是一种融合了流行音乐和氛围音乐元素的音乐流派。它起源于20世纪80年代的英国，以其旋律优美、氛围浓厚、梦幻般的声音而著称。这个流派的音乐通常具有迷离的感觉，常常通过层次丰富的吉他音效、轻柔的唱腔和电子音效来营造一种超现实的听觉体验。|
|56|南方摇滚|Southern Rock|从乡村音乐和蓝调在美国南部地区发展而来，焦点一般都集中在电吉他和人声上。|
|57|喜剧|Comedy|一种特殊的音乐类型，旨在通过音乐表达幽默和滑稽的情感。它可能包括搞笑的歌词、滑稽的旋律和令人发笑的声音效果。这种音乐通常被用于喜剧表演、电视节目、电影和广告等娱乐场合。|
|58|邪典|Cult|指那些虽然可能不是主流、但却拥有忠实追随者和独特文化影响力的音乐。这些音乐往往被视为独特或另类，具有强烈的个人风格和独特的艺术表达。|
|59|黑帮|Gangsta|一种起源于美国的Hip Hop音乐流派，主要探讨犯罪、街头文化和城市暴力等主题。这种音乐风格在20世纪80年代末和90年代初兴起于美国的东、西海岸，并成为当时Hip Hop文化中的重要分支之一。|
|60|40强|Top 40|通常是指当前最受欢迎的40首流行音乐单曲的排行榜。这个名词源自于广播电台的播放习惯，许多流行电台会播放最受欢迎的40首单曲，这样的排行榜被称为"Top 40"。这个概念也延伸到了音乐销售榜单和流媒体平台。|
|61|基督教说唱|Christian Rap|也被称为基督教嘻哈(Christian Hip Hop)，是一种结合了说唱音乐和基督教信仰的音乐流派。这种音乐通常以基督教的信仰、价值观和道德为主题，同时保留了说唱音乐的节奏、韵律和词曲结构。|
|62|流行/放克|Pop/Funk||
|63|丛林|Jungle|是一种融合了包括碎拍硬蕊和雷鬼/Dub/舞场雷鬼等其他类型的电子音乐。它是被统称为"丛林音乐"的一个亚种。高速(150到170bpm)的碎拍与高度切分的打击乐片段，以及采样和合成效果的大量使用是丛林舞曲最易识别的特点。小鼓滚动也十分常见。|
|64|美洲原住民音乐|Native American|一种将传统乐器和人声与现代元素相结合的音乐体裁。它通常以有节奏的鼓点、长笛和吟唱为特色。这种音乐深深植根于精神和文化传统，以自然、社区和祖先为主题。一些当代美国原住民音乐家将他们的传统音乐与摇滚、蓝调和流行音乐相融合，创造出一种独特的融合风格。|
|65|卡巴莱|Cabaret|一种起源于19世纪欧洲的音乐风格，通常与卡巴莱表演和夜总会文化相联系。这种音乐风格结合了歌剧、戏剧、舞蹈和音乐厅等元素，以及社交和娱乐活动，形成了一种独特的舞台表演形式。|
|66|新浪潮|New Wave|是摇滚乐的一种音乐分支风格，与朋克摇滚一同在1970年代中晚期出现。新浪潮吸收了很多原本朋克摇滚的声音与特质，例如简短以及具有强劲节奏的歌曲，同时在音乐表现与歌词方面具有更多的复杂性。除此之外，合成器的大量使用，后期制作的应用和艺术表现的多样性也是新浪潮音乐的特色。|
|67|灵歌|Psychadelic|起源于20世纪60年代，与迷幻文化和药物文化密切相关。这种音乐风格通常使用特殊的音效和录音技术，以及复杂的和弦结构和旋律，营造出一种超现实的、迷幻的听觉体验。|
|68|锐舞|Rave|一种起源于20世纪80年代的电子音乐流派，通常与夜店派对和舞蹈文化密切相关。这种音乐风格以其高能量、重复的节奏和强烈的舞曲性质而闻名，常常用于俱乐部、舞台音乐会和大型派对等场合。|
|69|表演曲调|Showtunes|指音乐剧(Musical)中的歌曲，也称为剧目音乐或舞台音乐。这些歌曲通常是音乐剧中的关键部分，用于表达角色的情感、推动情节发展，以及展示舞台上的故事情节。|
|70|预告片|Trailer|指的是电影预告片或游戏预告片，是在电影院、电视、互联网上播放的短视频，用于宣传即将上映的电影、电视节目或视频游戏。|
|71|低保真|Lo-Fi|通常以简单的录音方式和低保真音质为特征，强调了自然、舒适和原始的感觉。这种音乐风格经常与放松、思考和创作等活动相联系，成为许多人在日常生活中的背景音乐选择。|
|72|部落音乐|Tribal|一种源自传统社会的音乐形式，通常与特定文化或地区的原始部落社会相关联。这种音乐类型经常用于仪式、庆祝、宗教仪式以及社会聚会等活动中，具有深厚的文化意义和历史传统。|
|73|酸性朋克|Acid Punk|又称新迷幻音乐(Neo-psychedelia)，是迷幻音乐的一个多样化流派，它从20世纪60年代的迷幻音乐中汲取灵感，更新或复制了那个时代的音乐手法。新迷幻音乐起源于20世纪70年代，它最初是英国后朋克(post-punk)场景的产物，也被称为酸性朋克。在后朋克之后，新迷幻兴盛起来，成为一场更为广泛的国际艺术家运动，他们将迷幻摇滚的精神运用到新的声音和技术中。|
|74|酸性爵士|Acid Jazz|源于英国英格兰南部，是爵士乐的一种分支音乐类型，结合了部分灵魂乐、放克音乐和迪斯科成分，尤其是循环拍子和调式和声方面。|
|75|波尔卡|Polka|起源于19世纪的波希米亚(现在是捷克共和国的一部分)。虽然与捷克文化有关，但波尔卡在欧洲和美洲都很受欢迎。|
|76|复古|Retro|指具有复古风格或回归旧时代音乐特征的音乐，通常是对过去某个特定时期或音乐流派的致敬或模仿。这种音乐风格可以涵盖从20世纪50年代到90年代的各种音乐类型和流派，以及与之相关的时尚和文化元素。|
|77|音乐剧|Musical|音乐剧或乐剧是音乐、歌曲、舞蹈、戏剧、杂耍、特技和综艺结合的一种戏剧表演。它以幽默、讽刺、感伤、爱情、愤怒等情感引发剧情，再通过演员的语言、音乐、动作以及固定的演绎传达给观众。|
|78|摇滚|Rock & Roll|有时为了区别于现代摇滚乐，可称为早期摇滚乐)是一种音乐类型，起源于1940年代末期的美国，1950年代早期开始流行，迅速风靡全球。摇滚乐结合了当时流行的非裔美国人蓝调、乡村音乐、爵士乐、以及福音音乐。|
|79|硬式摇滚|Hard Rock|发源于1960年代的蓝调摇滚、车库摇滚与迷幻摇滚，比以往的摇滚乐更为猛烈。硬式摇滚主要使用大量回馈效果的音箱，加上产生失真的吉他效果器，其中电吉他加上娃娃音和其他音效的效果器，电贝斯也是可以，鼓组及键盘乐器(钢琴、电子琴、魔音琴)等乐器。|

Winamp扩展了下列流派：

|值|中文名称|英文名称|备注|
|-|-|-|-|
|80|民俗音乐|Folk|又称民俗音乐、民间歌谣，简称民歌、民谣、民乐，国际传统音乐协会之解释定义为"经过口传过程发展起来的普罗大众音乐"，也就是该音乐散布过程，纯粹是由演奏者或音乐接收者记录教习，并亲自相传所得。其范围包含歌曲、简单乐器演奏，甚至舞曲曲调与步伐。|
|81|民谣摇滚|Folk-Rock|又称为民间摇滚，是一种把民谣音乐与摇滚音乐相结合形成的音乐流派。这个名称最初在狭义上是指1960年代中期在美国和英国产生的此种音乐类型。这种流派的出现在英国的乐团如披头士乐团、搜索者乐团和动物乐团以及美国的鲍勃·迪伦和飞鸟乐队，他们将传统的民间音乐以摇滚方式创作和表演歌曲，并且使用摇滚乐器来表演歌曲。|
|82|民俗音乐|National Folk|指特定国家或地区的传统音乐形式，通常反映了该地区的文化、历史和生活方式。这种音乐风格传承了几代人的传统，是一种口头传统，通过口头传授、民歌、民间故事等方式传承下来。|
|83|摇摆乐|Swing|1930年代早期发源于美国的一种音乐类型，至1935年已形成独特的风格。摇摆乐依靠由低音提琴和鼓组成的节奏组来支撑旋律组。节拍速度大致多为中等至轻快，带有摇摆的节奏感。音乐之所以称为"摇摆乐"，是由于音乐强调小节里的下半拍或弱拍，因而带来摇摆的感觉。摇摆乐乐团通常会有独奏乐手为编制好的音乐加上即兴演奏。|
|84|快速融合乐|Fast Fusion|结合了爵士乐的技巧和复杂性，以及摇滚、放克、电子音乐或其他流派的元素，呈现出快节奏和高能量的音乐。这种风格强调技术性的演奏和即兴创作，常常具有复杂的编曲和快速的节奏。|
|85|咆勃爵士乐|Bebob|又译波普、比波普、或博普，源于美国，是一种兴起于1940年代初中期的爵士乐演奏形式，相较于大乐团时期的爵士乐，咆勃爵士乐在垂直的和声上采用许多延伸音，水平的旋律构筑除与和声的延伸音相应外，也运用许多邻接音与半音来即兴。节奏方面常运用许多重音错置手法造成特殊的律动。|
|86|拉丁音乐|Latin|笼统地表示源自西班牙语及葡萄牙语地区(即伊比利亚美洲和伊比利亚半岛)的音乐，也表示用这两种语言演唱的音乐。|
|87|复兴音乐|Revival||
|88|凯尔特音乐|Celtic|是一个被艺术家、唱片公司、音像店和音乐杂志用来描述一种宽泛的音乐类型的名词，这些音乐类型均自西欧凯尔特人的民间音乐传统发展而成。就概念本身而言，没有任何真正的音乐主体可以被精确地描述为凯尔特音乐，但这个名称依然被确定下来，既指口头传播的民俗音乐，也包含被录制下来的流行音乐。|
|89|蓝草音乐|Bluegrass|是美国民间音乐的一种。它在爱尔兰、苏格兰、英格兰的传统音乐分别有不同的来源。蓝草音乐的灵感来自于英国与爱尔兰、尤其是苏格兰阿巴拉契亚的美国新移民，和爵士乐与蓝调一样。|
|90|前卫音乐|Avantgarde|一种突破传统音乐形式和结构界限的音乐体裁。它通常融合实验技巧和非传统的声音，创造出独特而新颖的声音。历史上许多作曲家都曾探索过这一流派，包括将民间音乐和爵士乐元素融入其作品的作曲家。|
|91|哥特摇滚|Gothic Rock|是自1970年代晚期开始出现的摇滚乐类型。原本是指少数朋克摇滚或后朋克乐团的标签，而后在1980年代早期哥特一词(goth)开始成为某中分隔运动的定义。和朋克乐团精力旺盛的音乐风格相反，早期哥特乐团的音乐感觉较为自省，并与美学、黑暗主题文学或哲学相关，如哥特式恐怖、浪漫主义、存在主义、虚无主义等。|
|92|前卫摇滚|Progressive Rock|是在1960年代晚期兴起的摇滚乐分支之一，它的内涵十分复杂并且吸纳众多其他乐派精神，因此并不能明确地定义出其精确范围。前卫摇滚最热门的颠峰时期是在1970年代早期，并继续流传至今日。此种乐风通常结合爵士乐、古典音乐、民谣和世界音乐等元素，不同于美国摇滚是受到节奏蓝调以及乡村音乐的影响。|
|93|迷幻摇滚|Psychedelic Rock|一种摇滚乐流派，其源自迷幻文化及使用迷幻药而产生的思维骤变。迷幻摇滚涌现于1960年代的英美，与车库摇滚及民谣摇滚乐团属同一时期。迷幻摇滚介于早期以蓝调为基础的摇滚音乐，至前卫摇滚、艺术摇滚、实验摇滚、硬摇滚，甚至重金属间的过渡时期。迷幻摇滚同时融入了非西方的音乐元素，如印度音乐的拉加(ragas)与西塔尔琴(sitars)。|
|94|交响摇滚|Symphonic Rock|交响摇滚乐是一种将古典音乐元素与摇滚乐相结合的音乐类型。它通常以管弦乐器(如弦乐、号角和木管乐器)以及电吉他和鼓为特色。这种音乐的特点是音效宏大、编曲复杂，并使用故事性和概念性主题。|
|95|慢摇滚|Slow Rock|一种摇滚音乐的子类型，通常以较慢的节奏和抒情的旋律为特征。歌词和音乐往往具有深刻的情感内容，常常涉及爱情、失落和怀旧等主题。|
|96|大乐团|Big Band|演奏爵士乐的乐团，流行于美国1930年代初到1950年代末的摇摆年代。大乐团的编制通常有10到25位乐手，包括演奏萨克斯风、小喇叭、伸缩喇叭、铁琴的乐手，还有歌手以及负责节奏乐器的乐手。|
|97|Chorus|Chorus||
|98|轻松音乐|Easy Listening|一种以舒缓、圆润的声音为特点的音乐类型，通常以流畅的人声和管弦乐编曲为特色。这类音乐旋律平和、节奏轻柔，是放松身心的理想背景音乐。这种音乐流派通常与经典的男歌手和女歌手有关，他们已成为轻松音乐的代名词。|
|99|原音乐|Acoustic|原音乐及原音乐器是在20世纪电子音乐面世后，为了将传统真实的、未经电子处理(或不是由电子信号产生音波)的乐器与之区别而衍生的返璞词。广义的原音乐是指对声音是否真乐器所直接发出的判断，这解释下原音乐器在经过电子扩音器或其他存储媒介后所播出的声音，只要其音色没有受到破坏性处理就仍旧是原音乐。|
|100|幽默|Humour||
|101|朗诵|Speech||
|102|香颂|Chanson|chanson意为歌曲，源自于拉丁文cantio，源于中世纪的法国，是法国世俗歌曲的泛称。今天在法国，香颂往往指的是像雅克·布雷尔、艾迪特·皮雅芙等著名歌手的作品，区别于其他法国流行音乐，按照法语的节奏，而不是英语的节奏，因此被认为特别"法国"。此外，香颂的歌词也特别讲究韵味和意境。|
|103|歌剧|Opera|歌剧是一门西方舞台表演艺术，简单而言就是主要或完全以歌唱和音乐来交代和表达剧情的戏剧。歌剧在17世纪，即1600年前后才出现在意大利的佛罗伦斯，它源自古希腊戏剧的剧场音乐。歌剧的演出和戏剧的所需一样，都要凭借剧场的典型元素，如背景、戏服以及表演等。歌剧被视为西方古典音乐传统的一部分。|
|104|室内乐|Chamber Music|又称室乐，是一种古典音乐作品的体裁，为几件在室内演奏的乐器所作。室内乐由二到九人合奏，每人各演奏一个声部，通常不含独奏。室内乐的"室内"意指音乐可以在空间较小的室内演奏。室内乐一般不设指挥，所以每个表演者拥有较大的自由发挥空间。|
|105|奏鸣曲|Sonata|奏鸣曲是乐器音乐的写作方式之一。在古典音乐史上，此种曲式随着各个乐派的风格不同，也有着不同的发展。奏鸣曲的曲式从古典乐派时期开始逐步发展完善，19世纪初，给各类乐器演奏的奏鸣曲大量出现，奏鸣曲俨然成为了西方古典音乐的主要表现方式。到了20世纪，作曲家依然创作著给乐器演奏的奏鸣曲，但相较于古典乐派以及浪漫乐派时期，20世纪的奏鸣曲在曲式方面已有了不同的面貌。|
|106|交响曲|Symphony|是古典音乐的乐曲型式，是包含多个乐章的大型管弦乐曲，一般是为管弦乐团创作，乐团的演奏乐器有弦乐器(包括小提琴、中提琴、大提琴、低音提琴)、木管乐器、铜管乐器及打击乐器，编制在30至100人之间。交响曲多半会有四个乐章，第一乐章通常是奏鸣曲式。交响曲会有包括所有乐器的大谱表。而各乐器演奏家会有只对应各自乐器的乐谱。有些交响曲会包括人声(例如贝多芬的第9号交响曲)。|
|107|电臀贝斯|Booty Bass|一种舞曲音乐流派，起源于20世纪80年代和90年代的迈阿密贝斯音乐(Miami Bass)和电子音乐。它的特点是强烈的低音节拍、快速的节奏和性感、挑逗的歌词。电臀贝斯的音乐通常用于舞会和派对，旨在让人们跳舞和享受音乐的节奏。|
|108|Primus|Primus|一支美国摇滚乐队，1984年成立于加利福尼亚州埃尔索布兰特。|
|109|色情律动|Porn Groove|主要出现在20世纪70年代色情电影的配乐中。这种音乐的最初用途是模仿这些电影中出现在银幕上的性动作(通过音频)，其最显著、最常见的特点是使用带有wah-wah踏板的电吉他来演奏旋律。该流派的其他方面还包括(但不限于)受放克乐启发的拍打式低音吉他和强调边音的简约鼓。|
|110|讽刺音乐|Satire|指采用讽刺手法或被描述为讽刺手法的音乐。它涉及社会、政治、宗教、文化结构等主题，通常以黑色幽默或相关音乐类型为幌子，对这些主题进行评论或批判。主题包括性、种族、文化、宗教、政治、制度、禁忌话题、道德和人类状况。|
|111|慢板音乐|Slow Jam|一种受节奏布鲁斯和灵魂乐影响的音乐。慢速即兴音乐通常是R&B民谣或低节奏歌曲，大多音色柔和，抒情内容浓烈或浪漫。|
|112|俱乐部音乐|Club|多指在夜店或俱乐部播放的各种类型的音乐。|
|113|探戈|Tango|一种由多种文化融合而成的音乐与舞蹈形式，诞生于南美拉普拉塔河河口两岸的布宜诺斯艾利斯(阿根廷首都)及蒙特维多(乌拉圭首都)。探戈舞曾经常在港口的妓院和酒吧里进行，那里的业主雇佣乐队用音乐来娱乐他们的顾客，之后传播到了世界的各个地方。|
|114|桑巴|Samba|是在20世纪源于巴西里约热内卢非裔巴西族群的音乐类型。桑巴源自西非以及巴西民俗传统，特别是和巴西殖民时期及巴西帝国时期的乡村原始桑巴有关，桑巴是巴西重要的文化现象，也是巴西国家象征之一。|
|115|民间传说音乐|Folklore|通常源自特定文化或地区的传统音乐，它反映了该文化或地区的历史、价值观、传统和生活方式，常以口传传统的形式流传下来，以叙事的形式呈现，讲述着故事、神话或传说，并且在不同的地区会有不同的风格和变体。|
|116|谣曲|Ballad|又译民歌、民谣、歌谣、叙事曲等，是一种诗歌形式，通常是与音乐结合的叙事。民谣源自中世纪的法国香颂诗歌或叙事曲，最初是一种舞曲，结合了抒情(情绪化，情感)、史诗(故事)和戏剧(对话)的特征。|
|117|力量抒情歌|Power Ballad|一种流行音乐中的子类型，通常表现为慢节奏的抒情歌曲，表达深沉、强烈的情感，涉及爱情、失落、渴望、挣扎等主题，具有强烈的情感表达和磅礴的音乐氛围。这种风格在20世纪80年代和90年代特别流行，常见于摇滚乐和流行音乐中。|
|118|节奏灵魂乐|Rhythmic Soul|融合了灵魂音乐的基本元素和节奏感，通常具有强烈的节奏和舞蹈性。这种音乐风格在20世纪70年代末到80年代初尤为流行，是灵魂音乐演变的一种形式。|
|119|即兴饶舌|Freestyle|又称花式饶舌、自由说唱，经常用来形容临场即兴的说唱方式，其歌词很少预先写好，全靠表演者即兴发挥，有时会配合伴奏和强烈节拍将内容唱出。|
|120|二重奏|Duet|是为两位演奏者创作的音乐作品，其中演奏者在作品中的地位相当，通常是由两位歌手或两位钢琴家共同完成的作品。它与和声不同，表演者轮流表演独奏部分，而不是同时表演。|
|121|朋克摇滚|Punk Rock|常简称为朋克(Punk)，见流派列表43.Punk|
|122|鼓独奏|Drum Solo|用架子鼓演奏的器乐独奏。鼓独奏可以是设定的，也可以是即兴的，长度不限，甚至可以是主要的表演。|
|123|无伴奏合唱|A capella|中文音译阿卡贝拉或阿卡佩拉，是指无乐器伴奏的纯人声音乐。又简称阿贝、阿伯。|
|124|欧陆浩室|Euro-House|兴起于20世纪80年代末，是House的一种演唱风格。这种风格的歌曲深受流行舞曲的影响，并以House音乐为背景。|
|125|舞厅音乐|Dance Hall|牙买加流行音乐的一种流派，起源于20世纪70年代末。舞厅音乐的主要元素包括大量使用牙买加土语而非牙买加标准英语，以及注重音轨乐器。|