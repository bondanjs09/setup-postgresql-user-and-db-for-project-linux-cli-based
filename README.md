# Setup Database PostgreSQL (CLI Linux/Ubuntu)

## Langkah-langkah

1.  Pastikan PostgreSQL sudah terinstal di Ubuntu.

2.  Masuk ke interface PostgreSQL sebagai user `postgres`:

```{=bash}
```
    sudo -u postgres psql

3.  Buat user untuk proyek:

```{=sql}
```
    CREATE USER <user> WITH PASSWORD '<password>';

4.  Berikan hak akses user untuk membuat database:

```{=sql}
```
    ALTER USER <user> CREATEDB;

5.  Buat database proyek:

```{=sql}
```
    CREATE DATABASE <nama_database>;

6.  Berikan hak akses database ke user:

```{=sql}
```
    GRANT ALL PRIVILEGES ON DATABASE <nama_database> TO <user>;

7.  Keluar dari psql:

```{=bash}
```
    \q

8.  Edit file konfigurasi PostgreSQL:

```{=bash}
```
    sudo nano /etc/postgresql/<versi_postgresql>/main/pg_hba.conf

9.  Cari baris berikut:

```{=html}
```
    local   all             all                                     peer

10. Ubah `peer` menjadi:

```{=html}
```
    md5

11. Simpan file dan keluar:

-   Tekan `Ctrl + X`
-   Tekan `Y`
-   Tekan `Enter`

12. Restart service PostgreSQL:

```{=bash}
```
    sudo systemctl restart postgresql

13. Tes koneksi:

```{=bash}
```
    psql -U <user> -d <nama_database> -W

------------------------------------------------------------------------

## Catatan

-   Gunakan password yang kuat untuk keamanan.
-   Untuk PostgreSQL versi baru, bisa gunakan `scram-sha-256` sebagai
    pengganti `md5`.
