# MATERI PEMBELAJARAN: MANAJEMEN LAYANAN TI (ITSM) - PART 8

## Sub-Topik: Batasan Wewenang, *First Contact Resolution*, dan *Timeboxing*

### 1. Validasi Input dan Batasan Ruang Lingkup (*Scope Boundaries*)

Aplikasi tiketing harus memiliki validasi input yang ketat terkait "siapa menangani apa". Jika pengguna salah memilih kategori, sistem atau agen *Service Desk* harus merutekan ulang (*reroute*) tiket tersebut ke pihak yang tepat.

Berikut adalah matriks batasan wewenang untuk logika aplikasi:

| Jenis Tiket / Permintaan | Entitas Penanggung Jawab | Analisis Logika Sistem |
| --- | --- | --- |
| **Incident** (Gangguan) & **Request** (Permintaan Standar) | **Service Desk** | Wewenang utama *Service Desk* untuk dicatat, didiagnosis, dan diselesaikan (jika mampu). |
| **Inquiry** (Tanya Jawab / Informasi) | **Service Desk** | Pengguna hanya butuh panduan (misal: "Bagaimana cara konek WiFi?"). |
| **Problem** (Akar Masalah) | **Problem Manager** | *Service Desk* DILARANG menangani investigasi akar masalah agar waktu antrean tidak tersumbat. |
| **New Application / Enhancement** (Pengembangan Fitur Baru) | **Demand Manager / Business Analyst** | Ini bukan operasional, melainkan proyek pengembangan. *Service Desk* harus memblokir/menolak tiket operasional dan mengarahkannya ke jalur *Project/Demand Management*. |

### 2. Agen Investigasi (Level 1) vs. Dispatcher Pasif

*Service Desk* yang baik bukanlah sekadar **Dispatcher** (petugas penyalur tiket).

* **Dispatcher Pasif:** Hanya melihat judul tiket dan langsung melemparnya ke tim belakang (*forwarder*). Jika perannya hanya ini, fungsi manusia bisa 100% digantikan oleh robot *routing* (IVR/ACD).
* **Investigasi Awal (First Line Diagnosis):** Agen wajib melakukan *troubleshooting* dasar (misal: "Apakah sudah cek kabel LAN?", "Apakah ada notifikasi *error code* di layar?"). Ini berfungsi sebagai filter (*firewall*) agar teknisi di belakang (Tier 2) menerima tiket dengan data teknis yang sudah matang dan valid, bukan tiket "kosong".

### 3. Algoritma *Timeboxing* dan FCR (*First Contact Resolution*)

Ini adalah aturan emas dalam arsitektur manajemen antrean: **Jangan biarkan Service Desk memegang tiket terlalu lama.**

* *Service Desk* dituntut untuk mencapai *First Contact Resolution* (FCR), yaitu menyelesaikan masalah saat pengguna pertama kali menelepon/melapor.
* **Konsep Timeboxing:** Namun, penyelesaian ini dibatasi oleh waktu (misal: **15 - 30 menit**). Jika dalam 15 menit agen gagal menyelesaikan insiden (karena terlalu rumit), sistem mewajibkan agen untuk **Stop & Escalate** (Hentikan dan Eskalasi) tiket tersebut ke Level 2 (Spesialis).
* **Alasan Analitik:** Jika satu agen memaksakan diri memperbaiki satu tiket rumit selama 2 jam, ia akan mengabaikan 20 panggilan masuk lainnya. Tingkat pembatalan (*Abandonment Rate*) panggilan akan meroket, dan SLA keseluruhan perusahaan akan hancur.

---

### VISUALISASI LOGIKA SISTEM (DECISION TREE TIKET)

> **`[Tiket Masuk ke Service Desk]`**
> ├── **Apakah ini Permintaan Aplikasi Baru/Enhancement?**
> │   └── *YES* $\rightarrow$ **[REJECT TIKET]** $\rightarrow$ Arahkan ke *Demand Manager / BA*.
> │
> └── *NO* $\rightarrow$ **Apakah ini Insiden/Request Standar?**
> └── *YES* $\rightarrow$ Lakukan Investigasi Awal (Level 1 Diagnosis)
> │
> ├── **Bisa diselesaikan dalam batas waktu (< 15 Menit)?**
> │   └── *YES* $\rightarrow$ **[SOLVED at First Contact / FCR]** $\rightarrow$ *Close* Tiket.
> │
> └── *NO* $\rightarrow$ Waktu Habis / Terlalu Rumit! $\rightarrow$ **[ESCALATE]** $\rightarrow$ Lempar ke Level 2 (Spesialis/ERP/Network). $\rightarrow$ *Service Desk* kembali menerima telepon baru.

---

## GLOSARIUM: DEFINISI ISTILAH TEKNIS

