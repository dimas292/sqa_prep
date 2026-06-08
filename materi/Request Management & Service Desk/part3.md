# MATERI PEMBELAJARAN: MANAJEMEN LAYANAN TI (ITSM) - PART 3

## Sub-Topik: State Machine Logics dan Arsitektur Penghitungan SLA

### 1. Perbandingan Status Tiket (Workflow States): Request vs Incident

Sistem tiketing harus membatasi pilihan status (*dropdown list*) berdasarkan jenis tiketnya. Jika pengguna membuat tiket **Insiden**, sistem harus memblokir atau menyembunyikan status seperti *Waiting Authorization* atau *Rejected*.

Berikut adalah matriks validasi sistem untuk pengujian QA:

| Nama Status | Request Management | Incident Management | Analisis Logika Sistem |
| --- | --- | --- | --- |
| **In Review** | Ada | Ada | Pengecekan awal kelengkapan data oleh *Service Desk*. |
| **Waiting Authorization** | Ada | **TIDAK ADA** | Insiden (*unplanned interruption*) harus segera ditangani, dilarang ditahan oleh birokrasi *approval*. |
| **Rejected** | Ada | **TIDAK ADA** | Permintaan (*request*) bisa ditolak oleh atasan/sistem. Insiden tidak bisa "ditolak", karena itu adalah gangguan nyata. |
| **Canceled** | Ada | Ada | Bisa dilakukan oleh pengguna. Contoh Insiden di-*cancel*: User melapor error, ternyata *Capslock* menyala. |
| **Terminasi Kerja** | **Completed** | **Resolved** | *Request* sifatnya **dipenuhi** (*completed*). *Incident* sifatnya **disembuhkan** (*resolved*). |
| **Closed** | Ada | Ada | Tiket ditutup permanen *setelah* pengguna menyetujui bahwa permintaan/insiden telah beres. |

---

### 2. Arsitektur Penghitungan Waktu SLA (The SLA Clock)

Kapan sistem (aplikasi tiketing) mulai menghitung argumen waktu (SLA Timer)?
Sebagai analis, kita tidak boleh mendesain SLA TI dihitung sejak tiket pertama kali *disubmit* jika tiket tersebut membutuhkan persetujuan manajemen (*approval*).

**Aturan Emas SLA Request:** Timer SLA (*Fulfillment Time*) untuk tim TI **TIDAK BOLEH BERJALAN** selama tiket berada dalam status *Waiting Authorization*. Tim TI tidak bisa mengontrol berapa lama seorang manajer melakukan *approval*.

Terdapat dua pola rancangan sistem (Skenario) berdasarkan kompleksitas tiket:

#### Skenario 1: Verifikasi Service Desk di Awal (Validasi Berlapis)

Digunakan untuk tiket yang kompleks di mana TI harus mengecek kelengkapan lampiran (dokumen) sebelum meminta persetujuan ke manajemen.

> **Flowchart Sistem:**
> `New Ticket` $\rightarrow$ `Service Desk (Verifikasi/Review)` $\rightarrow$ **`[Sistem Minta Approval]`** $\rightarrow$ *(Tunggu Manajer)* $\rightarrow$ **`[Approved]`** $\rightarrow$ **(SLA TIM TI MULAI BERJALAN)** $\rightarrow$ `In Progress` $\rightarrow$ `Completed` $\rightarrow$ `Closed`

#### Skenario 2: Approval di Awal (Bypass Routing)

Digunakan untuk tiket yang sederhana dan sudah terstandarisasi. Tidak perlu verifikasi teknis, langsung dikirimkan oleh sistem ke atasan peminta.

> **Flowchart Sistem:**
> `New Ticket` $\rightarrow$ **`[Sistem Auto-Route minta Approval]`** $\rightarrow$ *(Tunggu Manajer)* $\rightarrow$ **`[Approved]`** $\rightarrow$ `Service Desk (Assign Team)` $\rightarrow$ **(SLA TIM TI MULAI BERJALAN)** $\rightarrow$ `In Progress` $\rightarrow$ `Completed` $\rightarrow$ `Closed`

---

## GLOSARIUM: DEFINISI ISTILAH TEKNIS

1. **State Machine:** Konsep dalam desain perangkat lunak di mana sebuah sistem (seperti tiket) hanya bisa berada pada satu kondisi (status) tertentu pada satu waktu, dan perubahannya diatur oleh aturan yang ketat.
2. **In Review:** Fase di mana tiket sedang diperiksa kelengkapannya oleh agen *Service Desk* (misalnya: apakah dokumen *requirement*-nya sudah lengkap, deskripsinya jelas, dsb).
3. **Routing / Sirkuler:** Otomatisasi pengiriman notifikasi persetujuan (biasanya berjenjang) di dalam sistem aplikasi kepada pihak-pihak yang memiliki otoritas (*Approver*).
4. **Completed vs Resolved:** *Completed* merujuk pada "Pemenuhan" (barang/akses sudah diberikan dalam proses Request). *Resolved* merujuk pada "Resolusi" (sistem yang rusak sudah kembali normal dalam proses Incident).
5. **Closed:** Status pamungkas (*terminal state*). Tiket berpindah ke *Closed* hanya setelah **pengguna** (*end-user*) memvalidasi dan menyetujui bahwa hasil pekerjaan tim TI sudah sesuai.

