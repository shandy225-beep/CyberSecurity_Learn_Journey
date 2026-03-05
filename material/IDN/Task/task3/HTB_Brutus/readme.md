
### Hack The Box — Brutus

Halo man teman semua, balik lagi dengan aku di write up CTF. Kali ini aku akan menulis write up dari hack the box dengan tema Brutus di segmen sherlock. Disini kita akan belajar tentang log, cara menganalisis log kemudiann menginvestigasi metode penyerang dalam log tersebut.

##This article use for education

> Skenario

> In this very easy Sherlock, you will familiarize yourself with Unix auth.log and wtmp logs. We’ll explore a scenario where a Confluence server was brute-forced via its SSH service. After gaining access to the server, the attacker performed additional activities, which we can track using auth.log. Although auth.log is primarily used for brute-force analysis, we will delve into the full potential of this artifact in our investigation, including aspects of privilege escalation, persistence, and even some visibility into command execution.

Di dalam kasus ini kita diminta untuk memahami bagaimana penyerang setelah berhasil masuk melalui metode brute force via ssh. Apa saja hal yang dilakukan oleh penyerang dengan investigasi melalui log. Tetapi hal pertama yang harus kita lakukan terlebih dahulu adalah mendownload artefak yang sudah disediakan dan melakukan ekstrak untuk mendapatkan isi dari file zip tersebut.

