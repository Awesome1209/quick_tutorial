## 🗂️ Struktur Proyek

* `/01_hello_world`: Aplikasi "Hello World!" file tunggal.

## Output
<img width="837" height="491" alt="image" src="https://github.com/user-attachments/assets/12c41d65-c31f-4d21-8fe4-bdfb9d305397" />

---

## 🚀 Panduan Menjalankan (Windows PowerShell)

Panduan ini berasumsi Anda menggunakan **PowerShell** di Windows, dan *virtual environment* Anda bernama `env`.

### 1. Pengaturan Awal (Hanya Dilakukan Sekali)

1.  **Buka Terminal di Root Proyek**
    Buka PowerShell Anda di folder `C:\projects\quick_tutorial`.

2.  **Buat Virtual Environment (Jika Belum Ada)**
    Jika folder `env` belum ada, jalankan:
    ```powershell
    python -m venv env
    ```

3.  **Aktifkan Virtual Environment**
    Setiap kali Anda membuka terminal **baru** untuk proyek ini, Anda harus mengaktifkan `env` Anda:
    ```powershell
    .\env\Scripts\Activate.ps1
    ```
    *Prompt* Anda akan berubah dan menampilkan `(env)` di depannya.

4.  **Instal Dependensi Inti**
    Setelah `(env)` aktif, instal Pyramid dan dependensi lainnya:
    ```bash
    pip install "pyramid<2.0" jinja2
    ```

---

### 2. Menjalankan Setiap Langkah Tutorial

Untuk setiap folder (misalnya `02_package`, `03_ini`, dst.), prosesnya hampir selalu sama:

1.  **Pastikan `(env)` Anda Aktif.**
    Jika tidak, jalankan `.\env\Scripts\Activate.ps1` dari folder root.

2.  **Masuk ke Folder Langkah (Contoh: 03_ini)**
    ```powershell
    cd 03_ini
    ```

3.  **Instal Proyek (Mode Editable)**
    Langkah ini "menghubungkan" kode di folder ini ke `env` Anda. **Ini wajib** untuk sebagian besar langkah (mulai dari 02 ke atas) agar `import` dan `pserve` berfungsi.
    ```bash
    # (env) PS C:\...\03_ini>
    pip install -e .
    ```

4.  **Jalankan Server Aplikasi**
    Sebagian besar tutorial (dari langkah 03 ke atas) dijalankan menggunakan `pserve`:
    ```bash
    # (env) PS C:\...\03_ini>
    pserve development.ini --reload
    ```
    Server akan berjalan di `http://localhost:6543`.

> **Pengecualian Langkah 01:**
> Untuk `01_hello_world`, Anda tidak perlu `pip install`. Anda cukup menjalankannya dengan:
> ```bash
> (env) PS C:\...\01_hello_world> python app.py
> ```

---

### 3. 🧪 Menjalankan Tes (Langkah 05 & 06)

Untuk menjalankan tes pada folder `05_unit_testing` atau `06_functional_testing`:

1.  **Pastikan `(env)` Anda Aktif.**

2.  **Masuk ke Folder Tes**
    ```powershell
    cd 05_unit_testing
    ```

3.  **Instal Proyek dengan Dependensi "Testing"**
    Ini akan menginstal `pytest` dan dependensi tes lainnya yang ditentukan di `setup.py`.
    ```bash
    # (env) PS C:\...\05_unit_testing>
    pip install -e ".[testing]"
    ```

4.  **Jalankan `pytest`**
    * Cara standar adalah dengan mengetik `pytest`:
        ```bash
        pytest
        ```
    * **PENTING:** Jika `pytest` melaporkan `collected 0 items`, itu karena nama file Anda adalah `tutorial/tests.py` (bukan `test_*.py`). Dalam kasus ini, jalankan `pytest` dengan menunjuk file secara spesifik:
        ```bash
        # Ini adalah perintah yang benar untuk file tes Anda
        pytest tutorial/tests.py -q
        ```
