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

---

### Level 30 → Level 31

There is a git repository at `ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo` via the port `2220`. The password for the user `bandit30-git` is the same as for the user `bandit30`.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

- Writeup
    
    Pada level ini, kita dihadapkan dengan sebuah repositori Git. Setelah memeriksa riwayat *commit* (`git log`) dan file yang ada, tidak ditemukan kata sandi. Ternyata, informasi penting disembunyikan di dalam fitur **Git Tags**, yang biasanya digunakan untuk menandai poin rilis atau versi penting.
    
    ```bash
    # Membuat direktori kerja sementara
    mkdir /tmp/my_work_30
    cd /tmp/my_work_30
    
    # Melakukan clone repositori (masukkan password bandit30 saat diminta)
    git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
    
    # Masuk ke direktori repo
    cd repo
    
    # Melihat daftar tag yang ada di repositori
    git tag
    
    # Melihat isi dari tag yang ditemukan
    git show secret
    ```
    
    - **`mkdir /tmp/...`**: Karena kita tidak memiliki izin menulis di direktori *home*, kita harus membuat folder di `/tmp` untuk menyimpan hasil *clone*.
    - **`git clone`**: Mengunduh seluruh riwayat dan file proyek dari server Git lokal ke mesin kita.
    - **`git tag`**: Menampilkan daftar semua label/tag yang telah dibuat di repositori ini. Tag sering kali luput dari pemeriksaan jika kita hanya melihat `git log`.
    - **`git show [tagname]`**: Perintah ini digunakan untuk mengekstrak detail dari objek Git. Jika tag tersebut adalah *annotated tag*, perintah ini akan menampilkan pesan yang disertakan pembuatnya, yang dalam kasus ini berisi kata sandi.