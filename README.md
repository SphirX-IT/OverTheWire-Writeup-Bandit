# 🚩 OverTheWire Bandit Writeups — [SphirX-IT]

![Linux](https://img.shields.io/badge/OS-Linux-orange?logo=linux)
![Security](https://img.shields.io/badge/Domain-CyberSecurity-red)
![Level](https://img.shields.io/badge/Progress-34%20%2F%2034-brightgreen)

Dokumentasi lengkap penyelesaian tantangan **Bandit** dari [OverTheWire](https://overthewire.org/wargames/bandit/). Repositori ini dirancang untuk menunjukkan pemahaman mendalam tentang administrasi sistem Linux, keamanan jaringan, dan teknik investigasi data.

---

## 🎯 Project Objectives
Tujuan dari pengerjaan tantangan ini adalah untuk menguasai:
* **Linux CLI Proficiency**: Manipulasi file, pengolahan teks, dan navigasi sistem tingkat lanjut.
* **Network Security**: Komunikasi aman menggunakan SSH, SSL/TLS, dan analisis port.
* **Cryptography & Encoding**: Identifikasi data terenkripsi dan pemecahan hashing sederhana.
* **Git Forensics**: Investigasi metadata, histori komit, dan manajemen branch pada repositori.

---

## 📂 Repository Structure

Setiap folder berisi langkah-langkah teknis, perintah yang digunakan, dan penjelasan konsep di balik setiap level.

| Folder | Level | Deskripsi Materi Utama |
| :--- | :--- | :--- |
| [**Level 00-19**](./Level-00-19/) | 0 → 19 | Dasar-dasar navigasi, filter teks (`grep`, `uniq`), dan file permission. |
| [**Level 20-33**](./Level-20-33/) | 20 → 33 | Automation (Brute-force), Shell Escape, dan Git Forensics. |

---

## 🛠️ Key Tools & Skills
* **Data Processing**: `grep`, `awk`, `sed`, `sort`, `uniq`, `strings`.
* **Network & Connect**: `ssh`, `nc`, `openssl`, `nmap`.
* **Version Control**: `git log`, `git checkout`, `git tag`, `git show`.
* **Scripting & Automation**: Bash scripting untuk otomatisasi pencarian password.

---

## 🗝️ Top Technical Takeaways
1. **Everything is a File**: Memahami bahwa di Linux, hampir semua hal (bahkan proses) direpresentasikan sebagai file.
2. **Metadata is Gold**: Pelajaran penting dari level Git bahwa data yang dihapus seringkali masih tertinggal di dalam metadata atau objek histori.
3. **Privilege Awareness**: Pentingnya memeriksa izin akses (SetUID) pada binary sistem untuk mencegah eskalasi hak akses.

---
*Created with ☕ and persistence by [SphirX-IT](https://github.com/SphirX-IT)*