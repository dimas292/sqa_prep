# MATERI PEMBELAJARAN: MANAJEMEN LAYANAN TI (ITSM) - PART 4

## Sub-Topik: Logika SLA Berbasis Due Date & Arsitektur *Multi-Assignment*

### 1. Logika Sistem Pengukuran: SLA Berbasis *Due Date*

Biasanya, SLA (*Service Level Agreement*) berjalan maju semenjak tiket dibuat atau disetujui. Namun, pada tipe *Request* tertentu (misal: "Tolong siapkan proyektor untuk *Townhall Meeting* tanggal 20 Agustus jam 10 pagi"), kita menggunakan parameter tenggat waktu (*Due Date*).

**Analisis Logika Sistem (Reverse Timer):**

* **Mitos yang Salah:** SLA baru berjalan ketika teknisi mengambil (*pick-up*) tiket. -> **SALAH!**
* **Fakta Sistem:** Timer SLA sistem akan bekerja seperti argometer taksi otomatis. Sistem harus diprogram untuk melakukan "Hitung Mundur" (Reverse Calculation) dari *Due Date*.
* **Rumus Trigger SLA:** `Waktu Mulai SLA = Waktu Due Date - Durasi SLA`
* *Contoh Kasus:* Pengguna me-request laptop siap pakai untuk karyawan baru dengan *Due Date* 11 Juli pukul 10:00. Durasi SLA pemenuhan adalah 2 jam. Meskipun tiket sudah disetujui seminggu sebelumnya, argo timer kinerja TI **baru akan otomatis menyala** tepat pada 11 Juli pukul 08:00. Kinerja tidak diukur berdasarkan seberapa cepat mereka merespons seminggu yang lalu, melainkan kesiapan mereka 2 jam sebelum tenggat waktu.

### 2. Arsitektur Penugasan (Assignment): Incident vs Request

Bagaimana sebuah tiket didistribusikan ke dalam tim operasional? Ada perbedaan struktur database dan workflow yang sangat mencolok di sini.

#### A. Insiden: Konsep "Bola Panas" (*Single-Threaded*)

* **Logika Penanganan:** Tiket Insiden tidak boleh dipecah menjadi beberapa *task* paralel.
* **Alasan Operasional:** Tujuannya adalah resolusi secepat mungkin. Tiket insiden dilempar seperti "bola panas" (misal: L1 tidak bisa $\rightarrow$ lempar ke L2 Server $\rightarrow$ lempar ke L3 Network). Hanya ada **satu nomor tiket tunggal** yang berpindah tangan untuk menghindari usaha ganda (*redundant effort*) antar tim.

#### B. Request: Konsep "Work Order / Task" (*Multi-Threaded*)

* **Logika Penanganan:** Permintaan terencana (*Request*) dapat dikerjakan secara komprehensif, melibatkan beberapa grup kerja sekaligus (*Multiple Assignment*).
* **Pemecahan Tiket (Parent-Child):** Jika eksekusinya paralel (dikerjakan berbarengan) dan tidak saling bergantung, satu nomor tiket utama (*Parent Ticket*) harus dipecah menjadi beberapa *Sub-Task* atau *Work Order* (*Child Tickets*).
* *Contoh Kasus:* Permintaan infrastruktur ruang rapat baru.
* *Parent Ticket:* Pembuatan Ruang Rapat.
* *Task 1 (Tim IT-Network):* Tarik kabel LAN (SLA: 4 Jam).
* *Task 2 (Tim IT-Support):* Setup PC & Proyektor (SLA: 2 Jam).
* Kedua tim bekerja berbarengan (*Parallel*). Tiket utama baru akan berstatus **Completed** ketika semua *Task* di bawahnya selesai.



---

### VISUALISASI ALUR KERJA (WORKFLOW LOGICS)

**Visualisasi 1: SLA By Due Date (Timeline)**

> `(T-7 Hari)`: Tiket Dibuat $\rightarrow$ `(T-6 Hari)`: Tiket Diapprove Manajer $\rightarrow$ **[SLA BELUM BERJALAN]** $\rightarrow$ `(T-2 Jam)`: **[SLA OTOMATIS BERJALAN]** $\rightarrow$ `(T-0)`: **DUE DATE TERCAPAI (Target Selesai)**

**Visualisasi 2: Arsitektur Multi-Assignment (Request Fulfillment)**

> `[Tiket Utama: Request Onboarding]`
> ├── $\rightarrow$ `[Work Order A]: Setup Akses Email` (Tim Sistem Administrator)
> ├── $\rightarrow$ `[Work Order B]: Kirim Laptop Fisik` (Tim IT Asset)
> └── $\rightarrow$ `[Work Order C]: Setup Akses ERP` (Tim Aplikasi)

---

## GLOSARIUM: DEFINISI ISTILAH TEKNIS

