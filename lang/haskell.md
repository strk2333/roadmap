# Haskell

## Install

Recommand haskell stack

https://docs.haskellstack.org/





## Basic Command

- Installation

  `curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`

- cargo

  `cargo build`	compile

  `cargo run`	compile & run

  `cargo check`	check code with no executable file

  `cargo build --release` compile with optimizations

- Mirror

  ```
  # vi C:\Users\Administrator\.cargo\config
  [source.crates-io]
  registry="https://github.com/rust-lang/crates.io-index"
  replace-with ='ustc'
  [source.ustc]
  registry = "git://mirrors.ustc.edu.cn/crates.io-index"
  
  ```

  





## Hello World

```rust
// create file main.rs
// edit file
fn main() {
    println!("hello, world!");
}
// run `rustc main.rs`
```





## Basic Data Types







## High Level Data Types



## Logic Statement



## Function



## Object Oriented Features



## Async



## Network



## UI



## Package Manager







Reference:

- https://rwh.readthedocs.io/en/latest/chp