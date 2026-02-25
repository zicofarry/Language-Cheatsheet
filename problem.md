# Permasalahan Goroutine: Goroutine Leak & Lainnya

> **Goroutine Leak** terjadi ketika goroutine dibuat tapi tidak pernah selesai (berjalan selamanya di background), sehingga menghabiskan memori dan resource.

---

## 1. Goroutine Leak: Channel Tidak Pernah Dibaca

### ❌ Kode Bermasalah

```go
func main() {
    ch := make(chan string)

    go func() {
        ch <- "hello" // goroutine mengirim data...
    }()

    // MASALAH: tidak ada yang membaca dari channel!
    // goroutine akan nunggu selamanya → LEAK
    fmt.Println("Main selesai")
}
```

### 🔍 Penjelasan Masalah
- Goroutine mengirim `"hello"` ke **unbuffered channel**.
- Tidak ada goroutine lain yang membaca (`<-ch`), sehingga goroutine **terblokir selamanya**.
- Goroutine tetap hidup di memori → **Goroutine Leak**.

### ✅ Solusi: Pastikan Ada Penerima

```go
func main() {
    ch := make(chan string)

    go func() {
        ch <- "hello"
    }()

    msg := <-ch // baca dari channel, goroutine bisa selesai
    fmt.Println(msg)
}
```

---

## 2. Goroutine Leak: Channel Tidak Pernah Dikirim

### ❌ Kode Bermasalah

```go
func main() {
    ch := make(chan string)

    go func() {
        msg := <-ch // goroutine menunggu data...
        fmt.Println(msg)
    }()

    // MASALAH: tidak ada yang mengirim ke channel!
    // goroutine nunggu selamanya → LEAK
    fmt.Println("Main selesai")
}
```

### 🔍 Penjelasan Masalah
- Goroutine menunggu data dari channel, tapi **tidak ada pengirim**.
- Goroutine **terblokir selamanya** menunggu data yang tidak akan pernah datang.

### ✅ Solusi: Kirim Data atau Tutup Channel

```go
func main() {
    ch := make(chan string)

    go func() {
        msg := <-ch
        fmt.Println(msg)
    }()

    ch <- "hello" // kirim data agar goroutine bisa lanjut
}
```

---

## 3. Goroutine Leak: Loop Tak Terbatas Tanpa Exit

### ❌ Kode Bermasalah

```go
func worker(ch chan int) {
    for {
        val := <-ch
        fmt.Println("Received:", val)
        // MASALAH: loop ini tidak pernah berhenti!
        // meskipun main sudah selesai, goroutine tetap jalan
    }
}

func main() {
    ch := make(chan int, 5)

    go worker(ch)

    for i := 0; i < 3; i++ {
        ch <- i
    }

    fmt.Println("Main selesai")
    // goroutine worker masih menunggu data dari channel → LEAK
}
```

### 🔍 Penjelasan Masalah
- `worker` berjalan dalam loop `for` tanpa batas.
- Setelah data habis, goroutine **terus menunggu** data baru dari channel.
- Jika channel tidak ditutup, goroutine tidak pernah tahu kapan harus berhenti.

### ✅ Solusi: Tutup Channel + Gunakan `range`

```go
func worker(ch chan int) {
    for val := range ch { // otomatis berhenti saat channel ditutup
        fmt.Println("Received:", val)
    }
    fmt.Println("Worker selesai")
}

func main() {
    ch := make(chan int, 5)

    go worker(ch)

    for i := 0; i < 3; i++ {
        ch <- i
    }

    close(ch) // tutup channel → worker tahu kapan berhenti
    time.Sleep(time.Second) // beri waktu worker menyelesaikan tugasnya
}
```

---

## 4. Deadlock: Semua Goroutine Saling Menunggu

### ❌ Kode Bermasalah

```go
func main() {
    ch := make(chan int)

    ch <- 42 // MASALAH: mengirim ke unbuffered channel di goroutine yang sama
    // tidak ada goroutine lain untuk menerima → DEADLOCK!

    fmt.Println(<-ch)
}
```

**Output:**
```
fatal error: all goroutines are asleep - deadlock!
```

### 🔍 Penjelasan Masalah
- `ch <- 42` memblokir goroutine `main` karena menunggu penerima.
- Tapi `fmt.Println(<-ch)` (penerima) ada **di bawahnya** dalam goroutine yang sama.
- Pengirim menunggu penerima, penerima tidak akan pernah tercapai → **Deadlock**.

### ✅ Solusi: Kirim dari Goroutine Terpisah

```go
func main() {
    ch := make(chan int)

    go func() {
        ch <- 42 // kirim dari goroutine terpisah
    }()

    fmt.Println(<-ch) // main menerima data
}
```

---

## 5. Race Condition: Akses Variabel Bersamaan

