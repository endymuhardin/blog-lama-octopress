---
layout: post
title: "Membuat Audit Log"
date: 2012-10-24 11:44
comments: true
categories: 
- java
---

# Membuat Audit Log dalam Aplikasi #

Dalam membuat aplikasi bisnis, kita sering diminta membuat audit log.

> Apa itu audit log?

Audit log adalah catatan mengenai perubahan data dalam aplikasi.
Yang dicatat biasanya : 

- kolom mana yang berubah
- siapa yang mengubah
- diubah dari apa menjadi apa
- kapan dia berubah

<!--more-->

Dengan menggunakan [Hibernate Envers](http://docs.jboss.org/hibernate/orm/4.1/devguide/en-US/html/ch15.html), 
audit log ini bisa dibuat dengan mudah sekali.
Hibernate Envers adalah salah satu modul tambahan [Hibernate](http://hibernate.org/) untuk keperluan ini.
Tentunya aplikasi kita harus menggunakan Hibernate supaya bisa memanfaatkan fitur ini.

Caranya sangat mudah, yaitu:

1. Tambahkan JAR hibernate-envers ke dalam aplikasi
2. Tambahkan anotasi `@Audited` di class `@Entity` kita.

Contoh perubahan ini bisa dilihat [di sini](https://github.com/endymuhardin/belajar-auditlog/commit/05ca0c7c90b10cf64560d4cec933774aa91a8a81). 

Secara default, Hibernate Envers hanya mencatat data yang berubah dan kapan dia berubah. 
Untuk menambahkan catatan tentang username, perlu ada sedikit coding seperti bisa dilihat [di sini](https://github.com/endymuhardin/belajar-auditlog/commit/e910b6bc5ef33ee61f0137c8297b0f2ec4f502fe). Kita harus :

1. Extends `DefaultRevisionEntity` dan menambahkan kolom username
2. Membuat `RevisionListener` untuk mengisi kolom username tersebut

Data username biasanya diambilkan dari user yang sedang login di aplikasi (dan dengan sendirinya dialah yang melakukan perubahan data).

Contoh kode yang lengkap bisa diambil di [repository saya di Github](https://github.com/endymuhardin/belajar-auditlog).
