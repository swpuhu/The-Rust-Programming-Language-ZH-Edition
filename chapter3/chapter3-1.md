# 变量和可变性

正如我们在第 2 章提到的，变量默认是不可变的。这是许多保证代码安全和并发性的手段之一。然而，你还是需要一个选项来使变量变得可变。让我们探索为什么 Rust 鼓励你支持不可变。

当一个变量是不可变的，一旦一个值绑定到了一个变量上，你就不能改变这个值。为了说明这一切，我们开始一个名为 _variables_ 新的项目。

然后，在这个项目文件夹中，打开 _src/main.rs_ 文件然后替换为下面的代码

```Rust
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}

```

保存并且运行这个程序，你会得到下面这个错误信息

```shell
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         - first assignment to `x`
3 |     println!("The value of x is: {}", x);
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable

```

这个例子展示了编译器是如何帮助你在你的程序中找到错误的。即使编译器错误会让你感到沮丧，这意味着你想要做的事情是不安全的。它不意味着你不是一个不好的程序员。有经验的 Rust 程序员也会得到这些错误。

这个错误信息 `cannot assign twice to immutable variable x` 表示你不能给一个不可变的变量 x 赋值两次。

我们得到的编译时的错误信息是非常重要的，因为他们有可能导致 bug。如果你的一部分程序绝对不会改变这个值，然而你的另一部分程序改变了这个值。那么你的第一部分的程序可能就不会按照你的预期所运行。并且这种 bug 也是难以追踪的，尤其是第二部分的 bug 只是寥寥几次改变了这个值而已。

在 Rust 中，编译器保证了当你声明了一个变量后它永远都不会被改变。这意味着当你阅读和编写代码时，你不需要去记忆和追踪这个值何时会发生变化。这样你的代码就会变得很容易通过编译。

但是可变性是非常有用的。变量只有在默认情况下是不可变的。正如你在第 2 章中所做的，你可以通过在变量名前面添加 `mut` 关键字来使变量变得可变。

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}

```

现在我们来运行这个程序

```shell
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
    Finished dev [unoptimized + debuginfo] target(s) in 0.30 secs
     Running `target/debug/variables`
The value of x is: 5
The value of x is: 6

```

当使用了 `mut` 后我们可以将 x 的值从 5 改变为 6。

这是一种为了阻止 bug 的权衡。例如，当你使用了一个很大的数据结构，使其变得可变会比通过返回一个新的实例来的更快。当你使用一个更小的结构时，创建新的实例可能更容易阅读，通过降低的性能来换取可读性也是物有所值的。

## 变量与常量的不同

不能够改变变量的值也许会使你想起其他编程语言中的一个特性： _constants_ ，类似不可变的变量，常量也使不允许被改变的。但是这二者之间还是有一些不同的地方。

第一，你不能在常量前面使用 `mut` ，常量不仅仅是默认是不可变的，它永远都是不可变的。

你通过 `const` 关键字而不是 `let` 关键字来声明一个变量，那么它的类型必须被声明。 我们会在下一节介绍数据类型。所以你现在不需要担心它的细节，你现在只需要知道你总是需要声明类型。

常量能够被声明在任何的作用域中，包括全局作用域。

最后一个不同是常量只能被常量表达式所赋值，而不能被在运行时通过调用函数获得其返回值来对常量赋值。

这儿有一个例子，`MAX_POINTS`常量被赋值为 100,000(Rust 中通常使用大写字母表示常量，在单词之间使用下划线 `_` 分隔开，在数字之间也可以使用下划线来增强可读性)

```rust
const MAX_POINTS: u32 = 100_000;
```

将你的程序中的那些硬编码的值转化为常量是十分有用的。它会帮助你在未来更好的重构你的代码。

## Shadowing

正如你在第 2 章中所见的，你可以重复声明一个和之前变量名字一样的变量，并且这个新的变量会遮蔽前一个变量。Rust 程序员将其称作第一个变量被第二个所遮蔽。

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    let x = x * 2;

    println!("The value of x is: {}", x);
}

```

这个程序首先给 `x` 赋值 `5` , 然后 `x` 被 `let x =` 语句所遮蔽，该语句给 `x` 增加了 1，所以现在 `x` 的值是 6。第三个 `let` 也会遮蔽之前的 `x` ，所以现在的值是 12。运行这个程序，会得到以下的输出：

```shell
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
    Finished dev [unoptimized + debuginfo] target(s) in 0.31 secs
     Running `target/debug/variables`
The value of x is: 12

```

Shadow 是与 `mut`不同的，因为如果我们偶然的给一个变量重新赋值而没有使用 `let` 关键字，我们会得到一个编译时的错误。

另一个 shadow 与 `mut` 不同的地方是我们可以高效的创建一个新的变量，我们可以改变这个值的类型并且重用这个变量名。例如，我们需要计算用户输入了多少个空格，来对单词之间的间隔进行调整。

```rust

#![allow(unused_variables)]
fn main() {
let spaces = "   ";
let spaces = spaces.len();
}

```

这个结构是合理的，因为第一个`spaces` 变量是字符串类型并且第二个`spaces`变量是一个新的数字类型的变量。Shadowing 避免我们引入两个变量名，我们可以重用 `spaces` 变量名。然而，如果我们试图使用 `mut` ，我们会得到一个编译时的错误

```rust
let mut spaces = "   ";
spaces = spaces.len();
```

这个错误表示我们不能够改变变量的类型

```shell
error[E0308]: mismatched types
 --> src/main.rs:3:14
  |
3 |     spaces = spaces.len();
  |              ^^^^^^^^^^^^ expected &str, found usize
  |
  = note: expected type `&str`
             found type `usize`

```

现在，我们已经探索了变量的工作原理，下一章我们来看看数据类型。
