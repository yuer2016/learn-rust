# Rust 编程语言

<https://doc.rust-lang.org/book/ch11-01-writing-tests.html>

## 基础表达式

## 变量

Rust 中的变量默认是不可变的。可以通过在声明的变量名称前添加 mut 关键字来使其可变。
使用 const 关键字可以声明一个常量。
在声明的同时，必须显式地标注值的类型。
常量可以被声明在任何作用域中，甚至包括全局作用域。

第一个变量可以被第二个变量隐藏了。
这意味着随后使用这个名称时，它指向的将会是第二个变量。
我们可以重复使用 let 关键字并配以相同的名称来不断地隐藏变量

```rust
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);

    let x = 6；
    println!("The value of x is: {}", x);
    
    let mut y  = 5;
    y = 6;
    println!("The value of y is: {}",y);

    const MAX_POINTS: u32 = 100_000;
}
```

## 数据类型

Rust 中的每一个值都有其特定的数据类型。
有两种不同的数据类型子集：

1. 标量类型 （scalar）
2. 复合类型 （compound）

### 标量

Rust 中内建了4 种基础的标量类型：整数、浮点数、布尔值及字符。

| 标量                  | 类型                           | 字面量                 |
| --------------------- | ------------------------------ | ---------------------- |
| integers              | i8, i16, i32, i64, i128, isize | -10, 0, 1_000, 123_i64 |
| Unsigned integers     | u8, u16, u32, u64, u128, usize | 0, 123, 10_u16         |
| Floating              | f32, f64                       | 3.14, -10.0e20, 2_f32  |
| Unicode scalar values | char                           | 'a', 'α', '∞'          |
| Booleans              | bool                           | true, false            |

```rust
fn main(){
  let i:i16 = -5
  println(i)
}
```

#### 字符串

Rust 有两种类型来表示字符串:

1. String: 一个可修改的、拥有的字符串。
2. &str: 只读字符串。字符串文字具有这种类型。

```rust
fn main() {
    let greeting: &str = "Greetings";
    let planet: &str = "🪐";
    let mut sentence = String::new();
    sentence.push_str(greeting);
    sentence.push_str(", ");
    sentence.push_str(planet);
    println!("final sentence: {}", sentence);
    println!("{:?}", &sentence[0..5]);
}
```

### 复合类型

复合类型 （compound type）可以将多个不同类型的值组合为一个类型。
Rust 提供了两种内置的基础复合类型：元组 （tuple）和数组 （array）

#### 元组类型

元组是一种常见的复合类型，它可以将其他不同类型的多个值组合进一个复合类型中。
元组拥有一个固定的长度：无法在声明结束后增加或减少其中的元素数量。

```rust
// 为了创建元组，需要把一系列的值使用逗号分隔后放置到一对圆括号中。元组每个位置的值都有一个类型，这些类型不需要是相同的
fn main() { 
    let tup: (i32, f64, u8) = (500, 6.4, 1); 

    let tup = (500, 6.4, 1); 
    let (x, y, z) = tup; 
    println!("The value of y is: {}", y);
    
    let x: (i32, f64, u8) = (500, 6.4, 1); 
    let five_hundred = x.0; 
    let six_point_four = x.1; 
    let one = x.2; 
} 
```

#### 数组

```rust
fn main() { 
    let a = [1, 2, 3, 4, 5]; 
    let a: [i32; 5] = [1, 2, 3, 4, 5];

    let index = 1; 
    let element = a[index]; 
    println!("The value of element is: {}", element); 
}
```

## 控制流结构

### if 表达式

```rust
fn main() {
    let x = 10;
    if x < 20 {
        println!("small");
    } else if x < 100 {
        println!("biggish");
    } else {
        println!("huge");
    }
}
```

### while 表达式

```rust
fn main() {
    let mut x = 200;
    while x >= 10 {
        x = x / 2;
    }
    println!("Final x: {x}");
}
```

### for 表达式

```rust
fn main() {
    for x in 1..5 {
        println!("x: {x}");
    }
}
```

### loop 表达式

```rust
fn main() {
    let mut i = 0;
    loop {
        i += 1;
        println!("{i}");
        if i > 100 {
            break;
        }
    }
}
```

## 复合表达式

### 结构体和枚举

### 模式匹配

### 函数和方法

Rust 中的块包含一系列表达式，用大括号 {} 括起来。
每个块都有一个值和类型，块的最后一个表达式的值和类型：

