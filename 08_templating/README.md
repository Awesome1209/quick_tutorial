# 08: HTML Generation With Templating

## Deskripsi Singkat  
Pada langkah ini kita akan belajar bagaimana menghasilkan HTML melalui *templating engine* dalam aplikasi Pyramid, menggantikan respons HTML yang dikode langsung di view. Kita akan mengaktifkan add-on pyramid_chameleon (atau engine templating lain seperti Jinja2/Mako) dan menghubungkan view ke file template. :contentReference[oaicite:2]{index=2}  
Dengan pendekatan ini, view hanya mengembalikan data (dictionary) dan template yang bertugas untuk menghasilkan HTML. Ini meningkatkan pemisahan antara logika aplikasi dan tampilan HTML.

## Output
<img width="1412" height="116" alt="image" src="https://github.com/user-attachments/assets/44ba889f-63c4-44de-a139-357a819df867" />

<img width="2560" height="646" alt="image" src="https://github.com/user-attachments/assets/f44098bf-7d30-431d-988f-fa7df71173f2" />

## Tujuan  
- Mengaktifkan add-on templating (pyramid_chameleon) di proyek. :contentReference[oaicite:3]{index=3}  
- Menghubungkan template file sebagai “renderer” untuk view code. :contentReference[oaicite:4]{index=4}  
- Mengubah view sehingga hanya mengembalikan data, dan template yang menghasilkan HTML. :contentReference[oaicite:5]{index=5}  
- Memverifikasi bahwa ketika membuka route aplikasi, HTML yang dihasilkan sesuai dengan yang diharapkan (template menerima data & merender dengan benar).

## Langkah-langkah  
1. Mulai dari hasil proyek sebelumnya (07: Views).  
   ```bash
   cd ..; cp -r views templating; cd templating
   ``` :contentReference[oaicite:6]{index=6}  
2. Tambahkan `pyramid_chameleon` (engine templating Chameleon) ke dalam `setup.py`, misalnya:  
   ```python
   from setuptools import setup

   requires = [
       'pyramid',
       'pyramid_chameleon',
       'waitress',
   ]

   dev_requires = [
       'pyramid_debugtoolbar',
       'pytest',
       'webtest',
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
3. Instal proyek dalam mode development:  
   ```bash
   $VENV/bin/pip install -e .
   ``` :contentReference[oaicite:8]{index=8}  
4. Ubah file `tutorial/__init__.py` agar menyertakan add-on templating dan route seperti:  
   ```python
   from pyramid.config import Configurator

   def main(global_config, **settings):
       config = Configurator(settings=settings)
       config.include('pyramid_chameleon')
       config.add_route('home', '/')
       config.add_route('hello', '/howdy')
       config.scan('.views')
       return config.make_wsgi_app()
   ``` :contentReference[oaicite:9]{index=9}  
5. Ubah atau buat modul `tutorial/views.py` menjadi seperti:  
   ```python
   from pyramid.view import view_config

   @view_config(route_name='home', renderer='home.pt')
   def home(request):
       return {'name': 'Home View'}

   @view_config(route_name='hello', renderer='home.pt')
   def hello(request):
       return {'name': 'Hello View'}
   ``` :contentReference[oaicite:10]{index=10}  
6. Buat file template `tutorial/home.pt` (atau sesuai setting) misalnya:  
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <title>Quick Tutorial: ${name}</title>
   </head>
   <body>
       <h1>Hi ${name}</h1>
   </body>
   </html>
   ``` :contentReference[oaicite:11]{index=11}  
7. Ubah file `development.ini` agar memuat setting untuk reload templates bila di-development, misalnya:  
   ```ini
   [app:main]
   use = egg:tutorial
   pyramid.reload_templates = true
   pyramid.includes =
       pyramid_debugtoolbar

   [server:main]
   use = egg:waitress#main
   listen = localhost:6543
   ``` :contentReference[oaicite:12]{index=12}  
8. Jalankan aplikasi dengan:  
   ```bash
   $VENV/bin/pserve development.ini --reload
````
* [http://localhost:6543/howdy](http://localhost:6543/howdy) → halaman HTML dengan `name = Hello View`

