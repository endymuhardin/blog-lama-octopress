---
comments: true
date: 2007-08-29 17:50:36
layout: post
slug: network-address-translation
title: Network Address Translation
wordpress_id: 261
categories:
- linux
---

NAT adalah singkatan dari Network Address Translation. 

Gunanya supaya kita bisa mempublish komputer di internal (misal, IP dalamnya : 192.168.0.1) 
ke internet (misal, IP luarnya : 202.159.11.11). 
Atau agar komputer di jaringan dalam bisa berbagi pakai akses internet.

Setiap request ke satu komputer, komputer asal mengirim paket data ke komputer tujuan. 
Mirip dengan kalau kita kirim surat. Pesannya ditulis di kertas, masukkan amplop, 
tulis alamat penerima dan alamat pengirim.

Contoh paketnya kira-kira seperti ini. 

    
    <code>Dari  : 202.159.22.22
    Untuk : 202.159.11.11
    Pesan : halo
    </code>



Gateway harus punya mapping IP luar dan IP dalam. 
Begitu gateway terima paket yang ditujukan ke IP luar, 
dia akan membungkus paket tersebut dengan amplop baru, alamat tujuannya diganti dengan IP dalam. 
Proses ini dinamakan DNAT (Destination NAT). 

Bila alamat tujuan tidak diganti, maka routernya akan bingung, karena dia tidak tahu IP luar itu mesinnya yang mana.

Kalau di Linux, ini dilakukan dengan perintah iptables. 
Contohnya adalah sebagai berikut: 

`iptables -t nat -I PREROUTING -d 202.159.11.11 --to-destination 192.168.0.1 -j DNAT`

Setelah diDNAT, paketnya menjadi seperti ini: 


    
    <code>Dari  : 202.159.22.22
    Untuk : 192.168.0.1
    Pesan : halo
    </code>



Setelah itu, amplop lewat proses routing. Router akan bisa menyampaikan paket tersebut, 
karena dia kenal 192.168.0.1 itu mesin yang mana.

Akhirnya paket sampai di tujuan. 

Dengan menggunakan DNAT ini, kita bisa mempublikasikan mesin di jaringan internal agar bisa diakses dari luar.

Bagaimana jika sebaliknya?

Penerima akan mengirim balasan. Dia masukkan ke amplop, kirim ke router. 
Router akan lihat bahwa tujuannya adalah komputer di luar, maka dia kirim ke gateway. 

Paketnya kira-kira seperti ini: 


    
    <code>Dari  : 192.168.0.1
    Untuk : 202.159.22.22
    Pesan : halo juga
    </code>



Gateway melihat bahwa alamat pengirim adalah IP dalam. 
Kalau diteruskan begitu saja, maka kalau ada balasan lagi, tidak akan sampai, 
karena IP dalam tidak dikenal di luar.
Jadi dia harus membungkus lagi dengan amplop lagi, kali ini yang diganti adalah alamat pengirim.
IP dalam diganti dengan IP public.
Proses ini dinamakan SNAT (Source NAT)

Contohnya adalah sebagai berikut: 
`iptables -t nat -I POSTROUTING -s 192.168.0.1 --to-source 202.159.11.11 -j SNAT`

Setelah diSNAT paketnya menjadi seperti ini: 

    
    <code>Dari  : 202.159.11.11
    Untuk : 202.159.22.22
    Pesan : halo juga
    </code>



Dengan menggunakan SNAT, kita bisa mengimplementasikan Internet Connection Sharing, 
yaitu satu koneksi internet dibagi beramai-ramai.

Ada satu kasus khusus untuk SNAT, namanya Masquerade. 
Ini digunakan apabila IP public yang digunakan berubah-ubah. 
Misalnya kalau kita pakai dialup connection. 

Kalau kita nekat pakai SNAT, nanti akan repot, karena harus update rule setiap dial ke internet.

Jadi, kita gunakan masquerade. 
Berikut perintahnya: 
`iptables -t nat -I POSTROUTING -p tcp -s 192.168.0.1 -j MASQUERADE`

Perhatian: Jangan lupa untuk mengaktifkan IP Forwarding di gateway dengan cara : 
`cat 1 > /proc/sys/net/ipv4/ip_forward `

Rangkaian perintah ini akan hilang pada saat reboot. 
Jadi harus ada usaha tambahan agar konfigurasi ini jadi permanen. 
Caranya, tergantung masing-masing distro. 

Sayangnya saat ini iptables hanya ada di Linux, dan nampaknya tidak akan ada versi Windowsnya. Karena iptables sangat tightly-coupled dengan kernel linux.

Untuk Windows, kita dapat gunakan fitur Internet Connection Sharing apabila ada. Beberapa versi Windows (misalnya XP Home), tidak punya fitur ini. Jadi solusinya adalah dengan menggunakan aplikasi tambahan seperti misalnya [WinGate](http://www.wingate.com/product-wingate.php) atau [WinRoute](http://www.kerio.com/kwf_home.html).
