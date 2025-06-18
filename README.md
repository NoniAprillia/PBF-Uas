# Selamat Datang Di Sistem Manajemen Magang 

Sistem Manajemen Magang adalah sistem yang digunakan untuk mengatur dan memantau seluruh proses magang, mulai dari pendaftaran, penempatan, pembimbingan, hingga penilaian magang secara terstruktur dan terpusat.

## Tahapan
1. GIT CLONE
![image](https://github.com/user-attachments/assets/d2611b6d-f286-4926-8e08-733e835f290c)

2. Masuk Ke Folder Backend
3. composer install
4. php spark serve
5. cek postman

Postman adalah aplikasi yang digunakan untuk mengirim, menguji, dan mengelola API (Application Programming Interface).

Dengan Postman, kamu bisa:

- Mengirim request GET, POST, PUT, DELETE, dll ke server.

- Melihat response dari API (seperti data JSON, status, dan waktu respons).

- Membantu debug dan uji coba API saat membuat atau menggunakan backend.

Cocok digunakan oleh developer frontend dan backend untuk memastikan API bekerja dengan benar.

6. w+r /cmd untuk install Laravel
![image](https://github.com/user-attachments/assets/843d355c-ce2a-4811-8a23-665b477ba996)
7. Masuk Folder Frontend 
8. Matikan Database

![image](https://github.com/user-attachments/assets/cbdc3cde-d19e-404d-8ab8-d5022d3c38bf)

9. Tambah URL APP_URL=http://localhost:8000
10. php artisan serve 

Perintah php artisan serve digunakan untuk menjalankan server lokal di Laravel. Fungsinya agar kamu bisa mengakses aplikasi Laravel lewat browser, biasanya di alamat:
http://localhost:8000 Jadi, ini berguna untuk mengembangkan dan menguji aplikasi secara lokal sebelum diunggah ke server produksi.

11. bikin mahasiswa kontroller 

php artisan make:controller MahasiswaController

- MahasiswaController.php
```sh
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Http;

class MahasiswaController extends Controller
{
    public function index()
    {
        $response = Http::get('http://localhost:8080/mahasiswa');
        $mahasiswa = $response->json();

        return view('mahasiswa.index', compact('mahasiswa'));
    }
    public function store(Request $request)
{
    $response = Http::post('http://localhost:8080/mahasiswa', [
        'npm_mhs' => $request->npm_mhs,
        'nama_mhs' => $request->nama_mhs,
        'prodi' => $request->prodi,
        'alamat' => $request->alamat,
        'no_telp' => $request->no_telp,
        'email' => $request->email,
    ]);

    if ($response->successful()) {
        return redirect('/mahasiswa')->with('success', 'Data berhasil ditambahkan');
    } else {
        return back()->with('error', 'Gagal tambah data');
    }
}
public function destroy($npm)
{
    $response = Http::delete("http://localhost:8080/mahasiswa/delete/{$npm}");

    if ($response->successful()) {
        return redirect('/mahasiswa')->with('success', 'Data berhasil dihapus');
    } else {
        return back()->with('error', 'Gagal hapus data: ' . $response->body());
    }
}



// Tampilkan form edit
public function edit($npm)
{
    $response = Http::get("http://localhost:8080/mahasiswa/update{$npm}");
    $data = $response->json();

    if ($response->successful()) {
        return view('mahasiswa.edit', ['mahasiswa' => $data]);
    } else {
        return back()->with('error', 'Data tidak ditemukan');
    }
}

// Update data mahasiswa
public function update(Request $request, $npm)
{
    $response = Http::put("http://localhost:8080/mahasiswa/{$npm}", [
        'nama_mhs' => $request->nama_mhs,
        'prodi' => $request->prodi,
        'alamat' => $request->alamat,
        'no_telp' => $request->no_telp,
        'email' => $request->email,
    ]);

    if ($response->successful()) {
        return redirect('/mahasiswa')->with('success', 'Data berhasil diupdate');
    } else {
        return back()->with('error', 'Gagal update data');
    }
}

}
```

- Index.blade.php
```sh
<!DOCTYPE html>
<html>
<head>
    <title>Dashboard Mahasiswa</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body {
            background-color: #f8f9fa;
        }
        .sidebar {
            height: 100vh;
            background-color: #343a40;
            padding: 1rem;
        }
        .sidebar a {
            color: white;
            display: block;
            margin-bottom: 1rem;
            text-decoration: none;
        }
        .sidebar a:hover {
            text-decoration: underline;
        }
        .content {
            padding: 2rem;
        }
    </style>
</head>
<body>

<div class="d-flex">
    {{-- Sidebar --}}
    <div class="sidebar">
        <h4 class="text-white">Menu</h4>
        <a href="/mahasiswa">Dashboard</a>
    </div>

    {{-- Konten --}}
    <div class="flex-grow-1 content">
        <h2 class="mb-4">Dashboard Mahasiswa</h2>

        {{-- Alert Sukses --}}
        @if (session('success'))
            <div class="alert alert-success">{{ session('success') }}</div>
        @endif

        {{-- Tombol Tambah --}}
        <button class="btn btn-primary mb-3" type="button" data-bs-toggle="collapse" data-bs-target="#formTambah" aria-expanded="false" aria-controls="formTambah">
            + Tambah Mahasiswa
        </button>

        {{-- Form Tambah --}}
        <div class="collapse" id="formTambah">
            <div class="card mb-4">
                <div class="card-header bg-primary text-white">
                    Form Tambah Mahasiswa
                </div>
                <div class="card-body">
                    <form method="POST" action="/mahasiswa/simpan">
                        @csrf
                        <div class="mb-3">
                            <label>NPM</label>
                            <input type="text" name="npm_mhs" class="form-control" required>
                        </div>
                        <div class="mb-3">
                            <label>Nama</label>
                            <input type="text" name="nama_mhs" class="form-control" required>
                        </div>
                        <div class="mb-3">
                            <label>Prodi</label>
                            <input type="text" name="prodi" class="form-control" required>
                        </div>
                        <div class="mb-3">
                            <label>Alamat</label>
                            <input type="text" name="alamat" class="form-control" required>
                        </div>
                        <div class="mb-3">
                            <label>No Telp</label>
                            <input type="text" name="no_telp" class="form-control" required>
                        </div>
                        <div class="mb-3">
                            <label>Email</label>
                            <input type="email" name="email" class="form-control" required>
                        </div>
                        <button type="submit" class="btn btn-success">Simpan</button>
                    </form>
                </div>
            </div>
        </div>

        {{-- Tabel Data Mahasiswa --}}
        @if (isset($mahasiswa['error']))
            <div class="alert alert-danger">
                Gagal ambil data dari API:<br>
                <pre>{{ $mahasiswa['error'] }}</pre>
            </div>
        @elseif (is_array($mahasiswa) && count($mahasiswa) > 0)
            <table class="table table-bordered table-striped">
                <thead class="table-light">
                    <tr>
                        <th>NPM</th>
                        <th>Nama</th>
                        <th>Prodi</th>
                        <th>Alamat</th>
                        <th>No Telp</th>
                        <th>Email</th>
                        <th>Aksi</th>
                    </tr>
                </thead>
                <tbody>
                    @foreach ($mahasiswa as $mhs)
                    <tr>
                        <td>{{ $mhs['npm_mhs'] }}</td>
                        <td>{{ $mhs['nama_mhs'] }}</td>
                        <td>{{ $mhs['prodi'] }}</td>
                        <td>{{ $mhs['alamat'] }}</td>
                        <td>{{ $mhs['no_telp'] }}</td>
                        <td>{{ $mhs['email'] }}</td>
                        <td>
                            <a href="/mahasiswa/edit/{{ $mhs['npm_mhs'] }}" class="btn btn-sm btn-warning">Edit</a>
                            <form action="/mahasiswa/delete/{{ $mhs['npm_mhs'] }}" method="POST" style="display:inline;">
    @csrf
    @method('DELETE')
    <button type="submit" class="btn btn-sm btn-danger" onclick="return confirm('Yakin mau hapus?')">Hapus</button>
</form>

                        </td>
                    </tr>
                    @endforeach
                </tbody>
            </table>
        @else
            <div class="alert alert-info">Data mahasiswa tidak tersedia.</div>
        @endif

    </div>
</div>

{{-- Bootstrap JS for collapse --}}
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>

</body>
</html>
```

- Edit.blade.php
```sh
<!DOCTYPE html>
<html>
<head>
    <title>Edit Mahasiswa</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body class="p-5">
    <h2>Edit Mahasiswa</h2>

    @if (session('error'))
        <div class="alert alert-danger">{{ session('error') }}</div>
    @endif

    <form method="POST" action="/mahasiswa/update/{{ $mahasiswa['npm_mhs'] }}">
        @csrf
        <div class="mb-3">
            <label>NPM</label>
            <input type="text" class="form-control" value="{{ $mahasiswa['npm_mhs'] }}" disabled>
        </div>
        <div class="mb-3">
            <label>Nama</label>
            <input type="text" name="nama_mhs" class="form-control" value="{{ $mahasiswa['nama_mhs'] }}" required>
        </div>
        <div class="mb-3">
            <label>Prodi</label>
            <input type="text" name="prodi" class="form-control" value="{{ $mahasiswa['prodi'] }}" required>
        </div>
        <div class="mb-3">
            <label>Alamat</label>
            <input type="text" name="alamat" class="form-control" value="{{ $mahasiswa['alamat'] }}" required>
        </div>
        <div class="mb-3">
            <label>No Telp</label>
            <input type="text" name="no_telp" class="form-control" value="{{ $mahasiswa['no_telp'] }}" required>
        </div>
        <div class="mb-3">
            <label>Email</label>
            <input type="email" name="email" class="form-control" value="{{ $mahasiswa['email'] }}" required>
        </div>
        <button type="submit" class="btn btn-primary">Simpan Perubahan</button>
        <a href="/mahasiswa" class="btn btn-secondary">Batal</a>
    </form>
</body>
</html>
```

- web.php
```sh
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\MahasiswaController;
/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider and all of them will
| be assigned to the "web" middleware group. Make something great!
|
*/

Route::get('/', function(){
    return redirect('/mahasiswa');
});
Route::get('/mahasiswa', [MahasiswaController::class, 'index']);
Route::post('/mahasiswa/simpan', [MahasiswaController::class, 'store']);
Route::delete('/mahasiswa/delete/{npm}', [MahasiswaController::class, 'destroy']);
Route::get('/mahasiswa/edit/{npm}', [MahasiswaController::class, 'edit']);
Route::put('/mahasiswa/update/{npm}', [MahasiswaController::class, 'update']);
```

Membuat Repo Github
- git init
- git remote add origin https://github.com/NoniAprillia/(namarepo)
- git add .
- git commit -m "Initial commit - FE Sistem Manajemen Magang"
- git pull origin master
- git push origin master
