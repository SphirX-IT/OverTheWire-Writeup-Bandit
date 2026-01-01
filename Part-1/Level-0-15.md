# Writeup OverTheWire : Bandit Part 1

## Level 0

### Level Goal

The goal of this level is for you to log into the game using SSH. The host to which you need to connect is **`bandit.labs.overthewire.org`**, on port `2220`. The username is **`bandit0`** and the password is **`bandit0`**. Once logged in, go to the Level 1 page to find out how to beat Level 1.

### Commands you may need to solve this level

`ssh`

- Writeup
    
    Gunakan `ssh` untuk masuk ke dalam game.
    
    ```bash
    ssh [bandit0@bandit.labs.overthewire.org](mailto:bandit0@bandit.labs.overthewire.org) -p 2220
    ```
    
    - **`-p`** : Opsi (*flag*) yang digunakan untuk menentukan nomor **port** tertentu.
    - **`ssh`**: Perintah utama untuk melakukan koneksi jarak jauh (*remote*) secara aman ke komputer atau server lain melalui jaringan OpenSSH.

## Level 0 → Level 1

### Level Goal

The password for the next level is stored in a file called **`readme`** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

### Commands you may need to solve this level

`ls , cd , cat , file , du , find`

**TIP:** Create a file for notes and passwords on your local machine!

Passwords for levels are *not* saved automatically. If you do not save them yourself, you will need to start over from bandit0.

Passwords also occasionally change. It is recommended to take notes on how to solve each challenge. As levels get more challenging, detailed notes are useful to return to where you left off, reference for later problems, or help others after you’ve completed the challenge.

- Writeup
    
    Password level 0 ada di file bernama `readme`. Gunakan perintah `cat` untuk melihat seluruh isi file.
    
    ```bash
    cat readme
    ```
    
    - `cat` : Menampilkan seluruh isi file ke layar terminal.

## Level 1 → Level 2

### Level Goal

The password for the next level is stored in a file called **`-`** located in the home directory.

### Commands you may need to solve this level

`ls , cd , cat , file , du , find`

- Writeup
    
    Password level 1 ada di file bernama `-`. Di terminal, simbol `-` biasanya digunakan untuk argumen perintah. Untuk membacanya sebagai nama file, kita gunakan path `./`.
    
    ```bash
    cat ./-
    ```
    
    - `cat` : Menampilkan seluruh isi file ke layar terminal.
    - `./` : Menentukan lokasi file di direktori saat ini (current directory).

## Level 2 → Level 3

### Level Goal

The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory.

### Commands you may need to solve this level

`ls , cd , cat , file , du , find`

