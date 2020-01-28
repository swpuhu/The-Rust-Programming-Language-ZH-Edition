# 数据类型

Rust 中的每一个值都有一种类型，它会告诉 Rust 数据的种类以便于 Rust 能够正确的工作。我们现在来看两种数据的子集：数值类型和复合类型(scalar and compound)。

记住，Rust 是一个静态类型语言，这意味着所有的变量都需要在编译时就明确其类型。编译器通常可以基础我们使用的值作出类型推断。但是当一个值可能有多个类型时，我们必须显式的指出变量的类型，就像这样：

```rust
#![allow(unused_variables)]
fn main() {
let guess: u32 = "42".parse().expect("Not a number!");
}
```

如果我们没有添加类型声明，Rust 就会显示下面的错误，这表示编译器需要更多的信息来明确我们想要使用的变量类型

```shell
error[E0282]: type annotations needed
 --> src/main.rs:2:9
  |
2 |     let guess = "42".parse().expect("Not a number!");
  |         ^^^^^
  |         |
  |         cannot infer type for `_`
  |         consider giving `guess` a type

```

## 数值类型

一个数值类型表示一个数值。Rust 有四种基本的数值类型： 整型数，浮点数，布尔，和字符。你也许在其他的编程语言中认识他们。让我们看看他们在 Rust 中如何工作的。

### 整数类型

整型就是没有小数部分的数字。我们在第二章中使用过一种整型，就是 `u32` 类型。这种类型表示它是无符号整数类型(有符号整数类型是以`i`开头的，而不是`u`)，它占据 32 位的内存空间。下表表示了 Rust 内置的整型。

| Length  | Signed  | Unsigned |
| ------- | ------- | -------- |
| 8-bit   | `i8`    | `u8`     |
| 16-bit  | `i16`   | `u16`    |
| 32-bit  | `i32`   | `u32`    |
| 64-bit  | `i64`   | `u64`    |
| 128-bit | `i128`  | `u128`   |
| arch    | `isize` | `usize`  |

每种类型都可以被显式的指定为有符号和无符号类型两种。_有符号_ 和 _无符号_ 表示这个数字可以为负数和不可以为负数。

每种有符号的变量可以表示的数字范围在 -(2<sup>n-1</sup>) ~ 2<sup>n-1</sup> - 1，n 表示该类型的数字占据多少位。所以`i8`类型的数字能表示-(2<sup>7-1</sup>) ~ 2<sup>7-1</sup> - 1 之间的数字。无符号类型的数字能够表示 0 ~ 2<sup>n</sup> - 1 之间的数字，所以 `u8` 类型的数字能够表示 0 ~ 2<sup>8</sup> - 1 之间的数字。

另外，`isize` 和 `usize` 类型取决于程序运行在哪种类型的电脑上，如果你运行在 64 位架构的电脑上就是 64 位，如果运行在 32 位架构的电脑上就是 32 位。

你也可以直接写出上表中表示的整型字面量。你可以使用类型后缀来表示不同的类型，例如`57u8`。

| Number literals | Example     |
| --------------- | ----------- |
| Decimal         | 98_222      |
| Hex             | 0xff        |
| Octal           | 0o77        |
| Binary          | 0b1111_0000 |
| Byte(`u8` only) | b'A'        |

所以你需要知道你要使用哪一种整型吗？如果你不确定，Rust 默认使用`i32`类型，这种类型通常是最快的，即使你用的 64 位的操作系统。

> ### 整数溢出
>
> 我们之前说过 `u8`类型的变量只能表示 0~255 之间的数字。如果你试图改变变量的值为 256。那么就会发生 _整数溢出_。对于整数溢出，Rust 有一些有趣的行为。当你处于调试模式时，Rust 会有整数溢出的检查以免你的程序在运行时 panic。Rust 使用 _Panic_ 这个术语来表示程序因为一个错误而退出。我们会在第九章中的 "Unrecoverable Errors with panic" 这一节中讨论更多的细节。

> 当你使用 `--release` 选项进入生产环境的模式中，Rust 不会检查整数溢出导致的错误。相反，如果整数溢出发生了，Rust 会执行二进制补码包装 _(two's complement wrapping)_ 。简单的说，如果一个值超过了其类型能够表示的最大值，那么就会被圆整到它能够表示的最小值。这个`u8`例子中，256 会变为 0，257 会变为 1，以此类推。程序不会 panic， 但是这个程序就不会按照你所想的那样运行。如果你想要显式的包装溢出，你可以使用标准库中的 `Wrapping` 类型。

### 浮点数类型

Rust 也有两种基本的浮点数类型。它们分别是 `f32` 和 `f64`，其表示分别占据 32 位的空间和 64 位的空间。默认的浮点数类型是 `f64`，因为在现代的 CPU 中他们的速度都大致相同，但是`f64`能够表示更准确的精度。

下面是一个使用浮点数的例子

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}

