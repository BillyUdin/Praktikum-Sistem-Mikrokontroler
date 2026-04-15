# 📘 Seven Segment & Push Button Counter (Arduino)

## 📌 Deskripsi
Program ini digunakan untuk menampilkan angka heksadesimal (0 sampai F) pada Seven Segment berbasis Arduino. Selain itu, terdapat dua push button yang berfungsi untuk menambah (increment) dan mengurangi (decrement) nilai yang ditampilkan.

---

## ⚙️ Konsep Dasar
Program ini menerapkan beberapa konsep utama:
- Input dan Output digital (GPIO)
- Penyimpanan data menggunakan array
- Pembacaan bit (bitwise)
- Perulangan (looping)
- Push button dengan mode INPUT_PULLUP
- Debouncing sederhana

---

## 🔢 Kode Program

```cpp
// Array pola heksadesimal untuk tampilan 0-F (Common Cathode)
byte segmentCodes[] = {
  0x3F, 0x06, 0x5B, 0x4F, 0x66, 0x6D, 0x7D, 0x07, 
  0x7F, 0x6F, 0x77, 0x7C, 0x39, 0x5E, 0x79, 0x71
};

const int pinUp = 9;     // Tombol untuk menambah nilai
const int pinDown = 10;  // Tombol untuk mengurangi nilai
int counter = 0;         // Variabel penyimpan angka

void setup() {
  // Mengatur pin 2 sampai 8 sebagai output (untuk Seven Segment)
  for (int i = 2; i <= 8; i++) {
    pinMode(i, OUTPUT);
  }

  // Mengatur pin tombol sebagai input dengan pull-up internal
  pinMode(pinUp, INPUT_PULLUP);
  pinMode(pinDown, INPUT_PULLUP);

  // Menampilkan angka awal
  displayDigit(segmentCodes[counter]);
}

void loop() {
  // Jika tombol tambah ditekan
  if (digitalRead(pinUp) == LOW) {
    counter++;                  // Nilai bertambah
    if (counter > 15) counter = 0; // Kembali ke 0 jika lebih dari F
    displayDigit(segmentCodes[counter]);
    delay(200);                // Delay untuk menghindari bouncing
  }

  // Jika tombol kurang ditekan
  if (digitalRead(pinDown) == LOW) {
    counter--;                 // Nilai berkurang
    if (counter < 0) counter = 15; // Kembali ke F jika kurang dari 0
    displayDigit(segmentCodes[counter]);
    delay(200);               // Delay debouncing
  }
}

// Fungsi untuk menampilkan angka ke Seven Segment
void displayDigit(byte code) {
  for (int bit = 0; bit < 7; bit++) {
    int state = bitRead(code, bit); // Mengambil nilai tiap bit
    digitalWrite(bit + 2, state);   // Mengirim ke pin Arduino
  }
}
