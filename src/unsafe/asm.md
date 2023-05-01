# Inline assembly

Rust cung cấp hỗ trợ cho việc sử dụng inline assembly thông qua macro `asm!`.
Nó có thể được dùng để nhúng mã assembly được viết bằng tay vào mã assembly được tạo ra bởi trình biên dịch.
Thường thì không cần phải dùng đến nó, nhưng nó có thể được dùng để đạt được hiệu năng hoặc thời gian thực thi mong muốn. Truy cập các thành phần cấp thấp của phần cứng, ví dụ trong kernel code, cũng có thể yêu cầu sử dụng tính năng này.

> **Ghi chú**: các ví dụ ở đây được viết bằng assembly x86/x86-64, nhưng các kiến trúc khác cũng được hỗ trợ.

Inline assembly hiện được hỗ trợ trên các kiến trúc sau:

- x86 and x86-64
- ARM
- AArch64
- RISC-V

## Cách dùng cơ bản

Hãy cùng bắt đầu với ví dụ đơn giản nhất:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

unsafe {
    asm!("nop");
}
# }
```

Đoạn mã sẽ chèn một lệnh `NOP` (no operation) vào mã assembly được tạo ra bởi trình biên dịch.
Lưu ý rằng tất cả các lệnh `asm!` phải được đặt trong một khối `unsafe`, vì chúng có thể chèn các lệnh bất kỳ và làm phá hỏng chương trình của bạn. Các lệnh cần chèn vào được liệt kê trong tham số đầu tiên của macro `asm!` dưới dạng một chuỗi ký tự.

## Các đầu vào và đầu ra

Việc chèn thêm một lệnh mà không gì cả thì không có ý nghĩa gì. Hãy thử làm một chút điều gì đó có thể tác động lên dữ liệu:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

let x: u64;
unsafe {
    asm!("mov {}, 5", out(reg) x);
}
assert_eq!(x, 5);
# }
```

Đoạn mã này sẽ ghi giá trị `5` vào biến `u64` `x`.
Bạn có thể thấy rằng chuỗi ký tự chúng ta sử dụng để chỉ định các lệnh thực sự là một chuỗi mẫu.
Nó được quản lý theo cùng quy tắc với [chuỗi định dạng][format-syntax] của Rust.
Các đối số được chèn vào chuỗi mẫu trông có vẻ khác một chút so với cái bạn đã có thể quen thuộc. Đầu tiên chúng ta cần chỉ định biến là đầu vào hay đầu ra của inline assembly. Trong trường hợp này nó là đầu ra. Chúng ta đã khai báo điều này bằng cách viết `out`.
Chúng ta cũng cấn chỉ định kiểu của thanh ghi mà assembly mong đợi biến đó. Trong trường hợp này chúng ta đặt nó trong một thanh ghi bất kỳ bằng cách chỉ định `reg`.
Trình biên dịch sẽ chọn một thanh ghi phù hợp để chèn vào mẫu và sẽ đọc biến từ đó sau khi inline assembly thực thi xong.

[format-syntax]: https://doc.rust-lang.org/std/fmt/#syntax

Cùng xem xét một ví dụ khác sử dụng đầu vào:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

let i: u64 = 3;
let o: u64;
unsafe {
    asm!(
        "mov {0}, {1}",
        "add {0}, 5",
        out(reg) o,
        in(reg) i,
    );
}
assert_eq!(o, 8);
# }
```

Ví dụ này sẽ thêm `5` vào biến `i` và ghi kết quả vào biến `o`.
Cách assembly thực hiện điều này là đầu tiên sao chép giá trị từ `i` đến đầu ra, và sau đó thêm `5` vào nó.

Ví dụ cho thấy một số điều:

Đầu tiên, chúng ta có thể thấy rằng `asm!` cho phép sử dụng nhiều chuỗi mẫu; mỗi chuỗi được xem như một dòng mã assembly riêng rẽ, giống như nó được nối với nhau bởi dấu xuống dòng. Điều này làm cho việc định dạng mã assembly dễ dàng hơn.

Thứ hai, chúng ta có thể thấy rằng các đầu vào được khai báo bằng cách viết `in` thay vì `out`.

Thứ ba, chúng ta có thể thấy rằng chúng ta có thể chỉ định một số thứ khác như số thứ tự của đối số, hoặc tên như trong bất kỳ chuỗi định dạng nào. Đối với các mẫu inline assembly, điều này đặc biệt hữu ích vì các đối số thường được sử dụng nhiều lần. Đối với inline assembly phức tạp hơn, sử dụng cơ chế này được khuyến khích, vì nó cải thiện khả năng đọc và cho phép sắp xếp lại các lệnh mà không thay đổi thứ tự đối số.

Chúng ta có thể điều chỉnh ví dụ trên để bỏ qua lệnh `move`

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

let mut x: u64 = 3;
unsafe {
    asm!("add {0}, 5", inout(reg) x);
}
assert_eq!(x, 8);
# }
```

