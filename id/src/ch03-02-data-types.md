## Tipe-tipe Data

Setiap nilai dalam Rust memiliki *tipe data* tertentu, yang memberitahu Rust
data jenis apa yang sedang dispesifikasikan sehingga ia tahu bagaimana
bekerja dengan data itu. Kita akan melihat dua sub set tipe data: skalar dan
kompon.

Ingatlah bahwa Rust adalah bahasa *bertipe statik*, yang berarti bahwa itu
harus tahu tipe dari semua variabel saat compile. Compiler biasanya bisa
menebak tipe apa yang ingin kita pakai berdasarkan pada nilai dan bagaimana
kita memakainya. Dalam kasus-kasus dimana banyak tipe yang mungkin, seperti
ketika kita mengonversi suatu `String` ke sebuah tipe numerik memakai `parse`
dalam bagian [“Membandingkan Tebakan ke Angka
Rahasia”][comparing-the-guess-to-the-secret-number]<!-- ignore --> dalam Bab
2, kita harus menambahkan sebuah anotasi tipe, seperti ini:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

Bila kita tidak menambahkan anotasi tipe `: u32` yang ditunjukkan dalam kode
sebelumnya, Rust akan menampilkan kesalahan berikut, yang berarti compiler
perlu lebih banyak informasi dari kita untuk mengetahui tipe mana yang ingin
kita pakai:

```console
{{#include ../listings/ch03-common-programming-concepts/output-only-01-no-type-annotations/output.txt}}
```

Anda akan melihat anotasi-anotasi tipe lain untuk tipe data lain.

### Tipe-tipe Skalar

Suatu tipe *skalar* mewakili sebuah nilai tunggal. Rust memiliki empat tipe
skalar primer: integer, bilangan floating-point, Boolean, dan karakter. Anda
bisa mengenali ini dari bahasa-bahasa pemrograman lain. Mari kita melompat
ke bagaimana mereka bekerja dalam Rust.

#### Tipe-tipe Integer

Suatu *integer* adalah sebuah bilangan tanpa komponen pecahan. Kita memakai
satu tipe integer dalam Bab 2, tipe `u32`. Deklarasi tipe ini mengindikasikan
bahwa nilai yang berasosiasi dengannya mesti berupa suatu bilangan bulat
tak bertanda (unsigned integer) (tipe-tipe integer bertanda dimulai dengan
`i` bukan `u`) yang mengambil tempat 32 bit. Tabel 3-1 menampilkan tipe-tipe
integer bawaan dalam Rust. Kita dapat memakai sebarang dari varian-varian ini
untuk mendeklarasikan tipe dari sebuah nilai bilangan bulat.

<span class="caption">Tabel 3-1: Tipe-tipe Integer dalam Rust</span>

| Panjang | Bertanda | Tak Bertanda |
|---------|----------|--------------|
| 8-bit   | `i8`     | `u8`         |
| 16-bit  | `i16`    | `u16`        |
| 32-bit  | `i32`    | `u32`        |
| 64-bit  | `i64`    | `u64`        |
| 128-bit | `i128`   | `u128`       |
| arch    | `isize`  | `usize`      |

Setiap varian bisa berupa bertanda atau tidak bertanda dan memiliki suatu 
ukuran eksplisit. *Bertanda* dan *tidak bertanda* mengacu ke apakah mungkin
suatu bilangan bernilai negatif-dengan kata lain apakah bilangan perlu
punya tanda dengannya (bertanda) atau apakah itu akan selalu positif dan
sehingga bisa direpresentasikan tanpa suatu tanda (tak bertanda). Itu 
seperti menulis bilangan di kertas: ketika tanda berarti, suatu bilangan
ditunjukkan dengan tanda plus atau tanda minus; namun, ketika aman untuk
mengasumsikan bahwa bilangannya positif, itu ditunjukkan tanpa tanda.
Bilangan bertanda disimpan memakai representasi [komplemen 
dua][twos-complement]<!-- ignore -->.

Setiap varian bertanda bisa menyimpan bilangan dari -(2<sup>n - 1</sup>)
sampai dengan 2<sup>n - 1</sup> - 1, dimana *n* adalah banyanyak bit yang
dipakai oleh varian itu. Sehingga suatu `i8` dapat menyimpan bilangan dari
-(2<sup>7</sup>) sampai 2<sup>7</sup> - 1, yang sama dengan -128 sampai 127.