### ❌ Kode Bermasalah

```go
func main() {
    counter := 0

    for i := 0; i < 1000; i++ {
        go func() {
            counter++ // MASALAH: banyak goroutine mengakses counter bersamaan!
        }()
    }

    time.Sleep(time.Second)
    fmt.Println("Counter:", counter)
    // Output tidak konsisten! Bisa 980, 995, atau angka acak lainnya
}
```

### 🔍 Penjelasan Masalah
- 1000 goroutine mengakses dan mengubah `counter` secara **bersamaan**.
- Operasi `counter++` bukan operasi atomik (baca → tambah → tulis).
- Goroutine bisa **menimpa** hasil goroutine lain → **Race Condition**.

### ✅ Solusi: Gunakan `sync.Mutex` atau `sync/atomic`

```go
func main() {
    var counter int64 = 0

    var wg sync.WaitGroup

    for i := 0; i < 1000; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            atomic.AddInt64(&counter, 1) // operasi atomik, aman!
        }()
    }

    wg.Wait()
    fmt.Println("Counter:", counter) // Output: 1000 (selalu konsisten)
}
```

---

## Rangkuman

| Masalah | Penyebab | Solusi |
|---|---|---|
| **Goroutine Leak (kirim)** | Channel tidak ada penerima | Pastikan ada `<-ch` |
| **Goroutine Leak (terima)** | Channel tidak ada pengirim | Kirim data atau `close(ch)` |
| **Goroutine Leak (loop)** | Loop tanpa sinyal berhenti | Gunakan `range ch` + `close(ch)` |
| **Deadlock** | Kirim & terima di goroutine yang sama | Pisahkan ke goroutine berbeda |
| **Race Condition** | Akses variabel bersama tanpa proteksi | Gunakan `sync.Mutex` atau `atomic` |

---
---

# 🧩 Latihan: Tebak Output!

> Coba tebak output dari setiap kode di bawah ini sebelum melihat jawabannya.

---

## Latihan 1: Berapa Kali "Say hello selesai" Dicetak?

### 📝 Kode

```go
func sayHello(ch <-chan int) {
    for i := 0; i < 10; i++ {
        fmt.Println("Hello", i)
        time.Sleep(100 * time.Millisecond)
    }
    <-ch // terima
    fmt.Println("Say hello selesai")
    fmt.Println("Say hello selesai")
    fmt.Println("Say hello selesai")
}

func main() {
    ch := make(chan int) // init channel
    go sayHello(ch)
    for i := 0; i < 3; i++ {
        fmt.Println("Main", i)
        time.Sleep(100 * time.Millisecond)
    }
    fmt.Println("Main selesai, tunggu")
    ch <- 1 // blocking, tunggu sayHello terima
}
```

### 🤔 Coba Tebak Dulu!

<details>
<summary>Klik untuk lihat jawaban</summary>

### 🔍 Jawaban: Kemungkinan besar **0 kali**

**Output yang muncul:**
```
Main 0
Hello 0
Hello 1
Main 1
Hello 2
Main 2
Hello 3
...
Hello 9
Main selesai, tunggu
```

**Penjelasan:**
1. `main` dan `sayHello` berjalan bersamaan.
2. `main` menunggu di `ch <- 1` sampai `sayHello` menerima (`<-ch`).
3. Begitu `sayHello` menerima dari channel, **kedua goroutine unblock bersamaan**.
4. Tapi `main` **sudah tidak punya kode lagi** setelah `ch <- 1` → `main()` langsung return → **program berhenti**.
5. `sayHello` belum sempat menjalankan `fmt.Println("Say hello selesai")` karena program sudah exit.

**Kesimpulan:** "Say hello selesai" kemungkinan besar **tidak pernah tercetak** (0 kali), karena `main` exit lebih dulu. Secara teknis bisa 0–3 kali (race condition), tapi hampir selalu 0.

**Solusi:** Jika ingin memastikan semua print selesai, gunakan `sync.WaitGroup` atau channel tambahan:
```go
func main() {
    done := make(chan bool)
    go func() {
        sayHello()
        done <- true
    }()
    <-done // tunggu sampai benar-benar selesai
}
```

</details>

---

## Latihan 2: Apa yang Terjadi? (Urutan Channel Berlawanan)

### 📝 Kode

```go
func main() {
    ch1 := make(chan int)
    ch2 := make(chan int)

    go func() {
        ch1 <- 1
        fmt.Println(<-ch2)
    }()

    ch2 <- 2
    fmt.Println(<-ch1)
}
```

### 🤔 Coba Tebak Dulu!

<details>
<summary>Klik untuk lihat jawaban</summary>

### 🔍 Jawaban: **DEADLOCK!** 💀

**Output:**
```
fatal error: all goroutines are asleep - deadlock!
```

