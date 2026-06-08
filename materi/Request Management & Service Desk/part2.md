# MATERI PEMBELAJARAN: MANAJEMEN LAYANAN TI (ITSM) - PART 2

## Sub-Topik: Eskalasi Problem, SLA, dan Workflow Request Management

### 1. Kriteria Eskalasi: Insiden vs. Problem

Sistem tiketing yang baik tidak bergantung pada tebakan, melainkan pada logika terprogram (kriteria yang didefinisikan).

* **Pola Berulang (Threshold-Based):** Insiden akan dieskalasi menjadi tiket *Problem* jika memenuhi kriteria perulangan (*threshold*) yang sudah dikonfigurasi dalam sistem.
* *Contoh Logika:* Jika `jumlah_komplain >= 10` DAN `durasi <= 5 hari` DARI `User_ID = sama` $\rightarrow$ *Trigger Problem Ticket*.


* **Known Error vs. Unknown Error:**
* **Insiden (Known Cause):** Server mati karena *hard disk error*. Akar masalahnya sudah jelas, solusinya pasti (ganti perangkat). Tidak perlu diinvestigasi sebagai tiket *Problem*.
* **Problem (Unknown Cause):** Server lambat tanpa sebab yang jelas. Admin tidak bisa menebak, sehingga **wajib** dicatat sebagai tiket *Problem* untuk melakukan investigasi menyeluruh demi mencari solusi permanen (*Permanent Solution*).


* **Proaktif vs. Reaktif:**
* Jika admin memonitor lampu indikator router/switch berwarna *orange* (tidak normal) tanpa ada laporan pengguna, ini ditangani secara proaktif sebagai **Problem**.
* Namun, jika efek lambatnya sudah dirasakan oleh pengguna dan pengguna melapor ke *Service Desk*, maka laporan pengguna tersebut dicatat sebagai **Insiden**.



### 2. SLA: Mengapa Problem Management Tidak Diukur SLA Resolusi?

Terdapat perbedaan fundamental dalam *Service Level Agreement* (SLA) antara *Incident*, *Request*, dan *Problem*.

* **Problem Management:** **TIDAK** diukur menggunakan SLA Resolusi waktu penyelesaian. Alasannya: Target *Problem Management* adalah menemukan **Akar Masalah (Root Cause)** dan **Solusi Permanen**. Analisis mendalam tidak bisa selalu dibatasi oleh waktu yang ketat. Jika dipaksakan dengan SLA ketat, analisis bisa menjadi dangkal dan masalah berpotensi terulang.
* **Request Management:** Menggunakan *Fulfillment Time* atau *Completion Time*, **bukan** *Resolution Time*. Karena *Request* adalah permintaan terencana (*planned*), tim TI tidak perlu "menyelesaikan" gangguan, melainkan "memenuhi" permintaan sesuai waktu wajar yang disepakati.

### 3. Workflow Tracking: Service Request

Alur kerja pemenuhan permintaan lebih kompleks dan sarat akan proses persetujuan (birokrasi) dibandingkan penanganan insiden.

**Visualisasi Siklus Tiket (Status Flow):**
`Draft` $\rightarrow$ `In Review` $\rightarrow$ `Waiting Authorization` $\rightarrow$ `In Progress` $\rightarrow$ `Completed` $\rightarrow$ `Closed`

**Pencabangan Status Pengecualian:**

* $\rightarrow$ `Rejected` (Ditolak oleh atasan/sistem)
* $\rightarrow$ `Canceled` (Dibatalkan oleh peminta)
* $\rightarrow$ `Suspended` (Ditunda karena alasan tertentu)

**Analisis Sistem QA:** Status `Draft`, `Waiting Authorization`, dan `Rejected` **TIDAK ADA** di dalam sistem *Incident Management*. Seseorang yang mengalami gangguan (Insiden) tidak perlu "meminta izin" agar sistemnya diperbaiki, sehingga tidak butuh otorisasi (Authorization) atau penolakan (Rejected).

---

## GLOSARIUM: DEFINISI ISTILAH TEKNIS

