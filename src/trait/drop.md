# Drop

Trait [`Drop`][drop] chỉ có một phương thức: `drop`, phương thức này sẽ được gọi một cách tự động
khi một đối tượng bị ra khỏi scope. Công dụng chính của trait `Drop` là để giải phóng tài nguyên bộ nhớ
mà đối tượng implement nó đang chiếm dụng.

`Box`, `Vec`, `String`, `File`, và `Process` là một vài ví dụ về các kiểu có
implement trait `Drop` để giải phóng tài nguyên. Trait `Drop` cũng có thể được
implement cho bất kì các kiểu dữ liệu tùy chỉnh nào.

Ví dụ sau đây thêm vào hàm `drop` chức năng in ra console để thông báo
mỗi khi nó được gọi.

```rust,editable
struct Droppable {
    name: &'static str,
}

// Implementation này của `drop` thêm chức năng in ra console.
impl Drop for Droppable {
    fn drop(&mut self) {
        println!("> Dropping {}", self.name);
    }
}

fn main() {
    let _a = Droppable { name: "a" };

    // khối A
    {
        let _b = Droppable { name: "b" };

        // khối B
        {
            let _c = Droppable { name: "c" };
            let _d = Droppable { name: "d" };

            println!("Exiting block B");
        }
        println!("Just exited block B");

        println!("Exiting block A");
    }
    println!("Just exited block A");

    // Biến có thể bị drop một cách thủ công sử dụng hàm `drop`
    drop(_a);
    // TODO ^ Thử biến dòng này thành comment

    println!("end of the main function");

    // `_a` *sẽ không* bị `drop` một lần nữa ở đây vì nó đã bị
    // drop (bằng cách thủ công)
}
```

[drop]: https://doc.rust-lang.org/std/ops/trait.Drop.html
