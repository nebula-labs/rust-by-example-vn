# Formatted print (Định dạng in dữ liệu) trong Rust

Printing (In văn bản) được xử lý bởi một loạt [`macros`][macros] được xác định trong [`std::fmt`][fmt],
một số trong số macro đó bao gồm:

* `format!`: ghi văn bản được định dạng vào một [`String`][string].
* `print!`: giống như `format!` nhưng văn bản được in ra console (io::stdout).
* `println!`: giống như `print!` nhưng sẽ xuống dòng sau khi in kí tự cuối.
* `eprint!`: giống như `print!` nhưng văn bản được in với định dạng standard error (io::stderr).
* `eprintln!`: giống như `eprint!` nhưng sẽ xuống dòng sau khi in kí tự cuối.

Rust còn kiểm tra tính đúng đắn của định dạng tại compile time (thời điểm biên dịch).

```rust,editable,ignore,mdbook-runnable
fn main() {
    // Dấu `{}` sẽ tự động được thay thế bằng bất kỳ
    // đối số nào. Chúng sẽ được xâu chuỗi lại.
    println!("{} days", 31);

    // Có thể chỉ định vị trí cho các đối số bằng một số nguyên bên trong `{}`,
    // nó sẽ xác định đối số nào sẽ được dùng để thay thế. Các đối số được liệt kê
    // phía sau xâu được định dạng, và được đánh vị trí bắt đầu từ 0.
    println!("{0}, this is {1}. {1}, this is {0}", "Alice", "Bob");

    // Và đối số cũng có thể được đặt tên như bên dưới đây
    println!("{subject} {verb} {object}",
             object="the lazy dog",
             subject="the quick brown fox",
             verb="jumps over");

    // Các định dạng khác nhau được xác định bằng ký tự định dạng được chỉ định sau một dấu `:`.
    println!("Base 10 repr:               {}",   69420);
    println!("Base 2 (binary) repr:       {:b}", 69420);
    println!("Base 8 (octal) repr:        {:o}", 69420);
    println!("Base 16 (hexadecimal) repr: {:x}", 69420);
    println!("Base 16 (hexadecimal) repr: {:X}", 69420);

    // Bạn có thể căn phải văn bản với chiều rộng được xác định. Câu lệnh dưới sẽ in ra
    // "     1". Với 5 khoảng trắng và một kí tự "1" ở cuối.
    println!("{number:>5}", number=1);

    // Bạn có thể thay các khoảng trắng trên bằng các số 0. Câu lệnh dưới sẽ xuất ra "000001".
    println!("{number:0>5}", number=1);

    // Bạn có thể sử dụng các đối số được đặt tên trong trình định dạng bằng cách thêm `$'
    println!("{number:0>width$}", number=1, width=5);

    // Rust thậm chí còn kiểm tra xem số lượng đối số được sử dụng trong xâu định dạng có chính xác hay không.
    println!("My name is {0}, {1} {0}", "Bond");
    // Câu lệnh trên sẽ báo lỗi bởi bạn chưa điền đối số có vị trí 1.  
    // FIXME ^ Để sửa nó, hãy thử thêm "James" vào sau "Bond".

    // Chỉ có các kiểu mà triển khai fmt::Display mới có thể được định dạng bằng `{}`. 
    // User-defined types (Các kiểu do người dùng định nghĩa) sẽ không triển khai fmt::Display theo mặc định.

    #[allow(dead_code)]
    struct Structure(i32);

    // Câu lệnh này sẽ không được biên dịch vì `Structure` không triển khai fmt::Display
    println!("This struct `{}` won't print...", Structure(3));
    // FIXME ^ Để hàm chạy đúng, hãy comment câu lệnh trên lại.

    // Đối với Rust 1.58 trở lên, bạn có thể lấy đối số từ các biến xung quanh. 
    // Cũng giống như ở trên, câu lệnh dưới đây sẽ in ra 
    // "     1". Với 5 khoảng trắng và một kí tự "1" ở cuối.
    let number: f64 = 1.0;
    let width: usize = 6;
    println!("{number:>width$}");
}
```

[`std::fmt`][fmt] có nhiều [`traits`][traits] để quản lý việc hiển thị của văn bản.
Dưới đây là hai dạng cơ bản:

* `fmt::Debug`: Sử dụng dấu `{:?}`. Định dạng văn bản cho mục đích gỡ lỗi.
* `fmt::Display`: Sử dụng dấu `{}`. Định dạng văn bản được hiển thị ra có hình thức thân thiện với người dùng.

Ở đây, chúng ta đã sử dụng `fmt::Display` vì thư viện std cung cấp các triển khai (implementations)
cho những kiểu này. Để in văn bản cho các kiểu dữ liệu tùy chỉnh, cần nhiều bước hơn.

Việc triển khai `fmt::Display` trait sẽ tự động triển khai
[`ToString`] trait cho phép chúng ta [chuyển đổi] kiểu thành [`String`][string].

### Thực hành

 * Sửa 2 sự cố trong đoạn code bên trên (mục FIXME) để nó chạy được mà không bị lỗi.
 * Thêm lời gọi `println!` macro để in ra: `Pi is roughly 3.142` bằng cách kiểm soát 
   số chữ số thập phân được hiển thị. Cho mục đích của bài tập này, hãy sử dụng 
   `let pi = 3.141592` là một ước lượng cho số pi. (Gợi ý: bạn có thể cần 
   kiểm tra tài liệu [`std::fmt`][fmt] để thiết lập số lượng các chữ số thập phân 
   được hiển thị)

### Xem thêm tại:

[`std::fmt`][fmt], [`macros`][macros], [`struct`][structs],
và [`traits`][traits]

[fmt]: https://doc.rust-lang.org/std/fmt/
[macros]: ../macros.md
[string]: ../std/str.md
[structs]: ../custom_types/structs.md
[traits]: https://doc.rust-lang.org/std/fmt/#formatting-traits
[`ToString`]: https://doc.rust-lang.org/std/string/trait.ToString.html
[chuyển đổi]: ../conversion/string.md
