SERI TUTORIAL CODEIGNITER 4: MEMBUAT FITUR LOGIN DAN REGISTER
GUN GUN PRIATNA  CODEIGNITER 4  01 MARET 2020
Seri Tutorial CodeIgniter 4: Membuat Fitur Login Dan Register
Pada tutorial CodeIgniter 4 sebelumnya kita telah membahas tentang CRUD di CodeIgniter 4, di edisi tutorial kali ini kita akan mencoba membuat aplikasi sederhana CodeIgniter 4 yang menerapkan fitur authentication seperti login dan register. Sebetulnya ini adalah eksperimen yang saya coba ketika CodeIgniter 4 resmi di rilis bulan Februari lalu. Ada hal yang mengganjal dan menjadi pertanyaan, kira-kira kode lama saya masih bisa jalan ga ya? Hmm, menarik kan? Yuk kita coba!

Pada tutorial CodeIgniter 4 ini kita akan coba membangun aplikasi dengan fitur login dan register untuk sebuah platform belajar koding. Studi kasus ini terinspirasi dari munculnya platform belajar yang belakangan ini bisa ditemukan iklannya waktu kita masuk ke grup pemrograman. Nah, apa saja fitur yang akan kita buat? Kita buat platform sederhana yang di dalamnya ada:

Halaman home atau mungkin landing page sederhana.
Fitur untuk Register atau pendaftaran user.
Fitur untuk Login ke platform belajar kita.
Fitur dashboard sederhana
Dan yang terakhir, fitur untuk logout.
Alur penggunaan platform belajar pada tutorial codeigniter 4 ini cukup sederhana, di mulai user membuka alamat website platform belajar, terus masuk ke halaman register untuk mendaftar. Lalu selanjutnya user bisa login menggunakan akun yang sudah terdaftar. Dan ketika sudah login, user bisa masuk ke halaman dashboard. Dan data yang akan disimpan ke database pun cukup sederhana, kita perlu data user seperti nama, email dan password yang akan digunakan untuk login ke platform belajar yang akan dikembangkan.

Oke sudah cukup jelas kan kebutuhan platform belajar kita. Selanjutnya kita bahas apa saja langkah-langkah dalam membuat fitur Login dan Register di CodeIgniter 4? Check this out, ya!

Daftar Isi

Step 1: Instalasi CodeIgniter 4
Step 2: Konfigurasi Project
Step 3: Membuat Database
Step 4: Membuat file migration
Step 5: Membuat template UI
Step 6: Membuat halaman utama website
Step 7: Membuat fitur register
Step 8: Membuat fitur login
Step 9: Membuat fitur logout
Kesimpulan
Step 1: Instalasi CodeIgniter 4
Seperti biasa, langkah pertama kita adalah menginstall framework CodeIgniter 4. Nah, kita install CodeIgniter 4 menggunakan composer. Misalkan kamu mau pakai cara lain itu boleh.

Oke, kita buka terminal atau cmd, lalu kita jalankan command ini di terminal / cmd:

composer create-project codeigniter4/appstarter platform-belajar
Baik, kita tunggu dulu sampai proses instalasi codeigniter 4 selesai.

Step 2: Konfigurasi Project
Ada dua cara untuk melakukan konfigurasi project di codeigniter 4, yang pertama dengan cara mengedit langsung file config yang ada di direktori app/Config dan cara kedua adalah melalui file .env. Di serial tutorial CodeIgniter 4 ini kita akan selalu menggunakan cara kedua, yaitu melalui file .env.

Buka kembali terminal, kita masuk ke folder project terlebih dahulu sebelum mengatur konfigurasi dengan command:

cd platform-belajar
Selanjutnya kita copy file .env dari file env yang udah ada di project kita. Buka lagi terminalnya, lalu kita jalankan command ini:

cp env .env
Setelah command di atas kita run, kita bisa lihat file baru .env di folder root project kita.

Kita buka project kita di text editor (di sini saya menggunakan Visual Studio Code), ketik command:

code .
Kawan, misalkan pakai text editor lain, misalnya sublime text, bisa coba buka langsung dengan memilih menu File di text editor, lalu pilih menu Open Folder lalu pilih folder project ini, yaitu platform-belajar.

Kita bisa lihat ada file .envyang telah kita buat sebelumnya.

