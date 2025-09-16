# WebshellFinder V3 - 

<p align="center">
  <img src="https://img.shields.io/badge/build-passing-brightgreen" />
  <img src="https://img.shields.io/badge/license-MIT-blue" />
  <img src="https://img.shields.io/badge/python-3.x-blue.svg" />
  <img src="https://img.shields.io/badge/platform-Windows%20%7C%20Linux%20%7C%20macOS%20%7C%20Termux-lightgrey" />
  <br/>
  <img src="https://img.shields.io/badge/contributions-welcome-orange" />
  <img src="https://img.shields.io/badge/issues-0%20open-blue" />
  <img src="https://img.shields.io/badge/security-checked-success" />
  <img src="https://img.shields.io/maintenance/yes/2025" />
</p>

---

![Preview](https://b.top4top.io/p_3546ev6m33.jpg)

---

## Deskripsi

**WebshellFinder** adalah tool Python untuk memindai daftar situs (domain) dan mencoba mengakses path/path potensial yang biasanya dipakai untuk webshell atau backdoor. Tool ini memeriksa setiap URL dari file path (`ShellPath.txt`) dan mencocokkannya dengan kata kunci yang ditentukan (`ShellKeword.txt`). Bila ditemukan indikasi (keyword) di dalam respons, URL akan disimpan ke `BackdorResult.txt`.

Tool ini berguna untuk penilaian surface reconnaissance dan audit keamanan internal — gunakan hanya pada sistem yang Anda miliki atau yang Anda punya izin tertulis untuk diuji.

---

## Fitur Utama

* Memeriksa list target dari file (satu domain per baris)
* Menguji banyak path (dari `ShellPath.txt`) pada tiap target
* Mencari kata kunci indikatif pada respons (dari `ShellKeword.txt`)
* Menyimpan hasil temuan ke `BackdorResult.txt`
* Support multithreading untuk mempercepat proses
* Rate limiting per domain (delay minimal antar-request per host)
* Output terminal berwarna (menggunakan `colorama`)
* Compatible: Termux, Linux, macOS, Windows

---

## Persyaratan

* Python 3.x
* Library Python:

  * `requests`
  * `colorama`

Instal library yang dibutuhkan:

```bash
pip install requests colorama
```

---

## Instalasi & Cara Menjalankan

### Termux / Android

```bash
pkg update -y
pkg install git python -y
pip install requests colorama
git clone https://github.com/LikeEx01/ShellFinder.git
cd ShellFinder 
python3 ShellFinder.py
```

### Windows (CMD / PowerShell)

1. Pastikan Python 3.x terpasang: [https://www.python.org/downloads/](https://www.python.org/downloads/)
2. Install dependencies & jalankan:

```bash
pip install requests colorama
git clone https://github.com/LikeEx01/ShellFinder.git
cd ShellFinder
python ShellFinder.py
```

### Linux / Ubuntu / Debian

```bash
sudo apt update && sudo apt install git python3 python3-pip -y
pip3 install requests colorama
git clone https://github.com/LikeEx01/ShellFinder.git
cd ShellFinder
python3 ShellFinder.py
```

---

## Struktur File yang Dibutuhkan

* `targets.txt` — daftar target
* `ShellPath.txt` — daftar path untuk diuji 
* `ShellKeword.txt` — daftar kata kunci bacdor
* `BackdorResult.txt` — file hasil yang otomatis dibuat real time

> Contoh `targets.txt`:
>
> ```
> example.com
> https://target-site.id
> http://insecure-site.org
> ```

---

## Penggunaan

1. Siapkan file-file `targets.txt`, `ShellPath.txt`, dan `ShellKeword.txt` (pastikan berada di folder yang sama dengan script).

2. Jalankan script:

```bash
python3 ShellFinder.py
```

3. Saat diminta, masukkan nilai:

```
ENTER LIST SITE (eg targets.txt): targets.txt
ENTER NUMBER OF THREADS (eg 50): 50
ENTER MIN DELAY PER HOST IN SECONDS (eg 1.5): 1.5
```

4. Script akan menampilkan hasil di terminal per-URL dan menyimpan URL yang terdeteksi mengandung keyword ke `BackdorResult.txt`.

---

## Output

* **`BackdorResult.txt`**
  Berisi daftar URL penuh yang ditemukan mengandung keyword (satu per baris).

Terminal akan menampilkan baris informasi berwarna seperti:

```
[INFO] http://example.com/shell.php [Yes Vuln Backdor]
[INFO] http://example.com/other.php [Not Vuln Backdor]
[INFO] http://target.com/login.php [Request Failed]
```

---

## Penjelasan Teknis Singkat

* Script akan membaca daftar target dan menghapus scheme (`http://` atau `https://`) saat memproses domain.
* Untuk setiap domain, script menggabungkan `http://` + domain + setiap path dari `ShellPath.txt`.
* Menggunakan `requests.get()` dengan header dasar dan `User-Agent` acak dari daftar untuk tiap request.
* Ada mekanisme *per-domain rate limiting* — script akan memastikan jeda minimal (`min_delay`) antar-request ke domain yang sama.
* Jika respons `status_code == 200`, script menurunkan teks menjadi huruf kecil dan memeriksa apakah ada kata kunci dari `ShellKeword.txt` di dalamnya. Jika cocok, URL disimpan ke `BackdorResult.txt`.
* Bila terjadi exception saat request, URL akan dicatat di output terminal sebagai `Request Failed`.

---

## Contoh Output di Terminal

```bash
[INFO] http://targetsite.com/shell.php [Yes Vuln Backdor]
[INFO] http://targetsite.com/upload.php [Not Vuln Backdor]
[INFO] http://blockedsite.com/admin.php [Request Failed]
```

---

## Catatan & Tips

* Gunakan jumlah thread yang wajar sesuai kapasitas mesin dan jaringan Anda. Thread terlalu tinggi dapat menyebabkan beban jaringan besar dan IP diblokir.
* Sesuaikan `min delay` untuk menghindari pemblokiran oleh WAF atau proteksi rate-limit target.
* `ShellPath.txt` dan `ShellKeword.txt` yang baik akan menentukan efektivitas deteksi — tambahkan path dan keyword yang relevan dengan target yang Anda audit.
* Pastikan Anda memiliki izin eksplisit untuk melakukan pemindaian pada domain yang ditargetkan.

---

## Penafian

> **PERINGATAN:**
> Tool ini hanya untuk tujuan edukasi, audit keamanan, dan pengujian pada sistem yang Anda miliki atau yang Anda memiliki izin tertulis untuk menguji. Penggunaan tanpa izin dapat melanggar hukum dan kebijakan. Penulis tidak bertanggung jawab atas penyalahgunaan.

---

## Lisensi

Dirilis di bawah **MIT License**.

---

## Kontak & Saluran

- GitHub: [LikeEx01](https://github.com/LikeEx01)
- Telegram: [usernamee1337](https://t.me/usernamee1337)
- Telegram Channel: [ZoneBreach Channel](https://t.me/+rGeAmTK7Mk83Mzg1)
- WhatsApp Channel: [ ZoneBreach Channel](https://whatsapp.com/channel/0029VaudLHc7YSd9S9c9800c)

---

Terima kasih telah menggunakan **Webshell Scanner by RidXploit**!