Chúng ta có thể thấy rằng `inout` được sử dụng để chỉ định một đối số là cả đầu vào và đầu ra. Điều này khác với việc chỉ định đầu vào và đầu ra riêng biệt ở chỗ nó đảm bảo gán cả hai vào cùng một thanh ghi.

Nó cũng có thể được sử dụng để chỉ định các biến khác nhau cho phần đầu vào và đầu ra của một đối số `inout`:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

let x: u64 = 3;
let y: u64;
unsafe {
    asm!("add {0}, 5", inout(reg) x => y);
}
assert_eq!(y, 8);
# }
```

## Các toán hạng đầu ra trễ

Trình biên dịch của Rust cẩn thận với việc phân bổ các toán hạng. Nó được giả định rằng một `out` có thể được ghi vào bất cứ lúc nào, và do đó không thể chia sẻ vị trí của nó với bất kỳ đối số nào khác. Tuy nhiên, để đảm bảo hiệu suất tối ưu, một điều quan trọng là sử dụng ít thanh ghi nhất có thể, vì vậy chúng sẽ không phải được lưu và tải lại xung quanh khối mã inline assembly được nhúng. Để đạt được điều này, Rust cung cấp một chỉ định `lateout`. Cái này có thể được sử dụng trên bất kỳ đầu ra nào được ghi chỉ sau khi tất cả các đầu vào đã được tiêu thụ. Cũng có một phiên bản `inlateout` của chỉ định này.

Đây là một ví dụ mà `inlateout` _không thể_ được sử dụng trong chế độ `release` hoặc các trường hợp tối ưu hóa khác:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

let mut a: u64 = 4;
let b: u64 = 4;
let c: u64 = 4;
unsafe {
    asm!(
        "add {0}, {1}",
        "add {0}, {2}",
        inout(reg) a,
        in(reg) b,
        in(reg) c,
    );
}
assert_eq!(a, 12);
# }
```

Mã trên có thể làm việc tốt trong các trường hợp chưa được tối ưu hóa (`Debug` mode), nhưng nếu bạn muốn hiệu suất tối ưu (`release` mode hoặc các trường hợp tối ưu hóa khác), nó có thể không hoạt động.

Điều đó là bởi vì trong các trường hợp tối ưu hóa, trình biên dịch có thể tự do phân bổ cùng một thanh ghi cho các đầu vào `b` và `c` vì nó biết chúng có cùng giá trị. Tuy nhiên, nó phải phân bổ một thanh ghi riêng cho `a` vì nó sử dụng `inout` và không phải `inlateout`. Nếu `inlateout` được sử dụng, thì `a` và `c` có thể được phân bổ vào cùng một thanh ghi, trong trường hợp đó lệnh đầu tiên sẽ ghi đè giá trị của `c` và làm cho mã assembly tạo ra kết quả sai.

Tuy nhiên, ví dụ sau có thể sử dụng `inlateout` vì đầu ra chỉ được sửa đổi sau khi tất cả các đầu vào đã được đọc:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

let mut a: u64 = 4;
let b: u64 = 4;
unsafe {
    asm!("add {0}, {1}", inlateout(reg) a, in(reg) b);
}
assert_eq!(a, 8);
# }
```

Như bạn thấy, đoạn mã này vẫn hoạt động chính xác nếu `a` và `b` được gán cho cùng một thanh ghi.

## Các toán hạng thanh ghi rõ ràng

Một vài lệnh yêu cầu rằng các toán hạng phải nằm trong một thanh ghi cụ thể. Do đó, Rust inline assembly cung cấp một số chỉ định ràng buộc cụ thể hơn. Trong khi `reg` có sẵn trên bất kỳ kiến trúc nào, các thanh ghi rõ ràng chỉ có sẵn trên kiến trúc cụ thể. Ví dụ, cho x86, các thanh ghi chung `eax`, `ebx`, `ecx`, `edx`, `ebp`, `esi`, và `edi` có thể được chỉ định bằng tên của chúng.

```rust,no_run
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

