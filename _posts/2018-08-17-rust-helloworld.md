---
layout: post
title:  " Rust hello world "
date:   2018-08-17 00:26:07 +0800
categories: rust 100days
typora-copy-images-to: ../assets/images/2018-08
typora-root-url: ../../blog_alexcode
---
Rust hello world


* TOC
{:toc}
## rust install



```bash
curl https://sh.rustup.rs -sSf | sh
```





查看版本：

```bash
rustc --version
rustc 1.25.0 (84203cac6 2018-03-25)
```



## cargo build



cargo 作为rust的编译管理工具



创建项目: 

```bash
cargo new guessing_game
```





build, run

```bash
cargo build
cargo run
```





## Example for Guess number



```rust
use std::io;

fn main() {
    println!("Guess the number!", );

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .ok()
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```



## 多线程

```rust
use std::thread;
use std::time::Duration;
 
fn my_thread() {
   println!("Thread {:?} is running", std::thread::current().id());
   thread::sleep(Duration::from_millis(1));
}
 
fn main() {
   let mut v = vec![];
 
   for _i in 1..10 {
      v.push( thread::spawn(|| { my_thread(); } ) );
   }
 
   println!("main() waiting.");
 
   for child in v {
      match child.join() {
         Ok(_) => (),
         Err(why) => println!("Join failure {:?}", why),
      };
   }
}
```





## list directory

```rust
use std::fs;

fn run() {
    if let Ok(entries) = fs::read_dir("/tmp") {
        for entry in entries {
            println!("{:?}", entry);
            if let Ok(entry) = entry {
                println!("{:?}", entry.path());
                println!("{:?}", entry.file_name());
                println!("{:?}", entry.file_type());
            }
        }
    }
}

fn main() {
    run();
}
```

