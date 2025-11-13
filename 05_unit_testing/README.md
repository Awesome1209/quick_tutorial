# 🧪 05: Unit Tests and pytest

## 📘 Deskripsi Singkat
Langkah ini menunjukkan bagaimana menambahkan **unit test** ke aplikasi **Pyramid** menggunakan **pytest**.  
Unit test memastikan bahwa fungsi dan komponen kecil dari aplikasi bekerja sebagaimana mestinya, membantu mencegah bug, serta menjaga stabilitas aplikasi.  

Pyramid menyediakan modul `pyramid.testing` yang memudahkan pengujian tanpa perlu menjalankan server HTTP sungguhan.

---

## 🎯 Tujuan Pembelajaran
- Memahami konsep **unit testing** dalam Pyramid.  
- Menggunakan `pytest` untuk menjalankan tes.  
- Membuat file pengujian untuk view `hello_world`.  
- Menganalisis hasil pengujian dan cara kerjanya.  

---

## ⚙️ Langkah-Langkah Implementasi

### 1️⃣ Persiapan Proyek
Pastikan kamu sudah menyelesaikan langkah sebelumnya (`04: debugtoolbar`).  
Kemudian salin folder tersebut agar kita bisa lanjut dari situ tanpa mengubah aslinya:

```bash
cd ..
cp -r debugtoolbar unit_testing
cd unit_testing

### 2️⃣ Struktur Awal Folder
Setelah disalin, struktur proyek akan tampak seperti ini:

arduino
Copy code
unit_testing/
│
├── development.ini
├── setup.py
└── tutorial/
    ├── __init__.py
    ├── views.py
    ├── static/
    └── templates/
### 3️⃣ Tambahkan Dependensi pytest ke setup.py
Edit file setup.py agar mendukung pengujian menggunakan pytest.

python
Copy code
from setuptools import setup

requires = [
    'pyramid',
    'waitress',
]

dev_requires = [
    'pyramid_debugtoolbar',
    'pytest',
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
Penjelasan:

install_requires → dependensi utama aplikasi.

extras_require → dependensi tambahan untuk pengembangan (di sini: pytest dan pyramid_debugtoolbar).

4️⃣ Instalasi Proyek dalam Mode Development
Gunakan virtual environment dari proyek Pyramid-mu, lalu jalankan:

bash
Copy code
$VENV/bin/pip install -e ".[dev]"
Outputnya akan kurang lebih seperti ini:

perl
Copy code
Obtaining file:///path/to/unit_testing
Installing collected packages: pytest, pyramid_debugtoolbar
Successfully installed pyramid_debugtoolbar pytest tutorial
5️⃣ Kode Utama Aplikasi (views.py)
Pastikan file tutorial/views.py berisi view hello_world seperti berikut:

python
Copy code
from pyramid.response import Response
from pyramid.view import view_config

@view_config(route_name='hello', renderer='json')
def hello_world(request):
    return Response(body=b"Hello World", content_type='text/plain')
6️⃣ Konfigurasi Aplikasi (tutorial/init.py)
Pastikan file tutorial/__init__.py masih sama seperti sebelumnya:

python
Copy code
from pyramid.config import Configurator
from pyramid.response import Response

def hello_world(request):
    return Response(body=b"Hello World", content_type='text/plain')

def main(global_config, **settings):
    with Configurator(settings=settings) as config:
        config.add_route('hello', '/')
        config.add_view(hello_world, route_name='hello')
        return config.make_wsgi_app()
7️⃣ Buat File Tes tutorial/tests.py
Sekarang buat file baru bernama tests.py di dalam folder tutorial/ dengan isi berikut:

python
Copy code
import unittest
from pyramid import testing

class TutorialViewTests(unittest.TestCase):
    def setUp(self):
        # Menyiapkan konfigurasi test environment Pyramid
        self.config = testing.setUp()

    def tearDown(self):
        # Membersihkan konfigurasi setelah test selesai
        testing.tearDown()

    def test_hello_world(self):
        from tutorial import hello_world

        # Membuat request dummy untuk menguji view
        request = testing.DummyRequest()
        response = hello_world(request)

        # Memastikan bahwa view mengembalikan response 200 OK
        self.assertEqual(response.status_code, 200)

        # Tes tambahan: pastikan teks "Hello World" ada di body response
        self.assertIn(b"Hello World", response.body)
Penjelasan:

testing.setUp() → Membuat environment konfigurasi Pyramid tiruan.

DummyRequest() → Membuat request palsu untuk simulasi HTTP request.

hello_world() → View yang diuji.

assertEqual dan assertIn → Memverifikasi hasil sesuai ekspektasi.

8️⃣ Jalankan Tes
Sekarang jalankan pengujian menggunakan pytest:

bash
Copy code
$VENV/bin/pytest tutorial/tests.py -q
Output yang diharapkan:

Copy code
.
1 passed in 0.14 seconds
Artinya semua tes berhasil dijalankan ✅

🧠 Analisis Lengkap
🔹 Konsep Unit Testing
Unit testing berfokus pada pengujian komponen terkecil dari aplikasi (fungsi, method, atau kelas) secara terpisah.
Dengan melakukan ini, kita bisa:

Menemukan bug lebih cepat.

Mengurangi risiko error setelah refactor.

Meningkatkan kepercayaan diri dalam pengembangan.

🔹 Menggunakan pyramid.testing
Modul ini menyediakan alat bantu khusus untuk testing aplikasi Pyramid tanpa harus menjalankan server.
Beberapa fitur penting:

testing.setUp() → menyiapkan environment konfigurasi sementara.

testing.tearDown() → membersihkan setelah pengujian selesai.

DummyRequest() → membuat request tiruan agar view bisa diuji langsung.

🔹 Kelebihan pytest
Dibandingkan dengan unittest bawaan Python, pytest menawarkan:

Output hasil yang lebih informatif dan berwarna.

Tidak perlu banyak boilerplate kode.

Mendukung plugin (misalnya coverage).

Bisa menjalankan unittest.TestCase tanpa modifikasi.

🔹 Hasil yang Diharapkan
Setelah menjalankan tes:

bash
Copy code
$VENV/bin/pytest tutorial/tests.py -q
Kamu akan melihat output seperti:

Copy code
.
1 passed in 0.14 seconds
📋 Makna hasil:

. → menandakan satu tes dijalankan.

1 passed → semua tes sukses.

0.14 seconds → waktu eksekusi.

Jika terjadi error, pytest akan menampilkan rincian traceback yang jelas, misalnya:

yaml
Copy code
E   AssertionError: 404 != 200
Menunjukkan bahwa view tidak mengembalikan status 200 seperti yang diharapkan.

🧾 Struktur Akhir Proyek
arduino
Copy code
unit_testing/
│
├── setup.py
├── development.ini
└── tutorial/
    ├── __init__.py
    ├── views.py
    ├── tests.py
    ├── templates/
    └── static/
🧩 Kesimpulan
Pada bagian ini kamu telah belajar bagaimana:
✅ Menambahkan pytest ke proyek Pyramid.
✅ Membuat file pengujian untuk view sederhana.
✅ Menjalankan dan menganalisis hasil tes.
✅ Menggunakan pyramid.testing dan DummyRequest untuk simulasi request.

Testing adalah komponen penting dalam siklus hidup perangkat lunak modern.
Dengan menulis tes secara rutin, kita dapat menjaga aplikasi tetap stabil meskipun terjadi banyak perubahan pada kode.

🚀 Langkah Berikutnya
Setelah memahami unit testing, kamu bisa lanjut ke:

06: Functional Testing
Di mana kamu akan mempelajari cara menguji keseluruhan aplikasi Pyramid dari perspektif pengguna.
