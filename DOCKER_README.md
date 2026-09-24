# Menjalankan Backend Laravel via Docker (Local Development)

## Prasyarat
- Docker Desktop terinstall
- Project Laravel sudah ada di root folder ini (atau jalankan `composer create-project laravel/laravel .` dulu kalau masih kosong)

## Langkah setup pertama kali

1. Salin file environment:
   ```bash
   cp .env.example .env
   ```

2. Build dan jalankan seluruh container:
   ```bash
   docker compose up -d --build
   ```

3. Generate APP_KEY (wajib untuk Laravel):
   ```bash
   docker compose exec app php artisan key:generate
   ```

4. Jalankan migration:
   ```bash
   docker compose exec app php artisan migrate
   ```

5. Akses aplikasi di `http://localhost:8080`

## Perintah umum sehari-hari

```bash
# Masuk ke shell container app
docker compose exec app sh

# Jalankan artisan command apapun
docker compose exec app php artisan <command>

# Lihat log
docker compose logs -f app

# Composer install (kalau nambah package)
docker compose exec app composer install

# Matikan semua container
docker compose down

# Matikan + hapus volume database (reset total data lokal)
docker compose down -v
```

## Catatan penting

- Service `queue-worker` otomatis memproses job queue (misal kirim email async). Kalau job tidak jalan, cek log dengan `docker compose logs -f queue-worker`.
- Database dan Redis di sini murni untuk **local development**. Saat deploy ke production di Oracle Cloud VM, `DB_HOST` dan `REDIS_HOST` di `.env` production diarahkan ke Neon dan Upstash (lihat komentar di `.env.example`), bukan ke service `db`/`redis` di compose ini.
- Kalau baru mulai dari nol dan folder Laravel belum ada, jalankan dulu:
  ```bash
  docker run --rm -v $(pwd):/app composer:2 create-project laravel/laravel .
  ```
  baru lanjut ke langkah setup di atas.
