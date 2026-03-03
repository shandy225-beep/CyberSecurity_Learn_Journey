
# PICO CTF -Ph4nt0m 1ntrud3r

[](https://medium.com/@sansssadt225?source=post_page---byline--8c2462ccd927---------------------------------------)
Halo temen temen semua, pada kesempatan kali ini aku akan menyelesaikan tugas dari IDN Network yaitu pengerjaan pico CTF yang berjudul “Ph4nt0m 1ntrud3r” dengan link  [https://play.picoctf.org/practice/challenge/459?category=4&difficulty=1&page=1&search=](https://play.picoctf.org/practice/challenge/459?category=4&difficulty=1&page=1&search=)

Pada case ini bertugas untuk menganalisis paket PCAP dan mengetahui serangan yang dilakukan penyerang.

> **Scenario :**
> 
> A digital ghost has breached my defenses, and my sensitive data has been stolen! 😱💻 Your mission is to uncover how this phantom intruder infiltrated my system and retrieve the hidden flag.To solve this challenge, you’ll need to analyze the provided PCAP file and track down the attack method. The attacker has cleverly concealed his moves in well timely manner. Dive into the network traffic, apply the right filters and show off your forensic prowess and unmask the digital intruder!Find the PCAP file here  [Network Traffic PCAP file](https://challenge-files.picoctf.net/c_verbal_sleep/586d0206891cc683bae1160ad6b0e05d7e10e7b2df122c0441ab06581038dd32/myNetworkTraffic.pcap)  and try to get the flag.

Setelah mendownload file PCAP yang diberikan, kita mendapatkan 22 TCP packet di dalam file PCAP tersebut.



![](https://miro.medium.com/v2/resize:fit:875/1*_DqRbAsixCgsfS6f6F0cjw.png)

Disini apabila kita perhatikan terdapat payload kecil yang berbentuk base64 encoding pada setiap paketnya. Selain itu jika kita menggunakan hintnya waktu di dalam paket PCAP ini teracak acak sehingga perlu disusun ulang agar sesuai dengan timestamp yang ada.

Disini saya menggunakan perintah reorder untuk menyusun kembali timestamp dan juga menghasilkan file PCAP baru dengan timestamp dengan urutan yang benar.
```
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/Pico_phantom$ reordercap myNetworkTraffic.pcap output_sorted.pcap  
22 frames, 8 out of order  
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/Pico_phantom$ ls  
myNetworkTraffic.pcap  output_sorted.pcap
```
Disini saya membuat file baru bernama  `output_sorted.pcap`  sebagai file PCAP dengan timestamp yang benar. Selanjutnya kita perlu mengekstrak TCP payload yang terdapat dalam setiap paket.

dengan menggunakan terminal jalankan perintah ini :
```
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/Pico_phantom$ tshark -r output_sorted.pcap -T fields -e tcp.segment_data  
6f6b4b544272383d  
7938382f7064413d  
5a44516b5033553d  
4d77624d5971513d  
4b695a502b75413d  
6d554b6c3471343d  
2b5256384e56593d  
5542635a5079593d  
554a64644e6a343d  
2b5448325269413d  
3677446f5438383d  
75316e376157673d  
6c7746703577303d  
42627a516731303d  
516a3961744d593d  
63476c6a62304e5552673d3d  
657a46305833633063773d3d  
626e52666447673064413d3d  
587a4d3063336c6664413d3d  
596d68664e484a664d673d3d  
5a54466d5a6a41324d773d3d  
66513d3d
```
Penjelasan perintah :

-   `tshark`  : merupakan versi CLI dari Wireshark.
-   `-r`  : Membaca file PCAP
-   `-T fields`  : Menentukan format tampilan  _output_  di terminal yang berdasarkan perintah -e didepannya (efek field)
-   `-e tcp.segment_data`  : -e adalah extract dan  `tcp.segment_data`  adalah  _filter_  yang merujuk pada payload  atau isi data mentah di dalam segmen TCP.

Baiklah, disini kita mendapatkan beberapa kode hex string dari setiap paket yang kita punya. Tetapi tidak semua hex string tersebut berguna, perlu dilakukan filtering kembali untuk mendapatkan flag yang dibutuhkan. Di dalam dunia CTF, Malware yang ingin bersembunyi biasanya tidak mengirim data berukuran besar yang acak. Mereka cenderung menggunakan Fixed-Length Payloads (beban data tetap). Jika kamu melihat banyak paket dengan panjang tepat 12 atau 4 byte yang muncul secara berkala, itu adalah konfirmasi bahwa itu bukan lalu lintas internet normal, melainkan Beaconing.

Maka perintah filter yang kita lakukan :
```
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/Pico_phantom$ tshark -r output_sorted.pcap -Y "tcp.len==12 || tcp.len==4" -T fields -e tcp.segment_data | xxd -r -p  
cGljb0NURg==ezF0X3c0cw==bnRfdGg0dA==XzM0c3lfdA==YmhfNHJfMg==ZTFmZjA2Mw==fQ==
```
Penjelasan perintah :

-   `tshark`  : merupakan versi CLI dari Wireshark.
-   `-r`  : Membaca file PCAP
-   `-Y "tcp.len==12 || tcp.len==4"`  : Filter ini hanya akan meloloskan paket TCP yang ukuran datanya tepat 12 byte atau 4 byte.
-   `-T fields`  : Menentukan format tampilan  _output_  di terminal yang berdasarkan perintah -e didepannya (efek field)
-   `-e tcp.segment_data`  : -e adalah extract dan  `tcp.segment_data`  adalah  _filter_  yang merujuk pada payload  atau isi data mentah di dalam segmen TCP.
-   `| xxd -r -p`  : Ini adalah pipelining yang dilakukan untuk mengolah output dari tshark yang berbentuk hex string diubah menjadi ASCII (yang bisa dibaca). -r berarti reverse yang Mengubah format Hex kembali menjadi karakter asli (ASCII atau biner) dan -p berarti Plain/Postscript yang Memberitahu  `xxd`  bahwa input yang datang dari  `tshark`  adalah format  _plain hex._

Dari output perintah diatas terdapat beberapa kode base64 yang tergabung menjadi satu sehingga kita perlu melakukan decoding. saya melakukan decoding pada terminal langsung :
```
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/Pico_phantom$ echo "cGljb0NURg==ezF0X3c0cw==bnRfdGg0dA==XzM0c3lfdA==YmhfNHJfMg==ZTFmZjA2Mw==fQ==" | base64 -d  
picoCTF{1t_w4snt_th4t_34sy_tbh_4r_2e1ff063}
```
Akhirnya kita mendapakan flag yang kita cari yaitu :  
**picoCTF{1t_w4snt_th4t_34sy_tbh_4r_2e1ff063}**

> Baiklah cukup sekian dari saya, apabila ada kesalahan maafkan saya. happy learning yall
