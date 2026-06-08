# MATERI PEMBELAJARAN: MANAJEMEN LAYANAN TI (ITSM) - PART 11

## Sub-Topik: Arsitektur *SLA Business Hours* dan Otomasi *Request Model*

### 1. Paradoks Waktu: *System Uptime* vs *Support Window*

Sebuah kesalahan fundamental dalam perancangan layanan TI adalah menyamakan "Waktu Hidup Server" dengan "Waktu Dukungan Teknisi".

* **System Uptime (Ketersediaan Sistem):** Aplikasi seperti Email atau ERP menyala dan bisa diakses 24 jam sehari, 7 hari seminggu (24x7). Kita tidak mematikan *data center* saat pegawai pulang kantor.
* **Support Window (Jendela Dukungan/SLA):** Meskipun aplikasi menyala 24x7, dukungan teknisnya bisa jadi hanya diatur pada **Business Hours** (Jam Kerja, misal 8x5: Senin-Jumat, 08.00 - 17.00).
* **Analisis Sistem (Kategorisasi Katalog):** Aplikasi ITSM harus memisahkan logika SLA per layanan. Jika pengguna komplain Email rusak pada hari Minggu, tiket akan terbuat, tetapi **argo timer SLA tidak akan berdetak** hingga Senin pukul 08.00 pagi. Namun, jika aplikasi yang rusak adalah CRM (yang *support window*-nya 24x7), argo SLA akan langsung berdetak detik itu juga di hari Minggu.

### 2. Anomali "SLA 0 Detik" (The Zero-Second Achievement)

Bagaimana jika tiket Email (SLA 8x5) masuk pukul 20.00 malam, dan ada teknisi piket malam yang iseng mengerjakannya hingga selesai pukul 21.00?

* **Logika Sistem:** Karena tiket dikerjakan dan ditutup **di luar** *Service Window* (jam argo mati), maka sistem akan mengkalkulasi waktu pengerjaan sebagai **0 detik**.
* **Status QA:** Ini BUKAN *bug* atau *error*. Secara sistem pelaporan, ini adalah pencapaian luar biasa (selesai dalam 0 detik dari target 2 jam), karena teknisi menyelesaikan pekerjaan bahkan sebelum waktu argo resminya dimulai.

### 3. Otomasi Templat: *Incident Model* & *Request Model*

Sistem yang dinamis memiliki fitur **Model (Templat)** untuk menstandarisasi masalah atau permintaan yang sangat sering terjadi dan memiliki pola berulang.

* **Incident Model:** Jika ada masalah rutin yang sudah diketahui persis solusinya (misal: *Error code* X, solusinya Y, diselesaikan oleh Tim Z), sistem menyediakan templat (*Auto-fill*). Saat *Service Desk* memilih kategori tersebut, deskripsi dan tim penyelesainya (*Assignment Group*) akan otomatis terisi penuh.
* **Request Model (Kasus Karyawan Baru / Onboarding):** Permintaan *Onboarding* melibatkan banyak tim (Tim Jaringan tarik kabel, Tim Server buat *Active Directory*, Tim Aplikasi buat akun ERP).
* **Aturan UX (User Experience):** *System Analyst* sangat **mengharamkan** pengguna (HRD) untuk membuat 5 tiket berbeda demi 1 karyawan baru. Pengguna hanya cukup menekan satu tombol "Request Onboarding" (1 Parent Ticket). Di belakang layar (*back-end*), *Request Model* akan memecah tiket utama tersebut menjadi 5 *Task/Work Order* berbeda untuk masing-masing divisi secara otomatis.

---

### VISUALISASI LOGIKA SISTEM (SLA TIMER & TASK GENERATION)

**Matriks Logika SLA (Timer Engine)**

| Kategori Layanan | Jam Pelaporan | Status *Timer* SLA di Sistem |
| --- | --- | --- |
| **Email (8x5)** | Senin, 10:00 Pagi | **RUNNING** (Argo Berjalan) |
| **Email (8x5)** | Sabtu, 14:00 Siang | **PAUSED** (Menunggu Senin 08:00) |
| **CRM (24x7)** | Minggu, 02:00 Pagi | **RUNNING** (Argo Berjalan) |

**Arsitektur *Request Model* (Onboarding)**

