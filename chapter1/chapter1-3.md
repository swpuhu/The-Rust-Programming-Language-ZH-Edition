# 第一章 1-3 Hello, Cargo!

Cargo 是Rust的构建系统和包管理器。大部分的Rust程序员使用这个工具来管理他们的Rust项目，因为Cargo为你的做了大量的任务，例如构建你的代码，下载你的代码所依赖的库并且构建这些库（我们将你所依赖的库称为 *依赖*）

类似我们刚刚编写的程序，没有任何依赖。因此我们用Cargo来构建 “Hello World” 程序只会使用到Cargo的一部分能力。当你写更复杂的Rust程序时，你会添加很多依赖，如果你用Cargo来开始你的项目，添加依赖会变得十分的容易

因为绝大多数的Rust项目都使用Cargo， 本书剩余的部分也假设你也在使用Cargo。如果你使用了官方的安装包，Cargo已经和Rust一同安装好了。如果你使用了其他的方式来安装Cargo，通过以下的命令来检查Cargo是否安装好了。

```shell
$ cargo --version
```

如果你看到了版本号，你已经拥有了Cargo。如果你看到了报错，例如```command not found```, 请查看文档如何单独安装Cargo。

## 使用Cargo创建一个项目
让我们使用Cargo来创建一个新的项目并且观察与我们之前的“Hello World”项目有什么不同。回到你之前的 *project*目中下，运行下列命令：

```shell
$ cargo new hello_cargo
$ cd hello_cargo
```
第一条命令创建了一个名为 *hello_cargo* 的目录。我们将我们的工程命名为 *hello_cargo* ，Cargo就创建了一个同名的目录

进入到 *hello_cargo*目录中。你会看到Cargo为我们生成了一个文件和一个目录： 一个*Cargo.toml* 文件和一个 *src* 目录，该目录下有一个 *main.rs* 文件。并且也会有一个新的被初始化了的带有 *.gitigonore* 文件 的Git仓库。

> Git 是一个通用的版本管理工具。你可以通过使用 ```--vsc``` flag来改变cargo new 来使用不同的版本管理同居。运行```cargo new --help```来查看可选信息

在你的编辑器中打开 *Cargo.toml*。它看起来如下所示：

