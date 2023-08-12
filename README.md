# Rust Programmer

rustup：Rust 工具链安装程序和更新程序。
该工具用于在 Rust 新版本发布时安装和更新 rustc 和 Cargo。

rustc：Rust 编译器，它将 .rs 文件转换为二进制文件和其他中间格式。

## Cargo 依赖管理器和构建工具

Cargo 用于构建和运行 Rust 应用程序的标准工具。

Cargo 知道如何下载依赖项，通常托管在 https://crates.io 上，并且在构建项目时会将它们传递给 rustc 。 
Cargo 还带有一个内置的测试运行器，用于执行单元测试。

```bash
# 查看 cargo 版本
cargo --version

# cargo 创建一个新的项目
cargo new hello_cargo

# cargo 构建项目
cargo build

# cargo 编译和运行项目
cargo run

# cargo 检查当前的代码是否可以通过编译
cargo check
```

Cargo 使用 TOML（Tom's Obvious, Minimal Language）作为标准的配置格式。

## 示例代码

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

## 通用编程概念

### 变量与可变性

Rust 中的变量默认是不可变的。可以通过在声明的变量名称前添加 **mut 关键字** 来使其可变。

```rust
fn main() {
  let x = 5;
  println!("The value of x is: {}", x);
  
  x = 6; // error cannot assign twice to immutable variable
  println!("The value of x is: {}", x);

  let mut x = 5;
  println!("The value of x is: {}", x);
  
  x = 6;
  println!("The value of x is: {}", x);
}
```