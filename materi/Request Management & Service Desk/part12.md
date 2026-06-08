# MATERI PEMBELAJARAN: MANAJEMEN LAYANAN TI (ITSM) - PART 12

## Sub-Topik: Analitik Performa (SLA/OLA) dan Pengecualian Status *Force Majeure*

### 1. Arsitektur *Request Model*: *Frontend* Sederhana, *Backend* Kompleks

Sistem yang baik harus menyembunyikan kerumitan operasional dari mata pengguna (*abstraction*).

* **Logika Sistem:** Pengguna (misal: HRD) hanya melihat 1 Form Pembuatan Tiket dan mendapatkan 1 Nomor Tiket Induk (misal: `REQ-001`).
* Di balik layar (*Backend*), mesin *workflow* akan memecah `REQ-001` menjadi beberapa *Work Order* (WO) atau *Sub-Task* secara paralel maupun serial. Tim Jaringan dan Tim Aplikasi bisa bekerja bersamaan tanpa harus menyuruh pengguna mengisi formulir berulang kali.

### 2. Paradigma Penyelesaian: *Firefighter* vs *Detective*

Banyak manajer TI salah kaprah dengan menuntut analisis mendalam pada proses *Incident Management*.

* **Incident Management (Logika Pemadam Kebakaran):** Target utama SLA Insiden adalah **layanan menyala kembali secepat mungkin**. Jika *server* mati, menyelesaikannya dengan cara di-*restart* atau bahkan "ditendang" (secara kiasan/fisik) diperbolehkan, asalkan layanan kembali normal (*workaround*). Insiden **TIDAK** membutuhkan *Root Cause Analysis* (RCA).
* **Problem Management (Logika Detektif):** Targetnya adalah mencari **Akar Masalah** (*Root Cause*) dan membuat **Solusi Permanen**. Proses ini murni investigasi, sehingga **SANGAT HARAM** diukur menggunakan SLA waktu yang ketat. Jika dipaksa pakai SLA, teknisi hanya akan mencari solusi instan (menjadi pemadam kebakaran lagi). Pengukuran *Problem Ticket* menggunakan fleksibilitas *Due Date* (Tenggat Waktu) yang bisa dinegosiasikan.

### 3. Cacat Desain SLA Tradisional: Sindrom "Tertuduh Terakhir"

Bagaimana menganalisis tim mana yang kinerjanya buruk jika sebuah tiket melampaui SLA (*Breached*)?

* **Kesalahan Alat (Tools Flaw):** Aplikasi ITSM tradisional hanya mencatat waktu saat tiket "Dibuat" dan waktu saat tiket "Selesai". Jika tiket berpindah dari *Service Desk* $\rightarrow$ *Tim Server* $\rightarrow$ *Tim Network*, dan tiket tersebut melewati batas SLA, sistem akan otomatis memberi rapor merah kepada pemegang tiket terakhir (Tim *Network*).
* **Solusi Sistem (Multi-SLA / OLA Timer):** Aplikasi yang canggih harus memiliki argo hitung per grup (*Operational Level Agreement* / OLA). Setiap kali tiket dilempar antar-grup, argo grup lama berhenti (*pause/stop*), dan argo grup baru berjalan (*start*). Dengan data ini, analis dapat melihat bahwa Tim *Network* mungkin hanya mengerjakan selama 10 menit, sementara *bottleneck* terbesarnya ada di Tim *Server* yang memegang tiket selama 10 jam.

### 4. Pengecualian SLA: Sistem Pemulihan Bencana (*Disaster Recovery*)

Apakah kegagalan *Data Center* karena gempa bumi atau *Force Majeure* diukur menggunakan SLA tiket Insiden?

* **Logika Pengecualian (Exception Logic):** Tidak. Kejadian Bencana/Disaster akan memicu penghentian sementara SLA Insiden reguler. Status tiket akan diekskalasi menjadi *Major Incident / Disaster*.
* **Parameter Pengganti:** Kinerja TI pada saat bencana diukur menggunakan metrik *Business Continuity*, yaitu **RTO** (*Recovery Time Objective*) dan **RPO** (*Recovery Point Objective*).

---

### VISUALISASI LOGIKA SISTEM (THE OLA TIMER ENGINE)

**Skenario SLA Tiket: 8 Jam (Total Waktu yang Dijanjikan ke Pengguna)**

> `[08:00] Tiket Dibuat` $\rightarrow$ **SLA UTAMA START (Argo 8 Jam Berjalan)**
> ├── `[08:00 - 09:00]` Dipegang *Service Desk* $\rightarrow$ **OLA SD: 1 Jam**
> ├── `[09:00 - 16:00]` Dilempar ke Tim Server $\rightarrow$ **OLA Server: 7 Jam** (Penyebab Keterlambatan)
> └── `[16:00 - 16:15]` Dilempar ke Tim Network $\rightarrow$ **OLA Network: 15 Menit**
> `[16:15] Tiket Closed` $\rightarrow$ **SLA UTAMA BERHENTI (Total: 8 Jam 15 Menit = BREACHED)**
> *Sistem Analitik OLA membuktikan bahwa penyebab Breach adalah Tim Server, bukan Tim Network sebagai pemegang terakhir.*

---

## GLOSARIUM: DEFINISI ISTILAH TEKNIS

