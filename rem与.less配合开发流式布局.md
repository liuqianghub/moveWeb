css3中单位rem与.less结合布局
rem是CSS3新增的一个相对单位（root em，根em），这个单位引起了广泛关注。这个单位与em有什么区别呢？区别在于使用rem为元素设定字体大小时，仍然是相对大小，但相对的只是HTML根元素。

通常的，我们把html的font-size设置成0.625%;因为浏览器默认的字体大小是16px,所以font-size:0.625也就相当于16px*0.625=10px;往后我们要设置字体大小的时候,就可以用rem了.
例如font-size:1.2rem; 就相当于 font-size:12px;了
但可能有一些浏览器的默认字体不是16px.所以我们可以直接把html的font-size设置成10px;就可以确保万无一失.
然后还有个问题,谷歌浏览器的最小文字大小为12px;假如把rem用到宽高,行高,边距的时候,html设定的10px并不起作用.会默认按照12px来计算.因此最好我们只把rem用到字体大小上.

通常的,我们的wap端页面设计稿的宽度为640px或者750px;我们想要按照设计稿的样子,尽可能不变形地适应各种宽度的显示屏.我们就需要字体大小宽高等全都用rem.

这时候,我们可以使用less来进行计算.在不同的浏览器宽度时,给html赋予不同的默认字体大小,且字体大小与屏幕宽度的比例是一样的.如下:
(为了配合谷歌浏览器最小字体为12px;所以我们把最窄的屏幕宽度320对应的字体大小设为12px; )

复制代码
1  @media screen and (min-width:320px){html{font-size:12px;}}
2  @media screen and (min-width:346px){html{font-size:13px;}}
3  @media screen and (min-width:352px){html{font-size:14px;}}
4  @media screen and (min-width:400px){html{font-size:15px;}}
5  @media screen and (min-width:480px){html{font-size:18px;}}
6  @media screen and (min-width:560px){html{font-size:21px;}}
7  @media screen and (min-width:640px){html{font-size:24px;}}
复制代码
 

另外就是,我们平时切图,可能要按照比例来,例如640宽的设计图,我们的字体大小,行高之类的,可能就取设计稿大小的一半.
这在320px宽的屏幕中显示是非常正常的. 但在稍微宽一点的屏幕里, 整个页面跟设计稿比起来就相对比较扁了..
(我想过一个wap布局的办法,就是按照设计图的大小,把页面设计出来,然后通过zoom来改变缩放比例,达到宽度100%的效果,貌似大家都不提倡这样,谁能告诉我原因?)
为了能根据设计图,通过很少的计算就能布局,我们用less,计算出一个10px大小的值,我们命名为rex;

 1 @rex : 5rem/12; /* 640的设计图时: 5rem/12; 750的设计图时: 16rem/45 */ 

然后这个@rex我们就可以按照设计图的尺寸,当它是10px来使用.(可以省去很多的计算)

例如在640宽的设计图里, 有一行字体大小为36px; 行高为100px的字.我们就可以写成

 1 .txt{font-size:3.6*@rex; line-height:10*@rex;} 

这样一来,在各个宽度下看的样子都几乎是一样的,不会有被"压扁"的现象存在.