# File Tambahan: Koneksi dan Integrasi Sistem (Final)

Dokumen ini wajib dijalankan secara bersama-sama oleh **Anggota 1, Anggota 2, dan Anggota 3** untuk mendemonstrasikan bahwa ketiga buah server yang berjalan saling terintegrasi satu sama lainnya dengan sukses untuk menyelesaikan Tugas VCC.

---

## 🔗 Skenario 1: Menghubungkan DNS (Anggota 1) ke Web Server (Anggota 2)

### 1. Pengujian DNS dari Internet
Sebelumnya, *Anggota 1* sudah membuat DNS A Record pada panel VestaCP di mana domain (misal `tugasvcc.xyz`) mengarah ke IP Server milik *Anggota 2*.
- Di PC Lokal (Windows/Linux Anda), buka terminal/Command Prompt.
- Jalankan pengecekan PING pada terminal Anda.
```cmd
ping tugasvcc.xyz
```
*Pastikan respons balasan berupa "Reply from..." memunculkan alamat IP dari Server Anggota 2.* 

*(Catatan: Anda juga bisa membypass file hosts lokal di PC Anda jika Domain tidak secara publik disiarkan di nameserver. Cukup edit `C:\Windows\System32\drivers\etc\hosts` di Windows).*

### 2. Mengakses Website via Domain
Buka Browser, ketik `http://tugasvcc.xyz`.
Jika layar memunculkan Default Web Page Apache atau hasil eksekusi dari File PHP, maka komunikasi DNS dengan Web Server telah berhasil 100%.

---

## 🤝 Skenario 2: Menghubungkan Web Server (Anggota 2) ke Database Server (Anggota 3)

Pada tahap ini kita akan membuktikan bahwa skrip PHP yang di hosting di Web Server berhasil tersambung ke MySQL Engine yang ada di Database Server, dan menampilkan notifikasi sukses.

### 1. Membuat Skrip Koneksi Data (Tugas Anggota 2)
Di terminal Web Server, jalankan:
```bash
sudo nano /var/www/html/koneksidb.php
```

### 2. Isi Berkas `koneksidb.php`
Silakan ubah variabel pada code PHP ini agar sesuai dengan kredensial yang dibuat oleh *Anggota 3*.
```php
<?php
// Ganti dengan IP Address dari Database Server (Anggota 3)
$host = "IP_DATABASE_SERVER"; 

// Ganti sesuai dengan yg diatur di MySQL (Anggota 3)
$user = "user_db"; 
$password = "passwordrahasia"; 
$database = "db_tugasvcc"; 

// Mencoba membangun Koneksi
$koneksi = new mysqli($host, $user, $password, $database);

// Validasi Koneksi
if ($koneksi->connect_error) {
    die("<h1 style='color:red;'>Oops! Koneksi ke Database Server Gagal: " . $koneksi->connect_error . "</h1>");
} else {
    echo "<h1 style='color:green;'>Selamat! Koneksi Web Server ke Database Server (MySQL) Berhasil!</h1>";
}
?>
```
*Simpan (Ctrl+O, Enter, Ctrl+X).*

---

## 🎬 Skenario Akhir Penutup Video Tugas

Rekam ini pada akhir durasi presentasi/video dokumentasi YouTube kelompok Anda:
1. Rekam browser dari PC Lokal / PC Host Anggota Kelompok.
2. Arahkan URL Browser langsung menuju **Domain DNS yang telah ditambahkan** seperti ini: `http://tugasvcc.xyz/koneksidb.php`.
3. Tunjukkan layar ketika laman memampang tulisan teks berwarna hijau **"Selamat! Koneksi Web Server ke Database Server (MySQL) Berhasil!"**.
4. Skenario Penutup: Video ini telah merepresentasikan kemampuan ketiga *node* (DNS, Web, & Database) untuk berjalan dengan terdistribusi secara harmonis dan mendemonstrasikan suksesnya final deployment arsitektur ini.
