
### Cyber Defender — PsExec Hunt Lab

Halo teman teman, kembali lagi dengan saya di CTF cyber defender yang berjudul **PsExec Hunt Lab.** Di ctf ini sendiri akan membahas tentang kegiatan investigasi forensik dari serangan yang dilakukan melalui file PCAP. Disini kita akan menganalisis bagaimana metode penyerangan, siapa yang menyerang, menggunakan mesin apa yang menyerang dan lainnya.

> **Skenario :**

> An alert from the Intrusion Detection System (IDS) flagged suspicious lateral movement activity involving PsExec. This indicates potential unauthorized access and movement across the network. As a SOC Analyst, your task is to investigate the provided PCAP file to trace the attacker’s activities. Identify their entry point, the machines targeted, the extent of the breach, and any critical indicators that reveal their tactics and objectives within the compromised environment.

Disini IDS mendeteksi adanya kegiatan mencurigakan yang melibatkan PsExec. Hal ini mengindikasikan adanya akses dan gerakan tidak sah yang terjadi di jaringan. Kita bertugas menganalisis file PCAP yang diberikan untuk mengetahui seluruh aktivitas hacker, mengidentifikasi entry pointnya, dan indikator indikator lainnya.

![](https://cdn-images-1.medium.com/max/1000/1*gZj7U5q09HrxyuqBGzhboQ.png)

Pertama, kita akan mendownload file labnya terlebih dahulu dan karena file nya masih berbentuk zip kita harus mengekstraknya terlebih dahulu dengan menggunakan perintah `unzip`
```
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/cyberdef_psexec$ unzip 143-psexec-hunt.zip  
Archive:  143-psexec-hunt.zip  
[143-psexec-hunt.zip] temp_extract_dir/psexec-hunt.pcapng password:   
  inflating: temp_extract_dir/psexec-hunt.pcapng    
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/cyberdef_psexec$ ls  
143-psexec-hunt.zip  temp_extract_dir  
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/cyberdef_psexec$ cd temp_extract_dir  
shandy@Axioo-H7:~/CyberSecurity_Learn_Journey/material/IDN/Task/task3/cyberdef_psexec/temp_extract_dir$ ls  
psexec-hunt.pcapng
```
**Q1 : To effectively trace the attacker’s activities within our network, can you identify the IP address of the machine from which the attacker initially gained access?**

Pada challenge pertama kita diminta untuk mencari alamat IP dari mesin yang digunakan oleh penyerang. Maka kita akan menganalisis file PCAP untuk mengetahui sesuatu yang aneh dari log aktivitas yang ada.

Saya mulai mencari dari statistic terutama bagian conversation, untuk memeriksa apakah ada aktivitas yang aneh di dalam PCAP tersebut.

![](https://cdn-images-1.medium.com/max/1000/1*3aoDj8ptZHA7bvtgFdvdYQ.png)

disini saya menemukan IP address dengan conversation paling banyak berasal dari ip address `10.0.0.130` yang tentu saja sangat mencurigakan karena berjumlah mencapai puluhan ribu dibandingkan request lainnya yang paling tinggi hanya ratusan. Maka kita bisa menginvestigasi lebih lanjut tentang IP Adress ini.

![](https://cdn-images-1.medium.com/max/1000/1*m8W2A8QtSaKtubFTfdL7Ng.png)

Setelah melihat aktivitas yang dilakukan oleh 10.0.0.130 terhadap 10.0.0.133 Kita bisa mendapatkan beberapa poin penting disini :  
1. Ip adress 10.0.0.130 melakukan tcp handshake secara normal dengan 10.0.0.133 dan menghasilkan hubungan koneksi.  
2. kemudian .130 melakukan perubahan metode conversation menjadi smb dan diterima oleh .133  
3. .130 melakukan login ke server .133 dengan metode NLTMSSP AUTH dengan username /ssales dan kemudian berhasil login. (Kemungkinan penyerang telah memiliki username dan password akun /ssales)  
4. Selanjutnya .130 melakukan request untuk mengakses admin di dalam mesin .133 dan diterima oleh .133 karena akun /ssales ternyata memiliki otoritas untuk itu. Proses ini disebut lateral movement yaitu proses setelah penyerang masuk ke satu komputer (misal via _phishing_), mereka tidak berhenti di situ. Mereka menggunakan SMB untuk “melompat” ke komputer lain di jaringan yang sama.  
5. Pengiriman file PSEXECSVC.exe ke .133 dari host .130 dengan PDU\

Dari aktivitas ini kita telah mengetahui apa metode yang digunakan kemudian IP Mana yang digunakan untuk mengakses mesin awal.

**answer : 10.0.0.130**

**Q2 : To fully understand the extent of the breach, can you determine the machine’s hostname to which the attacker first pivoted?**

Selanjutnya untuk mencari tingkat penyebaran serangan kita harus mencari hostname mesin mana yang pertama kali terkena pivot oleh attacker. Pivot (atau _Pivoting_) adalah teknik di mana penyerang menggunakan satu komputer yang sudah berhasil mereka kuasai sebagai “batu loncatan” untuk menyerang komputer lain di dalam jaringan yang sama. Untuk ini kita akan melihat proses dimana ntlsmssp challenge berlangsung karena disini korban mengirimkan identitas mesinnya kepada penyerang sehingga bisa dieksploitasi oleh penyerang.

![](https://cdn-images-1.medium.com/max/1000/1*omgQNY7Pksc-3hjMAWOKaA.png)

Dari sini bisa kita lihat identitas hostname mesin yang menjadi target pivot penyerang adalah SALES-PC.

**Answer : SALES-PC**

**Q3 : Knowing the username of the account the attacker used for authentication will give us insights into the extent of the breach. What is the username utilized by the attacker for authentication?**

pada challenge ini kita diminta untuk mencari tahu username yang digunakan untuk melakukan autentifikasi. Seperti yang kita tahu sebelumnya pada challenge 1 bahwa penyerang menggunakan username /ssales untuk autentifikasi lalu berhasil

![](https://cdn-images-1.medium.com/max/1000/1*_A2yTfKTihILHpZo8bbs7w.png)

**Answer : ssales**

**Q4 : After figuring out how the attacker moved within our network, we need to know what they did on the target machine. What’s the name of the service executable the attacker set up on the target?**

Setelah kita mengetahui bagaimana metode mereka bergerak, kita akan mencari tahu tentang program eksekusi yang mereka tanamkan ke target. Kembali lagi ke challenge no 1, kita mengetahui bahwa program eksekusi yang ditanamkan adalah PSEXESVC.

![](https://cdn-images-1.medium.com/max/1000/1*oM-99tj8AxNri9rZ22vjXA.png)

**Answer : PSEXESVC**

**Q5 : We need to know how the attacker installed the service on the compromised machine to understand the attacker’s lateral movement tactics. This can help identify other affected systems. Which network share was used by PsExec to install the service on the target machine?**

Pada challenge ke 5 kita diminta untuk mengetahui bagaimana penyerang bisa menginstall service di komputer lainnya dengan menggunakan taktik lateral movement.

![](https://cdn-images-1.medium.com/max/1000/1*qzkNn4A0U_ScLVpR9leYvQ.png)

sebenarnya kita bisa lihat disini, Penyerang melakukan Tree Connect Request ke folder tersembunyi `ADMIN$`. Karena server merespons dengan sukses, ini membuktikan bahwa akun `ssales` memiliki otoritas Lokal Administrator pada host `.133`. Tanpa hak admin, langkah ini pasti akan gagal (Access Denied). Jadi jaringan yang berbagi yang digunakan oleh PSExec untuk menginstall mesin target adalah admin$.

**Answer : admin$**

**Q6 : We must identify the network share used to communicate between the two machines. Which network share did PsExec use for communication?**

Selanjutnya kita diminta untuk mengidentifikasi network share mana yang digunakan oleh PSExec untuk komunikasi. Kita perhatikan kembali pada bagian request tree IPC oleh .130 ke .133.

![](https://cdn-images-1.medium.com/max/1000/1*edircnTCWb4D9RfUJpmIRg.png)

`IPC$` (Inter-Process Communication) adalah sebuah _network share_ tersembunyi yang tidak digunakan untuk menyimpan file, melainkan sebagai "jalur pipa" komunikasi antar program di jaringan. `IPC$` digunakan untuk mengontrol file tersebut, mengirim input perintah, dan menerima output (hasil perintah) kembali ke penyerang. Pada paket 134, penyerang melakukan `Tree Connect Request` ke `\\10.0.0.133\IPC$`. Ini adalah langkah pertama untuk membangun saluran perintah.

Jadi network share yang digunakan untuk komunikasi adalah IPC$

Answer : IPC$

**Q7 : Now that we have a clearer picture of the attacker’s activities on the compromised machine, it’s important to identify any further lateral movement. What is the hostname of the second machine the attacker targeted to pivot within our network?**

Selanjutnya kita diminta untuk mencari hostname mesin kedua yang menjadi target pivoting oleh penyerang. saya menggunakan fitur statistic pada wireshark kemudian melihat conversation lain yang mencurigakan dan saya menemukan bahwa conversation lainnya yang mencurigakan yaitu dengan destinasi ke **10.0.0.131.**

![](https://cdn-images-1.medium.com/max/1000/1*JDo0_X5J1mH6biJ0dht0pQ.png)

Lalu saya melakukan filtering untuk menemukan ip adress yang sesuai yaitu :

ip.addr == 10.0.0.131 && ip.addr == 10.0.0.130

saya ingin mencari tahu apa yang terjadi diantara kedua IP tersebut. Lalu saya mendapatkan hal ini

![](https://cdn-images-1.medium.com/max/1000/1*AMEdxESEp9EXN3WinQwpOg.png)

sama dengan challenge sebelumnya, penyerang mencoba melakukan pivoting tetapi mengalami kegagalan pada sesi autentifikasi karena tidak memiliki data yang benar untuk login. Ohiya untuk menemukan hostname second machine saya mencari pada balasan NTLSMSPP Challenge untuk mengetahui identitas dari mesin kedua dan saya mendapatkan bahwa hostname second machine adalah

![](https://cdn-images-1.medium.com/max/1000/1*4mwVCzG96yDrQvsIJxSIEQ.png)

**Answer : MARKETING-PC**

> Baiklah, cukup sekian dari saya. apabila ada yg kurang jelas dan salah saya mohon maaf. Karena kita sama sama belajar disni, happy learning all
