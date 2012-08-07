---
comments: true
date: 2005-11-22 14:59:04
layout: post
slug: subversion-backup-dan-restore
title: Subversion backup dan restore
wordpress_id: 15
categories:
- Aplikasi
---

Setelah repository terpasang dan telah terisi struktur folder awal, hal pertama yang wajib dilakukan adalah memasang perangkat backup otomatis. Berikut adalah checklistnya : 

1. Buat folder khusus backup (misalnya /home/endy/backup-svn)

2. [Download script backup](http://pragmaticprogrammer.com/titles/svn/code.html) dari buku [Pragmatic Version Control Using Subversion](http://pragmaticprogrammer.com/titles/svn/index.html). 
Ada dua script yang didapat di sini, weekly-backup.pl dan daily-backup.pl
Kedua script butuh instalasi Perl untuk dapat dijalankan. 

3. Letakkan script di folder backup

4. Sesuaikan permission : 

    
    <code> $ chmod 755 *.pl </code>



5. Test eksekusi script

    
    <code>$ ./weekly-backup.pl
    $ ./daily-backup.pl </code>


Seharusnya akan ada dua file : weekly-full-backup.yyyyMMdd.gz dan daily-incremental-backup.yyyyMMdd.gz

6. Otomasi dengan cron

    
    <code> # Repository Weekly Backup
    0 6 * * 1       /home/endy/backup-svn/weekly-backup.pl
    # Dijalankan setiap Senin jam 6 pagi.
    
    # Repository Daily Backup
    0 23 * * *      /home/endy/backup-svn/daily-backup.pl
    # Dijalankan setiap hari jam 23 malam
    </code>


7. Setiap Senin pagi, tulis semua file dari Senin minggu lalu sampai Minggu malam ke dalam CD. Simpan CD di lokasi yang berbeda (gedung lain, kota lain, negara lain, planet lain bila perlu). 

Bila semua ini dijalankan, cuma butuh 10 menit, sudah termasuk membaca artikel ini. 
Jangan sia-siakan hidup Anda hanya karena malas investasi 10 menit. 

Apabila terjadi bencana, dan repository rusak, kita tinggal ambil CD terbaru, dan dump dengan cara : 
1. Install ulang Subversion
2. Buat repository
3. Unzip dump file 

    
    <code>$ gunzip *.gz</code>



4. Load ke repository baru

    
    <code>$ svnadmin load newrepos < weekly-full-backup.yyyyMMdd
    $ svnadmin load newrepos < daily-incremental-backup.yyyyMMdd
    </code>


Dimulai dari tanggal paling lama, sampai tanggal terbaru.
