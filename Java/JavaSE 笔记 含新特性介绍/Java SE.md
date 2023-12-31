## 走进Java语言

前面我们介绍了 C 语言，它实际上就是通过编译，将我们可以看懂的代码，翻译为计算机能够直接执行的指令，这样计算机就可以按照我们想要的方式去进行计算了。当然，除了 C 语言之外，也有其他的语言，比如近几年也很火的 Python，它跟 C 语言不同，它并不会先进行编译，而是直接交给解释器解释执行：

```python
print("Hello World!")
```

可见，这种方式也可以让计算机按照我们的想法去进行工作。

一般来说，编程语言就分为两大类：

* **编译型语言：** 需要先编译为计算机可以直接执行的命令才可以运行。优点是计算机直接运行，性能高；缺点是与平台密切相关，在一种操作系统上编译的程序，无法在其他非同类操作系统上运行，比如 Windows 下的 exe 程序在 Mac 上就无法运行。
* **解释型语言：** 只需要通过解释器代为执行即可，不需要进行编译。优点是可以跨平台，因为解释是解释器的事情，只需要在各个平台上安装对应的解释器，代码不需要任何修改就可以直接运行；缺点是需要依靠解释器解释执行，效率肯定没直接编译成机器指令运行的快，并且会产生额外的资源占用。

> Java 语言（Java 之父：James Gosling，詹姆斯·高斯林）
> Write Once, Run Anywhere.（写一次，到处跑。）

这是 Java 语言的标语，它的目标很明确：一次编写，到处运行，它旨在打破平台的限制，让 Java 语言可以运行在任何平台上，并且不需要重新编译，实现跨平台运行。

Java 自 1995 年正式推出以来，已经度过了快 28 个春秋，而基于Java语言，我们的生活中也有了各种各样的应用：

* 诺基亚手机上的很多游戏都是使用Java编写的。
* 安卓系统中的各种应用程序也是使用Java编写的。
* 著名沙盒游戏《Minecraft》也有对应的Java版本，得益于 Java 跨平台特性，无论在什么操作系统上都可以玩到这款游戏。

（有关Java的详细发展历程，可以参考《Java核心技术·卷I》第一章）

可见，Java 实际上早已在我们生活中的各个地方扎根。那么，Java 语言是什么样的一个运行机制呢？

实际上我们的Java程序也是需要进行编译才可以运行的，这一点与C语言是一样的，Java程序编译之后会变成`.class`结尾的二进制文件：

不过不同的是，这种二进制文件计算机并不能直接运行，而是需要交给JVM（Java虚拟机）执行。

JVM是个什么东西呢？简单来说，它就像我们前面介绍的解释器一样，我们可以将编译完成的`.class`文件直接交给JVM去运行，而程序中要做的事情，也都是由它来告诉计算机该如何去执行。

在不同的操作系统下，都有着对应的JVM实现，我们只需要安装好就可以了，而我们程序员只需要将Java程序编译为`.class`文件就可以直接交给JVM运行，无论是什么操作系统，JVM都采用的同一套标准读取和执行`.class`文件，所以说我们编译之后，在任何平台都可以运行，实现跨平台。

由于Java又需要编译同时还需要依靠JVM解释执行，所以说Java **既是编译型语言，也是解释型语言。**

Java分为很多个版本：

* **JavaSE：** 是我们本教程的主要学习目标，它是标准版的Java，也是整个Java的最核心内容，在开始后续课程之前，这是我们不得不越过的一道坎，这个阶段一定要认真扎实地将Java学好，不然到了后面的高级部分，会很头疼。
* **JavaME：** 微缩版Java，已经基本没人用了。
* **JavaEE：** 企业级Java，比如网站开发，它是JavaSE阶段之后的主要学习方向。

从下节课开始，我们就正式地进行Java环境的安装和IDE的使用学习。

---

## 环境安装与IDE使用

前面我们介绍了Java语言，以及其本身的一些性质，这一部分我们就开始进行学习环境安装（这一部分请务必跟着操作，不要自作主张地去操作，一开始就出问题其实是最劝退新手的）

### JDK下载与安装

首先我们来介绍一下JDK和JRE，各位小伙伴一定要能够区分这两者才可以。

* **JRE（Java Runtime Environment）**：Java的运行环境，安装了运行环境之后，Java程序才可以运行，一般不做开发，只是需要运行Java程序直接按照JRE即可。
* **JDK（Java Development Kit）**：包含JRE，并且还附带了大量开发者工具，我们学习Java程序开发就使用JDK即可。

那么现在我们就去下载JDK吧，这里推荐安装免费的ZuluJDK：[https://www.azul.com/downloads/?version=java-8-lts&amp;package=jdk](https://www.azul.com/downloads/?version=java-8-lts&package=jdk)

在这里选择自己的操作系统对应的安装包：

**注意，这里不建议各位小伙伴去修改安装的位置！**新手只建议安装到默认位置（不要总担心C盘不够，该装的还是要装，尤其是这种环境，实在装不下就去将其他磁盘的空间分到C盘一部分）如果是因为没有安装到默认位置出现了任何问题，你要是找不到大佬问的话，又得重新来一遍，就很麻烦。

剩下的我们只需要一路点击Next即可，安装完成之后，我们打开CMD命令窗口（MacOS下直接打开“终端”）来验证一下（要打开CMD命令窗口，Windows11可以直接在下面的搜索框搜索cmd即可，或者直接在文件资源管理器路径栏输入cmd也可以）

我们直接输入java命令即可：


如果能够直接输出内容，说明环境已经安装成功了，正常情况下已经配置好了，我们不需要手动去配置什么环境变量，所以说安装好就别管了。

输入`java -version`可以查看当前安装的JDK版本：

只要是1.8.0就没问题了，后面的小版本号可能你们会比我的还要新。

这样我们就完成了Java环境的安装，我们可以来体验一下编写并且编译运行一个简单的Java程序，我们新建一个文本文档，命名为`Main.txt`（如果没有显示后缀名，需要在文件资源管理器中开启一下）然后用记事本打开，输入以下内容：

```java
public class Main{
		public static void main(String[] args){
				System.out.println("Hello World!");
		}
}
```

编辑好之后，保存退出，接着我们将文件的后缀名称修改为`.java`这是Java源程序文件的后缀名称

我们使用`cd`命令先进入到这个目录下

要编译一个Java程序，我们需要使用`javac`命令来进行

执行后，可以看到目录下多出来了一个`.class`文件

这样我们就成功编译了一个Java程序，然后我们就可以将其交给JVM运行了，我们直接使用`java`命令即可

注意不要加上后缀名称，直接输入文件名字即可，可以看到打印了一个 Hello World! 字样，我们的第一个Java程序就可以运行了。

### IDEA安装与使用

前面我们介绍了JDK开发环境的安装以及成功编译运行了我们的第一个Java程序。

但是我们发现，如果我们以后都使用记事本来进行Java程序开发的话，是不是效率太低了点？我们还要先编辑，然后要改后缀，还要敲命令来编译，有没有更加方便一点的写代码的工具呢？这里我们要介绍的是：**IntelliJ IDEA**（这里不推荐各位小伙伴使用Eclipse，因为操作上没有IDEA这么友好）

IDEA准确来说是一个集成开发环境（IDE），它集成了大量的开发工具，编写代码的错误检测、代码提示、一键完成编译运行等，非常方便。

下载地址：[IntelliJ IDEA：JetBrains 功能强大、符合人体工程学的 Java IDE](https://www.jetbrains.com.cn/idea/)