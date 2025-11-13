# 06: Functional Testing with WebTest

## Deskripsi Singkat  
Pada langkah ini kita akan melakukan *testing fungsional* penuh (end-to-end) terhadap aplikasi Pyramid dengan menggunakan paket WebTest.  
WebTest memungkinkan kita membuat tes yang mensimulasikan permintaan HTTP ke aplikasi WSGI kita dan memeriksa respons, tanpa harus menjalankan server HTTP nyata. :contentReference[oaicite:2]{index=2}

## Output
<img width="1414" height="110" alt="image" src="https://github.com/user-attachments/assets/978e1695-dbf3-47b2-bacd-07db40112bb4" />

## Latar Belakang  
Tes unit bagus untuk pengujian internal modul, tetapi aplikasi web sebenarnya terdiri dari templating, routing, middleware, dan seluruh rantai request-response — yang perlu diuji dari sisi pengguna/permintaan lengkap. WebTest menyediakan cara cepat untuk melakukan ini. :contentReference[oaicite:3]{index=3}  
Dengan testing fungsional, kita mendapatkan *confidence* bahwa seluruh aplikasi bekerja bersama-sama, bukan hanya fungsi terisolasi.

## Tujuan  
- Menambahkan `webtest` ke daftar dependensi pengembangan di `setup.py`. :contentReference[oaicite:4]{index=4}  
- Menulis tes fungsional yang membuat permintaan HTTP terhadap aplikasi dan memeriksa isi HTML/response. :contentReference[oaicite:5]{index=5}  
- Menjalankan aplikasi dalam lingkungan pengujian dan melihat hasil sukses-tes.

## Langkah-langkah  
1. Salin hasil dari langkah sebelumnya (05: Unit Tests and pytest) ke direktori baru:  
   ```bash
   cd ..; cp -r unit_testing functional_testing; cd functional_testing
   ``` :contentReference[oaicite:6]{index=6}  
2. Tambahkan `webtest` ke dalam `setup.py` sebagai bagian dari extras pengembangan:  
   ```python
   from setuptools import setup

   requires = [
       'pyramid',
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
3. Instal proyek dalam mode editable dengan semua dependensi pengembangan:  
   ```bash
   $VENV/bin/pip install -e ".[dev]"
   ``` :contentReference[oaicite:8]{index=8}  
4. Buat/ubah file tes fungsional `functional_testing/tutorial/tests.py` untuk menyertakan tes HTTP lengkap, misalnya:  
   ```python
   import unittest
   from webtest import TestApp
   from tutorial import main

   class TutorialFunctionalTests(unittest.TestCase):
       def setUp(self):
           app = main({})
           self.testapp = TestApp(app)

       def test_hello_world(self):
           response = self.testapp.get('/', status=200)
           # Pastikan body mengandung teks “Hello World”
           assert b"Hello World" in response.body
````

(Contoh kode ini merupakan adaptasi dari tutorial. Tutorial asli menunjukkan bagaimana melakukan permintaan ke aplikasi WSGI. ) ([docs.pylonsproject.org][1])
5. Jalankan tes menggunakan pytest atau unittest:

```bash
$VENV/bin/pytest tutorial/tests.py -q
```

Hasil yang diharapkan:

```
.
1 passed in 0.xx seconds
```
