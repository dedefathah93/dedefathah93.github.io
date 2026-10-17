Dalam:
```
struct Student {
    let name: String
    var score: Int
}
```
`name` dan `score` disebut property.

Jadi:
```
Student
│
├── property name
└── property score
```

Ketika Anda menulis:
```
student.name
```
Anda sedang membaca property `name`.

Ketika:
```
student.score
```
Anda membaca property `score`.