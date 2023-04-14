# Foreign Function Interface

Rust cung cấp một Foreign Function Interface (FFI) tới các thư viện C. Các
hàm ngoại lai phải được khai báo trong một khối `extern` được ghi chú với một
thuộc tính `#[link]` chứa tên của thư viện bên ngoài.

```rust,ignore
use std::fmt;

// khối extern này liên kết tới thư viện libm
#[link(name = "m")]
extern {
    // đây là một hàm ngoại lai tính cnă bậc hai của một số phức
    fn csqrtf(z: Complex) -> Complex;

    fn ccosf(z: Complex) -> Complex;
}

// Khi việc gọi một hàm ngoại lai được coi là không an toàn,
// các wrapper sẽ được sử dụng để bọc các hàm đó.
fn cos(z: Complex) -> Complex {
    unsafe { ccosf(z) }
}

fn main() {
    // z = -1 + 0i
    let z = Complex { re: -1., im: 0. };

    // gọi một hàm ngoại lai là hành động không an toàn
    let z_sqrt = unsafe { csqrtf(z) };

    println!("the square root of {:?} is {:?}", z, z_sqrt);

    // gọi một API an toàn bọc xung quanh một hành động không an toàn
    println!("cos({:?}) = {:?}", z, cos(z));
}

// Một thiết lập tối thiểu của số phức
#[repr(C)]
#[derive(Clone, Copy)]
struct Complex {
    re: f32,
    im: f32,
}

impl fmt::Debug for Complex {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        if self.im < 0. {
            write!(f, "{}-{}i", self.re, -self.im)
        } else {
            write!(f, "{}+{}i", self.re, self.im)
        }
    }
}
```