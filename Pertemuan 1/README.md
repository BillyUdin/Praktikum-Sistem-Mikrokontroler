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
![alt text](https://github.com/BillyUdin/Praktikum-Sistem-Mikrokontroler/blob/main/Pertemuan%201/source%20code%20percobaan_1A/LED%203%20bergantian.png?raw=true)

## 1.7 Pertanyaan Analisis

### Uraian Hasil Setiap Percobaan

- **Percobaan 1 – LED Blink dengan Perubahan Kecepatan**

  Pada percobaan pertama digunakan satu LED yang terhubung pada pin digital 6. Variabel `timeDelay` berperan sebagai pengatur jeda waktu antara kondisi LED menyala dan mati. Pada program awal, nilai delay akan terus berkurang sebesar 100 ms setiap siklus sehingga kedipan LED terlihat semakin cepat, dimulai dari 1000 ms hingga mencapai 100 ms. Setelah mencapai batas tersebut, program memberikan jeda sekitar 3 detik kemudian nilai delay dikembalikan ke nilai awal.  

  Pada versi modifikasi, mekanismenya dibalik. Nilai `timeDelay` dimulai dari 100 ms lalu bertambah secara bertahap setiap siklus sampai mencapai 1000 ms. Perubahan ini membuat pola kedipan LED berubah dari cepat, kemudian menjadi lebih lambat, hingga akhirnya berhenti sementara sebelum siklus diulang kembali.

- **Percobaan 2 – LED Running**

  Percobaan kedua memanfaatkan enam buah LED yang dihubungkan pada pin 2 sampai pin 7. Semua pin tersebut diinisialisasi menggunakan satu perulangan `for`. Program kemudian menggunakan dua perulangan yang berbeda arah: perulangan pertama dengan nilai yang meningkat untuk menghasilkan gerakan LED ke satu arah, dan perulangan kedua dengan nilai yang menurun untuk menghasilkan gerakan ke arah sebaliknya.  

  Pada program modifikasi, LED tidak lagi menyala satu per satu, melainkan dibagi menjadi dua kelompok. Kelompok pertama terdiri dari LED pada pin 2–4, sedangkan kelompok kedua berada pada pin 5–7. Kedua kelompok ini dinyalakan secara bergantian sehingga muncul efek dua bagian cahaya yang aktif secara bergantian di sisi kiri dan kanan.

---

### Pengaruh Struktur Perulangan terhadap Jalannya Program

- Struktur perulangan seperti `for` dan `while` memungkinkan suatu instruksi dijalankan berulang kali tanpa perlu menuliskan kode yang sama berkali-kali.  
- Dalam percobaan ini, penggunaan `for` sangat membantu dalam proses inisialisasi beberapa pin sekaligus sehingga program menjadi lebih ringkas.  
- Arah perubahan nilai pada variabel perulangan juga menentukan arah gerakan LED: nilai yang bertambah membuat LED bergerak ke satu arah, sedangkan nilai yang berkurang menghasilkan gerakan sebaliknya.  
- Perulangan `for` biasanya digunakan ketika jumlah pengulangan sudah diketahui sebelumnya, misalnya berdasarkan jumlah pin yang digunakan. Sebaliknya, `while` lebih cocok dipakai pada kondisi yang bergantung pada perubahan nilai tertentu, seperti pembacaan sensor.  
- Dengan adanya perulangan, program Arduino dapat berjalan secara terus-menerus dan menghasilkan perilaku sistem yang konsisten.

---

### Cara Kerja Percabangan (`if-else`) dalam Menentukan Kondisi LED

- Percabangan `if-else` berfungsi sebagai mekanisme pengambilan keputusan di dalam program.  
- Program terlebih dahulu memeriksa suatu kondisi logika, kemudian menentukan blok kode mana yang harus dijalankan.  
- Dalam percobaan ini, kondisi `if (timeDelay <= 100)` digunakan untuk memeriksa apakah nilai delay sudah mencapai batas minimum.  
- Jika kondisi tersebut terpenuhi, nilai `timeDelay` akan dikembalikan ke nilai awal. Jika tidak, program akan terus mengubah nilai delay sesuai aturan yang dibuat.  
- Dengan cara ini, Arduino dapat mengatur perilaku LED secara otomatis berdasarkan nilai variabel yang sedang diproses.

---

### Kombinasi Perulangan dan Percabangan untuk Sistem yang Responsif

- Perulangan dan percabangan merupakan dua struktur dasar yang sering digunakan secara bersamaan dalam pemrograman mikrokontroler.  
- Perulangan memungkinkan program berjalan secara terus-menerus, sedangkan percabangan menentukan tindakan yang harus dilakukan berdasarkan kondisi tertentu.  
- Pada praktikum ini, perulangan digunakan untuk mengatur urutan penyalaan LED, sementara percabangan mengatur perubahan kecepatan kedipan.  
- Konsep yang sama juga diterapkan pada sistem yang lebih kompleks seperti lampu otomatis berbasis sensor cahaya atau sistem peringatan suhu.  
- Untuk meningkatkan respons sistem, penggunaan fungsi `millis()` sering direkomendasikan dibandingkan `delay()`, karena `delay()` menghentikan proses program sementara waktu sedangkan `millis()` memungkinkan pengaturan waktu tanpa menghentikan proses lain yang sedang berjalan.
