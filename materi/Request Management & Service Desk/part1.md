# MATERI PEMBELAJARAN: MANAJEMEN LAYANAN TI (ITSM)

## Sub-Topik: Rekonsiliasi Service Desk, Request, Incident, dan Problem Management

### 1. Mengapa Request Management dan Service Desk Digabung?

Dalam operasional harian, **Service Desk** bertindak sebagai *Single Point of Contact* (SPOC) atau gerbang utama bagi pengguna (*user*). Sementara, **Request Management** adalah prosedur penanganan permintaan pengguna yang sifatnya rutin dan terstandarisasi.

Penggabungan kedua materi ini dilakukan karena mayoritas aktivitas harian *Service Desk* adalah menerima dan mengeksekusi *Service Request*.

**Evolusi Kerangka Kerja (Framework):**

* **ITIL v3 (Versi Sebelumnya):** *Request Fulfillment/Management* dikategorikan sebagai **Proses**, sedangkan *Service Desk* dikategorikan sebagai **Fungsi** (tim/organisasi).
* **ITIL 4 (Versi Terbaru):** Dikonsolidasikan di mana keduanya kini disebut sebagai **Practice (Praktik/Proses)**.
* *Catatan Implementasi:* Praktik nyata di industri memadukan acuan **ITIL 4**, **ISO 20000** (Standar Internasional ITSM), dan pengalaman empiris di lapangan selama puluhan tahun untuk menciptakan sistem yang fleksibel, bukan sekadar mengikuti teori secara kaku.

---

### 2. Tiga Pilar Utama Tiketing: Incident vs Request vs Problem

Sebagai System Analyst, kita harus memisahkan entitas data database tiketing ini berdasarkan karakteristik dasarnya agar tidak terjadi salah input data (*data anomaly*).

#### A. Incident Management (Gangguan)

* **Karakteristik:** *Unplanned* (Tidak Direcanakan).
* **Definisi:** Gangguan yang tidak direncanakan terhadap layanan TI atau penurunan kualitas layanan yang sedang berjalan.
* **Kasus Khusus (Infrastruktur DRC):** Jika ada komponen infrastruktur di *Disaster Recovery Center* (DRC) yang rusak, ini **wajib** dicatat sebagai **Insiden**. Mengapa? Karena jika data center utama mati, DRC tidak akan bisa beroperasi normal. Kegagalan komponen DRC menurunkan kesiapan sistem secara keseluruhan.

#### B. Request Management / Service Request (Permintaan)

* **Karakteristik:** *Planned* (Direncanakan).
* **Definisi:** Permintaan resmi dari pengguna untuk mendapatkan sesuatu yang belum mereka miliki atau bagian dari layanan standar.
* **Kriteria:** Risiko kecil, sering dilakukan (rutin), alur kerja terstandarisasi, dan biayanya rendah.

#### C. Problem Management (Akar Masalah)

* **Karakteristik:** *Root Cause Analysis* (Analisis Akar Penyebab).
* **Definisi:** Pencarian penyebab utama atau inti dari satu atau lebih insiden.
* **Mitos yang Salah:** *"Problem baru muncul jika insiden terjadi berulang kali."* -> **Salah!** Satu insiden pun bisa langsung memicu investigasi *Problem* jika dampaknya besar. Secara sistem tiketing, jika ada pola insiden berulang, tim QA/Analyst akan melakukan eskalasi dari tiket *Incident* menjadi tiket *Problem* untuk diselesaikan akar masalahnya agar insiden tersebut tidak terjadi lagi selamanya.

---

### 3. Studi Kasus & Visualisasi Alur Kerja (Skenario Praktis)

Untuk memperjelas perbedaan *Incident* dan *Request*, mari kita lihat visualisasi logika sistem berikut:

#### Kasus 1: Reset Password (Kategori: INCIDENT)

* **Logika Sistem:** Pengguna **sudah memiliki akun dan password**, namun tiba-tiba tidak bisa login (entah karena terblokir salah 3x, lupa, atau *glitch* sistem).
* **Analisis QA:** Kondisi normal terganggu (*Interruption to normal service*). Pengguna seharusnya bisa masuk sistem, tapi gagal. Karena sifatnya merusak kondisi normal operasional, ini masuk ke **Incident Management**.

#### Kasus 2: Minta Akun Baru / Laptop Baru (Kategori: REQUEST)

* **Logika Sistem:** Pengguna **belum pernah memiliki akses** ke aplikasi ERP tersebut atau membutuhkan perangkat kerja baru.
* **Analisis QA:** Ini adalah kondisi terencana (*planned*). Pengguna meminta sesuatu yang berada di luar layanan aktif yang sedang mereka nikmati saat itu. Karena jalurnya mengikuti prosedur standar pengadaan/pembuatan akun, ini masuk ke **Request Management**.