1. **Demand Manager:** Peran strategis (sering diisi oleh *Business Analyst*) yang bertugas menganalisis, menyetujui, dan memprioritaskan permintaan proyek TI atau fitur aplikasi baru dari unit bisnis.
2. **Enhancement:** Peningkatan, modifikasi, atau penambahan fitur baru pada aplikasi atau layanan TI yang sudah ada.
3. **Inquiry:** Pertanyaan, pencarian informasi, atau konsultasi ringan dari pengguna yang tidak melibatkan kerusakan sistem (bukan *Incident*) dan bukan permintaan barang (bukan *Request*).
4. **First Contact Resolution (FCR):** Metrik kinerja (*Key Performance Indicator*) yang mengukur persentase tiket atau panggilan pelanggan yang berhasil diselesaikan pada interaksi pertama, tanpa perlu eskalasi atau telepon ulang.
5. **Dispatcher / Forwarder:** Petugas atau sistem yang tugasnya hanya menerima permintaan dan meneruskannya (mendistribusikannya) ke pihak lain tanpa melakukan analisis atau perbaikan apa pun.
6. **Timeboxing:** Teknik manajemen waktu di mana sebuah tugas diberikan batas waktu maksimum yang tetap dan ketat. Jika waktu habis, tugas harus dihentikan, diserahkan, atau dievaluasi ulang.

---

## EVALUASI MANDIRI: SOAL UJI PEMAHAMAN

### Bagian A: Pilihan Ganda

1. **Seorang Kepala Divisi HRD menelepon *Service Desk* dan meminta agar departemen TI membuatkan modul aplikasi baru untuk menghitung lembur karyawan. Tindakan paling tepat sesuai kerangka kerja ITSM yang harus dilakukan agen *Service Desk* adalah...**
A. Langsung mengarahkan (*assign*) tiket ke *Programmer* / L2 *Developer*.
B. Menolak permintaan aplikasi baru dan mengarahkannya ke *Demand Manager / Business Analyst*.
C. Membuatkan tiket *Problem Management*.
D. Mengubah statusnya menjadi *Waiting Authorization*.
2. **Mengapa agen *Service Desk* sangat dilarang untuk menghabiskan waktu berjam-jam mencoba menyelesaikan sebuah insiden rumit secara mandiri?**
A. Karena mereka tidak dibayar untuk itu.
B. Karena hal tersebut akan memblokir antrean, menyebabkan pengguna lain tidak terlayani (*Abandonment Rate* naik).
C. Karena dapat merusak CMDB.
D. Karena insiden tersebut pasti akan batal (*Canceled*) dengan sendirinya.
3. **Indikator (KPI) yang mengukur keberhasilan agen *Service Desk* dalam menyelesaikan masalah saat pengguna pertama kali menelepon (tanpa dilempar ke teknisi belakang) disebut...**
A. Completion Time
B. SLA Timer
C. Timeboxing
D. First Contact Resolution (FCR)

### Bagian B: Studi Kasus Analitik

1. **Uji Skenario *Routing*:** Sebuah perusahaan memasang sistem tiket di mana setiap kali *user* memilih menu "Jaringan", tiket otomatis 100% langsung masuk antrean ke Tim *Network Engineer* tanpa melewati *Service Desk*. Mengapa arsitektur "Otomasi *Dispatcher*" ini berisiko tinggi menurunkan produktivitas *Engineer*? Jelaskan!
2. Jelaskan perbedaan mendasar antara **Inquiry** dan **Incident**!

---

### KUNCI JAWABAN & PEMBAHASAN

**Bagian A (Pilihan Ganda):**

1. **B.** *Service Desk* hanya melayani operasional (menjaga sistem yang sudah ada). Permintaan perangkat lunak baru adalah proyek/inisiatif (*Demand*), sehingga harus ditangani oleh *Demand Manager/BA* untuk dianalisis kelayakan bisnisnya.
2. **B.** *Service Desk* bertugas sebagai penjaga gerbang utama. Jika satu-satunya penjaga pintu terlalu sibuk memperbaiki satu masalah rumit (mengabaikan aturan *Timeboxing*), puluhan pelanggan lain di belakangnya akan terbengkalai dan tidak bisa melaporkan kerusakan lainnya.
3. **D.** *First Contact Resolution* (FCR).

**Bagian B (Studi Kasus Analitik):**

1. Risiko tertingginya adalah **Validasi Sampah (*Garbage In*)**. Banyak *user* awam tidak paham teknis. Sering kali mereka melaporkan "Internet Jaringan Mati", namun setelah dicek ternyata laptop mereka dalam mode *Airplane* atau kabel *LAN* belum dicolok. Jika sistem bertindak sebagai otomatisasi *dispatcher* tanpa validasi (*troubleshooting* awal level 1 oleh agen manusia di *Service Desk*), *Network Engineer* (yang bergaji mahal) akan membuang-buang waktu mengurusi "kabel belum dicolok" alih-alih fokus mengatur *router core* perusahaan.
2. **Incident** adalah gangguan/kerusakan teknis di mana sistem yang seharusnya berjalan normal ternyata bermasalah (misal: *Error* 404, tidak bisa cetak dokumen). Sedangkan **Inquiry** adalah kondisi di mana sistem berjalan 100% normal, tetapi pengguna tidak tahu cara menggunakannya dan butuh panduan informasi (misal: "Bagaimana cara mengganti *background* presentasi?", "Di mana saya bisa melihat *history* transaksi?").