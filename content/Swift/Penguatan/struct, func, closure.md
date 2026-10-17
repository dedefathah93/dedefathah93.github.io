Dalam Swift, `struct`, `func` (function), dan `closure` memang sering terlihat bersama. Namun ketiganya memiliki tujuan yang berbeda.

Hal paling penting yang perlu Anda pahami adalah:

- `struct` digunakan untuk mendefinisikan suatu tipe yang dapat memiliki data dan perilaku.
    
- `func` digunakan untuk mendefinisikan pekerjaan atau tindakan yang bisa dijalankan.
    
- `closure` juga berisi pekerjaan atau tindakan, tetapi dapat disimpan dalam variabel, diberikan sebagai parameter, dan dijalankan kemudian.
    

Kita gunakan contoh aplikasi presensi siswa agar lebih mudah dipahami.

## 1. Function: melakukan suatu pekerjaan

Misalkan Anda ingin membuat fungsi untuk menentukan status siswa.

```
func cekKehadiran(hadir: Bool) {
    if hadir {
        print("Siswa hadir")
    } else {
        print("Siswa tidak hadir")
    }
}

cekKehadiran(hadir: true)
```

Hasil:

```
Siswa hadir
```

Di sini:

`func` adalah kata kunci untuk mendeklarasikan fungsi.

`cekKehadiran` adalah nama fungsi.

`hadir: Bool` adalah parameter yang menerima data.

Kode di dalam `{ }` adalah pekerjaan yang dijalankan.

Function tersebut tidak perlu memiliki objek siswa sendiri. Ia cukup menerima informasi, melakukan pekerjaan, dan jika diperlukan mengembalikan hasil.

## 2. Struct: mengelompokkan data dan perilaku

Sekarang bayangkan aplikasi Anda mempunyai 100 siswa.

Masing-masing mempunyai nama, kelas, dan status kehadiran.

Daripada menyimpan semuanya dalam variabel yang terpisah, kita membuat `struct`.

```
struct Siswa {
    let nama: String
    let kelas: String
    var hadir: Bool
}
```

Sekarang kita bisa membuat beberapa siswa:

```
let siswa1 = Siswa(
    nama: "Ahmad",
    kelas: "10.1",
    hadir: true
)

let siswa2 = Siswa(
    nama: "Budi",
    kelas: "10.2",
    hadir: false
)
```

Perhatikan bahwa `Siswa` bukan seorang siswa tertentu.

`Siswa` merupakan definisi tipe data. Sedangkan `siswa1` dan `siswa2` merupakan dua nilai berbeda yang dibuat menggunakan tipe tersebut.

struct Siswa

Definisi tipe: nama, kelas, hadir

siswa1

Ahmad

10.1

Hadir

siswa2

Budi

10.2

Tidak hadir

Jadi, `struct` sangat cocok untuk mendefinisikan data yang mempunyai beberapa informasi saling berkaitan.

Contoh lain dalam aplikasi iOS adalah `User`, `ChatMessage`, `AttendanceRecord`, dan `LoginResponse`.

## 3. Mengapa di dalam struct bisa ada function?

Karena sebuah `struct` tidak terbatas untuk menyimpan data. Ia juga dapat memiliki perilaku yang berkaitan dengan data tersebut.

Perhatikan:

```
struct Siswa {
    let nama: String
    var hadir: Bool

    func tampilkanStatus() {
        print("\(nama): \(hadir)")
    }

    mutating func tandaiHadir() {
        hadir = true
    }
}
```

Sekarang penggunaannya:

```
var siswa = Siswa(
    nama: "Ahmad",
    hadir: false
)

siswa.tampilkanStatus()

siswa.tandaiHadir()

siswa.tampilkanStatus()
```

Hasil:

```
Ahmad: false
Ahmad: true
```

Di sini terdapat dua hal berbeda.

`nama` dan `hadir` adalah properties, yaitu data milik `Siswa`.

Sedangkan `tampilkanStatus()` dan `tandaiHadir()` adalah methods, yaitu function yang didefinisikan di dalam sebuah tipe.

Lalu mengapa ada `mutating`?

Karena `struct` adalah value type. Jika sebuah method mengubah properti tersimpan milik `struct`, kita harus menandai method tersebut menggunakan `mutating`. Instance yang diubah juga harus memungkinkan perubahan, misalnya dideklarasikan dengan `var`.

Poin pentingnya: function yang berada di dalam struct biasanya disebut method. Ia tetap function, hanya saja menjadi bagian dari tipe tersebut.

## 4. Lalu apa itu closure yang bisa dimasukkan ke dalam struct?

Ini bagian yang sering membingungkan ketika mulai belajar SwiftUI.

Function biasa ditulis menggunakan `func`.

```
func sapa() {
    print("Halo!")
}
```

Sedangkan closure dapat ditulis sebagai ekspresi:

```
let sapa = {
    print("Halo!")
}
```

