# 🚀 Guide Setup WSL 2 (Ubuntu) di Windows

Dokumentasi ringkas untuk menginstal Windows Subsystem for Linux 2 (WSL 2) menggunakan distro Ubuntu sebagai fondasi environment Linux di Windows.

Ringkasan berdasarkan video tutorial **Kelas Terbuka** — bagian *Setup WSL 2 + Ubuntu sampai Update/Upgrade*.

---

## 📋 Prasyarat

* OS Windows 10 (versi 2004 ke atas) atau Windows 11.
* Koneksi internet untuk mengunduh komponen WSL dan distro Ubuntu.

---

## 🛠️ Langkah-Langkah Instalasi

### 1. Buka Windows Terminal / PowerShell
Wajib jalankan sebagai **Administrator**.

Cari **Terminal** atau **PowerShell** di Search Bar Windows, klik kanan lalu pilih **Run as administrator**.

### 2. Jalankan Perintah Instalasi WSL
Proses download dan aktifkan fitur virtualisasi.

Ketikkan perintah berikut untuk menginstal WSL beserta distro Ubuntu:

```bash
wsl --install -d Ubuntu
```

Proses ini akan mengunduh kernel Linux, mengaktifkan fitur *Virtual Machine Platform*, dan memasang Ubuntu.

### 3. Restart Komputer / Laptop
Dibutuhkan jika WSL baru pertama kali diaktifkan.

Jika diminta oleh sistem (*"Reboot is required..."*), lakukan **Restart** pada laptop/PC kamu agar fitur virtualisasi Windows aktif sepenuhnya.

### 4. Buat Account UNIX (Username & Password)
Selesaikan inisialisasi Ubuntu.

Setelah restart, aplikasi Ubuntu akan terbuka secara otomatis (atau bisa dibuka manual via Terminal). Masukkan detail pengguna Linux kamu:

1. `Enter new UNIX username:` → `nama_kamu` (gunakan huruf kecil, tanpa spasi)
2. `New password:` → `masukkan_password` (catatan: karakter password memang tidak akan kelihatan saat diketik)
3. `Retype new password:` → `masukkan_password_lagi`

### 5. Update & Upgrade System Repository
Memastikan paket Linux terbaru.

Setelah berhasil masuk ke terminal Ubuntu (`nama_kamu@nama_pc:~$`), jalankan perintah update & upgrade repository:

```bash
sudo apt update && sudo apt upgrade -y
```

Masukkan password UNIX kamu jika diminta.

---

## ✅ Selesai!

Distro Ubuntu WSL 2 kamu sudah aktif dan siap digunakan.

---

## 💡 Langkah Selanjutnya (Khusus Workflow Docker)

Kamu **tidak perlu** menginstal Node.js/NVM di dalam Ubuntu ini. Cukup buka **Docker Desktop** di Windows, lalu pastikan opsi **WSL 2 Integration** di menu:

```
Settings > Resources > WSL Integration
```

sudah mengaktifkan distro Ubuntu kamu.

---

Cek Status WSL
```bash
wsl -l -v
```

Menjalankan WSL, Gunakan perintah berikut di Terminal:
```bash
wsl
```
Atau klik tanda panah kecil di sebelah tombol + pada Windows Terminal / PowerShell, lalu pilih Ubuntu.

Menghentikan WSL
```bash
wsl --shutdown
```