Varian-varian tak bertanda dapat menyimpan bilangan dari 0 sampai 2<sup>n</sup> 
- 1, sehingga sebuah `u8` dapat menyimpan bilangan-bilangan dari 0 sampai
2<sup>8</sup> - 1, yang sama dengan 0 sampai 255.

Sebagai tambahan, tipe-tipe `isize` dan `usize` bergantung pada arsitektur
dari komputer tempat program Anda berjalan, yang dicantumkan dalam tabel
sebagai "arch": 64 bit bila Anda pada suatu arsitektur 64-bit dan 32 bit
bila Anda pada suatu arsitektur 32-bit.

Anda bisa menulis literal bilangan bulat dalam sebarang bentuk yang 
ditampilkan di Tabel 3-2. Perhatikan bahwa literal bilangan yang bisa berupa 
beberapa tipe numerik mengizinkan suatu akhiran tipe, seperti misalnya `57u8`,
untuk menyatakan tipe. Literal bilangan juga bisa memakai `_` sebagai pemisah
visual untuk membuat bilangan lebih mudah dibaca, seperti misalnya `1_000`,
yang akan memiliki nilai yang sama bila Anda menyatakannya `1000`.

<span class="caption">Tabel 3-2: Literal Bilangan Bulat dalam Rust</span>

| Literal bilangan  | Contoh        |
|-------------------|---------------|
| Desimal           | `98_222`      |
| Heksa             | `0xff`        |
| Oktal             | `0o77`        |
| Biner             | `0b1111_0000` |
| Byte (hanya `u8`) | `b'A'`        |

Jadi bagaimana Anda tahu tipe bilangan bulat mana yang akan dipakai? Bila
Anda tidak yakin, baku Rust umumnya adalah tempat yang baik untuk memulai:
tipe bilangan bukat baku adalah `i32`. Situasi primer dimana Anda akan
memakai `isize` atau `usize` adalah ketika mengindeks beberapa jenis koleksi.

> ##### Overflow Bilangan Bulat
>
> Katakanlah Anda punya sebuah variabel bertipe `u8` yang dapat menyimpan
> nilai antara 0 dan 255. Bila Anda mencoba mengubah variabel ke suatu nilai
> di luar rentang itu, seperti misalnya 256, *overflow bilangan bulat* akan
> terjadi, yang akan menyebabkan satu dari dua perilaku. Ketika Anda 
> meng-compile dalam mode debug, Rust menyertakan pemeriksaan-pemeriksaan
> bagi overflow bilangan bulat yang menyebabkan program Anda *panik* saat
> runtime bila perilaku ini terjadi. Rust memakai istilah *panicking* ketika
> sebuah program keluar dengan sebuah kesalahan; kita akan mendiskusikan
> panik secara lebih mendalam di bagian [“Unrecoverable Errors with
> `panic!`”][unrecoverable-errors-with-panic]<!-- ignore --> di Bab 9.
>
> Saat Anda meng-compile dalam mode rilis dengan flag `--release`, Rust
> *tidak* menyertakan pemeriksaan-pemeriksaan overflow bilangan bulat yang
> menyebabkan panik. Sebagai pengganti, bila terjadi overvlow, Rust melakukan
> *pelipatan komplemen dua*. Secara singkat, nilai yang lebih dari nilai
> maksimum yang dapat ditampung oleh tipe "melipat" ke nilai minimum yang
> dapat ditampung oleh oleh tipe tersebut. Dalam kasus suatu `u8`, nilai 256
> menjadi 0, nilai 257 menjadi 1, dan seterusnya. Program tidak akan panik,
> tapi variabel tersebut akan memiliki suatu nilai yang mungkin bukan yang
> Anda harapkan dipunyainya. Mengandalkan ke perilaku pelipatan overflow
> bilangan bulat dianggap sebagai suatu kesalahan.
>
> Untuk secara eksplisit menangani kemungkinan overflow, Anda dapat memakai
> metode-metode keluarga ini yang disediakan oleh pustaka standar bagi
> tipe-tipe bilangan primitif:
> 
> * Bungkus semua mode dengan metode `wrapping_*`, seperti misalnya 
>   `wrapping_add`.
> * Kembalikan nilai `None` bila ada suatu overflow dengan metode-metode
>   `checked_*`.
> * Kembalikan nilai tersebut dan sebuah boolean yang mengindikasikan
>   apakah ada overflow dengan metode-metode `overflowing_*`.
> * Saturasikan pada nilai minimum atau maksimum bagi nilai tersebut dengan
>   metode-metode `saturating_*`.

