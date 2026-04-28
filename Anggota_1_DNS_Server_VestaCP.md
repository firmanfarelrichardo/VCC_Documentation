# Dokumentasi Deployment: Anggota 1 - DNS Server (VestaCP)

Dokumen ini berisi panduan langkah demi langkah untuk Anggota 1 dalam menyiapkan **DNS Server** menggunakan **VestaCP** pada sistem operasi Linux.

## 📋 Prasyarat
- Sistem Operasi: **Ubuntu 18.04 LTS** (Sangat disarankan karena kompatibilitas VestaCP sangat stabil di versi ini, OS lain yang didukung: RHEL/CentOS 5,6,7, Debian 7,8,9).
- Akses Root / Sudo privilege.
- Koneksi internet aktif.

---

## 🛠️ Langkah-Langkah Instalasi

### 1. Update dan Upgrade Sistem
Pastikan sistem operasi dalam keadaan terbaru sebelum melakukan instalasi.
```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Download Script Installer VestaCP
Unduh script bash instalasi VestaCP dari situs resminya menggunakan `curl` (pastikan `curl` sudah terinstal).
```bash
curl -O http://vestacp.com/pub/vst-install.sh
```

### 3. Eksekusi Script Instalasi VestaCP
Jalankan script yang telah didownload. Karena fokus utama server ini adalah sebagai **DNS Server**, pastikan komponen DNS (`named`) terinstal.
```bash
sudo bash vst-install.sh
```
*Catatan saat instalasi:*
- Sistem akan meminta konfirmasi. Ketik `y` untuk melanjutkan.
- Masukkan alamat email aktif (untuk notifikasi admin).
- Masukkan hostname (contoh: `ns1.namakelompok.local` atau IP Address Server).
- Tunggu proses instalasi (kurang lebih 10-15 menit).

### 4. Menyimpan Kredensial Login
Setelah proses selesai, VestaCP akan menampilkan informasi login di layar terminal Anda berupa URL, Username (`admin`), dan Password. **Simpan informasi penting ini.**

---

## ⚙️ Konfigurasi DNS Server di VestaCP

### 1. Login ke VestaCP Panel
Buka browser dan akses VestaCP menggunakan URL yang diberikan sebelumnya (contoh: `https://<IP_SERVER_DNS>:8083`).
- Masukkan *Username* dan *Password*.
- Abaikan peringatan SSL/TLS (pilih *Advanced* -> *Proceed/Accept Risk*).

### 2. Menambahkan DNS Zone & Record
Disinilah tugas utama Anggota 1 untuk menghubungkan nama domain ke Web Server (Server Anggota 2).
1. Pada menu utama (header), klik tab **DNS**.
2. Klik tombol **Add Web Domain** atau edit domain lokal yang sudah tersetting.
3. Masukkan nama domain proyek kelompok (misal: `tugasvcc.xyz`).
4. IP Address: **Masukkan IP Address dari Web Server milik Anggota 2**.
5. Klik **Add** atau **Save**.

### 3. Menambahkan Custom A Record (Bila perlu)
Jika butuh menambahkan subdomain (seperti `www` atau `db`), Anda bisa menambahkannya di VestaCP.
1. Pada tab DNS, arahkan mouse ke baris domain yang baru dibuat, lalu klik **LIST [x] RECORDS**.
2. Klik ikon hijau **+ Add Record**.
3. Record: `www`
4. Type: `A`
5. IP or Value: `IP Address Web Server (Anggota 2)`
6. Klik **Add**.

---

## 🎬 Skenario Dokumentasi Video (Untuk Anggota 1)
Agar dokumentasi video terstruktur dengan baik, pastikan Anggota 1 merekam aktivitas berikut:
1. Menunjukkan layar terminal saat proses `bash vst-install.sh` sedang berjalan dan menunjukkan output credentials sukses (username & password).
2. Membuka browser, login ke panel VestaCP di port `8083`.
3. Menunjukkan langkah-langkah menambahkan nama domain pada menu DNS.
4. Menunjukkan list DNS Records yang secara jelas memperlihatkan **A Record** nama domain mengarah ke **IP Adreess Server Anggota 2 (Apache)**.