```rust
fn main() {
    let z = 13;
    let x = {
        let y = 10;
        println!("y: {y}");
        z - y
    };
    println!("x: {x}");
}
```

#### 函数

Rust 代码使用蛇形命名法(snake case) 来作为规范函数和变量名称的风格。
蛇形命名法只使用小写的字母进行命名，并以下画线分隔单词

```rust
fn another_function() {
  println!("Another function."); 
}

fn five() -> i32 { 
    5 
} 
 
fn main() { 
    let x = five(); 
    println!("The value of x is: {}", x); 
}
```

#### 方法

Rust 允许您将函数与新类型关联起来。可以使用 impl 块来执行此操作

```rust
#[derive(Debug)]
struct Race {
    name: String,
    laps: Vec<i32>,
}

impl Race {
    // No receiver, a static method
    fn new(name: &str) -> Self {
        Self { name: String::from(name), laps: Vec::new() }
    }

    // Exclusive borrowed read-write access to self
    fn add_lap(&mut self, lap: i32) {
        self.laps.push(lap);
    }

    // Shared and read-only borrowed access to self
    fn print_laps(&self) {
        println!("Recorded {} laps for {}:", self.laps.len(), self.name);
        for (idx, lap) in self.laps.iter().enumerate() {
            println!("Lap {idx}: {lap} sec");
        }
    }

    // Exclusive ownership of self
    fn finish(self) {
        let total: i32 = self.laps.iter().sum();
        println!("Race {} is finished, total lap time: {}", self.name, total);
    }
}

fn main() {
    let mut race = Race::new("Monaco Grand Prix");
    race.add_lap(70);
    race.add_lap(68);
    race.print_laps();
    race.add_lap(71);
    race.print_laps();
    race.finish();
    // race.add_lap(42);
}
```

方法接收器的含义

| 接收器      | 含义                                                                                                                        |
| ----------- | --------------------------------------------------------------------------------------------------------------------------- |
| 无接收器    | 结构上的静态方法。通常用于创建按照惯例称为 new 的构造函数。                                                                 |
| &self       | 使用共享且不可变的引用从调用者借用对象。 该对象之后可以再次使用                                                             |
| &mut self： | 使用唯一且可变的引用从调用者那里借用对象。 该对象之后可以再次使用。                                                         |
| self        | 取得对象的所有权并将其从调用者手中移走。 该方法成为对象的所有者。方法返回时，该对象将被删除（释放），除非显式传输其所有权。 |
| mut self    | 与 self 相同，但是该方法可以改变对象                                                                                        |

#### Traits

```rust
struct Dog {
    name: String,
    age: i8,
}

struct Cat {
    lives: i8,
}

trait Pet {
    fn talk(&self) -> String;

    fn greet(&self) {
        println!("Oh you're a cutie! What's your name? {}", self.talk());
    }
}

impl Pet for Dog {
    fn talk(&self) -> String {
        format!("Woof, my name is {}!", self.name)
    }
}

impl Pet for Cat {
    fn talk(&self) -> String {
        String::from("Miau!")
    }
}

fn main() {
    let captain_floof = Cat { lives: 9 };
    let fido = Dog { name: String::from("Fido"), age: 5 };

    captain_floof.greet();
    fido.greet();
}
```

#### Traits Objects

```rust
struct Dog {
    name: String,
    age: i8,
}

struct Cat {
    lives: i8,
}

trait Pet {
    fn talk(&self) -> String;
}

impl Pet for Dog {
    fn talk(&self) -> String {
        format!("Woof, my name is {}!", self.name)
    }
}

impl Pet for Cat {
    fn talk(&self) -> String {
        String::from("Miau!")
    }
}

fn main() {
    let pets: Vec<Box<dyn Pet>> = vec![
        Box::new(Cat { lives: 9 }),
        Box::new(Dog { name: String::from("Fido"), age: 5 }),
    ];
    for pet in pets {
        println!("Hello, who are you? {}", pet.talk());
    }
}
```

#### 泛型

泛型数据类型

```rust
#[derive(Debug)]
struct Point<T> {
    x: T,
    y: T,
}

// impl<T> Point<T> 意味着该方法是为任何 T
impl<T> Point<T> {
    fn coords(&self) -> (&T, &T) {
        (&self.x, &self.y)
    }
}

fn main() {
    let integer = Point { x: 5, y: 10 };
    let float = Point { x: 1.0, y: 4.0 };
    println!("{integer:?} and {float:?}");
    println!("coords: {:?}", integer.coords());
}
```

泛型函数

