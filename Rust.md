# 安装

换字节源：http://rsproxy.cn/

- rustup：rust 版本管理工具

```
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

上述命令将安装 `rustup`，且自动安装最新稳定版本的 Rust。

- linker：链接器，用于将编译输出链接到文件，Linux 的 GCC 或 CLang 中包含链接器，Mac 请安装 xcode

````
xcode-select --install`
````

验证是否安装成功

```
rustc --version
```

更新和卸载

```
rustup update
rustup self uninstall
```

- rustfmt：自动格式化工具，随 rustup 一同安装。

# HelloWorld

创建源文件 main.rs

```rust
fn main() {
    println!("Hello World!"); // !表示调用宏，不带!表示调用函数
}
```

编译执行

```
rustc main.rs
./main
```

# 项目和包管理：Cargo

通过 Cargo 创建项目

```
cargo --version
cargo new rust_tutorial
```

 Cargo 构建并运行，会在 `target/debug` 中创建文件

```sh
# 构建并运行
cargo build
./target/debug/rust_tutorial
```

上述两个步骤可以合并运行

```
cargo run
```

> 若源文件没有发生改变，cargo 不会编译，而是直接运行。

检查源代码，而不编译成可执行文件

```
cargo check
```

发布项目，优化编译，耗时长，但生成的可执行文件运行快，在 `targte/release` 中创建文件

```
cargo build --release
```

**Crate**

Crate 是 Rust 源代码文件的集合，可以理解为外部库，Cargo 可以导入外部 Crate。

例如，导入 Crate `rand`，修改 `Cargo.toml`

```
[dependencies]
rand = "0.8.5"
```

说明符`0.8.5`实际上是 的简写`^0.8.5`，表示任何至少为 0.8.5 但低于 0.9.0 的版本。

后续使用 `cargo build` 时，Cargo 会自动检查 `[dependencies]` 未被下载的依赖，并从远程仓库([Cretes.io](https://crates.io/))下载。

`Cargo.lock` 文件用于确保可重现构建，任意时候不要手动修改。

**更新依赖**

```
cargo update
```

当你确实想要更新一个 crate 时，Cargo 提供命令`update`，并将更改写入 `Cargo.lock`  和 `Cargo.toml` 中。

本例中，默认情况下，Cargo 只会查找大于 0.8.5 且小于 0.9.0 的版本。

如果想要更新到 0.9 以上的版本，请手动修改 `Cargo.toml` 文件

```
[dependencies]
rand = "0.9.0"
```

# 特殊之处

## 函数返回值

函数可以向调用它的代码返回值。

- 我们并不对返回值命名，但要在箭头（`->`）后声明它的类型
- 在 Rust 中，**函数的返回值等同于函数体最后一个表达式的值**
- 使用 `return` 关键字和指定值，可以从函数中提前返回；但大部分函数隐式返回最后一个表达式。

```
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {}", x);
}
```

## 无限循环: loop

除 if、while、for之外，rust还提供了无限循环 loop。

> 其他语言中，采用 while(ture) 实现无限循环

```rust
fn main() {
    loop {
        println!("again!");
    }
}
```

此外，loop 可以拥有返回值：

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {result}");
}
```

# 更多

参见：[Rust 程序设计语言 ](https://rustwiki.org/zh-CN/book/)