#### Tipe-tipe Floating-Point

Rust juga punya dua tipe primitif bagi *bilangan floating-point*, yaitu
bilangan dengan koma desimal. Tipe-tipe floating point Rust adalah `f32` dan
`f64`, yang masing-masing berukuran 32 bit dan 64 bit. Tipe baku adalah `f64`
karena pada CPU modern, itu secara kasar punya kecepatan sama dengan `f32`
tapi mampu berpresisi lebih. Semua tipe floating point itu bertanda.

Ini adalah contoh yang menunjukkan bilangan floating point saat beraksi:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-06-floating-point/src/main.rs}}
```

Bilangan-bilangan floating point direpresentasikan menurut standar IEEE-754.
Tipe `f32` adalah float presisi-tunggal, dan `f64` adalah presisi ganda.

#### Operasi-operasi Numerik

Rust mendukung operasi-operasi matematis dasar yang Anda harapkan untuk semua
tipe bilangan: penambahan, pengurangan, perkalian, pembagian, dan sisa bagi.
Pembagian bilangan bulat memotong menuju nol ke bilangan bulat terdekat. Kode
berikut menunjukkan bagaimana Anda menggunakan setiap operasi numerik dalam suatu
pernyataan `let`.

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-07-numeric-operations/src/main.rs}}
```

Setiap ekspressi dalam pernyataan-pernyataan ini memakai suatu operator 
matematis dan mengevaluasi ke suatu nilai tunggal, yang kemudian diikat
ke sebuah variabel.  [Lampiran B][appendix_b]<!-- ignore --> memuat suatu
daftar dari semua operator yang disediakan oleh Rust.

#### Tipe Boolean

Seperti dalam kebanyakan bahasa pemrograman lain, suatu tipe Boolean dalam
Rust memiliki dua kemungkinan nilai: `true` dan `false`. Boolean berukuran
satu byte. Tipe Boolean dalam Rust dinyatakan memakai `bool`. Sebagai contoh:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-08-boolean/src/main.rs}}
```

Cara utama untuk memakai nilai-nilai Boolean adalah melalui kondisional, 
seperti misalnya suatu ekspresi `if`. Kita akan membahas bagaimana ekspresi
`if` bekerja di Rust dalam bagian [“Alur Kendali”][control-flow]<!-- ignore -->.

#### Tipe Karakter

Tipe `char` Rust adalah tipe alfabetik paling primitif dari bahasa ini. Di
sini adalah beberapa contoh mendeklarasikan nilai-nilai `char`:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-09-char/src/main.rs}}
```

Perhatikan bahwa kita menyatakan literal `char` dengan kutip tunggal, berbeda
dengan literal string, yang memakai kutip ganda. Tipe `char` Rust berukuran
empat byte dan merepresentasikan suatu Nilai Skalar Unicode, yang berarti itu
dapat merepresentasikan jauh lebih banyak daripada sekadar ASCII. Huruf-huruf
beraksen; karakter-karakter Cina, Jepang, dan Korea; emoji; dan spasi lebar
nol semua adalah nilai-nilai `char` yang valid dalam Rust. Nilai Skalar
Unicode merentang dari `U+0000` sampai `U+D7FF` dan `U+E000` sampai `U+10FFFF`
inklusif. Namun, suatu "karakter" bukan benar-benar suatu konsep dalam Unicode,
sehingga intuisi manusia Anda tentang apa suatu "karakter" itu mungkin tidak
cocok dengan apa suatu `char` dalam Rust. Kita akan mendiskusikan topik ini 
secara rinci di [“Menuimpan Teks Terkode UTF-8 dengan String”][strings]<!-- ignore -->
dalam Bab 8.

### Tipe Kompon

*Compound types* can group multiple values into one type. Rust has two
primitive compound types: tuples and arrays.

#### Tipe Tuple

A *tuple* is a general way of grouping together a number of values with a
variety of types into one compound type. Tuples have a fixed length: once
declared, they cannot grow or shrink in size.

We create a tuple by writing a comma-separated list of values inside
parentheses. Each position in the tuple has a type, and the types of the
different values in the tuple don’t have to be the same. We’ve added optional
type annotations in this example:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-10-tuples/src/main.rs}}
```

The variable `tup` binds to the entire tuple because a tuple is considered a
single compound element. To get the individual values out of a tuple, we can
use pattern matching to destructure a tuple value, like this:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-11-destructuring-tuples/src/main.rs}}
```

