# Alternate/custom key types
Bất kì kiểu nào triển khai các trait `Eq` và `Hash` đều có thể là một key trong `HashMap`. Bao gồm:
- `bool` (Mặc dù không hữu ích lằm vì chỉ có thể có hai key)
- `int`, `uint` và tất cả các biến thể của chúng.
- `String` và `&str` (protip: bạn có thể có HashMap được khóa bằng `String` và gọi `.get()` bằng `&str`)

Lưu ý rằng `f32` và `f64` không triển khai `Hash`, bời vì rất có thể  [Lỗi về độ chính xác trong dấu phẩy động](https://en.wikipedia.org/wiki/Floating-point_arithmetic#Accuracy_problems)
sẽ khiến việc sử dụng chúng làm các key hashmap rất dễ xảy ra lỗi.

Tất cả các lớp collection sẽ triển khai `Eq` và `Hash` nếu kiểu được chứa bên trong cũng được triển khai tương ứng `Eq` và `Hash`. Ví dụ, Vec<T> sẽ triển khai `Hash` nếu T triển khai `Hash`.

Bạn có thể dễ dàng triển khai `Eq` và `Hash` cho một loại tùy chỉnh chỉ với một dòng: `#[derive(PartialEq, Eq, Hash)]`

Trình biên dịch sẽ làm phần còn lại. Nếu bạn muốn kiểm soát nhiều hơn các chi tiết, bạn có thể tự triển khai `Eq` và/hoặc `Hash`. Hướng dẫn này không đề cập đến các chi tiết cụ thể của việc triển khai `Hash`.

Để hiểu hơn về cách `struct` được sử dụng trong `HashMap`, hãy thử tạo một hệ thống đăng nhập đơn giản:

```rust
use std::collections::HashMap;

// Eq yêu cầu bạn derive PartialEq trên type.
#[derive(PartialEq, Eq, Hash)]
struct Account<'a>{
    username: &'a str,
    password: &'a str,
}

struct AccountInfo<'a>{
    name: &'a str,
    email: &'a str,
}

type Accounts<'a> = HashMap<Account<'a>, AccountInfo<'a>>;

fn try_logon<'a>(accounts: &Accounts<'a>,
        username: &'a str, password: &'a str){
    println!("Username: {}", username);
    println!("Password: {}", password);
    println!("Attempting logon...");

    let logon = Account {
        username,
        password,
    };

    match accounts.get(&logon) {
        Some(account_info) => {
            println!("Successful logon!");
            println!("Name: {}", account_info.name);
            println!("Email: {}", account_info.email);
        },
        _ => println!("Login failed!"),
    }
}

fn main(){
    let mut accounts: Accounts = HashMap::new();

    let account = Account {
        username: "j.everyman",
        password: "password123",
    };

    let account_info = AccountInfo {
        name: "John Everyman",
        email: "j.everyman@email.com",
    };

    accounts.insert(account, account_info);

    try_logon(&accounts, "j.everyman", "psasword123");

    try_logon(&accounts, "j.everyman", "password123");
}
```



