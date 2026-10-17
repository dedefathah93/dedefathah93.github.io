Perhatikan:
```
struct Student {
    let name: String
    var score: Int

    var passed: Bool {
        score >= 75
    }
}

var studentA = Student(
    name: "Ahmad",
    score: 60
)

var studentB = studentA

studentB.score = 90
```

Jawab 6 pertanyaan ini saja:
1. Berapa `studentA.score` setelah seluruh kode dijalankan?
2. Berapa `studentB.score`?
3. Berapa `studentA.passed`?
4. Berapa `studentB.passed`?
5. Kenapa perubahan `studentB.score` tidak mengubah `studentA.score`?
6. Kalau kita menulis `studentA.name = "Dede"`, menurut Anda bisa atau error? Jelaskan kenapa.

Jawaban:
1. 60
2. 90
3. False
4. True
5. karena studentA.score punya nilai independen sendiri.
6. error, karena bentuk utama dari studentA.name adalah let

Tanggapan:
Semua 6 jawaban Anda benar secara konsep.

Satu detail syntax saja: Swift menulis Boolean dengan huruf kecil:
```
false
true
```

Jadi hasilnya:
```
studentA.score  = 60
studentB.score  = 90
studentA.passed = false
studentB.passed = true
```

Untuk nomor 6, alasan Anda juga tepat. Istilah yang lebih presisi adalah: property `name` dideklarasikan sebagai `let`, sehingga setelah instance `Student` dibuat, `name` tidak dapat dimutasi.

Dengan ini saya anggap Hari 1 selesai. Anda sudah memahami fondasi berikut:
```
struct
property
let
var
mutation
computed property
value semantics
```

Sekarang kita masuk Hari 2. Ini konsep yang sangat penting di Swift.