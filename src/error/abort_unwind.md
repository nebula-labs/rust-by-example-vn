# `abort` và `unwind`

Phần trước đã mô tả cơ chế xử lí lỗi `panic`. Các thành phần code khác nhau có thể được biên dịch một cách có điều kiện dựa trên các cài đặt `panic`. Các giá trị có thể là `unwind` và `abort`.

Dựa trên ví dụ về nước chanh trước đó, chúng ta sử dụng các panic strategy để thực hiện các dòng code khác nhau.

```rust,editable,mdbook-runnable

fn drink(beverage: &str) {
   // Bạn không nên uống quá nhiều đồ uống có đường.
    if beverage == "lemonade" {
        if cfg!(panic="abort"){ println!("This is not your party. Run!!!!");}
        else{ println!("Spit it out!!!!");}
    }
    else{ println!("Some refreshing {} is all I need.", beverage); }
}

fn main() {
    drink("water");
    drink("lemonade");
}
```

Đây là một ví dụ khác viết lại function `drink()` và sử dụng từ khóa `unwind`.

```rust,editable

#[cfg(panic = "unwind")]
fn ah(){ println!("Spit it out!!!!");}

#[cfg(not(panic="unwind"))]
fn ah(){ println!("This is not your party. Run!!!!");}

fn drink(beverage: &str){
    if beverage == "lemonade"{ ah();}
    else{println!("Some refreshing {} is all I need.", beverage);}
}

fn main() {
    drink("water");
    drink("lemonade");
}
```

Panic strategy có thể được cài đặt từ command line.

```console
rustc  lemonade.rs -C panic=abort
```
