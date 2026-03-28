# Dokumentasi Lengkap: Zigbee Gateway iothings.io (Pro Edition)

Selamat datang di pusat informasi teknis untuk **Zigbee Gateway v2.0**. Perangkat ini dirancang khusus untuk reliabilitas tinggi pada ekosistem otomasi industri dan rumah pintar.

---

## 1. Arsitektur Perangkat Keras
Perangkat ini mengadopsi desain *Dual-Processor Architecture* untuk memisahkan beban kerja jaringan WiFi dan koordinasi Zigbee.

### 1.1 Komponen Utama
| Komponen | Spesifikasi | Fungsi |
| :--- | :--- | :--- |
| **MCU** | ESP32-S3-WROOM-1 | Processor utama untuk handling WiFi & MQTT |
| **Zigbee Module** | E18-MS1PA2 (CC2652P) | Koordinator jaringan Zigbee dengan PA/LNA |
| **Antena** | External Omni 3dBi | Memperluas jangkauan hingga 100m (Open Field) |
| **Power Management** | IP5306 / LDO 3.3V | Proteksi tegangan dan manajemen daya baterai |

---

## 2. Pinout & Konektivitas
Bagi Bapak yang ingin melakukan kustomisasi atau perbaikan, berikut adalah pemetaan Pinout pada PCB:

* **UART0 (ESP32):** Digunakan untuk debugging dan flashing firmware utama.
* **UART1 (ESP32 to Zigbee):** Jalur komunikasi data serial antara ESP32 dan modul E18.
    * `TX (ESP32) -> RX (E18)`
    * `RX (ESP32) -> TX (E18)`
* **Status LED:** * `GPIO 2` : Indikator Power (Blue)
    * `GPIO 4` : Indikator Data Zigbee (Green)

---

## 3. Instalasi Firmware (Flashing Guide)
Jika Bapak perlu melakukan *re-flash* pada koordinator Zigbee (CC2652P), ikuti langkah berikut:

### 3.1 Persiapan Software
1.  Download **TI Flash Programmer 2**.
2.  Siapkan file firmware `.hex` (Z-Stack 3.x.0).

### 3.2 Prosedur Flashing
1.  Hubungkan pin `FLASH` dan `GND` pada modul E18.
2.  Gunakan USB-to-TTL (seperti CP2102) untuk menghubungkan modul ke PC.
3.  Pilih file firmware dan klik **Write**.
4.  Setelah selesai, lepas pin jumper dan lakukan *power cycle* (cabut-pasang kabel).

---

## 4. Konfigurasi Lanjutan (Advanced Setup)

### 4.1 Mengatur Channel Zigbee
Seringkali sinyal WiFi 2.4GHz mengganggu (interferensi) sinyal Zigbee. Kami menyarankan penggunaan channel berikut:
* **WiFi:** Gunakan Channel 1 atau 6.
* **Zigbee:** Gunakan Channel 11, 15, atau 20.

### 4.2 Mode TCP Bridge
Untuk integrasi ke **Zigbee2MQTT** secara remote, pastikan ESP32 dikonfigurasi dalam mode **Transparent Bridge**. Firmware kami mendukung baudrate 115200 secara default.

---

## 5. Topologi Jaringan
Gateway ini mampu menangani hingga:
* **32 Direct Children** (Perangkat yang terhubung langsung).
* **200 Total Devices** (Dengan bantuan Router/Repeater).

### Tips Jaringan Stabil:
Jangan biarkan perangkat "End-Device" (seperti sensor baterai) berada terlalu jauh tanpa adanya "Router" (seperti saklar lampu beraliran listrik AC) di tengah-tengah jalur komunikasi.

---

## 6. Integrasi Visual dengan Home Assistant
Bapak bisa mempercantik tampilan dashboard Home Assistant dengan menambahkan kartu status:

'''yaml
type: entities
entities:
  - entity: sensor.zigbee_gateway_status
  - entity: sensor.zigbee_gateway_uptime
  - entity: button.zigbee_gateway_restart
title: iothings Gateway Monitor

---
[Kembali ke Halaman Utama](../)
