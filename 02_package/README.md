# 02: Python Packages for Pyramid Applications

## Output
<img width="837" height="491" alt="image" src="https://github.com/user-attachments/assets/f97cccd1-fdb7-489b-b10c-07d6b6209345" />

## Deskripsi Singkat  
Di langkah ini kita akan membuat aplikasi “Hello World” minimal menggunakan Pyramid, namun dikemas sebagai *Python package* (bukan hanya satu berkas). Ini memperkenalkan struktur proyek yang rapi dan proses instalasi dengan `setup.py`. Merujuk ke dokumentasi: “Most modern Python development is done using Python packages …” :contentReference[oaicite:1]{index=1}

## Latar Belakang  
Seperti dijelaskan dalam dokumentasi:  
> “If a directory is on `sys.path` and has a special file named `__init__.py`, it is treated as a Python package.” :contentReference[oaicite:2]{index=2}  
Di sini kita mengorganisir kode ke dalam paket Python (`tutorial`) dan membuat proyek yang dapat di-install dalam *development mode*. Struktur ini memudahkan pemeliharaan, distribusi, dan penggunaan kembali kode.

## Tujuan  
- Membuat direktori paket Python dengan file `__init__.py`. :contentReference[oaicite:3]{index=3}  
- Membuat proyek Python minima lewat `setup.py`. :contentReference[oaicite:4]{index=4}  
- Menginstal paket dalam mode pengembangan dengan `pip install -e .`. :contentReference[oaicite:5]{index=5}  

## Langkah-langkah  
1. Buat area kerja untuk langkah ini:  
   ```bash
   cd ..  
   mkdir package  
   cd package
   ``` :contentReference[oaicite:6]{index=6}  
2. Buat file `setup.py` di dalam direktori `package/` dengan isi berikut:  
   ```python
   from setuptools import setup

   # List of dependencies installed via `pip install -e .`
   requires = [
       'pyramid',
       'waitress',
   ]

   setup(
       name='tutorial',
       install_requires=requires,
   )
   ``` :contentReference[oaicite:7]{index=7}  
3. Instal proyek dalam mode pengembangan lalu buat direktori kode:  
   ```bash
   $VENV/bin/pip install -e .  
   mkdir tutorial
   ``` :contentReference[oaicite:8]{index=8}  
4. Buat file `package/tutorial/__init__.py` dengan isi:  
   ```python
   # package
   ``` :contentReference[oaicite:9]{index=9}  
5. Buat file `package/tutorial/app.py` dengan isi sebagai berikut:  
   ```python
   from waitress import serve
   from pyramid.config import Configurator
   from pyramid.response import Response

   def hello_world(request):
       print('Incoming request')
       return Response('<body><h1>Hello World!</h1></body>')

   if __name__ == '__main__':
       with Configurator() as config:
           config.add_route('hello', '/')
           config.add_view(hello_world, route_name='hello')
           app = config.make_wsgi_app()
       serve(app, host='0.0.0.0', port=6543)
   ``` :contentReference[oaicite:10]{index=10}  
6. Jalankan aplikasi WSGI dengan perintah:  
   ```bash
   $VENV/bin/python tutorial/app.py
   ``` :contentReference[oaicite:11]{index=11}  
7. Buka di browser: [http://localhost:6543/](http://localhost:6543/) dan Anda akan melihat halaman Hello World. :contentReference[oaicite:12]{index=12}  

## Analisis  
Dengan pendekatan ini kita mendapatkan beberapa keuntungan:  
- Paket Python (`tutorial/`) memungkinkan pengorganisasian modul, file, dan resource dalam satu unit. :contentReference[oaicite:13]{index=13}  
- `setup.py` memungkinkan proyek kita “dipasang” (installed) sehingga modul-nya tersedia di `sys.path`, dan dapat dijalankan atau diuji secara lokal. :contentReference[oaicite:14]{index=14}  
- Instalasi dengan `pip install -e .` membuat paket dalam mode *editable/development*, yang artinya tiap perubahan kode langsung terlihat saat dijalankan; sangat berguna untuk pengembangan aktif. :contentReference[oaicite:15]{index=15}  
- Catatan: menjalankan modul di dalam paket secara langsung (`python tutorial/app.py`) adalah gaya tutorial dan **bukan** praktik terbaik untuk produksi. :contentReference[oaicite:16]{index=16}  

## Output yang Diharapkan  
- Saat menjalankan `python tutorial/app.py`, server WSGI akan berjalan di `0.0.0.0:6543`.  
- Ketika Anda membuka browser ke `http://localhost:6543/`, seharusnya muncul halaman berisi:  
