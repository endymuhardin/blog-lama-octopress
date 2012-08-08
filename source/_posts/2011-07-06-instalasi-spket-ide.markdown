---
comments: true
date: 2011-07-06 19:16:13
layout: post
slug: instalasi-spket-ide
title: Instalasi Spket IDE
wordpress_id: 716
categories:
- Aplikasi
- Programming
---

Beberapa minggu terakhir ini, saya mencari-cari cara terbaik untuk melakukan development dengan ExtJS. 
Tentunya fitur utama yang kita inginkan adalah autocomplete, 
sehingga tidak perlu bolak-balik membaca dokumentasi di websitenya. 

Setelah berhari-hari mencari, akhirnya saya menemukan [Spket IDE](http://www.spket.com). 
Di websitenya dinyatakan bahwa Spket sudah mendukung ExtJS versi 4, membuat saya tertarik untuk mencobanya. 
Sayangnya, petunjuk instalasi sulit didapat, sehingga harus trial-and-error. 

Di artikel ini, kita akan membahas petunjuk instalasi Spket IDE di Eclipse Indigo.





## Klik menu Install New Software


{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/01.-Install-New-Software.png  %}




## Masukkan Update Site Spket IDE


Tambahkan Update Site yang baru
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/02.-Add-Update-Site.png  %}

Update sitenya adalah http://www.spket.com/update/

{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/03.-Lokasi-Update-Site-Spket.png  %}


## Opsi Instalasi Spket


{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/04.-Centang-Semua-Opsi.png  %}


## Klik Next


{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/05.-Next-Terus.png  %}


{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/06.-Sit-Back-and-Relax.png  %}


{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/07.-Ada-Warning-Lanjut-Saja.png  %}

Ada warning, klik saja Yes. 



## Download Support ExtJS 4



Agar bisa mengenali ExtJS 4, kita harus [mengunduh update terbaru dari forumnya](http://forums.spket.com/viewtopic.php?f=6&t=1866). 
Entah apa alasannya, tiap ada update baru, versi jarnya tidak dinaikkan dan update sitenya tidak diperbarui. 
Ini menyebabkan kita harus mengunduh file dari forum. 

Ada dua file yang harus diunduh, yaitu jar 
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/08.-Download-jar-terbaru.png  %}

dan jsb
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/09.-Dukungan-Ext-4.png  %}
Hasilnya, kita akan memiliki dua file. 
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/13.-Hasil-donlod-dari-forum-spket.png  %}


## Patch Eclipse


File jar akan kita pasang di folder plugins di tempat Eclipse terinstal.  
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/11.-Folder-Plugin-Eclipse.png  %}

Ini akan menimpa file dengan nama sama.
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/14.-Replace-dengan-yang-baru.png  %}



## Patch ExtJS


Sedangkan file jsb akan kita pasang di folder ExtJS 4. 
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/15.-Masukkan-jsb-ke-folder-extjs.png  %}


## Edit jsb


Sayangnya, file jsb ini juga masih ada bugnya. Dia salah menyebutkan nama file dalam folder pkgs. 
Kita harus edit, ganti `all.js` menjadi `classes.js`.
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/16.-Edit-file-jsb-sesuai-nama-file.png  %}


## Konfigurasi Spket


Selanjutnya, kita masuk ke menu preferences untuk melakukan konfigurasi.
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/17.-Konfigurasi-Spket.png  %}
Masuk ke menu Spket - Javascript Profile
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/18.-Javascript-Profile.png  %}
Tambah Profile baru, beri nama ExtJS
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/19.-Tambah-profile-baru.png  %}
Di profile yang baru saja ditambahkan, Add Library dan pilih ExtJS
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/20.-Tambah-library-ExtJS.png  %}
Setelah itu, Add File jsb yang sudah kita edit tadi. 
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/21.-Add-File-JSB.png  %}
Lalu, set profile ExtJS menjadi default
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/22.-Set-Profile-ExtJS-sebagai-default.png  %}
Kemudian, pergi ke menu General - Editors - File Associations. Pilih file js, dan jadikan Spket sebagai editornya. 
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/23.-Set-file-associations.png  %}
Klik Ok, restart Eclipse. 
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/10.-Restart-Eclipse.png  %}


## Code Completion


Sekarang kita bisa melakukan code completion pada saat memberi titik di depan object. 
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/24.-Autocomplete-method.png  %}
Atau juga pada saat mengetik di dalam tanda kurung. 
{% img http://endy.artivisi.com/blog/wp-content/uploads/2011/07/25.-Autocomplete-property.png  %}

Demikianlah cara instalasi Spket IDE di Eclipse. 
