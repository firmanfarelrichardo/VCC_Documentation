# Dokumentasi Deployment: Anggota 2 - Web Server (Apache & PHP)

Dokumen ini berisi panduan langkah demi langkah untuk Anggota 2 dalam menyiapkan **Web Server** menggunakan **Apache dan PHP** pada sistem operasi Linux.

## 📋 Prasyarat
- Sistem Operasi: **Ubuntu 20.04 LTS atau 22.04 LTS** (Sangat disarankan karena ketersediaan package Apache dan PHP yang stabil).
- Akses Root / Sudo privilege.
- Koneksi internet aktif.

---

## 🛠️ Langkah-Langkah Instalasi

### 1. Update dan Upgrade Sistem
Pastikan sistem operasi dalam keadaan terbaru secara paket sebelum instalasi berlangsung.
```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Install Apache Web Server
Jalankan perintah berikut untuk menginstal Apache2.
```bash
sudo apt install apache2 -y
```

### 3. Konfigurasi Firewall (Opsional tapi Disarankan)
Jika menggunakan UFW (Uncomplicated Firewall), izinkan trafik menuju Apache agar bisa diakses secara eksternal.
```bash
sudo ufw allow 'Apache Full'
sudo ufw status
```

### 4. Enable Services Apache
Pastikan layanan web berjalan di belakang layar secara otomatis jika server di-restart.
```bash
sudo systemctl enable apache2
sudo systemctl start apache2
sudo systemctl status apache2
```

### 5. Install PHP dan Ekstensi
Agar server dapat membaca kode PHP dan terkoneksi ke Database MySQL (di server Anggota 3), diperlukan PHP dan ekstensinya.
```bash
sudo apt install php libapache2-mod-php php-mysql -y
```
Cek versi PHP yang berhasil terinstal:
```bash
php -v
```

### 6. Restart Server Apache
Setelah PHP diinstal, server Apache perlu direstart untuk menerapkan module baru.
```bash
sudo systemctl restart apache2
```

---

## ⚙️ Testing dan Konfigurasi

### 1. Membuat File Info PHP
Untuk memastikan bahwa server Web dan bahasa PHP sudah tervalidasi dengan baik, buat sebuah file tes di direktori utama Web (Document Root).
```bash
sudo nano /var/www/html/info.php
```

### 2. Isi File
Isikan baris kode berikut pada nano editor:
```php
<?php
phpinfo();
?>
```
*Simpan file (Ctrl+O, Enter, Ctrl+X).*

### 3. Hapus File Index HTML Default (Opsional)
Untuk memprioritaskan membaca file berextension `.php`.
```bash
sudo rm /var/www/html/index.html
```

---

## 🎬 Skenario Dokumentasi Video (Untuk Anggota 2)
Agar dokumentasi video terstruktur dengan baik, pastikan Anggota 2 merekam aktivitas berikut:
1. Menunjukkan terminal saat proses komando `apt install apache2` dan `apt install php libapache2-mod-php php-mysql` sukses.
2. Membuka browser (dari komputer lokal) dan mengakses menggunakan IP dari VPS / Server Linux (contoh: `http://<IP_SERVER_WEB>`).
3. Memperlihatkan bahwa halaman "Apache2 Ubuntu Default Page" (jika index.html belum dihapus) berhasil tampil di layar.
4. Mengakses URL `http://<IP_SERVER_WEB>/info.php` dan menunjukkan bahwa halaman informasi konfigurasi PHP berhasil ditampilkan. Ini membuktikan PHP berjalan terintegrasi dengan Apache.
