
# picoCTF - Forensic Challenge: Hidden In Plainsight

## 📝 Challenge Description

Tantangan ini memberikan sebuah file gambar JPG yang terlihat biasa saja, namun berisi informasi rahasia yang disembunyikan menggunakan teknik steganografi dan encoding berlapis.

## 🛠️ Tools Used

-   **ExifTool**: Analisis metadata file.
    
-   **Base64**: Mendecode string rahasia di dalam terminal Ubuntu.
    
-   **Steghide**: Mengekstrak data tersembunyi dari dalam gambar.
    
-   **Hex to ASCII (xxd)**: Mengubah string heksadesimal menjadi teks flag yang terbaca.
    

## 🚀 Step-by-Step Investigation
**1. Metadata Analisis**
Langkah awal dimulai dengan mengecek metadata file menggunakan `exiftool`. Ditemukan sebuah string mencurigakan pada kolom **Comment**:

Bash

```
exiftool img.jpg
# Output Comment: c3Rlc2hpZGU6Y0VGNVVuVnZHVzZiYjVE
```

![exiftool](/home/shandy/CyberSecurity_Learn_Journey/ctf/picoCTF/Forensic/HiddenInPlainSight_Easy)

