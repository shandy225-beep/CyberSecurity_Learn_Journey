
### AOTR 2 — Hack The Box

Halo teman teman semuanya, disini saya akan menyelesaikan challenge AOTR (Advent of The Relics ) 2 — Operation Winter Blackout. AOTR 2 sendiri merupakan challenge yang berbasis digital forensik yang bertujuan untuk memecahkan masalah dalam skenario challengen ini. AOT2 2 : Operation Winter Blackout memiliki 20 challenge di dalamnya.

**Scenario**

> A phishing attack on a logistics company has led investigators to a [private criminal forum](https://advent-of-the-relics-forum.htb.blue/). The crew behind it has been planning something big for months. Your job is to dig through their communications and uncover who they are, what they’re targeting, and how they plan to disappear.

> The forum can be accessed at: [**https://advent-of-the-relics-forum.htb.blue**](https://advent-of-the-relics-forum.htb.blue/)

> First, you will need to gain access to the forum using the password **SnowBlackOut_2026!**, which you found in your [previous investigation](https://app.hackthebox.com/sherlocks/Advent%2520of%2520The%2520Relics%25201%2520-%2520A%2520Call%2520from%2520the%2520Museum?tab=play_sherlock), to unlock the intel inside.

Pada skenario ini kita diminta untuk menginvestigasi forum privat kriminal yang bertujuan untuk mengetahui rencana mereka berdasarkan komunikasi yang dilakukan baik dari siapa mereka, apa yang mereka targetkan dan bagaimana rencana mereka untuk menghilang.

Kita akan masuk ke forum mereka melalui tautan [**https://advent-of-the-relics-forum.htb.blue**](https://advent-of-the-relics-forum.htb.blue/)  yang memiliki tampilan seperti berikut.

![](https://cdn-images-1.medium.com/max/800/1*Pbq7HnX7zYbikk41ZY1vfw.png)

Untuk masuk ke forum ini kita memerlukan kata sandi yang sebenarnya telah kita temukan pada case sebelumnya (AOTR 1) yaitu **SnowBlackOut_2026!.** Setelah kita memasuki website dengan kata sandi tersebut maka akan menampilkan tampilan seperti ini

![](https://cdn-images-1.medium.com/max/800/1*-URy4UxYBwVahYogw9sb-w.png)

Pada web ini kita bisa melihat topik apa saja yang mereka bicarakan, komunikasi yang dilakukan, bahkan jumlah anggota dari para penyerang.

**Q1 : How many suspects are using this forum?**

Pada tugas pertama kita diminta untuk mencari berapa banyak suspect atau tersangka dalam kasus ini. pada tampilan web tersebut terdapat bagian `member` yang bisa menjadi petunjuk untuk challenge ini.

![](https://cdn-images-1.medium.com/max/800/1*MfizPA7gzTepVqyzC7HoKA.png)

Dari gambar ini saya mendapatkan bahwa ada 5 orang yg bisa dijadikan tersangka dalam kasus ini yaitu :  
1. Curator : Ops Lead  
2. Bellringer : Sytems/Malware  
3. Snowfox : Signals/intel  
4. Driver_BUD : Logistic  
5. Ledger : Finance/infra

Dan setelah saya submit jawabannya ternyata benar, terdapat 5 suspect.

**Q2 : What is the username of the group’s leader?**

Berdasarkan nama 5 orang tersangka dan peran dari challenge sebelumnya saya menyadari bahwa pemimpin dari operasi ini adalah `curator` . Karena jelas tertulis pada peran dari si curator sebagai ops lead yang memimpin jalannya operasi

![](https://cdn-images-1.medium.com/max/800/1*5044ftDS5jYxHXSGCCWCIA.png)

Dan jawabannya tersebut benar ketika saya submit.

**Q3 : What is Driver_BUD’s real first name?**

Pada challenge ketiga kita diminta untuk mencari tahu tentang nama pertama asli dari pelaku yang memiliki nama samaran Driver_BUD’s. Pada awalnya saya mencoba mencari tahu melalui kolom physical logistic yang mungkin ada petunjuk tentang nama asli Driver_BUD’s.

![](https://cdn-images-1.medium.com/max/800/1*LdSc0wGCR18QIkDPk3-5NQ.png)

Tetapi pada kenyataannya saya tidak menemukan apapun yang berhubungan dengan nama asli si driver. Lalu saya berpikir, jika mereka bisa lengah dalam hal membagikan informasi sensitif seperti nama itu berarti bukan berada di segmen sesuatu yang penting. lalu, saya coba menginvestigasi kolom off topic yang ada.

![](https://cdn-images-1.medium.com/max/800/1*CQlqiBXy1lsnkolsBHhDBA.png)

setelah saya membaca beberapa kolom pesan, saya menemukan suatu topic menarik yang ternyata terdapat nama asli si driver yaitu topic **Rip diesel**

![](https://cdn-images-1.medium.com/max/800/1*g023-bsB8qYXw0vZMjv_6g.png)

Disini driver_bud’s berduka akan kematian diesel, teman terdekat dari si driver. Dan tanpa sengaja snowfox menyebutkan nama asli si driver yaitu **Daniel.**

Ketika saya coba submit ternyata jawabannya benar.

**Q4 : What is the codename of the operation?**

Pada challenge ini kita diminta untuk mencari nama operasi (codename) dari operasi penyerangan ini. mata saya langsung tertuju ke segmen **operation** yang dimana bisaa jadi ada jawabannya disana.

![](https://cdn-images-1.medium.com/max/800/1*50lU4CaT1qeoMT3qYZIEpQ.png)

Setelah saya baca topik pertama saya menemukan codename dari operasi ini.

![](https://cdn-images-1.medium.com/max/800/1*rMuj2pLKpt03JR4t5r1qyw.png)

Ternyata codename nya adalah **Winter Blackout** . ketika saya coba submit ternyata benar.

**Q5 : What single word is the trigger code that activates all nodes?**

Kita diminta untuk mencari satu kata yang akan memicu sebuah kode yang akan mengaktifkan semua node yang ada. Seperti yang kita baca sebelumnya pada kolom, **Operation** berisi tentang timeline, komando strategi, target dan log eksekusi operasi. Dalam hal ini, satu kata trigger pasti terdapat dalam komando strategi sehingga kita perlu mencari tahu satu kata itu disini.

![](https://cdn-images-1.medium.com/max/800/1*50lU4CaT1qeoMT3qYZIEpQ.png)

setelah membaca beberapa threads, terdapat satu threads yang mencurigakan disini yaitu sync logic. saya membaca pesan paling atas yang menyatakan bahwa ternyata satu kata yang mentrigger semua node adalah **FROST.**

![](https://cdn-images-1.medium.com/max/800/1*LbSwMa-pzCYXLuluAnBr5Q.png)

Ketika saya submit ternyata jawabannya benar.’

**Q6 : What is the name of the fake exhibition used as cover for the heist?**

Dalam rencananya ternyata penyerang mengadakan pameran yang dilakukan untuk menutupi rencana pencurian tersebut. kita diminta untuk mencari nama pameran tersebut. Disini tentunya mata saya langsung tertuju pada physical Logistic, karena disini memerlukan peran logistik sebagai penyelenggara pameran. maka dari itu saya coba menginvetigasi thread yang ada disini dan menemukan thread yang mencurigakan yaitu **Forged Manifests: “Miracles of Winter” Exhibition.** Dari namanya saja sebenarnya kita mengetahui bahwa nama pamerannya adalah miracles of winter tetapi untuk memastikannya saya membaca pesan pertama dalam thread dan benar saja itu nama pamerannya.

![](https://cdn-images-1.medium.com/max/800/1*G0euRg3sIexRLu-7hNVZ0g.png)

dan ketika saya submit, jawaban saya benar.

**Q7 : What time was the single word trigger scheduled to execute on New Year’s Eve?**

Kembali ke segmen operation, karena kita diminta untuk mencari waktu dimana kata “FROST” akan di eksekusi untuk mengaktifkan semua operasi. di Thread yang sama, kita akan menemukan pesan

![](https://cdn-images-1.medium.com/max/800/1*Rq7orDIIiGW6cMizSdgi8g.png)

pesan ini mengatakan bahwa setiap anggota harus mematuhi waktu eksekusi pada pukul 23:59:50 dan dihitung melalui countdown hingga 0.

ketika saya submit jawaban saya ternyata benar.

**Q8 : What is the full name of the phishing target at CALE?**

Tentunya ketika mendengar kata phising saya langsung terpikir bahwa ini mungkin terdapat di segmen malware and stuff. Kita diminta untuk mencari tahu nama orang yang menjadi target phising di CALE.

![](https://cdn-images-1.medium.com/max/800/1*8PL6iAZp0FE76x-sj8NCag.png)

saya mencari tahu dan membaca satu thread mencurigakan yakni **Operation Winter Blackout: Patient Zero (Belgrade).** Tentunya patient zero disini bermaksud tentang orang yang biasanya menjadi korban pertama.

![](https://cdn-images-1.medium.com/max/800/1*K6KtIZpZrfYX_BAQjs7ONw.png)

setelah dibaca dan dianalisis lebih lanjut, saya menemukan orang yang menjadi primary target oleh snowfox adalah **Kamil Poltavez**, seorang koordinator logistik yang sedang overtime dan rawan untuk menjadi korban.

saya menjawab ini dan ternyata benar.

**Q9 : What make and model is the truck used for transport?**

Disini kita diminta untuk mencari tahu tentang model truck yang digunakan sebagai transportasi. tentunya kita bisa mencari tahu pada segment physical logistics. kita analisis thread yang ada dan terdapat satu model truck yang ada yaitu Volvo FH.

![](https://cdn-images-1.medium.com/max/800/1*6YzHv2yFyauiHkecjdc5TA.png)

setelah kita jawab ternyata benar itu jawbaannya.

**Q10 : What is the name of Ledgers cat?**

Membahas tentang nama seorang kucing tentulah bukan sesuatu yang penting. maka kita bisa mencari tahu tentang nama kucing dari si ledgers. sebenarnya saya menemukan thread yang menarik yakni **Holiday stress relief (or lack thereof).** Seperti yang kita tahu ya, kucing bisa jadi pereda stres bagi seseorang.

![](https://cdn-images-1.medium.com/max/800/1*Y25iE_o9mpIT6w2lAX2yZQ.png)

Setelah kubaca pesan ini, aku sadar tidak ada seorang pun yang akan tidur diatas laptop yang hangat. tentunya kebiasaan ini hanya akan dilakukan oleh makhluk yg lebih kecil dan salah satunya kucing. disitu disebutkan bahwa nama makhluk tersebut adalah satoshi.

dan ketika saya menjawab satoshi ternyata benar.

**Q11 : What is the primary C2 domain used for beacon check-ins?**

Baiklah disini kita disini kita diminta untuk mencari primary c2 domain untuk check-in beacon. saya menyadari tentunya domain menjadi salah satu infrastructure penting dalam penggunaan c2 domain. maka saya mencoba untuk menginvestigasi segment infrastructure. disini saya menemukan thread **C2 Infrastructure: Domain Registration & SSL.** Tentunya dari thread ini kita bisa mengetahui bahwa petunjuk tentang domain yang digunakan dalam kasus ini.

![](https://cdn-images-1.medium.com/max/800/1*EgmAQyyBkgpLuuClcVq_dw.png)

disini bellringer menuliskan bahwa terdapat beberapa domain yang digunakan. tetapi untuk domain primary yang kita cari terdapat disini yaitu health-status-rs[.]com. dengan sedikit modifikasi pada domain sehingga kita mendapatkan jawabannya health-status-rs.com

ketika saya menjawab ini, jawabannya benar.

**Q12 : In which city is the VPS server hosting the C2 panel?**

Selanjutnya kita diminta untuk menemukan di kota mana VPS server hosting C2 panel berada. kembali lagi, saya menduga ini berada di segment infrastructure karena berhubungan dengan infrastructure yang digunakan. saya mencurigai thread yang berjudul **C2 Infrastructure: Domain Registration & SSL** Karena disini kita bisa mengetahui lebih jauh tentang infrastructure yang ada. dan saya menemukan

![](https://cdn-images-1.medium.com/max/800/1*cHpptNt5X6PUWk8qv7og5Q.png)

disini kita bisa melihat pada pesan “Ensure the VPS provider in Kragujevac is paid through the end of January.” yang memberitahu bahwa vps provider terletak pada kota Kragujevac.

ketika saya jawab ternyata jawabannya benar.

**Q13 : On what date did the C2 listeners go live?**

Pada challenge ini, kita disuruh untuk mencari tahu kapan C2 listener mulai live. Sejujurnya awalnya saya mencari di segmen infrastructure, tetapi saya tidak menemukan apapun yang berhubungan dengan c2 listener yang mulai aktif. Kemudian saya beralih ke segmen Malware and stuff dan menemukan thread yg menarik yaitu **C2 Infrastructure Status: GREEN** yang kemungkinan di dalamnya terdapat data tentang C2.

![](https://cdn-images-1.medium.com/max/800/1*RpnDdcxzBErvbEUGARGN0g.png)

setelah menganalisis isi thread tersebut saya menemukan pesan disini bahwa listener sudah hidup (live). saya pun melihat tanggal dibuatnya pesan tersebut yaitu **12 November 2025**. saya menyadari bahwa ada kemungkinan tanggal ia mengumumkan listener live merupakan tanggal dimana listener baru saja live.

maka saya mencoba jawaban tersebut dan ternyata benar.

**Q14 : In which city is the document forger located?**

Selanjutnya kita diminta untuk menemukan dimana lokasi kota dokument di palsukan. tentunya hal ini akan berhubungan dengan thread physical Logistic. Langsung saja saya mencoba untuk mencari petunjuk terutama pada thread **Forged Manifests: “Miracles of Winter” Exhibition.**

![](https://cdn-images-1.medium.com/max/800/1*E95U3tU3lDyhDb13_OruIg.png)

  

disini saya menemukan pesan yang mengandung petunjuk. terutama terletak pada paragraf akhir. “The bucharest forger has outdone himself” yang artinya pemalsu bucharest telah melampau dirinya sendiri. saya baru sadar bahwa bucharest itu nama kota.

ketika saya menjawab **bucharest** ternyata jawabannya benar.

**Q15 : What shell company was used as a backup cover story?**

Disini kita diminta untuk mencari tahu perusahaan palsu yang bisa dipakai sebagai backup ketika plan pameran tidak berjalan dengan baik. Yaitu menggunakan rencana sebagai kargo perbaikan lokal.

![](https://cdn-images-1.medium.com/max/800/1*c6tt0vsttiLEBZUeWF9gDQ.png)

disini ledger telah memiliki invoice dari shell company yang disiapkan untuk rencana backup selanjutnya.

![](https://cdn-images-1.medium.com/max/800/1*TeYQJPj5TxFhTL5GFjsJhw.png)

kita bisa mendapatkan disini invoice alternatif didapatkan dari perusahaan Danube Event Solutions Ltd. nah disini saya telah mencurigai bahwa ini adalah perusahaan backup yang dimaksud oleh challenge.

dan ketika saya coba ternyata benar

**Q16 : What is the filename of the wipe script used to destroy evidence?**

Pada challenge ke 16 ini, kita diminta untuk mencari wipe script yang berfungsi untuk menghapus bukti yang ada. Tentunya kita akan menuju segmen infrastructure karena disitu tertulis ada digital wipe protocol

![](https://cdn-images-1.medium.com/max/800/1*iE0Mn5T77HQSqDac4vg7sw.png)

  

Selanjutnya kita cek thread yang ada dan saya memerhatikan thread **server wipe protocol.** Pada thread ini kita bisa mencari tahu apa filename script yang dibuat. ternyata saya menemukan pada pesan yang dibuat oleh bellringer bahwa nama script tersebut adalah **burn_cycle.sh**

![](https://cdn-images-1.medium.com/max/800/1*jWAIZGmSHCBskoujnvNqjg.png)

script ini berfungsi untuk melakukan overwrite cryptographic header untuk melakukan wipe bukti, script ini juga mengisi ram dengan noise random dan kemudaian mentrigger kernel panic serta melakukan hard shutdown.

**Q17 : What is the name of the escape vessel?**

selanjutnya kita diminta untuk mencari apa nama kapal pelarian yang akan digunakan. dalam hal ini tentunya akan masuk ke dalam segmen operasi karena akan berisi prosedur tentang bagaimana DRIVER_BUD’S akan kabur melalui jalur laut. Disini saya menemukan thread mencurigakan yang ada pada segmen ini yang ditulis oleh curator berjudul Exfiltration Protocol: Staging South Exit Route

![](https://cdn-images-1.medium.com/max/800/1*vg7SyJ62NBuH79WgGrPO0Q.png)

dari pesan ini sebenarnya kita bisa menganalisis bahwa nama kapal yang ditumpangi adalah **adriatic wind**.

ketika saya menjawab hal tersebut, ternyata benar.

**Q18 : What is the captain’s name?**

sebenarnya masih berhubungan dengan soal sebelumnya, kita diminta untuk mencari nama kapten yang bertugas. dengan menganalisis file sebelumnya dapat diketahui nama sang captain adalah **stavros**

![](https://cdn-images-1.medium.com/max/800/1*vg7SyJ62NBuH79WgGrPO0Q.png)

dan ketika saya jawab ternyata benar.

**Q19 : What are the GPS coordinates of the emergency extraction point for Driver_BUD?**

Kita disini disuruh untuk menemukan gps coordinates untuk Driver_BUD. nah perhatikan pesan sebelumnya lebih tepatnya pada bagian ini.

![](https://cdn-images-1.medium.com/max/800/1*MzCftR831H-L61QhWtDkRQ.png)

curator telah merancang jalur pelarian untuk Driver_BUD kabur, rutenya dari yunani menuju makedonia utara. menyebrang melalui lautan dengan captain kapal yang telah dibayar dan akan bertemu di suatu tempat dengan kode **twitchy develop hulk.** 3 kata ini merupakan kode yang harus dipecahkan terlebih dahulu agar mendapatkan koordinat gps yang telah diatur.

disini saya menggunakan [https://what3words.com/](https://what3words.com/) untuk memecahkan kode dari 3 kata tersebut dan saya mendapatkan ini

![](https://cdn-images-1.medium.com/max/800/1*4A1gHD2yqX3T5pvZi97JJQ.png)

ini merupakan lokasi koordinat yang kita cari untuk Driver_BUD, tetapi belum sampai disitu saja karena kita memerlukan untuk mencari tahu letak tepat koordinat lokasi tersebut. saya menggunakan google map untuk mengetahui koordinat lokasi tersebut

![](https://cdn-images-1.medium.com/max/800/1*fPAQMWkr2J8jU48sXM38MQ.png)

bagian yang saya tandai merupakan koordinat yang dituju oleh Driver_BUD yaitu **37.936489, 23.68644**.

dan ketika saya submit jawabannya benar.

**Q20 : What are the GPS coordinates of the farmhouse hideout?**

Disini kita diminta untuk mencari apa koordinat dari persembuyian tempat pertanian. seperti di awal tadi, tempat persembunyian ini disebutkan pada segment physical logistic pada thread staging point “south” : site access and concealment.

![](https://cdn-images-1.medium.com/max/800/1*aoNnoNQacTqN1qLzY_BEwg.png)

disni kita bisa mendapatkan bahwa gps coordinat untuk lokasi pesembunyiannya adalah **43.84947615369828, 20.92715775614975**

ketika saya submit jawaban saya ternyata benar.

> baiklah cukup sekian writeup dari saya, happy learning semuanyaa