```rust
fn pick<T>(n: i32, even: T, odd: T) -> T {
    if n % 2 == 0 {
        even
    } else {
        odd
    }
}

fn main() {
    println!("picked a number: {:?}", pick(97, 222, 333));
    println!("picked a tuple: {:?}", pick(28, ("dog", 1), ("cat", 2)));
}
```

特质也可以是泛型的，就像类型和函数一样。
特质的参数在使用时会得到具体类型。

```rust
#[derive(Debug)]
struct Foo(String);

impl From<u32> for Foo {
    fn from(from: u32) -> Foo {
        Foo(format!("Converted from integer: {from}"))
    }
}

impl From<bool> for Foo {
    fn from(from: bool) -> Foo {
        Foo(format!("Converted from bool: {from}"))
    }
}

fn main() {
    let from_int = Foo::from(123);
    let from_bool = Foo::from(true);
    println!("{from_int:?}, {from_bool:?}");
}
```

使用泛型时，通常会要求类型实现某种特质，以便调用该特质的方法。
您可以使用 T: Trait 或 impl Trait 来做到这一点：

```rust
fn duplicate<T: Clone>(a: T) -> (T, T) {
    (a.clone(), a.clone())
}

// 当需要多个特质时，可以用 where
fn duplicate<T>(a: T) -> (T, T)
where
    T: Clone,
{
    (a.clone(), a.clone())
}

fn main() {
    let foo = String::from("foo");
    let pair = duplicate(foo);
    println!("{pair:?}");
}
```

示例一

```rust
use std::fmt::Display;

pub trait Logger {
    /// Log a message at the given verbosity level.
    fn log(&self, verbosity: u8, message: impl Display);
}

struct StderrLogger;

impl Logger for StderrLogger {
    fn log(&self, verbosity: u8, message: impl Display) {
        eprintln!("verbosity={verbosity}: {message}");
    }
}

fn do_things(logger: &impl Logger) {
    logger.log(5, "FYI");
    logger.log(2, "Uhoh");
}

/// Only log messages up to the given verbosity level.
struct VerbosityFilter<L: Logger> {
    max_verbosity: u8,
    inner: L,
}

impl<L: Logger> Logger for VerbosityFilter<L> {
    fn log(&self, verbosity: u8, message: impl Display) {
        if verbosity <= self.max_verbosity {
            self.inner.log(verbosity, message);
        }
    }
}

fn main() {
    let l = VerbosityFilter { max_verbosity: 3, inner: StderrLogger };
    do_things(&l);
}
```

示例二

```rust
use std::cmp::Ordering;

fn min<T: Ord>(l: T, r: T) -> T {
    match l.cmp(&r) {
        Ordering::Less | Ordering::Equal => l,
        Ordering::Greater => r,
    }
}

fn main() {
    assert_eq!(min(0, 10), 0);
    assert_eq!(min(500, 123), 123);

    assert_eq!(min('a', 'z'), 'a');
    assert_eq!(min('7', '1'), '1');

    assert_eq!(min("hello", "goodbye"), "goodbye");
    assert_eq!(min("bat", "armadillo"), "armadillo");
}
```

#### Deriving

derive 是一个语法宏（macro），用于自动实现一些常见的 trait（特质）或功能。
通过使用 #[derive] 注解，可以为结构体（struct）或枚举类型（enum）自动生成相关代码

```rust
#[derive(Debug, Clone, Default)]
struct Player {
    name: String,
    strength: u8,
    hit_points: u8,
}

fn main() {
    // Default trait adds `default` constructor.
    let p1 = Player::default(); 
    // Clone trait adds `clone` method.
    let mut p2 = p1.clone(); 
    
    p2.name = String::from("EldurScrollz");
    // Debug trait adds support for printing with `{:?}`.
    println!("{:?} vs. {:?}", p1, p2);
}
```

### 标准库

Rust 自带的标准库有助于建立一套 Rust 库和程序使用的通用类型。
Rust 标准库包含几个层次：core、alloc 和 std。

1. core 包括最基本的类型和函数，它们不依赖于 libc、allocator 甚至操作系统的存在。
2. alloc 包括需要全局堆分配器的类型，如 Vec、Box 和 Arc。

嵌入式 Rust 应用程序通常只使用 core，有时也使用 alloc。

#### 标准库类型

##### Option

