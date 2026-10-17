Baik. Kita mulai resmi dari track SwiftUI.

Materi pertama ini sengaja sangat dasar. Saya ingin Anda memahami cara SwiftUI berpikir sebelum menyentuh `@State`, navigation, atau ViewModel. Roadmap utama memang menempatkan SwiftUI setelah fondasi Swift, tetapi kita jalankan paralel seperti yang sudah disepakati. Akses Link Percakapan

## UI 1. Fondasi SwiftUI

SwiftUI adalah framework Apple untuk membuat antarmuka aplikasi secara deklaratif. Artinya, kita mendeskripsikan tampilan yang kita inginkan, lalu SwiftUI yang menentukan bagaimana tampilan tersebut dibuat dan diperbarui di layar. Materi SwiftUI yang sudah ada di project Anda juga menggunakan framing ini. Tutorial_SwiftUI_Chapter_2_samp…

Perhatikan kode paling dasar berikut:

```
import SwiftUI

struct ContentView: View {
    var body: some View {
        Text("Halo")
    }
}
```

Jangan hanya melihatnya sebagai kode yang harus dihafal. Kita bongkar cara kerjanya.

`import SwiftUI` berarti file ini ingin menggunakan tipe dan fitur dari framework SwiftUI, seperti `View`, `Text`, `Button`, `VStack`, `NavigationStack`, dan sebagainya.

Lalu:

```
struct ContentView: View
```

`ContentView` adalah tipe buatan kita sendiri.

Kata `struct` berasal dari Swift, bukan SwiftUI.

Bagian:

```
: View
```

berarti `ContentView` mengikuti protocol `View`.

Untuk sekarang cukup pahami bahwa SwiftUI mengatakan:

> Kalau Anda ingin membuat sesuatu yang dianggap sebagai tampilan SwiftUI, tipe tersebut harus memenuhi kontrak `View`.

Karena mengikuti `View`, `ContentView` harus menyediakan:

```
var body: some View
```

Materi project Anda juga menjelaskan bahwa sebuah `View` wajib memiliki `body`, dan isi `body` merupakan tampilan yang akan dirender. Tutorial_SwiftUI_Chapter_2_samp…

### Apa sebenarnya `body`?

`body` bukan function.

Perhatikan:

```
var body: some View
```

Ada `var`, bukan `func`.

Jadi `body` adalah property.

Lebih tepatnya, ini adalah computed property.

Di Swift biasa Anda nanti akan menemukan pola seperti:

```
var fullName: String {
    firstName + " " + lastName
}
```

`fullName` tidak menyimpan nilai sendiri. Nilainya dihitung ketika diakses.

Konsep yang sama berlaku pada:

```
var body: some View {
    Text("Halo")
}
```

Ketika SwiftUI membutuhkan tampilan `ContentView`, SwiftUI membaca `body`.

Hasilnya adalah:

```
Text("Halo")
```

Jadi secara mental:

```
ContentView
↓
SwiftUI meminta body
↓
body menghasilkan View
↓
Text("Halo")
↓
dirender ke layar
```

## Apa arti `some View`?

Ini sering membingungkan pemula.

```
var body: some View
```

Untuk tahap sekarang, jangan masuk terlalu dalam ke opaque return types.

Cukup baca seperti ini:

```
body akan menghasilkan suatu tipe
yang memenuhi View
```

Misalnya:

```
var body: some View {
    Text("Hello")
}
```

`Text` adalah sebuah `View`.

Atau:

```
var body: some View {
    Button("Masuk") {
        print("Ditekan")
    }
}
```

`Button` juga sebuah `View`.

Atau:

```
var body: some View {
    VStack {
        Text("Nama")
        Text("Dede")
    }
}
```

`VStack` juga sebuah `View`.

Maka `some View` bukan berarti `body` hanya boleh menghasilkan `Text`.

Ia berarti hasil `body` adalah suatu tipe yang memenuhi protocol `View`.

Nanti di track Swift kita akan membahas kenapa Apple tidak sekadar menulis:

```
var body: View
```

dan apa hubungan `some` dengan static type system Swift.

