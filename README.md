# UAS Sistem Operasi - Kelompok 7

## 👥 Anggota Kelompok

1. **Tebing Rizky Tsaniansyah** (2410501080)
2. **Heru Chandra** (2410501094)
3. **Muhammad Farrel Fauzan** (2410501092)
4. **Radinka Alifasya Dinova** (2410501073)
5. **Muhammad Ragil Hardika** (2410501103)

## 📝 Deskripsi Project

Project ini adalah implementasi **Private Docker Registry** untuk Ujian Akhir Semester mata kuliah Sistem Operasi. Project ini menggunakan Docker dan Docker Compose untuk menjalankan layanan registry pribadi beserta web UI untuk manajemen images.

## 🏗️ Arsitektur

Project ini terdiri dari beberapa komponen utama:

### 1. Private Docker Registry
- **Image**: `registry:2`
- **Port**: `5000`
- **Fungsi**: Menyimpan dan mengelola Docker images secara lokal

### 2. Registry Web UI
- **Image**: `joxit/docker-registry-ui:main`
- **Port**: `8080`
- **Fungsi**: Interface web untuk visualisasi dan manajemen Docker images
- **Fitur**: 
  - Browse images
  - Delete images
  - View image details dan tags

### 3. Sample Application
- **Teknologi**: Nginx (Alpine)
- **Fungsi**: Aplikasi web sederhana sebagai contoh deployment
- **Konten**: Halaman informasi anggota Kelompok 7

## 📂 Struktur Direktori

```
uas_kelompok7/
├── app/
│   ├── Dockerfile           # Dockerfile untuk sample app
│   └── index.html          # Halaman web kelompok
├── data_registry/          # Volume data untuk registry
│   └── docker/            # Image storage
├── docker-compose.yml      # Konfigurasi semua services
└── registry-config.yml     # Konfigurasi Docker Registry
```

## 🚀 Cara Menjalankan

### Prerequisites
- Docker Desktop terinstall
- Docker Compose terinstall

### Langkah-langkah

1. **Clone atau download repository ini**
   ```bash
   git clone <repository-url>
   cd uas_kelompok7
   ```

2. **Jalankan semua services dengan Docker Compose**
   ```bash
   docker-compose up -d
   ```

3. **Verifikasi services berjalan**
   ```bash
   docker-compose ps
   ```

4. **Akses layanan**
   - **Registry API**: http://localhost:5000
   - **Registry Web UI**: http://localhost:8080

## 🐳 Build dan Push Sample App

### Build Docker Image untuk Sample App

```bash
# Masuk ke direktori app
cd app

# Build image
docker build -t localhost:5000/kelompok7-app:latest .
```

### Push Image ke Private Registry

```bash
# Push image ke registry lokal
docker push localhost:5000/kelompok7-app:latest
```

### Verifikasi Image di Registry

1. Buka browser ke http://localhost:8080
2. Image `kelompok7-app` akan terlihat di UI
3. Atau gunakan API:
   ```bash
   curl http://localhost:5000/v2/_catalog
   ```

## 📋 Perintah Berguna

### Melihat Images di Registry
```bash
# Via API
curl http://localhost:5000/v2/_catalog

# Via browser
# Buka http://localhost:8080
```

### Menjalankan Sample App dari Registry
```bash
docker run -d -p 8000:80 localhost:5000/kelompok7-app:latest
# Akses: http://localhost:8000
```

### Menghentikan Services
```bash
docker-compose down
```

### Menghapus Data Registry (Reset)
```bash
docker-compose down -v
rm -rf data_registry/docker/*
```

### Melihat Logs
```bash
# Semua services
docker-compose logs -f

# Registry saja
docker-compose logs -f registry

# UI saja
docker-compose logs -f registry-ui
```

## ⚙️ Konfigurasi

### Registry Configuration (`registry-config.yml`)

Konfigurasi penting yang telah diatur:

- **Delete enabled**: Mengizinkan penghapusan images
- **CORS headers**: Mengizinkan akses dari Web UI (port 8080)
- **Storage**: Menggunakan filesystem local di `/var/lib/registry`

### Docker Compose Services

#### Registry Service
- Port mapping: `5000:5000`
- Volume: Data disimpan di `./data_registry`
- Auto-restart: `unless-stopped`

#### Registry UI Service
- Port mapping: `8080:80`
- Environment variables:
  - `REGISTRY_TITLE`: "UAS Kelompok 7"
  - `DELETE_IMAGES`: Enabled
  - `SINGLE_REGISTRY`: Enabled

## 🔧 Troubleshooting

### Registry tidak bisa diakses
```bash
# Cek status container
docker-compose ps

# Restart services
docker-compose restart
```

### Error saat push image
```bash
# Pastikan Docker daemon mengizinkan insecure registry
# Edit Docker Desktop settings atau /etc/docker/daemon.json
{
  "insecure-registries": ["localhost:5000"]
}
```

### Web UI tidak menampilkan images
```bash
# Cek CORS configuration di registry-config.yml
# Pastikan Access-Control-Allow-Origin sudah benar
```

## 📚 Teknologi yang Digunakan

- **Docker**: Container platform
- **Docker Compose**: Multi-container orchestration
- **Docker Registry**: Private image storage
- **Nginx**: Web server untuk sample app
- **Joxit Docker Registry UI**: Web interface untuk registry

## 📄 Lisensi

Project ini dibuat untuk keperluan akademik UAS Sistem Operasi.

---

**Made with ❤️ by Kelompok 7**
