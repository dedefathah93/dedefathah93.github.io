## Hari 1. Apa sebenarnya `struct`?

Lihat kode ini lagi:

```
struct Student {
    let name: String
    var score: Int
}
```

`struct Student` dapat Anda anggap sebagai definisi bentuk data.

Kita sedang mengatakan:
> Sebuah Student harus mempunyai nama dan nilai.

Belum ada siswa nyata ketika kita menulis `struct Student`.
Ini baru cetakannya.

Ketika kita menulis:
```
let student1 = Student(
    name: "Dede",
    score: 80
)
```
barulah dibuat sebuah data Student nyata.

Kita juga bisa membuat:
```
let student2 = Student(
    name: "Ahmad",
    score: 90
)
```

Sekarang ada dua nilai berbeda:
```
Student
├── Dede
│   └── 80
│
└── Ahmad
    └── 90
```

Ini alasan model Swift biasanya berisi banyak `struct`.
Contohnya aplikasi absensi kita nantinya secara konseptual dapat mempunyai:
```
struct Student {
    let id: Int
    let name: String
    var attendanceStatus: String
}
```

Aplikasi transaksi:
```
struct Transaction {
    let id: Int
    let title: String
    let amount: Int
}
```

Aplikasi chat:
```
struct Message {
    let id: String
    let sender: String
    let text: String
}
```

Jadi jawaban nomor 1:
> `struct Student` mendefinisikan bentuk atau model data untuk Student.

Belum menciptakan siswa tertentu.