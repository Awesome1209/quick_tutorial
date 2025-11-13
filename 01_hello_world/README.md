# 🐍 Single-File Web Application dengan Pyramid
## Output
<img width="837" height="491" alt="image" src="https://github.com/user-attachments/assets/06a3494b-0ba1-42f6-ab01-9741554abc1f" />
---

## 📘 Deskripsi Singkat
Proyek ini menunjukkan cara membuat **aplikasi web sederhana berbasis Pyramid** hanya dalam **satu file Python**.  
Pendekatan ini cocok bagi pemula yang ingin memahami dasar Pyramid sebelum membuat aplikasi berskala besar.  
Contoh ini diadaptasi dari dokumentasi resmi Pyramid:  
🔗 [https://docs.pylonsproject.org/projects/pyramid/en/latest/quick_tutorial/hello_world.html](https://docs.pylonsproject.org/projects/pyramid/en/latest/quick_tutorial/hello_world.html)

---

## ⚙️ Langkah-Langkah Instalasi dan Eksekusi

### 1️⃣ Persiapan Lingkungan
Pastikan Python dan pip sudah terinstal, kemudian pasang *dependencies* berikut:
```bash
pip install "pyramid==2.0" waitress
```

### 2️⃣ Buat Struktur Proyek
Buat folder kerja baru dan masuk ke dalamnya:
```bash
mkdir hello_world
cd hello_world
```

### 3️⃣ Buat File `app.py`
Buat satu file bernama `app.py` dan isi dengan kode berikut:

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
```

### 4️⃣ Jalankan Aplikasi
Jalankan server menggunakan:
```bash
python app.py
```

### 5️⃣ Akses di Browser
Buka browser dan kunjungi:
```
http://localhost:6543/
```

---

## 🧠 Analisis Kode
Beberapa bagian penting dari aplikasi ini:

| Bagian Kode | Penjelasan |
|--------------|-------------|
| `from waitress import serve` | Menggunakan **Waitress**, server WSGI ringan untuk menjalankan aplikasi. |
| `Configurator()` | Objek utama Pyramid untuk mendaftarkan *route* dan *view*. |
| `config.add_route('hello', '/')` | Menentukan rute URL `/` agar diarahkan ke fungsi `hello_world`. |
| `config.add_view(hello_world, route_name='hello')` | Menghubungkan rute `'hello'` dengan fungsi view yang menangani respon. |
| `Response('<body><h1>Hello World!</h1></body>')` | Mengembalikan HTML sederhana ke klien. |
| `serve(app, host='0.0.0.0', port=6543)` | Menjalankan aplikasi pada alamat lokal port 6543. |

Kode ini bekerja tanpa konfigurasi tambahan, karena semua komponen (routing, view, dan response) didefinisikan langsung di dalam satu file.

---

## 🧩 Alur Eksekusi
1. Saat file dijalankan, blok `if __name__ == '__main__':` akan membuat konfigurasi Pyramid.  
2. Pyramid mendaftarkan route `'/'` ke fungsi `hello_world`.  
3. Waitress menjalankan aplikasi di port 6543.  
4. Ketika browser mengakses `http://localhost:6543/`, fungsi `hello_world()` dipanggil, mencetak `"Incoming request"` di terminal, dan mengirim HTML sebagai respon ke browser.

---

## 🖥️ Output yang Diharapkan

### Di Browser:
Menampilkan halaman sederhana berisi:
```html
<h1>Hello World!</h1>
```

### Di Terminal:
Menampilkan log seperti:
```
Incoming request
```

---

## 🧾 Kesimpulan
Contoh **Single-File Web Application** ini menunjukkan betapa mudahnya membangun aplikasi web dasar menggunakan Pyramid.  
Dengan hanya beberapa baris kode, Anda sudah memiliki server web penuh yang bisa dikembangkan menjadi aplikasi REST API, sistem template, atau situs dinamis di masa depan.

---