```rust
fn main() {
    let name = "Löwe 老虎 Léopard Gepardi";
    let mut position: Option<usize> = name.find('é');
    println!("find returned {position:?}");
    assert_eq!(position.unwrap(), 14);
    position = name.find('Z');
    println!("find returned {position:?}");
    assert_eq!(position.expect("Character not found"), 0);
}
```

##### Result

Result 与 Option 类似，但表示操作的成功或失败，各有不同的类型。这与表达式练习中定义的 Res 相似，但属于通用类型：Result<T, E> 其中 T 用于 OK 变体，而 E 出现在 Err 变体中。

```rust
use std::fs::File;
use std::io::Read;

fn main() {
    let file: Result<File, std::io::Error> = File::open("diary.txt");
    match file {
        Ok(mut file) => {
            let mut contents = String::new();
            if let Ok(bytes) = file.read_to_string(&mut contents) {
                println!("Dear diary: {contents} ({bytes} bytes)");
            } else {
                println!("Could not read file content");
            }
        }
        Err(err) => {
            println!("The diary could not be opened: {err}");
        }
    }
}
```

##### String

String 是标准的堆分配可增长 UTF-8 字符串缓冲区

```rust
fn main() {
    let mut s1 = String::new();
    s1.push_str("Hello");
    println!("s1: len = {}, capacity = {}", s1.len(), s1.capacity());

    let mut s2 = String::with_capacity(s1.len() + 1);
    s2.push_str(&s1);
    s2.push('!');
    println!("s2: len = {}, capacity = {}", s2.len(), s2.capacity());

    let s3 = String::from("🇨🇭");
    println!("s3: len = {}, number of chars = {}", s3.len(), s3.chars().count());
}
```

##### Vec

Vec 是标准的可调整大小的堆分配缓冲区：

```rust
fn main() {
    let mut v1 = Vec::new();
    v1.push(42);
    println!("v1: len = {}, capacity = {}", v1.len(), v1.capacity());

    let mut v2 = Vec::with_capacity(v1.len() + 1);
    v2.extend(v1.iter());
    v2.push(9999);
    println!("v2: len = {}, capacity = {}", v2.len(), v2.capacity());

    // Canonical macro to initialize a vector with elements.
    let mut v3 = vec![0, 0, 1, 2, 3, 4];

    // Retain only the even elements.
    v3.retain(|x| x % 2 == 0);
    println!("{v3:?}");

    // Remove consecutive duplicates.
    v3.dedup();
    println!("{v3:?}");
}
```

##### HashMap

```rust
use std::collections::HashMap;

fn main() {
    // 创建一个空的 HashMap
    let mut my_map = HashMap::new();

    // 插入键值对
    my_map.insert("key1", "value1");
    my_map.insert("key2", "value2");

    // 获取值
    match my_map.get("key1") {
        Some(value) => println!("Value for key1: {}", value),
        None => println!("Key not found"),
    }

    // 遍历 HashMap
    for (key, value) in &my_map {
        println!("Key: {}, Value: {}", key, value);
    }

    // 检查键是否存在
    if my_map.contains_key("key2") {
        println!("Key2 exists");
    }

    // 删除键
    my_map.remove("key1");

    // 清空 HashMap
    my_map.clear();
}
```

#### 标准库特质

##### Comparisons

这些特性支持值之间的比较。所有特性都可以为包含实现这些特性的字段的类型派生。

```rust
// PartialEq 是一个部分等价关系，包含必要的方法 eq 和提供的方法 ne。== 和 != 运算符将调用这些方法。
struct Key {
    id: u32,
    metadata: Option<String>,
}

impl PartialEq for Key {
    fn eq(&self, other: &Self) -> bool {
        self.id == other.id
    }
}

use std::cmp::Ordering;

#[derive(Eq, PartialEq)]
struct Citation {
    author: String,
    year: u32,
}

impl PartialOrd for Citation {
    fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
        match self.author.partial_cmp(&other.author) {
            Some(Ordering::Equal) => self.year.partial_cmp(&other.year),
            author_ord => author_ord,
        }
    }
}
```

#### Operators

操作符重载是通过 std::ops 中的 traits 实现的：

```rust
#[derive(Debug, Copy, Clone)]
struct Point {
    x: i32,
    y: i32,
}

impl std::ops::Add for Point {
    type Output = Self;

    fn add(self, other: Self) -> Self {
        Self { x: self.x + other.x, y: self.y + other.y }
    }
}

fn main() {
    let p1 = Point { x: 10, y: 20 };
    let p2 = Point { x: 100, y: 200 };
    println!("{:?} + {:?} = {:?}", p1, p2, p1 + p2);
}
```

