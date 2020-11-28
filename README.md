# Jarkom_Modul3_Lapres_A12

- Achmad Sofyan Pratama (05111840000061)
- Oktarizka Asviananda Nursanty (05111840000156)

## SOAL ##
Anri adalah seorang mahasiswa tingkat akhir yang sedang mengerjakan TA mengenai DHCP dan Proxy. Bu Meguri sebagai dosen pembimbing Anri memberikan tugas pertamanya, (1) yaitu untuk membuat topologi jaringan demi kelancaran TA-nya dengan kriteria sebagai berikut:

<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100181724-8527f200-2f0d-11eb-9f77-ff1298491a64.png"></p>

Anri sudah pernah mempelajari teknik Jaringan Komputer sehingga Anri dapat membuat topologi tersebut dengan mudah. Bu Meguri memerintahkan Anri untuk menjadikan SURABAYA sebagai router, MALANG sebagai DNS Server, TUBAN sebagai DHCP server, serta MOJOKERTO sebagai Proxy server, dan UML lainnya sebagai client. 
Bu Meguri berpesan pada Anri untuk menyusun topologi secara hati-hati dan memperhatikan gambar topologi yang diberikan Bu Meguri. 
Karena TUBAN jauh dari client, maka perlu adanya perantara agar bisa saling terhubung. (2) SURABAYA ditunjuk sebagai perantara (DHCP Relay) antara DHCP Server dan client.
Kriteria lain yang diminta Bu Meguri pada topologi jaringan tersebut adalah:
Seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis.
(3) Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.
(4) Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.
(5) Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP
(6) Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan (6) client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.

Bu Meguri adalah dosbing yang suka overthinking. Ia tidak ingin jaringan lokalnya terhubung ke internet secara langsung. Sehingga ia memberi tugas tambahan pada Anri untuk membuatkan Proxy sebagai penghubung jaringan lokal ke internet. Ada beberapa ketentuan yang harus dipenuhi dalam pembuatan Proxy ini.
Pertama, akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. (7) User autentikasi milik Anri memiliki format:
User : userta_yyy
Password : inipassw0rdta_yyy
Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01
Anri sudah menjadwal pengerjaan TA-nya (8) setiap hari Selasa-Rabu pukul 13.00-18.00. Bu Meguri membatasi penggunaan internet Anri hanya pada jadwal yang telah ditentukan itu saja. Maka diluar jam tersebut, Anri tidak dapat mengakses jaringan internet dengan proxy tersebut. Jadwal bimbingan dengan Bu Meguri adalah (9) setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00). Agar Anri bisa fokus mengerjakan TA, (10) setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA🙂.
Untuk menandakan bahwa Proxy Server ini adalah Proxy yang dibuat oleh Anri, (11) Bu Meguri meminta Anri untuk mengubah error page default squid menjadi seperti berikut:

<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100181800-b0aadc80-2f0d-11eb-939e-d3cd6dd42ccd.png"></p>

Note : File error page bisa diunduh dengan cara wget 10.151.36.202/ERR_ACCESS_DENIED
   Tidak perlu di extract, cukup cp -r

(12) Karena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080. 
Keterangan : yyy adalah nama kelompok masing-masing. Contoh: janganlupa-ta.c01.pw

## JAWAB ##

**1. Buat topologi**

<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100514843-94779b80-31aa-11eb-85da-dcd0bd052078.png"></p>

Kemudian pada router, DNS server, DHCP server, dan Proxy server ditambahkan isi pada ```etc/network/interfaces``` seperti dibawah

**SURABAYA (Router/DHCP relay)**
<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100514903-ffc16d80-31aa-11eb-8789-c3999ae13f21.png"></p>

**MALANG (DNS server)**
<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100521239-dddce080-31d4-11eb-85b4-0d6f942bf516.png"></p>

**TUBAN (DHCP server)**
<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100523093-39f93200-31e0-11eb-93ae-d0c4b935be6b.png"></p>

**MOJOKERTO (Proxy server)**
<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100523106-57c69700-31e0-11eb-93a1-67de77bb23c0.png"></p>

lalu kemudian di ```service networking restart``` pada semua UML diatas, dan lakukan ```iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16``` pada SURABAYA. Tidak perlu melakukan export proxy. Lalu lakukan ```apt-get update``` pada UML diatas.

**2. SURABAYA jadi DHCP relay**

Pertama, install DHCP relay pada SURABAYA dengan cara ```apt-get install isc-dhcp-relay``` kemudian isikan IP yang dituju dengan IP TUBAN (DHCP server), dan pada interfaces dituliskan ```eth1 eth2``` (client)

<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100523184-feab3300-31e0-11eb-9b2b-30dab7f94b7f.png"></p>

setelahnya, install DHCP server pada TUBAN dengan cara ```apt-get install isc-dhcp-server``` kemudian pada interfaces dituliskan ```eth0```

<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100523278-e4be2000-31e1-11eb-9725-b66d983a495c.png"></p>

untuk testing apakah DHCP berhasil, dilakukan testing pada client dengan ketentuan yang diberikan oleh soal.

**3. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200**

Pada TUBAN, buka ```/etc/dhcp/dhcpd.conf``` kemudian tambahkan subnet yang berisi range IP yang diminta soal

<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100523990-12f22e80-31e7-11eb-887e-74dd499175fe.png"></p>

kemudian di restart dengan ```service isc-dhcp-server restart```

**4. Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70**

Sama seperti nomor 3, tambahkan subnet yang berisi range IP yang diminta soal

<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100524083-a1ff4680-31e7-11eb-8a5a-fcccf7c18d53.png"></p>

kemudian di restart dengan ```service isc-dhcp-server restart```

**5. Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP**

<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100524095-c22f0580-31e7-11eb-9737-154df429d67a.png"></p>

**6. Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit**

Kemudian tambahkan lama waktu peminjaman IP sesuai dengan yang diminta soal

**SUBNET 1**
<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100524118-fefafc80-31e7-11eb-919e-167c9e96ac70.png"></p>

**SUBNET 3**
<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100524124-10440900-31e8-11eb-8c40-c5898336cf98.png"></p>

**ISI KESELURUHAN CONFIG /etc/dhcp/dhcpd.conf**

**7. Membuat authentication user**

**8. Membatasi waktu akses internet dengan waktu akses hari Selasa-Rabu pukul 13:00 - 18:00**

**9. Ditambah waktu akses internet yaitu hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00)**

**10. Redirect dari google.com ke monta.if.its.ac.id**

**11. Mengubah halaman access error**

**12. Membuat domain yang mengarah ke IP DHCP server**
