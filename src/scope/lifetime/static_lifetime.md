# Static
Trong Rust có một vài tên lifetime được dành riêng. Một trong số đó là 'static. Bạn có thể gặp nó trong hai trường hợp:

// Một tham chiếu với lifetime 'static:
let s: &'static str = "hello world";

// 'static được sử dụng làm phần của trait bound:
fn generic<T>(x: T) where T: 'static {}
Cả hai trường hợp đều liên quan nhưng khác nhau nhẹ và đây là nguồn gốc thường gặp khi học Rust. Dưới đây là một số ví dụ cho mỗi trường hợp:

Lifetime của tham chiếu
Dưới dạng lifetime của tham chiếu, 'static chỉ ra rằng dữ liệu được trỏ tới bởi tham chiếu tồn tại trong toàn bộ vòng đời của chương trình đang chạy. Nó vẫn có thể được ép buộc thành một lifetime ngắn hơn.

Có hai cách để tạo một biến với lifetime 'static, và cả hai đều được lưu trữ trong bộ nhớ chỉ đọc của chương trình nhị phân:

Tạo một hằng số với khai báo static.
Tạo một chuỗi chứa ký tự (string) có kiểu dữ liệu là: &'static str.
Hãy xem ví dụ sau để hiển thị mỗi phương thức:

// Tạo một hằng số với lifetime 'static.
static NUM: i32 = 18;

// Trả về một tham chiếu tới NUM trong đó lifetime 'static của nó được ép buộc thành đối số đầu vào.
fn coerce_static<'a>(_: &'a i32) -> &'a i32 {
&NUM
}

# Trait bound
 
 Là một ràng buộc trait, điều đó có nghĩa là kiểu dữ liệu không chứa bất kỳ tham chiếu phi tĩnh nào. Ví dụ, người nhận có thể giữ kiểu dữ liệu trong bất kỳ khoảng thời gian nào mà họ muốn và nó sẽ không bao giờ trở nên không hợp lệ cho đến khi họ loại bỏ nó.
 Quan trọng là hiểu rằng điều này có nghĩa là bất kỳ dữ liệu nào được sở hữu luôn luôn vượt qua ràng buộc vòng đời 'static, nhưng một tham chiếu đến dữ liệu được sở hữu đó thông thường không vượt qua ràng buộc vòng đời 'static:
  
  use std::fmt::Debug;

fn print_it( input: impl Debug + 'static ) {
println!( "'static value passed in is: {:?}", input );
}

fn main() {
// i là sở hữu và không chứa tham chiếu, do đó nó là 'static:
let i = 5;
print_it(i);
  // Xin lỗi, &i chỉ có tuổi thọ được xác định bởi phạm vi của main(),
// do đó nó không phải là 'static:
print_it(&i);
}
  
Trình biên dịch sẽ thông báo cho bạn:

lỗi [E0597]: i không tồn tại trong thời gian đủ lâu
--> src/lib.rs:15:15
|
15 | print_it(&i);
| ---------^^--
| | |
| | giá trị mượn không tồn tại trong thời gian đủ lâu
| đối số yêu cầu i được mượn cho 'static
16 | }
| - i bị loại bỏ ở đây trong khi vẫn đang được mượn
