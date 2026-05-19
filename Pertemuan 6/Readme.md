# Modul 6 — Interrupt dan Timer (Arduino Uno)

> **Praktikum Sistem Microcontroler (TK244005)**  
> Laboratorium Multimedia — Jurusan Informatika, UNSOED  
> **Nama:** Muhammad Nabil Zaedan Agesy | **NIM:** H1H024062  
> **Tanggal:** 18 Mei 2026 | **Asisten:** Arga Aryanta Indrafata

---

## 📋 Deskripsi

Repositori ini berisi implementasi dua mekanisme fundamental pada sistem tertanam berbasis **Arduino Uno**:

| Percobaan | Topik | Metode |
|-----------|-------|--------|
| 6A | External Interrupt dengan Push Button | ISR + `attachInterrupt()` |
| 6B | Timer Non-Blocking | `millis()` |

---

## 🛠️ Alat dan Bahan

- Arduino Uno (ATmega328P)
- Breadboard
- 2 buah LED
- 2 buah resistor 220Ω
- 1 buah Push Button
- Kabel jumper
- Arduino IDE

---

## 📁 Struktur Repositori

```
modul-6-interrupt-timer/
├── README.md
├── percobaan_6A/
│   └── external_interrupt.ino   # External interrupt dengan push button
└── percobaan_6B/
    └── timer_millis.ino          # Timer non-blocking dengan millis()
```

---

## ⚡ Percobaan 6A — External Interrupt dengan Push Button

### Deskripsi
Implementasi external interrupt menggunakan push button pada **pin 2 (INT0)** dengan mode `FALLING`. LED di-toggle melalui ISR setiap kali tombol ditekan, tanpa polling.

### Skema Rangkaian
- LED → pin 13 → resistor 220Ω → GND
- Push button → pin 2 (INPUT_PULLUP) → GND

### Kode Program

```cpp
volatile bool ledState = false;

void toggleLED() {
  ledState = !ledState;
}

void setup() {
  pinMode(13, OUTPUT);
  pinMode(2, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(2), toggleLED, FALLING);
}

void loop() {
  digitalWrite(13, ledState);
}
```

### Cara Kerja
1. Pin 2 dijaga pada level **HIGH** oleh resistor pull-up internal.
2. Saat tombol ditekan, pin 2 bertransisi **HIGH → LOW** (*falling edge*).
3. Hardware interrupt **INT0** terpicu → CPU melompat ke ISR `toggleLED()`.
4. ISR membalik nilai `ledState`, lalu CPU melanjutkan `loop()`.
5. `digitalWrite(13, ledState)` menerapkan perubahan ke LED.

### Poin Penting
| Konsep | Penjelasan |
|--------|-----------|
| `volatile` | Mencegah compiler mengoptimasi variabel yang diubah oleh ISR |
| `FALLING` | Interrupt hanya terpicu saat sinyal HIGH → LOW |
| ISR singkat | `delay()` dan `Serial.print()` dilarang di dalam ISR karena bergantung pada interrupt yang dinonaktifkan saat ISR berjalan |

---

## ⏱️ Percobaan 6B — Timer Non-Blocking dengan millis()

### Deskripsi
Implementasi timer periodik menggunakan `millis()` untuk membuat LED berkedip setiap **500ms** tanpa menggunakan `delay()`, sehingga sistem tetap responsif.

### Skema Rangkaian
- LED → pin 13 → resistor 220Ω → GND

### Kode Program

```cpp
const int ledPin = 13;
unsigned long previousMillis = 0;
const long interval = 500;
bool ledState = false;

void setup() {
  pinMode(ledPin, OUTPUT);
}

void loop() {
  unsigned long currentMillis = millis();

  if (currentMillis - previousMillis >= interval) {
    previousMillis = currentMillis;
    ledState = !ledState;
    digitalWrite(ledPin, ledState);
  }
}
```

### Cara Kerja
1. Setiap iterasi `loop()`, nilai `currentMillis = millis()` diambil.
2. Selisih `currentMillis - previousMillis` dibandingkan dengan `interval` (500ms).
3. Jika sudah mencapai threshold → `previousMillis` diperbarui, LED di-toggle.
4. Program **tidak berhenti** — hanya memeriksa kondisi dalam orde nanodetik.

### Perbandingan `delay()` vs `millis()`

| Aspek | `delay()` | `millis()` |
|-------|-----------|------------|
| Blocking | ✅ Ya — CPU berhenti total | ❌ Tidak — CPU terus berjalan |
| Multitasking | ❌ Tidak memungkinkan | ✅ Memungkinkan |
| Responsivitas | ❌ Input diabaikan saat delay | ✅ Input tetap bisa diproses |

### Modifikasi: Dua LED dengan Interval Berbeda

```cpp
const int led1Pin = 13;  // interval 1000ms
const int led2Pin = 12;  // interval 500ms

unsigned long previousMillis1 = 0;
unsigned long previousMillis2 = 0;
bool ledState1 = false;
bool ledState2 = false;

void setup() {
  pinMode(led1Pin, OUTPUT);
  pinMode(led2Pin, OUTPUT);
}

void loop() {
  unsigned long currentMillis = millis();

  // LED 1 — 1 detik
  if (currentMillis - previousMillis1 >= 1000) {
    previousMillis1 = currentMillis;
    ledState1 = !ledState1;
    digitalWrite(led1Pin, ledState1);
  }

  // LED 2 — 500ms
  if (currentMillis - previousMillis2 >= 500) {
    previousMillis2 = currentMillis;
    ledState2 = !ledState2;
    digitalWrite(led2Pin, ledState2);
  }
}
```
