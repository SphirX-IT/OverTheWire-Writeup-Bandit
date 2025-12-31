# OverTheWire-Writeup-Bandit
Technical writeup for OverTheWire Bandit levels 0-25. Prepared for LKS Cyber Security preparation.


## Command Level 0

`ssh`

### Level 0

The goal of this level is for you to log into the game using SSH. The host to which you need to connect is **bandit.labs.overthewire.org**, on port **2220**. The username is **bandit0** and the password is **bandit0**. Once logged in, go to the Level 1 page to find out how to beat Level 1.

- Writeup
    
    Gunakan ssh untuk masuk ke dalam game dengan format :
    
    `ssh user@host -p port`
    
    ```bash
    ssh bandit0@bandit.labs.overthewire.org -p 2220
    ```
    
    - **`-p`** : Opsi (*flag*) yang digunakan untuk menentukan nomor **port** tertentu.
    - **`ssh`**: Perintah utama untuk melakukan koneksi jarak jauh (*remote*) secara aman ke komputer atau server lain melalui jaringan OpenSSH

## Command Level 1 - 6

`ls, cd, cat, file, du, find`

### Level 0 → Level 1

The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

- Writeup
    
    Password level 0 ada di file bernama `readme`. Gunakan perintah `cat` untuk melihat seluruh isi file.
    
    ```bash
    cat readme
    ```
    
    - `cat` : Menampilkan seluruh isi file ke layar terminal.

### Level 1 → Level 2

The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

- Writeup
    
    Password level 1 ada di file bernama `-`. Di terminal, simbol `-` biasanya digunakan untuk argumen perintah. Untuk membacanya sebagai nama file, kita gunakan path `./`.
    
    ```bash
    cat ./-
    ```
    
    - `cat` : Menampilkan seluruh isi file ke layar terminal.
    - `./` : Menentukan lokasi file di direktori saat ini (current directory).

### Level 2 → Level 3

The password for the next level is stored in a file called **-** located in the home directory.

