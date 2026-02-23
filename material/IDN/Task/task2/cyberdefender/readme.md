
### Cyber Defender Practice — Web Labstrike

> Halo teman teman semuanya, kembali lagi dengan saya di write up cyber security blue team. Saya disini akan menyelesaikan cyber defender ctf tentang web labstrike.

**Tools**  
- Wireshark : Untuk menangkap dan membedah paket data guna memantau lalu lintas jaringan, mencari masalah koneksi, dan melakukan audit keamanan siber.  

**To do :  
1.** Buka cyber defender dan open labnya

![](https://cdn-images-1.medium.com/max/1000/1*JAPdLd1SPMJEpS9f2Dv-HA.png)

2. Buka file start here dan cari dokumen pcap webstrike

![](https://cdn-images-1.medium.com/max/1000/1*8ooe2Ch-nvaZcejUU34hZg.png)

3. Analisis dan jawab pertanyaan di bawah ini

![](https://cdn-images-1.medium.com/max/1000/1*VZlpQDr3Xpg7ZssEZT78Og.png)

**Scenario**  
A suspicious file was identified on a company web server, raising alarms within the intranet. The Development team flagged the anomaly, suspecting potential malicious activity. To address the issue, the network team captured critical network traffic and prepared a PCAP file for review.  
Your task is to analyze the provided PCAP file to uncover how the file appeared and determine the extent of any unauthorized activity.

**_Question 1_**  
Identifying the geographical origin of the attack facilitates the implementation of geo-blocking measures and the analysis of threat intelligence. From which city did the attack originate?  
💡 **Note:** The lab machines do not have internet access. To look up the IP address and complete this step, use an IP geolocation service on your local computer outside the lab environment.

**Answer :**

1.  Analisis IP Adress Penyerang dan server

![](https://cdn-images-1.medium.com/max/1000/1*TNIl_5NH6u3GVwmux5I8Vw.png)

Kita bisa melihat disini penyerang disini memiliki IP Adress `117.11.88.124` dan webserver memiliki ip `24.29.63.79` dari IP ini kita dapat mencari kota darimana ip berasal sehingga kita bisa menerapkan blocking geolokasi pada sistem kita. Disini saya menggunakan [https://ipgeolocation.io/](https://ipgeolocation.io/) untuk menentukan dari kota mana IP Adress ini berasal.

![](https://cdn-images-1.medium.com/max/1000/1*LEFF2Fd1Cda8-1Mkzmxirw.png)

dan ternyata IP berasal dari kota Tianjin di china.

![](https://cdn-images-1.medium.com/max/1000/1*t1hACPtKFRIkqC5sixGGoA.png)

**Question 2**  
Knowing the attacker’s User-Agent assists in creating robust filtering rules. What’s the attacker’s Full User-Agent?

**Answer :**

1.  Memilih proses penyerangan yang tepat.  
    untuk mencari user agent penyerang kita perlu mencari proses dimana penyerang mencoba untuk menyerang webserver kita. pada case ini saya menggunakan proses `get` untuk melihatnya karena penyerang mencoba mengakses halaman admin.

![](https://cdn-images-1.medium.com/max/1000/1*uavT-v64oN5oQT2epcXvWA.png)

2. Menggunakan fitur `follow`  
fitur follow digunakan untuk untuk melihat seluruh kode HTML, skrip PHP yang diunggah, atau respon dari server web dan permintaan dari user.

![](https://cdn-images-1.medium.com/max/1000/1*QH3j5RC9ZfJomdS_Yy82wQ.png)

3. Mendapatkan user agent penyerang  
Ternyata user agent penyerang adalah **Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0** Maka kita mendapatkan user agentnya.

![](https://cdn-images-1.medium.com/max/1000/1*K-EiSg1NVJ3xvLrRVEMCXQ.png)

**Question 3**  
We need to determine if any vulnerabilities were exploited. What is the name of the malicious web shell that was successfully uploaded?

**Answer :**

disini kita diminta untuk melihat apa nama dari malicious web shell yang berhasil diupload.  
1. Mencari proses `POST`  
karena dengan methode post, penyerang dapat mengupload data ke web server yang bisa berisi Malicious web shell. Kita bisa menggunakan filter `http.request.methode == POST`untuk mencari proses mana yang menggunakan methode post untuk mengupload suatu data.

![](https://cdn-images-1.medium.com/max/1000/1*WH-tmRQgMM86Xl1ONBjsVQ.png)

2. Melihat isi webshell yang berhasil diupload di suatu proses menggunakan fitur `follow`  
untuk melihat web shell yang berhasil diupload kita perlu mencari proses mana yang berhasil mengupload web shell tersebut dan saya menemukan webshell yang diupload adalah `image.jpg.php`

![](https://cdn-images-1.medium.com/max/1000/1*zQ_A0-MpFWXnArsTfUoVkg.png)

![](https://cdn-images-1.medium.com/max/1000/1*GkHjJ_njkhlofbjtbn21QQ.png)

![](https://cdn-images-1.medium.com/max/1000/1*VhbL7h8O2c0zxOBFt9JA-g.png)

**Question 4**
Identifying the directory where uploaded files are stored is crucial for locating the vulnerable page and removing any malicious files. Which directory is used by the website to store the uploaded files?

**Answer :**
Dari pertanyaan sebelumnya kita telah mengetahui malicious file yang diupload oleh penyerang yaitu “image.jpg.php” karena tidak mungkin suatu images memiliki akhiran .php, teknik ini disebut juga sebagai double extension untuk mengelabui sistem keamanan. Kita perlu mencari direktori file yang diupload.

1.  mencari proses direktori file diupload  
    untuk mencari direktori file tersebut kita perlu mengetahui proses mana yang memiliki peran dalam penguploadan file tersebut. Dengan menggunakan filter `http.request.uri contains "image.jpg.php"` kita bisa mendapatkan proses yang kita cari

![](https://cdn-images-1.medium.com/max/1000/1*3_IE4upQr6ruMmI8CnZWHQ.png)

2. Menggunakan fitur `follow` untuk mengetahui isi proses

![](https://cdn-images-1.medium.com/max/1000/1*oVgX-qmjJuR6hsGjolBcOQ.png)

di proses ini sebenarnya penyerang menggunakan methode `GET` untuk menjalankan script pada web server.

3. Mendapatkan direktori file  
dari gambar diatas kita bisa mengetahui direktori file yang diupload yaitu pada “/reviews/uploads/”.

![](https://cdn-images-1.medium.com/max/1000/1*YF27Qu9_jCq_GUaY0fz2dA.png)

**Question 5**
Which port, opened on the attacker’s machine, was targeted by the malicious web shell for establishing unauthorized outbound communication?

**Answer :**
Kita disini bertugas untuk mencari port yang digunakan penyerang untuk mendirikan komunikasi yang tidak sah.

1.  Mencari method POST  
    untuk menemukan port yang digunakan penyerang kita menggunakan method post karena post sendiri bisa dipastikan dilakukan oleh penyerang.

![](https://cdn-images-1.medium.com/max/1000/1*jr9U0npZw7K9Dkh2y_4QXw.png)

2. Gunakan `follow` untuk mengetahui detail proses

![](https://cdn-images-1.medium.com/max/1000/1*RpdGa5DNy0zJzGYmKYLUtg.png)

seperti yang kita lihat pada bagian bawah dari request penyerang terdapat IP yang digunakan kemudian port yang digunakan yaitu `8080`

![](https://cdn-images-1.medium.com/max/1000/1*Vliko66KrjkbgDn3f5o5CA.png)

**Question 6**
Recognizing the significance of compromised data helps prioritize incident response actions. Which file was the attacker attempting to exfiltrate?

**Answer :**
Untuk mencari file yang dicari untuk dicuri oleh penyerang (exfiltration) kita bisa memanfaatkan port dari penyerang untuk mengetahui proses exfiltrasi dari penyerang (8080).

1.  Menerapkan filter untuk mencari proses exfiltration  
    filter yang bisa kita gunakan adalah `tcp.dstport == 8080` , kenapa kita menggunakan filter ini ? karena perintah ini adalah perintah untuk mencari proses tcp yang memiliki destination port 8080.

![](https://cdn-images-1.medium.com/max/1000/1*KtqOCB8RYrtixu9hk8AkYg.png)

2. Menggunakan fitur `follow` untuk mengetahui detail proses  
dengan menggunakan follow kita akan mengetahui tcp stream yang ada pada proses tersebut. kita akan melihat apa yang coba dilakukan penyerang terhadap web server.

![](https://cdn-images-1.medium.com/max/1000/1*NsyrFnMyWMOr09lqCTJkMw.png)

3. Menganalisis perilaku penyerang

![](https://cdn-images-1.medium.com/max/1000/1*YCpDDiHwrAoeenzKNl6CNA.png)

![](https://cdn-images-1.medium.com/max/1000/1*bZ9M_6oHsPQGhrFipBZf_w.png)

kita bisa melihat ternyata penyerang mampu menjalankan terminal walaupun terbatas. hal ini tentunya berbahaya karena penyerang bisa melakukan hal apapun terhadap web server kita. penyerang bisa melihat siapa yang memiliki akses terhadap server yang kita miliki. dan pada akhirnya penyerang menjalankan perintah `POST` yang mengupload file passwd untuk dicuri. Ternyata file yang hendak dicuri adalah `passwd`

> Terima kasih semuanya telah membacaa, lesgoo belajar barengg
