# Docker & Uptime Kuma untuk Monitoring Jaringan

Membangun sistem monitoring jaringan sederhana menggunakan Docker dan Uptime Kuma.

## 📖 Deskripsi

Monitoring merupakan salah satu aspek penting dalam pengelolaan jaringan komputer. Dengan monitoring, administrator dapat memantau kondisi perangkat dan layanan secara real-time sehingga potensi gangguan dapat diketahui lebih cepat.

Pada proyek ini, Uptime Kuma dijalankan menggunakan Docker di lingkungan Windows dengan bantuan WSL 2. Pendekatan ini dipilih karena proses instalasi lebih praktis, minim konfigurasi, serta lebih konsisten dibandingkan instalasi langsung menggunakan Node.js.

## 🚀 Tech Stack

* Windows Command Prompt (CMD)
* WSL 2 (Windows Subsystem for Linux)
* Docker Desktop
* Uptime Kuma
* Telegram Bot (Opsional)

## 🛠️ Instalasi WSL 2

Buka Command Prompt sebagai Administrator, lalu jalankan:

```bash
wsl --install
```

Setelah proses selesai:

1. Restart komputer.
2. Tunggu hingga Ubuntu terbuka otomatis.
3. Buat username dan password Linux sesuai kebutuhan.

---

## 🐳 Instalasi Docker Desktop

1. Unduh Docker Desktop dari:

```bash
https://www.docker.com/products/docker-desktop/
```

2. Jalankan installer.
3. Pastikan opsi **Use WSL 2 instead of Hyper-V** dicentang.
4. Selesaikan proses instalasi.
5. Jalankan Docker Desktop hingga status **Docker Engine Running**.

Verifikasi instalasi:

```bash
docker --version
```

---

## 📦 Deploy Uptime Kuma Menggunakan Docker

Jalankan perintah berikut:

```bash
docker run -d \
  --restart=always \
  -p 3001:3001 \
  -v uptime-kuma:/app/data \
  --name uptime-kuma \
  louislam/uptime-kuma:1
```

### Penjelasan Parameter

| Parameter                  | Fungsi                              |
| -------------------------- | ----------------------------------- |
| `docker run`               | Menjalankan container               |
| `-d`                       | Menjalankan container di background |
| `--restart=always`         | Auto start saat sistem menyala      |
| `-p 3001:3001`             | Mapping port host ke container      |
| `-v uptime-kuma:/app/data` | Menyimpan data secara persisten     |
| `--name uptime-kuma`       | Memberikan nama container           |

Tunggu hingga proses pull image selesai.

---

## ⚙️ Setup Awal Uptime Kuma

Buka browser dan akses:

```text
http://localhost:3001
```

Langkah konfigurasi:

1. Pilih database **SQLite**.
2. Buat akun administrator.
3. Login ke dashboard Uptime Kuma.

---

## 🌐 Simulasi Monitoring Website

### Membuat Monitor HTTP(S)

1. Klik **Add New Monitor**.
2. Isi data berikut:

| Field              | Value              |
| ------------------ | ------------------ |
| Monitor Type       | HTTP(S)            |
| Friendly Name      | Website Google     |
| URL                | https://google.com |
| Heartbeat Interval | 20 detik           |

3. Klik **Save**.

Jika berhasil, status monitor akan berubah menjadi **UP (Hijau)**.

---

## 📡 Simulasi Monitoring IP Address

### Membuat Monitor Ping

1. Klik **Add New Monitor**.
2. Isi data berikut:

| Field              | Value               |
| ------------------ | ------------------- |
| Monitor Type       | Ping                |
| Friendly Name      | Link Internet Utama |
| Hostname           | 8.8.8.8             |
| Heartbeat Interval | 20 detik            |

3. Klik **Save**.

Monitor akan menampilkan status **UP**.

---

## ❌ Simulasi Gangguan Jaringan

Untuk melihat cara kerja deteksi gangguan:

1. Edit monitor Ping yang sudah dibuat.
2. Ubah Hostname menjadi:

```text
192.168.254.254
```

atau

```text
inipastimati.com
```

3. Simpan perubahan.
4. Tunggu sekitar 20–40 detik.

Status monitor akan berubah menjadi:

```text
DOWN (Ping Timeout)
```

Untuk mengembalikan kondisi normal, ubah kembali Hostname menjadi:

```text
8.8.8.8
```

---

## 🔔 Notifikasi Telegram (Opsional)

### 1. Membuat Bot Telegram

Cari:

```text
@BotFather
```

Lalu jalankan:

```text
/newbot
```

Ikuti instruksi hingga mendapatkan:

```text
HTTP API Token
```

### 2. Mendapatkan Chat ID

Cari:

```text
@userinfobot
```

Klik:

```text
Start
```

Salin Chat ID yang diberikan.

### 3. Integrasi dengan Uptime Kuma

Masuk ke:

```text
Settings → Notifications → Setup Notification
```

Isi:

| Field             | Value                |
| ----------------- | -------------------- |
| Notification Type | Telegram             |
| Friendly Name     | Telegram Admin       |
| Bot Token         | Token dari BotFather |
| Chat ID           | ID dari UserInfoBot  |

Klik:

```text
Test
```

Jika berhasil, Telegram akan menerima pesan percobaan dari Uptime Kuma.

Selanjutnya aktifkan notifikasi tersebut pada monitor yang ingin dipantau.

---

## 📚 Referensi

* Docker: https://www.docker.com/
* Uptime Kuma: https://github.com/louislam/uptime-kuma
* WSL: https://learn.microsoft.com/windows/wsl/
