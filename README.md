# CodeIgniter 4 

## Apa itu CodeIgniter4 ?

CodeIgniter adalah framework web full-stack PHP yang ringan, cepat, fleksibel dan aman.
Informasi lebih lanjut dapat ditemukan di [situs resmi](https://codeigniter.com).

# > Instalasi
## *Instalasi Composer
Komposer dapat digunakan dalam beberapa cara untuk menginstal CodeIgniter4 di sistem Anda.
> [!IMPORTANT]
> CodeIgniter4 memerlukan Composer 2.0.14 atau lebih baru.

Teknik pertama menjelaskan pembuatan proyek kerangka menggunakan CodeIgniter4, yang kemudian akan Anda gunakan sebagai dasar untuk aplikasi web baru. Teknik kedua yang dijelaskan di bawah ini memungkinkan Anda menambahkan CodeIgniter4 ke aplikasi web yang sudah ada,

>[!NOTE]
>Jika Anda menggunakan repositori Git untuk menyimpan kode Anda, atau untuk berkolaborasi dengan orang lain, maka folder vendor biasanya akan “git diabaikan”. Dalam kasus seperti ini, Anda perlu melakukan hal berikut saat mengkloning repositori ke sistem baru.`composer update`

### Buat Project Baru
   Instalasi ini digunakan untuk pemula yang baru akan membuart project dari awal
   Pada folder Root anda
   ```shell
   "composer create-project codeigniter4/appstarter project-root"
   ```
   Perintah di atas akan membuat folder root proyek .
   Jika Anda menghilangkan argumen “project-root”, perintah akan membuat folder “appstarter”, yang dapat diganti namanya sesuai kebutuhan.

> [!IMPORTANT]
   >Saat Anda menerapkan ke server produksi, jangan lupa untuk menjalankan perintah berikut:
   >```shell
   >composer install --no-dev
   >```
   >perintah di atas akan menghapus paket Composer hanya untuk pengembangan yang tidak diperlukan di lingkungan produksi. Ini akan sangat mengurangi ukuran folder vendor.

   
### Menambahkan CodeIgniter4 ke project yang ada
Di root project anda ketikan sebagai berikut :
```shell
composer require codeigniter4/framework
```

# > Bangun Aplikasi Pertama Anda
## *Halaman Statis
Hal pertama yang akan Anda lakukan adalah menyiapkan aturan perutean untuk menangani halaman statis.

Perutean mengaitkan URI dengan metode pengontrol. Pengontrol hanyalah sebuah kelas yang membantu mendelegasikan pekerjaan. Kami akan membuat pengontrol nanti.
Mari kita siapkan aturan perutean. Buka file rute yang terletak di app/Config/Routes.php .
Satu-satunya petunjuk rute untuk memulai adalah:

```shell
<?php

use CodeIgniter\Router\RouteCollection;

/**
 * @var RouteCollection $routes
 */
$routes->get('/', 'Home::index');
```
Arahan ini mengatakan bahwa setiap permintaan masuk tanpa konten apa pun yang ditentukan harus ditangani oleh index()metode di dalam Homepengontrol.

### Buat Pengontrol Halaman
Buat file di app/Controllers/Pages.php dengan kode berikut.

```shell
<?php

namespace App\Controllers;

class Pages extends BaseController
{
    public function index()
    {
        return view('welcome_message');
    }

    public function view($page = 'home')
    {
        // ...
    }
}
```
Anda telah membuat kelas bernama Pages, dengan view()metode yang menerima satu argumen bernama $page. Ia juga memiliki index()metode, sama dengan pengontrol default yang ditemukan di app/Controllers/Home.php ; metode itu menampilkan halaman selamat datang CodeIgniter.

### Buat Tampilan
Buat header di app/Views/templates/header.php dan tambahkan kode berikut:
```shell
        <!doctype html>
    <html>
    <head>
        <title>Falah Putra Ardika</title>
    </head>
    <body>

        <h1><?= esc($title) ?></h1>
        <h2>FPA</h2>
        <h3>84</h3>
        <h4>ti2d</h4>
```
Header berisi kode HTML dasar yang ingin Anda tampilkan sebelum memuat tampilan utama, bersama dengan judul. Ini juga akan menampilkan $titlevariabel, yang akan kita definisikan nanti di pengontrol. Sekarang, buat footer di app/Views/templates/footer.php yang menyertakan kode berikut:
```shell
<em>&copy; 2024</em>
</body>

</html>
```

### Menambahkan Logika ke Kontroler
Untuk memuat halaman tersebut, Anda harus memeriksa apakah halaman yang diminta benar-benar ada. Ini akan menjadi isi metode view()pada Pagespengontrol yang dibuat di atas:
```shell
<?php

namespace App\Controllers;

use CodeIgniter\Exceptions\PageNotFoundException; // Add this line

class Pages extends BaseController
{
    // ...

    public function view($page = 'home')
    {
        if (! is_file(APPPATH . 'Views/pages/' . $page . '.php')) {
            // Halaman tidak ada
            throw new PageNotFoundException($page);
        }

        $data['title'] = ucfirst($page); // Gunakan Huruf Kapital pada abjad pertama

        return view('templates/header', $data)
            . view('pages/' . $page)
            . view('templates/footer');
    }
}
```
Hasilnya sebagai berikut :

![image](https://github.com/Fa1ahputr4/Tugas1/assets/134368686/9119faec-e884-473e-b17d-ebe9b1b2eb7c)

## *Struktur Aplikasi
Untuk mendapatkan hasil maksimal dari CodeIgniter, Anda perlu memahami bagaimana struktur aplikasi, secara default, dan apa yang dapat Anda ubah untuk memenuhi kebutuhan aplikasi Anda.

### Aplikasi
aplikasi Direktori adalah tempat semua kode aplikasi Anda berada. Ini hadir dengan struktur direktori default yang berfungsi dengan baik untuk banyak aplikasi. Folder berikut membentuk konten dasar. Karena appdirektori sudah diberi namespace, Anda bebas memodifikasi struktur direktori ini agar sesuai dengan kebutuhan aplikasi Anda. Misalnya, Anda mungkin memutuskan untuk mulai menggunakan pola Repositori dan Model Entitas untuk bekerja dengan data Anda. Dalam hal ini, Anda dapat mengganti nama Modelsdirektori menjadi Repositories, dan menambahkan direktori baru Entities.Semua file dalam direktori ini berada di bawah Appnamespace, meskipun Anda bebas mengubahnya di app/Config/Constants.php .

### System
Direktori ini menyimpan file-file yang membentuk kerangka itu sendiri. Meskipun Anda memiliki banyak fleksibilitas dalam cara menggunakan direktori aplikasi, file dalam direktori sistem tidak boleh diubah. Sebaliknya, Anda harus memperluas kelas, atau membuat kelas baru, untuk menyediakan fungsionalitas yang diinginkan.Semua file dalam direktori ini berada di bawah CodeIgniternamespace.

### Publik
Folder publik menampung bagian aplikasi web Anda yang dapat diakses browser, mencegah akses langsung ke kode sumber Anda. Ini berisi file .htaccess utama , index.php , dan aset aplikasi apa pun yang Anda tambahkan, seperti CSS, javascript, atau gambar.Folder ini dimaksudkan sebagai “root web” situs Anda, dan server web Anda akan dikonfigurasi untuk mengarah ke folder tersebut.

### Writeble
Direktori ini menampung semua direktori yang mungkin perlu ditulisi selama masa pakai aplikasi. Ini termasuk direktori untuk menyimpan file cache, log, dan unggahan apa pun yang mungkin dikirim pengguna. Anda harus menambahkan direktori lain tempat aplikasi Anda perlu menulis di sini. Hal ini memungkinkan Anda untuk menjaga direktori utama lainnya tidak dapat ditulisi sebagai langkah keamanan tambahan.

### Test
Direktori ini disiapkan untuk menyimpan file pengujian Anda. Direktori ini _supportmenampung berbagai kelas tiruan dan utilitas lain yang dapat Anda gunakan saat menulis pengujian Anda. Direktori ini tidak perlu ditransfer ke server produksi Anda.

## *Mode;, View, dan Controller
### Apa itu MVC?
Setiap kali Anda membuat aplikasi, Anda harus menemukan cara untuk mengatur kode agar mudah menemukan file yang tepat dan memudahkan pemeliharaan. Seperti kebanyakan kerangka web, CodeIgniter menggunakan pola Model, View, Controller (MVC) untuk mengatur file. Hal ini menjaga data, presentasi, dan aliran melalui aplikasi sebagai bagian yang terpisah.

### Model
Model adalah bagian dari aplikasi yang bertanggung jawab atas data. Mereka menangani penyimpanan dan pengambilan data dari database, serta menerapkan aturan bisnis pada data tersebut. Misalnya, model dapat memastikan bahwa data yang dimasukkan ke dalam database sesuai dengan standar tertentu.

### View
View adalah file yang sederhana dan biasanya berbentuk HTML dengan sedikit kode PHP. Mereka bertugas menampilkan informasi kepada pengguna, seperti teks atau tabel, yang diterima dari pengontrol. Contoh penggunaan tampilan adalah menampilkan halaman profil pengguna atau daftar postingan.

### Controller
Pengendali merupakan bagian yang menghubungkan pengguna dengan aplikasi. Mereka menerima masukan dari pengguna, seperti klik tombol atau permintaan halaman, dan menentukan tindakan yang harus dilakukan selanjutnya. Pengendali juga menangani tugas-tugas terkait permintaan HTTP, seperti verifikasi pengguna atau pengalihan halaman.

## *Bagian Berita
Di bagian ini, kita membahas beberapa konsep dasar kerangka kerja dengan menulis kelas yang mereferensikan halaman statis. Kami membersihkan URI dengan menambahkan aturan perutean khusus. Sekarang saatnya memperkenalkan konten dinamis dan mulai menggunakan database.

### Buat Database
Anda perlu membuat database ci4tutor yang dapat digunakan untuk tutorial ini, dan kemudian mengkonfigurasi CodeIgniter untuk menggunakannya.
Menggunakan klien database Anda, sambungkan ke database Anda dan jalankan perintah SQL di bawah ini (MySQL):
```shell
CREATE TABLE news (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    title VARCHAR(128) NOT NULL,
    slug VARCHAR(128) NOT NULL,
    body TEXT NOT NULL,
    PRIMARY KEY (id),
    UNIQUE slug (slug)
);
```
Lalu isi database dengan perintah berikut :
```shell
INSERT INTO news VALUES
(1, 'Pembangunan Infrastruktur', 'pembangunan-infrastruktur', 'Pemerintah mengumumkan proyek pembangunan infrastruktur baru untuk meningkatkan konektivitas di seluruh Indonesia.'),
(2, 'Penemuan Spesies Baru', 'penemuan-spesies-baru', 'Ilmuwan menemukan spesies baru kadal yang hanya ditemukan di hutan hujan Kalimantan.'),
(3, 'Peningkatan Ekonomi', 'peningkatan-ekonomi', 'Bank Indonesia melaporkan pertumbuhan ekonomi yang kuat, didorong oleh sektor manufaktur dan ekspor.');
```

### Hubungkan ke Basis Data
File konfigurasi lokal, .env , yang Anda buat saat menginstal CodeIgniter, harus memiliki pengaturan properti database yang tidak diberi komentar dan disetel dengan tepat untuk database yang ingin Anda gunakan. Pastikan Anda telah mengkonfigurasi database Anda dengan benar seperti yang dijelaskan dalam Konfigurasi Database :
```shell
database.default.hostname = localhost
database.default.database = ci4tutor
database.default.username = root
database.default.password = root
database.default.DBDriver = MySQLi
```

### Buat Model
Buka direktori app/Models dan buat file baru bernama NewsModel.php dan tambahkan kode berikut.
``` shell
<?php

namespace App\Models;

use CodeIgniter\Model;

class NewsModel extends Model
{
    protected $table = 'news';
}
```
Kode ini terlihat mirip dengan kode pengontrol yang digunakan sebelumnya. Ini menciptakan model baru dengan memperluas CodeIgniter\Modeldan memuat perpustakaan database. Ini akan membuat kelas database tersedia melalui $this->dbobjek.

### Buat Metode NewsModel::getNews()
``` shell
public function getNews($slug = false)
    {
        if ($slug === false) {
            return $this->findAll();
        }

        return $this->where(['slug' => $slug])->first();
    }
```
Dengan kode ini, Anda dapat melakukan dua kueri berbeda. Anda bisa mendapatkan semua catatan berita, atau mendapatkan item berita melalui siputnya. Anda mungkin memperhatikan bahwa $slugvariabel tidak di-escape sebelum menjalankan kueri; Pembuat Kueri melakukan ini untuk Anda.Dua metode yang digunakan di sini, findAll()dan first(), disediakan oleh CodeIgniter\Modelkelas. Mereka sudah mengetahui tabel yang akan digunakan berdasarkan $table properti yang kita atur di NewsModelkelas tadi. Mereka adalah metode pembantu yang menggunakan Pembuat Kueri untuk menjalankan perintahnya pada tabel saat ini, dan mengembalikan serangkaian hasil dalam format pilihan Anda. Dalam contoh ini, findAll()mengembalikan array dari array.

### Tambahkan routs
Ubah file app/Config/Routes.php Anda , sehingga terlihat seperti berikut:
```shell
<?php

// ...

use App\Controllers\News; // Add this line
use App\Controllers\Pages;

$routes->get('news', [News::class, 'index']);           // Add this line
$routes->get('news/(:segment)', [News::class, 'show']); // Add this line

$routes->get('pages', [Pages::class, 'index']);
$routes->get('(:segment)', [Pages::class, 'view']);
```

### Buat Controller untuk berita
Buat pengontrol baru di app/Controllers/News.php .
```shell
<?php

namespace App\Controllers;

use App\Models\NewsModel;

class News extends BaseController
{
    public function index()
    {
        $model = model(NewsModel::class);

        $data['news'] = $model->getNews();
    }

    public function show($slug = null)
    {
        $model = model(NewsModel::class);

        $data['news'] = $model->getNews($slug);
    }
}
```
### Buat Index Method
```shell
<?php

namespace App\Controllers;

use App\Models\NewsModel;

class News extends BaseController
{
    public function index()
    {
        $model = model(NewsModel::class);

        $data = [
            'news'  => $model->getNews(),
            'title' => 'News archive',
        ];

        return view('templates/header', $data)
            . view('news/index')
            . view('templates/footer');
    }

    // ...
}
```

### Buat View Indexs
Buat app/Views/news/index.php dan tambahkan potongan kode berikutnya.
```shell
<h2><?= esc($title) ?></h2>

<?php if (! empty($news) && is_array($news)): ?>

    <?php foreach ($news as $news_item): ?>

        <h3><?= esc($news_item['title']) ?></h3>

        <div class="main">
            <?= esc($news_item['body']) ?>
        </div>
        <p><a href="/news/<?= esc($news_item['slug'], 'url') ?>">View article</a></p>

    <?php endforeach ?>

<?php else: ?>

    <h3>No News</h3>

    <p>Unable to find any news for you.</p>

<?php endif ?>
```
### Berikut Hasilnya 
![image](https://github.com/Fa1ahputr4/Tugas1/assets/134368686/9e5c4efe-19f0-4ee0-950c-ba6738c9b6d9)