Selanjutnya buka file .env, lalu kita sesuaikan konfigurasi seperti baris kode di bawah ini:

CI_ENVIRONMENT = development

app.baseURL = 'http://localhost:8080/'

database.default.hostname = localhost
database.default.database = platform_belajar
database.default.username = root
database.default.password = 
database.default.DBDriver = MySQLi
Catatan: di bagian password, sesuaikan dengan password MySQL teman-teman. Jika passwordnya kosong, tidak perlu diisi seperti yang saya contohkan di atas.

Step 3: Membuat Database
Nah di file .env tadi kita kan sudah tambahkan pengaturan database, dan nama databasenya itu platform_belajar. Jadi langkah kita selanjutnya, kita buat database sesuai dengan nama database di file .env. Buka phpMyAdmin, lalu buat database baru dengan nama platform_belajar.

Step 4: Membuat file migration
Pada tahapan ini, kita akan membuat table baru untuk menyimpan data user. Untuk membuat table baru, kita akan menggunakan fitur migration CodeIgniter 4. Nah untuk nama table nya kita sesuaikan saja dengan namanya dengan data yang akan disimpan, yaitu users. Kita buat file migration untuk membuat table users. Buka kembali terminal / cmd, lalu jalankan command berikut ini untuk membuat file migration:

php spark migrate:create Users
Selanjutnya kita bisa lihat file baru yang dibuat command di atas di direktori app/Database/Migrations dan memiliki nama dengan format YYYY-MM-DD-HHIISS_namatable.php misalnya 2020-02-29-121955_Users.php sesuai dengan timestamp ketika filenya dibuat. Buka file 2020-02-29-121955_Users.php lalu sesuaikan kodenya menjadi seperti berikut ini:


<?php namespace App\Database\Migrations;

use CodeIgniter\Database\Migration;

class Users extends Migration
{
    public function up()
    {
        $this->forge->addField([
            'id' => [
                'type' => 'INT',
                'unsigned' => TRUE,
                'auto_increment' => TRUE
            ],
            'name' => [
                'type' => 'VARCHAR',
                'constraint' => 255,
                'null' => FALSE,
            ],
            'email' => [
                'type' => 'VARCHAR',
                'constraint' => 255,
                'null' => FALSE,
                'unique' => TRUE,
            ],
            'password' => [
                'type' => 'VARCHAR',
                'constraint' => 255,
                'null' => FALSE,
            ],
            'created_at' => [
                'type' => 'datetime',
                'null' => TRUE
            ],
            'updated_at' => [
                'type' => 'datetime',
                'null' => TRUE
            ]
        ]);

        $this->forge->addKey('id', TRUE);
        $this->forge->createTable('users');
    }

    //--------------------------------------------------------------------

    public function down()
    {
        $this->forge->dropTable('users');
    }
}
Selanjutnya kita run perintah migrate di terminal atau cmd. Buka terminal atau cmd lalu jalankan command:

php spark migrate
Nah, ketika kita cek di phpMyAdmin, kita bisa melihat table baru di database platform_belajar, ada table migrations dan table users.

Step 5: Membuat template UI
Untuk view di project platform belajar ini, kita akan menggunakan templating UI, supaya kita tidak mengulang-ngulang kode HTML untuk bagian header dan footer project kita. Kita buat folder baru terlebih dahulu di direktori app/Views dengan nama layouts. Setelah itu, kita buat file baru yang digunakan untuk templating UI di dalam folder tersebut (app/Views/layouts) dengan nama main.php. Kita sama-sama ketik baris kode ini ya..

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta name="<?= csrf_token() ?>" content="<?= csrf_hash() ?>">
    <title><?= $title; ?></title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
</head>

<body>

    <!-- Content -->
    <?= $this->renderSection('content') ?>

    <!-- /.Content -->

    <footer class="text-center mt-5">
        <p><em><small>Seri Tutorial CodeIgniter 4: Login dan Register CodeIgniter @ <a href="https://qadrlabs.com/">qadrlabs.com</a></small></em></p>
    </footer>
    <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>

</body>

</html>
Kita save file main.php setelah selesai coding untuk templating UI.

