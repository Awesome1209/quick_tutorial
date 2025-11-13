# 07: Basic Web Handling With Views

## Deskripsi Singkat  
Pada langkah ini kita akan memindahkan definisi view-function ke modul `views.py`, menggunakan decorator `@view_config` untuk mendeklarasikan route dan view secara deklaratif. Di :contentReference[oaicite:1]{index=1} dijelaskan bahwa:  
> “In Pyramid, views are the primary way to accept web requests and return responses.” :contentReference[oaicite:2]{index=2}  
Kita akan membuat dua view: satu untuk route `'home'` (`/`) dan satu untuk route `'hello'` (`/howdy`).  

## Output
<img width="2560" height="570" alt="image" src="https://github.com/user-attachments/assets/e4e829c8-33a9-4197-84a0-b34aaa10080a" />

## Tujuan  
- Memindahkan view ke modul terpisah (`tutorial/views.py`). :contentReference[oaicite:3]{index=3}  
- Mendeklarasikan route-name baru dalam file `__init__.py`. :contentReference[oaicite:4]{index=4}  
- Menggunakan decorator `@view_config` untuk mendefinisikan view secara deklaratif. :contentReference[oaicite:5]{index=5}  
- Menjalankan dan memastikan kedua route bekerja dan menghasilkan respons yang sesuai.  

## Langkah-Langkah  
1. Salin hasil dari langkah sebelumnya ke direktori baru:  
   ```bash
   cd ..; cp -r functional_testing views; cd views
   ``` :contentReference[oaicite:6]{index=6}  
2. Ubah file `tutorial/__init__.py` agar menjadi seperti berikut:  
   ```python
   from pyramid.config import Configurator

   def main(global_config, **settings):
       config = Configurator(settings=settings)
       config.add_route('home', '/')
       config.add_route('hello', '/howdy')
       config.scan('.views')
       return config.make_wsgi_app()
   ``` :contentReference[oaicite:7]{index=7}  
3. Tambahkan modul `tutorial/views.py` dengan konten:  
   ```python
   from pyramid.response import Response
   from pyramid.view import view_config

   @view_config(route_name='home')
   def home(request):
       return Response('<body>Visit <a href="/howdy">hello</a></body>')

   @view_config(route_name='hello')
   def hello(request):
       return Response('<body>Go back <a href="/">home</a></body>')
   ``` :contentReference[oaicite:8]{index=8}  
4. Jalankan proyek (misalnya dengan `pserve development.ini --reload`) dan buka browser ke:  
   - http://localhost:6543/ → harus menampilkan link ke `/howdy`  
   - http://localhost:6543/howdy → harus menampilkan link kembali ke `/`  
5. (Opsional) Ubah atau tambahkan tes untuk memastikan kedua view bekerja sebagaimana mestinya. Tutorial memberikan contoh unit test untuk kedua view. :contentReference[oaicite:9]{index=9}  

## Analisis  
- Dengan memindahkan view ke modul terpisah dan menggunakan `config.scan('.views')`, kita memisahkan *routing/configuration* dari *konteks business logic view*. Ini meningkatkan modularitas dan maintainability. :contentReference[oaicite:10]{index=10}  
- Dekorator `@view_config` membuat definisi view menjadi sangat deklaratif: tiap fungsi view diberi tahu route_name yang sesuai, sehingga konfigurasi menjadi lebih bersih dan eksplisit.  
- Karena kita menambahkan dua route (`home`, `hello`) dan view-function yang saling berpaut (link ke satu ke yang lain), aplikasi kecil ini memberikan mekanisme *navigation* sederhana yang menunjukkan cara kerja routing + views pada Pyramid.  
- Jika kita menjalankan aplikasi dan membuka URL sesuai route, kita ekspektasikan respons HTML sederhana yang berisi link yang sesuai. Kegagalan akan terjadi jika route atau view tidak ter‐scan atau route_name salah.

## Output yang Diharapkan  
- Buka browser ke `http://localhost:6543/` → halaman HTML menampilkan:  
  ```html
  <body>Visit <a href="/howdy">hello</a></body>
````

* Klik link → browser menuju `http://localhost:6543/howdy` → halaman HTML menampilkan:

  ```html
  <body>Go back <a href="/">home</a></body>
  ```
* Tidak ada error server, dan view streaming respons secara langsung.
* Struktur proyek kira-kira:

  ```
  views/
    setup.py
    development.ini
    tutorial/
      __init__.py
      views.py
      (…)
  ```