let cmd = 0xd1;
unsafe {
    asm!("out 0x64, eax", in("eax") cmd);
}
# }
```

Trong ví dụ này chúng ta gọi lệnh `out` để xuất nội dung của biến `cmd` ra cổng `0x64`. Vì lệnh `out` chỉ chấp nhận `eax` (và các thanh ghi con của nó) làm toán hạng, chúng ta phải sử dụng chỉ định ràng buộc `eax`.

> **Ghi chú**: không giống với các kiểu toán hạng khác, các toán hạng thanh ghi rõ ràng không thể được sử dụng trong chuỗi mẫu: bạn không thể sử dụng `{}` thay vào đó nên viết tên thanh ghi trực tiếp. Ngoài ra, chúng phải xuất hiện ở cuối danh sách toán hạng sau tất cả các loại toán hạng khác.

Xem xét ví dụ này sử dụng lệnh `mul` của x86:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

fn mul(a: u64, b: u64) -> u128 {
    let lo: u64;
    let hi: u64;

    unsafe {
        asm!(
            // Lệnh mul của x86 nhận rax là một đầu vào ngầm định và ghi
            // kết quả 128-bit của phép nhân vào rax:rdx.
            "mul {}",
            in(reg) a,
            inlateout("rax") b => lo,
            lateout("rdx") hi
        );
    }

    ((hi as u128) << 64) + lo as u128
}
# }
```

Ở đây sử dụng lệnh `mul` để nhân hai số 64-bit với kết quả 128-bit. Chỉ có một toán hạng rõ ràng là một thanh ghi, chúng ta lấy nó từ biến `a`. Toán hạng thứ hai là ngầm định và phải là thanh ghi `rax`, chúng ta lấy nó từ biến `b`. 64-bit thấp của kết quả được lưu trong `rax` từ đó chúng ta đưa vào biến `lo`. 64-bit cao được lưu trong `rdx` từ đó chúng ta đưa nó vào biến `hi`.

## Clobbered registers

Trong nhiều trường hợp, inline assembly sẽ sửa đổi trạng thái không cần thiết là đầu ra. Thông thường điều này là do chúng ta phải sử dụng một thanh ghi tạm trong assembly hoặc vì các lệnh sửa đổi trạng thái mà chúng ta không cần phải xem xét thêm. Trạng thái này thường được xem là "Bị xáo trộn" (`"clobbered"`). Chúng ta cần phải thông báo cho trình biên dịch về điều này vì nó có thể cần phải lưu và khôi phục trạng thái này xung quanh khối inline assembly.

```rust
use std::arch::asm;

# #[cfg(target_arch = "x86_64")]
fn main() {
    // three entries of four bytes each
    // Ba mục của bốn byte mỗi mục
    let mut name_buf = [0_u8; 12];

    // Chuỗi được lưu trữ dưới dạng ascii trong ebx, edx, ecx theo thứ tự
    // Vì ebx được dành riêng, asm cần phải giữ giá trị của nó.
    // Vì vậy chúng ta push và pop nó xung quanh asm chính.
    // (trong chế độ 64 bit đối với các vi xử lý 64 bit, các xử lý 32 bit sẽ sử dụng ebx)

    unsafe {
        asm!(
            "push rbx",
            "cpuid",
            "mov [rdi], ebx",
            "mov [rdi + 4], edx",
            "mov [rdi + 8], ecx",
            "pop rbx",
            // Chúng ta sử dụng con trỏ đến một mảng cho việc lưu trữ các giá trị để đơn giản hóa
            // mã Rust nhưng điều này đòi hỏi phải thêm một số lệnh asm
            // Điều này được thể hiện rõ ràng hơn cách hoạt động của asm, khác với
            // các đầu ra thanh ghi rõ ràng như `out("ecx") val`
            // *Con trỏ chính nó* chỉ là đầu vào ngầm định mặc dù nó được ghi sau
            in("rdi") name_buf.as_mut_ptr(),
            // chọn cpuid 0, cũng chỉ định eax là bị xáo trộn
            inout("eax") 0 => _,
            // cpuid cũng làm xáo trộn các thanh ghi này
            out("ecx") _,
            out("edx") _,
        );
    }

    let name = core::str::from_utf8(&name_buf).unwrap();
    println!("CPU Manufacturer ID: {}", name);
}

# #[cfg(not(target_arch = "x86_64"))]
# fn main() {}
```

