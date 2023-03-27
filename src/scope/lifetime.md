# Lifetimes
*Lifetimes* (Thời gian tồn tại) là một cấu trúc mà trình biên dịch (hay cụ thể hơn là trình kiểm tra mượn của nó) sử dụng để đảm bảo tất cả các [borrow - mượn](./borrow.md) đều hợp lệ. Cụ thể hơn, thời gian tồn tại của một biến bắt đầu khi nó được tạo và kết thúc khi nó bị hủy. Mặc dù trong nhiều trường hợp, thời gian tồn tại (*lifetimes*) và *scope* thường được đề cập đến cùng nhau, tuy nhiên chúng không giống nhau. <br/>
Ví dụ, trong trường hợp chúng ta mượn một biến thông qua tham chiếu của nó (`&`), thời hạn sử dụng của `borrow` được xác định từ thời điểm nó được khai báo. Do đó, `borrow` sẽ luôn có hiệu lực miễn là giá trị mà nó tham chiếu tới vẫn còn tồn tại. Tuy nhiên, `scope` của chúng sẽ được xác định bởi cách mà tham chiếu được sử dụng. <br/>
Trong ví dụ dưới đây, chúng ta sẽ thấy được sự liên quan giữa `lifetimes` và `scope`, cũng như sự khác nhau của chúng: <br/>
```rust,editable
// Lifetimes được chú thích bên dưới với các dòng biểu thị việc tạo
// và hủy từng biến.
// Giá trị `i` sẽ có thời gian tồn tại lâu nhất vì phạm vi của nó bao trùm toàn bộ
// Đối với cả `borrow1` và `borrow2`, thời lượng của `borrow1` và `borrow2` là không liên quan
// vì chúng không liên kết với nhau.
fn main() {
    let i = 3; // Bắt đầu Lifetime của giá trị i. ─────────┐
    //                                                     │
    { //                                                   │
        let borrow1 = &i; // `borrow1` lifetime bắt đầu. ─┐│
        //                                                ││
        println!("borrow1: {}", borrow1); //              ││
    } // `borrow1` kết thúc. ─────────────────────────────┘│
    //                                                     │
    //                                                     │
    { //                                                   │
        let borrow2 = &i; // `borrow2` lifetime bắt đầu. ─┐│
        //                                                ││
        println!("borrow2: {}", borrow2); //              ││
    } // `borrow2` kết thúc. ─────────────────────────────┘│
    //                                                     │
}   // Kết thúc Lifetime. ─────────────────────────────────┘
```
Cần lưu ý rằng các `name` hoặc `type` sẽ không có `lifetimes`, điều này cũng sẽ đem lại những hạn chế về cách sử dụng `lifetimes`, chúng ta sẽ thấy ở các phần tiếp theo. <br/>