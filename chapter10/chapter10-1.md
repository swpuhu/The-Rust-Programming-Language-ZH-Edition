# 通用数据类型

我们使用泛型来创建函数签名或者是结构体的定义，这样我们就不必关心数据类型。让我们先来看看如何使用泛型来定义函数，结构体，枚举和方法。

## 在函数定义中使用泛型

当我们使用泛型来定义函数，我们在函数签名中使用泛型。这样会使我们的代码变得更加的灵活并且给调用者提供了更多的功能，也提高了代码的复用性。

继续我们的 `largest` 函数，下面展示两个函数在 slice 中找到最大的值

```rust
fn largest_i32(list: &[i32]) -> i32 {
    let mut largest = list[0];

    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn largest_char(list: &[char]) -> char {
    let mut largest = list[0];

    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest_i32(&number_list);
    println!("The largest number is {}", result);
   assert_eq!(result, 100);

    let char_list = vec!['y', 'm', 'a', 'q'];

    let result = largest_char(&char_list);

println!("The largest char is {}", result);
   assert_eq!(result, 'y');
}

```

`largest_i32` 函数的功能是找到一个 slice 中最大的`i32`类型的数字。 `largest_char`函数的功能是找到 slice 中最大的 `char`类型的字符。它们之间都拥有相同的代码，我们现在来介绍以下通过泛型来排除重复的代码。

为了参数化我们定义的新函数，我们需要命名参数的类型，正如我们为函数参数值做的那样。你可以使用任何的标识符作为参数类型的名字。但是我们习惯于使用 `T`，约定俗成，Rust 中的参数类型名字通常是一个短的字母。

当我们在函数体中使用参数时，我们需要在函数签名中声明参数名字。简单地说，当我们要在函数签名中使用类型参数名，我们就必须先声明类型参数。为了定义通用的`largest`函数，先在尖括号之中放入类型名字，尖括号`<>` 放置在函数名和参数列表之间，就像这样：

```rust
fn largest<T>(list: &[T]) -> T {
```

我们来看看这个定义： `largest`函数是一个通用函数。这个函数有一个参数，名为 list, 它是一个包含`T` 类型的 slice。这个函数会返回一个 `T` 类型的值。

下面的代码展示了使用泛型的函数。这个函数展示了我们能够接受`i32`类型 slice 的参数，也能够接受`char`类型 slice 的参数。注意这个代码暂时还不能够被编译，但是我们会在后面修复这个问题

```rust
fn largest<T>(list: &[T]) -> T {
    let mut largest = list[0];

    for &item in list.iter() {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest(&number_list);
    println!("The largest number is {}", result);

    let char_list = vec!['y', 'm', 'a', 'q'];

    let result = largest(&char_list);
    println!("The largest char is {}", result);
}

```

如果我们立刻编译这个代码，我们会得到这个错误：

```shell
error[E0369]: binary operation `>` cannot be applied to type `T`
 --> src/main.rs:5:12
  |
5 |         if item > largest {
  |            ^^^^^^^^^^^^^^
  |
  = note: an implementation of `std::cmp::PartialOrd` might be missing for `T`

```

这个提示 `std::cmp::PatialOrd` 是一个 _trait_. 我们会在下一节讨论 _trait_。现在，`largest` 不能工作的原因可能是因为 `T` 类型。因为我们想要比较 `T` 类型的值。我们只能使用能够被排序的值。为了使其能够比较，标准库中有 `std::cmp::PartialOrd` trait 的接口以供实现（查看附录 C 查看更多的信息）。你能学习到如何指定一个有特定 trait 的泛型，但是现在让我们来探索使用泛型的其他方式。

## 在结构体定义中使用泛型

我们也能够在结构体中使用泛型在一个或多个域中。下面的代码定义了如何定义一个 `Point<T>` 的结构体。

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
}

```

在使用结构体中使用泛型的语法与在函数中使用泛型的语法类似。首先，我们在结构体名字的后面使用了一个尖括号。然后我们在结构体定义中我们指定相关的数据使用泛型。

我们只使用了一个泛型来定义 `Point<T>`，这个定义说的是 `Point<T>` 结构体是某种类型 `T` 的泛型，它的域 `x` 和 `y` 都是同一种的类型，无论这个类型是什么类型。如果我们创建了不同类型的 `Point<T>`的实例，如下面的代码所示：

```rust
struct Point<T> {
    x: T,
    y: T,
}

fn main() {
    let wont_work = Point { x: 5, y: 4.0 };
}

```

这个例子中，我们给 `x` 赋值为 5，编译器知道泛型 `T` 就是整型。然后给 `y` 赋值为 4，我们会得到下面的错误：

```rust
error[E0308]: mismatched types
 --> src/main.rs:7:38
  |
7 |     let wont_work = Point { x: 5, y: 4.0 };
  |                                      ^^^ expected integer, found
floating-point number
  |
  = note: expected type `{integer}`
             found type `{float}`

