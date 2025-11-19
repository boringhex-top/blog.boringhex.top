---
title: ESP32 Rust开发环境搭建指南
date: 2024-11-13 11:24:53
tags:
    - esp32
    - rust
    - 嵌入式
---

## 简介

ESP32是一款功能强大的微控制器，而Rust作为一种安全、并发的系统编程语言，越来越受到嵌入式开发者的青睐。本文将详细介绍如何搭建ESP32的Rust开发环境，让你能够使用Rust语言开发ESP32项目。

<!-- more -->

## 环境搭建步骤

### 1. 安装Rust

首先，我们需要安装Rust编程语言。访问[Rust官网](https://www.rust-lang.org/)，按照指引安装Rustup工具链管理器。

[![](https://mermaid.ink/img/pako:eNqrVkrOT0lVslJKL0osyFAIcYnJUwACx-inexqeLu-OVdDVtVNwin66rvPF4tag0uKS0oJYiBInsJQzkhRUwhks4RL9clXPi_WNEGmolAtYyjX6-e7Jz-bOBwoq6SjlphblJmamAJ1QDVIUo1SSkZqbGqNkBWSmpKYlluaUxCjF5NUClSaWluQHV-YlK1mVFJWm6iiVFqQklqS6ZCYCHZ-rZJWWmFMMFE1NySzJL_KFeAvsOx2lgsS8qPx8hJqi_NL0DCivFgCRil9F?type=png)](https://mermaid.live/edit#pako:eNqrVkrOT0lVslJKL0osyFAIcYnJUwACx-inexqeLu-OVdDVtVNwin66rvPF4tag0uKS0oJYiBInsJQzkhRUwhks4RL9clXPi_WNEGmolAtYyjX6-e7Jz-bOBwoq6SjlphblJmamAJ1QDVIUo1SSkZqbGqNkBWSmpKYlluaUxCjF5NUClSaWluQHV-YlK1mVFJWm6iiVFqQklqS6ZCYCHZ-rZJWWmFMMFE1NySzJL_KFeAvsOx2lgsS8qPx8hJqi_NL0DCivFgCRil9F)

安装完成后，打开终端并运行以下命令来验证安装：

```bash
rustc --version
cargo --version
```

### 2. 安装ESP-RS工具链

ESP-RS是专门为ESP32开发的Rust工具链。使用以下命令安装：

```bash
cargo install espup
espup install
```

### 3. 配置环境变量

espup 会创建一个 export 文件，其中包含构建项目所需的一些环境变量。

在 Windows 系统上（%USERPROFILE%\export-esp.ps1）

- 对于 Windows 用户，不需要 执行这个文件。这个文件只是用来展示哪些环境变量被修改了的。

在基于 Unix 的系统上（$HOME/export-esp.sh），有几种不同的方法来 source 这个文件：

- 在每个终端里 source 这个文件：

  1. source 这个 export 文件： `. $HOME/export-esp.sh`

  这种方法需要在每个新的 shell 里运行这个命令。

- 创建用于执行 export-esp.sh 的别名（alias）：

  1. 将以下命令复制粘贴到 shell 的配置文件中（.profile、.bashrc、.zprofile 等）：`alias get_esprs='. $HOME/export-esp.sh'`
  2. 通过重启终端，或执行 `source [配置文件的路径]`，例如 `source ~/.bashrc`，来刷新配置。

  这种方法需要在每个新的 shell 里运行别名（alias）。

- 直接将环境变量添加到 shell 配置文件中：

  1. 把 `$HOME/export-esp.sh` 的内容添加到 shell 的配置文件中：`cat $HOME/export-esp.sh >> [配置文件的路径]`，例如 `cat $HOME/export-esp.sh >> ~/.bashrc`。
  2. 通过重启终端，或执行 `source [配置文件的路径]`，例如 `source ~/.bashrc`，来刷新配置。

  这种方法 不需要 任何 source。export-esp.sh 脚本会在每个 shell 里自动 source。

### 4. 安装额外工具

我们还需要安装一些额外的工具：

```bash
cargo install cargo-espflash
cargo install cargo-espmonitor
```

### 5. 创建新项目

现在我们可以创建一个新的ESP32 Rust项目了：

```bash
cargo new --bin my_esp32_project
cd my_esp32_project
```

### 6. 配置项目

编辑`Cargo.toml`文件，添加必要的依赖：

```toml
[package]
name = "my_esp32_project"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2021"
license = "MIT OR Apache-2.0"

[dependencies]
hal = { package = "esp32-hal", version = "0.14.0" }
esp-backtrace = { version = "0.7.0", features = ["esp32", "panic-handler", "print-uart"] }

[build-dependencies]
embuild = "0.31.2"
```

### 7. 编写示例代码

在`src/main.rs`中编写一个简单的LED闪烁程序：

```rust
#![no_std]
#![no_main]

use esp32_hal::{clock::ClockControl, gpio::IO, peripherals::Peripherals, prelude::*, timer::TimerGroup, Delay, Rtc};
use esp_backtrace as _;

#[entry]
fn main() -> ! {
    let peripherals = Peripherals::take().unwrap();
    let system = peripherals.DPORT.split();
    let clocks = ClockControl::boot_defaults(system.clock_control).freeze();

    let timer_group0 = TimerGroup::new(peripherals.TIMG0, &clocks);
    let mut timer0 = timer_group0.timer0;
    let mut rtc = Rtc::new(peripherals.RTC_CNTL);

    let io = IO::new(peripherals.GPIO, peripherals.IO_MUX);
    let mut led = io.pins.gpio2.into_push_pull_output();

    loop {
        led.set_high().unwrap();
        timer0.delay_ms(500u32);
        led.set_low().unwrap();
        timer0.delay_ms(500u32);
    }
}
```

### 8. 编译和烧录

使用以下命令编译和烧录程序到ESP32：

```bash
cargo build
cargo espflash flash --monitor
```

## 开发流程图

[![](https://mermaid.ink/img/pako:eNpVkU9LwzAchr9KyXm96K0Hwa3dH0GQ1ZNtD2HNtsLaji49yNhBxnTCdP5hYzIEtyne2t6GzumXMbb9FmZZQMwpeZ-HH_zetEHFNRGQQM2DzbpwLOuOQM--RoLLZNEr-y1sCKK4J2R5oqhHYlklyxfSW6b3n8bWzzInp6W9q3gdxNchmXfJcJJeDLmQY4LMh6TzR_I83g7hgswERSP9KVm9_4yjdPYWTwNOFUbzfP4_lGeooMUfY3L-8L1axE9nHBUYKm5QEt7ysMjCkhZ3X8l6RPoR3Wh3h8MSgwda8nWTzAbkbpBE3SQcGSADbOTZ0DJpVe2NqwNcRzbSgUSvJqpCv4F1oDsdqkIfu-qpUwES9nyUAX7ThBjJFqQl20CqwkaLpsi0sOsdbutnv5ABTeicuO6f47l-rc5fnV8BsrCl?type=png)](https://mermaid.live/edit#pako:eNpVkU9LwzAchr9KyXm96K0Hwa3dH0GQ1ZNtD2HNtsLaji49yNhBxnTCdP5hYzIEtyne2t6GzumXMbb9FmZZQMwpeZ-HH_zetEHFNRGQQM2DzbpwLOuOQM--RoLLZNEr-y1sCKK4J2R5oqhHYlklyxfSW6b3n8bWzzInp6W9q3gdxNchmXfJcJJeDLmQY4LMh6TzR_I83g7hgswERSP9KVm9_4yjdPYWTwNOFUbzfP4_lGeooMUfY3L-8L1axE9nHBUYKm5QEt7ysMjCkhZ3X8l6RPoR3Wh3h8MSgwda8nWTzAbkbpBE3SQcGSADbOTZ0DJpVe2NqwNcRzbSgUSvJqpCv4F1oDsdqkIfu-qpUwES9nyUAX7ThBjJFqQl20CqwkaLpsi0sOsdbutnv5ABTeicuO6f47l-rc5fnV8BsrCl)

## 结论

通过以上步骤，我们成功搭建了ESP32的Rust开发环境。这为我们使用Rust语言开发ESP32项目提供了坚实的基础。随着ESP-RS生态系统的不断发展，相信未来会有更多强大的工具和库来支持ESP32的Rust开发。

## 参考资源

1. [ESP-RS官方文档](https://esp-rs.github.io/book/)
2. [Rust编程语言官网](https://www.rust-lang.org/)
3. [ESP32官方文档](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/)

希望这篇文章能帮助你顺利搭建ESP32的Rust开发环境。如果在过程中遇到任何问题，欢迎查阅官方文档或在社区中寻求帮助。祝你的ESP32 Rust开发之旅愉快！