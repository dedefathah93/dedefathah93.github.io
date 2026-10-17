Kode ini:
```
var passed: Bool {
    score >= 75
}
```
disebut computed property.

Perhatikan bahwa kita tidak memberi nilai:
```
var passed = true
```

atau:
```
var passed = false
```

Sebaliknya Swift menghitungnya.

Misalnya:
```
score = 70
```

ketika:
```
student.passed
```

dibaca, Swift menghitung:
```
70 >= 75
```

hasil:
```
false
```

Kalau:
```
score = 85
```

Swift menghitung:
```
85 >= 75
```

hasil:
```
true
```

Jadi jawaban nomor 6 adalah:
> `passed` tidak menyimpan data terpisah. Nilainya dihitung dari `score` setiap kali dibaca.

Visualisasinya:
```
score
  │
  ▼
score >= 75
  │
  ▼
passed
```

Kenapa ini bagus?
Karena kita tidak perlu menyimpan dua data seperti:
```
var score = 85
var passed = true
```

Bayangkan suatu saat programmer melakukan:
```
score = 40
```

tetapi lupa:
```
passed = false
```

Kita akan mempunyai data yang bertentangan:
```
score  = 40
passed = true
```

Dengan computed property hal itu tidak mungkin terjadi.
```
var passed: Bool {
    score >= 75
}
```

`passed` selalu mengikuti `score`.

Ini sudah merupakan contoh kecil prinsip engineering penting:
> Jangan menyimpan data yang dapat dihitung dengan aman dari sumber data lain jika tidak diperlukan.