This program first creates a tuple and binds it to the variable `tup`. It then
uses a pattern with `let` to take `tup` and turn it into three separate
variables, `x`, `y`, and `z`. This is called *destructuring* because it breaks
the single tuple into three parts. Finally, the program prints the value of
`y`, which is `6.4`.

We can also access a tuple element directly by using a period (`.`) followed by
the index of the value we want to access. For example:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-12-tuple-indexing/src/main.rs}}
```

This program creates the tuple `x` and then accesses each element of the tuple
using their respective indices. As with most programming languages, the first
index in a tuple is 0.

The tuple without any values has a special name, *unit*. This value and its
corresponding type are both written `()` and represent an empty value or an
empty return type. Expressions implicitly return the unit value if they don’t
return any other value.

#### Tipe Larik

Another way to have a collection of multiple values is with an *array*. Unlike
a tuple, every element of an array must have the same type. Unlike arrays in
some other languages, arrays in Rust have a fixed length.

We write the values in an array as a comma-separated list inside square
brackets:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-13-arrays/src/main.rs}}
```

Arrays are useful when you want your data allocated on the stack rather than
the heap (we will discuss the stack and the heap more in [Chapter
4][stack-and-heap]<!-- ignore -->) or when you want to ensure you always have a
fixed number of elements. An array isn’t as flexible as the vector type,
though. A *vector* is a similar collection type provided by the standard
library that *is* allowed to grow or shrink in size. If you’re unsure whether
to use an array or a vector, chances are you should use a vector. [Chapter
8][vectors]<!-- ignore --> discusses vectors in more detail.

However, arrays are more useful when you know the number of elements will not
need to change. For example, if you were using the names of the month in a
program, you would probably use an array rather than a vector because you know
it will always contain 12 elements:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

You write an array’s type using square brackets with the type of each element,
a semicolon, and then the number of elements in the array, like so:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Here, `i32` is the type of each element. After the semicolon, the number `5`
indicates the array contains five elements.

You can also initialize an array to contain the same value for each element by
specifying the initial value, followed by a semicolon, and then the length of
the array in square brackets, as shown here:

```rust
let a = [3; 5];
```

The array named `a` will contain `5` elements that will all be set to the value
`3` initially. This is the same as writing `let a = [3, 3, 3, 3, 3];` but in a
more concise way.

##### Mengakses Elemen-elemen Larik

An array is a single chunk of memory of a known, fixed size that can be
allocated on the stack. You can access elements of an array using indexing,
like this:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-14-array-indexing/src/main.rs}}
```

In this example, the variable named `first` will get the value `1` because that
is the value at index `[0]` in the array. The variable named `second` will get
the value `2` from index `[1]` in the array.

##### Akses Elemem Larik yang Tidak Valid

Let’s see what happens if you try to access an element of an array that is past
the end of the array. Say you run this code, similar to the guessing game in
Chapter 2, to get an array index from the user:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,panics
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access/src/main.rs}}
```

This code compiles successfully. If you run this code using `cargo run` and
enter `0`, `1`, `2`, `3`, or `4`, the program will print out the corresponding
value at that index in the array. If you instead enter a number past the end of
the array, such as `10`, you’ll see output like this:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access
cargo run
10
-->

```console
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

The program resulted in a *runtime* error at the point of using an invalid
value in the indexing operation. The program exited with an error message and
didn’t execute the final `println!` statement. When you attempt to access an
element using indexing, Rust will check that the index you’ve specified is less
than the array length. If the index is greater than or equal to the length,
Rust will panic. This check has to happen at runtime, especially in this case,
because the compiler can’t possibly know what value a user will enter when they
run the code later.

This is an example of Rust’s memory safety principles in action. In many
low-level languages, this kind of check is not done, and when you provide an
incorrect index, invalid memory can be accessed. Rust protects you against this
kind of error by immediately exiting instead of allowing the memory access and
continuing. Chapter 9 discusses more of Rust’s error handling and how you can
write readable, safe code that neither panics nor allows invalid memory access.

[comparing-the-guess-to-the-secret-number]:
ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[twos-complement]: https://en.wikipedia.org/wiki/Two%27s_complement
[control-flow]: ch03-05-control-flow.html#control-flow
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[stack-and-heap]: ch04-01-what-is-ownership.html#the-stack-and-the-heap
[vectors]: ch08-01-vectors.html
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[appendix_b]: appendix-02-operators.md
