---
tags: 
 - 技术开发
grammar_cjkRuby: true
catalog: true
layout:  post
header-img: "img/header/P15.jpg"
preview-img: "/img/preview/P35.jpg"
---
洛谷的spj配置很资瓷啊，以下部分引用来自luogu官方链接

codeforces同理

[https://www.luogu.org/wiki/show?name=%E5%B8%AE%E5%8A%A9%EF%BC%9Aspecial%20judge](https://www.luogu.org/wiki/show?name=%E5%B8%AE%E5%8A%A9%EF%BC%9Aspecial%20judge)

搞了一上午1020导弹拦截的spj

# step1

先从链接中下载那个testlib-master文件解压后，在那个文件夹中创建checker.cpp

![这里写图片描述](http://img.blog.csdn.net/20171221113303726?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXdlcnR5MTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在程序中，有3个重要的结构体：inf指数据输入文件（本例没有），ouf指选手输出文件，ans指标准答案。

然后，可以从这3表结构体读入数据，不需要用到标准输入输出。如果读到的数据和下面的期望不一致，则spj返回fail结果。

# step2

写spj辣！以P1020导弹拦截为例

> 以下读入命令可以使用：（为引用部分）

> void registerTestlibCmd(argc, argv)

> 初始化checker，必须在最前面调用一次。

> char readChar()

> 读入一个char，指针后移一位。

> char readChar(char c)

> 和上面一样，但是只能读到一个字母c

> char readSpace()

> 同 readChar(' ').

> string readToken()

> 读入一个字符串，但是遇到空格、换行、eof为止、

> long long readLong()

> 读入一个longlong/int64

> long long readLong(long long L, long long R)

> 同上，但是限定范围（包括L，R）

> int readInt()

> 读入一个int

> int readInt(int L, int R),

> 同上，但是限定范围（包括L，R）`

> double readReal()

> 读入一个实数

> double readReal(double L, double R),

> 同上，但是限定范围（包括L，R）

> double readStrictReal(double L, double R, int minPrecision, int maxPrecision),

> 读入一个限定范围精度位数的实数。

> string readString(),

> string readLine()

> 碰撞一行string，到换行或者eof为止

> void readEoln()

> 读入一个换行符

> void readEof()

> 读入一个eof

> int eof()

读完数据后，就可以开始spj了。选手程序能用的功能，spj一样能用。在洛谷中，spj照样受到时间空间限制。而且不能标准输入输出。

最后就是输出啦。输出跟printf有点像。

```quitf(_ok, "The answer is correct. answer is %d", ans);```

给出AC

```quitf(_wa, "The answer is wrong: expected = %f, found = %f", jans, pans);```

给出WA

```quitp(0.5,"Partially Correct get %d percent", 50);```

给出PC(Partially Correct)，并且可以获得该点50%的分数

```
#include "testlib.h"

int main(int argc, char* argv[]) {
    registerTestlibCmd(argc, argv);
   	int pans1=ouf.readInt();
   	int jans1=ans.readInt();
   	ans.readEoln();
   	int pans2=ouf.readInt();
   	int jans2=ans.readInt();
	if(pans1!=jans1&&pans2==jans2)quitp(0.5,"Partially Correct get %d percent.On Ques 1,your ans is wrong:expected = %d,found = %d", 50,jans1,pans1);
    else if(pans1==jans1&&pans2!=jans2)quitp(0.5,"Partially Correct get %d percent.On Ques 2,your ans is wrong:expected = %d,found = %d", 50,jans2,pans2);
    else if(pans1==jans1&&pans2==jans2)quitf(_ok, "Good job!The answer is correct.");
    else quitf(_wa, "On Ques 1,your ans is wrong:expected = %d,found = %d", jans1, pans1);
}
```

# step3

将checker.cpp编译形成checker.exe文件

使cmd定位到testlib-master文件夹下

输入如下命令 CD空格+文件夹位置

![这里写图片描述](http://img.blog.csdn.net/20171221140334212?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXdlcnR5MTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后键入如下命令

checker.exe in.txt（输入文件） out.txt（输出文件） ans.txt（标准答案文件）

![这里写图片描述](http://img.blog.csdn.net/20171221140603909?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXdlcnR5MTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

显示结果如图

# step4

直接将checker.cpp（必须这个名字）塞入测试数据的压缩包内然后上传就行了