---
title: Embassy：革新嵌入式Rust开发的异步框架
date: 2024-11-18 11:24:53
tags:
    - rust
    - 嵌入式
---

## 引言

在嵌入式系统开发领域,Rust语言因其内存安全性和高性能而备受青睐。然而,传统的嵌入式Rust开发往往依赖于裸机编程或RTOS,缺乏现代异步编程的便利性。Embassy框架的出现,为嵌入式Rust开发带来了一场革新,让开发者能够以异步、高效的方式编写嵌入式应用程序。本文将深入介绍Embassy框架的特性、工作原理及其在实际开发中的应用。

<!-- more -->

## Embassy简介

Embassy是一个为嵌入式系统设计的异步Rust执行时和HAL(硬件抽象层)框架。它的主要目标是:

- 提供零成本的异步抽象
- 支持多种微控制器平台
- 实现高效的资源利用
- 简化嵌入式应用程序的开发

Embassy的核心思想是将嵌入式开发中的各种操作(如I/O、定时器等)抽象为异步任务,并通过高效的调度器来管理这些任务。

## Embassy的主要特性

### 1. 零成本异步抽象

Embassy利用Rust的async/await语法,提供了零开销的异步编程模型。这意味着开发者可以使用熟悉的异步编程范式,而不会引入额外的运行时开销。

### 2. 高效的任务调度器

Embassy内置了一个轻量级的协作式多任务调度器,可以高效地管理和切换异步任务。这个调度器被设计为尽可能减少内存使用和上下文切换开销。

### 3. 丰富的HAL支持

Embassy为多种常见的微控制器外设(如GPIO、UART、SPI、I2C等)提供了异步HAL实现。这些HAL抽象了底层硬件细节,使得开发者可以更专注于应用逻辑。

### 4. 中断驱动的事件系统

Embassy实现了一个基于中断的事件系统,允许硬件中断直接唤醒相关的异步任务。这种设计极大地提高了系统的响应性和能效。

### 5. 静态内存分配

Embassy鼓励使用静态内存分配,避免了动态内存分配带来的不确定性。这对于资源受限的嵌入式系统尤其重要。

### 6. 无RTOS依赖

Embassy可以直接运行在裸机上,不需要依赖传统的RTOS。这简化了系统架构,减少了内存占用和系统复杂性。

## Embassy的工作原理

Embassy的核心是其异步执行时和任务调度器。当一个异步任务被创建时,它会被添加到调度器的任务队列中。调度器负责在合适的时机唤醒和执行这些任务。

当一个任务需要等待某个事件(如I/O操作完成)时,它会让出执行权,调度器会切换到其他可执行的任务。一旦等待的事件发生(通常由中断触发),相关的任务会被重新加入到可执行队列中。

这种基于事件的异步模型非常适合嵌入式系统,因为它可以充分利用系统资源,减少空闲等待时间。

## 使用Embassy的示例

下面是一个使用Embassy进行LED闪烁的简单示例:

```rust
#[embassy_executor::main]
async fn main(spawner: Spawner) {
    let p = embassy_stm32::init(Default::default());
    let mut led = Output::new(p.PC13, Level::High, Speed::Low);

    loop {
        led.set_high();
        Timer::after(Duration::from_millis(1000)).await;
        led.set_low();
        Timer::after(Duration::from_millis(1000)).await;
    }
}
```

在这个例子中,我们使用Embassy的异步Timer来实现LED的闪烁。注意`await`关键字的使用,它允许我们以同步的方式编写异步代码。

再来看一个稍微复杂的例子：

```rust
use defmt::info;
use embassy_executor::Spawner;
use embassy_time::{Duration, Timer};
use embassy_nrf::gpio::{AnyPin, Input, Level, Output, OutputDrive, Pin, Pull};
use embassy_nrf::Peripherals;

// Declare async tasks
#[embassy_executor::task]
async fn blink(pin: AnyPin) {
    let mut led = Output::new(pin, Level::Low, OutputDrive::Standard);

    loop {
        // Timekeeping is globally available, no need to mess with hardware timers.
        led.set_high();
        Timer::after_millis(150).await;
        led.set_low();
        Timer::after_millis(150).await;
    }
}

// Main is itself an async task as well.
#[embassy_executor::main]
async fn main(spawner: Spawner) {
    let p = embassy_nrf::init(Default::default());

    // Spawned tasks run in the background, concurrently.
    spawner.spawn(blink(p.P0_13.degrade())).unwrap();

    let mut button = Input::new(p.P0_11, Pull::Up);
    loop {
        // Asynchronously wait for GPIO events, allowing other tasks
        // to run, or the core to sleep.
        button.wait_for_low().await;
        info!("Button pressed!");
        button.wait_for_high().await;
        info!("Button released!");
    }
}
```

这个例子中,我们创建了两个异步任务: 一个用于LED闪烁,另一个用于监听按钮输入。这两个任务可以并发执行,并通过异步等待来实现事件驱动的响应。

## Embassy的优势

- 简化并发编程: 异步模型大大简化了并发操作的处理。
- 提高资源利用率: 基于事件的模型可以更高效地利用系统资源。
- 增强代码可读性: 异步/等待语法使得代码结构更清晰。
- 跨平台兼容性: Embassy支持多种微控制器平台,便于代码复用。
- 安全性: 利用Rust的安全特性,减少常见的嵌入式编程错误。

## 挑战和限制

尽管Embassy带来了诸多优势,但它也面临一些挑战:

- 学习曲线: 对于不熟悉异步编程的开发者来说,可能需要一定的学习时间。
- 工具链支持: 某些嵌入式开发工具可能尚未完全适配Embassy。
- 生态系统成熟度: 相比传统的嵌入式开发方法,Embassy的生态系统还在成长中。

## 结论

Embassy为嵌入式Rust开发带来了现代化的异步编程范式,极大地提升了开发效率和代码质量。它的零成本抽象、高效调度和丰富的HAL支持,使得开发复杂的嵌入式应用变得更加简单和直观。

虽然Embassy还在快速发展中,但它已经展现出了巨大的潜力,有望成为嵌入式Rust开发的重要工具。对于寻求高效、安全和现代化嵌入式开发方案的开发者来说,Embassy无疑是一个值得关注和尝试的框架。

随着Embassy的不断完善和生态系统的成长,我们可以期待看到更多创新和高质量的嵌入式Rust应用程序的出现。