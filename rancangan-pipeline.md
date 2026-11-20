# Rancangan CI/CD Pipeline
**Nama Proyek:** Aplikasi Toko Online Sederhana (Web E-Commerce)

---

## 1. Trigger Pipeline
Pipeline akan berjalan secara otomatis berdasarkan aturan pemicu berikut:
* **Branch Target:** `master` dan `develop`
* **Event Pemicu:** 
  * `push` (saat ada pembaruan kode yang di-*push* ke branch `master`)
  * `pull_request` (saat ada pengajuan penggabungan kode ke branch `master`)

---

## 2. Tahapan (Stages) Pipeline

### A. Source Control
* **Tujuan:** Mengambil kode sumber terbaru dari repositori ke dalam lingkungan runner.
* **Alat / Perintah:** Git & GitHub Actions (`actions/checkout@v4`)
  ```bash
  git clone https://github.com
  git checkout master
  ```

### B. Build
* **Tujuan:** Menginstal dependensi dan mengompilasi kode aplikasi toko online agar siap dijalankan.
* **Alat / Perintah:** Node.js & NPM
  ```bash
  npm install
  npm run build
  ```

### C. Test
* **Tujuan:** Memastikan seluruh fitur aplikasi (seperti keranjang belanja dan pembayaran) berjalan tanpa *bug*.
* **Alat / Perintah:** Jest / Vitest
  ```bash
  npm run test
  ```

### D. Security Scanning
* **Tujuan:** Memeriksa celah keamanan pada dependensi pustaka dan kode sumber aplikasi.
* **Alat / Perintah:** NPM Audit & Trivy
  ```bash
  npm audit --audit-level=high
  trivy fs .
  ```

### E. Artifact Generation
* **Tujuan:** Membungkus hasil *build* aplikasi menjadi paket siap rilis (*artifact* atau *container image*).
* **Alat / Perintah:** Docker / GitHub Actions (`actions/upload-artifact@v4`)
  ```bash
  docker build -t toko-online-app:latest .
  ```

### F. Deployment
* **Tujuan:** Menerapkan (*deploy*) aplikasi versi terbaru ke server produksi agar dapat diakses pengguna.
* **Alat / Perintah:** Docker Compose / SSH Deployment
  ```bash
  docker compose up -d
  ```
