# MATERI PEMBELAJARAN: MANAJEMEN LAYANAN TI (ITSM) - PART 9

## Sub-Topik: Automasi Notifikasi, Penutupan Tiket, dan Rekayasa CSAT

### 1. Arsitektur Komunikasi Berbasis Kejadian (*Event-Driven Notification*)

Sangat tidak masuk akal secara operasional jika agen *Service Desk* harus memonitor setiap tiket secara manual dan menelepon pengguna satu per satu setiap jam hanya untuk memberikan kabar (*update*).

* **Analisis Sistem (QA Perspective):** Aplikasi harus dibangun menggunakan arsitektur *Event-Driven* atau *Time-Driven*.
* **State-Change Trigger:** Sistem otomatis mengirimkan pesan (Email/WhatsApp) setiap kali status tiket berubah (misal: dari *In Progress* menjadi *Resolved*).
* **Time-Scale Trigger:** Sistem otomatis mengirimkan peringatan kepada pengguna berdasarkan persentase SLA yang telah berjalan (misal: Notifikasi terkirim saat SLA mencapai 25%, 50%, dan 75%). Hal ini menggeser beban komunikasi dari agen manusia ke *server* aplikasi.

### 2. Segel Validasi Pengguna (*The Closing Gate*)

Seperti yang dibahas pada materi sebelumnya, tugas *Service Desk* adalah memastikan tiket mencapai status akhir, yaitu **Closed**.

* **Logika Sistem:** Teknisi TI hanya berhak mengubah status menjadi **Resolved** (untuk Insiden) atau **Completed** (untuk Request). Status **Closed** BUKAN milik TI, melainkan hak prerogatif pengguna.
* Aplikasi harus memfasilitasi tombol "Terima Solusi" bagi pengguna. Jika pengguna tidak merespons dalam batas waktu tertentu (misal: 3x24 jam), barulah sistem (*Auto-Close Cronjob*) menyegel tiket tersebut secara permanen.

### 3. Manajemen CSAT dan Bias Ingatan (*Recency Bias*)

Melakukan survei kepuasan (*Customer Satisfaction* / CSAT) setahun sekali di bulan Desember adalah kegagalan fatal dalam metodologi analitik data.

* **Efek Recency Bias:** Psikologi manusia cenderung hanya mengingat kejadian buruk yang paling baru. Jika TI melayani dengan sempurna dalam 250 tiket sejak Januari hingga November, namun gagal di 1 tiket pada awal Desember, maka rapor tahunan TI akan hancur karena pengguna hanya mengingat kegagalan terakhir.
* **Solusi Sistem (Transactional Survey):** Survei wajib dilakukan segera setelah sebuah tiket selesai (*After Call Survey* atau *Email Feedback* yang berisi ikon kepuasan).

### 4. Rekayasa Paksa Pengisian Survei (*Hard-Gate Logic*)

Tingkat pengisian survei oleh pengguna sering kali sangat rendah (di bawah 10%). Sebagai System Analyst, kita bisa menanamkan "trik paksaan" yang beretika ke dalam sistem.

* **Sistem Blokir Buka Tiket Baru:** Jika pengguna masih memiliki tiket berstatus "Selesai" namun belum diberikan *rating* survei, tombol "Buat Tiket Baru" di dalam portal aplikasi akan dinonaktifkan (*disabled*) oleh sistem. Pengguna dipaksa memberikan penilaian terhadap layanan sebelumnya agar bisa meminta layanan baru.

---

### VISUALISASI LOGIKA SISTEM (HARD-GATE CSAT ENGINE)

> **`[User Mengklik Tombol 'Create New Ticket']`**
> ├── **Sistem mengecek Database: Apakah ada riwayat tiket terakhir yang belum di-rating?**
> │
> ├── *YES (Ada tiket yang belum di-rating)*
> │   └── $\rightarrow$ **[ACTION: BLOCK]** Tampilkan Pop-up Survei: *"Silakan nilai layanan kami sebelumnya untuk membuka tiket baru."*
> │
> └── *NO (Semua tiket sudah beres)*
> └── $\rightarrow$ **[ACTION: ALLOW]** Buka Form Form Pembuatan Tiket Baru.

---

## GLOSARIUM: DEFINISI ISTILAH TEKNIS

