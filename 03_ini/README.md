# 03: Application Configuration with `.ini` Files

## Output
<img width="638" height="372" alt="image" src="https://github.com/user-attachments/assets/35c39be3-96ec-40dd-806c-9db865509eef" />

## Deskripsi Singkat  
Pada langkah ini kita akan memindahkan konfigurasi aplikasi ke dalam berkas `.ini`, serta memperkenalkan cara menjalankan aplikasi melalui perintah `pserve`. Ini memungkinkan pemisahan konfigurasi dari kode dan penggunaan paket Python yang telah di-install. Dokumentasi menyebutkan bahwa “Use Pyramid’s `pserve` command with a `.ini` configuration file for simpler, better application running.” :contentReference[oaicite:1]{index=1}

## Latar Belakang  
Dalam Pyramid, konfigurasi aplikasi tidak harus tertanam dalam kode — ada konsep yang kuat tentang konfigurasi terpisah (melalui file `.ini`) yang membuat manajemen aplikasi menjadi lebih rapi. :contentReference[oaicite:2]{index=2}  
Langkah-langkah ini akan mengubah `setup.py` agar mendefinisikan entry point, membuat file `.ini` yang akan digunakan oleh server (melalui `pserve`), dan memindahkan titik masuk aplikasi ke dalam `__init__.py`. :contentReference[oaicite:3]{index=3}

## Tujuan  
- Memodifikasi file `setup.py` untuk mencantumkan *entry point* yang menunjukkan lokasi fungsi utama WSGI app. :contentReference[oaicite:4]{index=4}  
- Membuat aplikasi yang dikonfigurasi melalui file `.ini`. :contentReference[oaicite:5]{index=5}  
- Menjalankan aplikasi melalui `pserve` menggunakan file `.ini`. :contentReference[oaicite:6]{index=6}  
- Memindahkan kode startup aplikasi ke paket Python (`__init__.py`). :contentReference[oaicite:7]{index=7}

## Langkah-langkah  
1. Salin hasil dari langkah sebelumnya ke direktori baru:  
   ```bash
   cd ..  
   cp -r package ini  
   cd ini
   ``` :contentReference[oaicite:8]{index=8}  
2. Ubah file `ini/setup.py` agar memiliki entry point seperti berikut:  
   ```python
   from setuptools import setup

   requires = [
       'pyramid',
       'waitress',
   ]

   setup(
       name='tutorial',
       install_requires=requires,
       entry_points={
           'paste.app_factory': [
               'main = tutorial:main'
           ],
       },
   )
   ``` :contentReference[oaicite:9]{index=9}  
3. Instal proyek dalam mode pengembangan sehingga paket-nya tersedia sebagai egg:  
   ```bash
   $VENV/bin/pip install -e .
   ``` :contentReference[oaicite:10]{index=10}  
4. Buat file `ini/development.ini` dengan isi seperti:  
   ```ini
   [app:main]
   use = egg:tutorial

   [server:main]
   use = egg:waitress#main
   listen = localhost:6543
   ``` :contentReference[oaicite:11]{index=11}  
5. Refaktor kode startup dari `app.py` ke `ini/tutorial/__init__.py`, misalnya:  
   ```python
   from pyramid.config import Configurator
   from pyramid.response import Response

   def hello_world(request):
       return Response('<body><h1>Hello World!</h1></body>')

   def main(global_config, **settings):
       config = Configurator(settings=settings)
       config.add_route('hello', '/')
       config.add_view(hello_world, route_name='hello')
       return config.make_wsgi_app()
   ``` :contentReference[oaicite:12]{index=12}  
6. Jalankan aplikasi dengan perintah:  
   ```bash
   $VENV/bin/pserve development.ini --reload
   ``` :contentReference[oaicite:13]{index=13}  
7. Buka browser ke [http://localhost:6543/](http://localhost:6543/) dan seharusnya menampilkan halaman “Hello World!”. :contentReference[oaicite:14]{index=14}  

## Analisis  
- File `.ini` `development.ini` dibaca oleh `pserve` dan berfungsi sebagai titik awal pengaturan aplikasi. :contentReference[oaicite:15]{index=15}  
- `pserve` mencari bagian `[app:main]` dalam `.ini`, dan menemukan `use = egg:tutorial` — yang berarti paket ‘tutorial’ memiliki fungsi `main` sebagai entry point. :contentReference[oaicite:16]{index=16}  
- Bagian `[server:main]` dalam `.ini` menentukan server WSGI yang akan digunakan dan parameter seperti `listen` alamat–port. :contentReference[oaicite:17]{index=17}  
- Dengan memindahkan kode bootstrap ke paket (`__init__.py`), kita menjaga struktur aplikasi tetap modular dan terpisah dari file standalone seperti `app.py`. :contentReference[oaicite:18]{index=18}  
- Mode `--reload` sangat berguna saat pengembangan karena memantau perubahan kode atau konfigurasi dan melakukan restart otomatis. :contentReference[oaicite:19]{index=19}  

## Output yang Diharapkan  
- Setelah menjalankan `pserve development.ini --reload`, server WSGI berjalan di alamat `localhost:6543`.  
- Saat Anda membuka browser di `http://localhost:6543/`, akan muncul halaman yang menampilkan:  
  ```html
  <body><h1>Hello World!</h1></body>
