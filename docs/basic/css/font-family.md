苹果字体的官方文档：https://developer.apple.com/fonts/ 苹果预安装字体：https://developer.apple.com/fonts/system-fonts/

## 活动默认字体

这边UI尤其喜欢平方体。

### 平方字体

苹方提供了六个字重，font-family 定义如下：

```
苹方-简 常规体
font-family: PingFangSC-Regular;
苹方-简 极细体
font-family: PingFangSC-Ultralight;
苹方-简 细体
font-family: PingFangSC-Light;
苹方-简 纤细体
font-family: PingFangSC-Thin;
苹方-简 中黑体
font-family: PingFangSC-Medium;
苹方-简 中粗体
font-family: PingFangSC-Semibold;
```

所以PingFang-SC-Bold是什么？是windows和mac系统下，字体名字的不同导致的，还是用上述正规的名字。

### -apple-system

兼容主流浏览器和苹果端浏览器

```
-apple-system 是在以 WebKit 为内核的浏览器（如 Safari）中，调用 Apple（苹果公司）系统（iOS, macOS, watchOS, tvOS）中默认字体（现在一般情况下，英文是 San Francisco，中文是苹方
BlinkMacSystemFont 是在 Chrome 中实现调用 Apple 的系统字体
```

### H5活动使用字体

考虑的情况是只需要在手机上运行，然后除了ios，android还有一些别的小平台，ios的话使用平方字体来兼容

优先平方体，-apple-system会自动适应ios下的中英文，那么android和别的平台怎么处理？不指定的话使用默认字体，最后带一个sans-serif通用字体系列就行。

为了兼容旧版本的情况，倒数第二加个Helvetica。

```
font-family: PingFangSC-Regular, -apple-system, BlinkMacSystemFont,Helvetica, sans-serif;
```

## 默认字体

大家都知道，在不同操作系统、不同游览器里面默认显示的字体是不一样的，并且相同字体在不同操作系统里面渲染的效果也不尽相同，那么如何设置字体显示效果会比较好呢？下面我们逐步的分析一下：

### 首先我们看看各平台的默认字体情况

#### Window下：

