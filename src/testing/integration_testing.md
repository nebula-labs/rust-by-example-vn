# Kiểm thử tích hợp (Integration testing)

Các bài [Unit tests][unit] thường kiểm thử từng module một tách biệt nhau: chúng thường nhỏ và có thể kiểm thử
các đoạn private code[^†]. Các bài integration tests thực hiện bên ngoài crate của bạn và chỉ sử dụng
các public interface của crate đó như bất kỳ mã nguồn nào khác. Mục đích của chúng là
kiểm tra xem các phần trong thư viện của bạn có hoạt động đúng cách với nhau hay không.

Cargo sẽ tìm những bài integration tests ở thư mục `tests` gần bên cạnh thư mục `src`.

File `src/lib.rs`:

```rust,ignore
// Định nghĩa crate này tên là `adder`
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

File chứa bài test: `tests/integration_test.rs`:

```rust,ignore
#[test]
fn test_add() {
    assert_eq!(adder::add(3, 2), 5);
}
```

Thực thi các bài tests với lệnh `cargo test`:

```shell
$ cargo test
running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

     Running target/debug/deps/integration_test-bcd60824f5fbfe19

running 1 test
test test_add ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests adder

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

Mỗi file Rust bên trong thư mục `tests` sẽ được biên dịch y như hệt một crate riêng biệt. Để mà có thể
chia sẻ các đoạn code giữa các bài integration tests ta có thể tạo ra một module với các hàm public,
import và sử dụng các hàm đó bên trong các bài tests.

File `tests/common/mod.rs`:

```rust,ignore
pub fn setup() {
    // một vài đoạn code của hàm setup, ví dụ như tạo ra các files/thư mục cần thiết,
    // khởi chạy các servers, ...
}
```

File chứa bài test: `tests/integration_test.rs`

```rust,ignore
// import module common.
mod common;

#[test]
fn test_add() {
    // sử dụng code của module common.
    common::setup();
    assert_eq!(adder::add(3, 2), 5);
}
```

Việc tạo module giống như là `tests/common.rs` cũng sẽ hoạt động tương tự, nhưng cách làm này không được khuyến khích
vì công cụ thực thi test sẽ coi như file đó cũng là một crate để test và sẽ cố gắng chạy các bài tests bên trong nó.

[unit]: unit_testing.md
[mod]: ../mod.md

[^†]: Chú thích người dịch: private code là các các đoạn code _không_ được khai báo với từ khóa là pub. Những đoạn code này không được truy cập từ bên ngoài module chứa chúng, mà chỉ có thể được sử dụng bên trong cùng một module đó
