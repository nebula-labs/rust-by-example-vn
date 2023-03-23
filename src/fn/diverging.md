# Diverging functions
Hàm diverging (Diverging functions) không bao giờ trả về giá trị. Chúng được đánh dấu bằng !, một kiểu dữ liệu rỗng.

Ví dụ, hàm này sẽ không bao giờ trả về:

fn foo() -> ! {
    panic!("This call never returns.");
}
Khác với tất cả các kiểu dữ liệu khác, kiểu này không thể được khởi tạo, bởi vì tập hợp tất cả các giá trị có thể của kiểu này là rỗng. Lưu ý rằng, nó khác với kiểu dữ liệu () (cũng được gọi là empty tuple) - một kiểu dữ liệu với đúng một giá trị có thể.

Ví dụ, hàm này trả về như bình thường, mặc dù không có thông tin nào trong giá trị trả về:

fn some_fn() {
    ()
}

fn main() {
    let _a: () = some_fn();
    println!("This function returns and you can see this line.");
}
Ngược lại, hàm này sẽ không bao giờ trả lại điều khiển cho người gọi:

#![feature(never_type)]

fn main() {
    let x: ! = panic!("This call never returns.");
    println!("You will never see this line!");
}
Mặc dù điều này có vẻ như một khái niệm trừu tượng, nhưng nó thực sự rất hữu ích và thường được sử dụng. Lợi ích chính của kiểu dữ liệu này là nó có thể được chuyển đổi sang bất kỳ kiểu dữ liệu nào khác và do đó được sử dụng ở những nơi cần một kiểu chính xác, ví dụ như các mảnh trong câu lệnh match. Điều này cho phép chúng ta viết mã như thế này:

fn main() {
    fn sum_odd_numbers(up_to: u32) -> u32 {
    let mut acc = 0;
    for i in 0..up_to {
    // Chú ý rằng kiểu trả về của biểu thức "match" phải là u32,
    // vì kiểu của biến "addition" là u32.
        let addition: u32 = match i%2 == 1 {
        // Biến "i" có kiểu u32, điều này hoàn toàn đúng.
        true => i,
        // Ngược lại, biểu thức "continue" không trả về u32, nhưng vẫn hợp lệ,
        // vì nó không trả về và do đó không vi phạm yêu cầu kiểu dữ liệu của biểu thức "match".
        false => continue,
        };
     acc += addition;
    }
    acc
   }
        println!("Sum of odd numbers up to 9 (excluding): {}", sum_odd_numbers(9));
}
It is also the return type of functions that loop forever (e.g. loop {}) like network servers or functions that terminate the process (e.g. exit()).