##### From and Into

From 和 Into 类型实现了 From 和 Into，以方便类型转换：

```rust
fn main() {
    let s = String::from("hello");
    let addr = std::net::Ipv4Addr::from([127, 0, 0, 1]);
    let one = i16::from(true);
    let bigger = i32::from(123_i16);
    println!("{s}, {addr}, {one}, {bigger}");
}

fn main() {
    let s: String = "hello".into();
    let addr: std::net::Ipv4Addr = [127, 0, 0, 1].into();
    let one: i16 = true.into();
    let bigger: i32 = 123_i16.into();
    println!("{s}, {addr}, {one}, {bigger}");
}
```

#### Casting

Rust 没有隐式类型转换，但支持使用 as 进行显式类型转换。这些转换通常遵循 C 语言的语义。

```rust
fn main() {
    let value: i64 = 1000;
    println!("as u16: {}", value as u16);
    println!("as i16: {}", value as i16);
    println!("as u8: {}", value as u8);
}
```

#### Read and Write

使用 Read 和 BufRead，可以对 u8 源进行抽象

```rust
use std::io::{BufRead, BufReader, Read, Result};

fn count_lines<R: Read>(reader: R) -> usize {
    let buf_reader = BufReader::new(reader);
    buf_reader.lines().count()
}

fn main() -> Result<()> {
    let slice: &[u8] = b"foo\nbar\nbaz\n";
    println!("lines in slice: {}", count_lines(slice));

    let file = std::fs::File::open(std::env::current_exe()?)?;
    println!("lines in file: {}", count_lines(file));
    Ok(())
}

use std::io::{Result, Write};

fn log<W: Write>(writer: &mut W, msg: &str) -> Result<()> {
    writer.write_all(msg.as_bytes())?;
    writer.write_all("\n".as_bytes())
}

fn main() -> Result<()> {
    let mut buffer = Vec::new();
    log(&mut buffer, "Hello")?;
    log(&mut buffer, "World")?;
    println!("Logged: {:?}", buffer);
    Ok(())
}
```

##### Default, struct update syntax

```rust
#[derive(Debug, Default)]
struct Derived {
    x: u32,
    y: String,
    z: Implemented,
}

#[derive(Debug)]
struct Implemented(String);

impl Default for Implemented {
    fn default() -> Self {
        Self("John Smith".into())
    }
}

fn main() {
    let default_struct = Derived::default();
    println!("{default_struct:#?}");

    let almost_default_struct =
        Derived { y: "Y is set!".into(), ..Derived::default() };
    println!("{almost_default_struct:#?}");

    let nothing: Option<Derived> = None;
    println!("{:#?}", nothing.unwrap_or_default());
}
```

##### Closures

```rust
fn apply_with_log(func: impl FnOnce(i32) -> i32, input: i32) -> i32 {
    println!("Calling function on {input}");
    func(input)
}

fn main() {
    let add_3 = |x| x + 3;
    println!("add_3: {}", apply_with_log(add_3, 10));
    println!("add_3: {}", apply_with_log(add_3, 20));

    let mut v = Vec::new();
    let mut accumulate = |x: i32| {
        v.push(x);
        v.iter().sum::<i32>()
    };
    println!("accumulate: {}", apply_with_log(&mut accumulate, 4));
    println!("accumulate: {}", apply_with_log(&mut accumulate, 5));

    let multiply_sum = |x| x * v.into_iter().sum::<i32>();
    println!("multiply_sum: {}", apply_with_log(multiply_sum, 3));
}
```

### 内存管理

谈及内存管理，我们希望编程语言能具备两个特点：

1. 希望内存能在我们选定的时机及时释放，这使我们能控制程序的内存消耗；
2. 在对象被释放后，我们绝不希望继续使用指向它的指针，这是未定义行为，会导致崩溃和安全漏洞。

但上述情景似乎难以兼顾：只要指向值的指针仍然存在，释放这个值就必然会让这些指针悬空。
几乎所有主流编程语言都只能在两个阵营中 "二选一"，这取决于它们从中放弃了哪一项。

Rust 通过限制程序使用指针的方式打破了这种困局,Rust 的目标是既安全又高效。

在 Rust 中，所有权这个概念内置于语言本身，并通过编译期检查强制执行。
每个值都有决定其生命周期的唯一拥有者。
当拥有者被释放时，它拥有的值也会同时被释放，在 Rust 术语中，释放的行为被称为丢弃（drop）。