Trong ví dụ trên, chúng ta sử dụng lệnh `cpuid` để đọc ID của nhà sản xuất CPU. Lệnh này ghi vào `eax` với tham số `cpuid` tối đa được hỗ trợ và `ebx`, `edx`, và `ecx` với ID của nhà sản xuất CPU là các byte ASCII theo thứ tự đó.

Mặc dù `eax` không bao giờ được đọc chúng ta vẫn cần phải thông báo cho trình biên dịch rằng thanh ghi đã bị sửa đổi để trình biên dịch có thể lưu bất kỳ giá trị nào trong các thanh ghi này trước khi vào khối asm. Điều này được thực hiện bằng cách khai báo nó là một đầu ra nhưng với tên biến là `_` thay vì tên biến, điều này chỉ ra rằng giá trị đầu ra sẽ bị bỏ đi.

Đoạn mã này cũng giải quyết giới hạn của `ebx` là một thành ghi được dành riêng bởi LLVM. Điều đó có nghĩa là LLVM cho rằng nó có đầy đủ quyền kiểm soát thanh ghi và nó phải được khôi phục về trạng thái ban đầu trước khi thoát khỏi khối asm, vì vậy nó không thể được sử dụng như một input hoặc output **trừ khi** trình biên dịch sử dụng nó để gán cho một lớp thanh ghi chung (ví dụ: `in(reg)`). Điều này làm cho các toán hạng `reg` nguy hiểm khi sử dụng thanh ghi được dành riêng vì chúng ta có thể làm hỏng đầu vào hoặc đầu ra của mình mà không biết vì chúng chia sẻ cùng một thanh ghi.

Để làm việc với giới hạn này, chúng ta sử dụng `rdi` để lưu trữ con trỏ đến mảng đầu ra, lưu `ebx` thông qua `push`, đọc từ `ebx` bên trong khối asm vào mảng và sau đó khôi phục `ebx` về trạng thái ban đầu thông qua `pop`. `push` và `pop` sử dụng phiên bản 64 bit đầy đủ của `rbx` để đảm bảo rằng toàn bộ thanh ghi được lưu. Trên các kiến trúc 32 bit, mã sẽ sử dụng `ebx` trong `push`/`pop`.

Điều này có thể được sử dụng với một lớp thanh ghi chung để lấy một thanh ghi tạm thời sử dụng bên trong mã asm:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

// Nhân x với 6 sử dụng shifts và adds
let mut x: u64 = 4;
unsafe {
    asm!(
        "mov {tmp}, {x}",
        "shl {tmp}, 1",
        "shl {x}, 2",
        "add {x}, {tmp}",
        x = inout(reg) x,
        tmp = out(reg) _,
    );
}
assert_eq!(x, 4 * 6);
# }
```

## Các toán hạng biểu tượng và ABI clobber

Mặc định, `asm!` giả định rằng bất kỳ thanh ghi nào không được chỉ định là đầu ra sẽ được giữ nguyên bởi mã assembly. Tham số [`clobber_abi`] của `asm!` cho trình biên dịch biết để tự động thêm các toán hạng clobber tương ứng với ABI gọi hàm đã cho: bất kỳ thanh ghi nào không được hoàn toàn giữ nguyên trong ABI đó sẽ được coi là bị xáo trộn (clobbered). Nhiều đối số `clobber_abi` có thể được cung cấp và tất cả các clobber từ tất cả các ABI được chỉ định sẽ được thêm vào.

[`clobber_abi`]: ../../reference/inline-assembly.html#abi-clobbers

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

extern "C" fn foo(arg: i32) -> i32 {
    println!("arg = {}", arg);
    arg * 2
}

fn call_foo(arg: i32) -> i32 {
    unsafe {
        let result;
        asm!(
            "call {}",
            // Con trỏ hàm để gọi
            in(reg) foo,
            // đối số thứ nhất trong rdi
            in("rdi") arg,
            // giá trị trả về trong rax
            out("rax") result,
            // Đánh dấu tất cả các thanh ghi không được giữ nguyên bởi lời gọi "C"
            // là bị xáo trộn.
            clobber_abi("C"),
        );
        result
    }
}
# }
```

