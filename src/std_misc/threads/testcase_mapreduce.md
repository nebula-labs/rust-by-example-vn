# Testcase: map-reduce

Rust khiến cho việc xử lý dữ liệu đa luồng trở nên rất dễ dàng mà không đối mặt với
các rắc rối thường gặp khi xử lý so với các ngôn ngữ khác.

Thư viện tiêu chuẩn cung cấp các nguyên mẫu đa luồng tuyệt vời sẵn có. Kết hợp với khái niệm Sở hữu và quy tắc
định danh của Rust, hiện tượng cạnh tranh dữ liệu sẽ tự động được ngăn chặn.

Các quy tắc định danh (một tham chiếu có thể ghi (writable reference) XOR nhiều tham chiếu chỉ đọc (readable references))
tự động ngăn bạn thay đổi trạng thái mà các luồng khác có thể nhìn thấy. (Khi cần đồng bộ hóa, ta có thể
dùng các nguyên tắc đồng bộ hóa như `Mutex` hoặc `Channel`.)

Trong ví dụ này, ta sẽ tính tổng của tất cả các chữ số trong một khối số.
Ta sẽ làm điều này bằng cách chia khối thành các phần khác nhau trong các luồng khác nhau.
Mỗi luồng sẽ tính tổng các chữ số trong khối nhỏ của nó, và sau đó ta sẽ tổng hợp các tổng được tạo ra bởi mỗi luồng.

Lưu ý rằng, mặc dù ta đang truyền tham chiếu qua ranh giới luồng, Rust hiểu rằng ta
chỉ truyền tham chiếu chỉ đọc(read-only) và do đó không có tình trạng không an toàn hoặc tranh chấp dữ liệu
nào xảy ra. Ngoài ra, vì tham chiếu ta đang truyền có tuổi thọ `'static`, Rust hiểu rằng dữ
liệu của ta sẽ không bị phá hủy trong khi các luồng này vẫn đang chạy.
(Khi cần chia sẻ dữ liệu không có tuổi thọ `'static` giữa các luồng, bạn có thể sử dụng con trỏ
thông minh như `Arc` để giữ cho dữ liệu sống và tránh các tuổi thọ không phải là `'static`.)


