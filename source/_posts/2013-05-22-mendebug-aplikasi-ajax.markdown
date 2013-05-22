---
layout: post
title: "Mendebug Aplikasi AJAX"
date: 2013-05-22 16:46
comments: true
categories: 
- java
---

Di jaman modern ini, penggunaan AJAX sudah sangat mendunia, sehingga jarang sekali kita temui aplikasi yang tidak menggunakan AJAX. Demikian juga dengan template aplikasi standar yang digunakan di ArtiVisi, yaitu [Belajar RESTful](https://github.com/endymuhardin/belajar-restful).

Bagi yang baru mendengar atau sering dengar tapi belum paham, berikut penjelasan singkat mengenai AJAX. 

* AJAX adalah salah satu teknik pemrograman web
* AJAX bukanlah nama library atau framework, dia adalah cara membuat aplikasi
* Pada aplikasi non-AJAX, siklusnya sebagai berikut : 

    1. Ketik URL
    2. Tampil HTML
    3. Lakukan sesuatu (isi form + tekan submit)
    4. Browser mengirim data
    5. Server mengembalikan HTML full satu halaman


* Pada aplikasi AJAX, request ke server dilakukan oleh javascript, bukan oleh form submit. 
* Response dari server biasanya hanya berupa data dalam format XML atau JSON, bukan data + tampilan seperti HTML
* Data yang dikirim server digunakan javascript hanya untuk mengubah sebagian halaman tampilan, bukan seluruh halaman seperti aplikasi non-AJAX. 

Karena perbedaan cara kerja aplikasi AJAX, maka cara debug errornya juga berbeda. Untuk mendebug aplikasi AJAX, kita perlu melengkapi browser yang kita gunakan supaya bisa memantau request dan response yang dilakukan aplikasi. 

Pada artikel ini, kita akan membahas bagaimana cara mendebug aplikasi AJAX. Sebagai contoh kasus, kita akan gunakan aplikasi [Belajar RESTful](https://github.com/endymuhardin/belajar-restful).

<!--more-->

## Studi Kasus ##

Sebagai contoh, kita akan mendebug form entri user berikut

{% img /images/uploads/2013/05/debug-ajax/01-screen-form-user.png Form Entri User %}

dan tampilan daftar user berikut

{% img /images/uploads/2013/05/debug-ajax/02-screen-list-user.png Daftar User %}


## Persenjataan ##

Agar bisa mendebug aplikasi AJAX, browser kita harus bisa menampilkan lalu lintas request dan response. Bila menggunakan Firefox, instal plugin [Firebug](https://addons.mozilla.org/en-US/firefox/addon/firebug/). Bila menggunakan Chrome, kemampuan ini sudah ada di menu Tools > Developer Tools sehingga kita tidak perlu menginstal apa-apa. 

Setelah terinstal, pantau tab Network untuk melihat detail request dan response. 

Berikut contohnya di Firebug

{% img /images/uploads/2013/05/debug-ajax/03-network-tab-firebug.png Network Tab Firebug %}

Dan ini tampilannya di Chrome Developer Tools

{% img /images/uploads/2013/05/debug-ajax/04-network-tab-chrome.png Network Tab Chrome Developer Tools %}

Seiring waktu, tab ini bisa penuh, kita bisa bersihkan menggunakan tombol Clear.

{% img /images/uploads/2013/05/debug-ajax/05-clear-log.png Tombol Clear %}

Beberapa aplikasi AJAX yang mengikuti kaidah REST mengirim data ke server dalam format JSON dengan HTTP method yang sesuai fungsi seperti GET, POST, PUT, DELETE. Seringkali kita juga perlu memanipulasi HTTP request header dan melihat isi HTTP response header. Untuk itu, kita perlu menambah plugin lagi di browser. 

Silahkan install [Poster](https://addons.mozilla.org/En-us/firefox/addon/poster/) untuk Firefox, atau [Rest Console](https://chrome.google.com/webstore/detail/rest-console/cokgbflfommojglbmbpenpphppikmonn?hl=en) untuk Chrome. 

## Mendebug Form Submit ##

Di aplikasi Belajar RESTful, ketika kita mengisi form dan menekan tombol Simpan, prosesnya agak berbeda dengan aplikasi web tradisional.
Pada aplikasi ini, semua form data dikumpulkan dan dikonversi menjadi format JSON, dan kemudian dikirim dalam HTTP request body. Ini dilakukan menggunakan Javascript.

Di aplikasi web tradisional, data form ini disubmit dalam format HTTP request parameter dan tidak melibatkan Javascript sama sekali.

Karena ada proses konversi menjadi JSON ini, selama proses development kita ingin melihat dan memeriksa apakah nama variabel dan datanya sudah benar. Untuk itu kita bisa menggunakan Chrome Developer Tools, seperti bisa dilihat di screenshot berikut.


{% img /images/uploads/2013/05/debug-ajax/06-debug-siklus-cdt.png Debug Request di Chrome Developer Tools %}


Screen debug di Firebug bentuknya seperti ini

{% img /images/uploads/2013/05/debug-ajax/09-debug-siklus-firebug.png Debug Request di Firebug %}

Hal-hal yang perlu kita amati adalah : 

* Request

    * Request Method : GET, POST, PUT, atau DELETE
    * Request Header, terutama Accept dan Content-Type
    * Request Parameter
    * Data yang dikirim (bila ada)
    
* Response

    * Response Status : Sukses (200, 201, 204) atau Gagal (4xx atau 5xx)
    * Response Header, terutama Content-Type dan Location
    * Response Data (bila ada)

Berikut adalah data-data tersebut di Chrome Developer Tools


{% img /images/uploads/2013/05/debug-ajax/07-detail-request-cdt.png Detail Request Chrome Developer Tools %}

{% img /images/uploads/2013/05/debug-ajax/08-detail-response-cdt.png Detail Response Chrome Developer Tools %}


Di Firebug, header dan data dipisahkan

{% img /images/uploads/2013/05/debug-ajax/10-detail-request-firebug.png Tab Header %}

Request data ditampilkan di tab terpisah

{% img /images/uploads/2013/05/debug-ajax/11-detail-post-data-firebug.png Tab Request Data %}

Pada waktu membaca data response, kita bisa melihat data aslinya

{% img /images/uploads/2013/05/debug-ajax/12-detail-response-firebug.png Detail Response Firebug %}

dan bisa juga melihat yang sudah diformat dengan rapi

{% img /images/uploads/2013/05/debug-ajax/13-detail-json-firebug.png Detail JSON Firebug %}

## Mendebug REST Server ##

Pada waktu kita mengisi form dan menekan tombol Submit, client mengirim HTTP request dan data berbentuk JSON. Kemudian server memproses data yang diterima dan mengirim kembalian berupa JSON juga. Agar development lebih mudah, kita ingin melakukan request dan melihat response secara langsung, tanpa direpotkan dengan tampilan HTML dan coding Javascript. Kita bisa menggunakan plugin browser untuk melakukan hal ini.

Beberapa hal yang harus kita ketahui untuk mengetes proses di sisi server adalah:

* URL
* HTTP Method
* Tipe data yang dikirim
* Data yang akan dikirim

### Menggunakan Chrome ###

Berikut adalah tampilan plugin Chrome, yaitu REST Console yang sudah terisi data di atas.

{% img /images/uploads/2013/05/debug-ajax/14-request-form-rest-console.png Request Form REST Console %}

Selanjutnya kita tinggal menekan tombol yang sesuai dengan HTTP Method yang dibutuhkan. Bila sukses, kita akan mendapatkan tampilan seperti ini

{% img /images/uploads/2013/05/debug-ajax/15-response-sukses-rest-console.png Response Sukses REST Console %}

Bila gagal, kita akan melihat tampilan seperti ini

{% img /images/uploads/2013/05/debug-ajax/16-response-error-rest-console.png Response Gagal REST Console %}

### Menggunakan Firefox ###

Hal yang sama bisa dilakukan dengan Poster di Firefox. Berikut adalah form yang digunakan untuk entri data.

{% img /images/uploads/2013/05/debug-ajax/17-request-form-poster.png Request Form Poster %}

Bila sukses, kita mendapatkan hasil seperti ini

{% img /images/uploads/2013/05/debug-ajax/18-response-sukses-poster.png Response Sukses Poster %}

Bila gagal berikut tampilannya 

{% img /images/uploads/2013/05/debug-ajax/19-response-error-poster.png Response Gagal Poster %}

Dengan menggunakan REST Console dan Poster, kita dapat mengurus coding di sisi server tanpa perlu dipusingkan dengan coding di sisi client (HTML, CSS, dan JS).