1. **Breach (SLA Breached):** Kondisi di mana penyelesaian sebuah pekerjaan TI gagal memenuhi (melewati) batas waktu yang telah disepakati dalam dokumen SLA.
2. **OLA (Operational Level Agreement):** Perjanjian teknis dan target batas waktu antargrup di dalam internal departemen TI itu sendiri (misalnya antara Tim Jaringan dan Tim Server) untuk mendukung tercapainya SLA utama.
3. **Workaround:** Solusi instan atau sementara untuk memulihkan gangguan secepat mungkin (misalnya me-*restart* server), meskipun akar masalahnya belum diperbaiki.
4. **RCA (Root Cause Analysis):** Proses sistematis dan analitis untuk mengidentifikasi penyebab dasar dari sebuah insiden fatal atau insiden berulang.
5. **RTO (Recovery Time Objective):** Target batas waktu maksimal yang diizinkan untuk sebuah layanan TI mati saat terjadi bencana, hingga layanan tersebut harus menyala kembali secara operasional (misal: RTO Data Center adalah 24 jam).
6. **Force Majeure:** Kejadian tak terduga yang berada di luar kendali perusahaan (seperti gempa bumi, banjir, pemadaman listrik kota) yang membatalkan kewajiban SLA normal.

---

## EVALUASI MANDIRI: SOAL UJI PEMAHAMAN

### Bagian A: Pilihan Ganda

1. **Aplikasi *Mobile Banking* tiba-tiba mengalami *crash* dan nasabah tidak bisa bertransaksi. Tim Server memutuskan untuk melakukan *Restart* paksa pada sistem. Layanan menyala kembali, namun tim belum tahu mengapa sistem tersebut bisa *crash*. Dalam ITSM, tindakan me-*restart* ini disebut sebagai...**
A. Root Cause Analysis
B. Permanent Solution
C. Workaround (Tindakan Insiden/Pemadam Kebakaran)
D. Problem Management
2. **Sebuah sistem ITSM jadul mencatat Tim Aplikasi sebagai penyumbang kegagalan SLA tertinggi. Namun setelah diusut, Tim Aplikasi selalu menerima lemparan tiket dari Tim *Service Desk* hanya 5 menit sebelum batas SLA habis. Konfigurasi apa yang tidak dimiliki oleh aplikasi ITSM jadul tersebut?**
A. Parameter pengukuran SLA untuk Problem Management.
B. Pengukuran RTO dan RPO.
C. Pengukuran OLA (*Operational Level Agreement*) dengan *timer* Start/Pause per masing-masing grup.
D. Tidak ada menu untuk melakukan *Restart* sistem.
3. **Gedung *Data Center* utama mengalami kebakaran hebat. Komitmen waktu operasional pemulihan layanan dalam kondisi *Force Majeure* ini tidak diukur dengan SLA Insiden, melainkan diukur menggunakan indikator...**
A. FCR (*First Contact Resolution*)
B. RTO (*Recovery Time Objective*)
C. OLA (*Operational Level Agreement*)
D. RCA (*Root Cause Analysis*)

### Bagian B: Studi Kasus Analitik

1. **Analisis Kebijakan Manajemen:** Seorang Direktur TI mengeluarkan peraturan: *"Setiap tiket Insiden yang masuk harus diselesaikan secara permanen hingga ke akar masalahnya (RCA), dan SLA waktu resolusinya tetap maksimal 2 Jam!"*
Gunakan perspektif *System Analyst* Anda untuk menjelaskan mengapa peraturan ini cacat logika dan akan menghancurkan operasional TI!
2. Jelaskan perbedaan *user experience* pada aplikasi pemesanan tiket yang memiliki fitur *Request Model* vs aplikasi yang tidak memiliki fitur tersebut saat harus mendaftarkan 1 karyawan baru!

---

### KUNCI JAWABAN & PEMBAHASAN

**Bagian A (Pilihan Ganda):**

1. **C.** *Workaround*. Tujuan utama penanganan insiden adalah mengembalikan layanan normal secepat mungkin, meskipun akar masalahnya belum terselesaikan.
2. **C.** Sistem jadul hanya mengukur waktu dari awal pengguna meminta hingga selesai (A ke Z), tanpa mencatat argo internal antar departemen teknis, sehingga memunculkan jebakan "Sindrom Tertuduh Terakhir". Pengukuran OLA memecahkan masalah pelacakan kinerja ini.
3. **B.** *Recovery Time Objective* (RTO). Saat bencana (Major Incident/Disaster), pengukuran SLA reguler dibekukan dan diganti dengan parameter Kesinambungan Bisnis (*Business Continuity*).

**Bagian B (Studi Kasus Analitik):**

1. Peraturan tersebut cacat karena mencampurkan dua konsep bertolak belakang: **Kecepatan** (Insiden) vs **Ketelitian** (Problem). Jika teknisi diwajibkan melakukan riset *Root Cause Analysis* (yang memakan waktu investigasi berhari-hari) tapi dipaksa dibatasi SLA 2 jam, maka teknisi akan "memalsukan" laporan akar masalah demi mengejar kecepatan waktu, dan solusi permanen tidak akan pernah terwujud. Penanganan harus dipisah: Matikan apinya dulu di bawah 2 jam (Tiket Insiden), lalu buka tiket baru untuk mencari penyebab kebakarannya tanpa dikejar SLA ketat (Tiket Problem).
2. Tanpa *Request Model*, departemen HRD (*user*) harus mengklik menu, mengisi deskripsi, dan menekan tombol *Submit* berulang kali (misal 4 kali) untuk 4 departemen TI yang berbeda. Hal ini boros waktu dan melelahkan. Dengan *Request Model*, UX sangat ringkas: HRD hanya mengisi 1 formulir (Induk), dan sistem aplikasi di belakang layar yang akan bekerja keras memecah serta mendistribusikan permohonan tersebut ke 4 departemen TI secara otomatis.