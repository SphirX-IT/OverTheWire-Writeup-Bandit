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