Step 6: Membuat halaman utama website
Halaman utama website ini akan kita gunakan sebagain halaman yang pertama kali ditampilkan ketika project kita diakses. Jika kita lihat di project kita, secara default ada controller yang digunakan untuk halaman utama, yaitu controller Home. Pada tahapan ini kita tidak perlu membuat controller baru, kita coba modifikasi controller Home yang sudah ada.

Nah selanjutnya kita akan memodifikasi halaman tampilan awal CodeIgniter 4, yaitu halaman home.php. Kita buat tampilan home ini, kita tambahkan link menuju halaman login dan register. Oke selanjutnya kita modifikasi dulu controllernya. Buka file controller Home.php yang ada di direktori app/Controllers. Nah kita modifikasi menjadi:


<?php namespace App\Controllers;

class Home extends BaseController
{
    public function index()
    {
        $data = [
            'title' => 'Seri Tutorial CodeIgniter 4: Login dan Register @ qadrlabs.com'
        ];

        return view('home', $data);
    }

}
nah, langkah selanjutnya, sesuai dengan baris kode di atas, kita akan buat file views baru dengan nama home.php di direktori app/Views, lalu kita ketik kode ini...

<?= $this->extend('layouts/main') ?>

<?= $this->section('content') ?>

<section class="jumbotron text-center" style="height: 500px">
    <h1 class="mt-5">BELAJAR KODING MULAI DARI NOL</h1>
    <p class="lead text-muted">
        Mau buat website bingung mulai dari mana? Yuk kita sama-sama belajar di sini.
    </p>
    <a href="<?php echo base_url('login'); ?>" class="btn btn-outline-primary my-2">Login</a>
    <a href="<?php echo base_url('register'); ?>" class="btn btn-success my-2">Register</a>
</section>

<?= $this->endSection() ?>
Ya, tampilannya kita buat sederhana, hanya ada tulisan, dan link menuju halaman login dan register. Misalkan layout atau tulisannya mau teman-teman modif lagi, itu boleh banget.

Step 7: Membuat fitur register
Oke, kita masuk ke fitur utama dari studi kasus tutorial codeigniter 4 ini, yaitu fitur register. Nah, sekarang kita buat file modelnya dulu. Kita buat file models baru dengan nama UserModel.php di direktori app/Models, lalu kita ketik kode di bawah ini:

<?php

namespace App\Models;

use CodeIgniter\Model;

class UserModel extends Model
{
    protected $table = 'users';
    protected $allowedFields = ['name', 'email', 'password'];
    protected $useTimeStamps = true;
    protected $createdField = 'created_at';
    protected $updatedField = 'updated_at';

    protected $validationRules = [
        'name' => 'required',
        'email' => 'required|valid_email|is_unique[users.email]',
        'password' => 'required|min_length[8]'
    ];

    protected $validationMessages = [
        'email' => [
            'is_unique' => 'Sorry, That email has already been taken. Please choose another.'
        ]
    ];

    protected $skipValidation = false;
    protected $beforeInsert = ['hashPassword'];

    protected function hashPassword(array $data)
    {
        if (! isset($data['data']['password'])) {
            return $data;
        }

        $data['data']['password'] = password_hash($data['data']['password'], PASSWORD_DEFAULT);
        return $data;
    }

}
Nah selanjutnya kita buat controller baru dengan nama RegisterController.php di direktori app/Controllers, lalu kita tambahkan method index() untuk menampilkan halaman register.

<?php namespace App\Controllers;

use App\Models\UserModel;

class RegisterController extends BaseController
{
    protected $model;

    public function __construct()
    {
        $this->model = new UserModel();
        $this->helpers = ['form', 'url'];
    }

    public function index()
    {
        $data = [
            'title' => 'Register | Seri Tutorial CodeIgniter 4: Login dan Register @ qadrlabs.com'
        ];

        return view('auth/register', $data);
    }
}
Pada baris kode di atas ada kode view('auth/register', $data), itu artinya file view register.php ada di dalam folder auth. Sekarang kita buat terlebih dahulu folder auth di dalam app/Views. Misalkan foldernya itu sudah kita buat, selanjutnya kita buat file viewsnya dengan nama register.php, lalu kita ketik baris kode di bawah ini:

<?= $this->extend('layouts/main') ?>

<?= $this->section('content') ?>