Rust 从几个方面扩展了这种简单的思想。

1. 可以将值从一个拥有者转移给另一个拥有者。这允许你构建、重新排列和拆除树形结构。
2. 像整数、浮点数和字符这样的非常简单的类型，不受所有权规则的约束。这些称为 Copy 类型。
3. 标准库提供了引用计数指针类型 Rc 和 Arc，它们允许值在某些限制下有多个拥有者。
4. 可以对值进行“借用”（borrow），以获得值的引用。这种引用是非拥有型指针，有着受限的生命周期。

在 Rust 中，对大多数类型来说，像为变量赋值、将其传给函数或从函数返回这样的操作都不会复制值，而是会移动值。
源会把值的所有权转移给目标并变回未初始化状态，改由目标变量来控制值的生命周期。
Rust 程序会以每次只移动一个值的方式建立和拆除复杂的结构。

### 智能指针

### crate 和 modules

当你编写大型程序时，组织代码将变得越来越重要。
通过对相关功能进行分组，并将具有不同功能的代码分开，您可以明确在哪里可以找到实现特定功能的代码，以及在哪里可以更改功能的工作方式。

Rust 有许多功能可以让你管理代码的组织，包括哪些细节是公开的，哪些细节是私有的，以及程序中每个作用域的名称。这些功能有时统称为模块系统，包括:

1. Packages: cargo 功能可让您构建、测试和共享 crate
2. Crates: 生成库或可执行文件的模块树
3. Modules 和 use: 控制路径的组织、范围和私有性
4. Paths: 对结构体、函数或模块等项目的命名方式

Rust 程序由 crate 组成。
每个 crate 都是既完整又内聚的单元，包括单个库或可执行程序的所有源代码，以及任何相关的测试、示例、工具、配置和其他杂项。

```text
backyard
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs
```

```rust
// src/main.rs
use crate::garden::vegetables::Asparagus;

pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {plant:?}!");
}

// src/garden.rs
pub mod vegetables;

// src/garden/vegetables.rs 
#[derive(Debug)]
pub struct Asparagus {}
```

在模块中组合相关代码

通过模块，我们可以在 crate 中组织代码，以提高可读性并方便重复使用。
模块还允许控制项目的私有性，因为模块中的代码默认情况下是私有的。
私有项目是内部实现细节，不对外公开。
可以选择将模块和模块中的项目公开，这样就可以让外部代码使用和依赖它们。

```rust
mod foo {
    pub fn do_something() {
        println!("In the foo module");
    }
}

mod bar {
    pub fn do_something() {
        println!("In the bar module");
    }
}

fn main() {
    foo::do_something();
    bar::do_something();
}

// 模块可见性
mod outer {
    fn private() {
        println!("outer::private");
    }

    pub fn public() {
        println!("outer::public");
        private();
        inner::public();
    }

    mod inner {
        fn private() {
            println!("outer::inner::private");
        }

        pub fn public() {
            println!("outer::inner::public");
            private();
        }
    }
}

fn main() {
    outer::public();
}
```

为了向 Rust 展示如何在模块树中找到一个项目，我们使用了路径，就像我们在文件系统中使用路径一样。
要调用一个函数，我们需要知道它的路径。

路径有两种形式：

1. 绝对路径是从板块根开始的完整路径；对于来自外部板块的代码，绝对路径从板块名称开始，而对于来自当前板块的代码，则从字面板块开始。
2. 相对路径从当前模块开始，使用 self、super 或当前模块中的标识符。
绝对路径和相对路径后面都有一个或多个标识符，用双冒号（::）分隔。

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
    }
}

pub fn eat_at_restaurant() {
    // 绝对路径
    crate::front_of_house::hosting::add_to_waitlist();
    // 相对路径
    front_of_house::hosting::add_to_waitlist();
}
```

与文件系统对应的路径是 front_of_house/hosting/add_to_waitlist。以模块名开头意味着路径是相对的
使用 crate 名称从 crate 根目录开始，就像在 shell 中使用 / 从文件系统根目录开始一样
在绝对路径中，从 crate 开始，它是 crate 模块树的根。

使用 use 关键字将路径纳入范围
可以使用 use 关键字为路径创建一个快捷方式，然后在作用域的其他地方使用这个较短的名称。
在作用域中添加 use 和路径类似于在文件系统中创建符号链接。
通过在 crate 根中添加 use crate::front_of_house::hosting，hosting 现在是该作用域中的一个有效名称，就像在 crate 根中定义了 hosting 模块一样。

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```
