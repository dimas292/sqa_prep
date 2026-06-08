# MATERI PEMBELAJARAN: MANAJEMEN LAYANAN TI (ITSM) - PART 6

## Sub-Topik: Arsitektur Struktur *Service Desk* dan Logika *Routing*

Secara operasional dan sistem, struktur *Service Desk* dibagi menjadi lima model yang masing-masing memiliki logika penugasan (*assignment logic*) yang berbeda di dalam aplikasi ITSM.

### 1. Local Service Desk (Arsitektur Desentralisasi)

* **Konsep Bisnis:** Setiap kantor cabang memiliki tim *Service Desk* sendiri di lokasi fisiknya.
* **Analisis Sistem (Logika Routing):** Aplikasi harus diprogram dengan pembatasan geografis (*Geofencing/Site-based Routing*). Karyawan di Cabang A hanya bisa mengirim tiket ke *Service Desk* Cabang A.

### 2. Centralized Service Desk (Arsitektur Pemusatan)

* **Konsep Bisnis:** Seluruh kontak pengguna dipusatkan ke satu lokasi (misal: Kantor Pusat di Jakarta).
* **Analisis Sistem (Logika Routing):** Semua tiket masuk ke satu *pool* antrean utama (Sentral). Jika tiket butuh penanganan perangkat keras di daerah, *Service Desk* pusat akan melakukan *disposisi/assign* tiket tersebut ke teknisi lokal di cabang.

### 3. Virtual Service Desk (Arsitektur Terdistribusi Berbasis Cloud)

* **Konsep Bisnis:** Menggunakan teknologi telekomunikasi pintar dan AI (Chatbot) untuk mendistribusikan tiket/telepon ke agen di seluruh dunia secara acak berdasarkan ketersediaan (*availability*).
* **Analisis Sistem (Logika Routing):** Sistem sangat bergantung pada *Automatic Call Distribution* (ACD) dan *Interactive Voice Response* (IVR). Sistem yang akan mencari "Siapa agen yang sedang kosong/idle di belahan dunia mana pun?" lalu rute jaringan diarahkan ke sana.

### 4. Follow The Sun (Arsitektur Operasional Global 24/7)

* **Konsep Bisnis:** Memanfaatkan perbedaan zona waktu bumi untuk memberikan layanan 24 jam tanpa perlu menyuruh karyawan bekerja lembur (hanya *shift* siang).
* **Analisis Sistem (Logika Routing):** Sangat efisien, namun membutuhkan standarisasi infrastruktur TI yang ekstrem. *Routing* tiket dipindahkan otomatis berdasarkan jam dunia (*Time-Zone Trigger*).
* Pukul 08:00 - 17:00 (WIB) $\rightarrow$ Tiket masuk ke Hub Jakarta.
* Pukul 17:00 - 01:00 (WIB) $\rightarrow$ Tiket dialihkan ke Hub London.
* Pukul 01:00 - 08:00 (WIB) $\rightarrow$ Tiket dialihkan ke Hub New York.


* **Prasyarat Sistem:** Wajib menggunakan bahasa yang sama (umumnya bahasa Inggris) dan **Satu Sistem ITSM Global** agar riwayat tiket (*history*) bisa dibaca antar benua.

### 5. Specialized Service Desk (Arsitektur Eskalasi Nol / Ahli Langsung)

* **Konsep Bisnis:** Secara tradisional, entitas *Service Desk* pemula ditiadakan. Kontak pertama pengguna langsung dijawab oleh tim spesialis tingkat lanjut.
* **Analisis Sistem (Logika Routing):** Digunakan untuk industri yang sangat kompleks. Logika aplikasinya memotong jalur eskalasi standar (*bypass escalation*). Tiket langsung diterima, dianalisis, dan dieksekusi tuntas di satu lapisan *support* (*Tier 2/Tier 3 experts*).

---

### VISUALISASI PERBANDINGAN STRUKTUR (MATRIKS SISTEM)

| Struktur Service Desk | Kompleksitas *Routing* IT | Tingkat Skalabilitas (*Scalability*) | Karakteristik Utama |
| --- | --- | --- | --- |
| **Local** | Rendah | Sangat Rendah (Boros SDM) | Kedekatan fisik dengan *user*. |
| **Centralized** | Menengah | Tinggi | Efisiensi tinggi, satu pintu pelaporan. |
| **Virtual** | Sangat Tinggi | Sangat Tinggi | *Routing* AI, Agen terdistribusi di mana saja. |
| **Follow The Sun** | Tinggi (Time-Zone Logic) | Tinggi (Operasi 24/7) | Estafet antar zona waktu tanpa lembur malam. |
| **Specialized** | Rendah | Menengah (Boros Gaji Ahli) | Tanpa eskalasi panjang, langsung pakar teknis. |

---

## GLOSARIUM: DEFINISI ISTILAH TEKNIS

