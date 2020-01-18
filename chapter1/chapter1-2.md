# 第一章 1-2 Hello, World
现在，你已经安装好了Rust,让我们来编写你的第一个Rust程序。学习一门薪的语言时打印“Hello World”时一个传统，所以我们也会从这里开始！

> 本书假设读者已经对命令行操作有基本的认识。Rust不会指定你使用什么编译器和工具来编写代码，如果你偏好使用IDE而不是命令行，请随意使用你最喜欢的IDE。许多IDE对Rust都有不同程度的支持，你可以查看IDE的文档获取详细的信息。最近，Rust团队也在专注于实现更好的IDE支持，并且在这方面取得了迅速的进展。

## 创建一个工程目录

创建一个目录来存储你的Rust代码。你的代码在哪里对于Rust来说不重要，但是对于本书中的联系和工程来说，我们建议创建一个*projects*的目录在你的家目录下并且把你所有的工程文件都放在里面。

打开终端然后键入以下的命令来为“Hello, World”项目创建*projects*目录。

在Linux, macOS 和 Windows的Power shell中，输入下面的命令

```shell
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```
在Windows的CMD中，输入下面的命令
```shell
> mkdir "%USERPROFILE%\projects"
> cd /d "%USERPROFILE%\projects"
> mkdir hello_world
> cd hello_world
```

## 编写并且运行Rust程序
下一步，创建一个薪的源文件命名为*main.rs*。Rust文件总是以 *.rs* 的后缀名结尾。如果你的文件名不止一个单词，使用下划线来分开他们，例如：使用 *hello_world.rs* 而不是*helloword.rs*。

现在打开*main.rs*文件，并且输入一面的代码

```rust
fn main() {
    println!("Hello World!");
}
```

保存文件然后回到终端窗口中。在Linux或者macOS中，输入以下命令来编译并且运行它
```shell
$ rustc main.rs
$ ./main
Hello, World!
```

在Windows中，输入命令```.\main.exe```而不是```./main```

```shell
> rustc main.rs
> .\main.exe
Hello, World!
```
不论你时什么操作系统，“Hello World”字符串都应该打印在你的终端中了。如果你没有看到这个输出，重新参考”Trouble Shooting“中安装的章节来获取帮助

如果```Hello World!```已经被打印了，恭喜你！你已经正式的写出了一个Rust程序。你称为了一名Rust程序员————欢迎！

## Rust程序剖析

让我们回顾在”Hello World“程序中发生的一些细节。
这里是第一个令人迷惑的地方
```rust
fn main() {

}
```

这一行在Rust定义了一个函数。```main```函数非常的特别：它是每个Rust可执行程序的第一个执行的代码。第一行声明了一个名为```main```的没有任何参数和返回值的函数。如果这儿有参数的话，应该放在圆括号()中。

注意函数体被花括号{}所包围，Rust它们必须包围住函数体。放置一个花括号{在行尾，然后空一行再放置另一个花括号}在开头的位置是一种很好的编程风格。

在写这篇文章的时候，一个自动的格式化工具```rustfmt```正在开发当中，如果你想在Rust工程中坚持标准风格，```rustfmt```会以特定的风格来格式化代码。Rust团队计划最终将在Rust的版本中包含这一工具，就像```rustc```一样。所以，当你阅读这本书的时候，这个工具已经安装在了你的计算机中。检查在线文档来获取更多的帮助信息。

在```main```函数中是下列的代码

```rust
println!("Hello World");
```

这一行代码做了这个小程序所有的工作：它打印了一行字在屏幕上。这里有四个重要的细节。
1. Rust风格的缩进是四个空格，而不是一个TAB符号
2. ```println!```被称为**宏**而不是函数，如果是函数的话，没有后面的感叹号！。我们将会在后面的第19章节详细的讨论宏。现在，你只需要知道使用！意味着你调用了一个宏而不是一个函数。
3. 你看 “Hello World”字符串，我们传递这个字符串作为参数到```println!```宏中，这个字符串就会被打印到屏幕上。
4. 我们以分号；来作为每一行的结尾，它表明了这一个表达式结束了并且下一行准备好开始。大部分的Rust代码都会以分号结尾。


## 编译和运行是分开的步骤

你刚刚运行了新创建的程序。所以让我们来检查这个过程的每一步。

在运行程序之前，你必须使用Rust编译器来编译它，使用```rustc```命令并且传递你的源程序名字作为参数。

```shell
$ rustc main.rs
```
如果你有C/C++的编程经验，你会注意到它和 ```gcc```或者```clang```十分的相似。编译完成后，Rust会输出一个可执行的二进制文件。

在Linux, macOS和Windows的Power shell中，你可以通过```ls```命令查看可执行文件。

```shell
$ ls
main.rs main.exe
```

以 *.rs* 后缀结尾的是源代码文件，可执行文件在Windows平台中以 *.exe* 结尾。如果你使用的是Windows平台，还会输出一个包含了调试信息的以 *.pdb*结尾的文件。 这里，你可以运行 *main* 或者 *main.exe*，就像这样

```shell
$ ./main  or .\main.exe on Windows
```

如果你对Ruby，Python或者JavaScript这一类动态语言更熟悉一些，你也许不曾将编译和运行作为分开的步骤。Rust是一个 *ahead-of-time(AOT)* 语言，这意味着你能编译一个程序并且可以给另一个人一个可执行的文件，他们即使不需要Rust也能够运行该程序。如果你给他们一个 *.rb*, *.py*, *.js* 文件，他们需要安装Ruby, Python, JavaScript的运行环境才能够运行他们。但是这些动态语言，你仅仅只需要一个命令就可以编译并且运行他们。这一切都是语言设计的权衡。

刚刚我们使用```rustc```编译了一个简单的程序，但是随着项目的增长，你会想要管理所有的选项并且像要更容易的分享你的代码。接下来，我们会介绍```Cargo```工具，它可以帮助你写出真正的Rust程序。