**Penjelasan step-by-step:**
1. **Goroutine** mencoba `ch1 <- 1` → **terblokir** (tidak ada penerima untuk ch1).
2. **Main** mencoba `ch2 <- 2` → **terblokir** (tidak ada penerima untuk ch2).
3. Goroutine tidak bisa sampai ke `<-ch2` karena masih stuck di `ch1 <- 1`.
4. Main tidak bisa sampai ke `<-ch1` karena masih stuck di `ch2 <- 2`.
5. **Kedua goroutine saling menunggu** → Deadlock!

**Visualisasi:**
```
Goroutine:  ch1 <- 1  ❌ (nunggu main baca ch1, tapi main stuck)
Main:       ch2 <- 2  ❌ (nunggu goroutine baca ch2, tapi goroutine stuck)
```

**Solusi:** Balik urutan operasi agar tidak saling blokir:
```go
go func() {
    ch1 <- 1
    fmt.Println(<-ch2)
}()

fmt.Println(<-ch1) // terima ch1 dulu
ch2 <- 2           // baru kirim ch2
```

</details>

---

## Latihan 3: Urutan Kirim vs Urutan Terima Terbalik

### 📝 Kode

```go
func main() {
    ch1 := make(chan int)
    ch2 := make(chan int)

    go func(){
        ch1 <- 1
        ch2 <- 2
    }()

    fmt.Println(<-ch2)
    fmt.Println(<-ch1)
}
```

### 🤔 Coba Tebak Dulu!

<details>
<summary>Klik untuk lihat jawaban</summary>

### 🔍 Jawaban: **DEADLOCK!** 💀

**Output:**
```
fatal error: all goroutines are asleep - deadlock!
```

**Penjelasan step-by-step:**
1. **Goroutine** mencoba `ch1 <- 1` → **terblokir** (menunggu penerima untuk ch1).
2. **Main** mencoba `<-ch2` → **terblokir** (menunggu pengirim untuk ch2).
3. Goroutine tidak bisa sampai ke `ch2 <- 2` karena masih stuck di `ch1 <- 1`.
4. Main tidak bisa sampai ke `<-ch1` karena masih stuck di `<-ch2`.
5. **Deadlock!**

**Inti masalah:** Goroutine **kirim ch1 dulu**, tapi main **terima ch2 dulu**. Urutannya bertabrakan.

**Solusi:** Samakan urutan kirim dan terima:
```go
go func(){
    ch1 <- 1
    ch2 <- 2
}()

fmt.Println(<-ch1) // terima ch1 dulu (sesuai urutan kirim)
fmt.Println(<-ch2) // baru terima ch2
// Output: 1, lalu 2
```

</details>

---

## Latihan 4: Satu Goroutine Kirim Dua Data

### 📝 Kode

```go
func main() {
    ch1 := make(chan int)
    ch2 := make(chan int)

    go func(){
        ch1 <- 1
        ch2 <- 2
    }()

    fmt.Println(<-ch1)
    fmt.Println(<-ch2)
}
```

### 🤔 Coba Tebak Dulu!

<details>
<summary>Klik untuk lihat jawaban</summary>

### 🔍 Jawaban: **Berhasil!** ✅

**Output:**
```
1
2
```

**Penjelasan step-by-step:**
1. **Goroutine** mengirim `1` ke ch1 → **terblokir** sampai main menerima.
2. **Main** menerima dari ch1 (`<-ch1`) → dapat `1`, goroutine lanjut ke baris berikutnya.
3. **Goroutine** mengirim `2` ke ch2 → **terblokir** sampai main menerima.
4. **Main** menerima dari ch2 (`<-ch2`) → dapat `2`, goroutine selesai.
5. Program berakhir dengan sukses.

</details>

---

## Latihan 5: Dua Goroutine Saling Silang

### 📝 Kode

```go
func main() {
    ch1 := make(chan int)
    ch2 := make(chan int)

    go func(){
        ch1 <- 1
        fmt.Println(<-ch2)
    }()
    go func(){
        ch2 <- 2
        fmt.Println(<-ch1)
    }()

}
```

### 🤔 Coba Tebak Dulu!

<details>
<summary>Klik untuk lihat jawaban</summary>

### 🔍 Jawaban: **Tidak ada output!** (Program langsung selesai)

**Penjelasan step-by-step:**
1. **Goroutine 1** mencoba `ch1 <- 1` → **terblokir** (menunggu penerima).
2. **Goroutine 2** mencoba `ch2 <- 2` → **terblokir** (menunggu penerima).
3. Kedua goroutine saling stuck, tidak bisa mencapai operasi receive masing-masing.
4. **Main** tidak punya kode apapun setelah meluncurkan goroutine → **main langsung exit**.
5. Program berhenti. Kedua goroutine mati. **Tidak ada output**.