<div class="container mt-5">
    <div class="row">
        <div class="col-md-4 offset-md-4">
            <div class="card">
                <div class="card-body">
                    <h4 class="text-center" style="font-weight: bold;">REGISTER</h4>
                    <hr>
                    <?php if (session()->getFlashdata('success')) { ?>
                        <div class="alert alert-success">
                            <?php echo session()->getFlashdata('success'); ?>
                        </div>
                    <?php } ?>

                    <?php if (session()->getFlashdata('error')) { ?>
                        <div class="alert alert-danger">
                            <?php foreach (session()->getFlashdata('error') as $field => $error) : ?>
                                <p><?= $error ?></p>
                            <?php endforeach ?>
                        </div>
                    <?php } ?>

                    <?= form_open('register'); ?>
                    <div class="form-group">
                        <label for="name">Nama</label>
                        <input type="text" name="name" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="email">Email</label>
                        <input type="email" name="email" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="password">Password</label>
                        <input type="password" name="password" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <button class="btn btn-primary">Register</button>
                    </div>
                    <?= form_close(); ?>
                </div>

            </div>
            <div class="text-center mt-2">
                Sudah punya akun? <a href="<?php echo base_url('login'); ?>">Silakan login.</a>
            </div>
        </div>
    </div>
</div>

<?= $this->endSection() ?>
Selanjutnya untuk menghandle request post, kita buka controller RegisterController.php lagi. Kita tambahkan method store():


 public function store()
    {
        $name = $this->request->getPost('name');
        $email = $this->request->getPost('email');
        $password = $this->request->getPost('password');

        $user = [
            'name' => $name,
            'email' => $email,
            'password' => $password
        ];

        $save = $this->model->save($user);

        if ($save) {
            session()->setFlashdata('success', 'Register Berhasil!');
            return redirect()->to(base_url('login'));
        } else {
            session()->setFlashdata('error', $this->model->errors());
            return redirect()->back();
        }
    }
Jadi kesseluruhan controller RegisterController.php menjadi:

<?php

namespace App\Controllers;

use App\Models\UserModel;

class RegisterController extends BaseController
{
    protected $model;

    public function __construct()
    {
        $this->model = new UserModel();
        $this->helpers = ['form', 'url'];
    }

    public function index()
    {
        $data = [
            'title' => 'Register | Seri Tutorial CodeIgniter 4: Login dan Register @ qadrlabs.com'
        ];

        return view('auth/register', $data);
    }

    public function store()
    {
        $name = $this->request->getPost('name');
        $email = $this->request->getPost('email');
        $password = $this->request->getPost('password');

        $user = [
            'name' => $name,
            'email' => $email,
            'password' => $password
        ];

        $save = $this->model->save($user);

        if ($save) {
            session()->setFlashdata('success', 'Register Berhasil!');
            return redirect()->to(base_url('login'));
        } else {
            session()->setFlashdata('error', $this->model->errors());
            return redirect()->back();
        }
    }
}
Sebentar, sebentar.. di file view register.php ada baris kode ini.

 <?= form_open('register'); ?>
Action formnya itu langsung mengarah ke register bukan ke method store() yang ada di controller RegisterController ya?

Kawan, di tutorial ini kita coba belajar menggunakan route yang ada di codeigniter 4. Action form kita arahkan ke route yang kita tentukan, bukan langsung diarahkan ke method yang ada di controller. Sekarang kita coba atur route terlebih dahulu untuk proses pendaftaran atau register akun. Buka file app/Config/Routes.php. Cari baris kode ini di bagian Route Definition:

$routes->get('/', 'Home::index');
Di bawah baris kode tersebut, kita definisikan route untuk register:


$routes->group('register', function($routes){
    $routes->get('/', 'RegisterController::index');
    $routes->post('/', 'RegisterController::store');
});
Setelah selesai mendefinisikan route, kita save kembali file app/Config/Routes.php.

Step 8: Membuat fitur login
Selanjutnya kita akan menambahkan fitur untuk login. Pertama kita buat controller baru dengan nama LoginController.php di direktori app/Controllers, lalu kita ketik baris kode di bawah ini:

<?php namespace App\Controllers;

use App\Models\UserModel;

class LoginController extends BaseController
{
    protected $model;

    public function __construct()
    {
        $this->model = new UserModel();
        $this->helpers = ['form', 'url'];
    }

