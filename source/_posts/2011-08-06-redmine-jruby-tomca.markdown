---
comments: true
date: 2011-08-06 21:06:01
layout: post
slug: redmine-jruby-tomca
title: Instalasi Redmine di Tomcat
wordpress_id: 750
categories:
- Aplikasi
- Linux
---

Ada berbagai cara instalasi Redmine, diantaranya:



    
  * Dijalankan langsung dari command prompt dengan Webrick

    
  * Dijalankan menggunakan Mongrel dan FastCGI

    
  * Dijalankan menggunakan Ruby Enterprise Edition dan Passenger

    
  * Dibuat menjadi war dan dideploy ke application server Java seperti Tomcat, Glassfish, dsb



Pada artikel ini, kita akan mencoba cara terakhir, yaitu menggunakan Tomcat untuk menghosting Redmine.
Ini saya lakukan supaya semua tools manajemen proyek ArtiVisi bisa disatukan di satu Tomcat, sehingga memudahkan kegiatan maintenance.
Sebelum Redmine, Tomcat ArtiVisi juga menghosting :

    
  * [Nexus](http://nexus.sonatype.org/)

    
  * [Jenkins](http://jenkins-ci.org/)



Dan nantinya, kalau sudah ada waktu dan kesempatan, juga akan menghosting [Gerrit](http://code.google.com/p/gerrit/)

Mari kita mulai.




## Instalasi JRuby



Pertama, kita [Download JRuby](http://jruby.org/). Setelah itu, extract di folder yang diinginkan (contohnya /opt)

[gist id=1129237 file=install-jruby.sh]

Daftarkan jruby ke variabel PATH, supaya bisa diakses langsung dari command line.
Tulis baris berikut ini di dalam file ~/.bashrc

[gist id=1129237 file=bashrc]


Terakhir, test instalasi JRuby

[gist id=1129237 file=test-jruby.sh]



## Instalasi Paket Gem


Redmine membutuhkan beberapa library Ruby yang dipaket dalam format gem, yaitu :



    
  * rack versi 1.1.1 : ini adalah library untuk web server

    
  * rails versi 2.3.11 (dibutuhkan karena kita akan menginstal Redmine dari Subversion, bukan dari distribusi)

    
  * jruby-openssl : supaya bisa melayani https

    
  * activerecord-jdbcmysql-adapter : library untuk koneksi database

    
  * warbler : packager supaya Redmine bisa dibuat jadi war dan dideploy ke Tomcat



Mari kita install

[gist id=1129237 file=install-gem.sh bump=1]

Semua paket sudah lengkap, mari kita lanjutkan ke



## Mengambil Redmine dari Subversion Repository


Sebetulnya ada dua pilihan untuk mendapatkan Redmine, download versi rilis atau checkout langsung dari Subversion.
Saya lebih suka checkout langsung supaya nanti lebih gampang upgrade manakala rilis baru sudah terbit.

[gist id=1129237 file=checkout-redmine.sh]

Tunggu sejenak sampai proses checkout selesai. Setelah selesai, kita bisa langsung ke langkah selanjutnya.




## Konfigurasi Database



Masuk ke folder Redmine, lalu copy file config/database.yml.example ke database.yml, kemudian edit.
Saya menggunakan konfigurasi development sebagai berikut :

[gist id=1129237 file=database.yml]

Tentunya kita harus sediakan database dengan konfigurasi tersebut di MySQL. Login ke MySQL, kemudian buatlah database dan usernya.

[gist id=1129237 file=create-database.sh]

Setelah databasenya selesai dibuat, selanjutnya kita akan melakukan inisialisasi.



## Inisialisasi Redmine



Pertama, kita inisialisasi dulu session store. Ini digunakan untuk menyimpan cookie dan session variabel.

[gist id=1129237 file=inisialisasi-session.sh]

Setelah itu, inisialisasi skema database.

[gist id=1129237 file=inisialisasi-skema-db.sh]

Isi data awal.

[gist id=1129237 file=inisialisasi-data-awal.sh]

Setelah terisi, selanjutnya kita bisa test jalankan Redmine.

[gist id=1129237 file=start-webrick.sh]

Hasilnya bisa kita browse di http://localhost:3000
Kemudian kita bisa login dengan username admin dan password admin.



## Konfigurasi Email


Issue tracker yang baik harus bisa mengirim email, supaya dia bisa memberikan notifikasi pada saat ada issue baru ataupun perubahan terhadap issue yang ada.
Redmine versi 1.2 membutuhkan file konfigurasi yang bernama configuration.yml, berada di folder config. Berikut isi file configuration.yml untuk mengirim email ke Gmail.

[gist id=1129237 file=configuration.yml]

Selain itu, kita juga harus menginstal plugin action_mailer_optional_tls, seperti dijelaskan [di sini](http://redmineblog.com/articles/setup-redmine-to-send-email-using-gmail/). 

[gist id=1129237 file=install-action-mailer-tls.sh]

Coba restart Redmine, sesuaikan alamat email kita dengan cara klik link My Account di pojok kanan atas.
Di dalamnya ada informasi tentang email. Ganti dengan alamat email kita.
Kemudian pergi ke menu Administration > Settings > Email Notifications,
kemudian klik link Send a test email di pojok kanan bawah.
Tidak lama kemudian, seharusnya test email dari Redmine sudah masuk di mailbox kita.

Dengan demikian, Redmine sudah berhasil kita instal dan konfigurasi dengan baik.
Selanjutnya, kita akan paketkan supaya bisa dideploy di Tomcat.



## Generate WAR



Pertama, kita harus inisialisasi dulu konfigurasi warble.

[gist id=1129237 file=warble-config.sh]

Dia akan menghasilkan file config/warble.rb. Mari kita edit sehingga menjadi seperti ini.

[gist id=1129237 file=warble.rb]

Selanjutnya, kita tinggal menjalankan perintah warble untuk menghasilkan file war.

[gist id=1129237 file=warble-generate.sh]

File war yang dihasilkan tinggal kita deploy ke Tomcat

[gist id=1129237 file=redmine-war-deploy.sh]

Jalankan Tomcat, dan Redmine bisa diakses di http://localhost:8080/redmine