- Writeup
    
    File bernama `--spaces in this filename--` tidak bisa dibaca begitu saja karena terminal akan menganggap spasi sebagai pemisah antar file yang berbeda. Gunakan *backslash* (`\`) sebelum spasi agar terminal tidak menganggapnya sebagai beberapa argumen yang berbeda.
    
    ```bash
    cat ./"spaces in this filename"
    ```
    
    **`" "`** : Tanda kutip digunakan agar sistem membaca teks di dalamnya sebagai satu string tunggal.
    

## Level 3 → Level 4

### Level Goal

The password for the next level is stored in a hidden file in the **`inhere`** directory.

### Commands you may need to solve this level

`ls , cd , cat , file , du , find`

- Writeup
    
    Di Linux, file yang diawali dengan titik `.` tidak akan muncul pada perintah `ls` biasa. Kita perlu masuk ke direktori dan menambahkan opsi `-a` (all) untuk melihatnya.
    
    ```bash
    cd inhere
    ls -a
    cat ...Hiding-From-You
    ```
    
    - **`cd`**: Change Directory, untuk masuk ke dalam folder tertentu.
    - **`ls -a`**: Menampilkan semua file, termasuk file tersembunyi (dotfiles).

## Level 4 → Level 5

### Level Goal

The password for the next level is stored in the only human-readable file in the **`inhere`** directory. Tip: if your terminal is messed up, try the “reset” command.

### Commands you may need to solve this level

`ls , cd , cat , file , du , find`

- Writeup
    
    Di folder `inhere` terdapat banyak file biner. Kita harus mencari file yang tipenya *human-readable* menggunakan perintah `file` untuk menemukan file teks `ASCII`.
    
    ```bash
    file inhere/*
    cat inhere/-file07
    ```
    
    - **`file`**: Mengidentifikasi jenis data di dalam file (teks, biner, gambar, dll).
    - `*` : Wildcard yang berarti "semua file" di dalam direktori tersebut.

## Level 5 → Level 6

### Level Goal

The password for the next level is stored in a file somewhere under the **`inhere`** directory and has all of the following properties:

- `human-readable`
- `1033 bytes` in size
- `not executable`

### Commands you may need to solve this level

`ls , cd , cat , file , du , find`

- Writeup
    
    Kita perlu mencari file berdasarkan ukuran (bytes) dan kriteria tertentu. Perintah `find` sangat efisien untuk mencari file dengan ukuran tepat `1033 bytes`.
    
    ```bash
    cd inhere
    find . -type f -size 1033c ! -executable
    ```
    
    - **`find`**: Mencari file/folder berdasarkan kriteria tertentu.
    - **`type f`**: Memberitahu sistem untuk hanya mencari file (bukan folder).
    - **`size 1033c`**: Mencari file dengan ukuran spesifik (c = bytes).
    - **`! -executable`**: Tanda seru `!` berarti "bukan", jadi mencari file yang tidak bisa dieksekusi.

## Level 6 → Level 7

### Level Goal

The password for the next level is stored **somewhere on the server** and has all of the following properties:

- owned by user `bandit7`
- owned by group `bandit6`
- `33 bytes` in size

### Commands you may need to solve this level

`ls , cd , cat , file , du , find , grep`

- Writeup
    
    Pencarian dilakukan di seluruh sistem (`/`) berdasarkan user dan group pemilik file. Kita membuang pesan error akses ke `/dev/null`
    
    ```bash
    find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
    ```
    
    - **`/`**: Lokasi pencarian dimulai dari root (seluruh sistem).
    - **`user`**: Memfilter file berdasarkan pemilik pengguna tertentu.
    - **`group`**: Memfilter file berdasarkan pemilik grup tertentu.
    - **`2>/dev/null`**: Mengalihkan pesan error ke "sampah" agar tidak mengganggu output.

## Level 7 → Level 8

### Level Goal

The password for the next level is stored in the file **`data.txt`** next to the word **`millionth`.**

### Commands you may need to solve this level

`man, grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd`

- Writeup
    
    Untuk mencari satu baris spesifik di antara ribuan baris di dalam file `data.txt`, kita gunakan `grep` dengan kata kunci "millionth".
    
    ```bash
    grep millionth data.txt
    ```
    
    - **`grep`**: Mencari dan menampilkan baris yang mengandung pola teks tertentu.

## Level 8 → Level 9

### Level Goal

The password for the next level is stored in the file **`data.txt`** and is the only line of text that occurs only once.

### Commands you may need to solve this level

`grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd`

- Writeup
    
    Password adalah satu-satunya baris yang tidak duplikat. `uniq` butuh data yang terurut agar bisa bekerja, maka kita gunakan pipe (`|`) untuk menghubungkan `sort` dan `uniq`.
    
    ```bash
    sort data.txt | uniq -u
    ```
    
    - **`sort`**: Mengurutkan isi file secara alfabetis atau numerik.
    - **`uniq -u`**: Menampilkan hanya baris yang benar-benar unik (tidak ada duplikat).

## Level 9 → Level 10

### Level Goal

The password for the next level is stored in the file **`data.txt`** in one of the few human-readable strings, preceded by several ‘`=`’ characters.

### Commands you may need to solve this level

`grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd`

- Writeup
    
    File biner tidak bisa dibuka dengan `cat` secara bersih. Perintah `strings` akan menyaring karakter biner dan hanya mengambil teks yang bisa dibaca.
    
    ```bash
    strings data.txt | grep "="
    ```
    
    • **`strings`**: Menemukan dan mencetak string teks yang bisa dibaca dalam file biner.
    

## Level 10 → Level 11

### Level Goal

The password for the next level is stored in the file **`data.txt`**, which contains `base64` encoded data.

### Commands you may need to solve this level

`grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd`

- Writeup
    
    `Base64` adalah skema pengkodean teks. Untuk mendapatkan password asli, kita harus melakukan proses *decoding*.
    
    ```bash
    base64 -d data.txt
    ```
    
    - **`base64`**: Perintah untuk memproses data dalam format pengkodean Base64.
    - **`-d`**: Decode, menerjemahkan kembali kode Base64 menjadi teks asli.

## Level 11 → Level 12

### Level Goal

The password for the next level is stored in the file **`data.txt`**, where all lowercase `(a-z`) and uppercase (`A-Z`) letters have been rotated by `13` positions.

### Commands you may need to solve this level

`grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd`

- Writeup
    
    Password untuk level ini tersimpan di dalam file `data.txt` dan telah dienkripsi menggunakan sandi **`ROT13`** (huruf digeser sejauh 13 posisi dalam alfabet). Untuk membacanya, kita perlu memutar kembali karakter tersebut menggunakan perintah `tr`.
    
    ```bash
    cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
    ```
    
    - **`cat`**: Menampilkan isi file `data.txt` ke layar.
    - **`|` (Pipe)**: Menghubungkan output dari perintah `cat` untuk diproses oleh perintah `tr`.
    - **`tr`**: singkatan dari *translate*, digunakan untuk mengubah atau mengganti karakter tertentu.
    - **`'A-Za-z' 'N-ZA-Mn-za-m'`**: Pola instruksi untuk menggeser setiap huruf sebanyak 13 langkah (A menjadi N, B menjadi O, dst) untuk memecahkan sandi ROT13.

## Level 12 → Level 13

### Level Goal

The password for the next level is stored in the file **`data.txt`**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under `/tmp` in which you can work. Use `mkdir` with a hard to guess directory name. Or better, use the command “`mktemp -d`”. Then copy the datafile using `cp`, and rename it using `mv` (read the manpages!)

### Commands you may need to solve this level

`grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd, mkdir, cp, mv, file`

- Writeup
    
    Data harus dikembalikan dari format hexdump ke biner, lalu diekstrak berulang kali sesuai dengan jenis kompresinya (GZIP, BZIP2, atau TAR).
    
    Lakukan pengecekan tipe file secara iteratif menggunakan perintah `file`, lalu ekstrak sesuai formatnya hingga ditemukan file berformat ASCII text.
    
    ```bash
    xxd -r data.txt data_ori
    file data_ori
    gzip -d file.gz  # Jika tipe gzip
    bzip2 -d file    # Jika tipe bzip2
    tar -xf file     # Jika tipe tar
    ```
    
    - **`xxd -r`**: Mengubah format hexdump kembali menjadi file biner aslinya (*reverse*).
    - **`file`**: Mengidentifikasi jenis data atau kompresi di dalam file agar tidak salah langkah.
    - **`gzip -d` / `gunzip`**: Mengekstrak file yang terkompresi dalam format GZIP.
    - **`bzip2 -d`**: Mengekstrak file yang terkompresi dalam format BZIP2.
    - **`tar -xf`**: Mengekstrak file arsip atau kumpulan file dalam format TAR.

## Level 13 → Level 14

### Level Goal

The password for the next level is stored in **`/etc/bandit_pass/bandit14` and can only be read by user `bandit14`**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.

### Commands you may need to solve this level

`ssh, scp, umask, chmod, cat, nc, install`

- Writeup
    
    Kita perlu menggunakan kunci privat SSH (`sshkey.private`) untuk login sebagai user `bandit14` tanpa memerlukan password. Karena kebijakan keamanan SSH yang ketat, kunci tersebut harus memiliki izin akses privat agar bisa digunakan. Jika bekerja di lingkungan seperti WSL atau jika izin file asli tidak bisa diubah, **salin kunci ke direktori home (`~/`) terlebih dahulu** agar kita menjadi pemilik file dan bisa mengatur izinnya. Lakukan login menggunakan identitas kunci tersebut ke localhost, lalu baca password di direktori `bandit_pass`.
    
    ```bash
    # Salin kunci agar kita bisa mengubah izin filenya (khususnya user WSL)
    cp sshkey.private ~/sshkey.private
    cd ~
    
    # Mengatur izin file agar privat (hanya pemilik yang bisa baca)
    chmod 600 sshkey.private
    
    # Login ke bandit14 menggunakan kunci privat
    ssh -i sshkey.private bandit14@localhost -p 2220
    
    # Membaca password level selanjutnya setelah berhasil login
    cat /etc/bandit_pass/bandit14
    ```
    
    - **`cp`**: Menyalin file ke direktori home agar kepemilikan file berpindah ke user kita sehingga `chmod` bisa berfungsi.
    - **`ssh -i`**: Opsi *identity file* untuk melakukan login menggunakan kunci privat alih-alih password ketik.
    - **`chmod 600`**: Mengubah izin file sehingga hanya pemilik yang memiliki akses baca dan tulis (syarat wajib SSH agar kunci dianggap aman).
    - **`localhost`**: Menghubungkan ke mesin yang sama di mana kita sedang berada.
    - **`-p 2220`**: Menentukan port spesifik yang digunakan oleh server OverTheWire.
    - **`cat`**: Digunakan untuk menampilkan isi file password setelah mendapatkan hak akses sebagai bandit14.

## Level 14 → Level 15

### Level Goal

The password for the next level can be retrieved by submitting the password of the current level to **port `30000` on `localhost`**.

### Commands you may need to solve this level

`ssh, telnet, nc, openssl, s_client, nmap`

- Writeup
    
    Untuk mendapatkan password Level 15, kita perlu mengirimkan password Level 14 yang sedang kita gunakan saat ini ke layanan yang berjalan di port `30000` pada `localhost`. Karena layanan ini tidak menggunakan enkripsi (plain text), kita bisa menggunakan tool komunikasi jaringan sederhana untuk mengirimkan data tersebut dan menerima balasannya.
    
    ```bash
    # Membaca password level 14 yang tersimpan di sistem
    cat /etc/bandit_pass/bandit14
    
    # Menghubungkan ke port 30000 di localhost menggunakan netcat
    nc localhost 30000
    
    # Setelah terkoneksi, masukkan password yang didapat dari cat tadi
    # (Paste password dan tekan Enter)
    
    # Alternatif: Menggunakan teknik piping untuk penyelesaian satu baris
    cat /etc/bandit_pass/bandit14 | nc localhost 30000
    ```
    
    - **`cat /etc/bandit_pass/bandit14`**: Digunakan untuk menampilkan isi file password level saat ini. Di level ini, user bandit14 memiliki izin akses khusus untuk membaca file ini.
    - **`nc` (Netcat)**: Tool serbaguna untuk membaca dan menulis data di seluruh koneksi jaringan menggunakan protokol TCP atau UDP.
    - **`localhost`**: Nama host yang merujuk pada mesin yang sedang kita gunakan sendiri (loopback address).
    - **`30000`**: Nomor port spesifik di mana layanan (service) Level 15 sedang menunggu kiriman password.
    - **`|` (Pipe)**: Operator yang mengambil output dari perintah sebelah kiri (`cat`) dan mengirimkannya langsung sebagai input ke perintah sebelah kanan (`nc`).

## Level 15 → Level 16

### Level Goal

The password for the next level can be retrieved by submitting the password of the current level to **port `30001` on `localhost`** using SSL/TLS encryption.

**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

### Commands you may need to solve this level

`ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss`

- Writeup
    
    Untuk mendapatkan password Level 16, kita harus mengirimkan password Level 15 ke layanan yang berjalan di port `30001` pada `localhost`. Berbeda dengan level sebelumnya, layanan ini menggunakan enkripsi **SSL/TLS**, sehingga kita perlu menggunakan tool yang mampu menangani jabat tangan (*handshake*) kriptografi agar data dapat terkirim dengan aman.
    
    ```bash
    # Membaca password level 15 yang tersimpan di sistem
    cat /etc/bandit_pass/bandit15
    
    # Menghubungkan ke port SSL/TLS di localhost
    openssl s_client -connect localhost:30001
    
    # Setelah koneksi terbentuk dan muncul status "Verify return code: 0 (ok)",
    # masukkan password level 15 dan tekan Enter.
    
    # Alternatif: Menggunakan teknik piping untuk penyelesaian satu baris secara silent
    cat /etc/bandit_pass/bandit15 | openssl s_client -connect localhost:30001 -quiet
    ```
    
    - **`cat /etc/bandit_pass/bandit15`**: Perintah untuk menampilkan string password yang diperlukan untuk otentikasi ke level berikutnya.
    - **`openssl s_client -connect [host]:[port]`**: Rumus perintah untuk membuka koneksi klien menggunakan protokol SSL/TLS. Tool ini berfungsi untuk melakukan negosiasi keamanan sebelum data dikirim.
    - **`localhost:30001`**: Alamat tujuan (mesin sendiri) dan port spesifik yang menjalankan layanan terenkripsi.
    - **`quiet`**: Parameter untuk menyembunyikan informasi detail sertifikat SSL sehingga output lebih bersih dan hanya menampilkan respon utama dari server.