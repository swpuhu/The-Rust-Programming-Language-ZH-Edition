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
