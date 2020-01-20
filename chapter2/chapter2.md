# 第二章 2-1 编写猜测游戏

让我们亲自动手一起完成项目来进入 Rust 的世界中。本章将会为你介绍 Rust 中的一些通用的概念并且为你演示如何在真正的程序中使用它们。你会学习到`let`, `match`, 方法，关联函数，使用外部库等等！下面的章节会继续更详细的探索这些特性。本章你将会练习基础。

我们将以一个经典的程序初学者的问题开始： 一个猜谜游戏。这是它的原理：这个程序先生成一个 1 到 100 的随机数。然后提示玩家输入一个数来猜是多少，玩家输入完毕后，程序会提示玩家猜测的数字是低了还是高了。如果玩家才对了，程序会打印出恭喜的信息并且推出。

## 建立新项目

为了建立新的项目，我们回到 _projects_ 目录下，并且使用 Cargo 来创建新项目

```shell
$ cargo new guessing_game
$ cd guessing_game
```

## 处理猜测

这个猜测游戏第一步程序会要求玩家输入，然后处理输入并且校验这个输入是否是期望值。

```rust
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

这段打开保存了很多信息，让我们来仔细的一行一行的解读。为了获得玩家的输入并且作为结果打印出来，我们需要引入`io`库到作用域中。`io`库包含在标准库中

```rust
use std::io;
```

默认的，Rust 在 _prelude_ 中引入了一些类型。如果你想要使用的类型不在其中，你必须通过`use`表达式显式的引入到作用域中。使用`std:io`库给你提供一些有用的特性，其中也报错接受用户的输入。

正如你在第一章看到的那样，`main`函数是程序的起点

```rust
fn main () {
```

`fn`语法声明了一个新的函数，圆括号表示没有参数传入， `{`表示函数体的开始
与你在第一章中学习的一样， `println!`是一个宏，它打印一个字符串到屏幕上：

```rust
println!("Guess the number!");

println!("Please input your guess.");
```

这段代码打印了一段提示语来告诉玩家游戏的名字并且要求玩家输入。

## 用变量来保存值

下一步，我们创建了一个地方来保存玩家的输入内容，就像这样：

```rust
let mut guess = String::new();
```

现在程序变得有趣起来了！这一行中有很多东西，注意这里的`let`声明，它被用来创建一个变量。这里有另一个例子：

```rust
let foo = bar;
```

这一行创建了一个新的变量`foo`并且与变量`bar`的值绑定在一起，在 Rust 中，变量的值默认是 **不可变** 的。我们将会在第三章的 [变量和可变性](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html#variables-and-mutability)中详细讨论。下面的例子展示了如何使用`mut`使变量变为可变的。

```rust
let foo = 5; // immutable
let mut bar = 5; // mutable
```

> 注意： `//` 语法表示一行注释的开始直到这一行结束。

让我们回到猜测游戏程序。你现在知道`let mut guess`将声明一个可变的名为 guess 的变量，另一方面, `=` 等于符号将调用`String::new`的结果绑定在 guess 上，`String::new`这个函数返回一个`String`的实例。 `String`是由标准库提供的字符串类型。

在`::new`这一行中的`::`语法表明`new`是一个`String`类型的关联函数( _associated function_ )。一个关联函数被实现在一种类型中。在本例中的`String`而不是在某种特定的`String`实例中。一些语言把它称为静态方法( _static method_ )

`new`函数创建了一个新的，空字符串，你会发现`new`函数在许多类型中，因为这是可以创建一些值的函数的通用名字。

总结一下，`let mut guess = String::new();`这一行创建了一个可变的变量，并且被赋予了一个新的空字符串作为它的值。哇哦！

回忆使用标准库中的`std::io`。现在我们使用`io`模块中的`stdin`函数

```rust
io::stdin().read_line(&mut guess)
            .expect("Failed to read line");
```

如果我们没有把`std::io`放在程序的开头，我也可以在通过调用`std::io::stdin`来使用这个函数。`stdin`函数返回一个`std::io::Stdin`的实例，它是一种表示可以处理来自终端的标准输入流的类型。

在代码的下一部分， `read_line(&mut guess)`, 调用了`read_line`方法来处理玩家的输入。我们也给`read_line`函数传递了一个参数`&mut guess`。

`read_line`的任务是将玩家输入的任何内容什么转换为字符串，所以它接受一个字符串作为其参数。这个字符串需要是可变的以便于该方法能够根据玩家的输入改变字符串的内容。

`&`符号表示这个参数是一个引用，引用给予你一种方式能够让你代码的多个部分来春去同一片数据，而不用将数据多次的拷贝进内存中。引用是一种复杂的特性，并且 Rust 的一个主要的优点就是可以安全和简单的引用。你不需要知道更多的细节来完成这个程序。 现在，你需要知道就是类似变量，引用默认都是不可变的。因此，你需要写`&mut guess`而不是 `guess`来使其可变。（第四章将会更彻底的解释引用）

## 使用 `Result`类型来处理潜在的错误

这一行代码我们还没有做完。即使我们目前位置只讨论一行简单的代码文本，它仅仅只是单个代码逻辑的第一部分，第二部分是这个方法：

```rust
.expect("Failed to read line");
```

当你用`.foo()`语法来调用一个方法时，引入换行和空格来打破长的代码是非常明智的。我们可以这样书写代码：

```rust
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

然而，一行很长的代码时难以阅读的，所以最好的方法就是将他们分开为两行，变为两个方法调用。现在我们来讨论以下这行代码干了什么。

如前所述，`read_line`把玩家的输入转为字符串传递到我们传入的参数中。但是它也会返回一个值————在本例中，是`io::Result`。

`Result`类型是枚举类型(enumerations)，通常也被称为 _enums_。美剧类型是一种拥有一系列固定值的类型，这些值被称为枚举变量。第六章会详细的介绍枚举。

对于`Result`类型，它的枚举变量是 `Ok`或者`Err`。`Ok`变量表明操作成功，`Err`变量意味着操作失败了，并且`Err`白能量包含了为什么操作会失败的信息。

`Result`类型的意图是编码错误的操作信息，`Result`类型的值有一些定义在内部的方法。 `io::Result`的实例有一个 `expect` 方法供你调用。如果`io::Result`实例是一个`Err`类型的值， `expect`会时程序中断并且将你传递到这个函数的参数作为错误信息显示出来。如果`read_line`方法返回了`Err`， 可能是遇到了来自操作系统底层的错误。如果`io::Result`的值是`Ok`类型的值， `expect`会返回之前方法的返回值。这本例中，这个值是玩家向标准输入流中输入的字节数。

如果你不调用`expect`，程序可以完成编译，但是你会得到一个警告

```shell
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
warning: unused `std::result::Result` which must be used
  --> src/main.rs:10:5
   |
10 |     io::stdin().read_line(&mut guess);
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: #[warn(unused_must_use)] on by default
```

Rust 警告你没有使用来自`read_line`方法返回的`Result`类型的值，这表明这个程序可能没有处理潜在的错误。

正确的废止错误的方式是真正的去写一个异常处理，但是你仅仅只是想当一个错误发生时就让程序崩溃，你可以使用`expect`。你可以在第九章学习到如何从一个错误中恢复程序。

## 使用`println!`和占位符来打印值

```rust
println!("You guessed: {}", guess);
```

这一行打印了一个我们保存的玩家的输入的字符串。这一对花括号`{}`就是一个占位符。想象 `{}`就像是小螃蟹的一对钳子在这里钳住了一个值。你可以使用花括号来打印不止一个值。第一个花括号表示第一个值放在这里，第二个花括号表示第二个值放在这里，以此类推。打印多个值就像下面这样

```rust
#![allow(unused_variables)]
fn main() {
let x = 5;
let y = 10;

println!("x = {} and y = {}", x, y);
}

```

这段代码将会打印出`x = 5 and y = 10`

## 测试第一部分

让我们测试猜测游戏的第一部分，使用`cargo run`来运行程序

```shell
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```

此时，这个游戏的第一部分已经完成了。我们获取了键盘的输入并且将其打印了出来。

## 生成一个秘密数字

下一步，我们需要生成一个秘密数字以供玩家来猜测这个数是多少。这个秘密数字应该在程序每次运行的时候都不一样，这样游戏才能更有趣而不会让人玩一次就厌烦。我们生成一个 1 到 100 的随机数，这样游戏不会太难。Rust 的标准库中目前没有包含随机数的生成函数。然而，Rust 团队提供了一个`rand`包。

## 使用包来获得更多的功能

记住一个包就是 Rust 源代码文件的集合。这个我们构建的工程就是一个可执行的二进制的包。`rand`包是一个库，它包含了可能将要被用在其他程序中的代码。

Cargo 的外部包的用法是真正的亮点。在我们使用`rand`包在写代码之前，我们需要修改 _Cargo.toml_ 文件来将`rand`包作为我们的依赖项。 现在打开文件并且在 `[dependencies]`部分的下方添加：

```toml
[dependencies]
rand = "0.5.5"
```

在 *Cargo.toml*文件中，在一个头标签下的所有内容都属于这个头标签直到遇到下一个标签。 `[dependencies]`告诉 Cargo 你的项目依赖了哪些包的哪个版本。在本例中，我们指定`rand`包为 0.5.5 语义版本。这是一种描述版本的标准。0.5.5 实际上是 `^0.5.5`的速记，这意味着与版本 0.5.5 版本有公共的可兼容的 API 的版本都可以。

现在，不改变你的代码，重新构建项目

```shell
$ cargo build
    Updating crates.io index
  Downloaded rand v0.5.5
  Downloaded libc v0.2.62
  Downloaded rand_core v0.2.2
  Downloaded rand_core v0.3.1
  Downloaded rand_core v0.4.2
   Compiling rand_core v0.4.2
   Compiling libc v0.2.62
   Compiling rand_core v0.3.1
   Compiling rand_core v0.2.2
   Compiling rand v0.5.5
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 s
```

可能你会看到不同的版本信息（但是他们都是与其兼容的）并且打印的行数可能有写不一样。

现在我们拥有了一个外部的依赖，Cargo 从 _registry_ 拉取最新的版本信息，然后从 _Crates.io_ 中复制数据。 在 Rust 生态中，Crates.io 是一个 Rust 程序员在这里开源代码以供其他人使用的地方。

> 译者注：
> 国内用户都被墙了，所以需要配置一下 crates 的镜像来加速。目前只有 USTC 的镜像可用。在你的家目录(C:\Users\Administrator)下找到 ./cargo 文件夹，打开这个文件夹在其中新建一个 config 文件（没有后缀名），用记事本打开输入下面的内容

```toml
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with = 'ustc'
[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"
```

> 就 OK 了。

在更新了 registry 之后，Cargo 会检查`[dependencies]`的内容并且下载你现在没有的包。在本例中，我们只是列出了`rand`作为我们的包，Cargo 也抓取了`libc`和`rand_core`两个包，因为`rand`包依赖了这两个包才能工作。下载完成之后，Rust 会编译他们然后编译这个项目。

## *Cargo.lock*文件确保项目是可重新构建的

Cargo 拥有一项机制来确保你能够重新构建同样的工程，无论是你还是其他人，无论构建多少次代码，最终的结果都是一样的。Cargo 将只使用你指定的版本直到你指定其他的版本。例如，如果`rand`的版本下一周更新为了 0.5.6 并且可能引起你的代码崩溃那么会发生什么呢？

这个问题的答案就是 _Cargo.lock_ 文件，这个文件在你第一次运行`cargo build`命令时就被创建了。当你第一次构建项目的时候，Cargo 计算出所有的依赖版本并且将其写入到 *Cargo.lock*文件中。当你以后构建项目的时候，Cargo 会检查 _Cargo.lock_ 文件并且使用其中指定的版本而不是又一次的做计算版本的事情。这就是能让你能够自动的反复构建版本的基础。换句话说，你的版本会一直保持在 0.5.5 直到你显式的更新。感谢 _Cargo.lock_ 文件。

## 更新包的新版本

当你想要更新包的时候， Cargo 提供了另一个命令`update`, 它会忽视 *Cargo.lock*文件并且计算出所有适合你 _Cargo.toml_ 配置文件的标准的版本信息，如果一切起作用了，Cargo 会将这些版本信息写入到 *Cargo.lock*文件

默认的，Cargo 仅仅只会查找大于 0.5.5 而小于 0.6.0 的版本。如果`rand`包已经发型了两个新的版本`0.5.5`和`0.6.0`，当你运行`cargo update`时会看到下列的信息

```shell
$ cargo update
    Updating crates.io index
    Updating rand v0.5.5 -> v0.5.6

```

这个时候，你注意到你使用的是 0.5.6 版本的`rand`

如果你想要使用 0.6.0 版本的`rand`或者 0.6.x 系列的其他版本，你必须要手动的更新 *Cargo.toml*文件像下面这样：

```toml
[dependencies]
rand = "0.6.0"
```

你下一次运行`cargo build`命令时， Cargo 会根据你指定的新的版本来更新这个包的注册表并且重新评估你的`rand`包的要求。

这里说了很多关于 Cargo 和其生态的内容，这一部分的内容我们会在第 14 章再讨论，但是现在，这就是你所需要了解的。Cargo 让库复用变得更容易，所以 Rust 开发者能够写出用若干包组装而成的更小的工程。

## 生成一个随机数

现在，你添加了`rand`包到 *Cargo.toml*文件中了，让我们开始使用`rand`。下一步就是更新 _src/main.rs_

```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

首先，我们添加了一行： `use rand:Rng`。`Rng`traits 定义了随机数生成的方法，并且这个 traits 必须在作用域中。第 10 章会详细的介绍 traits。

下一步，我们在中间的部分添加了两行.`rand::thread_rng`函数将会给我们一个特别的随机数字生成器。然后我们调用随机数生成器的`gen_range()`方法。该方法被`Rng`trait 所定义并且我们用`use rand::Rng`语句将其带入了作用域中。`gen_range()`方法接受两个参数并且生成一个在这两个数之间的随机数。下边界被包含在其中，上边界没有被包含在其中，所以我们需要指定 1 和 101 作为参数来产生一个 1 到 100 的随机数。

我们添加的代码中第二行打印了这个随机数，当我们开发一个程序的时候是一个非常有用的手段。但是我们会从最终的版本中删除它，如果一开始就知道了游戏的答案，这就不是什么游戏了。

试着多运行几遍这个程序

```rust
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4
$ cargo run
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
```

你应该得到不同的随机数并且他们都应该在 1 到 100 之间，非常好！

## 比较猜测数与神秘数字

现在我们有了玩家的输入和随机数，我们现在来比较他们。如下面的代码所示，但下面的代码不能被编译，随后我们会解释这是为什么

```rust
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {

    // ---snip---

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

第一行新增加了一个`use`语句，从标准库中引入了称为`std::cmp::Ordering`类型。类似`Result`类型，`Ordering`类型是另一个枚举值，但是其枚举变量是`Less`, `Greater` 和 `Equal`。当你比较三个值的时候会产生三种结果。

接下来我们在末尾添加了 5 行代码。`cmp`方法比较两个值，这个方法能够被任何可以被比较的值所调用。它接受一个你想要比较的引用值： 本例中是比较`guess`值与`secret_number`值。随后其会返回一个`Ordering`类型的枚举值。我们使用一个`match`表达式来决定下一步要做什么。

一个`match`表达式是由 *arms*所组成的。一个 _arms_ 是由一个 _pattern_ 组成并且一旦给出的值与 _arms_ 的值所匹配，就会执行相应的表达式。Rust 传递值给`match`表达式并且轮流的匹配 _arm_ 模式。`match`是 Rust 中一个非常强大的功能可以让你表达你可能遇到的各种场景并且处理他们。这个特性会在第 6 章和第 18 章详细介绍。

让我们看看本例中在`match`表达式中发生了什么。加入玩家猜了一个为 50 的数字并且随机生成的数字是 38.当我们的代码比较 50 和 38 时，`cmp`方法会返回一个`Ordering::Greater`类型的值，因为 50 比 38 要大。`match`表达式得到这个值后就开始与 *arm*中的模式依次比较。让我们看看第一个 _arm_ 的匹配模式， `Ordering::Less` ， 这似乎看起来与 `Ordering::Greater`不匹配，所以`match`忽略了这行代码并且移动到下一条 *arm*中进行匹配。下一个 _arm_ 中的模式： `Ordering::Greater`与 `Ordering::Greater` 完全匹配。 _arm_ 中的相关代码会被执行并且打印出 `Too big!`到屏幕上。并且`match`表达式也会终止因为已经匹配完成了不要再去匹配剩下的 _arm_ 了。

然后，这些代码还是不能被编译，让我们试着编译一下：

```Rust
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
error[E0308]: mismatched types
  --> src/main.rs:23:21
   |
23 |     match guess.cmp(&secret_number) {
   |                     ^^^^^^^^^^^^^^ expected struct `std::string::String`, found integer
   |
   = note: expected type `&std::string::String`
   = note:    found type `&{integer}`

error: aborting due to previous error
Could not compile `guessing_game`.
```

这个错误的核心就是类型不匹配。Rust 拥有一个健全的静态类型系统。而且 Rust 还拥有类型推断的能力。当我们写出`let mut guess = String::new()` 这样的代码时， Rust 能够推断出`guess`变量应该是一个字符串类型的变量所以不需要我们再写出类型。`secret_number`变量，是一个数字类型的变量。有些数字类型的变量都能够表示 1~100 的数字： `i32`， 有符号 32 位整数，`u32`，无符号 32 位整数，`i64`，有符号 64 位整数等等都能够表示 1~100 的数字。Rust 默认数字类型变量是`i32`类型的，除非你再别处添加额外的信息让 Rust 能够推断出它是不同的数字类型。这就是 Rust 不能比较数字和字符串类型的原因。

最终，我们想要将字符串转换为数字类型以便于我们与神秘数字进行比较。我们可以再添加下面的两行代码

```Rust
// --snip--

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = guess.trim().parse()
        .expect("Please type a number!");

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}

```

新的两行是：

```Rust
let guess: u32 = guess.trim().parse()
    .expect("Please type a number!");

```

我们创建了一个名为`guess`的变量。等等！我们刚刚不是已经创建了一个名为`guess`的变量了吗？但是 Rust 允许我们用一个新的名为`guess`的变量去 *遮蔽*之前的变量。在你想要转为一个值的类型到另一种类型时，这个特性非常的有用。 _遮蔽_ 这个特性允许我们重用`guess`这个变量名，而不是去创建两个不一样的变量，例如`guess_str`和`guess`。（我们会在第三章详细介绍 _遮蔽_）

我们绑定了一个表达式`guess.trim().parse()`到`guess`上。在表达式中的`guess`指的是玩家输入的字符串类型的`guess`。在`String`实例上的`trim`方法会移除字符串首尾的空白符。用户最后必须按下 Enter 键来满足`read_line`的条件，但是`u32`只包含数字字符。当玩家按下 Enter 键时，一个换行符被添加到了末尾。例如：如果玩家输入了“5”这个字符并且按下 Enter，`guess`变量看起来是这样的：`5\n`，`\n`表示换行。`trim`方法就会排除掉这个换行符，仅仅只保留“5”这个字符。

在字符串中的`parse`方法解析字符串为某种类型的数字。因为这个方法可以解析各种数字类型，我们需要通过`let guess: u32`表达式告诉 Rust 我们要想的到底是哪种类型。`guess`值后的冒号（：）告诉 Rust 我们声明的变量的类型。Rust 有一些内建的数字变量。这里的`u32`就是无符号的 32 位整数。这对于一些小的正整数来说是一个很好的默认选择。你会在第 3 章学习到其他的数字类型。另外，由于有`u32`这个注解并且与`secrect_number`进行比较，那么 Rust 就会将`secret_number`也推断为`u32`类型的数字。现在我们要比较的两个数都是同一种类型了。

调用`parse`非常容易引起一些错误。例如，字符串可能包含`%A`或其他字符，它是没有办法被转换为数字的。`parse`方法会返回一个`Result`类型，类似于`read_line`方法。我们也再一次使用`expect`方法来处理它。如果`parse`方法返回一个`Err`，调用`expect`就会导致程序中止并且将我们给出的错误信息打印在屏幕上。如果`parse`呢个够成功的转换字符串为数字，它会返回`Ok`类型的枚举值，并且`expect`会返回我们想要的数字。

让我们运行这个程序！

```Rust
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43 secs
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!

```

非常好！
