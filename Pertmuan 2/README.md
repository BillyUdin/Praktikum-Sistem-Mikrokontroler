# 📘 Seven Segment & Push Button Counter (Arduino)

## 📌 Deskripsi Proyek
Proyek ini merupakan implementasi sistem counter heksadesimal menggunakan Arduino Uno dan Seven Segment Display. Sistem mampu menampilkan angka dari 0 hingga F (basis 16), serta memberikan interaksi kepada pengguna melalui dua buah push button.

Tombol pertama digunakan untuk menambah nilai (increment), sedangkan tombol kedua digunakan untuk mengurangi nilai (decrement). Nilai yang ditampilkan akan selalu berada dalam rentang 0 sampai 15.

---

## 🎯 Tujuan
Adapun tujuan dari pembuatan program ini adalah:
- Memahami cara kerja GPIO pada Arduino
- Mengontrol perangkat output berupa Seven Segment
- Menggunakan push button sebagai input digital
- Menerapkan logika counter naik dan turun
- Menggunakan array untuk mempermudah pengolahan data
- Memahami penggunaan INPUT_PULLUP

---

## 🧰 Alat dan Bahan
Komponen yang digunakan:
- Arduino Uno R3
- Seven Segment Display (Common Cathode)
- Resistor 220 Ohm (±7-8 buah)
- Push Button (2 buah)
- Breadboard
- Kabel jumper
- Kabel USB
- Arduino IDE

---

## ⚙️ Prinsip Kerja Sistem

### 🔹 Seven Segment
Seven Segment terdiri dari 7 LED utama (a–g) yang dapat dikombinasikan untuk membentuk angka atau huruf tertentu. Setiap segmen akan menyala berdasarkan sinyal HIGH atau LOW dari Arduino.

---

### 🔹 Push Button
Push button digunakan sebagai input digital. Pada proyek ini digunakan mode `INPUT_PULLUP`, sehingga:
- Kondisi normal: HIGH
- Saat ditekan: LOW

---

### 🔹 Counter
Counter bekerja dengan cara:
- Menambah nilai saat tombol increment ditekan
- Mengurangi nilai saat tombol decrement ditekan
- Nilai akan kembali ke awal jika melewati batas

---

## 🔢 Kode Program

```cpp
byte segmentCodes[] = {
  0x3F, 0x06, 0x5B, 0x4F, 0x66, 0x6D, 0x7D, 0x07, 
  0x7F, 0x6F, 0x77, 0x7C, 0x39, 0x5E, 0x79, 0x71
};

const int pinUp = 9;
const int pinDown = 10;
int counter = 0;

void setup() {
  for (int i = 2; i <= 8; i++) {
    pinMode(i, OUTPUT);
  }

  pinMode(pinUp, INPUT_PULLUP);
  pinMode(pinDown, INPUT_PULLUP);

  displayDigit(segmentCodes[counter]);
}

void loop() {
  if (digitalRead(pinUp) == LOW) {
    counter++;
    if (counter > 15) counter = 0;
    displayDigit(segmentCodes[counter]);
    delay(200);
  }

  if (digitalRead(pinDown) == LOW) {
    counter--;
    if (counter < 0) counter = 15;
    displayDigit(segmentCodes[counter]);
    delay(200);
  }
}

void displayDigit(byte code) {
  for (int bit = 0; bit < 7; bit++) {
    int state = bitRead(code, bit);
    digitalWrite(bit + 2, state);
  }
}
