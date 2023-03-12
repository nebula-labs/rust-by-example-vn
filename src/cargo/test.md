# Tests
Như chúng ta đã biết kiểm thử  không thể thiếu đối với bất kì giai đoạn nào trong quá trình phát triển phần mềm. Rust ưu tiên cho việc unit test( kiểm thử đơn vị) và integration test( kiểm thử tích hợp) ( [xem chương này](https://doc.rust-lang.org/book/ch11-00-testing.html) tại TRPL).

Từ các chương kiểm thử trong liên kết phía trên, chúng ta sẽ biết cách để viết unit test và integration test.
Về mặt tổ chức, chúng tôi đặt các unit test bên trong các mô-đun, chúng sẽ test và integration test trong chính thư mục `tests/`:
```rust
foo
├── Cargo.toml
├── src
│   └── main.rs
│   └── lib.rs
└── tests
    ├── my_test.rs
    └── my_other_test.rs

```
Mỗi tệp tin trong `tests` là một [integration test](https://doc.rust-lang.org/book/ch11-03-test-organization.html#integration-tests) riêng biệt, tức là kiểm thử đó nhằm kiểm tra thư viện của bạn như thể nó được gọi từ một dependency crate. 

Chương [Testing](https://doc.rust-lang.org/rust-by-example/testing.html) này xây dựng trên 3 kiểu kiểm thử: [Unit](https://doc.rust-lang.org/rust-by-example/testing/unit_testing.html), [Doc](https://doc.rust-lang.org/rust-by-example/testing/doc_testing.html), và [Integration](https://doc.rust-lang.org/rust-by-example/testing/integration_testing.html).

`cargo`đơn thuần giúp bạn chạy toàn bộ các kiểm thử đó một cách rất dễ dàng. 
```shell
$ cargo test
```
Bạn sẽ thấy kết quả đầu ra như sau: 
```rust
$ cargo test
   Compiling blah v0.1.0 (file:///nobackup/blah)
    Finished dev [unoptimized + debuginfo] target(s) in 0.89 secs
     Running target/debug/deps/blah-d3b32b97275ec472

running 3 tests
test test_bar ... ok
test test_baz ... ok
test test_foo_bar ... ok
test test_foo ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

```
Bạn cũng có thể chạy kiểm thử với tên test cụ thể(`test_foo`): 

```rust
$ cargo test test_foo
```
```rust
$ cargo test test_foo
   Compiling blah v0.1.0 (file:///nobackup/blah)
    Finished dev [unoptimized + debuginfo] target(s) in 0.35 secs
     Running target/debug/deps/blah-d3b32b97275ec472

running 2 tests
test test_foo ... ok
test test_foo_bar ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out
```
Lưu ý: Cargo có thể chạy nhiều kiểm thử cùng lúc, vì thế hãy chắc chắn rằng không xảy ra tranh chấp tài nguyên. 

Một ví dụ về kiểm thử đồng thời này gây ra sự cố nếu hai kiểm thử cố gắng ghi vào cùng một tệp tin, chẳng hạn như dưới đây:
```rust
#![allow(unused)]
fn main() {
#[cfg(test)]
mod tests {
    // Import vào chương trình các mô-đun cần thiết.
    use std::fs::OpenOptions;
    use std::io::Write;

    // Kiểm thử này được ghi ra một tệp. 
    #[test]
    fn test_file() {
        // Mở tệp ferris.txt hoặc tạo ra nó nếu nó không tồn tại.
        let mut file = OpenOptions::new()
            .append(true)
            .create(true)
            .open("ferris.txt")
            .expect("Failed to open ferris.txt");

        // Ghi vào tệp tin từ "Ferris" 5 lần.
        for _ in 0..5 {
            file.write_all("Ferris\n".as_bytes())
                .expect("Could not write to ferris.txt");
        }
    }

    // Kiểm thử này cố ghi vào cùng tệp tin( ferris.txt). 
    #[test]
    fn test_file_also() {
        // Mở tệp ferris.txt hoặc tạo ra nó nếu nó không tồn tại.
        let mut file = OpenOptions::new()
            .append(true)
            .create(true)
            .open("ferris.txt")
            .expect("Failed to open ferris.txt");

        // ghi vào tệp ti từ "Corro" 5 lần.
        for _ in 0..5 {
            file.write_all("Corro\n".as_bytes())
                .expect("Could not write to ferris.txt");
        }
    }
}
}

```
Mặc dù mục đích là để có được kết quả như sau:
```rust
$ cat ferris.txt
Ferris
Ferris
Ferris
Ferris
Ferris
Corro
Corro
Corro
Corro
Corro
```
Nhưng điều thực sự được ghi vào ferris.txt là: 
```rust
$ cargo test test_foo
Corro
Ferris
Corro
Ferris
Corro
Ferris
Corro
Ferris
Corro
Ferris
```