# Phantom type parameters

Một tham số kiểu phantom là một kiểu không xuất hiện trong thời gian chạy, nhưng được kiểm tra tĩnh (và chỉ) tại thời điểm biên dịch.

Các kiểu dữ liệu có thể sử dụng các tham số kiểu generic bổ sung để hoạt động như các nhãn hoặc để thực hiện kiểm tra kiểu tại thời điểm biên dịch. Những tham số bổ sung này không giữ các giá trị bộ nhớ và không có hành vi thời gian chạy (runtime behavior).

Trong ví dụ sau, chúng ta kết hợp [std::marker::PhantomData] với khái niệm tham số kiểu phantom để tạo các tuple chứa các kiểu dữ liệu khác nhau.

```rust,editable
use std::marker::PhantomData;

// Một cấu trúc tuple phantom được tạo ra dựa trên kiểu `A` với tham số kiểu `B` ẩn.
#[derive(PartialEq)] // Cho phép kiểu này có thể kiểm tra bằng nhau.
struct PhantomTuple<A, B>(A, PhantomData<B>);

// Một cấu trúc phantom được tạo ra dựa trên kiểu `A` với tham số kiểu `B` ẩn.
#[derive(PartialEq)] // Cho phép kiểu này có thể kiểm tra bằng nhau.
struct PhantomStruct<A, B> { first: A, phantom: PhantomData<B> }

// Ghi chú: bộ nhớ được cấp phát cho kiểu tổng quát `A`, nhưng không được cấp phát cho `B`.
//       Vì vậy, `B` không thể được dùng trong tính toán.

fn main() {
    // Ở đây, `f32` và `f64` là các tham số ẩn.
    // Kiểu PhantomTuple được chỉ định là `<char, f32>`.
    let _tuple1: PhantomTuple<char, f32> = PhantomTuple('Q', PhantomData);
    // Kiểu PhantomTuple được chỉ định là `<char, f64>`.
    let _tuple2: PhantomTuple<char, f64> = PhantomTuple('Q', PhantomData);

    // Kiểu chỉ định là `<char, f32>`.
    let _struct1: PhantomStruct<char, f32> = PhantomStruct {
        first: 'Q',
        phantom: PhantomData,
    };
    // Kiểu chỉ định là `<char, f64>`.
    let _struct2: PhantomStruct<char, f64> = PhantomStruct {
        first: 'Q',
        phantom: PhantomData,
    };

    // Compile-time Error! Kiểu dữ liệu không khớp với kiểu đã chỉ định nên không thể so sánh:
    // println!("_tuple1 == _tuple2 yields: {}",
    //           _tuple1 == _tuple2);

    // Compile-time Error! Kiểu dữ liệu không khớp với kiểu đã chỉ định nên không thể so sánh:
    // println!("_struct1 == _struct2 yields: {}",
    //           _struct1 == _struct2);
}
```

### Xem thêm:

[Derive], [struct], and [TupleStructs]

[derive]: ../trait/derive.md
[struct]: ../custom_types/structs.md
[tuplestructs]: ../custom_types/structs.md
[std::marker::phantomdata]: https://doc.rust-lang.org/std/marker/struct.PhantomData.html