Keduanya dapat dipanggil:

```
sapa()
```

Perbedaannya, closure merupakan nilai yang dapat disimpan dan diteruskan seperti nilai lainnya. Function bernama dalam Swift juga dapat digunakan sebagai nilai function.

Sekarang kita buat sebuah `struct` yang menyimpan closure.

```
struct TombolAksi {
    let judul: String

    let ketikaDitekan: () -> Void

    func tekan() {
        ketikaDitekan()
    }
}
```

Perhatikan properti:

```
let ketikaDitekan: () -> Void
```

Artinya, `ketikaDitekan` menyimpan sebuah closure yang tidak menerima parameter `()` dan tidak mengembalikan nilai `Void`.

Sekarang buat nilainya:

```
let tombol = TombolAksi(
    judul: "Simpan",
    ketikaDitekan: {
        print("Presensi disimpan!")
    }
)
```

Pada tahap ini kita sudah memberikan pekerjaan yang akan dilakukan, tetapi belum menjalankannya.

Ketika:

```
tombol.tekan()
```

barulah closure dijalankan.

Hasil:

```
Presensi disimpan!
```

struct TombolAksi

Menyimpan judul dan closure ketikaDitekan

tombol.tekan()

Method memanggil closure

Presensi disimpan!

Ini sangat berguna karena `TombolAksi` tidak harus mengetahui detail pekerjaan yang diberikan kepadanya. Kita bisa membuat tombol lain menggunakan tipe yang sama, tetapi closure-nya berbeda.

## 5. Hubungannya dengan SwiftUI

Sekarang Anda akan lebih mudah memahami kode yang sangat sering muncul di Xcode:

```
struct ContentView: View {

    var body: some View {
        Button("Simpan") {
            simpanPresensi()
        }
    }

    func simpanPresensi() {
        print("Menyimpan presensi...")
    }
}
```

Di sini terdapat ketiganya sekaligus.

|Kode|Peran|
|---|---|
|`struct ContentView: View`|Mendefinisikan tampilan SwiftUI|
|`var body: some View`|Computed property yang menghasilkan tampilan|
|`Button("Simpan")`|Membuat tombol SwiftUI|
|`{ simpanPresensi() }`|Closure untuk aksi tombol|
|`func simpanPresensi()`|Method yang menjalankan pekerjaan|

Ketika SwiftUI membangun tampilan, closure tombol diberikan sebagai aksi. Method `simpanPresensi()` baru dipanggil ketika pengguna menekan tombol.

Perhatikan juga bahwa `ContentView` merupakan `struct`, dan `Button` sendiri merupakan tipe `struct` dari SwiftUI.

Jadi tidak ada pertentangan antara `struct`, function, dan closure. Ketiganya justru digunakan bersama untuk membentuk aplikasi.

## 6. Kapan sebaiknya memilih masing-masing?

|Kondisi|Gunakan|
|---|---|
|Membuat tipe data siswa, user, atau pesan|`struct`|
|Menghitung total kehadiran|`func`|
|Membuat tindakan yang berkaitan langsung dengan data siswa|Method di dalam `struct`|
|Memberikan aksi ketika tombol ditekan|Closure|
|Mengirim pekerjaan sebagai parameter ke function lain|Closure atau referensi function|
|Membuat tampilan SwiftUI|`struct` yang mengikuti `View`|

Namun jangan menganggap function harus selalu berada di luar struct. Jika pekerjaannya berhubungan erat dengan tipe yang Anda buat, method di dalam struct sering lebih masuk akal.

## 7. Latihan untuk menguji pemahaman

Coba perhatikan kode ini:

```
struct Kalkulator {
    let angka1: Int
    let angka2: Int

    func jumlahkan() -> Int {
        return angka1 + angka2
    }
}

let hitung = Kalkulator(
    angka1: 10,
    angka2: 20
)

print(hitung.jumlahkan())
```

1. Apa fungsi `struct Kalkulator`?

Menjalankan perhitungan secara otomatis

Mendefinisikan tipe yang menyimpan angka dan memiliki method

Mencetak hasil ke layar

2. Mengapa `jumlahkan()` berada di dalam struct?

Karena setiap function wajib berada dalam struct

Karena pekerjaannya berkaitan dengan data angka1 dan angka2

Karena Swift tidak mengizinkan function di luar struct

3. Berapa hasil `print(hitung.jumlahkan())`?

10

20

30

Kesimpulan yang sebaiknya Anda ingat: `struct` mendefinisikan tipe yang dapat memiliki data dan perilaku, `func` mendefinisikan pekerjaan, dan `closure` memungkinkan pekerjaan diperlakukan sebagai nilai yang dapat disimpan atau diteruskan.

Jika konsep ini sudah dipahami, kode SwiftUI seperti `Button { }`, `.onAppear { }`, `Task { }`, serta penggunaan callback dalam ViewModel akan jauh lebih mudah dibaca.