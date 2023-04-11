# Supertraits

Rust không có "kế thừa", nhưng bạn có thể định nghĩa một trait như là một superset của một trait khác. Ví dụ:

```rust,editable
trait Person {
    fn name(&self) -> String;
}

// Person là một supertrait của Student.
// Thực hiện triển khai Student yêu cầu bạn cũng phải triển khai(impl) Person.
trait Student: Person {
    fn university(&self) -> String;
}

trait Programmer {
    fn fav_language(&self) -> String;
}

// CompSciStudent (Sinh viên khoa học máy tính) là một subtrait của cả Programmer 
// và Student. Thực hiện triển khai CompSciStudent yêu cầu bạn triển khai(impl) cả 2 supertrait trên.
trait CompSciStudent: Programmer + Student {
    fn git_username(&self) -> String;
}

fn comp_sci_student_greeting(student: &dyn CompSciStudent) -> String {
    format!(
        "My name is {} and I attend {}. My favorite language is {}. My Git username is {}",
        student.name(),
        student.university(),
        student.fav_language(),
        student.git_username()
    )
}

fn main() {}
```

### Xem thêm:

[The Rust Programming Language chapter on supertraits][trpl_supertraits]

[trpl_supertraits]: https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#using-supertraits-to-require-one-traits-functionality-within-another-trait