``` toml
[package]
name = "hello_cargo"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```
该文件是以TOML(Tom's Obvious, Minimal Language)格式编写，它是Cargo的配置格式。

第一行， ```[package]```表示接下里的西信息是一个包的配置信息。当我们给这个文件添加更多的信息时，我们再进行讨论。

接下来的四行设置了编译程序所需要的配置信息
名字， 版本信息。 Cargo会通过你的环境来获取这些信息，如果这些信息不正确，你可以手动修改他们并且保存该文件。我们将会在附录E中讨论```edition```关键字

最后一行, ```[dependencies]```，是你项目的依赖部分的开始。在Rust中，代码包被称为 *crates*。 这个项目中我们不需要其他的 crates，但是在本章的第一个项目中，我们还是会使用dependencies这个部分。

现在打开 *src/main.rs*

```rust
fn main () {
    println!("Hello, world!");
}
```
Cargo已经为你生成了一个“Hello World”的程序，就像我们刚刚写的程序那样。目前为止，我们之前的项目和Cargo生成的项目不同的地方就是代码放置的位置改变了，并且我们在根目录有了 *Cargo.toml* 配置文件。

Cargo期望你的源文件都放置在 *src* 目录下。根目录仅仅只放置一些例如 README 文件，许可信息，配置文件等等与你的代码无关的内容。使用Cargo来帮助你组织你的项目。这个地方可以放一切东西，但是每样东西都应该在它的位置上。

如果你开始了一个没有使用Cargo的项目，正如我们刚刚建立的“Hello World”项目，你可以将它转换成Cargo项目。移动项目代码到 *src* 目录并且创建一个恰当的 *Cargo.tmml* 文件就可以了。

## 构建并且运行Cargo项目

现在让我们观察一下构建和运行程序和刚刚有什么不同，回到你的 *hello_cargo* 目录下，输入以下命令：

```shell
$ cargo build
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 2.85 secs
```
这个命令创建了一个可执行的文件在这个路径 *target/debug/hello_cargo* (或者是 *target/debug/hello_cargo.exe*)而不是在你当前的目录下。你可以运行它用下面的命令

```shell
$ ./target/debug/hello_cargo # or .\target\debug\hello_cargo.exe on Windows
Hello, world!
```
如果一切正常，```Hello, world```将会被打印在终端中。第一次运行```cargo build```会使Cargo在根目录下创建一个 *Cargo.lock* 的文件。该文件是对项目现有依赖的跟踪。这个项目中并没有依赖，所以这个文件的内容有点少，你不需要手动的改变这个文件，Cargo会自己管理里面的内容。

我们刚刚利用```cargo build``` 构建了这个项目，但是我们也可以使用```cargo run```这一条命令来编译代码并且运行结果。

```shell
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

注意这一次我们没有看到输出表明 Cargo 编译了 hello_cargo。Cargo表明这个文件没有被改变，所以仅仅只是运行二进制可执行文件。如果你修改你的源代码，Cargo会在运行之前重新构建项目，并且你会看到以下的输出

```shell
$ cargo run
   Compiling hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.33 secs
     Running `target/debug/hello_cargo`
Hello, world!
```

Cargo也提供了一个命令叫做```cargo check```。这个命令会快速的检查你的代码并且不会产生可执行文件

```shell
$ cargo check
   Checking hello_cargo v0.1.0 (file:///projects/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.32 secs
```

为什么我们不想要一个可执行文件？通常，```cargo check```比 ```cargo build```快非常多，因为它跳过了生成可执行文件的不走。如果你在写diamagnetic的过程中频繁的检查你的工作，使用```cargo check```会加速你的处理过程。像这样，许多Rust开发者定期的运行```cargo check```来保证他们的代码能够被编译。然后在他们需要可执行文件时运行```cargo build```命令。

让我们来扼要重述到目前为止我们学习到了什么

- 我们可以使用```cargo check```或者```cargo build```来构建项目
- 我们可以使用```cargo run```来一步构建项目并且运行
- Cargo会将构建的结果保存在 *target/debug* 目录下，而不是和源代码在同一个目录

另一个使用Cargo的好处就是无论你时使用的什么操作系统，Cargo的命令都是一样的。所以这时候，我们不再需要为不同的平台指定不同的操作指令了。

## 构建发行版本
当你的项目最终准备发行，你可以使用```cargo build --release```命令来进行优化编译。该命令会创建 *target/release* 目录来代替之前的 *target/debug*目录。这些优化会使你的Rust代码运行的更快，但是这也会延长编译时间，这也是为什么这儿有两个不同的配置文件：一个为开发准备，当你想要快速的重新构建时；另一个使最终的程序尽可能的运行的更快。如果你要测试你的代码运行素材，确保运行```cargo build --release```命令然后再测试你的程序。

## Cargo的约定
在简单的项目中，除了使用```rustc```，Cargo没有提供太多的价值，但是当你的程序变得复杂时，它将证明它的价值。那些由许多复杂的包组成的项目，使用Cargo来构建项目将会变得容易许多

即使这个```hello_cargo```项目十分的简单，它也使用了需要以后你的Rust生涯中真正要使用的工具。实际上，为了处理现有的项目，你可以使用下面的命令来签出代码，进入到项目目录中，然后构建它：

```shell
$ git clone someurl.com/someproject
$ cd someproject
$ cargo build
```
更多的信息，请参考[文档](https
://doc.rust-lang.org/cargo/)

## 总结
你现在已经完成了你的Rust之旅的开始，在本章中，你已经学到了：

- 使用```rustup```安装最新的Rust稳定版本
- 更新Rust版本
- 打开本地离线文档
- 直接使用```rustc```编译并且运行“Hello World"程序
- 使用Cargo来创建和运行一个新项目

现在是一个非常好的时机来构建更多的实质性的程序以让你习惯于阅读和编写Rust程序。所以在第二章，我们会构建一个guessing game程序。如果你宁愿从一个普通的程序如何工作开始，请看第三章再跳回第二章。