Untuk sekarang, belum perlu.

## Semua yang terlihat biasanya adalah View

Ini konsep penting SwiftUI.

```
Text("Halo")
```

adalah View.

```
Image(systemName: "person")
```

adalah View.

```
Button("Simpan") {
}
```

adalah View.

Bahkan pengatur layout seperti:

```
VStack
HStack
ZStack
```

juga merupakan View.

Jadi SwiftUI pada dasarnya membangun pohon View.

Misalnya:

```
VStack {
    Text("Profil")
    Image(systemName: "person.circle")
    Button("Edit") {
    }
}
```

Secara konsep:

```
VStack
├── Text
├── Image
└── Button
```

Ini disebut view hierarchy.

Materi project Anda juga menjelaskan bahwa UI SwiftUI tersusun dari kumpulan `View`, mulai dari teks, gambar, tombol, daftar, form, dan struktur layout. Tutorial_SwiftUI_Chapter_2_samp…

## SwiftUI bersifat deklaratif

Sekarang bagian paling penting.

Bayangkan kita ingin menampilkan:

```
Halo Dede
```

Di SwiftUI:

```
Text("Halo Dede")
```

Kita mengatakan:

> Saya ingin ada Text berisi "Halo Dede".

Kita tidak mengatakan:

```
buat object label
atur frame
pasang label ke parent
set teksnya
refresh layar
```

Itulah salah satu makna pendekatan deklaratif.

Kita mendeskripsikan keadaan UI yang seharusnya tampil.

Nanti konsep ini menjadi jauh lebih jelas saat belajar `@State`.

Misalnya:

```
@State private var isLoggedIn = false
```

Lalu:

```
if isLoggedIn {
    Text("Selamat datang")
} else {
    Text("Silakan login")
}
```

Kita tidak secara manual mengatakan:

```
hapus Text login
buat Text baru
tambahkan ke layar
```

Kita hanya mengatakan:

```
kalau state seperti ini
UI harus berbentuk seperti ini
```

Ketika state berubah, SwiftUI menghitung ulang bagian tampilan yang relevan.

Itu nanti menjadi inti SwiftUI.

## Jangan salah memahami `ContentView`

Nama:

```
ContentView
```

bukan keyword khusus.

Anda boleh membuat:

```
struct LoginView: View {
    var body: some View {
        Text("Login")
    }
}
```

atau:

```
struct HomeView: View {
    var body: some View {
        Text("Home")
    }
}
```

atau:

```
struct AttendanceView: View {
    var body: some View {
        Text("Presensi")
    }
}
```

`ContentView` hanya nama yang lazim dibuat oleh template Xcode.

Yang penting adalah:

```
: View
```

dan adanya:

```
var body: some View
```

## Lalu siapa yang membuka `ContentView`?

Biasanya ada file aplikasi seperti:

```
import SwiftUI

@main
struct BelajarSwiftUIApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

Materi Anda juga menjelaskan struktur `App`, `@main`, `Scene`, dan `WindowGroup` sebagai titik awal aplikasi SwiftUI. Tutorial_SwiftUI_Chapter_2_samp…

Alurnya secara sederhana:

```
Aplikasi dijalankan
↓
@main
↓
BelajarSwiftUIApp
↓
body
↓
WindowGroup
↓
ContentView()
↓
ContentView.body
↓
UI tampil
```

Perhatikan ada dua `body`:

```
struct BelajarSwiftUIApp: App {
    var body: some Scene {
```

dan:

```
struct ContentView: View {
    var body: some View {
```

Yang pertama menghasilkan `Scene`.

Yang kedua menghasilkan `View`.

Jangan dicampur.

## Satu kebiasaan membaca kode yang ingin saya bangun

Ketika melihat:

```
struct ProfileView: View {
    var body: some View {
        Text("Dede")
    }
}
```

saya ingin otak Anda mulai membaca:

```
ProfileView adalah struct.

ProfileView mengikuti View.

Karena itu ia menyediakan body.

body menghasilkan sebuah View.

View yang dihasilkan sekarang adalah Text.
```

Bukan sekadar:

> Ini syntax SwiftUI.

Perbedaan cara membaca ini penting sekali kalau nanti Anda membuka file SwiftUI 300 sampai 500 baris.

---

## Latihan UI 1

Jangan cari jawabannya. Jawab berdasarkan pemahaman Anda. Tidak masalah jika ada yang salah.

1. Pada kode berikut, apa peran `ContentView`?

```
struct ContentView: View {
    var body: some View {
        Text("Halo")
    }
}
```

2. Apa arti `: View` pada:

```
struct ProfileView: View
```

3. `body` pada SwiftUI merupakan function atau property? Jelaskan alasan Anda berdasarkan syntax.
    
4. Dengan bahasa Anda sendiri, apa arti:
    

```
some View
```

5. Mana yang merupakan View dalam kode berikut?

```
VStack {
    Text("Dede")

    Button("Simpan") {
        print("save")
    }
}
```

6. Apakah nama `ContentView` wajib digunakan SwiftUI? Jelaskan.
    
7. Perhatikan:
    

```
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            HomeView()
        }
    }
}
```

View pertama yang diminta untuk ditampilkan aplikasi adalah apa?

8. Jelaskan urutan aliran berikut menggunakan bahasa Anda sendiri:

```
@main
→ App
→ WindowGroup
→ ContentView
→ body
```

9. Menurut Anda, apa yang dimaksud pendekatan deklaratif pada SwiftUI?
    
10. Tantangan kecil. Tanpa menjalankan Xcode, jelaskan kira-kira struktur hierarki View dari kode ini:
    

```
struct HomeView: View {
    var body: some View {
        VStack {
            Text("NFBS Attendance")

            Button("Presensi") {
                print("Open attendance")
            }

            Button("Riwayat") {
                print("Open history")
            }
        }
    }
}
```

Jawab nomor 1 sampai 10. Tidak perlu dibuat terlalu formal. Saya lebih ingin melihat cara Anda memahami konsepnya daripada definisi buku. Setelah itu saya koreksi satu per satu, termasuk bagian yang benar tetapi alasannya belum tepat.

Jawaban:
1. ContentView adalah sebuah struct yang mengikuti view, karena itu ia menyediakan body. saya kurang faham arah pertanyaannya mohon dijelaskan
2. sebuah protocol yang berasal dari ProfileView. dan semua yang terlihat biasanya adalah view.saya kurang faham
3. body itu adalah property karena body menggunakan var bukan func
4. some View adalah tempat dimana suatu tipe yang dihasilkan oleh body
5. Text dan Button
6. tidak, ia hanya template yang disediakan oleh xcode, kita bisa membuat nama lain sesuai kebutuhan tampilan atau aplikasi
7. WindowGroup
8. aplikasi dibuka - masuk ke @main - ada body - menghasilkan WindowGroup - lanjut ke ContentView - terus body - UI tampil
9. kita mendeskripiskan langsung keadaan UI yang ingin ditampilkan dengan code
10. ada pohon view dengan isi secara hirarki dalam bentuk vstack, 1. Text, 2. Button, 3. Button

Tanggapan
Bagus. Dasar cara berpikirnya sudah mulai terbentuk. Yang perlu kita benahi terutama nomor 1, 2, 4, 5, dan 7. Nomor 3, 6, 8, 9, dan 10 sudah cukup kuat.

|No.|Penilaian|Koreksi|
|---|---|---|
|1|Hampir benar|`ContentView` adalah sebuah tipe `struct` yang merepresentasikan sebuah tampilan SwiftUI. Karena mengikuti protocol `View`, ia wajib menyediakan `body`.|
|2|Perlu diperbaiki|`View` bukan berasal dari `ProfileView`. Justru `View` adalah protocol milik SwiftUI, lalu `ProfileView` menyatakan bahwa dirinya mengikuti protocol tersebut.|
|3|Benar|`body` adalah property karena dideklarasikan dengan `var`, bukan `func`. Lebih tepatnya computed property.|
|4|Hampir benar|`some View` berarti property tersebut mengembalikan suatu tipe konkret yang memenuhi protocol `View`.|
|5|Kurang satu|`VStack`, `Text`, dan `Button` semuanya adalah View.|
|6|Benar|`ContentView` hanyalah nama yang diberikan template Xcode.|
|7|Salah|View pertama yang diminta `WindowGroup` untuk ditampilkan adalah `HomeView()`, bukan `WindowGroup`.|
|8|Benar|Alurnya sudah Anda pahami.|
|9|Benar|Ini inti dari declarative UI.|
|10|Benar|`VStack` menjadi parent, lalu memiliki tiga child yaitu `Text`, `Button`, dan `Button`.|

Sekarang saya jelaskan bagian yang masih kabur.

### Nomor 1. Apa sebenarnya peran `ContentView`?

Pertanyaan saya bukan sekadar menanyakan apakah dia `struct`.

Perhatikan:

```
struct ContentView: View {
    var body: some View {
        Text("Halo")
    }
}
```

Secara Swift:

```
ContentView
= sebuah tipe data buatan kita
```

Tetapi secara SwiftUI:

```
ContentView
= sebuah definisi tampilan
```

Misalnya Anda punya:

```
struct LoginView: View {
    var body: some View {
        Text("Login")
    }
}
```

`LoginView` merupakan tipe Swift yang mendeskripsikan layar login.

Begitu juga:

```
struct HomeView: View
```

berarti kita sedang mendefinisikan suatu jenis tampilan bernama `HomeView`.

Yang sering membingungkan pemula adalah menganggap `ContentView` seperti sebuah layar yang sudah ada di layar.

Sebenarnya belum.

Ini:

```
struct ContentView: View
```

baru mendefinisikan tipenya.

Barulah ketika kita menulis:

```
ContentView()
```

kita membuat instance dari `ContentView`.

Mirip fondasi Swift yang nanti Anda pelajari lebih dalam:

```
struct Student {
    let name: String
}
```

Ini definisi tipe.

Sedangkan:

```
Student(name: "Dede")
```

adalah instance.

Pada SwiftUI:

```
struct ContentView: View {
    ...
}
```

definisi tipe.

Sedangkan:

```
ContentView()
```

instance View yang ingin digunakan.

Ini sangat penting.

---

### Nomor 2. Arah hubungan `ProfileView: View`

Jawaban Anda:

> sebuah protocol yang berasal dari ProfileView

Arahnya terbalik.

Yang benar:

```
SwiftUI
↓
menyediakan protocol View
↓
ProfileView memilih mengikuti View
```

Jadi:

```
struct ProfileView: View
```

dapat kita baca:

> Buat sebuah struct bernama `ProfileView` yang mengikuti kontrak `View`.

Bukan:

> View berasal dari ProfileView.

Analogi dari Swift nanti:

```
struct Dog: Animal
```

Kalau `Animal` adalah protocol, artinya:

```
Dog mengikuti Animal
```

bukan:

```
Animal berasal dari Dog
```

Pada SwiftUI:

```
ProfileView : View
```

artinya:

```
ProfileView
    ↓ conforms to
View
```

Istilah developer yang akan sering Anda dengar adalah:

```
ProfileView conforms to View
```

atau:

```
ProfileView mengadopsi protocol View
```

---

### Nomor 4. `some View`

Jawaban Anda sudah mengarah benar:

> some View adalah tempat dimana suatu tipe yang dihasilkan oleh body

Kita rapikan.

`some View` bukan tempat.

Ini adalah tipe return dari `body`.

Contoh:

```
var body: some View {
    Text("Halo")
}
```

Bacanya:

> `body` menghasilkan suatu tipe konkret yang memenuhi protocol `View`.

Di sini tipe konkretnya sebenarnya:

```
Text
```

Contoh lain:

```
var body: some View {
    VStack {
        Text("A")
        Text("B")
    }
}
```

Tipe konkretnya sekarang bukan `Text`, tetapi suatu `VStack<...>`.

Namun kita tidak perlu menulis tipe SwiftUI lengkap yang bisa sangat panjang.

Kita cukup mengatakan:

```
some View
```

Untuk saat ini, mental model Anda cukup:

```
some View

=
"saya mengembalikan suatu jenis View"
```

Nanti ketika fondasi Swift Anda sudah sampai protocol dan generic, kita bedah kenapa ada kata `some`.

---

### Nomor 5. Ini jebakan pertama SwiftUI

Kode:

```
VStack {
    Text("Dede")

    Button("Simpan") {
        print("save")
    }
}
```

Anda menjawab:

> Text dan Button

Benar, tetapi belum lengkap.

`VStack` sendiri juga sebuah `View`.

Hierarkinya:

```
VStack     ← View
│
├── Text   ← View
│
└── Button ← View
```

Ini konsep penting karena container layout SwiftUI juga merupakan View.

Begitu juga nanti:

```
HStack
ZStack
List
Form
NavigationStack
ScrollView
```

semuanya merupakan View.

Jadi jangan membayangkan View hanya sebagai sesuatu seperti tombol atau teks.

Dalam SwiftUI, container pun View.

---

### Nomor 7. Bedakan Scene dengan View

Ini bagian yang sangat penting.

Kode:

```
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            HomeView()
        }
    }
}
```

Anda menjawab:

> WindowGroup

Padahal pertanyaannya adalah:

> View pertama yang diminta untuk ditampilkan apa?

Jawabannya:

```
HomeView()
```

Mengapa?

Karena:

```
WindowGroup
```

bukan `View` utama layar tersebut.

Ia adalah bagian dari sistem `Scene`.

Perhatikan:

```
var body: some Scene
```

Bukan:

```
var body: some View
```

Strukturnya:

```
MyApp
│
└── Scene
    │
    └── WindowGroup
        │
        └── HomeView