```

根据 IEEE-754 标准，`f32`表示表示单精度浮点数， `f64` 表示双精度浮点数。

### 数值运算

Rust 支持基础的数学运算：加，减，乘，除，取模。

```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;

    // remainder
    let remainder = 43 % 5;
}

```

附录 B 包含了 Rust 提供的所有的操作符。

### 布尔类型

正如大多数的编程语言，Rust 中的布尔类型有两个值： `true` 和 `false`。布尔值只占据一个字节。

```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}

```

使用布尔值的大多数方式是通过使用 `if` 表达式。我们会在 "Control Flow" 章节中讨论如何使用 `if` 表达式。

### 字符类型

到目前为止我们只使用了数字，但是 Rust 也支持字母。Rust 的 `char` 类型是基础的字母类型。下面的代码展示如何使用。(注意： `char` 字面量是用单引号，字符串使用的是双引号)

```rust
fn main() {
    let c = 'z';
    let z = 'ℤ';
    let heart_eyed_cat = '😻';
}

```

Rust 的 `char` 类型占用 4 个字节并且表示为 Unicode 编码，这意味着它能够表示不仅仅是 ASCII 码，中文，日文，韩文，emoji 表情，甚至零宽字符在 Rust 中都是合法的字符类型。Unicode 值从 `U+0000` 到 `U+D7FF` 和 `U+E000` 到 `U+10FFFF`。我们会在第八章的 "Storing UTF-8 Encoded Text with Strings" 中继续讨论这个话题。

## 复合类型

复合类型能够组合多个多个值到一个类型中。Rust 有两种基本的复合类型： tuples 和 arrays

### Tuple 类型

使用 tuple 来将不同的类型的变量组合在一起是一种常见的方式。Tuple 拥有固定的长度，一旦被声明了，它不能够扩张也不能收缩其大小。

我们通过在圆括号之间使用逗号来分隔不同的数值来创建 tuple。tuple 中的每个值都有一个类型，并且他们的类型可以不一样。如下例所示：

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}

```

`tup` 变量绑定了整个 tuple, 因为一个 tuple 被认为是一个单独的复合类型元素。为了得到 tuple 中单独的元素，我们可以使用模式匹配来解构 tuple 的值，如下所示

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}

```

这被称为 _解构_， 因为它将一个 tuple 分解为了 3 部分。

除了使用解构之外，我们还可以直接使用 `.`来读取 tuple 中的元素。如下：

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}

```

### Array 类型

存储多个数值集合的另一种方式是使用 _array_。与 tuple 不同的是，array 中的每一个元素必须拥有相同的类型。Rust 中的 array 与其他编程语言中的 array 有所不同，Rust 中的 array 拥有固定的长度，就像 tuple 一样。

在 Rust 中，在方括号中使用逗号将多个值分隔开就可以创建 array，如下：

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}

```

当你想要给你的数据在栈中分配空间而不是在堆中分配空间时使用 array 是非常有用的。（我们会在第四章中讨论栈和堆）或者你有一个固定大小的空间的集合也可以使用 array。array 不像 vector 类型那样灵活，vector 是标准库中所提供的一种简单的集合类型。它可以扩容和收缩，如果你不确定要使用 array 还是 vector，你应该使用 vector，第八章我们会讨论 vector。

你可以在方括号中使用分号来分隔开类型和 array 的大小，如下所示：

```rust

#![allow(unused_variables)]
fn main() {
let a: [i32; 5] = [1, 2, 3, 4, 5];
}
```

`i32` 表示每个元素的类型，分号后面的`5`表示 array 能够包含的元素数量。

#### 读取 Array 元素

一个 Array 是一片连续的在栈中分配的内存空间，你可以使用下标来读取 array 的元素

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}

```

在这个例子中，`first` 变量会得到数字 `1`，`second`变量会得到数字 `2`。

#### 非法的 Array 元素读取

如果你试图越界读取 array 的内容会发生什么？ 正如下面的例子，它会通过编译但是会在运行时发生一个错误：

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
    let index = 10;

    let element = a[index];

    println!("The value of element is: {}", element);
}

```

运行代码会产生下面的错误

```shell
$ cargo run
   Compiling arrays v0.1.0 (file:///projects/arrays)
    Finished dev [unoptimized + debuginfo] target(s) in 0.31 secs
     Running `target/debug/arrays`
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is
 10', src/main.rs:5:19
note: Run with `RUST_BACKTRACE=1` for a backtrace.

```

编译时不会产生任何错误，但是程序会在运行时引起错误。当你试图使用下标来读取 array 中的元素时，Rust 会检测你指定的下标是否小于 array 的大小，如果下标大于等于 array 的大小，Rust 会陷入 panic。

这是第一个 Rust 的安全法则，在许多低级的语言中，没有这种检查，并且当你提供了一个错误的下标，会读取到错误的内存中的值。Rust 会防止你范这种错误。第九章会讨论更多的 Rust 的错误处理细节。
