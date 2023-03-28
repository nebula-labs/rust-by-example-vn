# Mutability

Kiểu dữ liệu có thể thay đổi có thể được mượn theo kiểu có thể thay đổi bằng cách sử dụng `&mut T`. Điều này được gọi là *mutable reference*(tham chiếu có thể thay đổi) và nó cung cấp quyền truy cập đọc/ghi cho borrower. Ngược lại, `&T` mượn dữ liệu thông qua *immutable reference*(tham chiếu không thể thay đổi) và borrower chỉ có thể đọc dữ liệu nhưng không thể sửa đổi nó:

```rust,editable,ignore,mdbook-runnable
#[allow(dead_code)]
#[derive(Clone, Copy)]
struct Book {
    // &'static str là một tham chiếu đến một chuỗi được cấp phát trong bộ nhớ chỉ đọc.
    author: &'static str,
    title: &'static str,
    year: u32,
}

// Hàm này lấy một tham chiếu tới một book
fn borrow_book(book: &Book) {
    println!("I immutably borrowed {} - {} edition", book.title, book.year);
}

// Hàm này lấy tham chiếu tới một book có thể thay đổi và thay đổi `year` thành 2014
fn new_edition(book: &mut Book) {
    book.year = 2014;
    println!("I mutably borrowed {} - {} edition", book.title, book.year);
}

fn main() {
    // Tạo một đối tượng Book không thể thay đổi với tên `immutabook`
    let immutabook = Book {
        // string literals have type `&'static str`
        author: "Douglas Hofstadter",
        title: "Gödel, Escher, Bach",
        year: 1979,
    };

    // Tạo một bản sao có thể thay đổi của `immutabook` và gọi nó là `mutabook`. 
    let mut mutabook = immutabook;
    
    // Mượn một đối tượng không thể thay đổi sử dụng tham chiếu không thể thay đổi. 
    borrow_book(&immutabook);

    // Mượn một đối tượng có thể thay đổi sử dụng tham thiếu không thể thay đổi.
    borrow_book(&mutabook);
    
    // Mượn một đối tượng có thể thay đổi sử dụng tham chiếu có thể thay đổi
    new_edition(&mut mutabook);
    
    // Lỗi! Không thể mượn một đối tượng không thể thay đổi bằng cách sử dụng một tham chiếu có thể thay đổi
    new_edition(&mut immutabook);
    // FIXME ^ Comment dòng phía trên
}
```

### See also:
[`static`][static]

[static]: ../lifetime/static_lifetime.md