![](https://cdn-images-1.medium.com/max/1000/1*Mecqz285T47UppVodvAqQQ.png)
```
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/HTB_Brutus$ 7z x Brutus.zip  
  
7-Zip 23.01 (x64) : Copyright (c) 1999-2023 Igor Pavlov : 2023-06-20  
 64-bit locale=en_US.UTF-8 Threads:16 OPEN_MAX:1024  
  
Scanning the drive for archives:  
1 file, 5756 bytes (6 KiB)  
 
Extracting archive: Brutus.zip  
--  
Path = Brutus.zip  
Type = zip  
Physical Size = 5756  
  
      
Enter password (will not be echoed):  
Everything is Ok  
  
Files: 3  
Size:       58201  
Compressed: 5756
```
Baiklah setelah melakukan ekstraksi file zip kita mendapatkan 3 file utama yakni auth.log, utmp.py, wtmp tapi kita berfokus dulu terhadap file **auth.log**

**Q1 : Analyze the auth.log. What is the IP address used by the attacker to carry out a brute force attack?**

Disini kita diminta untuk menganalisa IP Adress mana yang melakukan metode brute force terhadap SSH. Disini saya menggunakan perintah `grep` untuk mencari ip mana yang melakukan brute force. Brute force merupakan metode paling sederhana yang dimana penyerang melakukan percobaan berkali kali secara konsisten untuk menembus keamanan server. Serangan ini dapat dengan mudah dikenali dengan jumlah kuantitas percobaan yang ada terutama jika hanya dari 1 IP Adress saja.
```
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/HTB_Brutus$ grep -i "invalid" auth.log  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2325]: Invalid user admin from 65.2.161.68 port 46380  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2325]: Disconnected from invalid user admin 65.2.161.68 port 46380 [preauth]  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2327]: Invalid user admin from 65.2.161.68 port 46392  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2332]: Invalid user admin from 65.2.161.68 port 46444  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2331]: Invalid user admin from 65.2.161.68 port 46436  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2330]: Invalid user admin from 65.2.161.68 port 46422  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2337]: Invalid user admin from 65.2.161.68 port 46498  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2328]: Invalid user admin from 65.2.161.68 port 46390  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2335]: Invalid user admin from 65.2.161.68 port 46460  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2334]: Invalid user admin from 65.2.161.68 port 46454  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2329]: Invalid user admin from 65.2.161.68 port 46414  
Mar  6 06:31:31 ip-172-31-35-28 sshd[2333]: Invalid user admin from 65.2.161.68 port 46452  
Mar  6 06:31:33 ip-172-31-35-28 sshd[2327]: Failed password for invalid user admin from 65.2.161.68 port 46392 ssh2  
Mar  6 06:31:33 ip-172-31-35-28 sshd[2331]: Failed password for invalid user admin from 65.2.161.68 port 46436 ssh2  
Mar  6 06:31:33 ip-172-31-35-28 sshd[2332]: Failed password for invalid user admin from 65.2.161.68 port 46444 ssh2  
Mar  6 06:31:33 ip-172-31-35-28 sshd[2335]: Failed password for invalid user admin from 65.2.161.68 port 46460 ssh2  
Mar  6 06:31:33 ip-172-31-35-28 sshd[2337]: Failed password for invalid user admin from 65.2.161.68 port 46498 ssh2  
Mar  6 06:31:33 ip-172-31-35-28 sshd[2334]: Failed password for invalid user admin from 65.2.161.68 port 46454 ssh2  
Mar  6 06:31:33 ip-172-31-35-28 sshd[2330]: Failed password for invalid user admin from 65.2.161.68 port 46422 ssh2  
Mar  6 06:31:33 ip-172-31-35-28 sshd[2328]: Failed password for invalid user admin from 65.2.161.68 port 46390 ssh2  
Mar  6 06:31:33 ip-172-31-35-28 sshd[2329]: Failed password for invalid user admin from 65.2.161.68 port 46414 ssh2  
Mar  6 06:31:33 ip-172-31-35-28 sshd[2333]: Failed password for invalid user admin from 65.2.161.68 port 46452 ssh2  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2327]: Disconnected from invalid user admin 65.2.161.68 port 46392 [preauth]  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2332]: Disconnected from invalid user admin 65.2.161.68 port 46444 [preauth]  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2331]: Disconnected from invalid user admin 65.2.161.68 port 46436 [preauth]  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2337]: Disconnected from invalid user admin 65.2.161.68 port 46498 [preauth]  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2328]: Disconnected from invalid user admin 65.2.161.68 port 46390 [preauth]  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2330]: Disconnected from invalid user admin 65.2.161.68 port 46422 [preauth]  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2334]: Disconnected from invalid user admin 65.2.161.68 port 46454 [preauth]  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2335]: Disconnected from invalid user admin 65.2.161.68 port 46460 [preauth]  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2329]: Disconnected from invalid user admin 65.2.161.68 port 46414 [preauth]  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2333]: Disconnected from invalid user admin 65.2.161.68 port 46452 [preauth]  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2359]: Invalid user server_adm from 65.2.161.68 port 46596  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2361]: Invalid user server_adm from 65.2.161.68 port 46614  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2368]: Invalid user server_adm from 65.2.161.68 port 46676  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2369]: Invalid user server_adm from 65.2.161.68 port 46682  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2366]: Invalid user server_adm from 65.2.161.68 port 46648  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2365]: Invalid user server_adm from 65.2.161.68 port 46644  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2364]: Invalid user server_adm from 65.2.161.68 port 46632  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2367]: Invalid user server_adm from 65.2.161.68 port 46664  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2363]: Invalid user server_adm from 65.2.161.68 port 46620  
Mar  6 06:31:35 ip-172-31-35-28 sshd[2377]: Invalid user server_adm from 65.2.161.68 port 46684  
Mar  6 06:31:36 ip-172-31-35-28 sshd[2379]: Invalid user server_adm from 65.2.161.68 port 46698  
Mar  6 06:31:36 ip-172-31-35-28 sshd[2380]: Invalid user server_adm from 65.2.161.68 port 46710  
Mar  6 06:31:36 ip-172-31-35-28 sshd[2383]: Invalid user svc_account from 65.2.161.68 port 46722  
Mar  6 06:31:36 ip-172-31-35-28 sshd[2384]: Invalid user svc_account from 65.2.161.68 port 46732  
Mar  6 06:31:36 ip-172-31-35-28 sshd[2387]: Invalid user svc_account from 65.2.161.68 port 46742  
Mar  6 06:31:36 ip-172-31-35-28 sshd[2389]: Invalid user svc_account from 65.2.161.68 port 46744
```
`grep -i "Invalid" auth.log`

**Penjelasan Perintah :**

-   `grep` : Perintah untuk menemukan suatu kata atau kalimat dalam file
-   `-i` : Tipe grep yang mengambil kalimat atau kata tanpa peduli huruf kapital atau tidak.
-   `"invalid"` : Kata yang dicari
-   `auth.log` : file yang digunakan

Disini saya mengambil beberapa contoh saja dari perintah grep yang saya gunakan. saya menggunakan kata invalid untuk membuktikan bahwa terdapat percobaan berkali kali yang gagal login dari suatu IP. Dan IP Addres tersebut adalah **“65.2.161.68”** dengan berbagai macam percobaan Port.

**Answer : 65.2.161.68**

**Q2 : The bruteforce attempts were successful and attacker gained access to an account on the server. What is the username of the account?**

Selanjutnya kita diminta untuk menemukan percobaan brute force yang sukses dan menemukan apa username dari akun yang sukses tersebut. Maka kita akan kembali menggunakan perintah grep untuk menemukan kata kata yang menyatakan bahwa percobaan tersebut success. Awalnya saya menggunakan kata “Success” untuk mendapatkan log tersebut tetapi tidak ada. saya baru menyadari bahwa “Accepted” adalah kata yang lebih tepat untuk mengetahui bahwa penyerang telah berhasil masuk.
```
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/HTB_Brutus$ grep -i "accepted" auth.log  
Mar  6 06:19:54 ip-172-31-35-28 sshd[1465]: Accepted password for root from 203.101.190.9 port 42825 ssh2  
Mar  6 06:31:40 ip-172-31-35-28 sshd[2411]: Accepted password for root from 65.2.161.68 port 34782 ssh2  
Mar  6 06:32:44 ip-172-31-35-28 sshd[2491]: Accepted password for root from 65.2.161.68 port 53184 ssh2  
Mar  6 06:37:34 ip-172-31-35-28 sshd[2667]: Accepted password for cyberjunkie from 65.2.161.68 port 43260 ssh2
```
`grep -i "accepted" auth.log`

Disini untuk pertama kalinya penyerang berhasil menembus ssh dengan username bernama “root”. Ketika saya memasukkan nama username root ternyata benar.

**Answer : root**

**Q3 : Identify the UTC timestamp when the attacker logged in manually to the server and established a terminal session to carry out their objectives. The login time will be different than the authentication time, and can be found in the wtmp artifact.**

Baiklah, disini kita diminta untuk mengidentifikasi timestamp UTC ketika penyerang berhasil masuk manual ke server dan mendirikan sesi terminal untuk mencari tahu tujuan mereka. Dengan menggunakan file wtmp yang ada kita harus mencari tahu hal tersebut.

File WTMP ini adalah **binary log** yang mencatat sejarah login, logout, dan sistem reboot. Untuk membacanya, membutuhkan tools khusus yang sudah ada di Linux yaitu perintah last. `last` secara otomatis menerjemahkan data biner tersebut menjadi format yang jauh lebih bersih.
```
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/HTB_Brutus$ TZ=utc last -f wtmp -F  
cyberjun pts/1        65.2.161.68      Wed Mar  6 06:37:35 2024 gone - no logout  
root     pts/1        65.2.161.68      Wed Mar  6 06:32:45 2024 - Wed Mar  6 06:37:24 2024  (00:04)  
root     pts/0        203.101.190.9    Wed Mar  6 06:19:55 2024 gone - no logout  
reboot   system boot  6.2.0-1018-aws   Wed Mar  6 06:17:15 2024 still running  
root     pts/1        203.101.190.9    Sun Feb 11 10:54:27 2024 - Sun Feb 11 11:08:04 2024  (00:13)  
root     pts/1        203.101.190.9    Sun Feb 11 10:41:11 2024 - Sun Feb 11 10:41:46 2024  (00:00)  
root     pts/0        203.101.190.9    Sun Feb 11 10:33:49 2024 - Sun Feb 11 11:08:04 2024  (00:34)  
root     pts/0        203.101.190.9    Thu Jan 25 11:15:40 2024 - Thu Jan 25 12:34:34 2024  (01:18)  
ubuntu   pts/0        203.101.190.9    Thu Jan 25 11:13:58 2024 - Thu Jan 25 11:15:12 2024  (00:01)  
reboot   system boot  6.2.0-1017-aws   Thu Jan 25 11:12:17 2024 - Sun Feb 11 11:09:18 2024 (16+23:57)  
  ```
`wtmp begins Thu Jan 25 11:12:17 2024`

Penjelasan Perintah :

-   `TZ=utc` : Ini adalah instruksi untuk mengatur **Time Zone** (zona waktu) secara sementara hanya untuk perintah yang sedang dijalankan. Karena server penyerang menggunakan UTC dan jika tanpa disesuaikan timestamp akan menyesuaikan server waktu lokal.
-   `last` : Ini adalah perintah utama yang berfungsi untuk menampilkan daftar user yang terakhir kali login ke sistem.
-   `-f`: perintah untuk ekstrak file di depannya
-   `-F` : Singkatan dari **Full**. Menampilkan **format waktu lengkap**, termasuk detik dan tahun (contoh: `Wed Mar 6 06:32:45 2024`).

Dari log file wtmp diatas, akun dengan username bernama root dengan ip address “65.2.161.68” melakukan login pada tanggal 06/03/2024 dengan pukul 06:32:45

Ketika saya jawab ternyata jawabannya benar.

**Answer : 2024–03–06 06:32:45**

**Q4 : SSH login sessions are tracked and assigned a session number upon login. What is the session number assigned to the attacker’s session for the user account from Question 2?**

Selanjutnya kita diminta untuk mencari tahu nomor sesi yang digunakan attacker untuk login sebagai attacker untuk mengakses ssh sebagai akun dengan username bernama root. Sebenarnya attacker berhasil 2 kali menemukan 2 kali password yang benar.

![](https://cdn-images-1.medium.com/max/1000/1*BngCvX48BWfObqpD9sCCCA.png)

ini merupakan percobaan pertama dengan nomor sesi 34, tetapi percobaan ini gagal karena akun yang dicari oleh penyerang itu tidak ada sehingga ini hanya menjadi preauth. `preauth` berarti koneksi terputus sebelum proses login (autentikasi) selesai dilakukan secara utuh. Selanjutnya saya mencari sesi login untuk username root dan saya menemukan log ini

![](https://cdn-images-1.medium.com/max/1000/1*iq92p0dTnb_wBjhn-9vXtA.png)

Pada log ini saya menemukan bahwa penyerang berhasil masuk dengan nomor sesi 37. Dan ketika saya jawab ternyata benar yaitu 37.

**Answer : 37**

**Q5 : The attacker added a new user as part of their persistence strategy on the server and gave this new user account higher privileges. What is the name of this account?**

Masih berhubungan dengan gambar di atas, penyerang menambahkan user baru untuk strategi persistence penyerangan server dan memberikan user baru ini kendali yang lebih tinggi.

![](https://cdn-images-1.medium.com/max/1000/1*iq92p0dTnb_wBjhn-9vXtA.png)

Akun ini bernama cyberjunkie yang dimasukkan ke dalam folder /etc/group.

**Answer : cyberjunkie**

**Q6 : What is the MITRE ATT&CK sub-technique ID used for persistence by creating a new account?**

Disni kita diminta untuk mengidentifikasi ID dari sub teknik MITRE ATT&CK yang digunakan untuk persistence dengan cara membuat akun baru oleh penyerang. Seperti yang kita ketahui bahwa penyerang membuat akun cyberjunkie dalam lingkungan lokal komputer.

![](https://cdn-images-1.medium.com/max/1000/1*SreYqLlHCeyaNxlOgLpEBA.png)

Maka dari itu kita perlu mencari tahu id sub teknik yang digunakan oleh penyerang di MITRE ATT&CKl, sebuah basis pengetahuan (knowledge base) global yang mendokumentasikan taktik dan teknik serangan siber berdasarkan pengamatan nyata di dunia cyber security. Maka saya menemukan ini

![](https://cdn-images-1.medium.com/max/1000/1*6REVkkQdCwEFbrhBFIb2-Q.png)

Teknik yang paling sesuai memiliki kode T1136.001.

**Answer : T1136.001**

**Q7 : What time did the attacker’s first SSH session end according to auth.log?**

![](https://cdn-images-1.medium.com/max/1000/1*CxbEByHXLeIyDiLFlguaPA.png)

dengan menggunakan perintah yang sama untuk mengetahui kapan penyerang masuk pada Q3, kita bisa mendapatkan data waktu yang digunakan oleh penyerang untuk keluar dari akun dengan username root yaitu 2024–03–06 06:37:24

**Answer : 2024–03–06 06:37:24**

**Q8 : The attacker logged into their backdoor account and utilized their higher privileges to download a script. What is the full command executed using sudo?**

Tentu saja dengan privilege yang ada dan keinginan untuk melakukan persistence penyerang akan menyisipkan backdoor dan menggunakan privilege akun yang mereka dapat untuk mendownload script backdoor tersebut. maka kita cari log dari akun cyberjunkie yang memiliki privilige yang tinggi yang mereka buat.

![](https://cdn-images-1.medium.com/max/1000/1*A1tyUIGTzJYzuDNKJiO9Pw.png)

Maka dsini kita bisa melihat dengan menggunaka perintah sudo, penyerang mendownload script dari url yang berisi link pada command

> Baiklah cukup sekian pemaparan dari saya, apabila ada kesalahan mohon dimaafkan. terima kasih semuanya dan happy learning
