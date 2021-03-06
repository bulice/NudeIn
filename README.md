<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/logo.png" width = "100%" />

# NudeIn

[![Build Status](https://travis-ci.org/hon-key/NudeIn.svg?branch=master)](https://travis-ci.org/hon-key/NudeIn)
[![Cocoapods](https://img.shields.io/badge/pod-1.2.6-orange.svg)](https://img.shields.io/badge/pod-1.2.4-orange.svg)
[![Platform](https://img.shields.io/badge/platform-iOS-lightgrey.svg)](https://img.shields.io/badge/platform-iOS-lightgrey.svg)
[![Language](https://img.shields.io/badge/language-Objective--C-blue.svg)](https://img.shields.io/badge/language-Objective--C-blue.svg)

NudeIn 是一个基于 UITextView ，书写风格类似于 masonry 的 iOS 端富文本控件，它采用优雅的声明式(链式)方法定义富文本控件，和编程式的不同，它所需的代码量相当短，且非常直观易用。

与此同时，NudeIn 不止于此，它会是一款非常灵性的富文本控件，它会将减少代码冗余提高到极致。比如考虑到一点，富文本里可能会有多于 2 个的风格一致的富文本，也有可能仅仅只是风格部分一致的富文本，比如字体大小不一样，比如颜色不一样。这样的代码如果按照常规去写，可能会出现大量相同的代码段。为了解决这个问题，NudeIn 引入了 **`模板`** ，你可以轻松声明一个模板，应用到任何需要它的组件上，而每个组件甚至可以声明属于自己的属性来覆盖模板上的属性，以达到部分一致的效果。这就是 NudeIn 非常灵活的地方。

相比其他第三方富文本库，NudeIn 将是最符合人类思维方式的，使用它将不会花费你太多的学习成本。如果你有 masonry 经验，你将几乎没有学习成本，如果你没有，也无需担心，它看起来就像是为你的思维方式精心打造的一般，只需稍微看看例子，就可以完全学会使用方法。

NudeIn is an iOS rich text control based on UITextView. It has a writing style similar to masonry. NudeIn uses an elegant declarative (chained) programming to define rich text controls. Unlike programming, it requires a very small amount of code, and it is intuitive and easy to use.

In addition to reducing code redundancy to almost zero, NudeIn is very sensitive and intelligent. If you are writing a code where in the rich text there are more than two rich texts of consistent styles, or two rich texts of partially consistent styles, e.g. of different font sizes or different colors, and if you write it as what we usually do, you may end up having a large number of identical code segments. NudeIn provides a solution. It introduces templates. You can easily declare a template to be applied to any component that needs it. Or each component can even declare its own properties to override the properties on the template to achieve a partially consistent effect. In either way NudeIn is much more flexible than its alternatives.

Compared to other third-party rich text libraries, NudeIn is developed to possess an “intuitive” mind so it is absolutely easy to learn. If you have masonry experience, there is almost no cost of learning. If you don't…No worries! NudeIn is purposely crafted to follow how you think. Just take a look at the guide and you will be all ready to start.


## Usage

NudeIn 的用法非常简单明了，这里给出一个非常简单的例子，相信你会被这样的用法惊艳到，一旦用起来就会爱不释手:

1、引入控件
```Objective-C
#import "NudeIn.h"
```

2、声明控件为你的成员变量

```objc
@property (nonatomic,strong) NudeIn *attrLabel;
```

3、Do it yourself
```objc
_attrLabel = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"this is a ").font(14).color([UIColor blackColor]).attach();
    make.text(@"BlueLink").font(17).color([UIColor blueColor]).link(self,@selector(linkHandler:)).attach();
    make.text(@", and this is a ").font(14).color([UIColor blackColor]).attach();
    make.text(@"RedLink").font(17).color([UIColor redColor]).link(self,@selector(linkHandler:)).attach();
}];

```

3、对声明了 **`link`** 属性的部分定义回调
```objc

- (void)linkHandler:(NUDAction *)action {
    
    if ([action isKindOfClass:[NUDLinkAction class]]) {
        
        NUDLinkAction *linkAction = (NUDLinkAction *)action;
        
        UIAlertController *alertController = [UIAlertController alertControllerWithTitle:linkAction.string message:nil preferredStyle:UIAlertControllerStyleAlert];
    
        [alertController addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
        }]];
        
        [self presentViewController:alertController animated:YES completion:nil];
        
    }
    
}

```

结果会是这样：

<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/1.png" width = "50%" />

点击带有 **`link`** 属性的部分，将产生回调：

<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/2.png" width = "50%" />

## Installation

### 1、Cocoapods

```
pod 'NudeIn'
```
最新 pod 版本：1.2.7

1、优化了触摸时的逻辑，修复了 makeTemplate 方法在继承时无法独立使用的问题，修复了一个阴影在没有传入 template 时无效的问题

2、makeTemplate 新增了 allText 和 allImage，现在可以对全局进行统一配置

3、新增了一个很酷的功能：自定义方法:

为了让自己的代码更加适应不同工程的开发，能够自定义方法是一件很有意义的事，它不但能够减少代码量，提高复用性，还能让你的NudeIn看起来独一无二，完全属于其所在的工程。这为 NudeIn 提供了非常好的可读性。不过这并不能算是实际意义上的新增，因为它基于 OC 的 Category，所以你完全可以自己通过 Category 去自定义属于自己的方法，这里 NudeIn 只是提供了更加方便，更加好管理的宏定义来实现这个操作。

我们举个例子：

如果你的 app 里有一个属于自己的需求：一种属于该 app 的文字主题色（假如是橙色 Orange ）,那么你可能会把这种颜色编辑为宏定义以在工程里大范围重用。但是如果你大范围用到了 NudeIn，使用宏定义的方法可能会稍显麻烦，而如果使用自定义功能，假设你想为该需求定义一个完全属于该工程的方法：themeColor，你将可以得到看起来最为原生的使用体验。

我们首先创建一对cocoa源文件（MyNudeInMethod.h, MyNudeInMethod.m）

在 MyNudeInMethod.h 里添加：
```Objc
NUDAnounceTextAttributeSet(themeColor);
```
在 MyNudeInMethod.m 里添加：
```Objc
NUDMakeTextAttributeSet(themeColor, color([UIColor orangeColor]));
```

这样我们就完成了对 themeColor 的自定义，我们可以在任何引用了该源文件的源文件里使用它：
```Objc
nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"text").themeColor().attach();
}];
```

NudeIn 提供了两种自定义方法的宏，一种就是上面的无参数方法自定义，另一种为单参数方法自定义，该定义方式可在 textExample 里示范。

至于更多参数的自定义方法，只能通过自己实现 Category 的方式去使用，不过为了让一个方法看起来更加易读，添加更多参数其实并不推荐，一个参数我认为基本上是够用的。

最低 iOS 版本： `8.0`

### 2、Copy files

你可以拷贝 master 或者 1.2.7 里 NudeIn 文件夹的所有文件到你的工程里。

master 分支可能包含一些新的功能或者为不稳定版本，如果你在使用过程中遇到问题，欢迎 commit an issue 或者提交 PR。

## Indexes

* ### [Text](#usage)

    - [**font**](#font) **`通过大小声明字体，统一使用系统字体`**

    - [**fontName**](#fontname) **`通过字体名以及大小声明字体`**

    - [**fontRes**](#fontres) **`通过 UIFont 声明字体`**

    - [**fontStyle**](#fontstyle) **`声明字体的风格，如 Bold、Light 等`**

    - [**bold**](#bold) **`声明字体为 Bold 风格，如果有的话`**

    - [**color**](#color) **`声明文字的前景色`**

    - [**mark**](#mark) **`声明文字的底色`**

    - [**hollow**](#hollow) **`声明文字为镂空`**
    
    - [**solid**](#solid) **`声明文字为实心`**

    - [**link**](#link) **`声明文字为链接文字`**

    - [**_**](#_) **`声明文字带下划线`**

    - [**deprecated**](#deprecated) **`声明文字带删除线`**

    - [**skew**](#skew) **`声明文字为斜体`**

    - [**kern**](#kern) **`声明文字的紧凑程度`**

    - [**ln**](#ln) **`声明文字换行`**
    
    - [**ligature**](#ligature) **`声明文字为连体字`**
    
    - [**letterpress**](#letterpress) **`声明文字为印刷风格（凸起效果），该属性占用内存较高，谨慎使用`**
    
    - [**vertical**](#vertical) **`声明文字的垂直偏移`**
    
    - [**stretch**](#stretch) **`声明文字的水平拉伸程度（产生变形）`**
    
    - [**reverse**](#reverse) **`声明文字逆序书写`**
    
    - [**shadow**](#shadow) **`声明文字带默认阴影`**
    
    - [**shadowDirection**](#shadowdirection) **`声明文字带阴影，并且设定阴影为八个基本方向`**
    
    - [**shadowOffset**](#shadowoffset) **`声明文字带阴影，并且完全自定义阴影的方向`**
    
    - [**shadowBlur**](#shadowblur) **`声明文字带阴影，并自定义阴影的模糊程度`**
    
    - [**shadowColor**](#shadowcolor) **`声明文字带阴影，并自定义阴影颜色`**
    
    - [**shadowRes**](#shadowres) **`通过 NSShadow 声明并自定义阴影`**
    
    - [**lineSpacing**](#linespacing) **`声明文字的行距`**
    
    - [**lineHeight**](#lineheight) **`声明文字的行高，（最小行高，最大行高，行高倍数）`**
    
    - [**paraSpacing**](#paraspacing) **`声明文字每个自然段对其他自然段拉开的点距，（与前自然段拉开的点距，与后自然段拉开的点距）`**
    
    - [**aligment**](#aligment) **`声明文字的对齐方式，参数为 NUDAlignment`**
    
    - [**indent**](#indent) **`声明文字的缩进，（前缩进，后缩进）`**
    
    - [**fl_headIndent**](#fl_headindent) **`声明文字的首行前缩进，该属性会在首行覆盖 indent 的前缩进属性`**
    
    - [**linebreak**](#linebreak) **`声明文字的断行方式，参数为 NUDLineBreakMode`**
    
    - [**highlight**](#highlight) **`声明文字的触摸高亮，参数为一个模板的 id `** 
    
    - [**tap**](#tap) **`声明文字的触摸回调，参数为回调的 target 以及回调方法 `**  

* ### [Image](#usage)

    - [**origin**](#origin) **`声明图像的偏移，锚点为左下角`**
    
    - [**size**](#size) **`声明图像的大小`**
    
    - [**ln**](#ln-image) **`声明图像换行`**
    
    - [**vertical**](#vertical-image) **`声明图像换行`**
    
    - [**aligment**](#aligment-image) **`声明图像对齐属性，参数为 NUDAligment`**

* ### [Template](#usage)

    - [**textTemplate**](#usage) **`声明一个 text 模板，以参数 identifier 来标识这个模板以重复使用，其使用方法和 text 一样`**
    
    - [**imageTemplate**](#usage) **`声明一个 image 模板，以参数 identifier 来标识这个模板以重复使用，其使用方法和 image 一样`**
    
    - [**allText**](#usage) **`声明所有使用 .attach() 的 text 都会被附加的属性，使用 .attachWith(@"") 不受影响`**
    
    - [**allImage**](#usage) **`声明所有使用 .attach() 的 image 都会被附加的属性，使用 .attachWith(@"") 不受影响`**
    
* ### [makeTemplate](#usage)

    - [**textTemplate**](#usage) **`全局声明一个 text 模板，以参数 identifier 来标识这个模板以重复使用，其使用方法和 text 一样`**
    
    - [**imageTemplate**](#usage) **`全局声明一个 image 模板，以参数 identifier 来标识这个模板以重复使用，其使用方法和 image 一样`**
    
    - [**allText**](#usage) **`全局声明所有使用 .attach() 的 text 都会被附加的属性，使用 .attachWith(@"") 不受影响`**
    
    - [**allImage**](#usage) **`全局声明所有使用 .attach() 的 image 都会被附加的属性，使用 .attachWith(@"") 不受影响`**

## Documents

### **font**

**font** 默认使用系统字体，如果只使用系统字体，它会让你的代码更简洁一些

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github.com").font(32).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/font.png" />

<p align="right"><a href="#indexes">back</a></p>

### **fontName**

**fontName** 使用字体名称来设定字体，字体的名称可以是默认的family名称，也可以是特定粗细的字体名称

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github.com").fontName(@"AmericanTypewriter",32).ln(1).attach();
    make.text(@"Github.com").fontName(@"AmericanTypewriter-Bold",32).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/fontName.png" />

<p align="right"><a href="#indexes">back</a></p>

### **fontRes**

**fontRes** 使用自定义的UIFont来设定字体，这里可以尽你所好

```objc
UIFont *font = [UIFont fontWithName:@"AmericanTypewriter" size:32];
UIFont *fontBold = [UIFont fontWithName:@"AmericanTypewriter-Bold" size:32];
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github.com").fontRes(font).ln(1).attach();
    make.text(@"Github.com").fontRes(fontBold).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/fontRes.png" />

<p align="right"><a href="#indexes">back</a></p>

### **fontStyle**

**fontStyle** 可以更加可读地去设定某个字体的风格，不过有一点要注意的是，这些得和字体本身的名字适配，如NUDBold风格，这需要相应字体拥有-Bold后缀,否则该方法将无效。

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github.com").fontName(@"AmericanTypewriter",32).fontStyle(NUDBold).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/fontStyle.png" />

<p align="right"><a href="#indexes">back</a></p>

### **bold**

**bold** 出于bold比较常用来考虑，将 bold 作为属性出现也许会更加易读一些，其使用效果等同于 fontStyle

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github.com").fontName(@"AmericanTypewriter",32).bold().attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/bold.png" />

<p align="right"><a href="#indexes">back</a></p>

### **color**

**color** 用于设定文字颜色，传入 UIColor 即可。

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().fontName(@"AmericanTypewriter",32).bold().attach();
    make.text(@"G").color([UIColor orangeColor]).attach();
    make.text(@"i").color([UIColor redColor]).attach();
    make.text(@"t").color([UIColor blueColor]).attach();
    make.text(@"h").color([UIColor magentaColor]).attach();
    make.text(@"u").color([UIColor brownColor]).attach();
    make.text(@"b").color([UIColor greenColor]).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/color.png" />

<p align="right"><a href="#indexes">back</a></p>

### **mark**

**mark** 用于设定文字的底色，就像在书本里为某一行文字涂抹马克笔一样，这样看起来比较形象

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().fontName(@"AmericanTypewriter",32).color([UIColor whiteColor]).bold().attach();
    make.text(@"G").mark([UIColor orangeColor]).attach();
    make.text(@"i").mark([UIColor redColor]).attach();
    make.text(@"t").mark([UIColor blueColor]).attach();
    make.text(@"h").mark([UIColor magentaColor]).attach();
    make.text(@"u").mark([UIColor brownColor]).attach();
    make.text(@"b").mark([UIColor greenColor]).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/mark.png" />

<p align="right"><a href="#indexes">back</a></p>

### **hollow**

**hollow** 可以让文字成为镂空状态，类似于艺术字那样,第一个参数可以边线的粗细，而第二个参数则可以设定颜色

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().fontName(@"AmericanTypewriter",32).bold().attach();
    make.text(@"G").hollow(4,[UIColor orangeColor]).attach();
    make.text(@"i").hollow(4,[UIColor redColor]).attach();
    make.text(@"t").hollow(4,[UIColor blueColor]).attach();
    make.text(@"h").hollow(4,[UIColor magentaColor]).attach();
    make.text(@"u").hollow(4,[UIColor brownColor]).attach();
    make.text(@"b").hollow(4,[UIColor greenColor]).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/hollow.png" />

<p align="right"><a href="#indexes">back</a></p>

### **solid**

**solid** 表示其为实心，这样的话，我们可以自定义夹心的颜色，让字体看起来更加艺术

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().fontName(@"AmericanTypewriter",64).bold().color([UIColor cyanColor]).attach();
    make.text(@"G").solid(4,[UIColor orangeColor]).attach();
    make.text(@"i").solid(4,[UIColor redColor]).attach();
    make.text(@"t").solid(4,[UIColor blueColor]).attach();
    make.text(@"h").solid(4,[UIColor magentaColor]).attach();
    make.text(@"u").solid(4,[UIColor brownColor]).attach();
    make.text(@"b").solid(4,[UIColor greenColor]).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/solid.png" />

<p align="right"><a href="#indexes">back</a></p>

### **link**

**link** 可以让文字成为一条链接，点击该链接将回调给予的方法。而在点击链接时，链接的背景会被置灰

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github.com").font(64).link(self,@selector(linkHandler:)).color([UIColor blueColor])._(NUD_,[UIColor blueColor]).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/link.png" />

<p align="right"><a href="#indexes">back</a></p>

### **_**

**_** 顾名思义，即下划线，使用它可以为你的文字加入下划线

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().fontName(@"AmericanTypewriter",32).bold().ln(1).color([UIColor cyanColor]).attach();
    make.text(@"Github")._(NUD_,[UIColor blueColor]).attach();
    make.text(@"Github")._(NUD__,[UIColor blackColor]).attach();
    make.text(@"Github")._(NUDThick_,[UIColor blueColor]).attach();
    make.text(@"Github")._(NUDDot,[UIColor blueColor]).attach();
    make.text(@"Github")._(NUDDotDot,[UIColor blueColor]).attach();
    make.text(@"Github")._(NUDDash,[UIColor blueColor]).attach();
    make.text(@"Github")._(NUDDashDot,[UIColor blueColor]).attach();
    make.text(@"Github")._(NUDDashDotDot,[UIColor blueColor]).attach();
    make.text(@"Github com")._(NUDDashDotDot|NUDByWord,[UIColor blueColor]).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/_.png" />

<p align="right"><a href="#indexes">back</a></p>

### **deprecated**

**deprecated** 加入删除线，表示该文字已经被删除了或者是被废弃了，相对来说可读性高一些

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github.com").font(64).deprecated([UIColor blackColor]).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/deprecated.png" />

<p align="right"><a href="#indexes">back</a></p>

### **skew**

**skew** 给文字加斜度,斜度根据传入的 CGFloat 值的大小决定其斜度

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().fontName(@"AmericanTypewriter",32).bold().ln(1).aligment(NUDAliCenter).attach();
    make.text(@"Github.com").skew(0.0).attach();
    make.text(@"Github.com").skew(0.2).attach();
    make.text(@"Github.com").skew(0.4).attach();
    make.text(@"Github.com").skew(0.8).attach();
    make.text(@"Github.com").skew(1).attach();
    make.text(@"Github.com").skew(1.2).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/skew.png" />

<p align="right"><a href="#indexes">back</a></p>

### **kern**

**kern** 设定文字的紧凑程度,根据传入的 CGFloat 值的大小决定其紧凑度

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().fontName(@"AmericanTypewriter",32).bold().ln(1).aligment(NUDAliCenter).attach();
    make.text(@"Github.com").kern(0.0).attach();
    make.text(@"Github.com").kern(0.2).attach();
    make.text(@"Github.com").kern(0.4).attach();
    make.text(@"Github.com").kern(0.8).attach();
    make.text(@"Github.com").kern(1).attach();
    make.text(@"Github.com").kern(1.2).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/kern.png" />

<p align="right"><a href="#indexes">back</a></p>

### **ln**

**ln** 设定该组件换行，使用该属性可以无需手动在text里添加\n，并且传入大于1的整数还可以多次换行，特别要注意的是，换行符的属性也和该组件的其他字符串等同

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github.com").font(32).ln(2).attach();
    make.text(@"Github.com").font(16).ln(2).attach();
    make.text(@"Github.com").font(32).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/ln.png" />

<p align="right"><a href="#indexes">back</a></p>

### **ligature**

**ligature** 声明文字为连体，使用之后，字体将会带有连体书写的效果，特别要注意的是，该属性只对部分支持连体的字母组以及字体有效

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"This is a attributed string").fontName(@"zapfino",23).ligature(NO).ln(1).attach();
    make.text(@"This is a attributed string").fontName(@"zapfino",23).ligature(YES).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/ligature.png" />

<p align="right"><a href="#indexes">back</a></p>

### **letterpress**

**letterpress** 声明文字带有印刷效果，

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github.com").font(64).letterpress().ln(1).attach();
    make.text(@"Github.com").font(64).ln(1).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/letterpress.png" />

<p align="right"><a href="#indexes">back</a></p>

### **vertical**

**vertical** 会让文字在垂直方向有一个偏移，传入一个CGFloat，如果大于0，则往上偏移，如果小于0，则往下偏移

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github").font(64).attach();
    make.text(@"[1]").font(15).color([UIColor blueColor]).vertical(35).ln(1).attach();
    make.text(@"Github").font(64).attach();
    make.text(@".com").font(32).color([UIColor orangeColor]).vertical(-20).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/vertical.png" />

<p align="right"><a href="#indexes">back</a></p>

### **stretch**

**stretch** 让文字在水平上有拉伸，其拉伸程度根据传入的CGFloat值而有所不同，换句话说，值越小越扁，越大越长

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github").font(64).ln(1).attach();
    make.text(@"Github").font(64).stretch(0.5).ln(1).attach();
    make.text(@"Github").font(64).stretch(-0.5).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/stretch.png" />

<p align="right"><a href="#indexes">back</a></p>

### **reverse**

**reverse** 让文字逆序书写

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github").font(64).ln(1).attach();
    make.text(@"Github").font(64).reverse().attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/reverse.png" />

<p align="right"><a href="#indexes">back</a></p>

### **shadow**

**shadow** 让文字带有默认的阴影效果，该效果让文字看起来会微小的凸起

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.text(@"Github").font(64).color([UIColor whiteColor]).shadow().attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/shadow.png" />

<p align="right"><a href="#indexes">back</a></p>

### **shadowDirection**

**shadowDirection** 让文字带有默认的阴影效果，并且可以定义阴影的四个最基本的方向：上下左右，至于第二个参数，则用于定义阴影的突出程度，需要注意的是，如果你用autolayout布局控件，它并不会因为阴影而自动扩大自己的frame，这时候你只能手动进行调整。

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(64).color([UIColor orangeColor]).aligment(NUDAliCenter).ln(1).attach();
    make.text(@"Github").shadowDirection(NUDLeft,10).attach();
    make.text(@"Github").shadowDirection(NUDRight,10).attach();
    make.text(@"Github").shadowDirection(NUDBottom,10).attach();
    make.text(@"Github").shadowDirection(NUDTop,10).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/shadowDirection.png" />

<p align="right"><a href="#indexes">back</a></p>

### **shadowOffset**

**shadowOffset** 让文字带有默认的阴影效果，并且可以完全自定义阴影的延伸方向

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(64).color([UIColor orangeColor]).aligment(NUDAliCenter).ln(1).attach();
    make.text(@"Github").shadowOffset(-5,-5).attach();
    make.text(@"Github").shadowOffset(5,5).attach();
    make.text(@"Github").shadowOffset(-5,5).attach();
    make.text(@"Github").shadowOffset(5,-5).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/shadowOffset.png" />

<p align="right"><a href="#indexes">back</a></p>

### **shadowBlur**

**shadowBlur** 让文字带有默认的阴影效果，并且可以完全自定义阴影的模糊程度，该值越高，则阴影越模糊，有种文字距离阴影越远的感觉

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(64).color([UIColor orangeColor]).shadowOffset(20,0).aligment(NUDAliCenter).ln(1).attach();
    make.text(@"Github").shadowBlur(0).attach();
    make.text(@"Github").shadowBlur(2).attach();
    make.text(@"Github").shadowBlur(4).attach();
    make.text(@"Github").shadowBlur(8).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/shadowBlur.png" />

<p align="right"><a href="#indexes">back</a></p>

### **shadowColor**

**shadowColor** 让文字带有默认的阴影效果，并且可以完全自定义阴影的颜色

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().shadowDirection(NUDRight,15).font(64).shadowBlur(3).color([UIColor orangeColor]).ln(1).attach();
    make.text(@"Github").shadowColor([UIColor redColor]).attach();
    make.text(@"Github").shadowColor([UIColor blueColor]).attach();
    make.text(@"Github").shadowColor([UIColor greenColor]).attach();
    make.text(@"Github").shadowColor([UIColor orangeColor]).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/shadowColor.png" />

<p align="right"><a href="#indexes">back</a></p>

### **shadowRes**

**shadowRes** 让文字带有自定义的阴影效果，你可以传入 NSShadow，自定义阴影。

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    NSShadow *shadow = [[NSShadow alloc] init];
    shadow.shadowOffset = CGSizeMake(8, 0);
    shadow.shadowColor = [UIColor redColor];
    shadow.shadowBlurRadius = 3;
    make.text(@"Github").shadowRes(shadow).font(64).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/shadowRes.png" />

<p align="right"><a href="#indexes">back</a></p>

### **lineSpacing**

**lineSpacing** 让文字可以声明文字换行时产生的行距

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(32).color([UIColor orangeColor]).ln(1).attach();
    make.text(@"Github").lineSpacing(0).attach();
    make.text(@"Github").lineSpacing(8).attach();
    make.text(@"Github").lineSpacing(16).attach();
    make.text(@"Github").lineSpacing(32).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/lineSpacing.png" />

<p align="right"><a href="#indexes">back</a></p>

### **lineHeight**

**lineHeight** 声明文字的行高，它会往上延伸,需要注意这里有三个参数：

**`minimumLineHeight`** 为第一个参数，它定义一行的小高度，换句话说，只要该值不为 0，则行高最少为 **`minimumLineHeight`**

**`maximumLineHeight`** 为第二个参数，它定义一行的大高度，换句话说，只要该值不为 0，则行高最大为 **`maximumLineHeight`**

**`lineHeightMultiple`** 为第三个参数，它定义一行行高的倍数，换句话说，如果该值不为 0，则会基于原始的行高乘以 **`lineHeightMultiple`**。

一行的高并不只决定于 **`lineHeightMultiple`** ，如果你限制了 **`minimumLineHeight`** 和 **`maximumLineHeight`** ，就算设定过大的 **`lineHeightMultiple`** ，也不会超出你所限制的范围。

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(32).color([UIColor orangeColor]).ln(1).attach();
    make.text(@"Github").lineHeight(0,0,4).mark([UIColor redColor]).attach();
    make.text(@"Github").lineHeight(0,100,4).mark([UIColor blueColor]).attach();
    make.text(@"Github").lineHeight(100,0,0).mark([UIColor greenColor]).attach();
    make.text(@"Github").lineHeight(100,0,1).mark([UIColor blueColor]).attach();
    make.text(@"Github").lineHeight(100,0,4).mark([UIColor redColor]).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/lineHeight.png" />

<p align="right"><a href="#indexes">back</a></p>

### **paraSpacing**

**paraSpacing** 让文字换行时产生额外的间距，分为两个参数：第一个参数为上间距，第二个参数为下间距

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(32).color([UIColor orangeColor]).ln(1).attach();
    make.text(@"Github").paraSpacing(20,20).mark([UIColor redColor]).attach();
    make.text(@"Github").paraSpacing(20,30).mark([UIColor blueColor]).attach();
    make.text(@"Github").paraSpacing(30,40).mark([UIColor greenColor]).attach();
    make.text(@"Github").paraSpacing(40,50).mark([UIColor blueColor]).attach();
    make.text(@"Github").paraSpacing(50,60).mark([UIColor redColor]).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/paraSpacing.png" />

<p align="right"><a href="#indexes">back</a></p>

### **aligment**

**aligment** 定义文字的对齐情况

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(32).color([UIColor orangeColor]).mark([UIColor greenColor]).ln(1).attach();
    make.text(@"Github").aligment(NUDAliLeft).attach();
    make.text(@"Github").aligment(NUDAliCenter).attach();
    make.text(@"Github").aligment(NUDAliRight).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/aligment.png" />

<p align="right"><a href="#indexes">back</a></p>

### **indent**

**indent** 定义文字的缩进，第一个参数为前缩进，第二个参数为后缩进

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(17).color([UIColor orangeColor]).mark([UIColor blueColor]).ln(2).indent(30,20).attach();
    make.text(@"GitHub Inc. is a web-based hosting service for version control using Git.").attach();
    make.text(@"It is mostly used for computer code.").indent(60,0).attach();
    make.text(@"It offers all of the distributed version control and source code management (SCM) functionality of Git as well as adding its own features.").indent(0,60).attach();
    make.text(@"It provides access control and several collaboration features such as bug tracking, feature requests, task management, and wikis for every project.").attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/indent.png" />

<p align="right"><a href="#indexes">back</a></p>

### **fl_headIndent**

**fl_headIndent** 定义文字的首行缩进，如果你定义过 indent 属性，它会在首行覆盖 indent 属性的前缩进属性

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(17).color([UIColor orangeColor]).mark([UIColor blueColor]).ln(1).fl_headIndent(40).attach();
    make.text(@"GitHub Inc. is a web-based hosting service for version control using Git. ").attach();
    make.text(@"It is mostly used for computer code. ").attach();
    make.text(@"It offers all of the distributed version control and source code management (SCM) functionality of Git as well as adding its own features.").attach();
    make.text(@"It provides access control and several collaboration features such as bug tracking, feature requests, task management, and wikis for every project.").attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/fl_headIndent.png" />

<p align="right"><a href="#indexes">back</a></p>

### **linebreak**

**linebreak** 定义文字的换行方式

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(17).color([UIColor orangeColor]).mark([UIColor blueColor]).ln(2).attach();
    make.text(@"It is mostly used for computer code.").linebreak(NUDWord).attach();
    make.text(@"It is mostly used for computer code.").linebreak(NUDChar).attach();
    make.text(@"It is mostly used for computer code.").linebreak(NUDClip).attach();
    make.text(@"It is mostly used for computer code.").linebreak(NUDTr_head).attach();
    make.text(@"It is mostly used for computer code.").linebreak(NUDTr_tail).attach();
    make.text(@"It is mostly used for computer code.").linebreak(NUDTr_middle).attach();
}];
```
<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/linebreak.png" />

<p align="right"><a href="#indexes">back</a></p>

### **highlight**

**highlight** 定义文字高亮，通过传入模板，可以定义高亮时文字的属性

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.textTemplate(@"Highlight").color([UIColor greenColor]).font(64).attach();
    make.text(@"Github").font(64).color([UIColor blackColor]).highlighted(@"Highlight").attach();
}];
```

<p align="right"><a href="#indexes">back</a></p>

### **tap**

**tap** 定义文字单击所产生的回调，该属性和 link 属性的效果是一致的，不过相比link属性，它的单击区域紧紧限制在文字之内，并且配合highlight方法，你可以做到完全自定义单击风格

```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.textTemplate(@"Highlight").color([UIColor greenColor]).font(64).attach();
    make.text(@"Github").font(64).color([UIColor blackColor]).highlighted(@"Highlight").tap(self,@selector(linkHandler:)).attach();
}];
```

<p align="right"><a href="#indexes">back</a></p>

### **origin**

**origin** 定义 image 的 起始点，注意，origin 的所在点为图片的左下角，baseline 所在的那条线上，即锚点为左下角，这和 UIView 的坐标系统是不一样的
```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(17).color([UIColor whiteColor]).attach();
    make.text(@"Hi,").attach();
    make.image(@"githubIcon").origin(0,0).size(100,100).attach();
    make.text(@"Github").attach();
}];
nude.backgroundColor = [UIColor purpleColor];
```

<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/origin.png" />

<p align="right"><a href="#indexes">back</a></p>

### **size**

**size** 定义 image 的尺寸，需要注意的是，layoutManager 并不会自动 resize 图片的尺寸，如果你在你的 image 组件里不定义 size 值，layoutManager 自动使用 UIImage 的 size 属性
```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(19).color([UIColor whiteColor]).attach();
    make.text(@"wow").attach();
    make.image(@"githubIcon").size(50,50).attach();
    make.text(@"yeah").attach();
    make.image(@"githubIcon").size(100,100).vertical(10).attach();
    make.text(@"right").attach();
}];
nude.backgroundColor = [UIColor purpleColor];
```

<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/size.png" />


### **vertical-image**

**vertical** 定义 image 的垂直位移，实际上只是 origin 修改 y 值的一个便利方法，由于 x 值设置无效，只使用此方法即可
```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(19).color([UIColor whiteColor]).attach();
    make.text(@"wow").attach();
    make.image(@"githubIcon").size(100,100).attach();
    make.text(@"yeah").attach();
    make.image(@"githubIcon").size(100,100).vertical(10).attach();
    make.text(@"right").attach();
}];
nude.backgroundColor = [UIColor purpleColor];
```

<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/vertical(image).png" />

<p align="right"><a href="#indexes">back</a></p>

### **aligment-image**

**aligment(image)** 定义 image 的水平对齐，传入 NUDAligment 即可，前提条件为 iamge 组件必须单独一行
```objc
NudeIn *nude = [NudeIn make:^(NUDTextMaker *make) {
    make.allText().font(19).color([UIColor whiteColor]).attach();
    make.allImage().size(100,100).ln(1).attach();
    make.image(@"githubIcon").aligment(NUDAliLeft).attach();
    make.text(@"Github").aligment(NUDAliCenter).ln(1).attach();
    make.image(@"githubIcon").aligment(NUDAliCenter).attach();
    make.text(@"Github").aligment(NUDAliCenter).ln(1).attach();
    make.image(@"githubIcon").aligment(NUDAliRight).attach();
    make.text(@"Github").aligment(NUDAliCenter).attach();
}];
nude.backgroundColor = [UIColor purpleColor];
```

<img src="https://github.com/hon-key/HKAttributedTextView/raw/master/Screenshots/aligment(image).png" />

<p align="right"><a href="#indexes">back</a></p>

### **ln-image**

**ln(image)** 定义换行

<p align="right"><a href="#indexes">back</a></p>

## License

NudeIn is released under the MIT license. See [LICENSE](https://github.com/hon-key/HKAttributedTextView/raw/master/LICENSE) for details.