- Writeup
    
    File bernama `--spaces in this filename--` tidak bisa dibaca begitu saja karena terminal akan menganggap spasi sebagai pemisah antar file yang berbeda. Gunakan *backslash* (`\`) sebelum spasi agar terminal tidak menganggapnya sebagai beberapa argumen yang berbeda.
    
    ```bash
    cat ./"spaces in this filename"
    ```
    
    **`" "`** : Tanda kutip digunakan agar sistem membaca teks di dalamnya sebagai satu string tunggal.
    

### Level 3 → Level 4

The password for the next level is stored in a hidden file in the **inhere** directory.

- Writeup
    
    Di Linux, file yang diawali dengan titik `.` tidak akan muncul pada perintah `ls` biasa. Kita perlu masuk ke direktori dan menambahkan opsi `-a` (all) untuk melihatnya.
    
    ```bash
    cd inhere
    ls -a
    cat ...Hiding-From-You
    ```
    
    - **`cd`**: Change Directory, untuk masuk ke dalam folder tertentu.
    - **`ls -a`**: Menampilkan semua file, termasuk file tersembunyi (dotfiles).

### Level 4 → Level 5

The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.

- Writeup
    
    Di folder `inhere` terdapat banyak file biner. Kita harus mencari file yang tipenya *human-readable* menggunakan perintah `file` untuk menemukan file teks ASCII.
    
    ```bash
    file inhere/*
    cat inhere/-file07
    ```
    
    - **`file`**: Mengidentifikasi jenis data di dalam file (teks, biner, gambar, dll).
    - `*` : Wildcard yang berarti "semua file" di dalam direktori tersebut.

### Level 5 → Level 6

The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:
human-readable, 1033 bytes in size, not executable

- Writeup
    
    Kita perlu mencari file berdasarkan ukuran (bytes) dan kriteria tertentu. Perintah `find` sangat efisien untuk mencari file dengan ukuran tepat 1033 bytes.
    
    ```bash
    cd inhere
    find . -type f -size 1033c ! -executable
    ```
    
    - **`find`**: Mencari file/folder berdasarkan kriteria tertentu.
    - **`type f`**: Memberitahu sistem untuk hanya mencari file (bukan folder).
    - **`size 1033c`**: Mencari file dengan ukuran spesifik (c = bytes).
    - **`! -executable`**: Tanda seru `!` berarti "bukan", jadi mencari file yang tidak bisa dieksekusi.

### Level 6 → Level 7

The password for the next level is stored **somewhere on the server** and has all of the following properties:
owned by user bandit7, owned by group bandit6, 33 bytes in size

- Writeup
    
    Pencarian dilakukan di seluruh sistem (`/`) berdasarkan user dan group pemilik file. Kita membuang pesan error akses ke `/dev/null`
    
    ```bash
    find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
    ```
    
    - **`/`**: Lokasi pencarian dimulai dari root (seluruh sistem).
    - **`user`**: Memfilter file berdasarkan pemilik pengguna tertentu.
    - **`group`**: Memfilter file berdasarkan pemilik grup tertentu.
    - **`2>/dev/null`**: Mengalihkan pesan error ke "sampah" agar tidak mengganggu output.

## Command Level 7 - 11

`grep, sort, uniq, strings, base64, tr`

### Level 7 → Level 8

The password for the next level is stored in the file **data.txt** next to the word **millionth**

- Writeup
    
    Untuk mencari satu baris spesifik di antara ribuan baris di dalam file `data.txt`, kita gunakan `grep` dengan kata kunci "millionth".
    
    ```bash
    grep millionth data.txt
    ```
    
    - **`grep`**: Mencari dan menampilkan baris yang mengandung pola teks tertentu.

### Level 8 → Level 9

The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once.

- Writeup
    
    Password adalah satu-satunya baris yang tidak duplikat. `uniq` butuh data yang terurut agar bisa bekerja, maka kita gunakan pipe (`|`) untuk menghubungkan `sort` dan `uniq`.
    
    ```bash
    sort data.txt | uniq -u
    ```
    
    - **`sort`**: Mengurutkan isi file secara alfabetis atau numerik.
    - **`uniq -u`**: Menampilkan hanya baris yang benar-benar unik (tidak ada duplikat).

### Level 9 → Level 10

The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

- Writeup
    
    File biner tidak bisa dibuka dengan `cat` secara bersih. Perintah `strings` akan menyaring karakter biner dan hanya mengambil teks yang bisa dibaca.
    
    ```bash
    strings data.txt | grep "="
    ```
    
    • **`strings`**: Menemukan dan mencetak string teks yang bisa dibaca dalam file biner.
    

### Level 10 → Level 11

The password for the next level is stored in the file **data.txt**, which contains base64 encoded data.

- Writeup
    
    Base64 adalah skema pengkodean teks. Untuk mendapatkan password asli, kita harus melakukan proses *decoding*.
    
    ```bash
    base64 -d data.txt
    ```
    
    - **`base64`**: Perintah untuk memproses data dalam format pengkodean Base64.
    - **`-d`**: Decode, menerjemahkan kembali kode Base64 menjadi teks asli.

### Level 11 → Level 12

The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.

- Writeup
    
    Password untuk level ini tersimpan di dalam file `data.txt` dan telah dienkripsi menggunakan sandi **ROT13** (huruf digeser sejauh 13 posisi dalam alfabet). Untuk membacanya, kita perlu memutar kembali karakter tersebut menggunakan perintah `tr`.
    
    ```bash
    cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
    ```
    
    - **`cat`**: Menampilkan isi file `data.txt` ke layar.
    - **`|` (Pipe)**: Menghubungkan output dari perintah `cat` untuk diproses oleh perintah `tr`.
    - **`tr`**: singkatan dari *translate*, digunakan untuk mengubah atau mengganti karakter tertentu.
    - **`'A-Za-z' 'N-ZA-Mn-za-m'`**: Pola instruksi untuk menggeser setiap huruf sebanyak 13 langkah (A menjadi N, B menjadi O, dst) untuk memecahkan sandi ROT13.

## Command Level 12

`tar, gzip, bzip2, xxd, mkdir, cp, mv, file`

### Level 12 → Level 13

The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command `mktemp -d`. Then copy the datafile using cp, and rename it using mv (read the manpages!)