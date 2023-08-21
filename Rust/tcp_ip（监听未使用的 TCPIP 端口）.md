### 监听未使用的 TCP/IP 端口

[![std-badge](https://badge-cache.kominick.com/badge/std-1.29.1-blue.svg)](https://doc.rust-lang.org/std) [![cat-net-badge](https://badge-cache.kominick.com/badge/net--x.svg?style=social)](https://crates.io/categories/network-programming)

在本实例中，程序将监听显示在控制台上的端口，直到一个请求被发出。当将端口设置为 0 时，`SocketAddrV4` 会分配一个随机端口。

```rust
use std::io::{Error, Read};
use std::net::{Ipv4Addr, SocketAddrV4, TcpListener};

fn main() -> Result<(), Error> {
    let loopback = Ipv4Addr::new(127, 0, 0, 1);
    let socket = SocketAddrV4::new(loopback, 0);
    let listener = TcpListener::bind(socket)?;
    let port = listener.local_addr()?;
    println!("Listening on {}, access this port to end the program", port);
    let (mut tcp_stream, addr) = listener.accept()?; // 阻塞，直到被请求
    println!("Connection received! {:?} is sending data.", addr);
    let mut input = String::new();
    let _ = tcp_stream.read_to_string(&mut input)?;
    println!("{:?} says {}", addr, input);
    Ok(())
}

```

