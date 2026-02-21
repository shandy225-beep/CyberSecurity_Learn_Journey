
# Introduction to Digital Forensics | Lab-01 Digital Forensics

**Penulis:** Shandyka Aditya Putra  
**Tanggal:** Februari 2026

## Introduction
Halo semuanya, pada pembahasan kali ini kita akan menyelesaikan exercise dari IDN Networker Lab 01. Pada exercise ini sendiri akan membahas tentang command line dasar, manipulasi file, serta pembahasan digital forensic lainnya.

**Exercise/Lab 01:** [Link GitHub](https://github.com/vonderchild/digital-forensics-lab/tree/main/Lab%2001)

---


## **Exercise**

_For this section, provide the complete commands for all the exercises where asked for the command, and provide a descriptive answer where asked for an explanation. There may be multiple answers/commands for these exercises, so feel free to submit the answer you feel most comfortable with._

_Question 1_  
_If we wanted to list all the_ `.txt` _files in the current directory, what command would we want to use?_

**_Answer :_**

```
ls *.txt
```


![](https://miro.medium.com/v2/resize:fit:875/1*-emLJBtinguJndV8FtGVjA.png)


**Penjelasan :**  
1.  `ls`  merupakan perintah untuk menampilkan seluruh file yang berada dalam direktori tertentu.  
2.  `*`  merupakan wildcard untuk menampilkan apa saja (disesuaikan dengan perintah selanjutnya).  
3.  `.txt`  adalah format file yang dicari.  
4.  `ls *.txt`  menampilkan keseluruhan file dengan format txt.

**Tentang** `ls`  :  
_list_  atau biasa digunakan menjadi  `ls`  adalah salah satu  _command line_  linux yang berfungsi untuk menampilkan semua file di dalam suatu direktori. Perintah ls sendiri tidak hanya sekedar menampilkan file yang ada tetapi bisa menjadi lebih spesifik berdasarkan tipe perintah ls yang berbeda. Contohnya  `ls -a`  yang berfungsi menampilkan semua file yang ada di dalam direktori file bahkan file tersembunyi. Selanjutnya ada  `ls -l`  yang menampilkan file lengkap dengan tanggal dibuat, ukuran serta izin yang dimiliki dan masih banyak contoh  _command_  lainnya

_Question 2_  
_What command can we use to read the contents of the file_  `/etc/passwd`?

**Answer :**

```
cat /etc/passwd
```


![](https://miro.medium.com/v2/resize:fit:875/1*S9k6Yj-32Si5f31EVqxhAg.png)


Penjelasan :  
1.  `cat`  adalah command line yang berfungsi menampilkan isi dari suatu file.  
2.  `/etc/passwd`  adalah direktori file yang akan dilihat  
3.  `cat /etc/passwd`  adalah menampilkan isi dari  `/etc/passwd ke terminal`

**Tentang** `cat` : Cat  _Command_ dalam Linux adalah perintah dasar yang digunakan untuk menampilkan isi file teks, menggabungkan beberapa file, atau membuat file baru. Contoh perintah dari cat sendiri adalah  `cat namafile.txt`  yang akan menampilkan isi dari file berjudul “namafile”. Selain itu, kita bisa menggabungkan file dengan cat seperti  `cat 1.txt 2.txt > filegabungan.txt`  maupun membuat file baru dengan perintah  `cat > file.txt`  .

_Question 3_  
_If we wanted to search for the string_ `Error` _in all files in the_ `/var/log` _directory, what would our command be?_

**Answer :**

```
grep -r "Error" /var/log
```

![](https://miro.medium.com/v2/resize:fit:875/1*ZR4fxBdIU_eKlkMhxWHmow.png)

Penjelasan :  
1.  `grep`  adalah  _command line_ yang berfungsi untuk mencari kata spesifik di dalam file.  
2.  `-r`  adalah perintah untuk mencari secara  _recursive_ kata yang dicari ke dalam keseluruhan file di suatu direktori maupun sub directory.  
3.  `/var/log`  adalah direktori yang digunakan.  
4.  `"Error"`  adalah kata yang dicari  
5.  `grep -r "Error" /var/log`  adalah perintah mencari kata error secara  _recursive_  di dalam direktori /var/log.

**Tentang** `grep` : Grep Command dalam Linux adalah perintah dasar yang digunakan untuk mencari string atau pola teks tertentu di dalam satu atau beberapa file. Nama grep berasal dari  _Global Regular Expression Print_. Contoh perintah dari grep sendiri adalah  `grep "kata" namafile.txt`  yang akan menampilkan semua baris yang mengandung kata tersebut. Selain itu, kita bisa mencari tanpa mempedulikan huruf besar/kecil dengan  `grep -i "kata" file.txt`  maupun mencari di seluruh folder secara mendalam menggunakan perintah  `grep -r "kata" /folder/`.

_Question 4_  
_What would be the commands to calculate MD5 and SHA1 hashes of the file_ `/etc/passwd`_?_

**Answer :**
```
md5sum /etc/passwd

sha1sum /etc/passwd
```

![](https://miro.medium.com/v2/resize:fit:875/1*xU8Y2WUbSU8sK6xbia7siQ.png)

Penjelasan :  
1.  `md5sum`  adalah perintah untuk menghitung hash md5 dari suatu file.  
2.  `sha1sum`  adalah perintah untuk menghitung hash sha1sum dari suatu file.

Tentang  `md5sum`  dan  `sha1sum`  :  
**md5sum**  dan  **sha1sum**  dalam Linux adalah perintah dasar yang digunakan untuk menghitung dan memverifikasi  _checksum_  atau sidik jari digital dari sebuah file. Perintah ini berfungsi untuk memastikan bahwa file yang kita miliki tidak rusak atau tidak dimodifikasi oleh pihak lain.

_Question 5_  
_Use the_ `file` _command to determine the type of the file_ `/usr/bin/cat` _and explain the output in 2-3 sentences._
```
file /usr/bin/cat
```

![](https://miro.medium.com/v2/resize:fit:875/1*KOTModaZmiKiJOwjoVSzLA.png)

Penjelasan :  
1.  `file`  adalah perintah untuk menentukan jenis data atau tipe file berdasarkan kontennya.  
2.  `/usr/bin/cat`  adalah direktori yang digunakan.

**Tentang file :**  
File Command dalam Linux adalah perintah dasar yang digunakan untuk mengklasifikasikan tipe file dengan membaca tanda khusus di dalam isi file tersebut. Contoh perintah dari file sendiri adalah  `file /usr/bin/cat`  yang akan menampilkan informasi bahwa file tersebut merupakan file  _executable_  (ELF) 64-bit. Selain itu, kita bisa melihat informasi tipe file secara ringkas dengan  `file -b namafile`  maupun memeriksa seluruh file dalam sebuah direktori menggunakan perintah  `file *`.

_Question 6_  
_What command can we use to display all printable strings of length ≥ 8 in the file_ `/bin/bash`_?_

**Answer :**
```
strings -n 8 /bin/bash
```

![](https://miro.medium.com/v2/resize:fit:875/1*f8iZaFjWKQRCZ_G4cKJ6cA.png)


Penjelasan :  
1.  `strings`  adalah perintah untuk mencari dan menampilkan karakter yang dapat dicetak (printable strings) dari dalam file binari atau file non-teks.  
2.  `-n 8`  adalah opsi untuk menentukan batas minimal panjang karakter yang akan ditampilkan, dalam hal ini minimal 8 karakter.  
3.  `/bin/bash`  adalah direktori yang digunakan.  
4.  `strings -n 8 /bin/bash`adalah perintah untuk mengekstrak dan menampilkan teks yang terbaca dengan panjang minimal 8 karakter dari file binari bash.

[](https://medium.com/write?source=promotion_paragraph---post_body_banner_better_place_scribble--b0b4d6b63470---------------------------------------)

**Tentang** `strings` :
Strings Command dalam Linux adalah perintah dasar yang digunakan untuk melihat file binari untuk menemukan pesan teks, nama fungsi, atau pustaka yang terdapat di dalamnya tanpa harus menjalankan program tersebut. Contoh perintah dari strings sendiri adalah  `strings nama_file`  yang akan menampilkan semua teks yang tersimpan di dalam file. Selain itu, kita bisa memfilter hasil pencarian dengan menggabungkannya bersama grep seperti  `strings /bin/ls | grep "Copyright"`  maupun mencari lokasi offset data di dalam file dengan perintah  `strings -t d file_binari`.

_Question 7_  
_Given the following output of the_ `file` _command, can you determine what’s wrong with this file?_
```
$ file image.jpg  
image.jpg: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=3ab23bf566f9a955769e5096dd98093eca750431, for GNU/Linux 3.2.0, not stripped
```
Penjelasan :  
Dalam output yang dihasilkan oleh perintah  `file image.jpg`  kita melihat bahwa identitas file tersebut bukan file jpg yang seharusnya. Jika file tersebut berjenis jpg akan menghasilkan output seperti ini :
```
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task$ file MedparRamadhan.jpg  
MedparRamadhan.jpg: JPEG image data, JFIF standard 1.01, resolution (DPCM), density 28x28, segment length 16, baseline, precision 8, 1080x1350, components 3
```
Di dalam contoh yang diberikan file tersebut berisi sebuah program yang dapat dieksekusi (LF 64-bit LSB pie executable) untuk sistem operasi Linux 64-bit. Hal ini merupakan salah satu teknik yang disebut “Extension Masquerading” yang berfungsi untuk menyamarkan suatu file program berbahaya (malware) menjadi jenis file yang tidak dicurigai (dalam hal ini berbentuk jpg). Maka dari itu, file tersebut adalah suatu program yang dapat dijalankan di sistem linux dan bukanlah sebuah gambar berformat jpg.

_Question 8_  
_If we wanted to look for files modified in the last 30 minutes in_ `/home` _directory, what command would we want to use?  
Hint: Explore how you can use_ `find` _command to achieve this._

**Answer :**
```
find /home -mmin -30
```

![](https://miro.medium.com/v2/resize:fit:875/1*fyE-H42YPiHfEasD3qka4A.png)

Penjelasan :  
1.  `find`  adalah perintah dasar Linux yang digunakan untuk mencari file atau direktori dalam hierarki sistem berdasarkan berbagai kriteria.  
2.  `/home`adalah  _path_  yang menentukan lokasi awal pencarian dilakukan.  
3.  `-mmin`  adalah kriteria pencarian berdasarkan waktu modifikasi data dalam satuan menit.  
4.  `-30`adalah argumen numerik yang berarti “kurang dari 30 menit yang lalu”.  
5.  `find /home -mmin 30`  adalah perintah untuk melacak semua file di dalam folder /home yang isinya baru saja diubah dalam 30 menit terakhir.

**Tentang find :**  
Find Command dalam Linux adalah perintah untuk melakukan pencarian file berdasarkan atribut seperti nama, ukuran, tipe, hingga waktu akses. Contoh perintah dari find sendiri adalah  `find . -name "*.txt"`  yang akan mencari semua file teks di direktori saat ini. Selain itu, kita bisa mencari file berdasarkan ukuran dengan  `find / -size +100M`  maupun menghapus file hasil pencarian secara otomatis menggunakan perintah  `find /tmp -mtime +7 -exec rm {} \;`.

_Question 9_  
_What command can we use to display information about all active TCP connections on the system?_

**Answer :**
```
netstat -at
```


![](https://miro.medium.com/v2/resize:fit:875/1*65iEBivTI9eacY5XNK7HrA.png)

Gambar 8 Output question 9

Penjelasan :  
1.  `netstat`  adalah perintah utilitas jaringan yang digunakan untuk menampilkan koneksi jaringan, tabel routing, dan statistik antarmuka.  
2.  `-a`adalah opsi untuk menampilkan semua  _socket_  yang sedang dalam keadaan  _listening_  maupun  _non-listening_.  
3.  `-t`  adalah filter untuk membatasi hasil pencarian hanya pada koneksi yang menggunakan protokol TCP.  
4.  `netstat -at`  adalah perintah untuk melihat daftar seluruh koneksi TCP yang aktif serta  _port_  yang sedang terbuka di sistem.

**Tentang netstat :**  
Netstat Command dalam Linux adalah perintah dasar yang digunakan untuk mendiagnosis masalah jaringan dan memeriksa port yang sedang terbuka. Contoh perintah dari netstat sendiri adalah  `netstat -plnt`  yang akan menampilkan program (PID) mana yang sedang menggunakan port TCP tertentu dalam bentuk angka. Selain itu, kita bisa melihat tabel routing dengan  `netstat -r`  maupun memantau statistik antarmuka jaringan secara real-time menggunakan perintah  `netstat -i`.

_Question 10  
_Given_ [_this corrupted image file_](https://github.com/vonderchild/digital-forensics-lab/blob/main/Lab%2001/files/challenge.png)_, can you find a way to recover and view its contents?  
Hint 1: A quick google search for “magic bytes” might help.  
Hint 2: Explore how_ `hexedit` _can help you here._

_You may download the image using following command:_

curl https://raw.githubusercontent.com/vonderchild/digital-forensics-lab/main/Lab%2001/files/challenge.png -o challenge.png

**Answer :**  
1. Mengetahui terlebih dahulu identitas dari file tersebut.

file challenge.png



![](https://miro.medium.com/v2/resize:fit:875/1*yy_PutKwVlZaeS1oRQH6NQ.png)

pada pemeriksaan identitas dari file tersebut terdapat sesuatu yang aneh, yaitu output dari file tersebut adalah “data”. Hal ini aneh karena output file png yang seharusnya keluar adalah “ png image data” seperti di bawah ini :


![](https://miro.medium.com/v2/resize:fit:875/1*j83_RmhBJ0UfxX6-E-55ew.png)

maka hal ini jelas, terdapat suatu masalah pada  _magic bytes_ header file tersebut dan kita perlu memperbaikinya.

2. Memeriksa raw data dari file challenge.png  
Selanjutnya kita perlu untuk memeriksa raw data yang dimiliki oleh file challenge.png dengan menggunakan tools xxd untuk melihat header file tersebut.
```
xxd challenge.png | head -n 6
```
Disini kita batasin menjadi 6 baris saja karena kita hanya perlu melihat baris awal (header) untuk memperbaiki file tersebut.

![](https://miro.medium.com/v2/resize:fit:875/1*HI06cHgHu0ruxV3aCdBOdA.png)

Ternyata benar terjadi kerusakan pada header. File png seharusnya memiliki magic bytes “89 50 4E 47 0D 0A 1A 0A” sementara pada header file tersebut adalah “17 29 4E 47 0D 0A 1A 0A”.

3. Perbaikan header file  
Dengan menggunakan  `hexedit`  sebagai tools untuk memperbaiki header, kita akan memperbaiki file tersebut menjadi file png yang benar.

hexedit challenge,png

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*pGeiixaJ_bjjKC1NJiT_vg.png)

Gambar 11 Sebelum Perbaikan



![](https://miro.medium.com/v2/resize:fit:875/1*mnR5dYTf4aXCiXv8CKFMMw.png)

Gambar 12 Setelah Perbaikan

Berikut adalah hasil dari perbaikan header file tersebut:

![](https://miro.medium.com/v2/resize:fit:703/1*3sN2MDU2TYxAX7o4peDFjQ.png)

Gambar 13 PNG yang sudah diperbaiki

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*7In4LFwBh6GZvmJekjiboA.png)

Gambar 14 Identitas file PNG

> Terima kasih banyak telah membaca write up ini, sampai berjumpa di write up selanjutnya.