- 宋体（SimSun）：Win下大部分游览器的默认字体，宋体在小字号下（如12px、14px）的显示效果还可以接受，但是字号一大就非常糟糕了，所以使用的时候要注意。
- 微软雅黑（"Microsoft Yahei"）：从 Vista 开始，微软提供了这款新的字体，一款无衬线的黑体类字体，并且拥有 Regular、Bold 两种粗细的字重， 显著提高了字体的显示效果。 现在这款字体已经成为Windows游览器中最值得使用的中文字体。从Win8开始，微软雅黑又加入了 Light 这款更细的字重 ，对于喜欢细字体的设计、开发人员又多了一个新的选择。
- Arial：Win平台上默认的无衬线西文字体（为什么要说英文字体后面会解释），有多种变体，显示效果一般。
- Tahoma：十分常见的无衬线字体，被采用为Windows 2000、Windows XP、Windows Server 2003 及Sega游戏主机Dreamcast等系统的预设字型 ， 显示效果比Arial要好。
- Verdana：无衬线字体，优点在于它在小字上仍结构清晰端整、阅读辨识容易。
- 其他：Windows 下默认字体列表：[微软官网](https://docs.microsoft.com/en-us/typography/?pid=161)、[维基百科](https://en.wikipedia.org/wiki/List_of_typefaces_included_with_Microsoft_Windows)

**结论**：微软雅黑为Win平台上最值得选择的中文字体，但非游览器默认，需要设置；西文字体的选择以Arial、Tahoma等无衬线字体为主。

#### Mac OS下：

- 华文黑体（STHeiti）、华文细黑（STXihei）：属于同一字体家族系列，OS X 10.6 之前的简体中文系统界面默认字体, 也是目前Chrome游览器下的默认字体， 有 Regular 和 Bold 两个字重，显示效果可以接受。
- 黑体-简（Heiti SC）：从 10.6 开始，黑体-简代替华文黑体用作简体中文系统界面默认字体，苹果生态最常用的字体之一，包括iPhone、iPad等设备用的也是这款字体，显示效果不错，但是喇叭口设计遭人诟病。
- 冬青黑体（ Hiragino Sans GB ）：听说又叫苹果丽黑，日文字体Hiragino KakuGothic的简体中文版，简体中文有 常规体 和 粗体 两种，冬青黑体是一款清新的专业印刷字体，小字号时足够清晰，拥有很多人的追捧。
- Times New Roman：Mac平台Safari下默认的字体，是最常见且广为人知的西文衬线字体之一，众多网页浏览器和文字处理软件都是用它作为默认字体。
- Helvetica、Helvetica Neue：一种被广泛使用的传奇般的西文字体（这货还有专门的记录片呢），在微软使用山寨货的Arial时，乔布斯却花费重金获得了Helvetica苹果系统上的使用权，因此该字体也一直伴随着苹果用户，是苹果生态中最常用的西文字体。Helvetica Neue为Helvetica的改善版本，且增加了更多不同粗细与宽度的字形，共拥有51种字体版本，极大的满足了日常的使用。
- 苹方（PingFang SC）：在Mac OS X EL Capitan上， 苹果为中国用户打造了一款全新中文字体--苹方， 去掉了为人诟病的喇叭口， 整体造型看上去更加简洁， 字族共六枚字体：极细体、纤细体、细体、常规体、中黑体、中粗体。
- San Francisco：同样是Mac OS X EL Capitan上最新发布的西文字体，感觉和Helvetica看上去差别不大，目前已经应用在Mac OS 10.11+、iOS 9.0+、watch OS等最新系统上。
- 其他：Mac下默认字体列表：[苹果官网](https://developer.apple.com/fonts/)、[维基百科](https://en.wikipedia.org/wiki/List_of_typefaces_included_with_macOS)

**结论**：目前苹方和San Francisco为苹果推出的最新字体，显示效果也最为优雅，但只有最新系统才能支持，而黑体-简和Helvetica可以获得更多系统版本支持，显示效果也相差无几，可以接受。

#### Android系统：

- Droid Sans、Droid Sans Fallback：Droid Sans为安卓系统中默认的西文字体，是一款人文主义无衬线字体，而Droid Sans Fallback则是包含汉字 、日文假名 、韩文的文字扩展支持。

**结论**：Droid Sans为默认的字体，并结合了中英文，无需单独设置。

#### iOS系统：

- iOS系统的字体和Mac OS系统的字体相同，保证了Mac上的字体效果，iOS设备就没有太大问题。

#### Linux：

- 文泉驿点阵宋体：类似宋体的衬线字体，一般不推荐使用。
- 文泉驿微米黑：几乎是 Linux 社区现有的最佳简体中文字体。

### 选择字体需要注意的问题

#### 字体的中英文写法

我们在操作系统中常常看到宋体、微软雅黑这样的字体名称，但实际上这只是字体的显示名称，而不是字体文件的名称，一般字体文件都是用英文命名的，如SimSun、Microsoft Yahei。在大多数情况下直接使用显示名称也能正确的显示，但是有一些用户的特殊设置会导致中文声明无效。

因此，保守的做法是使用字体的字体名称（英文）或者两者兼写。如下示例：

```
font-family: STXihei, "Microsoft YaHei"; font-family: STXihei, "华文细黑", "Microsoft YaHei", "微软雅黑";
```

或者只写英文么？

#### 声明英文字体

绝大部分中文字体里都包含英文字母和数字，不进行英文字体声明是没有问题的，但是大多数中文字体中的英文和数字的部分都不是特别漂亮，所以建议也对英文字体进行声明。

由于英文字体中大多不包含中文，我们可以先进行英文字体的声明，这样不会影响到中文字体的选择，因此优先使用最优秀的英文字体，中文字体声明则紧随其次。

#### 照顾不同的操作系统

英文、数字部分：在默认的操作系统中，Mac和Win都会带有Arial, Verdana, Tahoma等几个预装字体，从显示效果来看，Tahoma要比Arial更加清晰一些，因此字体设置Tahoma最好放到前面，当找不到Tahoma时再使用Arial；而在Mac中，还拥有一款更加漂亮的Helvetica字体，所以为了照顾Mac用户有更好的体验，应该更优先设置Helvetica字体；Android系统下默认的无衬线字体就可以接受，因此无需单独设置。最后，英文、数字字体的最佳写法如下：

```
font-family: Helvetica, Tahoma, Arial;
```

中文部分：在Win下，微软雅黑为大部分人最常使用的中文字体，由于很多人安装Office的缘故，Mac电脑中也会出现微软雅黑字体，因此把显示效果不错的微软雅黑加入到字体列表是个不错的选择；同样，为了保证Mac中更为优雅字体苹方（PingFang SC）、黑体-简（Heiti SC）、冬青黑体（ Hiragino Sans GB ）的优先显示，需要把这些字体放到中文字体列表的最前面；同时为了照顾到Linux操作系统的体验，还需要添加文泉驿微米黑字体。最后，中文字体部分最佳写法如下：

```
font-family: "PingFang SC", "Hiragino Sans GB", "Heiti SC", "Microsoft YaHei", "WenQuanYi Micro Hei";
```

中英文整合写法：

```
font-family: Helvetica, Tahoma, Arial, "Heiti SC", "Microsoft YaHei", "WenQuanYi Micro Hei"; font-family: Helvetica, Tahoma, Arial, "PingFang SC", "Hiragino Sans GB", "Heiti SC", "Microsoft YaHei", "WenQuanYi Micro Hei";
```

#### 注意向下兼容

如果还需要考虑旧版本操作系统用户的话，不得不加上一些旧版操作系统存在的字体：Mac中的华文黑体、冬青黑体，Win中的黑体等。同样按照显示效果排列在列表后面，写法如下：

```
font-family: Helvetica, Tahoma, Arial, "PingFang SC", "Hiragino Sans GB", "Heiti SC", STXihei, "Microsoft YaHei", SimHei, "WenQuanYi Micro Hei";
```

加入了 STXihei（华文细黑）和 SimHei（黑体）。

#### 补充字体族名称

字体族大体上分为两类：sans-serif（无衬线体）和serif（衬线体），当所有的字体都找不到时，我们可以使用字体族名称作为操作系统最后选择字体的方向。一般非衬线字体在显示器中的显示效果会比较好，因此我们需要在最后添加 sans-serif，写法如下：。

```
font-family: Helvetica, Tahoma, Arial, "PingFang SC", "Hiragino Sans GB", "Heiti SC", "Microsoft YaHei", "WenQuanYi Micro Hei", sans-serif;
```

#### 字体何时需要添加引号

当字体具体某个取值中若有一种样式名称包含空格，则需要用双引号或单引号表示，如：

```
font-family: "Microsoft YaHei", "Arial Narrow", sans-serif;
```

如果书写中文字体名称为了保证兼容性也会添加引号，如：

```
font-family: "黑体-简", "微软雅黑", "文泉驿微米黑";
```

#### 最后，推荐写法

设计师的审美不同，所以可能提供不同的优先顺序。

```
font-family: "Helvetica Neue", Helvetica, Arial, "PingFang SC", "Hiragino Sans GB", "Heiti SC", "Microsoft YaHei", "WenQuanYi Micro Hei", sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol';;
```

'Apple Color Emoji', 'Segoe UI Emoji', 'Segoe UI Symbol'是三个符号字体。

### 别人的建议

但是——的确还有另外两个因素在纠结着：

1. 不少 Windows 用户因为各种原因关闭了 ClearType，在此情形下微软雅黑将会惨不忍睹！但是 Mac 的字体也不是好的选择，真正的胜出者？猜对了，宋体。
2. 绝大部分 Mac 下的黑体在 Windows 下模糊不清，而微软雅黑虽然丑但在 Mac 下至少能看。（间接体现了两个平台的字体渲染技术的差距）

所以在实践中，真正接近 “万无一失” 的方案需要考虑以下几点：

1. 利用 UA 判断为不同的平台加载不一样的字体声明；
2. 除非有特别的原因，否则尽量保持正文用宋体，标题和其他可以放大些的地方用微软雅黑（针对 Windows）；
3. Mac 下的冬青体效果极佳，但是该字体在 Mac OS X 10.6 以前是没有的，所以谨慎考虑你的用户群体，或者使用华文黑体系列做 fallback；