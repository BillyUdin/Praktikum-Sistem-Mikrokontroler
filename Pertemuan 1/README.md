Nama : Muhammad Nabil Zaedan Agesy, Shift Awal : B, Shit Akhir : D

## 1.5 Pertanyaan Praktikum

### 1. Kapan blok `if` dijalankan?
Blok `if` akan dieksekusi ketika nilai variabel `timeDelay` telah mencapai **100 ms atau lebih kecil dari nilai tersebut**. Kondisi ini menandakan bahwa proses percepatan kedipan LED sudah mencapai batas paling cepat yang diizinkan oleh program. Saat syarat tersebut terpenuhi, program akan menghentikan proses percepatan sementara dengan memberikan jeda selama **3 detik**, kemudian nilai `timeDelay` dikembalikan ke nilai awal yaitu **1000 ms** agar siklus kedipan dimulai kembali dari awal.

### 2. Kapan blok `else` dijalankan?
Selama nilai `timeDelay` masih berada **di atas 100 ms**, maka bagian `else` akan dijalankan. Pada bagian ini program akan mengurangi nilai `timeDelay` sebesar **100 ms setiap satu siklus kedipan**. Dampaknya adalah interval waktu antara LED menyala dan mati semakin pendek, sehingga LED terlihat berkedip semakin cepat.

### 3. Fungsi perintah `delay(timeDelay)`
Instruksi `delay(timeDelay)` digunakan untuk memberikan **waktu tunggu** pada jalannya program sesuai nilai yang tersimpan dalam variabel `timeDelay`. Dalam satu siklus, perintah ini digunakan dua kali, yaitu ketika LED dalam kondisi **menyala (HIGH)** dan ketika LED dalam kondisi **mati (LOW)**. Karena kedua jeda memiliki nilai yang sama, maka durasi nyala dan mati LED menjadi seimbang dan membentuk pola kedipan yang teratur.

### 4. Modifikasi Program (Cepat → Sedang → Mati)

Program dimodifikasi agar pola LED tidak langsung kembali ke kondisi awal setelah berkedip cepat. Sebaliknya, LED akan berubah secara bertahap dari **kedipan cepat menjadi lebih lambat**, kemudian berhenti sejenak sebelum siklus diulang kembali.
Berikut merupakan kode program yang digunakan pada percobaan ini:
![alt text](https://github.com/BillyUdin/Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%201/source%20code%20percobaan_1A/Kode_LED.png?raw=true)

## 1.6 Pertanyaan Praktikum

### Schematic 5 LED Running
![alt text](https://github.com/BillyUdin/Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%201/source%20code%20percobaan_1A/skematik%20LED%20berjalan.png?raw=true)
### Bagaimana program menghasilkan efek LED bergerak dari kiri ke kanan?

Pergerakan LED dari sisi kiri menuju kanan dibuat menggunakan sebuah perulangan `for` yang dimulai dari pin bernilai kecil menuju pin yang lebih besar. Nilai pin dimulai dari **2** kemudian bertambah sampai **7** (`ledPin++`). Pada setiap iterasi, program menyalakan satu LED menggunakan `digitalWrite(ledPin, HIGH)`, kemudian menunggu beberapa saat sebelum LED dimatikan kembali. Karena proses ini terjadi secara berurutan dan hanya satu LED yang aktif dalam satu waktu, cahaya yang terlihat akan tampak berpindah dari LED pertama menuju LED terakhir.

### Bagaimana LED bergerak kembali dari kanan ke kiri?

Gerakan sebaliknya dibuat dengan menggunakan perulangan `for` yang berjalan secara menurun. Nilai pin dimulai dari **7** kemudian berkurang hingga mencapai **2** (`ledPin--`). Dengan urutan tersebut, LED yang menyala akan berpindah dari sisi kanan menuju sisi kiri. Kombinasi kedua perulangan ini menghasilkan efek visual LED yang bergerak bolak-balik menyerupai pola lampu pada kendaraan atau efek Knight Rider.

### Program LED menyala tiga di kiri dan tiga di kanan secara bergantian
