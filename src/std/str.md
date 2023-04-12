# Chuỗi

Có 2 kiểu chuỗi trong rust: `String` và `&str`.

Một `String` sẽ được lưu trữ như là 1 vector dưới định dạng bytes (`Vec<u8>`), luôn theo chuẩn UTF-8. `String` được phân bổ theo vùng nhớ heap, có thể mở rộng và kết thúc không phải là kí tự null.

`&str` là một dạng (`&[u8]`) luôn trỏ đến một chuỗi UTF-8 hợp lệ và có thể được sử dụng để xem như 1 `String`, giống như `&[T]` được coi như ` Vec<T>`.


```rust,editable
fn main() {
    // (tất cả các kiểu chú thích đều là thừa)
    // Tham chiếu đến một string được cấp phát trong bộ nhớ chỉ đọc
    let pangram: &'static str = "the quick brown fox jumps over the lazy dog";
    println!("Pangram: {}", pangram);

    // Lặp lại các từ theo thứ tự ngược lại, không tạo thêm chuỗi mới
    println!("Words in reverse");
    for word in pangram.split_whitespace().rev() {
        println!("> {}", word);
    }

    let mut chars: Vec<char> = pangram.chars().collect();
    chars.sort();
    chars.dedup();

    // Tạo 1 `String` mới có thể mở rộng được
    let mut string = String::new();
    for c in chars {
        // Thêm 1 char vào cuối chuỗi string
        string.push(c);
        // Thêm 1 string vào cuối chuỗi string
        string.push_str(", ");
    }

    // Chuỗi đã cắt là một cắt lát của chuỗi ban đầu, do đó không có cấp phát mới nào được thực hiện
    let chars_to_trim: &[char] = &[' ', ','];
    let trimmed_str: &str = string.trim_matches(chars_to_trim);
    println!("Used characters: {}", trimmed_str);

    // Heap cấp phát một chuỗi
    let alice = String::from("I like dogs");
    // Cấp phát bộ nhớ mới và lưu trữ chuỗi đã sửa đổi ở đó
    let bob: String = alice.replace("dog", "cat");

    println!("Alice says: {}", alice);
    println!("Bob says: {}", bob);
}
```

Có thể tìm thấy các phương thức `str`/`String` khác trong
[std::str][str] và
[std::string][string]
modules

## Ký tự và chuỗi thoát

Có nhiều cách để viết chuỗi ký tự có ký tự đặc biệt trong đó. Tất cả đều dẫn đến một `&str` giống nhau, vì vậy tốt nhất bạn nên sử dụng biểu mẫu thuận tiện nhất để viết. Tương tự, có nhiều cách để viết các ký tự chuỗi byte, tất cả đều có kết quả là `&[u8; N]`.

Nói chung, các ký tự đặc biệt được thoát bằng ký tự gạch chéo ngược: `\`. Bằng cách này, bạn có thể thêm bất kỳ ký tự nào vào chuỗi của mình, kể cả những ký tự không in được và những ký tự mà bạn không biết cách nhập. Nếu bạn muốn có dấu gạch chéo ngược đứng độc lập, hãy thoát(escape) nó bằng một dấu gạch chéo ngược khác: `\\`

Các dấu phân cách bằng chữ của chuỗi hoặc ký tự xuất hiện trong một chữ phải được thoát ra: `"\""`, `'\''`.

```rust,editable
fn main() {
    // Bạn có thể sử dụng các dấu thoát để ghi byte theo giá trị thập lục phân của chúng...
    let byte_escape = "I'm writing \x52\x75\x73\x74!";
    println!("What are you doing\x3F (\\x3F means ?) {}", byte_escape);

    // ...hoặc các điểm mã Unicode.
    let unicode_codepoint = "\u{211D}";
    let character_name = "\"DOUBLE-STRUCK CAPITAL R\"";

    println!("Unicode character {} (U+211D) is called {}",
                unicode_codepoint, character_name );


    let long_string = "String literals
                        can span multiple lines.
                        The linebreak and indentation here ->\
                        <- can be escaped too!";
    println!("{}", long_string);
}
```

Đôi khi có quá nhiều ký tự cần được thoát hoặc việc viết một chuỗi nguyên trạng sẽ thuận tiện hơn nhiều. Đây là nơi các chuỗi nguyên trạng phát huy tác dụng.

```rust, editable
fn main() {
    let raw_str = r"Escapes don't work here: \x3F \u{211D}";
    println!("{}", raw_str);

    // Nếu bạn cần trích dẫn trong một chuỗi dạng nguyên trạng, hãy thêm một cặp #
    let quotes = r#"And then I said: "There is no escape!""#;
    println!("{}", quotes);

    // Nếu bạn cần "# trong chuỗi của mình, chỉ cần sử dụng nhiều # hơn trong dấu phân cách.
    // Bạn có thể sử dụng tối đa 65535 #.
    let longer_delimiter = r###"A string with "# in it. And even "##!"###;
    println!("{}", longer_delimiter);
}
```

Bạn muốn một chuỗi không phải UTF-8? (Hãy nhớ rằng `str` và `String` phải là UTF-8 hợp lệ).
Hoặc có thể bạn muốn một mảng byte chủ yếu là văn bản? Chuỗi byte để giải cứu!

```rust, editable
use std::str;

fn main() {
    // Lưu ý rằng đây không thực sự là `&str`
    let bytestring: &[u8; 21] = b"this is a byte string";

    // Mảng byte không có thuộc tính `Display` nên việc in chúng hơi bị hạn chế
    println!("A byte string: {:?}", bytestring);

    // Chuỗi byte có thể có byte thoát...
    let escaped = b"\x52\x75\x73\x74 as bytes";
    // ...nhưng không thoát unicode
    // let escaped = b"\u{211D} is not allowed";
    println!("Some escaped bytes: {:?}", escaped);


    // Chuỗi byte thô hoạt động giống như chuỗi thô
    let raw_bytestring = br"\u{211D} is not escaped here";
    println!("{:?}", raw_bytestring);

    // Chuyển đổi một mảng byte thành `str` có thể thất bại
    if let Ok(my_str) = str::from_utf8(raw_bytestring) {
        println!("And the same as text: '{}'", my_str);
    }

    let _quotes = br#"You can also use "fancier" formatting, \
                    like with normal raw strings"#;

    // Chuỗi byte không nhất thiết phải là UTF-8
    let shift_jis = b"\x82\xe6\x82\xa8\x82\xb1\x82\xbb"; // "ようこそ" in SHIFT-JIS

    // Nhưng không phải lúc nào chúng cũng có thể được chuyển thành `str`
    match str::from_utf8(shift_jis) {
        Ok(my_str) => println!("Conversion successful: '{}'", my_str),
        Err(e) => println!("Conversion failed: {:?}", e),
    };
}
```

Để chuyển đổi giữa các mã hóa ký tự, hãy xem [encoding][encoding-crate].

Một danh sách chi tiết hơn về cách viết chuỗi ký tự và ký tự thoát
được đưa ra trong chương ['Tokens'][tokens] của Rust Reference.

[str]: https://doc.rust-lang.org/std/str/
[string]: https://doc.rust-lang.org/std/string/
[tokens]: https://doc.rust-lang.org/reference/tokens.html
[encoding-crate]: https://crates.io/crates/encoding
