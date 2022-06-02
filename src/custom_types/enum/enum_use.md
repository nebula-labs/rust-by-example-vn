# use

Sử dụng từ khóa `use` để dùng `enum` mà không cần xác định scope một cách thủ công:
```rust,editable
// Đây là một attribute dùng để ẩn cảnh báo về những đoạn code không sử dụng.
#![allow(dead_code)]

enum Status {
    Rich,
    Poor,
}

enum Work {
    Civilian,
    Soldier,
}

fn main() {
    // Sử dụng `use` để dùng các name trong `enum` mà không cần xác định 
    // scope một cách thủ công
    use crate::Status::{Poor, Rich};
    // Tự động `use` tất cả name nằm trong `Work`.
    use crate::Work::*;

    // Tương đương với `Status::Poor`.
    let status = Poor;
    // Tương đương với `Work::Civilian`.
    let work = Civilian;

    match status {
        // Không cần xác định phạm vi bởi đã sử dụng `use`
        Rich => println!("The rich have lots of money!"),
        Poor => println!("The poor have no money..."),
    }

    match work {
        // Không cần xác định phạm vi bởi đã sử dụng `use`
        Civilian => println!("Civilians work!"),
        Soldier  => println!("Soldiers fight!"),
    }
}
```

### Xem thêm tại đây:

[`match`][match] và [`use`][use] 

[use]: ../../mod/use.md
[match]: ../../flow_control/match.md
