# Praktikum Modul 5 – FreeRTOS pada Arduino

Repository ini berisi program hasil praktikum Modul 5 mengenai penerapan FreeRTOS pada Arduino menggunakan multitasking dan queue.

## 📁 Daftar File

| File | Deskripsi |
|---|---|
| `percobaan_5A.ino` | Program multitasking LED dan serial monitor menggunakan FreeRTOS |
| `Percobaan_5B.ino` | Program queue pada FreeRTOS untuk mengirim data sensor |
| `Modifikasi_Program_5A.ino` | Modifikasi pembacaan sensor DHT22 menggunakan queue |
| `Modifikasi_Program_5B.ino` | Modifikasi queue dan multitasking dengan sensor DHT22 |

---

# ⚙️ Percobaan 5A – Multitasking FreeRTOS

## 📌 Deskripsi
Program ini menggunakan FreeRTOS untuk menjalankan beberapa task secara bersamaan.

## 💻 Code Program

```cpp
#include <Arduino_FreeRTOS.h>

void TaskBlink1(void *pvParameters);
void TaskBlink2(void *pvParameters);
void TaskCounter(void *pvParameters);

void setup() {
  pinMode(8, OUTPUT);
  pinMode(7, OUTPUT);

  Serial.begin(9600);

  xTaskCreate(TaskBlink1, "Blink1", 128, NULL, 1, NULL);
  xTaskCreate(TaskBlink2, "Blink2", 128, NULL, 1, NULL);
  xTaskCreate(TaskCounter, "Counter", 128, NULL, 1, NULL);
}

void loop() {
}

void TaskBlink1(void *pvParameters) {
  while (1) {
    digitalWrite(8, HIGH);
    vTaskDelay(200 / portTICK_PERIOD_MS);
    digitalWrite(8, LOW);
    vTaskDelay(200 / portTICK_PERIOD_MS);
    Serial.println("Task1");
  }
}

void TaskBlink2(void *pvParameters) {
  while (1) {
    digitalWrite(7, HIGH);
    vTaskDelay(300 / portTICK_PERIOD_MS);
    digitalWrite(7, LOW);
    vTaskDelay(300 / portTICK_PERIOD_MS);
    Serial.println("Task2");
  }
}

void TaskCounter(void *pvParameters) {
  int count = 0;

  while (1) {
    count++;
    Serial.println(count);
    vTaskDelay(500 / portTICK_PERIOD_MS);
  }
}
```

---

# ⚙️ Percobaan 5B – Queue FreeRTOS

## 📌 Deskripsi
Program ini menggunakan Queue pada FreeRTOS untuk komunikasi antar task.

## 💻 Code Program

```cpp
#include <Arduino_FreeRTOS.h>
#include <queue.h>

QueueHandle_t queue;

void SenderTask(void *pvParameters);
void ReceiverTask(void *pvParameters);

void setup() {
  Serial.begin(9600);

  queue = xQueueCreate(5, sizeof(int));

  xTaskCreate(SenderTask, "Sender", 128, NULL, 1, NULL);
  xTaskCreate(ReceiverTask, "Receiver", 128, NULL, 1, NULL);
}

void loop() {
}

void SenderTask(void *pvParameters) {
  int value = 0;

  while (1) {
    value++;

    xQueueSend(queue, &value, portMAX_DELAY);

    vTaskDelay(1000 / portTICK_PERIOD_MS);
  }
}

void ReceiverTask(void *pvParameters) {
  int receivedValue;

  while (1) {
    if (xQueueReceive(queue, &receivedValue, portMAX_DELAY)) {
      Serial.print("Data diterima: ");
      Serial.println(receivedValue);
    }
  }
}
```

---

# ⚙️ Modifikasi Program 5A – Sensor DHT22

## 📌 Deskripsi
Program membaca suhu dan kelembapan menggunakan sensor DHT22 dengan FreeRTOS.

## 💻 Code Program

```cpp
#include <Arduino_FreeRTOS.h>
#include <queue.h>
#include <DHT.h>

#define DHTPIN 2
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);

QueueHandle_t queue;

typedef struct {
  float temp;
  float hum;
} SensorData;

void ReadSensor(void *pvParameters);
void DisplayData(void *pvParameters);

void setup() {
  Serial.begin(9600);

  dht.begin();

  queue = xQueueCreate(5, sizeof(SensorData));

  xTaskCreate(ReadSensor, "ReadSensor", 128, NULL, 1, NULL);
  xTaskCreate(DisplayData, "DisplayData", 128, NULL, 1, NULL);
}

void loop() {
}

void ReadSensor(void *pvParameters) {
  SensorData data;

  while (1) {
    data.temp = dht.readTemperature();
    data.hum = dht.readHumidity();

    xQueueSend(queue, &data, portMAX_DELAY);

    vTaskDelay(2000 / portTICK_PERIOD_MS);
  }
}

void DisplayData(void *pvParameters) {
  SensorData receivedData;

  while (1) {
    if (xQueueReceive(queue, &receivedData, portMAX_DELAY)) {
      Serial.print("Temperature = ");
      Serial.print(receivedData.temp);
      Serial.println(" C");

      Serial.print("Humidity = ");
      Serial.print(receivedData.hum);
      Serial.println(" %");
    }
  }
}
```

---

# ⚙️ Modifikasi Program 5B – Queue + DHT22

## 📌 Deskripsi
Pengembangan program queue menggunakan sensor DHT22 dan multitasking.

## 💻 Code Program

```cpp
#include <Arduino_FreeRTOS.h>
#include <queue.h>
#include <DHT.h>

#define DHTPIN 2
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);

QueueHandle_t queue;

float temperature;

void SensorTask(void *pvParameters);
void PrintTask(void *pvParameters);

void setup() {
  Serial.begin(9600);

  dht.begin();

  queue = xQueueCreate(5, sizeof(float));

  xTaskCreate(SensorTask, "SensorTask", 128, NULL, 1, NULL);
  xTaskCreate(PrintTask, "PrintTask", 128, NULL, 1, NULL);
}

void loop() {
}

void SensorTask(void *pvParameters) {
  while (1) {
    temperature = dht.readTemperature();

    xQueueSend(queue, &temperature, portMAX_DELAY);

    vTaskDelay(1000 / portTICK_PERIOD_MS);
  }
}

void PrintTask(void *pvParameters) {
  float receivedTemp;

  while (1) {
    if (xQueueReceive(queue, &receivedTemp, portMAX_DELAY)) {
      Serial.print("Suhu: ");
      Serial.print(receivedTemp);
      Serial.println(" C");
    }
  }
}
```

---

# 📚 Library yang Digunakan

```cpp
#include <Arduino_FreeRTOS.h>
#include <queue.h>
#include <DHT.h>
```

---

# 🎯 Kesimpulan

Pada praktikum ini dipelajari:
- Konsep multitasking menggunakan FreeRTOS
- Penggunaan task pada Arduino
- Queue untuk komunikasi antar task
- Pembacaan sensor menggunakan multitasking

---

# 👨‍💻 Identitas

**Nama:** Muhammad Nabil Zaedan Agesy  
**NIM:** H1H024062  
**Program Studi:** Teknik Komputer  
**Universitas:** Universitas Jenderal Soedirman
