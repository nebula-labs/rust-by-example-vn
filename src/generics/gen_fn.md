# Functions
Một bộ quy tắc có thể được áp dụng cho các function: Một kiểu `T` trở thành generic khi khai báo generic type '<T>' sau tên function.

Việc sử dụng generic function đôi khi yêu cầu kiểu dữ liệu cụ thể cho tham số . Điều này xảy ra nếu hàm được gọi trong trường hợp kiểu trả về là generic, hoặc nếu trình biên dịch không đủ thông tin để suy ra các kiểu của tham số cần thiết.

Một hàm với kiểu dữ liệu cụ thể có cú pháp như sau: `fun::<A, B, ...>()`.

```rust
struct A;          // Kiểu cụ thể `A`.
struct S(A);       // Kiểu cụ thể `S`.
struct SGen<T>(T); // Kiểu Generic `SGen`.


// Tất cả các function dưới đây sẽ take ownership của variable và
// ngay lập tức thoát khỏi scope, giải phóng variable. 

// Định nghĩa function `reg_fn` với tham số ``_s` có kiểu `S`
// Không có `<T>` nên đây không phải là generic function.
fn reg_fn(_s: S) {}

// Định nghĩa function `gen_spec_t` với một tham số `_s` có kiểu `SGen<T>`.
// Hàm này đã chỉ rõ type parameter là `A`, nhưng bời vì `A` không được 
// chỉ định như một generic type parameter cho `gen_spec_t` nên đây không phải là generic.
fn gen_spec_t(_s: SGen<A>) {}

// Định nghĩa một hàm `gen_spec_i32` với tham số là `_s` có kiểu `SGen<i32`>.
// Hàm này chỉ rõ type parameter là `i32`. 
// Bởi vì `i32` không phải là một generic type, nên hàm này cũng không phải là generic function.
fn gen_spec_i32(_s: SGen<i32>) {}

// Định nghĩa một `generic` function với tham số '_s' có kiểu 'SGen<T>'.
// Bởi vì `SGen<T>` được đứng trước bởi `<T>`, nên function này là generic bởi `T`.

fn generic<T>(_s: SGen<T>) {}

fn main() {
    // Sử dụng non-generic function
    reg_fn(S(A));          // Concrete type.
    gen_spec_t(SGen(A));   // Ngầm chỉ định type parameter `A`
    gen_spec_i32(SGen(6)); // Ngầm chỉ định type parameter `i32`.

    // Chỉ định cụ thể kiểu `char` cho `generic()` function.
    generic::<char>(SGen('a'));

    // Chỉ định cụ thể kiểu `char` cho `generic()` function.
    generic(SGen('c'));
}
```