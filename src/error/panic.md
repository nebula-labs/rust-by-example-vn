# `panic`

`Panic` chính là cơ chế xử lí lỗi đơn giản nhất mà ta sắp đề cập tới.  
Nó in ra thông điệp lỗi, huỷ bỏ các hoạt động đang thực thi, giải phóng stack và thường sẽ
thoát chương trình.
Bây giờ chúng ta sẽ gọi hàm `panic` dựa trên câu điều kiện lỗi:

```rust,editable,ignore,mdbook-runnable
fn drink(beverage: &str) {
    // You shouldn't drink too much sugary beverages.
    if beverage == "lemonade" { panic!("AAAaaaaa!!!!"); }

    println!("Some refreshing {} is all I need.", beverage);
}

fn main() {
    drink("water");
    drink("lemonade");
}
```