---

## GLOSARIUM: DEFINISI ISTILAH TEKNIS

1. **ITSM (IT Service Management):** Metode pengelolaan layanan TI yang berfokus pada keselarasan antara teknologi informasi dengan kebutuhan bisnis perusahaan.
2. **ITIL (Information Technology Infrastructure Library):** Kerangka kerja (framework) berisi best practice global untuk mengelola layanan TI. Versi terbarunya adalah ITIL 4.
3. **ISO 20000:** Standar internasional resmi pertama untuk manajemen layanan TI yang dikeluarkan oleh ISO, menjadi tolak ukur audit sertifikasi perusahaan.
4. **DRC (Disaster Recovery Center):** Pusat pemulihan bencana berupa fasilitas/data center cadangan yang digunakan untuk memulihkan operasional TI ketika data center utama mengalami kegagalan total (*collapse*).
5. **Best Practice:** Metode atau teknik yang telah terbukti secara konsisten memberikan hasil yang superior dibandingkan dengan metode lainnya di industri.
6. **Root Cause:** Akar penyebab paling mendasar dari suatu masalah atau kegagalan sistem, yang jika dihilangkan, akan mencegah masalah tersebut terulang kembali.
7. **Unplanned vs Planned:** *Unplanned* berarti kejadian mendadak yang merusak jalurnya operasional normal; *Planned* berarti aktivitas terstruktur yang dijadwalkan dan mengikuti alur persetujuan (*approval*) standar.

---

## EVALUASI MANDIRI: SOAL UJI PEMAHAMAN

Silakan jawab pertanyaan-pertanyaan berikut untuk menguji pemahaman Anda terhadap materi di atas:

### Bagian A: Pilihan Ganda

1. **Berdasarkan ITIL 4, *Request Management* dan *Service Desk* diklasifikasikan sebagai...**
A. Proses dan Fungsi
B. Fungsi dan Praktik
C. Keduanya adalah Practice (Praktik/Proses)
D. Keduanya adalah Fungsi
2. **Sebuah komponen server cadangan di DRC mengalami kerusakan fisik (harddisk bad sector). Sebagai QA/Analyst, kategori tiket apa yang harus dibuka?**
A. Request Management karena itu komponen cadangan
B. Incident Management karena dapat mengganggu operasional pemulihan bencana
C. Problem Management karena servernya rusak
D. Change Management untuk mengganti harddisk
3. **Seorang staf HR meminta pemasangan aplikasi pengedit video (Adobe Premiere) ke laptop kerjanya untuk kebutuhan proyek bulan depan. Ini merupakan contoh dari...**
A. Incident Management
B. Problem Management
C. Service Desk Function
D. Request Management

### Bagian B: Esai Singkat

1. Mengapa insiden "Lupa Password" atau "Akun Terkunci akibat salah input 3 kali" dikategorikan sebagai **Incident**, bukan **Request**? Jelaskan menggunakan perspektif gangguan layanan (*interruption*)!
2. Jelaskan kekeliruan pemahaman mengenai kalimat berikut: *"Problem Management baru bisa berjalan jika sebuah insiden sudah terjadi berkali-kali."*!

---

### KUNCI JAWABAN & PEMBAHASAN

**Bagian A (Pilihan Ganda):**

1. **C.** Pada ITIL 4, perbedaan kaku antara proses dan fungsi dilebur, keduanya kini disebut sebagai *Practice*.
2. **B.** *Incident Management*. Kerusakan di DRC berdampak pada penurunan kualitas kesiapan layanan jika sewaktu-waktu data center utama mati.
3. **D.** *Request Management*. Ini adalah permintaan terencana, standar, dan meminta sesuatu yang belum dimiliki sebelumnya oleh pengguna.

**Bagian B (Esai Singkat):**

1. Karena pengguna tersebut sejatinya sudah memiliki hak akses fungsional secara normal. Ketika ia tidak bisa masuk (karena lupa/terkunci), terjadi interupsi terhadap operasional normalnya (*unplanned interruption*). Sesuai definisi, segala sesuatu yang menghentikan atau menurunkan kualitas layanan normal adalah **Incident**.
2. Kalimat tersebut keliru karena *Problem Management* berfokus pada pencarian **akar masalah (root cause)**. Satu insiden tunggal yang belum pernah terjadi sebelumnya namun dampaknya sangat fatal atau masif bisa langsung dibuatkan tiket *Problem* untuk diinvestigasi penyebabnya agar tidak terjadi lagi di kemudian hari.