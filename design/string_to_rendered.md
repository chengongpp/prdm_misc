# 如何从字符串渲染到字形(未完结)

本文尝试讲解一个看似简单的流程：如题。下面先从一个简单的情况说起。

假设一个情景：现在我们的手头有一个unicode字符串```u"好"```，一个字体文件```SourceHanSerif-regular.ttf```，和一个空闲的屏幕区域```screenBuf```。我们的最终目的是构建一个渲染系统，使得屏幕区域上显示出

渲染系统读入字符串并解析，发现"好"的Unicode[码位](https://zh.wikipedia.org/wiki/%E7%A0%81%E4%BD%8D)是```U+597D```。渲染器就会拿着这个码位，去ttf文件的**CMAP表**中寻找其映射到的字形ID(glyph id)中。

## CMAP表

CMAP的全称是"Character to Glyph Mapping"，也即从**字符**映射到**字形**。为什么要这么做呢？好比说，字符```U+0020```(空格)和```U+00A0```(不换行空格)，在排版时印出的形状都是一样的，此时没有必要额外占用存储空间，可以将两个码位映射到同一个空格字形。

一个CMAP表可能包含多个子表，称为**编码表**。毕竟字符串的编码形式多了去了，除了Unicode，像GB2312和Big5之类的编码也应该能够正确地被映射到字形上。不过，微软**强烈建议**所有的字体都使用Unicode编码表。

在CMAP表找到了符合的编码表后，进入子表进行进一步的查找，即可找到对应该码位的字形信息在**glyf表**的偏移。

> 子表使用的映射方式和规范历经多年发展也有所不同，最新近的标准是Format 14 Unicode异体序列(Unicode Variation Sequences)，它允许为同一个码位指定不同的字形变体以提供支持，这对处理[CJK统一表意文字问题](https://zh.wikipedia.org/wiki/%E4%B8%AD%E6%97%A5%E9%9F%93%E7%B5%B1%E4%B8%80%E8%A1%A8%E6%84%8F%E6%96%87%E5%AD%97)提供了很大的帮助。

有关于表的具体内容，可以参考[OpenType规范: cmap](https://docs.microsoft.com/en-us/typography/opentype/spec/cmap)。

> [不换行空格](https://zh.wikipedia.org/wiki/%E4%B8%8D%E6%8D%A2%E8%A1%8C%E7%A9%BA%E6%A0%BC)早先用于印刷时指示两个单词间不要换行，比如100 km，排版时不应该换行断开；现在多用于在HTML插入空格(```&nbsp;```)，因为HTML会把重复的空格吞掉。

## glyf表