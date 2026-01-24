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

---

### Level 27 → Level 28

There is a git repository at `ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

- Writeup
    
    Level ini mengajarkan cara berinteraksi dengan **Git** melalui protokol SSH untuk mengambil data dari repositori jarak jauh (remote repository).
    
    ```bash
    # Kloning repositori dari server OverTheWire ke komputer lokal
    git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
    
    # Masukkan password bandit27 saat diminta
    
    # Masuk ke direktori hasil kloning
    cd repo
    
    # Lihat daftar file untuk menemukan file password
    ls
    
    # Baca isi file tersebut (misalnya file README)
    cat README
    ```
    
    - **`git clone [URL]`**: Perintah untuk menyalin seluruh isi repositori Git dari server remote ke komputer Anda. Format URL `ssh://user@host:port/path` digunakan karena server Bandit menggunakan port non-standar (2220).
    - **`cd repo`**: Berpindah ke folder `repo` yang baru saja dibuat oleh proses `git clone`.
    - **`cat README`**: Membaca file di dalam repositori tersebut yang biasanya berisi informasi penting atau password untuk level selanjutnya.

---

### Level 28 → Level 29

There is a git repository at `ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo` via the port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

- Writeup
    
    Tantangan ini menunjukkan bahwa dalam sistem kontrol versi seperti Git, menghapus informasi sensitif hanya dengan membuat commit baru tidaklah cukup. Informasi tersebut akan tetap tersimpan secara permanen dalam riwayat (history) repositori selama database `.git` masih ada.
    
    ```bash
    # 1Buat direktori kerja di lokal agar rapi
    mkdir -p ~/otw/bandit28
    cd ~/otw/bandit28
    
    # Clone repositori dari server OverTheWire
    # Gunakan password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
    git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
    
    # Masuk ke folder repositori
    cd repo
    
    # Periksa file README.md (Versi saat ini)
    # Hasilnya: password berisi "xxxxxxxxxx"
    cat README.md
    
    # Bongkar riwayat perubahan untuk melihat data yang dihapus
    git log -p
    
    # Analisis output untuk mencari data yang dihapus
    # Pada commit "fix info leak", perhatikan baris yang diawali tanda '-'
    # Password ditemukan di sana sebelum diubah menjadi 'xxxxxxxxxx'
    ```
    
    - **`git log -p`**: Perintah ini menampilkan log riwayat commit sekaligus perbedaan (*diff*) teks yang terjadi pada setiap commit. Tanda  (biasanya merah) menunjukkan baris yang dihapus, dan tanda `+` (biasanya hijau) menunjukkan baris yang ditambahkan.
    - **`HEAD -> master`**: Menunjukkan posisi Anda saat ini berada di cabang utama (master) pada commit terbaru di mana password sudah disensor.
    - **`HEAD^` (Commit d0cf2ab...)**: Merupakan commit sebelumnya di mana password baru saja ditambahkan (dari `<TBD>` menjadi password asli) sebelum akhirnya dihapus kembali di commit terakhir.

---

### Level 29 → Level 30

There is a git repository at `ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo` via the port `2220`. The password for the user `bandit29-git` is the same as for the user `bandit29`.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

- Writeup
    
    Pada level ini, kita dihadapkan dengan sebuah repositori Git. Pesan di cabang utama (`master`) menyatakan bahwa kata sandi tidak ada di sana. Kita harus mengeksplorasi cabang-cabang lain (*branches*) yang ada di repositori untuk menemukan informasi yang disembunyikan.
    
    ```bash
    # Masuk ke server Bandit 29
    ssh bandit29@bandit.labs.overthewire.org -p 2220
    
    # Buat direktori kerja sementara dan masuk ke sana
    mkdir /tmp/myrepo_level29
    cd /tmp/myrepo_level29
    
    # Clone repositori git yang diberikan
    git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
    
    # Masuk ke direktori repositori
    cd repo
    
    # Lihat semua cabang (lokal dan remote)
    git branch -a
    
    # Berpindah ke cabang 'dev' yang mencurigakan
    git checkout dev
    
    # Baca file README.md untuk mendapatkan password
    cat README.md
    ```
    
    - **`git clone`**: Menyalin seluruh repositori dari server ke direktori lokal kita agar bisa diperiksa.
    - **`git branch -a`**: Menampilkan semua cabang yang tersedia. Flag `a` (all) sangat penting karena secara default Git hanya menunjukkan cabang aktif, sementara password berada di cabang *remote* (`remotes/origin/dev`).
    - **`git checkout dev`**: Mengalihkan *working directory* kita ke versi kode yang ada di cabang `dev`. Git akan secara otomatis mengambil data dari `remotes/origin/dev`.
    - **`cat README.md`**: Membaca isi file setelah berada di cabang yang tepat. Di cabang ini, isi filenya berbeda dengan cabang `master`.