1. **SLA (Service Level Agreement):** Perjanjian resmi antara penyedia layanan TI dan pelanggan mengenai tingkat kualitas, ketersediaan, dan tanggung jawab layanan (diukur dalam durasi waktu).
2. **OLA (Operational Level Agreement):** Perjanjian teknis *internal* di antara berbagai grup di dalam departemen TI (misal: antara tim Jaringan dan tim Server) untuk memastikan mereka bersama-sama dapat memenuhi SLA pelanggan.
3. **Permanent Solution:** Solusi akhir yang diterapkan pada infrastruktur atau perangkat lunak untuk memastikan sebuah masalah (*root cause*) dihilangkan secara permanen.
4. **Fulfillment Time / Completion Time:** Waktu maksimal yang disepakati untuk memenuhi sebuah permintaan (*Request*) sejak diajukan hingga barang/layanan diserahkan.
5. **Waiting Authorization:** Status pada tiket di mana pelaksanaan kerja ditahan sementara hingga mendapatkan persetujuan manajerial atau persetujuan biaya dari pihak yang berwenang.
6. **Draft:** Status awal di mana pengguna baru menyusun formulir permintaan namun belum menekan tombol kirim (*submit*), biasanya digunakan jika pengguna masih mencari detail informasi pendukung.

---

## EVALUASI MANDIRI: SOAL UJI PEMAHAMAN

### Bagian A: Pilihan Ganda

1. **Sebuah server tiba-tiba mati total. Tim IT mengecek ruang server dan menemukan bahwa ada kabel power yang terbakar secara fisik. Tindakan apa yang paling tepat secara kerangka kerja ITSM?**
A. Membuka tiket Problem untuk mencari tahu siapa yang memasang kabel.
B. Menutup layanan Service Desk sementara.
C. Mencatatnya sebagai Insiden dan langsung mengganti kabel tersebut.
D. Memasukkan tiket ke status Waiting Authorization untuk membeli kabel.
2. **Perbedaan utama pengukuran metrik SLA antara *Incident* dan *Request* terletak pada...**
A. Insiden diukur dengan OLA, Request diukur dengan SLA.
B. Insiden menggunakan *Resolution Time*, Request menggunakan *Fulfillment/Completion Time*.
C. Keduanya sama-sama menggunakan *Response Time* sebagai metrik akhir.
D. Request tidak memerlukan SLA karena terencana.
3. **Status tiket mana di bawah ini yang HANYA logis eksis di proses *Request Management*, tetapi TIDAK di *Incident Management*?**
A. In Progress
B. Closed
C. Suspended
D. Waiting Authorization

### Bagian B: Studi Kasus Analitik

1. Sebagai System Analyst, Anda mendapati tiket dari Admin Database. Admin tersebut melihat *query execution time* berjalan 5 detik lebih lambat dari standarnya di malam hari. Tidak ada user yang melapor karena ini terjadi pukul 2 dini hari. Kategori tiket apa yang seharusnya dibuka (Incident atau Problem)? Jelaskan alasan teknis Anda!
2. Mengapa indikator metrik SLA tidak efektif jika diterapkan secara kaku pada penyelesaian *Problem Management*?

---

### KUNCI JAWABAN & PEMBAHASAN

**Bagian A (Pilihan Ganda):**

1. **C.** Mencatatnya sebagai Insiden dan langsung mengganti kabel tersebut. Akar masalahnya sudah jelas (*Known error* - kabel terbakar), solusinya pasti, sehingga tugas IT adalah mengembalikan layanan normal (*Incident response*) secepat mungkin.
2. **B.** *Resolution Time* vs *Fulfillment Time*. Gangguan harus "diresolusi" (disembuhkan), sedangkan permintaan harus "dipenuhi" (di-*fulfill*).
3. **D.** *Waiting Authorization*. Pemulihan dari kegagalan sistem (Insiden) tidak memerlukan prosedur persetujuan finansial/manajerial, IT harus langsung memperbaikinya. Persetujuan hanya ada pada proses permintaan barang/akses baru.

**Bagian B (Studi Kasus Analitik):**

1. **Problem Ticket**. Alasan teknis: Pertama, penyebab melambatnya *query* masih berstatus *Unknown Error* (belum diketahui mengapa itu terjadi). Kedua, ini adalah deteksi proaktif oleh admin tanpa ada komplain/interupsi langsung dari operasional normal pengguna (*Unplanned interruption* belum dirasakan pengguna ujung). Tujuan tiket ini adalah mencari akar masalah mengapa performa degradasi setiap pukul 2 dini hari.
2. Karena penyelesaian *Problem Management* bersifat investigatif (*Root Cause Analysis*). Pemecahan teka-teki masalah kompleks tidak memiliki pola waktu yang pasti. Memaksakan batas waktu penyelesaian yang ketat akan membuat teknisi mengaplikasikan solusi instan (*workaround* sementara), bukan mencari **Solusi Permanen**.