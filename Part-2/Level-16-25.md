# Writeup OverTheWire : Bandit Part 2

## Level 16 - Level 17

### Level Goal

The credentials for the next level can be retrieved by submitting the password of the current level to **a port on `localhost` in the range `31000` to `32000`**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

**Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.**

### Commands you may need to solve this level

`ssh, telnet, nc, ncat, socat, openssl, s_client, nmap, netstat, ss`

- Writeup
    
    Tujuan dari level ini adalah untuk mengirimkan password dari level sebelumnya (level 16) ke sebuah port SSL/TLS pada `localhost`. Port yang digunakan berada dalam rentang `31000` hingga `32000`. Kita perlu mencari port mana yang terbuka dan melayani layanan SSL, lalu mengirimkan password untuk mendapatkan private key SSH level 17.
    
    ```bash
    # Melakukan scanning port untuk menemukan layanan SSL yang memberikan respon
    nmap -p 31000-32000 localhost
    
    # Menghubungkan ke port yang tepat menggunakan SSL dan mengirim password level 16
    openssl s_client -connect localhost:30001
    ```
    
    - **`nmap -p 31000-32000 localhost`**: Digunakan untuk memindai rentang port guna melihat layanan mana yang aktif. Dalam level ini, Anda mencari port yang menjalankan layanan SSL (biasanya akan muncul keterangan *echo* atau layanan yang merespon input).
    - **`openssl s_client -connect localhost:30001`**: Menginisialisasi koneksi terenkripsi SSL/TLS ke host dan port spesifik. Setelah terkoneksi, Anda dapat memasukkan password level 16 untuk menerima RSA Private Key.

## Level 17 - Level 18

### Level Goal

There are 2 files in the `homedirectory`: **`passwords.old` and `passwords.new`**. The password for the next level is in **`passwords.new`** and is the only line that has been changed between **`passwords.old` and `passwords.new`**

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**

### Commands you may need to solve this level

`cat, grep, ls, diff`

- Writeup
    
    Tujuan dari level ini adalah membandingkan dua file konfigurasi (`passwords.old` dan `passwords.new`) untuk menemukan satu-satunya baris yang berubah. Password untuk level berikutnya adalah baris yang berbeda tersebut.
    
    ```bash
    # Membandingkan dua file dan mencari perbedaan unik
    diff passwords.old passwords.new
    
    # Cara alternatif menggunakan grep dengan file pembanding
    grep -v -f passwords.old passwords.new
    ```
    
    - **`diff passwords.old passwords.new`**: Membandingkan konten kedua file baris demi baris dan menampilkan perbedaannya. Simbol `>` menunjukkan baris yang ada di file baru namun tidak ada di file lama.
    - **`grep -v -f passwords.old passwords.new`**: Mencari baris di `passwords.new` yang **tidak** ada (`v`) di dalam daftar pola yang diambil dari file `passwords.old` (`f`).

## Level 18 - Level 19

### Level Goal

The password for the next level is stored in a file **`readme`** in the `homedirectory`. Unfortunately, someone has modified `**.bashrc**` to log you out when you log in with SSH.

### Commands you may need to solve this level

`ssh, ls, cat`

- Writeup
    
    Kata sandi untuk level berikutnya disimpan dalam file bernama **`readme`** di direktori home. Kendalanya adalah file `.bashrc` telah dimodifikasi untuk memutus koneksi (logout) secara otomatis saat login.
    
    ```bash
    # Mengeksekusi perintah cat secara remote tanpa masuk ke interactive shell
    ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat readme'
    
    # Alternatif: Memaksa penggunaan shell lain untuk bypass .bashrc
    # ssh bandit18@bandit.labs.overthewire.org -p 2220 /bin/sh
    ```
    
    - **`ssh [user]@[host] -p [port] 'cat readme'`**: Memerintahkan SSH untuk menjalankan `cat readme` segera setelah login berhasil dan mengirimkan hasilnya kembali ke layar kita, sehingga perintah `exit` di `.bashrc` tidak sempat memutus sesi kita sebelum data terbaca.
    - **`cat readme`**: Menampilkan isi file yang berisi password.
    - **`/bin/sh`**: Menjalankan shell dasar yang berbeda dari Bash untuk menghindari eksekusi konfigurasi `.bashrc`.

## Level 19 - Level 20

### Level Goal

To gain access to the next level, you should use the `setuid binary` in the `homedirectory`. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (`/etc/bandit_pass`), after you have used the `setuid binary`.

- Writeup
    
    Password untuk level berikutnya disimpan di `/etc/bandit_pass/bandit20`. File ini hanya bisa dibaca oleh user `bandit20`. Namun, terdapat sebuah file binary setuid bernama `bandit20-do` di direktori home yang memungkinkan kita menjalankan perintah dengan hak akses pemiliknya (bandit20).
    
    ```bash
    # Melihat daftar file untuk menemukan binary setuid
    ls -l
    
    # Menjalankan perintah cat melalui binary setuid bandit20-do
    ./bandit20-do cat /etc/bandit_pass/bandit20
    ```
    
    - **`./bandit20-do`**: Mengeksekusi file binary yang memiliki bit **setuid**. Program ini akan menjalankan argumen perintah yang diberikan setelahnya dengan hak akses user `bandit20`.
    - **`cat /etc/bandit_pass/bandit20`**: Perintah yang dikirimkan sebagai argumen ke `bandit20-do` untuk membaca file password yang sebelumnya tidak bisa kita akses sebagai `bandit19`.

## Level 20 - Level 21

### Level Goal

There is a `setuid binary` in the `homedirectory` that does the following: it makes a connection to `localhos`t on the `port` you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

**NOTE:** Try connecting to your own network daemon to see if it works as you think

### Commands you may need to solve this level

`ssh, nc, cat, bash, screen, tmux, Unix ‘job control’ (bg, fg, jobs, &, CTRL-Z, …)`

- Writeup
    
    Level ini mengajarkan konsep **Network Sockets** dan komunikasi antar-proses. Kita harus membuat sebuah *listener* (server mini) yang akan menerima koneksi dari program setuid `./suconnect`. Program tersebut akan mengirimkan password level saat ini, dan jika kita mengirimkan balik password yang sama, ia akan memberikan password level berikutnya.
    
    ```bash
    # Cek file yang ada di direktori home
    ls -l
    
    # Buka port listener di background dan siapkan password level 20 untuk dikirim
    # Ganti [PASSWORD_LEVEL_20] dengan password asli Anda
    echo "VxCazJaVykH6Lv6yueYv17Zb02uS2Scf" | nc -l -p 1234 &
    
    # Jalankan program suconnect untuk menghubungkan ke port yang baru kita buka
    ./suconnect 1234
    ```
    
    - **`nc -l -p 1234`**: Menggunakan **Netcat** untuk mendengarkan (*listen*) pada port 1234.
    - **`&`**: Menjalankan perintah di *background* sehingga kita bisa mengetikkan perintah selanjutnya tanpa harus menutup listener tersebut.
    - **`./suconnect 1234`**: Menjalankan binary yang akan mencoba melakukan koneksi ke localhost pada port 1234. Program ini akan mengirimkan password bandit20 ke port tersebut dan menunggu balasan yang sama.
    - **`echo "[pass]" | nc ...`**: Mengirimkan string password secara otomatis segera setelah `./suconnect` melakukan koneksi ke listener kita.