1. **Event-Driven Architecture:** Desain arsitektur perangkat lunak di mana aksi tertentu (seperti pengiriman email) dipicu secara otomatis oleh terjadinya sebuah kejadian atau perubahan data (seperti status yang berubah).
2. **CSAT (Customer Satisfaction Score):** Metrik utama yang digunakan untuk mengukur tingkat kepuasan pengguna terhadap suatu layanan spesifik yang baru saja mereka terima.
3. **Recency Bias:** Bias kognitif di mana seseorang lebih mudah mengingat kejadian yang paling baru terjadi dibandingkan kejadian historis yang sudah lama berlalu.
4. **Hard-Gate System:** Logika pemrograman yang memberikan pemblokiran akses absolut (gerbang tertutup) hingga pengguna memenuhi prasyarat tertentu (seperti mengisi survei).
5. **Cronjob / Auto-Close:** Skrip otomatis di dalam *server* yang berjalan pada jadwal waktu tertentu untuk mengeksekusi perintah massal (contoh: otomatis menutup tiket yang sudah berstatus *Resolved* selama 3 hari tanpa respons pengguna).
6. **Outbound Call:** Panggilan keluar yang diinisiasi oleh pihak *Service Desk* kepada pengguna (berlawanan dengan *Inbound Call* di mana pengguna yang menelepon duluan).

---

## EVALUASI MANDIRI: SOAL UJI PEMAHAMAN

### Bagian A: Pilihan Ganda

1. **Mengapa aplikasi manajemen tiket harus dilengkapi dengan fitur notifikasi otomatis (*Event-Driven Notification*)?**
A. Agar sistem bisa menghitung SLA.
B. Untuk membebani *server* email.
C. Membebaskan *Service Desk* dari beban manual harus menginformasikan progres ke pengguna satu per satu setiap jam.
D. Untuk mencegah *user* membuat tiket baru.
2. **Dampak analitik apa yang akan terjadi jika departemen TI hanya menyebarkan survei kepuasan layanan setahun sekali di akhir tahun?**
A. Tingkat partisipasi survei akan mencapai 100%.
B. Hasilnya tidak valid karena terkena efek *Recency Bias*, di mana pengguna hanya mengingat insiden buruk yang paling baru terjadi.
C. Pengguna akan memberikan nilai yang sangat tinggi karena apresiasi.
D. Server *database* akan kelebihan beban.
3. **Cara paling efektif secara sistem (aplikasi) untuk meningkatkan persentase pengisian survei kepuasan pengguna adalah...**
A. Mengunci fitur pembuatan tiket baru sampai pengguna mengisi survei tiket lama.
B. Mendelegasikan pengisian survei kepada *Service Desk*.
C. Membuat kuesioner dengan 50 pertanyaan esai yang detail.
D. Menghapus status *Closed* dari aplikasi.

### Bagian B: Studi Kasus Analitik

1. **Analisis Risiko UX:** Sebagai QA, Anda menemukan *bug* di mana teknisi *Network* bisa mengeklik tombol **"Closed"** secara langsung tanpa melewati status **"Resolved"** dan tanpa ada konfirmasi dari pihak pengguna. Mengapa ini dianggap sebagai pelanggaran tata kelola IT (*IT Governance Violation*) yang serius?
2. Jelaskan logika operasional dari mekanisme *Auto-Close* (Penutupan Otomatis by Sistem) dalam membantu agen *Service Desk* jika pengguna tidak mau mengklik konfirmasi "Selesai"!

---

### KUNCI JAWABAN & PEMBAHASAN

**Bagian A (Pilihan Ganda):**

1. **C.** Automasi mengambil alih tugas repetitif (melapor progres), sehingga agen bisa fokus mendiagnosis dan menyelesaikan keluhan tiket lainnya (*Timeboxing* yang lebih efektif).
2. **B.** *Recency bias* menghapus memori tentang pelayanan yang baik berbulan-bulan yang lalu dan menyoroti kekecewaan yang paling baru, membuat rapor departemen TI tampak lebih buruk dari realitasnya.
3. **A.** Sistem *Hard-Gate*. Pemblokiran bersyarat ini memaksa pengguna untuk berpartisipasi dalam evaluasi layanan sebagai "harga" untuk mendapatkan dukungan TI selanjutnya.

**Bagian B (Studi Kasus Analitik):**

1. **Pelanggaran Hak Validasi:** Tiket yang ditutup (*Closed*) secara sepihak oleh teknisi berarti mencabut hak evaluasi pengguna (*User Acceptance*). Bisa jadi teknisi merasa masalah jaringan sudah beres dari layar pemantauan *server*, namun di meja karyawan jaringan tersebut masih terputus. Jika tiket ditutup sepihak, keluhan tidak tuntas, namun secara SLA sistem terlihat 100% sempurna. Ini menciptakan laporan kinerja yang fiktif/palsu.
2. **Logika Auto-Close:** Sering kali pengguna yang masalahnya sudah teratasi akan langsung kembali bekerja dan mengabaikan email konfirmasi dari sistem. Jika tidak di-*Auto-Close*, tiket ini akan menggantung selamanya berstatus *Resolved*, dan *Service Desk* tidak bisa mencapai target pembersihan tiket. Maka, sistem akan menjalankan penghitung waktu (misal: 72 jam). Jika dalam 72 jam sejak status *Resolved* tidak ada bantahan dari *user*, sistem akan menganggap solusi telah diterima dan mengunci tiket tersebut menjadi *Closed* secara otomatis.