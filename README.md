<div align="center">

# 🕊️Ekko
__Echo Request Utility__

<p>

[![Rust](https://github.com/dev-bio/Ekko/workflows/Rust/badge.svg)](https://crates.io/crates/ekko)
[![dependency status](https://deps.rs/crate/ekko/0.2.1/status.svg)](https://deps.rs/crate/ekko/0.2.1)
[![Documentation](https://docs.rs/ekko/badge.svg)](https://docs.rs/ekko)
[![License](https://img.shields.io/crates/l/ekko.svg)](https://choosealicense.com/licenses/mit/)

</p>
</div>

---

Ekko is a simple and light utility for sending echo requests, giving you (mostly) everything you need.

## Usage
To use `ekko`, add this to your `Cargo.toml`:

```toml
[dependencies]
ekko = "0.2.1"
```

## Example
The following example will trace the route to the specified destination.
```rust
use ekko::{ error::{EkkoError},
    EkkoResponse,
    Ekko,
};
 
fn main() -> Result<(), EkkoError> {
    let mut ping = Ekko::with_target("rustup.rs")?;
    
    for hops in 1..32 {
        let response = ping.send(hops)?;
 
        match response {
            EkkoResponse::DestinationResponse(data) => {
                println!("DestinationResponse: {:#?}", data);
                break
            }
            
            EkkoResponse::UnreachableResponse((data, reason)) => {
                println!("UnreachableResponse: {:#?} | {:#?}", data, reason);
                continue
            }
            
            EkkoResponse::UnexpectedResponse((data, (t, c))) => {
                println!("UnexpectedResponse: ({}, {}), {:#?}", t, c, data);
                continue
            }
            
            EkkoResponse::ExceededResponse(data) => {
                println!("ExceededResponse: {:#?}", data);
                continue
            }
            
            EkkoResponse::LackingResponse(data) => {
                println!("LackingResponse: {:#?}", data);
                continue
            }
        }
    }
    
    Ok(())
}
```

## Contributing
All contributions are welcome, don't hesitate to open an issue if something is missing!

## License
[MIT](https://choosealicense.com/licenses/mit/)