```

Lalu:

```
HomeView
↓
body
↓
View hierarchy
```

Maka dua dunia ini harus mulai Anda pisahkan:

```
APP LEVEL

App
↓
Scene
↓
WindowGroup
```

dan:

```
UI LEVEL

View
↓
body
↓
Text / Button / VStack / dst.
```

Ini akan sering muncul nanti.

---

Sekarang saya ingin memastikan lima koreksi tadi benar-benar masuk sebelum kita masuk UI 2.

Jawab tiga soal penguatan ini saja.

A.

```
struct SettingsView: View {
    var body: some View {
        Text("Pengaturan")
    }
}
```

Jelaskan hubungan antara `SettingsView`, `View`, `body`, dan `Text`.

B.

Perhatikan:

```
@main
struct NFBSApp: App {
    var body: some Scene {
        WindowGroup {
            LoginView()
        }
    }
}
```

Mana yang merupakan `App`, mana yang merupakan `Scene`, dan mana yang merupakan `View`?

C.

Perhatikan:

```
var body: some View {
    HStack {
        Image(systemName: "person")
        Text("Dede")
    }
}
```

Sebutkan seluruh View yang terdapat dalam hierarchy tersebut, lalu tentukan mana parent dan mana child.

Kalau tiga ini sudah tepat, kita langsung naik ke UI 2: `Text`, `Image`, `Button`, dan modifier. Di sana Anda akan mulai benar-benar menyusun UI SwiftUI, bukan sekadar membaca strukturnya.

Jawaban:
A. SettingView merupakan tipe Swift yang mendeskripsikan layar Settings. sebuah struct dengan nama SettingView yang mengikuti kontrak view. Body menghasilkan suatu tipe konkret yang memenuhi protocol View, disini tipe konkretnya adalah Text
B. yang merupakan App adalah NFBSApp, WindowGroup adalah bagian dari Scene, dan untuk View nya adalah yang LoginView
C. HStack, Image dan Text. Hirarki nya adalah Hstack - Image - Text

Tanggapan
Ketiganya sudah tepat. Ada satu penyempurnaan kecil pada C: hierarkinya bukan `HStack → Image → Text` seolah `Text` anak dari `Image`. Yang benar, `HStack` adalah parent, sedangkan `Image` dan `Text` adalah dua child yang sejajar.

```
HStack
├── Image
└── Text
```

Dengan ini, fondasi UI 1 sudah cukup kuat. Kita naik ke UI 2.