---
comments: true
date: 2009-01-16 15:24:04
layout: post
slug: enkripsi-jdbc-properties
title: Enkripsi JDBC Properties
wordpress_id: 402
categories:
- java
---

Enkripsi Password Database dengan Jasypt

Bila kita membuat aplikasi Java yang menggunakan database, pasti kita akan membuat satu file untuk konfigurasi koneksi database, biasanya diberi nama jdbc.properties. Isinya kira-kira sebagai berikut: 


    
    
    jdbc.driver = com.mysql.jdbc.Driver
    jdbc.url = jdbc:mysql://localhost/belajar
    jdbc.username = belajar
    jdbc.password = java
    



Cepat atau lambat, kita akan menemui kebutuhan untuk menyembunyikan nilai yang diisikan ke dalam file tersebut untuk alasan keamanan. Tentunya kita tidak ingin orang yang bisa membaca file tersebut login ke database dan melihat berbagai data rahasia dalam database. 

Kita ingin mengenkripsi minimal variabel jdbc.password, supaya tidak bisa dibaca sembarang orang. Bila kita menggunakan Spring Framework untuk membaca file tersebut, kita bisa menggunakan Jasypt yang mampu menangani masalah enkripsi file tersebut. 



Biasanya, kita membaca file jdbc.properties di dalam konfigurasi Spring, seperti ini. 


    
    
    <context:property-placeholder location="classpath:jdbc.properties"/>
    



atau dalam versi yang lebih konvensional, seperti ini


    
    
    <bean id="propertyConfigurer" 
          class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    	<property name="location" value="classpath:jdbc.properties"></property>
    </bean>
    



dan menggunakannya dalam konfigurasi dataSource seperti ini. 


    
    
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    	<property name="driverClassName" value="${jdbc.driver}"></property>
    	<property name="url" value="${jdbc.url}"></property>
    	<property name="username" value="${jdbc.username}"></property>
    	<property name="password" value="${jdbc.password}"></property>
    </bean>
    



Dengan menggunakan Jasypt, kita dapat mengenkripsi jdbc.properties sehingga menjadi seperti ini. 


    
    
    jdbc.driver = com.mysql.jdbc.Driver
    jdbc.url = jdbc:mysql://localhost/belajar
    jdbc.username = belajar
    jdbc.password = ENC(y5+6JdRA4Xv/qbYjmYdo7A==)
    



Agar jdbc.password bisa dibaca dengan baik, kita ganti PropertyPlaceholderConfigurer dengan miliknya Jasypt, sehingga konfigurasi Spring kita menjadi seperti ini. 


    
    
    <bean class="org.jasypt.spring.properties.EncryptablePropertyPlaceholderConfigurer">
    	<constructor-arg>
    	    <bean class="org.jasypt.util.text.BasicTextEncryptor">
    		<property name="password" value="test1234"/>
    	    </bean>
    	</constructor-arg>
    	<property name="locations">
    	    <list>
    		<value>classpath:jdbc.properties</value>
    	    </list>
    	</property>
    </bean>
    



Terakhir, bagaimana cara kita mengetahui bahwa hasil enkripsi dari 'java' adalah 'y5+6JdRA4Xv/qbYjmYdo7A==' ?? 
Gampang, buat saja kode Java sederhana seperti ini. 


    
    
    public class Main {
    
        public static void main(String[] args) {
            String encryptionPassword = "test1234";
            BasicTextEncryptor encryptor = new BasicTextEncryptor();
            encryptor.setPassword(encryptionPassword);
    
            String jdbcPassword = "java";
    
            System.out.println("String "+jdbcPassword+" diencrypt menjadi : "+encryptor.encrypt(jdbcPassword));
    
        }
    
    }
    
