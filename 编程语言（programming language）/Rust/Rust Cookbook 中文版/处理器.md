### 检查逻辑 cpu 内核的数量

[![num_cpus-badge](https://badge-cache.kominick.com/crates/v/num_cpus.svg?label=num_cpus)](https://docs.rs/num_cpus/) [![cat-hardware-support-badge](https://badge-cache.kominick.com/badge/hardware_support--x.svg?style=social)](https://crates.io/categories/hardware-support)

使用 `num_cpus::get` 显示当前机器中的逻辑 CPU 内核的数量。

```rust
fn main() {
    println!("Number of logical cores is {}", num_cpus::get());
}

```

