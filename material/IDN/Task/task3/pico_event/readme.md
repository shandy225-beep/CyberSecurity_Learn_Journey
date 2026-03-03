
### PICO CTF — Event Viewing

Halo teman teman semuanya, aku disini bakal mengerjakan tugas dari IDN Network yaitu CTF dari PICO ctf yang berjudul event viewing “https://play.picoctf.org/practice/challenge/456?category=4&difficulty=2&page=1&search=Even”. Di tugas ini sendiri akan membahas tentang bagaimana caranya membaca log dari event viewer milik windows, menganalisis masalah yang terjadi di dalam log dan mendokumetasikannya.

> Skenario :  
> One of the employees at your company has their computer infected by malware! Turns out every time they try to switch on the computer, it shuts down right after they log in. The story given by the employee is as follows:

> 1. They installed software using an installer they downloaded online  
> 2. They ran the installed software but it seemed to do nothing  
> 3. Now every time they bootup and login to their computer, a black command prompt screen quickly opens and closes and their computer shuts down instantly.

> See if you can find evidence for the each of these events and retrieve the flag (split into 3 pieces) from the correct logs!

Di dalam ctf ini kita diberikan log event viewer dari komputer yang terinfeksi. Salah satu karyawan dari perusahaan mengalami infeksi malware di komputernya yang membuat komputer tersebut mati setelah mereka login. Awal mula kasus ini disebabkan oleh penginstallan sebuah software dengan menggunakan installer yang didownload secara online. Walaupun awalnya tidak terjadi apa apa, beberapa saat kemudian muncul command prompt screen yang secara cepat terbuka dan tertutup kemudian menyebabkan komputer mati seketika. Kita diminta mencari tahu bukti yang terjadi pada komputer dan mencari flag yang ada.

![](https://cdn-images-1.medium.com/max/1000/1*OSDWsD2l2x4DsGbXqUjy0w.png)

ini merupakan tampilan awal dari log event viewer windows yang diberikan. Baiklah pada awal kasusnya disini pegawai tersebut melakukan instalasi sebuah installer pada komputer kantor. Dari sini kita bisa memulai investigasi tentang software apa yang diinstall tersebut.

untuk mengetahui hal tersebut kita bisa melakukan filter terhadap log melalui event id untuk mengetahui software apa yang diinstall. saya menggunakan event id `11707` untuk mengetahui software apa yg sukses terinstall.

![](https://cdn-images-1.medium.com/max/1000/1*z0Jdv15lA4z41wYkn_NkQA.png)

disini saya menemukan ada nama software yang aneh, yaitu **Totally_Legit_Software** yang kemungkinan ini adalah malware yang kita cari. Maka kita akan mencari di waktu dimana penginstalan ini terjadi sekitar pukul 10.55

![](https://cdn-images-1.medium.com/max/1000/1*Ov_3rc7kR0cQTFOnAw5IwQ.png)

Disini saya menemukan data tentang software yang diinstall dan saya menyadari ada keanehan pada deskripsi manufacturenya yang berbentuk base64 encoding. Saya akan coba decode base64 ini menggunakan website [https://www.base64decode.org/](https://www.base64decode.org/).

dan saya menemukan flag pertama yaitu **picoCTF{Ev3nt_vi3wv3r_** tetapi ini masih belum lengkap sehingga saya perlu mencari flag lainnya.

Selanjutnya, kita mengetahui bahwa karyawan tersebut menginstall sebuah installer. Installer adalah bagian dari perangkat lunak yang digunakan untuk menginstal program atau aplikasi ke dalam sistem komputer atau perangkat lainnya. Nah, untuk sebuah program berjalan di dalam komputer pasti perlu untuk mendaftarkan program ke registry agar program bisa berjalan. Untuk itu, kita menggunakan filter event id `4657` untuk mengetahui apa saja perubahan dalam registry.

![](https://cdn-images-1.medium.com/max/1000/1*5BrHShoQ7szE7BbkwfFU9A.png)

disini saya menemukan log yang mencurigakan yang mendaftarkan registry program. Program tersebut berisi perintah **“Immediate Shutdown”** yang menjelaskan kenapa komputer tiba tiba mati sendiri saat dihidupkan/ dijalankan. Selain itu kita juga melihat ada base64 code disamping object value name yang mungkin adalah flag, maka kita decode lagi dan mendapatkan flag **“1s_a_pr3tty_us3ful_”**

Selanjutnya kita perlu mencari flag lainnya, kita akan mencari program mana yang menyebabkan komputer seketika shutdown ketika dihidupkan. Kali ini kita akan menggunakan filter event id yaitu `1074` yang berfungsi Mencari nama file/proses yang mematikan PC.

![](https://cdn-images-1.medium.com/max/1000/1*Z5mbeXiy9ILXXZzMv_-7OQ.png)

Disini kita menemukan bahwa program bernama **shutdown.exe** yang selama ini mematikan komputer karyawan tersebut. Kita juga mendapatkan flag yang masih base64 encode. maka kita perlu decode dan menjadi **“t00l_81ba3fe9}”**

Flag yang digabungkan adalah “**picoCTF{Ev3nt_vi3wv3r_1s_a_pr3tty_us3ful_t00l_81ba3fe9}”**

>**Maka berakhirlah pico ctf case “Event Viewing”, semoga bermanfaat dan semangat belajar semuanyaa**
