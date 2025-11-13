# 04: Easier Development with `debugtoolbar`

## Output
<img width="2068" height="630" alt="image" src="https://github.com/user-attachments/assets/2726657e-bc4c-44b4-b349-163f2ae93682" />

## Deskripsi Singkat  
Pada langkah ini kita akan menambahkan add-on pyramid_debugtoolbar ke aplikasi Pyramid kita untuk memudahkan pengembangan dan debugging. Toolbar ini akan muncul pada antarmuka aplikasi di browser, menyediakan berbagai alat introspeksi dan penanganan kesalahan yang interaktif. Dari dokumentasi:  
> “`pyramid_debugtoolbar` is a popular Pyramid add-on which makes several tools available in your browser.” :contentReference[oaicite:2]{index=2}

## Latar Belakang  
Saat mengembangkan aplikasi web, kesalahan dan kebutuhan introspeksi sangat sering muncul — kita ingin melihat alur request, response, context, variable, dan stacktrace secara mudah. Add-on debugtoolbar menyediakan semua itu dalam antarmuka browser yang ramah pengembang. :contentReference[oaicite:3]{index=3}  
Dengan menambahkan add-on ini, kita juga belajar bagaimana konfigurasi add-on di proyek Pyramid, serta bagaimana menggunakan fitur “extras_require” di `setup.py` untuk dependensi pengembangan.

## Tujuan  
- Menambahkan `pyramid_debugtoolbar` sebagai dependensi **pengembangan** di `setup.py`. :contentReference[oaicite:4]{index=4}  
- Mengonfigurasi `.ini` file agar menyertakan `pyramid_debugtoolbar` melalui `pyramid.includes`. :contentReference[oaicite:5]{index=5}  
- Menjalankan aplikasi dengan toolbar aktif dan mengobservasi fitur-fiturnya seperti tampilan toolbar di browser dan stacktrace interaktif.

## Langkah-langkah  
1. Salin hasil dari langkah sebelumnya (langkah 03) ke direktori baru misalnya:  
   ```bash
   cd ..; cp -r ini debugtoolbar; cd debugtoolbar
   ``` :contentReference[oaicite:6]{index=6}  
2. Tambahkan `pyramid_debugtoolbar` ke dalam `setup.py` sebagai bagian dari extras pengembangan:  
   ```python
   from setuptools import setup

   requires = [
       'pyramid',
       'waitress',
   ]

   dev_requires = [
       'pyramid_debugtoolbar',
   ]

   setup(
       name='tutorial',
       install_requires=requires,
       extras_require={
           'dev': dev_requires,
       },
       entry_points={
           'paste.app_factory': [
               'main = tutorial:main'
           ],
       },
   )
   ``` :contentReference[oaicite:7]{index=7}  
3. Instal proyek dalam mode editable dengan dependensi dev:  
   ```bash
   $VENV/bin/pip install -e ".[dev]"
   ``` :contentReference[oaicite:8]{index=8}  
4. Ubah `development.ini` supaya menyertakan `pyramid_debugtoolbar` dalam `pyramid.includes`:  
   ```ini
   [app:main]
   use = egg:tutorial
   pyramid.includes =
       pyramid_debugtoolbar

   [server:main]
   use = egg:waitress#main
   listen = localhost:6543
   ``` :contentReference[oaicite:9]{index=9}  
5. Jalankan aplikasi dengan menggunakan `pserve --reload`:  
   ```bash
   $VENV/bin/pserve development.ini --reload
   ``` :contentReference[oaicite:10]{index=10}  
6. Buka browser ke `http://localhost:6543/` dan Anda akan melihat toolbar kecil di sudut kanan atas (atau serupa) yang berfungsi untuk debugging. :contentReference[oaicite:11]{index=11}  

## Analisis  
- `pyramid_debugtoolbar` adalah paket Python penuh yang bisa di-install via pip, sama seperti paket lainnya. :contentReference[oaicite:12]{index=12}  
- Karena ini adalah add-on untuk Pyramid, kita harus memasukkan konfigurasinya ke dalam aplikasi; hal ini bisa dilakukan secara imperatif di kode (`config.include('pyramid_debugtoolbar')`) atau secara deklaratif lewat `.ini` file menggunakan `pyramid.includes`. Tutorial ini memilih opsi `.ini`. :contentReference[oaicite:13]{index=13}  
- Dengan toolbar ini aktif, Anda akan melihat tombol “floating” pada halaman HTML Anda yang membuka antarmuka debugging untuk request tersebut, serta halaman traceback yang lebih baik bila terjadi exception. :contentReference[oaicite:14]{index=14}  
- Karena toolbar inject sedikit HTML/CSS–nya (menyisipkan dirinya sebelum `</body>`), jika Anda mengalami perilaku aneh di sisi klien, Anda bisa menonaktifkannya dengan menghapus `pyramid_debugtoolbar` dari `pyramid.includes` tanpa mengubah kode. :contentReference[oaicite:15]{index=15}  
- Konsep “extras_require” di `setup.py` memungkinkan kita memisahkan dependensi yang hanya dibutuhkan untuk pengembangan (`dev`) dari dependensi runtime. Hal ini menjadikan package kita lebih bersih untuk produksi. :contentReference[oaicite:16]{index=16}  

## Output yang Diharapkan  
- Setelah menjalankan `pserve development.ini --reload`, server WSGI akan berjalan pada `localhost:6543`.  
- Ketika Anda membuka `http://localhost:6543/`, selain halaman utama aplikasi, Anda akan melihat sebuah tombol atau elemen toolbar di pojok kanan atas (atau sesuai tema) milik debugtoolbar.  
- Bila Anda mengakses URL yang menyebabkan exception, toolbar akan menampilkan halaman traceback yang interaktif — Anda bisa mengeksplor variabel lokal, menjalankan ekspresi Python di konteks stacktrace. :contentReference[oaicite:17]{index=17}  
- Toolbar juga memungkinkan Anda melihat panel-panel seperti versi perangkat lunak, setting aplikasi, request/response headers, variabel request, renderings, dan lainnya. :contentReference[oaicite:18]{index=18}  

## Penutup  
Dengan langkah ini Anda memperoleh alat pengembangan yang sangat membantu — bukan hanya untuk debugging ketika ada kesalahan, tetapi juga untuk memahami aliran request/response, view, konfigurasi, dan lingkungan aplikasi Anda. Sebagai catatan penting: jangan aktifkan toolbar ini di lingkungan produksi atau pada server yang portnya terbuka ke internet, karena ini dapat mengekspos eksekusi kode ke pihak asing. :contentReference[oaicite:19]{index=19}  
Selamat mencoba! 🚀  
