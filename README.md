# tokio-modbus

A [tokio](https://tokio.rs)-based modbus library.

[![Crates.io version](https://img.shields.io/crates/v/tokio-modbus.svg)](https://crates.io/crates/tokio-modbus)
[![Docs](https://docs.rs/tokio-modbus/badge.svg)](https://docs.rs/tokio-modbus/)
[![Build Status](https://travis-ci.org/slowtec/tokio-modbus.svg?branch=master)](https://travis-ci.org/slowtec/tokio-modbus)
[![Coverage Status](https://coveralls.io/repos/github/slowtec/tokio-modbus/badge.svg?branch=master)](https://coveralls.io/github/slowtec/tokio-modbus?branch=master)
[![Percentage of issues still open](http://isitmaintained.com/badge/open/slowtec/tokio-modbus.svg)](http://isitmaintained.com/project/slowtec/tokio-modbus "Percentage of issues still open")
[![Average time to resolve an issue](http://isitmaintained.com/badge/resolution/slowtec/tokio-modbus.svg)](http://isitmaintained.com/project/slowtec/tokio-modbus "Average time to resolve an issue")

## Features

- pure Rust library
- async (non-blocking)
- Modbus TCP
- Modbus RTU

## Installation

Add this to your `Cargo.toml`:

```toml
[dependencies]
tokio-modbus = "*"
```

## TCP client example

```rust
extern crate futures;
extern crate tokio_core;
extern crate tokio_modbus;

use tokio_core::reactor::Core;
use futures::future::Future;
use tokio_modbus::{Client, TcpClient};

pub fn main() {
    let mut core = Core::new().unwrap();
    let handle = core.handle();
    let addr = "192.168.0.222:502".parse().unwrap();

    let task = TcpClient::connect(&addr, &handle).and_then(|client| {
        println!("Fetching the coupler ID");
        client
            .read_input_registers(0x1000, 7)
            .and_then(move |buff| {
                println!("Response is '{:?}'", buff);
                Ok(())
            })
    });

    core.run(task).unwrap();
}
```
More examples can be found in the [examples](https://github.com/slowtec/tokio-modbus/tree/master/examples) folder.

## Protocol-Specification

- [MODBUS Application Protocol Specification v1.1b3 (PDF)](http://modbus.org/docs/Modbus_Application_Protocol_V1_1b3.pdf)
- [MODBUS over serial line specification and implementation guide v1.02 (PDF)](http://modbus.org/docs/Modbus_over_serial_line_V1_02.pdf)
- [MODBUS Messaging on TCP/IP Implementation Guide v1.0b (PDF)](http://modbus.org/docs/Modbus_Messaging_Implementation_Guide_V1_0b.pdf)

## License

MIT/Apache-2.0
