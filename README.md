# Tutorial Setup & Merekap Docker: Dari Instalasi Hingga Running Container

Ringkasan lengkap dan panduan praktis berdasarkan video tutorial [Kelas Terbuka](http://www.youtube.com/watch?v=t1qdBtJWJWU) (*Install DOCKER, Lengkap Step By Step sampai bisa run Container*).

---

## 📌 Daftar Isi
1. [Prasyarat & Persiapan](#1-prasyarat--persiapan)
2. [Instalasi Docker Desktop di Windows](#2-instalasi-docker-desktop-di-windows)
3. [Konsep Utama: Image vs Container](#3-konsep-utama-image-vs-container)
4. [Tutorial Praktis: Menjalankan Node.js App di Docker](#4-tutorial-praktis-menjalankan-nodejs-app-di-docker)
   - [Langkah 1: Menyiapkan File Aplikasi](#langkah-1-menyiapkan-file-aplikasi)
   - [Langkah 2: Membuat Dockerfile](#langkah-2-membuat-dockerfile)
   - [Langkah 3: Build Docker Image](#langkah-3-build-docker-image)
   - [Langkah 4: Jalankan Docker Container](#langkah-4-jalankan-docker-container)
5. [Optimasi: Menggunakan Image Alpine (Ringan)](#5-optimasi-menggunakan-image-alpine-ringan)
6. [Perintah-Perintah Penting Docker (Cheat Sheet)](#6-perintah-perintah-penting-docker-cheat-sheet)

---

## 1. Prasyarat & Persiapan

Sebelum menginstal Docker Desktop di Windows, pastikan beberapa hal berikut sudah siap:
* **WSL2 (Windows Subsystem for Linux 2):**
  Docker Desktop di Windows disarankan menggunakan backend WSL2 daripada Hyper-V agar performa lebih cepat dan tidak perlu mengubah konfigurasi BIOS `[00:06:02]`. [Tutorial Install WSL 2](https://github.com/adisaputra0/tutorial-docker/tree/wsl2)
* **Akun Docker Hub:**
  Siapkan akun Docker Hub (bisa *sign up* langsung menggunakan akun Google/GitHub) untuk kemudahan *pull* dan *push* image `[00:06:53]`.

---

## 2. Instalasi Docker Desktop di Windows

1. **Unduh Installer:**
   - Buka browser dan kunjungi situs resmi [Docker Desktop](https://www.docker.com/products/docker-desktop/).
   - Unduh installer sesuai sistem operasi Anda (Windows / macOS / Linux) `[00:01:04]`.
2. **Jalankan Instalasi:**
   - Buka file installer yang telah diunduh.
   - Pada opsi konfigurasi, pastikan opsi **"Use WSL 2 instead of Hyper-V"** dicentang `[00:06:02]`.
   - Tunggu proses *unpacking* dan instalasi hingga selesai.
3. **Restart Komputer:**
   - Setelah instalasi selesai, klik tombol **Close and Restart** untuk mereboot sistem `[00:06:24]`.
4. **First Launch & Sign In:**
   - Setelah sistem menyala kembali, terima *Terms and Agreement*.
   - Lakukan *Sign In* menggunakan akun Docker Hub (atau via Google) pada Docker Desktop UI `[00:06:53]`.

---

## 3. Konsep Utama: Image vs Container

| Konsep | Penjelasan | Analogian |
| :--- | :--- | :--- |
| **Docker Image** | Cetak biru (*blueprint*), template, atau paket terisolasi yang berisi kode program, *runtime*, *environment*, dan *library* yang dibutuhkan. Bersifat *read-only*. | Resep makanan / Desain Rumah |
| **Docker Container** | Instance aktif/berjalan dari sebuah Docker Image. Lingkungan terisolasi tempat aplikasi dieksekusi secara nyata `[00:17:46]`. | Makanan jadi / Rumah hasil bangunan |

---

## 4. Tutorial Praktis: Menjalankan Node.js App di Docker

Berikut langkah demi langkah membuat web server sederhana Node.js, mengemasnya menjadi Docker Image, lalu menjalankannya dalam Docker Container.

### Langkah 1: Menyiapkan File Aplikasi
Buat sebuah folder project baru (misal: `my-node-app`) dan buat file bernama `index.js`:

```javascript
// index.js
const http = require('http');

const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World dari Docker Container!\n');
});

server.listen(port, () => {
  console.log(`Server berjalan di http://localhost:${port}/`);
});
```

### Langkah 2: Membuat Dockerfile

Buat file bernama `Dockerfile` (tanpa ekstensi file) di direktori yang sama:

```dockerfile
# Menggunakan base image Node.js versi resmi
FROM node:latest

# Menentukan working directory di dalam container
WORKDIR /app

# Menyalin file aplikasi lokal ke dalam container
COPY index.js .

# Membuka port 3000 container
EXPOSE 3000

# Perintah untuk menjalankan aplikasi
CMD ["node", "index.js"]
```

### Langkah 3: Build Docker Image

Buka terminal/command prompt di direktori project tersebut, lalu jalankan perintah build:

```bash
docker build -t node-app .
```

> **Keterangan:**
> * `-t node-app`: Memberi nama/tag pada image dengan nama `node-app`.
> * `.`: Menandakan bahwa Dockerfile berada di direktori saat ini.

### Langkah 4: Jalankan Docker Container

Setelah proses build selesai, jalankan container dengan melakukan *port forwarding*:

```bash
docker run -p 3000:3000 node-app
```

> **Keterangan:**
> * `-p 3000:3000`: Meneruskan traffic dari port `3000` di komputer lokal (host) ke port `3000` di dalam container `[00:19:48]`.

Buka browser dan akses `http://localhost:3000`. Anda akan melihat output **"Hello World dari Docker Container!"** `[00:17:31]`.

---

## 5. Optimasi: Menggunakan Image Alpine (Ringan)

Base image `node:latest` biasanya berukuran cukup besar. Untuk efisiensi resource dan kecepatan deployment, gunakan varian **Alpine Linux** `[00:18:28]`.

Ubah isi `Dockerfile` menjadi:

```dockerfile
# Menggunakan base image Node.js versi Alpine yang sangat ringan
FROM node:alpine

WORKDIR /app

COPY index.js .

EXPOSE 3000

CMD ["node", "index.js"]
```

Build ulang image dengan nama baru:

```bash
docker build -t node-app:alpine .
```

Buat dan jalankan container menggunakan image alpine:

```bash
docker run -p 3000:3000 node-app:alpine
```

Sebelum buat dan jalankan container menggunakan image alpine, lebih baik buat namanya dulu:

```bash
docker run -d -p 3000:3000 --name my-node-app node-app:alpine
```

Saat di stop dan ingin jalankan container yang sudah ada:

```bash
docker start nama_container
```

Kalau mau cek logs:

```bash
docker logs nama_container
```

---

## 6. Perintah-Perintah Penting Docker (Cheat Sheet)

### 🔹 Manajemen Container

* **Melihat container yang sedang berjalan:**
```bash
docker ps
```

* **Melihat seluruh container (termasuk yang mati/stopped):**
```bash
docker ps -a
```

* **Menghentikan container:**
```bash
docker stop <CONTAINER_ID_atau_NAMA>
```

* **Menghapus container:**
```bash
docker rm <CONTAINER_ID_atau_NAMA>
```

### 🔹 Manajemen Image

* **Melihat daftar Docker Image lokal:**
```bash
docker images
```

* **Menghapus Docker Image:**
```bash
docker rmi <IMAGE_ID_atau_NAMA>
```

---

*Disusun berdasarkan video tutorial [Kelas Terbuka - Install DOCKER, Lengkap Step By Step sampai bisa run Container](http://www.youtube.com/watch?v=t1qdBtJWJWU).*