> `[UI/Front-End]` **HRD Mendaftar 1 Karyawan Baru (Parent Ticket #RQ-100)**
> `[System Engine]` *Auto-Triggered Workflow Tasks* (Paralel):
> ├── $\rightarrow$ `Task 1:` Setup PC & Jaringan $\rightarrow$ *Assign to:* IT Infrastructure
> ├── $\rightarrow$ `Task 2:` Create Email & AD $\rightarrow$ *Assign to:* IT Server
> └── $\rightarrow$ `Task 3:` Create ERP Account $\rightarrow$ *Assign to:* IT Application

---

## GLOSARIUM: DEFINISI ISTILAH TEKNIS

1. **Business Hours (8x5):** Jam operasional kerja normal standar perusahaan, biasanya 8 jam sehari, 5 hari seminggu (Senin - Jumat).
2. **Service / Support Window:** Jendela waktu spesifik di mana tim TI berjanji untuk mengukur SLA dan memberikan dukungan perbaikan.
3. **Achievement Time:** Waktu aktual yang dihabiskan atau dicatat oleh sistem untuk menyelesaikan suatu tiket kerja.
4. **Incident / Request Model:** Templat standar di dalam aplikasi ITSM untuk jenis pelaporan yang sudah sangat sering terjadi, dilengkapi dengan otomasi pengisian data (*auto-fill*) dan rute penugasan (*auto-route*).
5. **Parent-Child Ticket:** Hubungan hierarki tiket di mana satu tiket permohonan utama (*Parent*) memiliki banyak anak tiket tugas (*Child/Work Order*) yang harus diselesaikan untuk menutup tiket utama.

---

## EVALUASI MANDIRI: SOAL UJI PEMAHAMAN

### Bagian A: Pilihan Ganda

1. **Aplikasi HRIS perusahaan menyala dan bisa diakses 24 jam sehari oleh karyawan. Namun, SLA dukungan teknisnya ditetapkan 8x5 (08.00 - 17.00 WIB). Jika seorang karyawan mengirim tiket komplain HRIS pada pukul 21.00 WIB, kapan SLA argo *Response Time* akan mulai berjalan?**
A. Langsung pada pukul 21.00 WIB.
B. Pukul 00.00 WIB tengah malam.
C. Keesokan harinya pada pukul 08.00 WIB.
D. Saat teknisi piket membuka tiket tersebut.
2. **Seorang staf *Service Desk* (yang kebetulan *shift* malam 24x7) mengerjakan tiket aplikasi dengan SLA 8x5 yang masuk pada jam 20.00 malam. Ia menyelesaikannya pukul 22.00 malam itu juga. Berapa lama durasi penyelesaian (*Achievement Time*) yang akan dicatat oleh laporan sistem SLA?**
A. 2 Jam
B. 0 Jam 0 Detik
C. 14 Jam
D. SLA Gagal
3. **Sebagai System Analyst, rancangan sistem seperti apa yang paling optimal untuk mengelola permohonan fasilitas TI bagi "Karyawan Baru" yang melibatkan 4 departemen TI berbeda?**
A. Meminta staf HRD membuat 4 tiket berbeda secara manual agar adil.
B. Membuat 1 tiket, lalu *Service Desk* memindah-mindahkannya ke 4 departemen secara berurutan.
C. Meminta HRD menelepon langsung tiap manajer departemen.
D. Membuat 1 tiket utama (*Parent Ticket*) yang secara sistematis dan otomatis melahirkan 4 tugas (*Child Tasks/Work Orders*) paralel ke tiap departemen.

### Bagian B: Studi Kasus Analitik

1. **Skenario Pengujian Laporan:** Direktur TI marah saat melihat laporan akhir bulan, karena sebuah tiket tertulis selesai dalam "0 Detik". Beliau menganggap ini adalah *bug database* dan memarahi tim QA. Berikan argumen teknis untuk membela tim QA dan jelaskan bahwa sistem justru berjalan dengan sangat akurat!
2. Apa perbedaan utama operasional antara penanganan **Insiden Biasa (Unplanned)** dengan **Insiden Model**?

---

### KUNCI JAWABAN & PEMBAHASAN

**Bagian A (Pilihan Ganda):**

1. **C.** Waktu dukungan teknis (*Support Window*) tunduk pada aturan Business Hours 8x5. Jam di luar itu, argo perhitungan SLA dihentikan sementara (*paused/pending*).
2. **B.** 0 Jam 0 Detik. Pengerjaan dilakukan sepenuhnya di dalam zona waktu mati (*Out of Service Window*). Secara komputasi SLA resmi, tiket selesai sebelum jam kerjanya dimulai.
3. **D.** UX (Pengalaman Pengguna) adalah prioritas. Pengguna (HRD) hanya perlu melihat satu antarmuka permohonan (1 Parent Ticket). Kompleksitas pembagian tugas (4 Child Tasks) adalah urusan otomatisasi aplikasi di dapur TI (Request Model).

**Bagian B (Studi Kasus Analitik):**

1. Sistem berjalan dengan sangat akurat, bukan *bug*. Tiket tersebut adalah tiket dengan kategori layanan **Business Hours (8x5)**, yang dilaporkan pada malam hari. Sistem didesain untuk "membekukan" (*pause*) argo waktu SLA di luar jam kerja. Karena ada teknisi lembur/piket yang mengeksekusinya malam itu juga sebelum jam 08.00 pagi keesokan harinya, maka saat tiket ditutup, argo SLA resmi di sistem belum pernah berjalan sama sekali. Laporan "0 detik" ini valid dan justru menunjukkan efisiensi teknisi di luar jam layanannya.
2. **Insiden Biasa:** Harus diinvestigasi dari awal, agen *Service Desk* harus mengetik deskripsi secara manual, mencari tahu akar penyebab dasar, dan menerka ke departemen mana tiket ini harus dilempar. **Insiden Model:** Memiliki pola yang sudah dikenali. Agen hanya perlu memilih nama Model (Templat), dan sistem akan otomatis mengisi prosedur penyelesaiannya (*workaround*) serta langsung menugaskannya (*auto-assign*) ke tim penyelesai yang spesifik tanpa perlu analisis ulang dari nol.