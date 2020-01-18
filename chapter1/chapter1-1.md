# 第一章 1-1 安装
## 安装

第一步是安装Rust。我们通过```rustup```来下载Rust，这是一个管理Rust版本和其相关联工具的命令行工具。你需要连接因特网来完成下载

> 注意： 如果你处于某种考虑倾向不使用```rustup```，请阅读这篇文章[the Rust installation page](https://www.rust-lang.org/tools/install)

接下来的步骤安装Rust编译器的最新的稳定版。Rust的稳定性保障确保了本书中的所有例子都能够被最新的Rust版本编译。由于Rust通常提供了一些错误和警告的消息，运行的结果也许在不同的版本之间有一些不同。换句话说，你安装的任何稳定的Rust版本都会得到本书内容中所期望的结果

> ### 命令行注解：
> 在本章甚至贯穿本书，我们会在终端中像你展示一些命令。你按下Enter时，终端中会出现一个以 '$'符号开始的行，你不需要去键入 '$'符号，它表示的时每一行的开始。 那些不以 '$' 符号开头的行通常展示的是上一条命令的结果。 另外，PowerShell会使用 '>'符号来代替'$'符号。

## 在Linux / macOS上安装```rustup```

如果你正在使用Linux或者macOS， 打开一个终端然后输入以下的命令

```shell
curl https://sh.rustup.rs -sSf|sh
```

该命令会下载一个脚本并且开始安装```rustup```工具，它会默认安装最近的稳定版本。可能你会被要求输入你的密码。如果安装成功了，下列的信息会被展示在命令行中

```shell
Rust is installed now. Great!
```
安装脚本会在你下次登录时自动的给Rust添加环境变量到你的计算机系统中。如果你想要立刻使用Rust而不是等到下次登陆时，运行下面的命令来手动添加环境变量到你的计算机中

```shell
source $HOME/.cargo/env
```

你也可以添加下面这一行语句到你的 *~/.bash_profile*文件中

```shell
export PATH="$HOME/.cargo/bin:$PATH"
```

另外，你还需要某种链接器。很有可能有一个链接器已经被安装了，然而当你试图编译一个Rust程序并且得到了错误信息，那么表示这个链接器时不可用的。这意味着你需要手动安装一个。C语言的编译器通常会自带一个链接器。检查你的平台文档如何安装一个C语言的编译器。一些通用的Rust包依赖了C语言编写的代码，所以你需要一个C语言的编译器。

## 在Windows上安装```rustup```

在Windows平台， 前往 https://www.rust-lang.org/tools/install 并且按照安装步骤来安装Rust。在某一时刻，你会接受到一条消息你需要安装Visual Studio 2013 或者更新的C++ build Tools。一个最简单的获取这些build tools的方法时安装 Visual Studio 2019.

本书剩下的使用命令行的部分都会在cmd.exe 和 Power shell中进行。如果有不同的地方，我们会解释如何使用。

## 更新和卸载

当你通过```rustup```安装了Rust后，更新最新的版本非常的简单。通过你的终端程序，运行下面的更新脚本

```shell
rustup update
```

卸载Rust和```rustup```，运行下面的卸载命令

```shell
rustup self uninstall
```

## 故障排除

 检查是否正确安装了Rust，打开终端并且输入以下命令
 ```shell
rustc --version
 ```

 你应该看到以下格式的版本信息，提交HASH和提交日期

 ```shell
rustc x.y.z (abcabcabc yyy-mm-dd)
 ```

 如果你看到了这些信息，你已经成功的安装了Rust!如果你没有看到这些信息并且你使用的时Windows平台。检查Rust是否在你的环境变量的PATH中。如果这些也是正确的然而Rust还是不能正确工作。这里有许多地方你能获取帮助。最方便是在[official Rust Discord](https://discord.gg/rust-lang) #beginner channel。在这里，你可以与其他的Rust开发者聊天。在StackOverflow 和 [the User forum](https://users.rust-lang.org/) 中也有一些很棒的资源。

 ## 本地文档

Rust的安装包含了一份本地的文档副本，你能够离线的阅读它。运行```rustup doc```在你的浏览器中打开本地文档

任何一个标准库中的函数当你不确定它的行为或者你不知道如何使用它，使用API文档来进行查询！