    public function index()
    {
        if ($this->isLoggedIn()) {
            return redirect()->to(base_url('dashboard'));
        }

        $data = [
            'title' => 'Login | Seri Tutorial CodeIgniter 4: Login dan Register @ qadrlabs.com'
        ];

        return view('auth/login', $data);
    }

    private function isLoggedIn(): bool
    {
        if (session()->get('logged_in')) {
            return true;
        }

        return false;
    }
}
Di dalam controller LoginController.php terdapat dua method, yaitu method index() untuk menampilkan halaman login dan method isLoggedIn() untuk mengecek apakah user sudah dalam keadaan login.

Selanjutnya kita buat file Views baru untuk halaman login dengan nama login.php di direktori app/Views/auth. Di dalam file login.php, kita tambahkan kode berikut ini.

<?= $this->extend('layouts/main') ?>

<?= $this->section('content') ?>

<div class="container mt-5">
    <div class="row">
        <div class="col-md-4 offset-md-4">
            <div class="card">
                <div class="card-body">
                    <h4 class="text-center" style="font-weight: bold;">LOGIN</h4>
                    <hr>
                    <?php if (session()->getFlashdata('success')) { ?>
                        <div class="alert alert-success">
                            <?php echo session()->getFlashdata('success'); ?>
                        </div>
                    <?php } ?>

                    <?php if (session()->getFlashdata('error')) { ?>
                        <div class="alert alert-danger">
                            <?php echo session()->getFlashdata('error'); ?>
                        </div>
                    <?php } ?>

                    <?= form_open('login'); ?>
                    <div class="form-group">
                        <label for="email">Email</label>
                        <input type="email" name="email" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="password">Password</label>
                        <input type="password" name="password" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <button class="btn btn-primary">Login</button>
                    </div>
                    <?= form_close(); ?>

                </div>

            </div>
            <div class="text-center mt-2">
                Belum punya akun? <a href="<?php echo base_url('register'); ?>">Silakan daftar.</a>
            </div>
        </div>
    </div>
</div>

<?= $this->endSection() ?>
Oke, kita save dulu filenya sebelum melanjutkan.

Untuk menghandle proses login, kita tambahkan method baru di controller login kita. Kita buka lagi file LoginController.php, lalu kita tambahkan method login() di dalam class LoginController:

    ...kode sebelumnya

    public function login()
    {
        $email = $this->request->getPost('email');
        $password = $this->request->getPost('password');

        $credentials = ['email' => $email];

        $user = $this->model->where($credentials)
            ->first();

        if (! $user) {
            session()->setFlashdata('error', 'Email atau password anda salah.');
            return redirect()->back();
        }

        $passwordCheck = password_verify($password, $user['password']);

        if (! $passwordCheck) {
            session()->setFlashdata('error', 'Email atau password anda salah.');
            return redirect()->back();
        }

        $userData = [
            'name' => $user['name'],
            'email' => $user['email'],
            'logged_in' => TRUE
        ];

        session()->set($userData);
        return redirect()->to(base_url('dashboard'));
    }

    ... kode setelahnya
Sehingga keseluruhan file LoginController.php menjadi:

<?php namespace App\Controllers;

use App\Models\UserModel;

class LoginController extends BaseController
{
    protected $model;

    public function __construct()
    {
        $this->model = new UserModel();
        $this->helpers = ['form', 'url'];
    }

    public function index()
    {
        if ($this->isLoggedIn()) {
            return redirect()->to(base_url('dashboard'));
        }

        $data = [
            'title' => 'Login | Seri Tutorial CodeIgniter 4: Login dan Register @ qadrlabs.com'
        ];

        return view('auth/login', $data);
    }

    public function login()
    {
        $email = $this->request->getPost('email');
        $password = $this->request->getPost('password');

        $credentials = ['email' => $email];

        $user = $this->model->where($credentials)
            ->first();

        if (! $user) {
            session()->setFlashdata('error', 'Email atau password anda salah.');
            return redirect()->back();
        }

        $passwordCheck = password_verify($password, $user['password']);

        if (! $passwordCheck) {
            session()->setFlashdata('error', 'Email atau password anda salah.');
            return redirect()->back();
        }

        $userData = [
            'name' => $user['name'],
            'email' => $user['email'],
            'logged_in' => TRUE
        ];

        session()->set($userData);
        return redirect()->to(base_url('dashboard'));
    }

