# Jaringan Komputer C-09 (2021)
Laporan Resmi Jaringan Komputer Modul 2 - DNS and Web Server

NRP              | Nama
-----------------|-----------
05111940000014   | Ega Prabu Pamungkas
05111940000178   | Muhammad Rizqullah Akbar
05111940000227   | Rayhan Daffa Alhafish

# Nomor 1
### Soal
*EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet (1).*
### Jawaban

# Nomor 2
### Soal
*Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses **franky.yyy.com** dengan alias **www.franky.yyy.com** pada folder kaizoku (2).*
### Jawaban

# Nomor 3
### Soal
*Setelah itu buat subdomain super.franky.yyy.com dengan alias **www.super.franky.yyy.com** yang diatur DNS nya di EniesLobby dan mengarah ke Skypie(3).*
### Jawaban

# Nomor 4
### Soal
*Buat juga reverse domain untuk domain utama (4).*
### Jawaban

# Nomor 5
### Soal
*Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama (5).*
### Jawaban

# Nomor 6
### Soal
*Setelah itu terdapat subdomain **mecha.franky.yyy.com** dengan alias **www.mecha.franky.yyy.com** yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo(6).*
### Jawaban

# Nomor 7
### Soal
*Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama **general.mecha.franky.yyy.com** dengan alias **www.general.mecha.franky.yyy.com** yang mengarah ke Skypie(7).*
### Jawaban
Pada file `/etc/bind/delegasi/mecha.franky.com` ditambahkan konfigurasi subdomain sebagai berikut.
```
general         IN      A       192.188.2.4 ; IP Skypie
www.general     IN      A       192.188.2.4 ; IP Skypie
```

![7](/img/7-ping-general-franky-alias.jpeg)

# Nomor 8
### Soal
*(8) Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver **www.franky.yyy.com**. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com.*
### Jawaban
Pada Script di Web Server Skypie dibuatkan directory baru dengan perintah `mkdir /var/www/franky.C09.com`. Lalu pada konfigurasi Apache di file `/etc/apache2/sites-available/franky.C09.com.conf` ditambahkan konfigurasi `DocumentRoot /var/www/franky.C09.com`.

![8](img/8-9-lynx-franky-franky=home.jpeg)

# Nomor 9
### Soal
*(9) Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home.*
### Jawaban
Dilakukan konfigurasi pada file /.htaccess untuk melakukan *rewrite* menggunakan **REGEX** dengan konfigurasi berikut.
```
 RewriteEngine On
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule ^([^\.]+)$ index.php/$1 [NC,L]
```
Dengan `RewriteCond %{REQUEST_FILENAME} !-d` berarti melakukan *rewrite* ketika kondisi request file dan tidak boleh sebuah direktori. selanjutnya `RewriteRule ^([^\.]+)$ index.php/$1 [NC,L]` merupakan aturan **REGEX**-nya, dimana `NC` berarti *Non Case Sensitive* dan `L` berarti ketika kondisi `index.php/$1` maka `rewrite rule tidak berjalan`.

![9](img/8-9-lynx-franky-franky=home.jpeg)

# Nomor 10
### Soal
*Setelah itu, pada subdomain **www.super.franky.yyy.com**, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com.*
### Jawaban
ada Script di Web Server Skypie dibuatkan directory baru dengan perintah `mkdir /var/www/super.franky.C09`. Lalu pada konfigurasi Apache di file `/etc/apache2/sites-available/super.franky.C09.com.conf` ditambahkan konfigurasi `DocumentRoot /var/www/super.franky.C09`.

![10](img/10-lynx-super.jpeg)

# Nomor 11
### Soal
*(11) Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja.*
### Jawaban
Untuk melakukan *directory listing* maka pada file `/etc/apache2/sites-available/super.franky.C09.com.conf` ditambahkan konfigurasi sebagai berikut.
```
  <Directory /var/www/super.franky.C09/public/css>
                Options +Indexes
                IndexOptions Showforbidden
                <FilesMatch "\.css$">
                        Deny from all
                </FilesMatch>
        </Directory>

        <Directory /var/www/super.franky.C09/public/images>
                Options +Indexes +FollowSymLinks -Multiviews
                AllowOverride All
                IndexOptions Showforbidden
        </Directory>

        <Directory /var/www/super.franky.C09/public/js>
                Options +Indexes
                IndexOptions Showforbidden
                <FilesMatch "\.js$">
                        Deny from all
                </FilesMatch>
        </Directory>

```
Dimana di konfigurasi juga ditambahkan pembatasan akses agar file tidak bisa dibuka, sehingga hanya *directory listing* saja.

![11.1](img/11.1-dir-list-public-super.jpeg)

![11.2](img/11.2-access-denied-file.jpeg)

# Nomor 12
### Soal
*(12) Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache.*
### Jawaban
Untuk menyiapkan itu maka dimasukan file 404.html terlebih dulu di direktori `/error`, Selanjutnya ditambahkan konfigurasi pada file `/etc/apache2/sites-available/super.franky.C09.com.conf` dengan `ErrorDocument 404 /error/404.html` dengan ini ketika kita mengakses file atau alamat yang tidak tersedia akan menampilkan file 404.html sebagai pesan error.

![12](img/12-error-html.jpeg)

# Nomor 13
### Soal
*(13) Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset **www.super.franky.yyy.com/public/js** menjadi **www.super.franky.yyy.com/js**.*
### Jawaban

# Nomor 14
### Soal
*(14) Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500.*
### Jawaban

# Nomor 15
### Soal
*(15) dengan autentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy.*
### Jawaban

# Nomor 16
### Soal
*(16)  Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke **www.franky.yyy.com**.*
### Jawaban

# Nomor 17
### Soal
*(17). Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website **www.super.franky.yyy.com**, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png. Maka bantulah Luffy untuk membuat konfigurasi dns dan web server ini!*
### Jawaban



