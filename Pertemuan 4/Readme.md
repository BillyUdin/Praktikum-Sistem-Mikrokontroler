# 📘 Praktikum Modul 4

## ADC, Servo, dan PWM pada Arduino

---

## 🔹 Percobaan 1: ADC dan Kontrol Motor Servo

### 📌 Tujuan

Memahami cara membaca sinyal analog dan menggunakannya untuk mengontrol posisi motor servo.

---

### ❓ 1. Apa itu pembacaan analog pada Arduino?

Pembacaan analog dilakukan menggunakan fungsi `analogRead()`, yang berfungsi untuk membaca tegangan dari pin analog Arduino.

Arduino Uno memiliki **ADC (Analog to Digital Converter) 10-bit**, sehingga nilai yang dihasilkan berada pada rentang:

* `0` → 0 Volt
* `1023` → 5 Volt

Artinya, setiap perubahan tegangan akan dikonversi menjadi nilai digital yang bisa diproses oleh program.

---

### ❓ 2. Mengapa menggunakan fungsi `map()`?

Fungsi `map()` digunakan untuk mengubah suatu rentang nilai ke rentang lain.

Pada percobaan ini:

* Input dari ADC: `0 – 1023`
* Output untuk servo: `0 – 180 derajat`

Tanpa `map()`, nilai dari sensor tidak bisa langsung digunakan untuk mengontrol servo secara proporsional.

---

### 🛠️ 3. Modifikasi Program (Servo 20° – 160°)

```cpp
#include <Servo.h>

Servo servoKu;

void setup() {
  servoKu.attach(9);
}

void loop() {
  int adc = analogRead(A0);

  int sudut = map(adc, 0, 1023, 20, 160);

  servoKu.write(sudut);

  delay(15);
}
```

---

### ⚙️ Cara Kerja

1. Arduino membaca nilai dari potensiometer.
2. Nilai tersebut dikonversi ke sudut servo.
3. Servo bergerak mengikuti perubahan input.
4. Delay kecil membantu pergerakan lebih stabil.

---

### 📊 Hasil Pengamatan

* Pergerakan servo terasa halus
* Sudut terbatas antara 20° hingga 160°
* Respons mengikuti posisi potensiometer secara real-time

---

## 🔹 Percobaan 2: PWM untuk Kontrol LED

### 📌 Tujuan

Mengetahui cara mengatur intensitas LED menggunakan PWM.

---

### ❓ 1. Apa fungsi PWM?

PWM (*Pulse Width Modulation*) adalah teknik untuk menghasilkan sinyal yang menyerupai analog dari pin digital.

PWM digunakan untuk:

* Mengatur terang redup LED
* Mengontrol kecepatan motor
* Mengatur daya output

Pin PWM pada Arduino biasanya ditandai dengan simbol `~`.

---

### ❓ 2. Hubungan ADC dan PWM

Dalam percobaan ini:

* ADC membaca input dari potensiometer
* PWM mengatur output LED

Karena perbedaan resolusi:

* ADC: `0 – 1023` (10-bit)
* PWM: `0 – 255` (8-bit)

Maka diperlukan fungsi `map()` agar nilainya sesuai.

---

### 📊 Perbandingan Resolusi

| Komponen | Resolusi | Rentang  |
| -------- | -------- | -------- |
| ADC      | 10-bit   | 0 – 1023 |
| PWM      | 8-bit    | 0 – 255  |

---

### 🛠️ 3. Modifikasi Program (LED 30% – 90%)

```cpp
void setup() {
  pinMode(9, OUTPUT);
}

void loop() {
  int nilai = analogRead(A0);

  int pwm = map(nilai, 0, 1023, 77, 230);

  analogWrite(9, pwm);

  delay(10);
}
```

---

### ⚙️ Penjelasan

* Nilai `77` ≈ 30% dari 255
* Nilai `230` ≈ 90% dari 255

---

### 📊 Hasil Pengamatan

* LED tidak pernah mati total
* Perubahan terang terjadi secara bertahap
* Output terlihat stabil

---

## 🧾 Kesimpulan

Dari praktikum ini dapat disimpulkan bahwa:

* Arduino mampu membaca sinyal analog dengan ADC
* Data analog bisa digunakan untuk mengontrol perangkat seperti servo
* PWM memungkinkan pengaturan output secara fleksibel
* Fungsi `map()` sangat penting untuk menyamakan perbedaan rentang nilai

---

## 🚀 Catatan Tambahan

Praktikum ini menunjukkan bagaimana input analog dapat langsung dihubungkan dengan output fisik, yang merupakan dasar penting dalam sistem embedded dan IoT.

---