## Register template modifiers

Trong một vài trường hợp, ta cần điều khiển cách mà tên các thanh ghi được định dạng khi chèn vào trong một chuỗi mẫu. Điều này là cần thiết khi một ngôn ngữ lập trình hợp ngữ của một kiến trúc có nhiều tên cho cùng một thanh ghi, mỗi tên thường là một "view" trên một tập con của thanh ghi (ví dụ: 32 bit thấp của một thanh ghi 64 bit).

Mặc định thì trình biên dịch sẽ luôn chọn tên mà tham chiếu đến kích thước đầy đủ của thanh ghi (ví dụ: `rax` trên x86-64, `eax` trên x86, v.v.).

Mặc định này có thể bị ghi đè bằng cách sử dụng các bộ điều khiển trên các toán hạng chuỗi mẫu, giống như bạn làm với chuỗi định dạng:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

let mut x: u16 = 0xab;

unsafe {
    asm!("mov {0:h}, {0:l}", inout(reg_abcd) x);
}

assert_eq!(x, 0xabab);
# }
```

Trong ví dụ này, chúng ta sử dụng lớp thanh ghi `reg_abcd` để hạn chế bộ phân bổ thanh ghi chỉ sử dụng 4 thanh ghi x86 cổ điển (`ax`, `bx`, `cx`, `dx`) mà mỗi thanh ghi có 2 byte đầu có thể được truy cập độc lập .

Chúng ta giả định rằng bộ phân bổ thanh ghi đã chọn để phân bổ `x` trong thanh ghi `ax`. Bộ điều khiển `h` sẽ xuất tên thanh ghi cho byte cao của thanh ghi đó và bộ điều khiển `l` sẽ xuất tên thanh ghi cho byte thấp của nó. Mã asm sẽ được mở rộng thành `mov ah, al` để sao chép byte thấp của giá trị vào byte cao.

Nếu bạn sử dụng một kiểu dữ liệu nhỏ hơn (ví dụ: `u16`) với một toán hạng và quên sử dụng các bộ điều khiển mẫu, trình biên dịch sẽ xuất cảnh báo và đề nghị sử dụng bộ điều khiển mẫu chính xác.

## Các toán hạng địa chỉ bộ nhớ

Thỉnh thoảng các chỉ dẫn lập trình yêu cầu các toán hạng được truyền qua địa chỉ bộ nhớ hoặc vị trí bộ nhớ. Bạn phải tự sử dụng cú pháp địa chỉ bộ nhớ được chỉ định bởi kiến trúc đích. Ví dụ, trên x86/x86_64 sử dụng cú pháp lập trình hợp ngữ Intel, bạn nên bao các đầu vào/đầu ra trong `[]` để chỉ rõ chúng là các toán hạng bộ nhớ:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

fn load_fpu_control_word(control: u16) {
    unsafe {
        asm!("fldcw [{}]", in(reg) &control, options(nostack));
    }
}
# }
```

## Các nhãn

Việc tái sử dụng một nhãn đã đặt tên, cục bộ hoặc không, có thể dẫn đến lỗi trình biên dịch hoặc liên kết hoặc có thể gây ra các hành vi khác. Việc tái sử dụng một nhãn có thể xảy ra mtheo các cách khác nhau bao gồm:

- explicitly: sử dụng một nhãn nhiều lần trong một khối `asm!`, hoặc nhiều lần trên nhiều khối.
- implicitly thông qua inlining: trình biên dịch được phép tạo ra nhiều bản sao của một khối `asm!`, ví dụ khi hàm chứa nó được inlined vào nhiều nơi.
- implicitly thông qua LTO: LTO có thể gây ra việc mã từ _các crate khác_ được đặt trong cùng một đơn vị biên dịch, và do đó có thể đưa vào các nhãn bất kỳ.

Do đó, bạn chỉ nên sử dụng các [nhãn cụ bộ] **số học** của GNU assembler trong inline assembly. Định nghĩa các ký hiệu trong mã có thể dẫn đến lỗi trình biên dịch và/hoặc liên kết do định nghĩa các ký hiệu trùng lặp.

