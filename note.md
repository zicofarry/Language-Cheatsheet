> **Synchronous** = Jalankan satu per satu, secara serial/berurutan.
> **Asynchronous** = Jalankan tanpa menunggu, bisa lanjut ke tugas lain, secara paralel (bersamaan).

---

## 1. Perbandingan: Synchronous vs Asynchronous

| Fitur | Synchronous | Asynchronous |
|---|---|---|
| **Urutan** | Berurutan (*Serial*) | Tidak harus berurutan |
| **Waktu Tunggu** | Menunggu sampai selesai (*Blocking*) | Bisa lanjut ke tugas lain (*Non-blocking*) |
| **Efisiensi** | Rendah jika banyak tugas berat | Sangat tinggi untuk sistem jaringan |
| **Kompleksitas** | Mudah dipahami & di-*debug* | Lebih menantang (*callback/channel*) |

---

## 2. Perbandingan: Process vs Thread

| Fitur | Process (Proses) | Thread (Utas) |
|---|---|---|
| **Memori** | Terisolasi (punya ruang sendiri) | Berbagi memori (*Shared memory*) |
| **Overhead** | Berat (lambat dibuat/dipindah) | Ringan (cepat dibuat/dipindah) |
| **Stabilitas** | Jika satu crash, yang lain aman | Jika satu crash, satu proses bisa mati |
| **Komunikasi** | Lambat (harus lewat sistem OS) | Sangat cepat (akses variabel langsung) |

---

## 3. Goroutine (Spesial di Go)

Sebagai perbandingan tambahan untuk materi Go:

| Aspek | Thread OS | Goroutine |
|---|---|---|
| **Ukuran Memori** | ~2 MB | ~2 KB |
| **Kapasitas** | Ribuan | Jutaan |
| **Level** | Dikelola Kernel OS | Dikelola Go Runtime (*User Space*) |

---

## 4. Channel

```go
ch <- "hello"  // kirim data ke channel
msg := <-ch    // terima data dari channel
```
### Analogi Entitas
- Goroutine = Orang
- Channel = Jalan (Penghubung)
- Buffer = Loker


### Analogi Channel Unbuffered
> Seperti serah terima barang langsung — pengirim dan penerima harus ketemu di waktu yang sama.

- Kirim = ngasih barang ke orang langsung
- Nerima = menerima barang dari orang langsung
- Kalau penerima tidak ada → pengirim nunggu
- Kalau pengirim tidak ada → penerima nunggu

```go
func main() {
    ch := make(chan string) // unbuffered, tidak ada loker

    go func() {
        ch <- "barang" // goroutine A: ngasih barang langsung, nunggu ada yang nerima
    }()

    msg := <-ch          // goroutine main: nerima barang langsung dari A
    fmt.Println(msg)     // Output: barang
}
```

---

### Analogi Channel Buffered
> Seperti loker penitipan — pengirim bisa titip dulu, penerima ambil belakangan.

- Kirim = menaruh barang di loker
- Nerima = mengambil barang dari loker
- Kalau loker penuh → pengirim nunggu
- Kalau loker kosong → penerima nunggu

```go
func main() {
    ch := make(chan string, 2) // buffered, ada 2 slot loker

    ch <- "barang 1" // taruh di loker slot 1, tidak nunggu
    ch <- "barang 2" // taruh di loker slot 2, tidak nunggu
    // ch <- "barang 3" // loker penuh! → blocking

    fmt.Println(<-ch) // ambil dari loker: barang 1
    fmt.Println(<-ch) // ambil dari loker: barang 2
}
```

---

### Perbandingan: Unbuffered vs Buffered Channel

| Fitur | Unbuffered Channel | Buffered Channel |
|---|---|---|
| **Kapasitas** | Nol (0) | Ditentukan (misal: 2 atau lebih) |
| **Mekanisme** | Sinkron (harus ketemu langsung) | Asinkron (bisa titip di buffer) |
| **Sender Block** | Saat tidak ada penerima | Hanya saat buffer penuh |
| **Receiver Block** | Saat tidak ada pengirim | Hanya saat buffer kosong |

---

## 5. Konkurensi vs Paralelisme & GOMAXPROCS

Go punya variabel `GOMAXPROCS` yang menentukan berapa banyak thread OS yang bisa dipakai untuk menjalankan goroutine secara bersamaan.

```go
runtime.GOMAXPROCS(1) // hanya 1 thread → goroutine jalan bergantian (**concurrent**)
runtime.GOMAXPROCS(4) // 4 thread → goroutine bisa jalan benar-benar **paralel**
```

**Default-nya sejak Go 1.5** — `GOMAXPROCS` otomatis diset sesuai jumlah core CPU di mesin kamu. Jadi kalau CPU kamu punya 8 core, goroutine bisa benar-benar paralel di 8 thread sekaligus.

### 💡 Perbedaan Konsep

- **Concurrent** = banyak tugas dikelola bersamaan, tapi belum tentu jalan di waktu yang sama (bisa bergantian sangat cepat).
- **Parallel** = banyak tugas benar-benar jalan di waktu yang sama (butuh banyak core/thread OS).

> Goroutine itu selalu **concurrent**, dan bisa **paralel** kalau `GOMAXPROCS > 1` dan hardware mendukung.

---

### Analogi: Tukang Sulap (Juggler)

Agar lebih mudah dipahami, bayangkan seorang tukang sulap yang memainkan 3 bola:

1. **Tukang Sulap Konkuren**: Dia hanya punya dua tangan, tapi dia bisa memainkan tiga bola. Dia melempar satu bola ke atas, lalu menangkap bola lain, lalu melempar lagi. Di mata penonton, ketiga bola itu seolah-olah "sedang dimainkan bersamaan", padahal tangan si tukang sulap hanya menyentuh satu bola di satu waktu. Dia sangat ahli dalam mengelola transisi antar bola.

2. **Tukang Sulap Paralel**: Bayangkan ada dua orang tukang sulap yang berdiri berdampingan. Masing-masing memainkan bolanya sendiri secara bersamaan. Inilah paralelisme; tugas benar-benar dikerjakan oleh dua "mesin" (tangan/core) yang berbeda di waktu yang sama.