    private function isLoggedIn(): bool
    {
        if (session()->get('logged_in')) {
            return true;
        }

        return false;
    }
}
Berdasarkan alur skenario project login dan register codeigniter 4 yang sudah kita tentukan sebelumnya, pengguna dialihkan ke halaman dashboard setelah proses login berhasil. Kita buat file controller baru dengan nama Dashboard.php. Selanjutnya kita ketik baris kode di bawah ini:

<?php namespace App\Controllers;

class Dashboard extends BaseController
{
    public function index()
    {
        if (! $this->isLoggedIn()) {
            return redirect()->to('/');
        }

        $data = [
            'title' => 'Dashboard | Seri Tutorial CodeIgniter 4: Login dan Register @ qadrlabs.com'
        ];

        return view('dashboard', $data);
    }

    private function isLoggedIn() : bool
    {
        if (session()->get('logged_in')) {
            return true;
        }

        return false;
    }

}
Ya, pada baris kode di atas, terdapat dua method di dalam class Dashboard, yaitu method index() untuk menampilkan halaman dashboard dan isLoggedIn() untuk mengecek apakah user sudah login atau belum. Pada method index() terdapat algoritma untuk proteksi halaman di mana user tidak bisa mengakses halaman dashboard apabila user belum login. Selain itu, pada method index() terdapat kode return view('dashboard', $data);. Nah dari kode tersebut, kita tahu file view yang digunakan untuk halaman dashboard, yaitu dashboard.php.

Selanjutnya kita buat file views baru untuk halaman dashboard dengan nama dashboard.php di direktori app/Views, lalu kita sama-sama ketik baris kode di bawah ini:

<?= $this->extend('layouts/main') ?>

<?= $this->section('content') ?>
<section class="jumbotron text-center">
    <h1>Welcome, <?= session()->name ?></h1>
    <p>Untuk logout dari sistem silakan klik <a href="<?php echo base_url('logout');?>">Logout</a></p>
</section>

<?= $this->endSection() ?>
Ya, isi halaman dashboard ini kita buat sederhana. Di dalamnya ada contoh kode untuk menampilkan data session untuk nama dan ada link untuk logout.

Nah terakhir kita atur route untuk login. Buka kembali file app/Config/Routes.php lalu kita tambahkan route login di bawah route register.


$routes->group('login', function ($routes) {
    $routes->get('/', 'LoginController::index');
    $routes->post('/', 'LoginController::login');
});
Setelah selesai jangan lupa, disave filenya ya....

Step 9: Membuat fitur logout
Nah fitur terakhir, kita tambahkan fitur logout. Kita buat file controller baru untuk fitur logout dengan nama LogoutController.php di direktori app/Controller. Selanjutnya kita ketik baris kode ini.

<?php namespace App\Controllers;

class LogoutController extends BaseController
{
    public function index()
    {

        $userData = [
            'name',
            'email',
            'logged_in'
        ];

        session()->remove($userData);

        return redirect()->to(base_url('login'));

    }

    //--------------------------------------------------------------------

}
Dan terakhir kita atur route untuk fitur logout ini. Buka kembali file app/Config/Routes.php, lalu kita tambahkan route logout tepat di bawah route login:


$routes->group('logout', function ($routes) {
    $routes->get('/', 'LogoutController::index');
});
Save kembali file Routes.php

Ya, akhirnya selesai.

Kesimpulan
Setelah kita coba membuat project sederhana untuk menerapkan fitur login dan register menggunakan codeigniter 4, ada beberapa hal yang bisa kita simpulkan. Sejauh ini kodingan dari CodeIgnter 3 dan CodeIgniter 4 ternyata tidak terlalu beda jauh. Jadi kemungkinan untuk mempelajarinya pun tidak akan terlalu sulit. Ya, walaupun tidak jauh beda ada banyak potensi yang masih bisa kita kembangkan, misalnya bagian algoritma untuk menangani pengecekan apakah user itu sudah login atau belum. Selain itu, di tutorial ini kita juga menambahkan penggunaan route-nya. Ini menjawab pertanyaan dari salah satu anggota komunitas. Apakah route ini bermanfaat atau tidak? Kira-kira menurut kamu gimana? ^^