---

## EVALUASI MANDIRI: SOAL UJI PEMAHAMAN

### Bagian A: Pilihan Ganda

1. **Mengapa timer SLA *Fulfillment Time* pada sebuah *Request* baru mulai berjalan SETELAH tiket di-*approve* (disetujui)?**
A. Karena aplikasi tiketing tidak bisa menghitung waktu di hari libur.
B. Karena *Service Desk* harus memperbaiki insiden terlebih dahulu.
C. Karena waktu yang dibutuhkan manajer untuk menyetujui (*approval time*) berada di luar kendali performa operasional tim TI.
D. Karena *Request* memiliki prioritas yang selalu lebih rendah dari *Incident*.
2. **Seorang staf melaporkan bahwa komputernya tidak bisa menyala dan mengirimkan tiket Insiden. Sepuluh menit kemudian, staf tersebut menyadari colokan listriknya terlepas. Status apa yang tepat untuk kasus tiket ini?**
A. Rejected (Ditolak)
B. Canceled (Dibatalkan oleh pengguna)
C. Suspended (Ditangguhkan)
D. Waiting Authorization (Menunggu otorisasi)
3. **Perusahaan Anda mengimplementasikan "Skenario 1" untuk pembuatan Hak Akses VPN. Urutan yang paling tepat dari proses tersebut adalah...**
A. Tiket Dibuat $\rightarrow$ Manajer Approve $\rightarrow$ Service Desk Cek Kelengkapan $\rightarrow$ Selesai
B. Tiket Dibuat $\rightarrow$ Service Desk Cek Kelengkapan $\rightarrow$ Manajer Approve $\rightarrow$ Dikerjakan TI $\rightarrow$ Selesai
C. Tiket Dibuat $\rightarrow$ Dikerjakan TI $\rightarrow$ Manajer Approve $\rightarrow$ Selesai
D. Tiket Dibuat $\rightarrow$ Suspended $\rightarrow$ Selesai

### Bagian B: Studi Kasus Analitik

1. Sebagai seorang *QA Tester*, Anda menemukan sebuah *bug* di aplikasi *Service Desk* internal perusahaan. Anda berhasil mengubah status tiket **Incident: Database Down** menjadi **Waiting Authorization**. Jelaskan secara logis dan operasional mengapa hal ini dikategorikan sebagai *bug/error* berisiko tinggi (*high-severity*)!
2. Mengapa status tiket belum berhenti sepenuhnya saat tim teknis menyetelnya ke mode **Completed** atau **Resolved**? Mengapa harus ada status **Closed** setelahnya?

---

### KUNCI JAWABAN & PEMBAHASAN

**Bagian A (Pilihan Ganda):**

1. **C.** Tim teknisi TI tidak dapat dikontrol oleh kelambatan birokrasi *approval* pengguna/manajemen. SLA tim TI hanya mengukur kecepatan kerja teknis mereka sendiri.
2. **B. Canceled.** Insiden tersebut bukan kesalahan sistem, melainkan *user error* yang disadari pengguna, sehingga tiket dibatalkan (*canceled*). Tiket insiden tidak memiliki status *Rejected*.
3. **B.** *Skenario 1* memposisikan *Service Desk* di depan untuk memfilter *typo* atau dokumen yang kurang dari formulir VPN sebelum form tersebut mengganggu atasan untuk minta izin (*approval*).

**Bagian B (Studi Kasus Analitik):**

1. Hal tersebut adalah *bug* berisiko tinggi karena sistem yang mengalami kerusakan masif (seperti *Database Down*) adalah **Insiden** yang membutuhkan perbaikan instan. Jika tiket Insiden bisa diubah menjadi *Waiting Authorization*, operasional pemulihan sistem bisa terhenti hanya karena menunggu persetujuan (birokrasi), yang akan memperpanjang waktu matinya layanan (*downtime*) dan merugikan bisnis perusahaan.
2. Karena **Completed** atau **Resolved** hanyalah klaim sepihak dari tim teknis (TI) bahwa pekerjaan "telah selesai". Status **Closed** mewajibkan persetujuan pengguna. Hal ini berguna sebagai jaring pengaman untuk memastikan bahwa pengguna benar-benar merasa masalahnya hilang atau permintaannya sesuai sebelum tiket disegel secara sistem.