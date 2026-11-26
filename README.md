# Proyek Mini IoT: Dashboard Node-RED untuk Kontrol Relay dan Monitoring Suhu

Proyek ini merupakan implementasi sistem kontrol jarak jauh dan monitoring berbasis protokol **MQTT** dan **Node-RED Dashboard** menggunakan simulasi perangkat berbasis Python dan broker Mosquitto lokal.

## Identitas Mahasiswa
* **Nama:** Jody Setiawan
* **NIM:** 2410010177
* **Kelas:** 5D REG BJM
* **Mata Kuliah:** Pemrograman Internet of Things

---

## Arsitektur Sistem
Sistem bekerja dengan komunikasi dua arah (*bi-directional*) melalui broker MQTT lokal (`localhost:1883`):
1. **Uplink (Monitoring Sensor):** Skrip Python (`proyek_iot.py`) mempublikasikan data suhu simulasi sensor DHT22 setiap **3 detik** ke broker MQTT, yang kemudian ditampilkan secara *real-time* pada widget **Gauge** di Node-RED Dashboard.
2. **Downlink (Kontrol Aktuator):** Pengguna mengendalikan **Switch Relay** dan **Slider PWM** melalui antarmuka web Node-RED Dashboard (`http://localhost:1880/ui`), yang mengirimkan perintah ke skrip Python untuk memproses status GPIO dan mengirimkan kembali *feedback* status aktual.

---

## Daftar Topik MQTT
| Fungsi | Topik MQTT | Payload | Keterangan |
| :--- | :--- | :--- | :--- |
| **Monitoring Suhu** | `iot/2410010177/sensor/suhu` | Angka float (`°C`) | Publish otomatis setiap 3 detik |
| **Kontrol Relay** | `iot/2410010177/aktuator/relay` | `"ON"` / `"OFF"` | Mengendalikan simulasi GPIO 26 |
| **Kontrol PWM LED** | `iot/2410010177/aktuator/pwm` | `0` – `255` | Mengatur kecerahan LED simulasi GPIO 2 |
| **Feedback Status Relay** | `iot/2410010177/status/relay` | `"ON"` / `"OFF"` | Sinkronisasi status aktual relay ke dashboard |

---

## Struktur File Repository
* `proyek_iot.py` — Kode program Python (pengganti perangkat IoT/ESP32) untuk simulasi pembacaan suhu DHT22 dan eksekusi kontrol aktuator.
* `flow_proyek_iot.json` — File ekspor *flow* Node-RED Dashboard.
* `screenshot_dashboard.png` — Tangkapan layar dashboard Node-RED dan terminal perangkat yang sedang berjalan.
* `demo_kontrol.mp4` — Video demonstrasi singkat kontrol aktuator dan monitoring suhu secara *real-time*.

---

## Cara Menjalankan Proyek
1. Jalankan broker **Mosquitto** secara lokal pada port `1883`.
2. Jalankan **Node-RED** (`node-red`), import file `flow_proyek_iot.json`, lalu klik **Deploy**.
3. Instal dependensi Python (`pip install paho-mqtt`) dan jalankan skrip perangkat:
   ```bash
   python proyek_iot.py