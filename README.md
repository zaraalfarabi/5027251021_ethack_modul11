# PRAKTIKUM ETHICAL HACKING MODUL 11

| Keterangan | Isi |
| :--- | :--- |
| Nama | Az Zahra Fiddien Al Farabi |
| NRP | 5027251021 |
| Kelas | B |

# Log Password Cracking — Bab 11

## Info Umum

- **Dataset**: `hashes/md5_samples.txt` dan `hashes/ntlm_samples.txt`
- **Hardware**: Intel Core i5-1145G7 (CPU), Hashcat 7.1.2 dengan PoCL OpenCL 3.0 CPU runtime
- **Tanggal**: 19 November 2026

### Bukti Samples 
**- MD5 Samples:**
  
<img width="1918" height="889" alt="image" src="https://github.com/user-attachments/assets/f94a86b6-fa2a-45ef-bbfe-84a321c18ad0" />

**- NTLM Samples:** 

<img width="1918" height="894" alt="image" src="https://github.com/user-attachments/assets/0034068a-6dd5-45f3-afe9-71f43a51f15d" />


---

## Tahap 1 — Dictionary Attack (rockyou.txt, tanpa rule)

| Format Hash | Total Hash | Berhasil Di-crack | Waktu | Persentase |
|---|---:|---:|---|---:|
| MD5 | 16 | 13 | Tidak tercatat | 81,25% |
| NTLM | 16 | 2 | Tidak tercatat | 12,5% |

### Hasil Dictionary Attack MD5

<img width="1189" height="516" alt="image" src="https://github.com/user-attachments/assets/b12c125f-e85b-4f0d-affa-08f85394afa9" />

### Hasil Dictionary Attack NTLM

<img width="1302" height="247" alt="image" src="https://github.com/user-attachments/assets/75a9df65-13e3-4e4a-b1eb-06e6fbb5dbd5" />

**Hash yang gagal ditemukan:**
- MD5: 3 hash.
- NTLM: 14 hash.

---

## Tahap 2 — Rule-Based Attack

### Rule Kustom yang Digunakan

```text
# Starter rules
c
$1
$!
$1$2$3
so0

# Custom rules
$2
$0
^!
u
r
```

### Verifikasi Custom Rules

<img width="1230" height="583" alt="image" src="https://github.com/user-attachments/assets/7d9e9e45-c255-4d44-a97d-77c61da3690e" />

| Format Hash | Total Hash Sisa | Berhasil Di-crack | Waktu Tambahan |
|---|---:|---:|---|
| MD5 | 3 | 0 tambahan | Tidak tercatat |
| NTLM | 14 | 0 tambahan | Tidak tercatat |

### Hasil Rule-Based Attack MD5

<img width="1015" height="265" alt="image" src="https://github.com/user-attachments/assets/9881ec17-39d9-4e13-b049-db4751c4b658" />

### Hasil Rule-Based Attack NTLM

<img width="793" height="190" alt="image" src="https://github.com/user-attachments/assets/1c167095-165d-455e-8b33-9a5dcab61d67" />

**Analisis:** Rule-based attack tidak menemukan password tambahan. Hasil akhir tetap 13/16 untuk MD5 dan 2/16 untuk NTLM.



## Tahap 3 — Hybrid Attack

Pengujian menggunakan Hashcat attack mode 6 (wordlist + dua digit angka di akhir).

| Format Hash | Total Hash | Berhasil Di-crack | Waktu |
|---|---:|---:|---|
| MD5 | 16 | 2 | Tidak tercatat |
| NTLM | 16 | 0 | 4 menit 47 detik |

### Hasil Hybrid Attack MD5

<img width="934" height="196" alt="image" src="https://github.com/user-attachments/assets/bdd36d6e-166b-46a3-8abd-2a8db30224af" />

Password yang ditemukan:
- `qwerty123`
- `admin123`

### Hasil Hybrid Attack NTLM