1. **IVR (Interactive Voice Response):** Sistem telepon otomatis yang merespons input suara atau *keypad* pengguna (misal: "Tekan 1 untuk Bahasa Indonesia, Tekan 2 untuk Jaringan...").
2. **ACD (Automatic Call Distribution):** Algoritma sistem telekomunikasi yang mendistribusikan panggilan atau tiket masuk ke agen (petugas) yang paling tepat atau agen yang sedang kosong (*idle*).
3. **Follow the Sun:** Metode alur kerja operasional global yang membagi *shift* pekerjaan berdasarkan zona waktu (rotasi bumi) untuk mencapai cakupan operasional 24 jam.
4. **Disposisi / Assignment:** Proses mengalihkan kepemilikan kerja dari satu entitas pusat (misal *Service Desk*) kepada teknisi lapangan (*Field Engineer*) untuk dieksekusi.
5. **Bypass:** Tindakan melompati suatu tahapan atau aturan konvensional dalam suatu alur kerja agar proses berjalan lebih cepat.

---

## EVALUASI MANDIRI: SOAL UJI PEMAHAMAN

### Bagian A: Pilihan Ganda

1. **Sebuah bank nasional memusatkan seluruh pelaporan gangguan ATM ke satu nomor telepon *Call Center* di Jakarta. Namun, jika ada ATM rusak di Surabaya, pusat akan membuat tiket dan mengirim perintah (*assign*) ke teknisi yang ada di Surabaya. Ini adalah contoh struktur...**
A. Local Service Desk
B. Centralized Service Desk
C. Virtual Service Desk
D. Follow The Sun
2. **Kelemahan paling fatal (atau risiko tertinggi) jika sebuah perusahaan global ingin mengimplementasikan metode *Follow The Sun* adalah...**
A. Biaya lembur karyawan akan membengkak drastis.
B. Setiap negara wajib memiliki 3 shift kerja penuh waktu.
C. Terjadinya kegagalan komunikasi jika tidak ada standarisasi bahasa (*Language*) dan kesatuan Sistem Aplikasi ITSM (*Single Tool*).
D. *Interactive Voice Response* (IVR) sering memutus telepon pengguna.
3. **Perusahaan rintisan (*startup*) di bidang *Cybersecurity* tidak menggunakan agen *Service Desk* tingkat awal (*entry-level*). Saat klien mengalami kebocoran data, tiket langsung dipegang oleh *Senior Security Engineer*. Ini menerapkan konsep struktur...**
A. Specialized Service Desk Group
B. Virtual Service Desk
C. Follow The Sun
D. Local Service Desk

### Bagian B: Studi Kasus Analitik

1. **Analisis *Time-Zone* QA:** Sebagai QA Tester, Anda sedang menguji modul aplikasi pergantian pergantian *shift* pada struktur *Follow The Sun* (Jakarta-London-New York). Jika sistem aplikasi masih memisahkan *database* tiket untuk tiap benua secara terisolasi (sistem Jakarta berdiri sendiri, London berdiri sendiri), jelaskan mengapa ini dikategorikan sebagai *Critical System Design Failure* (Kegagalan Desain Sistem Kritis)!
2. Jelaskan perbedaan cara kerja sistem distribusi panggilan pada *Local Service Desk* dengan sistem *Virtual Service Desk* (yang menggunakan *IVR/ACD*)!

---

### KUNCI JAWABAN & PEMBAHASAN

**Bagian A (Pilihan Ganda):**

1. **B.** *Centralized Service Desk*. Pintu masuk komunikasi hanya satu (di pusat), sementara eksekusi teknis perangkat keras tetap dibagikan ke cabang-cabang.
2. **C.** Jika tim di Jakarta siang dan London malam memakai aplikasi tiketing yang terpisah atau mencatat dalam bahasa lokalnya masing-masing, saat tiket dilimpahkan dari Jakarta ke London, tim London tidak akan bisa membaca dan melanjutkan penyelesaian masalah (*breakdown of communication*).
3. **A.** *Specialized Service Desk Group*. Melompati jalur *Service Desk* level dasar langsung kepada pakar ahlinya karena tingkat kompleksitas masalah yang ditangani sehari-hari sangat tinggi.

**Bagian B (Studi Kasus Analitik):**

1. Jika *database* diisolasi, maka saat matahari terbenam di Jakarta dan *shift* berakhir, agen di London **tidak bisa melihat** apa yang sudah dikerjakan oleh agen Jakarta. Tiket menggantung dan riwayat (*history log*) terputus. Filosofi *Follow The Sun* mewajibkan estafet yang mulus, sehingga infrastruktur *database* harus terpusat secara global (*Single Database ITSM Instance*) agar agen lintas benua bisa meneruskan penanganan masalah tanpa menyuruh pengguna menceritakan ulang masalahnya dari awal.
2. Pada *Local Service Desk*, pengguna secara manual menelepon nomor lokal cabang mereka (tidak ada algoritma *routing* terpusat). Pada *Virtual Service Desk*, ada gerbang masuk terpusat berupa sistem teknologi (ACD & IVR) yang secara otomatis akan mengecek ketersediaan agen secara *real-time* di seluruh dunia, dan merutekan ulang sinyal komunikasi ke agen mana pun yang bersedia menangani panggilan, terlepas dari di mana agen tersebut berada secara fisik.