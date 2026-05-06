# Jam Digital Arduino dengan LCD 20x4 dan RTC DS3231

Proyek ini merupakan implementasi jam digital menggunakan Arduino, LCD 20x4, modul RTC DS3231, dan Buzzer. Sistem ini menampilkan tanggal dan waktu secara real-time pada layar LCD dengan format yang rapi dan informatif.

## 📋 Daftar Komponen

| Komponen | Spesifikasi | Jumlah |
|----------|-----------|--------|
| Arduino UNO | Microcontroller | 1 |
| LCD 20x4 | Display | 1 |
| RTC DS3231 | Real-Time Clock Module | 1 |
| Buzzer | Audio Indicator | 1 |
| Kabel Jumper | - | Sesuai kebutuhan |
| Power Supply | 5V | 1 |

## 🔌 Koneksi Pin

### LCD 20x4 ke Arduino UNO
- RS (Register Select) → Pin 7
- EN (Enable) → Pin 6
- D4 (Data 4) → Pin 5
- D5 (Data 5) → Pin 4
- D6 (Data 6) → Pin 3
- D7 (Data 7) → Pin 2
- VSS (GND) → GND
- VDD (5V) → 5V

### RTC DS3231 ke Arduino UNO
- VCC → 5V
- GND → GND
- SDA (Data) → A4 (SDA)
- SCL (Clock) → A5 (SCL)

### Buzzer ke Arduino UNO
- Positif → Pin 8
- Negatif → GND

## 🔧 Instalasi dan Setup

### 1. Persyaratan Perangkat Lunak
- [PlatformIO](https://platformio.org/) atau Arduino IDE
- Driver USB Arduino (CH340 atau FT232)

### 2. Library yang Digunakan
- `RTClib` - Untuk komunikasi dengan modul RTC DS3231
- `LiquidCrystal` - Untuk kontrol LCD 20x4 (sudah built-in di Arduino IDE)

### 3. Instalasi Melalui PlatformIO
```bash
# Buka folder proyek
cd "Arduino With LCD 20x4 and RTC DS3231 and Buzzer"

# Upload program ke Arduino
platformio run --target upload
```

### 4. Instalasi Melalui Arduino IDE
1. Buka Arduino IDE
2. Pergi ke **Sketch → Include Library → Manage Libraries**
3. Cari dan install `RTClib` oleh Adafruit
4. Upload sketch `src/main.cpp` ke Arduino UNO

## 💻 Fitur Program

### Tampilan LCD
Sistem menampilkan informasi pada 4 baris LCD:
```
        JAM DIGITAL
        LAB ROBOTIK
Ahad, 06/05/2026
    05,45,30 BQ
```

**Penjelasan:**
- **Baris 1**: Judul "JAM DIGITAL" (ditampilkan saat startup)
- **Baris 2**: Subtitle "LAB ROBOTIK" (ditampilkan saat startup)
- **Baris 3**: Nama hari (Ahad-Sabtu), tanggal, bulan, dan tahun
- **Baris 4**: Jam, menit, detik, dan label "BQ"

### Fungsi Buzzer
Buzzer akan menghasilkan suara pada frekuensi 1000 Hz selama 50ms untuk setiap update waktu. Hal ini berfungsi sebagai indikator audio atau efek "tik-tak" pada jam.

### Format Waktu
- **Hari**: Nama hari dalam bahasa Indonesia (Ahad, Senin, Selasa, Rabu, Kamis, Jumat, Sabtu)
- **Tanggal**: Format DD/MM/YYYY dengan leading zero
- **Waktu**: Format HH,MM,SS dengan leading zero

## 📝 Penjelasan Kode Utama

### Setup Awal (`setup()`)
```cpp
void setup() {
  pinMode(10, OUTPUT);           // Konfigurasi pin untuk output
  lcd.begin(20, 4);              // Inisialisasi LCD 20x4
  rtc.begin();                   // Inisialisasi RTC DS3231
  
  // Jika RTC kehilangan daya, set waktu ke waktu kompilasi
  if (rtc.lostPower()) {
    rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
  }
  
  // Tampilkan splash screen
  lcd.setCursor(0,0);
  lcd.print("   JAM DIGITAL   ");
  lcd.setCursor(0,1);
  lcd.print("   LAB ROBOTIK   ");
  delay(1000);
  lcd.clear();
}
```

### Loop Utama (`loop()`)
Program membaca waktu dari RTC setiap 250ms dan menampilkannya di LCD dengan format yang sesuai.

### Helper Function (`printAngka()`)
Fungsi ini memastikan angka ditampilkan dengan format dua digit (leading zero). Contoh: 5 menjadi 05.

## ⚙️ Konfigurasi

Jika perlu mengubah konfigurasi, edit bagian berikut di `src/main.cpp`:

- **Pin LCD**: Ubah parameter pada `LiquidCrystal lcd(7, 6, 5, 4, 3, 2);`
- **Pin Buzzer**: Ubah parameter pada `tone(8, 1000, 50);`
- **Frekuensi Buzzer**: Ubah nilai `1000` sesuai kebutuhan
- **Durasi Buzzer**: Ubah nilai `50` (dalam ms)
- **Update Interval**: Ubah nilai `delay(250);` di akhir loop

## 🐛 Troubleshooting

| Masalah | Solusi |
|---------|--------|
| LCD tidak menampilkan apa-apa | - Cek koneksi kabel<br>- Sesuaikan contrast LCD (potensio pada LCD)<br>- Verifikasi pin konfigurasi |
| Waktu RTC tidak tepat | - Pastikan RTC terhubung dengan benar<br>- Download library RTClib terbaru<br>- Set ulang waktu RTC melalui upload pertama |
| Buzzer tidak berbunyi | - Cek polaritas buzzer<br>- Verifikasi pin 8 sesuai konfigurasi<br>- Test menggunakan kode buzzer terpisah |
| Program tidak terupload | - Cek koneksi USB<br>- Verifikasi driver CH340/FT232<br>- Pilih board yang benar di IDE (Arduino UNO) |

## 📚 Referensi Library

- **RTClib**: https://github.com/adafruit/RTClib
- **LiquidCrystal**: https://www.arduino.cc/en/Reference/LiquidCrystal

## 📄 Lisensi

Proyek ini tersedia untuk keperluan pendidikan dan pengembangan pribadi.

## 👥 Catatan

Proyek ini dikembangkan untuk Lab Robotik dan menggunakan Wokwi simulator untuk prototype digital.

---

**Terakhir diperbarui**: Mei 2026