```

为了定义在 `Point` 结构使用不同的泛型，我们可以使用多泛型类型参数。例如，我们改变`Point`定义如下：

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

fn main() {
    let both_integer = Point { x: 5, y: 10 };
    let both_float = Point { x: 1.0, y: 4.0 };
    let integer_and_float = Point { x: 5, y: 4.0 };
}

```

现在所有类型的 `Point`实例都已经可以使用了！你可以在定义中使用多泛型类型参数，但是使用多个泛型会让你的代码阅读变得困难起来。当你需要使用许多泛型时，你可能需要重构你的代码变为多个几个小的片段。

### 在枚举中使用泛型

与我们在结构体中使用泛型那样，我们可以在枚举定义中使用泛型。让我们看看标准库中提供的 `Option<T>`枚举类型。

```rust
enum Option<T> {
    Some(T),
    None
}
```

这个定义现在对于你应该更有意义了。正如你看见的， `Option<T>` 是一个 T 类型的泛型枚举，它定义了两种枚举变量。通过使用 `Option<T>` 枚举，我们能够表达有可选值的抽象概念，并且由于 `Option<T>` 是泛型的，无论这个可选值是什么，我们都可以使用这个抽象概念。

枚举也可以使用多个泛型。我们在第九章中使用过的 `Result` 泛型定义就是一个例子

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

## 在方法中使用泛型

我们可以在结构体和枚举中使用泛型来实现方法。下面的代码展示了我们定义的结构体实现了一个名为 `x` 的方法。

```rust
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

fn main() {
    let p = Point { x: 5, y: 10 };

    println!("p.x = {}", p.x());
}

```

这里我们定义了一个名为`x`的方法，它返回了一个`x` 的引用。

我们在 `impl` 关键字后声明了 `T`，我们可以使用它来指定我们在 `Point<T>` 类型中实现的方法。通过在 `impl` 后面声明 `T`, Rust 可以鉴别在 `Point`的尖括号中的类型是泛型而不是一个确定的类型。

例如，我们只是想要在 `Point<f32>` 类型实现一个方法而不是在泛型的 `Point<T>` 结构体中实现一个方法。这样我们就不需要在 `impl` 后面声明任何类型。

```rust

#![allow(unused_variables)]
fn main() {
struct Point<T> {
    x: T,
    y: T,
}

impl Point<f32> {
    fn distance_from_origin(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}
}
```

上面的代码意思是`Point<f32>`类型的结构体有一个 `distance_from_origin`的方法并且其他的`Point<T>`类型的实例，如果 `T` 不是 `f32` 类型的，那么就没有这个方法。这个方法是测量一个点距离远点的距离，并且它只对浮点数的计算有效。

在结构体中的泛型参数不总是相同的。如下代码所示，在结构体`Point<T, U>`中的`mixup`方法接受了另一个`Point`结构体作为参数，它可能也与调用这个方法的`Point`类型不一致。这个方法创建了一个新的`Point`实例，它的 x 值来自于它本身，它的 y 值来源于参数`Point`的 y 值。

```rust
struct Point<T, U> {
    x: T,
    y: U,
}

impl<T, U> Point<T, U> {
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 5, y: 10.4 };
    let p2 = Point { x: "Hello", y: 'c'};

    let p3 = p1.mixup(p2);

    println!("p3.x = {}, p3.y = {}", p3.x, p3.y);
}

```

## 使用泛型的代码性能

当你使用泛型时你可能会担心这会导致运行时的性能开销。但是有个好消息是 Rust 实现泛型的方式并不会导致使用泛型而造成性能下降。

Rust 在编译时完成了代码的单态化(monomorphization)。 _Monomorphization_ 是一种在编译时将特定的特性填充到泛型中的处理过程。

让我们用一个例子来看看在枚举中是如何工作的。

```rust

#![allow(unused_variables)]
fn main() {
let integer = Some(5);
let float = Some(5.0);
}

```

当 Rust 编译这个代码时就会执行单态化。在这个过程中，编译器读取了在`Option<T>`实例中使用了泛型的值，并且识别出两种类型：其中一种是`i32`类型，另一种是`f64`类型。这样就将`Option<T>`的定义扩展到了`Option_i32`和`Option_f64`，从而使用指定的类型替换了泛型。

单态化的过程就像下面所示。`Option<T>` 泛型被编译器用特定的定义所替代。

```rust
enum Option_i32 {
    Some(i32),
    None,
}

enum Option_f64 {
    Some(f64),
    None,
}

fn main() {
    let integer = Option_i32::Some(5);
    let float = Option_f64::Some(5.0);
}

```

因为 Rust 编译泛型代码为指定类型的代码，所以我们不会因为使用泛型而付出运行时的性能损失。当这个代码运行时，它正如我们之前手动的一个一个的定义重复的函数那样。单态化的处理使 Rust 的泛型在运行时有很高效的性能。