1. **Due Date:** Tenggat waktu (tanggal dan jam yang spesifik) spesifik yang diminta oleh pengguna untuk sebuah layanan siap digunakan.
2. **Reverse Calculation (Hitung Mundur):** Logika pemrograman di mana dimulainya waktu hitung argo SLA tidak dihitung dari awal klik persetujuan, melainkan ditarik mundur dari titik akhir batas waktu (*Due Date*).
3. **Hot Potato Routing (Bola Panas):** Konsep alur kerja di mana sebuah masalah ditangani oleh satu orang/grup pada satu waktu, dan segera dilempar ke pihak lain jika mereka tidak mampu menyelesaikannya.
4. **Work Order / Task:** Tiket anakan (*child-ticket*) yang diturunkan dari tiket permohonan utama, berfungsi untuk membagi beban kerja secara terpisah namun masih terintegrasi.
5. **Serial vs Parallel Task:** *Serial* berarti pekerjaan diselesaikan satu per satu secara berurutan. *Parallel* berarti pekerjaan dilakukan berbarengan secara serentak oleh grup yang berbeda.

---

## EVALUASI MANDIRI: SOAL UJI PEMAHAMAN

### Bagian A: Pilihan Ganda

1. **Seorang atasan meminta laporan disiapkan untuk *Due Date* 15 Desember pukul 13:00. SLA yang ditetapkan sistem adalah 3 jam. Jika sistem dikonfigurasi dengan benar, kapan argo SLA untuk teknisi mulai berjalan?**
A. Segera setelah tiket dibuat.
B. Segera setelah atasan menyetujui tiket tersebut.
C. 15 Desember pukul 10:00.
D. Saat teknisi mengklik tombol "In Progress".
2. **Mengapa penanganan tiket *Incident* HINDARI penggunaan arsitektur tugas *Parallel*?**
A. Karena Insiden terlalu mudah diselesaikan.
B. Karena hal itu menciptakan pekerjaan ganda yang membuang waktu teknisi dalam menghadapi krisis yang seharusnya diselesaikan linear layaknya "bola panas".
C. Karena sistem database tidak mendukung tiket bertingkat.
D. Karena Insiden membutuhkan *Waiting Authorization*.
3. **Kapan diperbolehkannya sebuah *Request Management* hanya menggunakan satu nomor tiket (tanpa Work Order/Task tambahan) walau ditangani oleh beberapa tim berbeda?**
A. Apabila pengerjaannya bersifat paralel.
B. Apabila tidak ada tim yang mampu mengerjakannya.
C. Apabila pengerjaannya bersifat serial (satu selesai, baru dilempar ke yang lain secara berurutan).
D. Apabila SLA-nya berbasis *Due Date*.

### Bagian B: Studi Kasus Analitik

1. **Uji Logika QA:** Anda sedang menguji (*testing*) aplikasi ITSM yang baru dibuat oleh tim *developer* Anda. Skenarionya: SLA adalah 5 hari sebelum acara (*Due Date*). Tim teknis terlambat mengeklik tombol *"Pick up/Assign"* sampai H-2 acara. Sistem ternyata mengkalkulasi SLA belum berjalan karena teknisi belum *Pick-up*. Mengapa ini Anda identifikasi sebagai **BUG**, dan bagaimana seharusnya sistem berjalan?
2. Jelaskan syarat utama kapan sebuah *Request* wajib dipecah menjadi beberapa tiket turunan (*Work Orders* / *Tasks*)!

---

### KUNCI JAWABAN & PEMBAHASAN

**Bagian A (Pilihan Ganda):**

1. **C.** SLA otomatis berdetak mulai jam 10:00. Dihitung menggunakan rumus mundur: `13:00 - 3 Jam = 10:00`.
2. **B.** Penanganan gangguan (*Incident*) fokus pada kecepatan pemulihan tunggal. Membagi Insiden secara paralel hanya membingungkan teknisi soal siapa yang melakukan eksekusi, merusak konsep lemparan "bola panas".
3. **C.** Jika eksekusinya linear/serial berurutan, tiket tidak akan tumpang tindih waktu, sehingga bisa memakai nomor tiket yang sama secara beranting.

**Bagian B (Studi Kasus Analitik):**

1. Hal ini dikategorikan sebagai **BUG** (cacat logika sistem) karena argo *timer* SLA di sistem berbasis *Due Date* harus terpicu **secara otomatis (by system clock)** tepat 5 hari sebelum *Due Date*, bukan berdasarkan aksi manual klik *"Pick Up"* oleh teknisi. Jika menunggu klik teknisi, performa buruk teknisi (terlambat mengambil tugas) tidak akan terdeteksi di dalam laporan karena argo waktu SLA menunggunya.
2. Sebuah tiket *Request* wajib dipecah menjadi *Work Order/Tasks* ketika pekerjaan tersebut membutuhkan **penanganan paralel** (dikerjakan dalam waktu bersamaan) oleh dua atau lebih *Solver Group* (grup penyelesai) yang berbeda. Jika tidak dipecah, SLA penyelesaian dari tiap-tiap departemen tidak akan bisa dilacak dan diukur dengan akurat.