<img width="1135" height="418" alt="image" src="https://github.com/user-attachments/assets/b295b3cc-1f2e-4ba0-bc75-d726be1a8e32" />

**Analisis:**
- MD5 menemukan 2 password dengan pola kata umum diikuti angka.
- NTLM tidak menemukan password menggunakan pola tersebut.
- Penambahan angka di akhir kata umum tidak selalu cukup untuk meningkatkan keamanan password.

---

## Tahap 4 — John the Ripper (Pembanding)

| Format Hash | Berhasil Di-crack | Catatan Perbedaan vs Hashcat |
|---|---:|---|
| MD5 | 13/16 (81,25%) | Hasil sama dengan Hashcat pada dictionary attack. |
| NTLM | 2/16 (12,5%) | Hasil sama dengan Hashcat pada dictionary attack. |

### Hasil John the Ripper MD5

<img width="1300" height="307" alt="image" src="https://github.com/user-attachments/assets/d762b388-5b2a-4fab-adfa-729bb647147a" />

### Bukti Verifikasi NTLM

<img width="1167" height="196" alt="image" src="https://github.com/user-attachments/assets/5bfcced8-b2ca-453c-a238-b771e87383b3" />

---

## Analisis Pattern Password

| Kategori Pattern | Contoh yang Ditemukan | Frekuensi (perkiraan) |
|---|---|---|
| Kata kamus polos (tanpa variasi) | `password` | Minimal 1 |
| Kata kamus + angka di akhir | `qwerty123`, `admin123` | 2 |
| Kata kamus + simbol di akhir | Belum dapat dipastikan | Belum diketahui |
| Huruf besar di awal + variasi | Belum dapat dipastikan | Belum diketahui |
| Lain-lain | `1` | Minimal 1 |

*Catatan: Frekuensi merupakan jumlah contoh yang dapat diidentifikasi dari hasil yang tercatat, bukan perhitungan seluruh password.*

## Rekomendasi Kebijakan Password

Berdasarkan hasil praktikum, penambahan angka sederhana di akhir kata umum masih dapat diprediksi melalui teknik cracking.

Organisasi sebaiknya:
1. Mewajibkan penggunaan password yang panjang dan unik.
2. Mendorong penggunaan passphrase yang terdiri dari beberapa kata yang tidak mudah ditebak.
3. Melarang penggunaan password umum dan pola berurutan.
4. Tidak mengandalkan penambahan angka atau simbol sederhana di akhir kata.
5. Menggunakan password manager dan MFA.
6. Memeriksa password terhadap daftar password umum atau password yang pernah bocor.

---

# Observasi AV Evasion — Bab 11

## Info Umum
- **Versi Windows VM**:
- **Status Windows Defender** (pastikan aktif, real-time protection ON):
- **Tanggal observasi**:

## Payload 1 — Baseline (Tanpa Encoding)

| Tahap | Terdeteksi? (Ya/Tidak) | Nama Threat (jika ada) | Catatan |
|---|---|---|---|
| Saat file landing di disk | | | |
| Saat file di-scan manual | | | |
| Saat file dicoba dijalankan | | | |

Metode deteksi yang kamu duga aktif berdasarkan KAPAN terdeteksi:

## Payload 2 — Encoded (shikata_ga_nai x3)

| Tahap | Terdeteksi? (Ya/Tidak) | Nama Threat (jika ada) | Catatan |
|---|---|---|---|
| Saat file landing di disk | | | |
| Saat file di-scan manual | | | |
| Saat file dicoba dijalankan | | | |

Perbedaan dibanding Payload 1:

## Kesimpulan

**Seberapa efektif Windows Defender bawaan terhadap payload Metasploit standar?**

**Apa yang bisa disimpulkan soal pentingnya EDR di atas AV bawaan, berdasarkan observasi ini?**

**Dari sisi blue team: log/event apa yang seharusnya dipantau sysadmin untuk mendeteksi aktivitas seperti ini?**
