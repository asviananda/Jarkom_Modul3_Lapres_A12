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
<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100514843-94779b80-31aa-11eb-85da-dcd0bd052078.png"></p>

**TUBAN (DHCP server)**
<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100514843-94779b80-31aa-11eb-85da-dcd0bd052078.png"></p>

**MOJOKERTO (Proxy server)**
<p align ="center"><img width="500" src="https://user-images.githubusercontent.com/62512432/100514843-94779b80-31aa-11eb-85da-dcd0bd052078.png"></p>

lalu kemudian di ```service networking restart``` pada semua UML diatas, dan lakukan ```iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16``` pada SURABAYA. Tidak perlu melakukan export proxy.

