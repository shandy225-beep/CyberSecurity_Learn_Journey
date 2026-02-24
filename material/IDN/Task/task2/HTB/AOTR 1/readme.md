# AOTR 1 — Hack The Box

> Hii, kembali lagi dengan saya di tugas write up. Kali ini saya akan menyelesaikan tugas AOTR (Advent of The Relics) 1 dari hack the box

**To Do :**  
1. Membaca skenario tugas AOTR  
Tugas AOTR 1 memiliki skenario yang dikemas dalam pdf dan github.  
anda dapat mengakses github pada link berikut :  [https://github.com/hackthebox/advent-of-the-relics](https://github.com/hackthebox/advent-of-the-relics)


![](https://miro.medium.com/v2/resize:fit:875/1*N3T78CiIAcjRKYXBu4qNzQ.png)

2. Mendownload file Zip dan PDF instruksi  
Terdapat skenario kasus pada file pdf dan file berekstensi .eml pada zip untuk dianalisis

![](https://miro.medium.com/v2/resize:fit:875/1*q7x-V7uyQaCiN7AXODHzwA.png)


![](https://miro.medium.com/v2/resize:fit:875/1*TYO5C5AhSfOGDqjaR8JvhQ.png)

## **Skenario :**

On a quiet mid-November evening, a fatigued CALE employee opened an unexpected email and, without much thought, followed the instructions it contained. Moments later, something felt off, panic set in, and he abruptly yanked the power cable from the wall to stop whatever had started. One month later, that same email resurfaces as the crucial starting point of a cyber investigation, holding the first clues to what really happened.

The scenario portrayed in this challenge is entirely fictional and created solely for educational and entertainment purposes. Any resemblance to actual persons, living or dead, organizations, or real events is purely coincidental and unintentional. All characters, scenarios, and data presented are products of imagination.

**_Question 1_**
Who is the suspicious sender of the email?

**Answer :**

1.  Membuka file  `.eml`  dengan menggunakan text editor  
    Saya membuka file eml dari case ini menggunakan file editor untuk menganalisis apa isi yang terdapat pada file tersebut.

![](https://miro.medium.com/v2/resize:fit:875/1*syWibCSKoMD276_YhOqBeA.png)

2. Analisis isi file  `.eml`  
Dari file tersebut kita bisa melihat bahwa yang mengirimkan pesan email tersebut adalah  **eu-health@ca1e-corp.org** sehingga email inilah jawabannya.

**_Question 2_**
What is the legitimate server that initially sent the email?

1.  Menganalisis kembali isi file tersebut

![](https://miro.medium.com/v2/resize:fit:875/1*hOZ5sJXBQ_1FN0ikg9PahA.png)

saya menemukan bahwa server yang digunakan untuk mengirimkan email tersebut adalah  **BG1P293CU004.outbound.protection.outlook.com**

**_Question 3_**
What is the attachment filename?

**Answer :**

1.  Menganalisis isi file

![](https://miro.medium.com/v2/resize:fit:875/1*jhXMNe3pvKyUBAX5PxFnQA.png)

di dalam file ini tertulis content type bertipe multi part, yang artinya tidak hanya berisi pesan teks tapi juga berisi tipe file lainnya didalam pesan.

2. Mencari file yang berada di dalam teks


![](https://miro.medium.com/v2/resize:fit:875/1*YFC_Qpnc4ihfo8wc5q_7Ig.png)

Dari pencarian saya ternyata ada file bernama  **Health_Clearance-December_Archive.zip**  yang dikirimkan bersamaan dengan teks  `.eml`  tersebut. File tersebut masih dalam kondisi terkompresi dan perlu untuk di ekstrak

**Question 4**
What is the Document Code?

**Answer :**

1.  Mengekstrak file  `.zip`  dari file  `.eml`  
    untuk mendapatkan file  `.zip`  dari file  `.eml`  tersebut kita akan memerlukan proses yang namanya MIME Decoding. Untuk itu saya menggunakan  `ripmime`  sebagai tools untuk decoding pesan tersebut. ripmime dirancang khusus untuk membedah struktur email dan mengeluarkan semua lampiran secara otomatis.

Perintah : `ripmime -i ‘URGENT_ Updated Health & Customs Compliance for Cross-Border Festive Event.eml’ -d ./ekstrak`

-   -i : bertugas sebagai input untuk file yang digunakan
-   -d : bertugas sebagai penentu direktori file yang akan digunakan

![](https://miro.medium.com/v2/resize:fit:875/1*8Fj5ydxnPtEr1ESAmWwmgA.png)

![](https://miro.medium.com/v2/resize:fit:875/1*g9X7xfvWReescdbsPI_U1A.png)

file  `.eml`  tersebut telah terekstrak dan menghasilkan 3 file.

2. Mengekstrak file  `.zip`  yang ada  
selanjutnya kita perlu mengekstrak file  `.zip`  yang ada pada file tersebut yakni **Health_Clearance-December_Archive.zip** dengan metode  `unzip`

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:875/1*aV6-mnDsJwY2PP3QkWfSAw.png)

disiini memerlukan password, password bisa kita dapatkan dari dokumen pdf yang telah diberikan yakni  **Up7Pk99G**

![](https://miro.medium.com/v2/resize:fit:875/1*MLUTUQfEfmki-yUIfVZAsg.png)

dari hasil ekstraksi file zip tersebut kita mendapatkan dua file utama yakni Health_Clearance_Guidelines.pdf yang menjadi dokumen pengecoh seolah olah ini merupakan perintah resmi kemudian EU_Health_Compliance_Portal.lnk yang menjadi shortcut untuk menjalankan file tersebut.

3. Mendapatkan kode dokumen tersebut.


![](https://miro.medium.com/v2/resize:fit:875/1*ZYRumlJxTJBasWr8aMCgvQ.png)

sesuai dengan tujuan awal untuk mendapatkan kode dokumen tersebut maka kita mendapatkan kodenya adalah  **EU-HMU-24X**

**_Question 5_**
What is the full URL of the C2 contacted through a POST request?

[](https://medium.com/plans?source=promotion_paragraph---post_body_banner_rabbit_hole_scribble--249fe9502220---------------------------------------)

**Answer :**

Karena kita membutuhkan url yang terhubung melalui POST request maka kita perlu mengidentifkasi lagi file  `link`  yang terdapat pada hasil ekstraksi tadi.

1.  Gunakan  `Strings`  untuk mengetahui isi file  `.lnk`  secara aman.

disini kita menggunakan perintah string untuk membaca isi dokumen EU_Health_Compliance_Portal.lnk. Tetapi kita tidak bisa menggunakan string biasa, kita penggunakan perintah :

`strings -el EU_Health_Compliance_Portal.lnk`  
**penjelasan :**

-   Jika kamu hanya menjalankan  `strings file.lnk`, kamu mungkin tidak akan melihat apa-apa karena teksnya tersimpan dalam format 16-bit.
-   Dengan menjalankan  `strings -el file.lnk`, kamu memaksa  `strings`  untuk membaca format Unicode tersebut, sehingga perintah tersembunyi seperti  **PowerShell**  atau  **URL**  akan muncul dengan jelas.

![](https://miro.medium.com/v2/resize:fit:875/1*kI4FgfyLeI439qQpeUizmw.png)

disini kita mendapatkan url yang dibutuhkan dalam kasus ini adalah `https%3A%2F%2Fhealth%2Dstatus%2Drs%2Ecom%2Fapi%2Fv1%2Fcheckin` . Tapi ada yang aneh disini karena sepertinya link tersebut mengalami encoding.

2. Decoding Link  
Untuk mengetahui isi link tersebut kita perlu melakukan decoding terhadap url tersebut. disini saya menggunakan  `python`  untuk melakukan decoding dengan perintah :

`python3 -c “import urllib.parse; print(urllib.parse.unquote(‘https%3A%2F%2Fhealth%2Dstatus%2Drs%2Ecom%2Fapi%2Fv1%2Fcheckin’))”`

maka kita akan mendapatkan hasil seperti ini

![](https://miro.medium.com/v2/resize:fit:875/1*2PCCgkNivIAore79Uqwlxw.png)

kita bisa mendapatkan link yang sesuai yakni  [**https://health-status-rs.com/api/v1/checkin**](https://health-status-rs.com/api/v1/checkin)

**_Question 6_**
The malicious script sent three pieces of information in the POST request. What is the registry key from which the last one is retrieved?

**Answer :**

Dari powershell script yang diberikan oleh penyerang kita bisa mengetahui tentang 3 informasi dalam post request. tetapi registry key yg diambil oleh penyerang adalah

![](https://miro.medium.com/v2/resize:fit:875/1*q8zdyXsyYF89H_RJ_dedBA.png)

**_HKLM\SOFTWARE\Microsoft\Cryptography\MachineGuid_**

disini sebenarnya post digunakan Menerima data profil korban (Username, Domain, dan MachineGuid) melalui metode  **HTTP POST**.

-   **Cara Kerja**: Skrip mengirimkan paket data  `$pP`  ke sini. Jika berhasil, server ini akan memberikan respons balik berupa kode atau ID unik (yang disimpan dalam variabel  `$Zu`).

**_Question 7_**
Then the script downloads and executes a second stage from another URL. What is the domain?

**Answer :**

Dari script powershell diatas kita mengetahui bahwa penyerang melakukan exfiltration pada script url pertama dan pada script kedua digunakan untukBertindak sebagai server  _Command and Control_  (C2) untuk mengirimkan instruksi atau  _malware_  tambahan ke komputer korban.

**Cara Kerja**: Skrip menghubungi tautan ini dengan menambahkan ID unik yang didapat dari link pertama (`$Lj$Zu`).

Jika sesuai pertanyaan, maka second url perlu decode

1.  decode second URL

`https%3A%2F%2Fadvent%2Dof%2Dthe%2Drelics%2Dforum%2Ehtb%2Eblue%2Fapi%2Fv1%2Fimplant%2Fcid%3D`



![](https://miro.medium.com/v2/resize:fit:875/1*Jem4P9gxNT26I2H4EAgj0Q.png)

kita decode menggunakan python tadi

`python3 -c “import urllib.parse; print(urllib.parse.unquote(‘https%3A%2F%2Fadvent%2Dof%2Dthe%2Drelics%2Dforum%2Ehtb%2Eblue%2Fapi%2Fv1%2Fimplant%2Fcid%3D’))”`



![](https://miro.medium.com/v2/resize:fit:875/1*6M07YXIpiGuSmqdK5m60Jw.png)

maka kita mendapatkan urlnya adalah :

`[https://advent-of-the-relics-forum.htb.blue/api/v1/implant/cid=](https://advent-of-the-relics-forum.htb.blue/api/v1/implant/cid=)`

tetapi kita hanya membutuhkan domainnnya sehingga jawabannya adalah  **advent-of-the-relics-forum.htb.blue**

**_Question 8_**A set of credentials was used to access the previous resource. Retrieve them.

**Answer :**

dari powershell script sebelumnya sebenarnya kita bisa mendapatkan kredensial yg digunakan, lebih tepatnya pada bagian ini  
**$Bs = (-join(‘Basic c3’,’ZjX3Rlb’,’XA6U2',’5',’vd0JsY’,’WNrT’,’3V’,’0X’,’zIwM’,’jYh’))**

tetapi kredensial ini masih belum dalam bentuk yg sebenarnya karena dimanipulasi oleh penyerang agar tidak terdeteksi oleh antivirus. maka kita harus merekonstruksi kredensial tersebut menjadi

**Basic c3ZjX3RlbXA6U25vd0JsYWNrT3V0XzIwMjYh**

dalam hal ini kredensial masih berbentuk base64 encoding dan kita perlu untuk melakukan decoding.

1.  Melakukan decode base 64

gunakan perintah `echo “c3ZjX3RlbXA6U25vd0JsYWNrT3V0XzIwMjYh” | base64 -d`

![](https://miro.medium.com/v2/resize:fit:875/1*KcA1bt7rSkPl2jwNOqrTFg.png)

kita mendapatkan kredensialnya adalah  **svc_temp:SnowBlackOut_2026!**

> Baiklah cukup sekian writeup dari saya, apabila ada kesalahan saya mohon maaf. Happy learning semuanyaa
