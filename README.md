# Setup Database PostgreSQL (CLI Linux/Ubuntu)

## Langkah-langkah

1.  Pastikan PostgreSQL sudah terinstal di Ubuntu.

2.  Masuk ke interface PostgreSQL sebagai user `postgres`:

```bash
    sudo -u postgres psql
```

3.  Buat user role:

```sql
CREATE USER <nama_user> WITH PASSWORD '<password>';
```

Opsional (umumnya diperlukan untuk aplikasi backend) :

```sql
ALTER USER <nama_user> WITH LOGIN;
```

4.  Berikan hak akses user untuk membuat database:

```sql
    ALTER USER <user> CREATEDB;
```

5.  Buat database dengan owner:

```sql
CREATE DATABASE <nama_database>
OWNER <nama_user>
ENCODING 'UTF8';
```

6. Koneksi ke database

```bash
\c <nama_database>
```

7. Buat schema khusus

```sql
CREATE SCHEMA <nama_schema> AUTHORIZATION <nama_user>;
```

8. Set default schema

```sql
ALTER ROLE <nama_user> SET search_path TO <nama_schema>;
```

9. Amankan schema public

```sql
REVOKE ALL ON SCHEMA public FROM PUBLIC;
REVOKE ALL ON SCHEMA public FROM <nama_user>;
```

10. Grant privilege yang eksplisit

```sql
GRANT ALL PRIVILEGES ON DATABASE <nama_database> TO <nama_user>;
GRANT ALL PRIVILEGES ON SCHEMA <nama_schema> TO <nama_user>;
```

11. Keluar dari interface superuser

```bash
\q
```

12. Masuk ke dalam database dengan user yang sudah dibuat :

```bash
psql -U <nama_user> -d <nama_database> -h localhost
```

13. Tes dengan membuat tabel :

```sql
CREATE TABLE test_table (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);
```

14. Pastikan tidak ada error

## Menangani Error Saat Login 

1.  Edit file konfigurasi PostgreSQL:

```bash
    sudo nano /etc/postgresql/<versi_postgresql>/main/pg_hba.conf
```

2.  Cari baris berikut:

```html
    local   all             all                                     peer
```

3. Ubah `peer` menjadi:

```html
    md5
```

4. Simpan file dan keluar:

-   Tekan `Ctrl + X`
-   Tekan `Y`
-   Tekan `Enter`

5. Restart service PostgreSQL:

```bash
    sudo systemctl restart postgresql
```

6. Tes koneksi:

```bash
    psql -U <user> -d <nama_database> -W
```

------------------------------------------------------------------------

## Catatan

-   Gunakan password yang kuat untuk keamanan.
-   Untuk PostgreSQL versi baru, bisa gunakan `scram-sha-256` sebagai
    pengganti `md5`.
