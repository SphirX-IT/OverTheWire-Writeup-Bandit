### Level 25 → Level 26

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user `bandit26` is not **`/bin/bash`**, but something else. Find out what it is, how it works and how to break out of it.

> NOTE: if you’re a Windows user and typically use `Powershell` to `ssh` into bandit: `Powershell` is known to cause issues with the intended solution to this level. You should use command prompt instead.
> 
- Writeup
    
    User **bandit26** memiliki shell yang tidak standar; ia menjalankan script `more` dan langsung melakukan logout. Kita harus melakukan *breakout* melalui Vim dengan memanfaatkan ukuran jendela terminal yang sempit agar `more` tidak langsung selesai.
    
    ```bash
    # Perkecil jendela terminal secara vertikal hingga sangat pendek (3-5 baris)
    
    # Login ke bandit26 menggunakan SSH key
    ssh -i bandit26.sshkey bandit26@localhost -p 2220
    
    # Saat teks tertahan oleh 'more', tekan tombol 'v'
    # (Anda akan masuk ke dalam Vim)
    
    # Di dalam Vim, ganti shell ke bash dan panggil shell-nya
    :set shell=/bin/bash
    :shell
    
    # Baca password bandit26
    cat /etc/bandit_pass/bandit26
    ```
    
    - **`v`**: Shortcut di dalam `more` untuk membuka editor teks Vim.
    - **`:set shell=/bin/bash`**: Perintah Vim untuk menentukan program shell yang akan digunakan.
    - **`:shell`**: Perintah Vim untuk keluar sementara ke shell interaktif yang telah ditentukan.

---

### Level 26 → Level 27

Good job getting a shell! Now hurry and grab the password for bandit27!

- Writeup
    
    Kita perlu membaca file password milik **bandit27**. Karena tidak memiliki izin akses langsung, kita menggunakan binary SetUID yang tersedia di direktori home.
    
    ```bash
    # 1. Pastikan Anda masih berada di shell bandit26 hasil langkah sebelumnya
    
    # 2. Cek file executable dengan izin khusus
    ls -l
    
    # 3. Jalankan perintah cat melalui wrapper bandit27-do
    ./bandit27-do cat /etc/bandit_pass/bandit27
    ```
    
    - **`ls -l`**: Melihat detail izin file; perhatikan tanda `s` pada `bandit27-do` yang menandakan SetUID.
    - **`./bandit27-do`**: Menjalankan perintah yang mengikutinya dengan hak akses user `bandit27`.
    - **`cat /etc/bandit_pass/bandit27`**: Perintah untuk membaca file password target.