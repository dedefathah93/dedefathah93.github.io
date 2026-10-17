Ketiganya sudah tepat. Ada satu penyempurnaan kecil pada C: hierarkinya bukan `HStack → Image → Text` seolah `Text` anak dari `Image`. Yang benar, `HStack` adalah parent, sedangkan `Image` dan `Text` adalah dua child yang sejajar.

```
HStack
├── Image
└── Text
```

Dengan ini, fondasi UI 1 sudah cukup kuat. Kita naik ke UI 2.

UI 2. Text, Image, Button, dan Modifier

Sekarang kita mulai menyusun elemen UI yang benar-benar terlihat.

Bagian pertama: `Text`

Contoh:

```
Text("Halo")
```

`Text` adalah View yang menampilkan teks.

Karena `Text` adalah View, kita bisa mengubah tampilannya menggunakan modifier.

Contoh:

```
Text("Halo")
    .font(.title)
```

Di sini:

```
.font(.title)
```

adalah modifier.

Mental model sederhananya:

```
Text asli
↓
diberi modifier
↓
menghasilkan View baru
```

Ini penting. Modifier bukan "mengedit object Text yang sama" seperti pola imperative klasik. Di SwiftUI, modifier biasanya menghasilkan View baru dengan konfigurasi tambahan.

Contoh:

```
Text("NFBS Attendance")
    .font(.title)
    .bold()
```

Urutannya dibaca:

```
buat Text
↓
gunakan font title
↓
buat menjadi bold
```

Dan modifier bisa dirangkai.

Contoh:

```
Text("Selamat Datang")
    .font(.headline)
    .padding()
```

Sekarang masuk ke `Image`.

SwiftUI dapat menampilkan gambar dengan:

```
Image("logo")
```

Ini biasanya digunakan untuk image asset yang ada di project.

Sedangkan:

```
Image(systemName: "person")
```

menggunakan SF Symbols milik Apple.

Contoh:

```
Image(systemName: "person.circle")
```

Anda bisa memberinya modifier:

```
Image(systemName: "person.circle")
    .font(.largeTitle)
```

Atau:

```
Image(systemName: "checkmark.circle")
    .imageScale(.large)
```

Untuk sekarang jangan terlalu fokus pada variasi modifier Image. Yang penting adalah membedakan:

```
Image("logo")
```

dan:

```
Image(systemName: "person")
```

Yang pertama biasanya asset Anda sendiri.

Yang kedua menggunakan simbol sistem.

Sekarang bagian ketiga: `Button`.

Contoh paling sederhana:

```
Button("Simpan") {
    print("Tombol ditekan")
}
```

Struktur ini punya dua bagian.

```
"Simpan"
```

adalah label tombol.

Sedangkan:

```
{
    print("Tombol ditekan")
}
```

adalah action yang dijalankan ketika tombol ditekan.

Bagian dalam `{ }` ini menggunakan konsep Swift bernama closure.

Karena Anda mungkin belum masuk closure secara penuh di track Swift, cukup pahami dulu:

```
Button("Simpan") {
    // kode yang dijalankan saat ditekan
}
```

Artinya:

> tampilkan tombol bernama Simpan, dan ketika ditekan, jalankan kode di dalam closure.

Misalnya:

```
Button("Login") {
    print("Login")
}
```

Secara UI:

```
[ Login ]
```

Ketika ditekan:

```
print("Login")
```

dijalankan.

Button juga dapat menggunakan label yang lebih kompleks:

```
Button {
    print("Simpan")
} label: {
    Text("Simpan")
}
```

Bahkan:

```
Button {
    print("Profil")
} label: {
    Image(systemName: "person")
}
```

Atau gabungan:

```
Button {
    print("Profil")
} label: {
    HStack {
        Image(systemName: "person")
        Text("Profil")
    }
}
```

Di sinilah konsep View composition mulai terasa.

`Button` adalah View.

Label di dalam Button juga bisa merupakan View.

Dan label tersebut bahkan bisa memiliki child View lagi.

Strukturnya:

```
Button
└── HStack
    ├── Image
    └── Text
```

Sekarang modifier.

Contoh:

```
Text("Judul")
    .font(.title)
    .bold()
    .padding()
```

Setiap modifier diterapkan berurutan.

Secara konsep:

```
Text
↓
.font
↓
.bold
↓
.padding
```

Urutan modifier kadang bisa memengaruhi hasil.

Contoh sederhana:

```
Text("Halo")
    .padding()
    .background(.gray)
```

dibanding:

```
Text("Halo")
    .background(.gray)
    .padding()
```

Secara konsep hasilnya berbeda.

Pada versi pertama:

```
Text
↓
padding ditambahkan
↓
background membungkus hasil setelah padding
```

Pada versi kedua:

```
Text
↓
background ditempel pada Text
↓
padding ditambahkan di luar background
```

Jadi modifier bukan sekadar daftar dekorasi acak. Urutannya bisa penting.

Sekarang lihat contoh utuh:

```
struct ProfileView: View {
    var body: some View {
        VStack {
            Image(systemName: "person.circle")
                .font(.largeTitle)

            Text("Dede Fathah")
                .font(.title)
                .bold()

            Button("Edit Profil") {
                print("Edit profile")
            }
        }
    }
}
```

Coba baca secara hirarki:

```
ProfileView
↓
body
↓
VStack
├── Image
├── Text
└── Button
```

Lalu masing-masing View dapat memiliki modifier sendiri.

Misalnya:

```
Image
└── .font(.largeTitle)

Text
├── .font(.title)
└── .bold()
```

Satu hal penting lagi: modifier ditempel pada View tepat di atasnya.

Contoh:

```
Text("Dede")
    .font(.title)
```

