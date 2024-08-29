---
layout: post
title:  "Instal Nginx di Ubuntu Server 24.04"
categories: Server Linux
tags: server linux ubuntu Nginx
---

* content
{:toc}

## Intro

[Nginx][Nginx]{:target="_blank"} adalah salah satu web server yang populer. Karakteristiknya ringan tapi mampu berfungsi sebagai web server sekaligus reverse proxy.

Di sini kita mau nginstal Nginx pada server <mark>Ubuntu 24.04</mark>.





# Bahan-bahan

* Ubuntu Server 24.04.
* User yang punya hak sudo.

# Langkah 1 - Instal Nginx

1. Update dulu daftar package agar mendapatkan yang terbaru.
```console
$ sudo apt update
```

1. Lanjutkan instal Nginx.
```console
$ sudo apt install nginx
```
Tekan `Y` untuk mengkonfirmasi.

# Langkah 2 - Memeriksa Status Web Server

Untuk memeriksa status web server apakah sudah berjalan atau tidak bisa dengan cara:

* Melalui terminal:
```console
$ systemctl status nginx
```

* Diakses dari browser:
```console
http://localhost
```

# Langkah 3 - Manajemen Proses Nginx

Seperti proses-proses lain yang berjalan pada Linux, proses Nginx bisa diatur apabila diperlukan dengan beberapa perintah dasar:

* Menghentikan web server:
	```console
	$ sudo systemctl stop nginx
	```
* Menjalankan kembali setelah dihentikan:
	```console
	$ sudo systemctl start nginx
	```
* Menjalankan ulang:
	```console
	$ sudo systemctl restart nginx
	```
* Reload digunakan jika ingin menerapkan perubahan konfigurasi tanpa memutuskan koneksi ke web server:
	```console
	$ sudo systemctl reload nginx
	```
* Saat server dinyalakan, Nginx akan otomatis berjalan. Jika tidak ingin Nginx berjalan secara otomatis maka service-nya bisa dimatikan:
	```console
	$ sudo systemctl disable nginx
	```
* Untuk mengaktifkannya kembali:
	```console
	$ sudo systemctl enable nginx
	```

# Langkah 4 - Membuat Blok Server

Blok server kalau di IIS bisa disebut site (website). Kalau ada beberapa site dalam satu server maka dibuatkan blok server.

Di sini kita membuat site baru agar tidak mengganggu bawaan Nginx.

1. Buat folder baru
	```console
	$ sudo mkdir -p /var/www/your_domain/html
	```
1. Mengatur kepemilikan folder
    ```console
	$ sudo chown -R $USER:$USER /var/www/your_domain/html
	```
1. Mengatur permission
     ```console
	$ sudo chmode -R 755 /var/www/your_domain/html
	```
1. Membuat file `index.html` menggunakan text editor `nano`.
    ```console
	nano /var/www/your_domain/html/index.html
	```
1. Masukkan contoh HTML.
    ```html
	<html>
		<head>
			<title>Selamat datang di web saya!</title>
		</head>
		<body>
			<h1>Halo!  Ini web saya sudah jalan!</h1>
		</body>
	</html>
	```
	Tekan `Ctrl+O` dan `Ctrl+X` untuk menyimpan dan menutup file.
1. Buat server blok untuk site ini.
    ```console
	$ sudo nano /etc/nginx/sites-available/your_domain
	```
1. Kopikan pengaturan site default Nginx
    ```
	server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
	}
	```
	Jangan lupa arahkan `root` dan `server_name` ke domain yang benar.
1. Sekarang buat symbolic link agar site yang kita buat di-enable.
    ```console
	$ sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
	```
1. Periksa pengaturan apakah ada kesalahan.
    ```console
	$ sudo nginx -t
	```
1. Jika statusnya `ok`, restart Nginx untuk menjalankan konfigurasi yang baru.
    ```console
    $ sudo systemctl restart nginx
	```
1. Buka web browser dan arahkan ke `http://your_domain`.


# Referensi

- [Nginx][Nginx]{:target="_blank"}
- [https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04){:target="_blank"}


[Nginx]:	https://www.f5.com/go/product/welcome-to-nginx