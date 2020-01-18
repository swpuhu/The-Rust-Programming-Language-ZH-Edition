# 第二章 2-1 编写猜测游戏

让我们亲自动手一起完成项目来进入Rust的世界中。本章将会为你介绍Rust中的一些通用的概念并且为你演示如何在真正的程序中使用它们。你会学习到```let```, ```match```, 方法，关联函数，使用外部库等等！下面的章节会继续更详细的探索这些特性。本章你将会练习基础。

我们将以一个经典的程序初学者的问题开始： 一个猜谜游戏。这是它的原理：这个程序先生成一个1到100的随机数。然后提示玩家输入一个数来猜是多少，玩家输入完毕后，程序会提示玩家猜测的数字是低了还是高了。如果玩家才对了，程序会打印出恭喜的信息并且推出。

## 建立新项目

为了建立新的项目，我们回到 *projects* 目录下，并且使用Cargo来创建新项目

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

这段打开保存了很多信息，让我们来仔细的一行一行的解读。为了获得玩家的输入并且作为结果打印出来，我们需要引入```io```库到作用域中。```io```库包含在标准库中

```rust
use std::io;
```

默认的，Rust在 *prelude* 中引入了一些类型。如果你想要使用的类型不在其中，你必须通过```use```表达式显式的引入到作用域中。使用```std:io```库给你提供一些有用的特性，其中也报错接受用户的输入。

正如你在第一章看到的那样，```main```函数是程序的起点
```rust
fn main () {
```

```fn```语法声明了一个新的函数，圆括号表示没有参数传入， ```{```表示函数体的开始
与你在第一章中学习的一样， ```println!```是一个宏，它打印一个字符串到屏幕上：

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

现在程序变得有趣起来了！这一行中有很多东西，注意这里的```let```声明，它被用来创建一个变量。这里有另一个例子：

```rust
let foo = bar;
```

这一行创建了一个新的变量```foo```并且与变量```bar```的值绑定在一起，在Rust中，变量的值默认是 **不可变** 的。我们将会在第三章的 [变量和可变性](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html#variables-and-mutability)中详细讨论。下面的例子展示了如何使用```mut```使变量变为可变的。

```rust
let foo = 5; // immutable
let mut bar = 5; // mutable
```

> 注意： ``` // ``` 语法表示一行注释的开始直到这一行结束。

让我们回到猜测游戏程序。你现在知道```let mut guess```将声明一个可变的名为guess的变量，另一方面, ```=``` 等于符号将调用```String::new```的结果绑定在guess上，```String::new```这个函数返回一个```String```的实例。 ```String```是由标准库提供的字符串类型。

在```::new```这一行中的```::```语法表明```new```是一个```String```类型的关联函数( *associated function* )。一个关联函数被实现在一种类型中。在本例中的```String```而不是在某种特定的```String```实例中。一些语言把它称为静态方法( *static method* )

```new```函数创建了一个新的，空字符串，你会发现```new```函数在许多类型中，因为这是可以创建一些值的函数的通用名字。

总结一下，```let mut guess = String::new();```这一行创建了一个可变的变量，并且被赋予了一个新的空字符串作为它的值。哇哦！