modifier tersebut berlaku ke `Text`.

Sedangkan:

```
VStack {
    Text("Dede")
    Text("Guru")
}
.padding()
```

`padding()` berlaku pada `VStack` sebagai keseluruhan, bukan hanya Text kedua.

Itu akan sangat penting saat UI mulai kompleks.

Sekarang latihan UI 2.

1. Apa fungsi `Text` dalam SwiftUI?
    
2. Jelaskan perbedaan:
    

```
Image("logo")
```

dan:

```
Image(systemName: "person")
```

3. Pada kode:

```
Button("Simpan") {
    print("saved")
}
```

mana label tombol dan mana action?

4. Bagian:

```
{
    print("saved")
}
```

menggunakan konsep Swift apa?

5. Apakah `Button` termasuk View?
    
6. Pada kode berikut:
    

```
Text("Halo")
    .font(.title)
    .bold()
```

apa yang disebut modifier?

7. Menurut pemahaman Anda, kenapa urutan modifier bisa memengaruhi hasil?
    
8. Perhatikan:
    

```
VStack {
    Text("Nama")
    Text("Dede")
}
.padding()
```

`padding()` diterapkan ke `Text("Dede")` saja atau ke `VStack` secara keseluruhan?

9. Gambarkan hierarchy View dari:

```
Button {
    print("Profil")
} label: {
    HStack {
        Image(systemName: "person")
        Text("Profil")
    }
}
```

10. Tantangan kecil. Tulis kode SwiftUI yang menghasilkan struktur ini:

```
VStack
├── Image system bernama "checkmark.circle"
├── Text "Berhasil"
└── Button "Selesai"
```

Ketika tombol ditekan, cukup:

```
print("Done")
```

Jawab nomor 1 sampai 10. Setelah itu saya koreksi konsep dan syntax-nya, lalu kita masuk UI 3: `VStack`, `HStack`, `ZStack`, `Spacer`, dan `padding`.

Jawaban:
1. untuk menampilkan karakter huruf langsung pada layar, berbeda untuk print yang menampilkannya dalam console atau biasanya berupa action yang dijalankan
2. Image("Logo") berarti kita mempunyai asset sendiri bernama "logo". sedangkan yang kedua kita menggunakan simbol yang sudah disediakan sistem
3. labelnya "simpan" dan action adalah {print("saved")}
4. konsep Swift bernama closure
5. ya, karena kita juga bisa menggunakan modifier padanya
6. .font(.tiitle) dan .bold() disebut modifier karena menghasilkan View baru dari Text("Halo") dengan konfigurasi tambahan
7. karena modifier mempunya sistem perubahan berurutan, sehingga yang satu tingkat diatasnya itu yang akan dieksekusi terlebih dahulu
8. diterapkan kepada VStack
9. Button & HStack - Image & Text
10. VStack {  
    Image(systemName: "checkmark.circle")  
    Text("Berhasil")  
    Button ("Selesai"){  
    print("Done")  
    }  
    }

Tanggapan
Jawaban Anda kali ini jauh lebih kuat. Hampir semua konsep sudah tepat. Yang perlu saya rapikan terutama nomor 7 dan 9, plus sedikit syntax pada nomor 10.

Koreksi UI 2

|No.|Hasil|Catatan|
|---|---|---|
|1|Benar|`Text` menampilkan teks pada UI, sedangkan `print()` menulis ke console untuk debugging/logging.|
|2|Benar|`Image("logo")` memakai asset milik aplikasi, sedangkan `Image(systemName:)` memakai SF Symbols.|
|3|Benar|`"Simpan"` adalah label, closure berisi `print("saved")` adalah action.|
|4|Benar|Itu adalah closure.|
|5|Benar|`Button` adalah `View`. Alasan Anda juga bagus karena `Button` dapat diberi modifier, meskipun secara formal alasannya karena `Button` memang conform ke `View`.|
|6|Benar|`.font(.title)` dan `.bold()` adalah modifier. Ada typo kecil `tiitle`, tetapi konsepnya tepat.|
|7|Hampir benar|Modifier bekerja berantai dari View sebelumnya. Jadi setiap modifier menerima hasil dari tahap sebelumnya lalu menghasilkan View baru.|
|8|Benar|`.padding()` diterapkan ke `VStack` secara keseluruhan.|
|9|Perlu dirapikan|`Button` adalah parent, `HStack` adalah child dari Button, lalu `Image` dan `Text` adalah child dari HStack.|
|10|Benar|Struktur dan syntax SwiftUI-nya sudah tepat.|

Nomor 7 saya ingin perjelas karena nanti sangat penting.

Misalnya:

```
Text("Halo")
    .padding()
    .background(.gray)
```

Bacanya:

```
Text
↓
hasilnya diberi padding
↓
hasil setelah padding diberi background
```

Jadi bukan sekadar "baris atas dijalankan dulu", tetapi lebih tepat:

```
modifier berikutnya bekerja pada hasil modifier sebelumnya
```

Itulah sebabnya ini:

```
Text("Halo")
    .padding()
    .background(.gray)
```

bisa berbeda dengan:

```
Text("Halo")
    .background(.gray)
    .padding()
```

Untuk nomor 9, hierarki yang benar:

```
Button
└── HStack
    ├── Image
    └── Text
```

Jadi jangan dibaca:

```
Button dan HStack
Image dan Text
```

seolah semuanya berada pada level yang sama.

Nomor 10 kalau kita bersihkan dari formatting chat, kodenya:

```
VStack {
    Image(systemName: "checkmark.circle")

    Text("Berhasil")

    Button("Selesai") {
        print("Done")
    }
}
```

Ini sudah benar.

Kita lanjut.