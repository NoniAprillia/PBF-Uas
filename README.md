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

```
acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (main)
$ git init
Reinitialized existing Git repository in C:/laragon/www/belajarPBFuas230102040/.git/

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (main)
$ git add .

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (main)
$ git commit -m "first commit"
On branch main
nothing to commit, working tree clean

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (main)
$ git log --oneline
eaaf44a (HEAD -> main) first commit

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (main)
$ rm -rf .git

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040
$ del /s /q .git
bash: del: command not found

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040
$ git init
Initialized empty Git repository in C:/laragon/www/belajarPBFuas230102040/.git/

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (master)
$ git add .
warning: in the working copy of 'resources/views/mahasiswa/edit.blade.php', CRLF will be replaced by LF the next time Git touches it
warning: in the working copy of 'resources/views/mahasiswa/index.blade.php', CRLF will be replaced by LF the next time Git touches it

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (master)
$ git add .

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (master)
$ git commit -m "first commit"
[master (root-commit) d2f77ab] first commit
 82 files changed, 11444 insertions(+)
 create mode 100644 .editorconfig
 create mode 100644 .env.example
 create mode 100644 .gitattributes
 create mode 100644 .gitignore
 create mode 100644 README.md
 create mode 100644 app/Console/Kernel.php
 create mode 100644 app/Exceptions/Handler.php
 create mode 100644 app/Http/Controllers/Controller.php
 create mode 100644 app/Http/Controllers/MahasiswaController.php
 create mode 100644 app/Http/Kernel.php
 create mode 100644 app/Http/Middleware/Authenticate.php
 create mode 100644 app/Http/Middleware/EncryptCookies.php
 create mode 100644 app/Http/Middleware/PreventRequestsDuringMaintenance.php
 create mode 100644 app/Http/Middleware/RedirectIfAuthenticated.php
 create mode 100644 app/Http/Middleware/TrimStrings.php
 create mode 100644 app/Http/Middleware/TrustHosts.php
 create mode 100644 app/Http/Middleware/TrustProxies.php
 create mode 100644 app/Http/Middleware/ValidateSignature.php
 create mode 100644 app/Http/Middleware/VerifyCsrfToken.php
 create mode 100644 app/Models/User.php
 create mode 100644 app/Providers/AppServiceProvider.php
 create mode 100644 app/Providers/AuthServiceProvider.php
 create mode 100644 app/Providers/BroadcastServiceProvider.php
 create mode 100644 app/Providers/EventServiceProvider.php
 create mode 100644 app/Providers/RouteServiceProvider.php
 create mode 100644 artisan
 create mode 100644 bootstrap/app.php
 create mode 100644 bootstrap/cache/.gitignore
 create mode 100644 composer.json
 create mode 100644 composer.lock
 create mode 100644 config/app.php
 create mode 100644 config/auth.php
 create mode 100644 config/broadcasting.php
 create mode 100644 config/cache.php
 create mode 100644 config/cors.php
 create mode 100644 config/database.php
 create mode 100644 config/filesystems.php
 create mode 100644 config/hashing.php
 create mode 100644 config/logging.php
 create mode 100644 config/mail.php
 create mode 100644 config/queue.php
 create mode 100644 config/sanctum.php
 create mode 100644 config/services.php
 create mode 100644 config/session.php
 create mode 100644 config/view.php
 create mode 100644 database/.gitignore
 create mode 100644 database/factories/UserFactory.php
 create mode 100644 database/migrations/2014_10_12_000000_create_users_table.php
 create mode 100644 database/migrations/2014_10_12_100000_create_password_reset_tokens_table.php
 create mode 100644 database/migrations/2019_08_19_000000_create_failed_jobs_table.php
 create mode 100644 database/migrations/2019_12_14_000001_create_personal_access_tokens_table.php
 create mode 100644 database/seeders/DatabaseSeeder.php
 create mode 100644 package.json
 create mode 100644 phpunit.xml
 create mode 100644 public/.htaccess
 create mode 100644 public/favicon.ico
 create mode 100644 public/index.php
 create mode 100644 public/robots.txt
 create mode 100644 resources/css/app.css
 create mode 100644 resources/js/app.js
 create mode 100644 resources/js/bootstrap.js
 create mode 100644 resources/views/mahasiswa/edit.blade.php
 create mode 100644 resources/views/mahasiswa/index.blade.php
 create mode 100644 resources/views/welcome.blade.php
 create mode 100644 routes/api.php
 create mode 100644 routes/channels.php
 create mode 100644 routes/console.php
 create mode 100644 routes/web.php
 create mode 100644 storage/app/.gitignore
 create mode 100644 storage/app/public/.gitignore
 create mode 100644 storage/framework/.gitignore
 create mode 100644 storage/framework/cache/.gitignore
 create mode 100644 storage/framework/cache/data/.gitignore
 create mode 100644 storage/framework/sessions/.gitignore
 create mode 100644 storage/framework/testing/.gitignore
 create mode 100644 storage/framework/views/.gitignore
 create mode 100644 storage/logs/.gitignore
 create mode 100644 tests/CreatesApplication.php
 create mode 100644 tests/Feature/ExampleTest.php
 create mode 100644 tests/TestCase.php
 create mode 100644 tests/Unit/ExampleTest.php
 create mode 100644 vite.config.js

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (master)
$ git remote add origin https://github.com/NoniAprillia/LatihanKel1.git

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (master)
$ git branch -M main

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (main)
$ git push -u origin main
Enumerating objects: 106, done.
Counting objects: 100% (106/106), done.
Delta compression using up to 4 threads
Compressing objects: 100% (90/90), done.
Writing objects: 100% (106/106), 75.76 KiB | 500.00 KiB/s, done.
Total 106 (delta 7), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (7/7), done.
To https://github.com/NoniAprillia/LatihanKel1.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.

acerq@LAPTOP-ROE623UF MINGW64 /c/laragon/www/belajarPBFuas230102040 (main)
$
```