**Catatan:** Go tidak mendeteksi deadlock di sini karena `main` tidak terblokir — `main` langsung selesai. Kedua goroutine yang terblokir menjadi **goroutine leak**, tapi karena program exit, efeknya tidak terasa.

**Solusi:** Tambahkan cara menunggu goroutine selesai:
```go
var wg sync.WaitGroup
wg.Add(2)

go func(){
    defer wg.Done()
    ch1 <- 1
    fmt.Println(<-ch2)
}()
go func(){
    defer wg.Done()
    ch2 <- 2
    fmt.Println(<-ch1)
}()

wg.Wait() // tunggu kedua goroutine selesai
// ⚠️ Tapi ini tetap DEADLOCK karena goroutine saling tunggu!
// Perlu redesign logika channel-nya.
```

</details>

---

## Latihan 6: Goroutine Kirim dan Terima ke Channel Sendiri

### 📝 Kode

```go
func main() {
    ch1 := make(chan int)
    ch2 := make(chan int)

    go func(){
        ch1 <- 1
        fmt.Println(<-ch1)
    }()
    go func(){
        ch2 <- 2
        fmt.Println(<-ch2)
    }()

}
```

### 🤔 Coba Tebak Dulu!

<details>
<summary>Klik untuk lihat jawaban</summary>

### 🔍 Jawaban: **Tidak ada output!** (Program langsung selesai)

**Penjelasan step-by-step:**
1. **Goroutine 1** mencoba `ch1 <- 1` → **terblokir** (tidak ada penerima).
2. **Goroutine 2** mencoba `ch2 <- 2` → **terblokir** (tidak ada penerima).
3. Kedua goroutine stuck di operasi send, tidak ada yang bisa sampai ke receive.
4. **Main** langsung exit → program berhenti, tidak ada output.

**Masalah ganda:**
- Bahkan jika ada yang menerima `ch1 <- 1`, goroutine 1 kemudian mencoba **menerima dari ch1 sendiri** (`<-ch1`). Tapi siapa yang akan mengirim? Goroutine 1 sendiri sudah di-receive — ini adalah **self-deadlock**.
- Satu goroutine **tidak bisa kirim DAN terima** ke channel yang sama secara berurutan (unbuffered), karena setiap operasi membutuhkan pihak lain.

**Solusi:** Pisahkan pengirim dan penerima:
```go
go func(){
    ch1 <- 1  // kirim
}()
go func(){
    ch2 <- 2  // kirim
}()

fmt.Println(<-ch1) // main terima
fmt.Println(<-ch2) // main terima
```

</details>

---

## Latihan 7: Dua Goroutine Kirim, Main Terima ✅

### 📝 Kode

```go
func main() {
    ch1 := make(chan int)
    ch2 := make(chan int)

    go func(){
        ch1 <- 1
    }()
    go func(){
        ch2 <- 2
    }()

    fmt.Println(<-ch1)
    fmt.Println(<-ch2)
}
```

### 🤔 Coba Tebak Dulu!

<details>
<summary>Klik untuk lihat jawaban</summary>

### 🔍 Jawaban: **Berhasil!** ✅

**Output:**
```
1
2
```

**Penjelasan step-by-step:**
1. **Goroutine 1** mengirim `1` ke ch1. Terblokir sampai ada penerima.
2. **Goroutine 2** mengirim `2` ke ch2. Terblokir sampai ada penerima.
3. **Main** menerima dari ch1 (`<-ch1`) → dapat `1`, goroutine 1 selesai.
4. **Main** menerima dari ch2 (`<-ch2`) → dapat `2`, goroutine 2 selesai.
5. Semua sinkron dan selesai dengan baik!

**Kenapa ini berhasil?**
- Setiap channel punya **pasangan pengirim dan penerima** yang jelas.
- Main bertindak sebagai penerima untuk kedua channel.
- Tidak ada saling tunggu antar goroutine.

</details>

---

## Rangkuman Latihan

| Latihan | Kode Singkat | Output |
|---|---|---|
| **1** | `sayHello` dengan channel + 3x print | Kemungkinan **0 kali** tercetak (main exit duluan) |
| **2** | Goroutine kirim ch1, main kirim ch2 | **Deadlock** (saling tunggu) |
| **3** | Goroutine kirim ch1→ch2, main terima ch2→ch1 | **Deadlock** (urutan terbalik) |
| **4** | Satu goroutine kirim 1 lalu 2 | ✅ **Berhasil:** `1`, `2` |
| **5** | 2 goroutine kirim-terima silang, main kosong | **Tidak ada output** (main exit) |
| **6** | Goroutine kirim & terima channel sendiri | **Tidak ada output** (self-deadlock) |
| **7** | 2 goroutine kirim, main terima keduanya | ✅ **Berhasil:** `1`, `2` |