Hơn nữa, trên x86 khi sử dụng cú pháp Intel mặc định, do [một lỗi LLVM], bạn không nên sử dụng các nhãn chỉ bao gồm các chữ số `0` và `1`, ví dụ: `0`, `11` hoặc `101010`, vì chúng có thể được hiểu là các giá trị nhị phân. Sử dụng `options(att_syntax)` sẽ giúp tránh được bất kỳ một sự hiểu lầm nào, nhưng điều đó ảnh hưởng đến cú pháp của _toàn bộ_ khối `asm!`. (Xem [Options](#options), bên dưới, để biết thêm về `options`.)

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

let mut a = 0;
unsafe {
    asm!(
        "mov {0}, 10",
        "2:",
        "sub {0}, 1",
        "cmp {0}, 3",
        "jle 2f",
        "jmp 2b",
        "2:",
        "add {0}, 2",
        out(reg) a
    );
}
assert_eq!(a, 5);
# }
```

Mã này sẽ giảm giá trị của thanh ghi `{0}` từ 10 xuống 3, sau đó cộng 2 và lưu vào `a`.

Ví dụ này cho thấy một vài điều:

- Đầu tiên, cùng một số có thể được sử dụng làm nhãn nhiều lần trong cùng một khối inline.
- Thứ hai, khi một nhãn số được sử dụng như một tham chiếu (ví dụ: như một toán hạng của một chỉ dẫn), bạn nên thêm các hậu tố `b` (`backward`) hoặc `f` (`forward`) vào nhãn số. Nó sẽ tham chiếu đến nhãn gần nhất được định nghĩa bởi số này theo hướng này.

[local labels]: https://sourceware.org/binutils/docs/as/Symbol-Names.html#Local-Labels
[an LLVM bug]: https://bugs.llvm.org/show_bug.cgi?id=36144

## Các tùy chọn

Mặc định, một khối inline assembly được xem như một lời gọi hàm FFI bên ngoài với một quy ước gọi tùy chỉnh: nó có thể đọc/ghi bộ nhớ, có thể có các tác động phụ quan sát được, v.v. Tuy nhiên, trong nhiều trường hợp, việc cung cấp cho trình biên dịch thêm thông tin về những gì mã assembly đang làm để nó có thể tối ưu hóa tốt hơn.

Hãy xem xét ví dụ trước về một chỉ dẫn `add`:

```rust
# #[cfg(target_arch = "x86_64")] {
use std::arch::asm;

let mut a: u64 = 4;
let b: u64 = 4;
unsafe {
    asm!(
        "add {0}, {1}",
        inlateout(reg) a, in(reg) b,
        options(pure, nomem, nostack),
    );
}
assert_eq!(a, 8);
# }
```

Các lựa chọn có thể được cung cấp như một đối số cuối cùng tùy chọn cho macro `asm!`. Chúng ta chỉ định ba tùy chọn ở đây:

- `pure` có nghĩa là mã assembly không có bất kỳ tác động phụ nào có thể quan sát được và đầu ra của nó chỉ phụ thuộc vào đầu vào. Điều này cho phép trình biên dịch tối ưu hóa để gọi mã assembly ít hơn hoặc thậm chí loại bỏ nó hoàn toàn.
- `nomem` có nghĩa là mã assembly không đọc/ghi bộ nhớ. Theo mặc định, trình biên dịch sẽ giả định rằng mã assembly có thể đọc/ghi bất kỳ địa chỉ bộ nhớ nào mà nó có thể truy cập (ví dụ: thông qua một con trỏ được truyền như một toán hạng, hoặc một biến toàn cục).
- `nostack` có nghĩa là mã assembly không đẩy bất kỳ dữ liệu nào lên stack. Điều này cho phép trình biên dịch sử dụng các tối ưu hóa như ngăn xếp vùng đỏ trên x86-64 để tránh điều chỉnh con trỏ ngăn xếp.

Những điều này cho phép trình biên dịch tối ưu hóa mã sử dụng `asm!` tốt hơn, ví dụ như bằng cách loại bỏ các khối `asm!` hoàn toàn không có đầu ra.

Xem [reference](../../reference/inline-assembly.html) để biết danh sách đầy đủ các tùy chọn và tác dụng của chúng.
