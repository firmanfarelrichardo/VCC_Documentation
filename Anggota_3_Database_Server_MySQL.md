# Dokumentasi Deployment: Anggota 3 - Database Server (MySQL)

Dokumen ini berisi panduan langkah demi langkah untuk Anggota 3 dalam menyiapkan **Database Server** menggunakan **MySQL** pada sistem operasi Linux dan membukanya untuk akses dari luar.

## 📋 Prasyarat
- Sistem Operasi: **Ubuntu 20.04 LTS atau 22.04 LTS**.
- Akses Root / Sudo privilege.
- Koneksi internet aktif.

---

## 🛠️ Langkah-Langkah Instalasi

### 1. Update dan Upgrade Sistem
Pastikan package sistem operasi dalam keadaan terbaru secara paket.
```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Install MySQL Server
Jalankan perintah berikut untuk menginstal package MySQL Server di Linux.
```bash
sudo apt install mysql-server -y
```

### 3. Mengamankan Instalasi (Secure Installation)
Tahap ini untuk memberikan keamanan awal pada MySQL, termasuk root password.
```bash
sudo mysql_secure_installation
```
*Catatan Skenario:*
- Tekan `Y` untuk validasi password atau `N` jika ingin skip (disarankan skip untuk mempermudah lab).
- Buat password root yang mudah diingat untuk akses lokal (misal: `root123`).
- Setujui *Remove anonymous users*, *Disallow root login remotely*, *Remove test database*, dan *Reload privilege tables*.

---

## ⚙️ Konfigurasi Remote Access
Tujuan konfigurasi ini penting agar Web Server (Anggota 2) dapat membuat koneksi menuju Database ini dari jaringan eksternal.

### 1. Ubah Bind-Address MySQL
Secara default, MySQL hanya akan menerima dari koneksi lokal (`127.0.0.1`). Kita harus menggantinya.
```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
Cari baris yang bertuliskan:
```text
bind-address = 127.0.0.1
```
Dan ubah menjadi:
```text
bind-address = 0.0.0.0
```
*Simpan file dengan menekan Ctrl+O, Enter, lalu Ctrl+X.*

### 2. Restart Layanan MySQL
Agar perubahan pada konfigurasi dapat diterapkan:
```bash
sudo systemctl restart mysql
sudo systemctl enable mysql
sudo systemctl status mysql
```

---

## 🗄️ Pembuatan Database dan User

### 1. Login ke MySQL (Sebagai Root)
```bash
sudo mysql -u root -p
```
*(Masukkan password root Anda bila diminta)*

### 2. Buat Database
Membuat database yang nantinya diisi oleh aplikasi.
```sql
CREATE DATABASE db_tugasvcc;
```

### 3. Buat User Remote dan Beri Hak Akses
Buat username dan password yang bebas lalu berikan limitasi IP dari **Web Server (Anggota 2)**, atau `\%` untuk all hosts (agar aman, gunakan IP Anggota 2).
```sql
CREATE USER 'user_db'@'%' IDENTIFIED BY 'passwordrahasia';
GRANT ALL PRIVILEGES ON db_tugasvcc.* TO 'user_db'@'%';
FLUSH PRIVILEGES;
EXIT;
```
*(Catatan: Karakter `%` artinya username 'user_db' dapat login dan diakses dari segala jenis IP. Anda juga bisa memasukkan secara spesifik mengganti `%` dengan `IP_Anggota_2_Web_Server`)*

---

## 🎬 Skenario Dokumentasi Video (Untuk Anggota 3)
Agar dokumentasi video terstruktur dengan baik, pastikan Anggota 3 merekam aktivitas berikut:
1. Menunjukkan instalasi `apt install mysql-server`.
2. Menunjukkan saat melakukan editing config `nano /etc/mysql/mysql.conf.d/mysqld.cnf`, di mana Anda beralih dari `127.0.0.1` ke `0.0.0.0`.
3. Menunjukkan tampilan masuk console MySQL (`mysql>`) saat mengeksekusi `CREATE DATABASE` dan membagikan `PRIVILEGES` ke user yang baru saja dibuat.
4. Tunjukkan menggunakan command `ss -tulpan | grep mysql` pada terminal bahwa port MySQL `3306` sedang listening di state `0.0.0.0`.
