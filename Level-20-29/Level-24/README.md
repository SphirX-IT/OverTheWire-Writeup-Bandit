# 📁 Level 24 → 25: PIN Brute Force Automation

Halaman ini berisi dokumentasi dan skrip otomatisasi untuk menembus proteksi PIN 4-digit pada port 30002.

---

### Level 24 - Level 25
A daemon is listening on port `30002` and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.

You do not need to create new connections each time

- Writeup
    
    Pada level ini, kita diminta untuk melakukan brute force pada layanan yang berjalan di port `30002`. Layanan tersebut memerlukan password dari level saat ini (bandit24) dan sebuah PIN 4-digit (0000-9999) yang dipisahkan oleh spasi.
    
    ```bash
    # Membuat direktori kerja sementara di /tmp
    mkdir /tmp/work_dir && cd /tmp/work_dir

    # Menjalankan script bash yang sudah dibuat untuk melakukan brute force
    # Password Bandit 24 dikirim bersama urutan PIN 0000-9999
    chmod +x bruteforce25.sh
    ./bruteforce25.sh | nc localhost 30002

    # [Optional: Alternative solution using Python]
    # Menggunakan script Python untuk menangani logika string zfill(4)
    python3 bruteforce25.py | nc localhost 30002
    ```

    - **`​mkdir /tmp/... && cd /tmp/...`**: Membuat dan berpindah ke direktori sementara karena folder home Bandit bersifat read-only.
    - **`​chmod +x`**: Memberikan izin eksekusi pada file script agar dapat dijalankan oleh sistem.
    - **​`./bruteforce25.sh`**: Script Bash yang melakukan perulangan PIN menggunakan brace expansion {0000..9999}.
    - **`​python3 bruteforce25.py`**: Script Python yang menghasilkan output password dan PIN dengan format 4-digit tetap.
    - **`​nc localhost 30002`**: Mengirimkan seluruh aliran data (stream) dari script ke port tujuan dalam satu sesi koneksi.