```rust,editable
use std::thread;

// Luồng "main"
fn main() {

    // Dữ liệu chúng ta sẽ xử lý
    // Chúng ta sẽ tính tổng của tất cả các chữ số thông qua thuật toán map-reduce trên nhiều luồng.
    // Mỗi phân đoạn được tách ra bằng dấu cách sẽ được xử lý trên một luồng khác nhau.
    //
    // TODO: hãy xem kết quả sẽ ra sao nếu bạn chèn thêm dấu cách!
    let data = "86967897737416471853297327050364959
11861322575564723963297542624962850
70856234701860851907960690014725639
38397966707106094172783238747669219
52380795257888236525459303330302837
58495327135744041048897885734297812
69920216438980873548808413720956532
16278424637452589860345374828574668";

    // Tạo một vector để chứa các luồng con mà chúng ta sẽ khởi động.
    let mut children = vec![];

    /*************************************************************************
     * Giai đoạn "Map"
     *
     * Chia dữ liệu của chúng ta thành các phân đoạn, và bắt đầu xử lý
     ************************************************************************/

    // Tách dữ liệu của chúng ta thành các phân đoạn cho từng phần tính toán
    // Mỗi phân đoạn sẽ là một tham chiếu (&str) đến dữ liệu thực tế
    let chunked_data = data.split_whitespace();

    // Chạy qua các phân đoạn dữ liệu.
    // .enumerate() thêm chỉ mục (index) của vòng lặp hiện tại vào bất cứ điều gì được chạy qua
    // bộ đôi kết quả "(chỉ mục, phần tử)" sau đó được tự động
    // "destructured" thành hai biến, "i" và "data_segment" với một
    // "destructuring assignment"
    for (i, data_segment) in chunked_data.enumerate() {
        println!("Phân đoạn dữ liệu {} là \"{}\"", i, data_segment);

        // Xử lý mỗi phân đoạn dữ liệu trong một luồng riêng biệt
        //
        // spawn() trả về một handle cho luồng mới,
        // mà chúng ta PHẢI giữ lại để truy cập giá trị trả về
        //
        // 'move || -> u32' là cú pháp cho một closure mà:
        // * không có đối số ('||')
        // * lấy sở hữu các biến bị chụp ('move') và
        // * trả về một số nguyên 32-bit không dấu ('-> u32')
        //
        // Rust đủ thông minh để có thể suy luận được '-> u32' 
        // từ chính closure nên ta có thể bỏ nó đi.
        //
        // TODO: thử xóa 'move' và xem điều gì sẽ xảy ra
        children.push(thread::spawn(move || -> u32 {
            // Tính tổng giữa kết quả tạm thời của đoạn dữ liệu này:
            let result = data_segment
                        // chạy qua các ký tự trong đoạn dữ liệu..
                        .chars()
                        // lặp lại các ký tự trong đoạn dữ liệu..
                        .map(|c| c.to_digit(10).expect("nên là chữ số"))
                        //  .. và tính tổng
                        .sum();

            // println! khóa stdout, để không có tình trạng giao thoa văn bản xảy ra
            println!("Đoạn dữ liệu {}, kết quả={}", i, result);

            // không cần "return", bởi vì Rust là một "ngôn ngữ biểu thức",
            // biểu thức được đánh giá cuối cùng trong mỗi khối sẽ tự động là giá trị của nó.
            result

        }));
    }


    /*************************************************************************
     * Giai đoạn "Reduce"
     *
     * Tổng hợp kết quả trung gian và kết hợp chúng thành kết quả cuối cùng
     ************************************************************************/

    // Kết hợp kết quả trung gian của từng thread thành một tổng kết quả cuối cùng.
    //
    // chúng ta sử dụng "turbofish" ::<> để cung cấp cho sum() một gợi ý kiểu dữ liệu.
    //
    // TODO: hãy thử không sử dụng turbofish, thay vào đó chỉ rõ kiểu của final_result
    let final_result = children.into_iter().map(|c| c.join().unwrap()).sum::<u32>();

    println!("Kết quả tổng cuối cùng: {}", final_result);
}


```

### Gán giá trị
Không nên để số lượng luồng phụ thuộc vào dữ liệu được nhập từ người dùng.
Nếu người dùng quyết định nhập nhiều dấu cách, liệu chúng ta có muốn tạo ra 2,000 luồng không?
Ta nên sửa đổi chương trình sao cho dữ liệu luôn được chia thành một số lượng nhỏ các phần, được xác định
bởi một hằng số tĩnh ở đầu chương trình.

### Xem thêm:
* [Threads][thread]
* [vectors][vectors] và [iterators][iterators]
* [closures][closures], [move][move] semantics và [`move` closures][move_closure]
* [destructuring][destructuring] gán giá trị
* [turbofish notation][turbofish] để hỗ trợ suy luận kiểu
* [unwrap vs. expect][unwrap]
* [enumerate][enumerate]

[thread]: ../threads.md
[vectors]: ../../std/vec.md
[iterators]: ../../trait/iter.md
[destructuring]: https://doc.rust-lang.org/book/ch18-03-pattern-syntax.html#destructuring-to-break-apart-values
[closures]: ../../fn/closures.md
[move]: ../../scope/move.md
[move_closure]: https://doc.rust-lang.org/book/ch13-01-closures.html#closures-can-capture-their-environment
[turbofish]: https://doc.rust-lang.org/book/appendix-02-operators.html?highlight=turbofish
[unwrap]: ../../error/option_unwrap.md
[enumerate]: https://doc.rust-lang.org/book/loops.html#enumerate