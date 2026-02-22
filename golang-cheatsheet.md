# 🐹 GOLANG CHEATSHEET LENGKAP
## Untuk Tes Coding & Problem Solving

---

## Daftar Isi
1. [Dasar-Dasar Go](#1-dasar-dasar-go)
2. [Tipe Data & Variabel](#2-tipe-data--variabel)
3. [String Operations](#3-string-operations)
4. [Array, Slice, Map](#4-array-slice-map)
5. [Control Flow](#5-control-flow)
6. [Functions](#6-functions)
7. [Struct & Interface](#7-struct--interface)
8. [Pointer](#8-pointer)
9. [Sorting & Searching](#9-sorting--searching)
10. [Math Operations](#10-math-operations)
11. [Input/Output](#11-inputoutput)
12. [Common Patterns untuk Problem Solving](#12-common-patterns-untuk-problem-solving)
13. [Tips & Tricks](#13-tips--tricks)

---

## 1. Dasar-Dasar Go

### Struktur Program
```go
package main

import (
    "fmt"
    "math"
    "strings"
    "sort"
    "strconv"
)

func main() {
    fmt.Println("Hello, World!")
}
```

### Import Packages yang Sering Dipakai
```go
import (
    "fmt"       // Print, Scan, Sprintf
    "math"      // Sqrt, Pow, Max, Min, Abs
    "strings"   // Split, Join, Contains, Replace
    "strconv"   // Atoi, Itoa, ParseFloat
    "sort"      // Sort, Search, SliceIsSorted
    "unicode"   // IsLetter, IsDigit, ToUpper
    "bufio"     // Scanner untuk input cepat
    "os"        // Stdin, Stdout
)
```

---

## 2. Tipe Data & Variabel

### Deklarasi Variabel
```go
// Explicit type
var x int = 10
var y float64 = 3.14
var s string = "hello"
var b bool = true

// Short declaration (hanya di dalam fungsi)
x := 10
y := 3.14
s := "hello"
b := true

// Multiple declaration
var a, b, c int = 1, 2, 3
a, b, c := 1, 2, 3

// Constant
const PI = 3.14159
const (
    MONDAY  = 1
    TUESDAY = 2
)
```

### Tipe Data
```go
// Integer
int     // platform dependent (32/64 bit)
int8    // -128 to 127
int16   // -32768 to 32767
int32   // -2^31 to 2^31-1
int64   // -2^63 to 2^63-1

// Unsigned Integer
uint    uint8   uint16   uint32   uint64

// Float
float32   float64

// String
string

// Boolean
bool    // true / false

// Byte & Rune
byte    // alias untuk uint8 (1 karakter ASCII)
rune    // alias untuk int32 (1 karakter Unicode)
```

### Konversi Tipe
```go
// Int <-> Float
x := 10
y := float64(x)        // int ke float64
z := int(3.14)          // float64 ke int → 3 (truncate)

// Int <-> String
s := strconv.Itoa(42)           // int ke string → "42"
n, err := strconv.Atoi("42")   // string ke int → 42
// err != nil jika gagal konversi

// Float <-> String
f, _ := strconv.ParseFloat("3.14", 64)  // string ke float64
s := fmt.Sprintf("%.2f", 3.14)          // float ke string → "3.14"

// Int -> binary/hex/octal string
fmt.Sprintf("%b", 10)   // "1010" (binary)
fmt.Sprintf("%x", 255)  // "ff" (hex)
fmt.Sprintf("%o", 8)    // "10" (octal)

// Char <-> Int
ch := 'A'               // rune = 65
s := string(ch)          // "A"
n := int('A')            // 65
ch = rune(65)            // 'A'
```

### Zero Values (Default)
```go
// Go otomatis inisialisasi variabel dengan zero value
int     → 0
float64 → 0.0
string  → ""
bool    → false
pointer → nil
slice   → nil
map     → nil
```

---

## 3. String Operations

### Operasi Dasar
```go
s := "Hello World"

len(s)                      // 11 (panjang)
s[0]                        // 72 (byte 'H', bukan string!)
string(s[0])                // "H"

// Slicing
s[0:5]                      // "Hello"
s[6:]                       // "World"
s[:5]                       // "Hello"

// Concat
s1 := "Hello" + " " + "World"

// Repeat (tidak ada built-in, pakai strings.Repeat)
strings.Repeat("ab", 3)    // "ababab"
```

### Package strings
```go
import "strings"

s := "Hello World"

// Check
strings.Contains(s, "World")     // true
strings.HasPrefix(s, "Hello")    // true
strings.HasSuffix(s, "World")    // true
strings.EqualFold("Go", "go")    // true (case-insensitive)

// Transform
strings.ToUpper(s)               // "HELLO WORLD"
strings.ToLower(s)               // "hello world"
strings.Title(s)                 // "Hello World" (deprecated, pakai cases)
strings.TrimSpace("  hi  ")     // "hi"
strings.Trim("**hi**", "*")     // "hi"
strings.TrimLeft("xxhi", "x")   // "hi"
strings.TrimRight("hixx", "x")  // "hi"

// Replace
strings.Replace("aabbcc", "a", "x", -1)  // "xxbbcc" (-1 = semua)
strings.Replace("aabbcc", "a", "x", 1)   // "xabbcc" (1 kali)
strings.ReplaceAll("aabb", "a", "x")      // "xxbb"

// Split & Join
strings.Split("a,b,c", ",")     // ["a", "b", "c"]
strings.Join([]string{"a","b","c"}, "-") // "a-b-c"
strings.Fields("  hello  world  ")       // ["hello", "world"] (split by whitespace)

// Find
strings.Index(s, "World")       // 6 (posisi pertama, -1 jika tidak ada)
strings.LastIndex(s, "l")       // 9
strings.Count(s, "l")           // 3
```

### Iterasi String
```go
s := "Hello"

// By byte
for i := 0; i < len(s); i++ {
    fmt.Printf("%c ", s[i])     // H e l l o
}

// By rune (Unicode-safe) — RECOMMENDED
for i, ch := range s {
    fmt.Printf("%d:%c ", i, ch) // 0:H 1:e 2:l 3:l 4:o
}

// Reverse string
func reverse(s string) string {
    runes := []rune(s)
    for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
        runes[i], runes[j] = runes[j], runes[i]
    }
    return string(runes)
}
```

### String Builder (Efisien untuk banyak concat)
```go
import "strings"

var sb strings.Builder
for i := 0; i < 1000; i++ {
    sb.WriteString("hello")
}
result := sb.String()
```

---

## 4. Array, Slice, Map

### Array (Fixed Size)
```go
// Deklarasi
var arr [5]int                  // [0, 0, 0, 0, 0]
arr := [5]int{1, 2, 3, 4, 5}
arr := [...]int{1, 2, 3}       // ukuran otomatis = 3

// Akses
arr[0]          // 1
arr[len(arr)-1] // elemen terakhir
len(arr)        // panjang

// Array = VALUE TYPE (copy saat di-assign)
a := [3]int{1, 2, 3}
b := a          // b adalah copy, bukan reference!
b[0] = 99       // a[0] tetap 1
```

### Slice (Dynamic Size) — PALING SERING DIPAKAI
```go
// Deklarasi
s := []int{1, 2, 3, 4, 5}
s := make([]int, 5)           // [0, 0, 0, 0, 0] (length=5)
s := make([]int, 0, 10)       // length=0, capacity=10

// Append
s = append(s, 6)              // tambah 1 elemen
s = append(s, 7, 8, 9)        // tambah banyak elemen
s = append(s, other...)        // gabung 2 slice

// Slicing
s[1:3]                        // [2, 3] (index 1 sampai 2)
s[:3]                         // [1, 2, 3]
s[2:]                         // [3, 4, 5]

// Length vs Capacity
len(s)                        // jumlah elemen
cap(s)                        // kapasitas sekarang

// Copy (deep copy)
src := []int{1, 2, 3}
dst := make([]int, len(src))
copy(dst, src)

// Delete elemen index i
s = append(s[:i], s[i+1:]...)

// Insert elemen di index i
s = append(s[:i], append([]int{val}, s[i:]...)...)

// Reverse slice
for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
    s[i], s[j] = s[j], s[i]
}

// Slice = REFERENCE TYPE
a := []int{1, 2, 3}
b := a          // b reference ke a!
b[0] = 99       // a[0] juga berubah jadi 99!
```

### 2D Slice (Matrix)
```go
// Buat matrix n x m
n, m := 3, 4
matrix := make([][]int, n)
for i := range matrix {
    matrix[i] = make([]int, m)
}

// Akses
matrix[0][0] = 1

// Iterate
for i := 0; i < n; i++ {
    for j := 0; j < m; j++ {
        fmt.Print(matrix[i][j], " ")
    }
    fmt.Println()
}
```

### Map (Key-Value / Dictionary)
```go
// Deklarasi
m := map[string]int{
    "alice": 90,
    "bob":   85,
}
m := make(map[string]int)     // map kosong

// CRUD
m["charlie"] = 78             // insert / update
val := m["alice"]             // get → 90
val, ok := m["dave"]          // ok=false jika key tidak ada
delete(m, "bob")              // delete

// Check key exist
if val, ok := m["alice"]; ok {
    fmt.Println(val)           // 90
}

// Iterate (urutan TIDAK dijamin!)
for key, val := range m {
    fmt.Println(key, val)
}

// Counting frequency
freq := make(map[string]int)
words := []string{"go", "go", "python", "go"}
for _, w := range words {
    freq[w]++
}
// freq = {"go": 3, "python": 1}

// Length
len(m)

// Set (pakai map[T]bool atau map[T]struct{})
set := make(map[int]bool)
set[1] = true
set[2] = true
if set[1] {
    fmt.Println("1 exists")
}
```

---

## 5. Control Flow

### If-Else
```go
x := 10

if x > 0 {
    fmt.Println("Positive")
} else if x < 0 {
    fmt.Println("Negative")
} else {
    fmt.Println("Zero")
}

// If with init statement (variabel hanya berlaku di dalam if)
if val, ok := m["key"]; ok {
    fmt.Println(val)
}

// If with error check (pattern sangat umum di Go)
if err != nil {
    fmt.Println("Error:", err)
    return
}
```

### Switch
```go
// Switch biasa
switch day {
case "Monday":
    fmt.Println("Senin")
case "Tuesday", "Wednesday":   // multiple values
    fmt.Println("Selasa/Rabu")
default:
    fmt.Println("Lainnya")
}

// Switch tanpa kondisi (pengganti if-elif chain)
switch {
case x > 90:
    fmt.Println("A")
case x > 80:
    fmt.Println("B")
case x > 70:
    fmt.Println("C")
default:
    fmt.Println("D")
}

// Switch dengan init statement
switch v := x % 2; v {
case 0:
    fmt.Println("Even")
case 1:
    fmt.Println("Odd")
}

// CATATAN: Go switch TIDAK perlu break (auto break)
// Gunakan fallthrough jika ingin lanjut ke case berikutnya
```

### For Loop (Satu-satunya loop di Go!)
```go
// Classic for
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// While-style
i := 0
for i < 10 {
    fmt.Println(i)
    i++
}

// Infinite loop
for {
    // break untuk keluar
    break
}

// Range (iterate slice)
nums := []int{1, 2, 3, 4}
for i, v := range nums {       // i=index, v=value
    fmt.Println(i, v)
}

for _, v := range nums {       // skip index
    fmt.Println(v)
}

for i := range nums {          // index only
    fmt.Println(i)
}

// Range string (iterate rune)
for i, ch := range "Hello" {
    fmt.Printf("%d: %c\n", i, ch)
}

// Range map
for key, val := range myMap {
    fmt.Println(key, val)
}

// Break & Continue
for i := 0; i < 10; i++ {
    if i == 5 {
        break       // keluar loop
    }
    if i%2 == 0 {
        continue    // skip iterasi
    }
    fmt.Println(i)  // 1, 3
}
```

---

## 6. Functions

### Deklarasi Function
```go
// Basic
func add(a int, b int) int {
    return a + b
}

// Parameter tipe sama bisa disingkat
func add(a, b int) int {
    return a + b
}

// Multiple return values
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}

result, err := divide(10, 3)

// Named return values
func swap(a, b int) (x, y int) {
    x = b
    y = a
    return  // naked return
}

// Variadic function (parameter tak terbatas)
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}

sum(1, 2, 3, 4)        // 10
nums := []int{1, 2, 3}
sum(nums...)            // 6 (spread slice)
```

### Anonymous Function & Closure
```go
// Anonymous function
square := func(x int) int {
    return x * x
}
square(5)  // 25

// Immediately invoked
func(msg string) {
    fmt.Println(msg)
}("Hello!")

// Closure (akses variabel luar)
func counter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}

c := counter()
c()  // 1
c()  // 2
c()  // 3
```

### Defer
```go
// defer: eksekusi di akhir function (LIFO order)
func main() {
    defer fmt.Println("1")
    defer fmt.Println("2")
    defer fmt.Println("3")
    // Output: 3, 2, 1
}

// Biasa dipakai untuk cleanup
func readFile(path string) {
    f, err := os.Open(path)
    if err != nil {
        return
    }
    defer f.Close()  // pasti tertutup saat fungsi selesai
    // ... baca file
}
```

---

## 7. Struct & Interface

### Struct (Pengganti Class)
```go
// Deklarasi
type Student struct {
    Name  string
    Age   int
    Grade float64
}

// Inisialisasi
s1 := Student{Name: "Alice", Age: 20, Grade: 3.5}
s2 := Student{"Bob", 21, 3.8}  // urutan field
var s3 Student                   // zero values: "", 0, 0.0

// Akses field
s1.Name     // "Alice"
s1.Age = 21 // update

// Method (function yang terikat ke struct)
func (s Student) Info() string {
    return fmt.Sprintf("%s, age %d", s.Name, s.Age)
}

// Method dengan pointer receiver (bisa modify struct)
func (s *Student) SetAge(age int) {
    s.Age = age  // perubahan berlaku di luar
}

s1.Info()       // "Alice, age 20"
s1.SetAge(22)   // s1.Age sekarang 22
```

### Embedded Struct (Inheritance-like)
```go
type Person struct {
    Name string
    Age  int
}

type Employee struct {
    Person           // embedded (seperti inheritance)
    Company string
}

emp := Employee{
    Person:  Person{Name: "Alice", Age: 25},
    Company: "Google",
}

emp.Name    // "Alice" (akses langsung field Person)
emp.Age     // 25
```

### Interface
```go
// Deklarasi interface
type Shape interface {
    Area() float64
    Perimeter() float64
}

// Struct yang implement interface (IMPLISIT, tidak perlu keyword)
type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * math.Pi * c.Radius
}

type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

// Polymorphism: function menerima interface
func printArea(s Shape) {
    fmt.Println("Area:", s.Area())
}

printArea(Circle{Radius: 5})
printArea(Rectangle{Width: 3, Height: 4})

// Empty interface (menerima semua tipe)
var anything interface{}
anything = 42
anything = "hello"
anything = []int{1, 2, 3}

// Type assertion
val, ok := anything.(string)
if ok {
    fmt.Println(val)
}

// Type switch
switch v := anything.(type) {
case int:
    fmt.Println("int:", v)
case string:
    fmt.Println("string:", v)
default:
    fmt.Println("unknown")
}
```

---

## 8. Pointer

### Dasar Pointer
```go
x := 10
p := &x         // p = pointer ke x (tipe *int)
fmt.Println(*p)  // 10 (dereference, ambil nilai)
*p = 20          // x sekarang 20

// Pointer sebagai parameter
func increment(val *int) {
    *val++
}

x := 10
increment(&x)
fmt.Println(x)  // 11

// new() mengalokasikan memory dan return pointer
p := new(int)   // *int, nilai awal = 0
*p = 42
```

### Kapan Pakai Pointer?
```go
// 1. Saat ingin modifikasi data asli
func updateName(s *Student) {
    s.Name = "Updated"
}

// 2. Saat struct besar (hindari copy yang mahal)
func process(data *BigStruct) { ... }

// 3. Pointer receiver di method (konvensi: konsisten)
func (s *Student) SetGrade(g float64) {
    s.Grade = g
}
```

---

## 9. Sorting & Searching

### Sort Slice
```go
import "sort"

// Sort integer slice
nums := []int{5, 3, 1, 4, 2}
sort.Ints(nums)                 // [1, 2, 3, 4, 5]

// Sort string slice
words := []string{"banana", "apple", "cherry"}
sort.Strings(words)             // [apple, banana, cherry]

// Sort float slice
floats := []float64{3.14, 1.41, 2.72}
sort.Float64s(floats)           // [1.41, 2.72, 3.14]

// Reverse sort
sort.Sort(sort.Reverse(sort.IntSlice(nums)))    // [5, 4, 3, 2, 1]
sort.Sort(sort.Reverse(sort.StringSlice(words)))

// Check if sorted
sort.IntsAreSorted(nums)        // bool
```

### Custom Sort (sort.Slice)
```go
// Sort by custom function
people := []struct {
    Name string
    Age  int
}{
    {"Alice", 25},
    {"Bob", 20},
    {"Charlie", 30},
}

// Sort by age ascending
sort.Slice(people, func(i, j int) bool {
    return people[i].Age < people[j].Age
})

// Sort by age descending
sort.Slice(people, func(i, j int) bool {
    return people[i].Age > people[j].Age
})

// Sort by multiple keys (age ascending, lalu name ascending)
sort.Slice(people, func(i, j int) bool {
    if people[i].Age != people[j].Age {
        return people[i].Age < people[j].Age
    }
    return people[i].Name < people[j].Name
})

// Stable sort (menjaga urutan relatif elemen yang sama)
sort.SliceStable(people, func(i, j int) bool {
    return people[i].Age < people[j].Age
})
```

### Binary Search
```go
// sort.SearchInts (data harus sudah terurut!)
nums := []int{1, 3, 5, 7, 9}
idx := sort.SearchInts(nums, 5)  // 2 (index dimana 5 berada/seharusnya)

// Manual binary search
func binarySearch(arr []int, target int) int {
    lo, hi := 0, len(arr)-1
    for lo <= hi {
        mid := lo + (hi-lo)/2    // hindari overflow
        if arr[mid] == target {
            return mid
        } else if arr[mid] < target {
            lo = mid + 1
        } else {
            hi = mid - 1
        }
    }
    return -1
}
```

---

## 10. Math Operations

### Arithmetic
```go
a + b       // tambah
a - b       // kurang
a * b       // kali
a / b       // bagi (integer division jika kedua int)
a % b       // modulo

// CATATAN: Go TIDAK punya operator ** (power)
// Pakai math.Pow()

// Integer division
7 / 2       // 3 (bukan 3.5!)
7.0 / 2.0   // 3.5
```

### Package math
```go
import "math"

math.Abs(-5.0)          // 5.0 (float64 only!)
math.Sqrt(16)           // 4.0
math.Pow(2, 10)         // 1024.0
math.Log(math.E)        // 1.0 (ln)
math.Log2(8)            // 3.0
math.Log10(100)         // 2.0

math.Floor(3.7)         // 3.0
math.Ceil(3.2)          // 4.0
math.Round(3.5)         // 4.0

math.Max(3, 7)          // 7.0 (float64!)
math.Min(3, 7)          // 3.0 (float64!)

math.MaxInt64           // 9223372036854775807
math.MinInt64           // -9223372036854775808
math.MaxFloat64         // 1.7976931348623157e+308
math.Inf(1)             // +Inf
math.Inf(-1)            // -Inf

math.Pi                 // 3.141592653589793
math.E                  // 2.718281828459045
```

### Integer Helper Functions (Harus Buat Sendiri!)
```go
// Go TIDAK punya built-in abs, min, max untuk integer (sebelum Go 1.21)
// Go 1.21+ punya min() dan max() built-in untuk semua tipe

// Untuk versi lama, buat sendiri:
func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}

func maxInt(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func minInt(a, b int) int {
    if a < b {
        return a
    }
    return b
}

// GCD (Greatest Common Divisor)
func gcd(a, b int) int {
    for b != 0 {
        a, b = b, a%b
    }
    return a
}

// LCM (Least Common Multiple)
func lcm(a, b int) int {
    return a / gcd(a, b) * b
}

// Power (integer version)
func pow(base, exp int) int {
    result := 1
    for exp > 0 {
        result *= base
        exp--
    }
    return result
}

// Fast Power (modular)
func fastPow(base, exp, mod int) int {
    result := 1
    base %= mod
    for exp > 0 {
        if exp%2 == 1 {
            result = result * base % mod
        }
        exp /= 2
        base = base * base % mod
    }
    return result
}
```

---

## 11. Input/Output

### Fmt (Basic)
```go
// Output
fmt.Println("Hello", "World")   // Hello World\n
fmt.Printf("x = %d\n", 10)     // formatted
fmt.Print("no newline")         // tanpa newline
s := fmt.Sprintf("%d-%d", 1, 2)// return string "1-2"

// Format specifiers
%d      // integer
%f      // float (%.2f = 2 desimal)
%s      // string
%c      // character (rune)
%v      // value (any type)
%t      // boolean
%b      // binary
%x      // hexadecimal
%o      // octal
%T      // type
%%      // literal %

// Input (basic, lambat untuk competitive)
var n int
fmt.Scan(&n)                    // baca 1 value
fmt.Scanf("%d %d", &a, &b)     // formatted input
fmt.Scanln(&s)                  // baca 1 baris
```

### Bufio (Fast I/O untuk Competitive Programming)
```go
import (
    "bufio"
    "fmt"
    "os"
)

// Fast reader
reader := bufio.NewReader(os.Stdin)
writer := bufio.NewWriter(os.Stdout)
defer writer.Flush()

// Baca integer cepat
var n int
fmt.Fscan(reader, &n)

// Baca banyak integer
var a, b, c int
fmt.Fscan(reader, &a, &b, &c)

// Baca string 1 baris
line, _ := reader.ReadString('\n')
line = strings.TrimSpace(line)

// Output cepat
fmt.Fprintln(writer, result)
fmt.Fprintf(writer, "%d %d\n", a, b)

// Template competitive programming
func main() {
    reader := bufio.NewReader(os.Stdin)
    writer := bufio.NewWriter(os.Stdout)
    defer writer.Flush()

    var t int
    fmt.Fscan(reader, &t)  // jumlah test case

    for t > 0 {
        var n int
        fmt.Fscan(reader, &n)

        // ... solve ...

        fmt.Fprintln(writer, answer)
        t--
    }
}
```

### Scanner (Alternatif)
```go
scanner := bufio.NewScanner(os.Stdin)
scanner.Split(bufio.ScanWords) // split by word (default: by line)

// Baca integer
scanner.Scan()
n, _ := strconv.Atoi(scanner.Text())

// Baca array of integers
arr := make([]int, n)
for i := 0; i < n; i++ {
    scanner.Scan()
    arr[i], _ = strconv.Atoi(scanner.Text())
}
```

---

## 12. Common Patterns untuk Problem Solving

### Two Pointers
```go
// Contoh: cek palindrome
func isPalindrome(s string) bool {
    i, j := 0, len(s)-1
    for i < j {
        if s[i] != s[j] {
            return false
        }
        i++
        j--
    }
    return true
}

// Two Sum (sorted array)
func twoSum(nums []int, target int) (int, int) {
    lo, hi := 0, len(nums)-1
    for lo < hi {
        sum := nums[lo] + nums[hi]
        if sum == target {
            return lo, hi
        } else if sum < target {
            lo++
        } else {
            hi--
        }
    }
    return -1, -1
}
```

### Sliding Window
```go
// Max sum subarray of size k
func maxSumSubarray(arr []int, k int) int {
    windowSum := 0
    for i := 0; i < k; i++ {
        windowSum += arr[i]
    }

    maxSum := windowSum
    for i := k; i < len(arr); i++ {
        windowSum += arr[i] - arr[i-k]
        if windowSum > maxSum {
            maxSum = windowSum
        }
    }
    return maxSum
}
```

### Frequency Counter
```go
func topKFrequent(words []string, k int) []string {
    freq := make(map[string]int)
    for _, w := range words {
        freq[w]++
    }

    type pair struct {
        word string
        cnt  int
    }

    pairs := make([]pair, 0)
    for w, c := range freq {
        pairs = append(pairs, pair{w, c})
    }

    sort.Slice(pairs, func(i, j int) bool {
        if pairs[i].cnt != pairs[j].cnt {
            return pairs[i].cnt > pairs[j].cnt
        }
        return pairs[i].word < pairs[j].word
    })

    result := make([]string, k)
    for i := 0; i < k; i++ {
        result[i] = pairs[i].word
    }
    return result
}
```

### Stack
```go
// Go tidak punya built-in stack, pakai slice
stack := []int{}

// Push
stack = append(stack, 10)

// Pop
top := stack[len(stack)-1]
stack = stack[:len(stack)-1]

// Peek
top := stack[len(stack)-1]

// IsEmpty
len(stack) == 0

// Contoh: Valid Parentheses
func isValid(s string) bool {
    stack := []rune{}
    pairs := map[rune]rune{')': '(', ']': '[', '}': '{'}

    for _, ch := range s {
        if ch == '(' || ch == '[' || ch == '{' {
            stack = append(stack, ch)
        } else {
            if len(stack) == 0 || stack[len(stack)-1] != pairs[ch] {
                return false
            }
            stack = stack[:len(stack)-1]
        }
    }
    return len(stack) == 0
}
```

### Queue
```go
// Pakai slice (simple, tapi bukan O(1) amortized dequeue)
queue := []int{}

// Enqueue
queue = append(queue, 10)

// Dequeue
front := queue[0]
queue = queue[1:]

// Pakai container/list untuk O(1) dequeue
import "container/list"

q := list.New()
q.PushBack(10)          // enqueue
front := q.Front()      // peek
q.Remove(front)         // dequeue
```

### BFS
```go
func bfs(graph map[int][]int, start int) []int {
    visited := make(map[int]bool)
    queue := []int{start}
    visited[start] = true
    result := []int{}

    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]
        result = append(result, node)

        for _, neighbor := range graph[node] {
            if !visited[neighbor] {
                visited[neighbor] = true
                queue = append(queue, neighbor)
            }
        }
    }
    return result
}
```

### DFS
```go
// Recursive
func dfs(graph map[int][]int, node int, visited map[int]bool) {
    visited[node] = true
    fmt.Println(node)
    for _, neighbor := range graph[node] {
        if !visited[neighbor] {
            dfs(graph, neighbor, visited)
        }
    }
}

// Iterative (pakai stack)
func dfsIterative(graph map[int][]int, start int) {
    visited := make(map[int]bool)
    stack := []int{start}

    for len(stack) > 0 {
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]

        if visited[node] {
            continue
        }
        visited[node] = true
        fmt.Println(node)

        for _, neighbor := range graph[node] {
            if !visited[neighbor] {
                stack = append(stack, neighbor)
            }
        }
    }
}
```

### Dynamic Programming
```go
// Fibonacci dengan memoization
func fib(n int, memo map[int]int) int {
    if n <= 1 {
        return n
    }
    if val, ok := memo[n]; ok {
        return val
    }
    memo[n] = fib(n-1, memo) + fib(n-2, memo)
    return memo[n]
}

// Fibonacci bottom-up (tabulation)
func fibBottomUp(n int) int {
    if n <= 1 {
        return n
    }
    dp := make([]int, n+1)
    dp[0], dp[1] = 0, 1
    for i := 2; i <= n; i++ {
        dp[i] = dp[i-1] + dp[i-2]
    }
    return dp[n]
}

// Space optimized
func fibOptimized(n int) int {
    if n <= 1 {
        return n
    }
    a, b := 0, 1
    for i := 2; i <= n; i++ {
        a, b = b, a+b
    }
    return b
}
```

---

## 13. Tips & Tricks

### Go-Specific Tips
```go
// 1. Swap tanpa temp
a, b = b, a

// 2. Multiple return untuk error handling
val, err := someFunc()
if err != nil {
    // handle error
}

// 3. Blank identifier (ignore value)
_, err := someFunc()    // ignore first return
for _, v := range arr { // ignore index

// 4. iota untuk enum
const (
    Monday = iota    // 0
    Tuesday          // 1
    Wednesday        // 2
)

// 5. Init function (dijalankan otomatis sebelum main)
func init() {
    // setup
}

// 6. Goroutine (concurrent execution)
go func() {
    fmt.Println("running async")
}()

// 7. Channel (komunikasi antar goroutine)
ch := make(chan int)
go func() { ch <- 42 }()
val := <-ch  // 42

// 8. Quick min/max dari slice
func minSlice(s []int) int {
    m := s[0]
    for _, v := range s[1:] {
        if v < m {
            m = v
        }
    }
    return m
}

// 9. Sum slice
func sumSlice(s []int) int {
    total := 0
    for _, v := range s {
        total += v
    }
    return total
}

// 10. Cek apakah angka prima
func isPrime(n int) bool {
    if n < 2 {
        return false
    }
    for i := 2; i*i <= n; i++ {
        if n%i == 0 {
            return false
        }
    }
    return true
}

// 11. Digit sum
func digitSum(n int) int {
    sum := 0
    for n > 0 {
        sum += n % 10
        n /= 10
    }
    return sum
}
```

### Perbedaan Penting Go vs Python
| Aspek | Python | Go |
|-------|--------|------|
| Typing | Dynamic | Static |
| Indentation | Wajib (semantik) | Opsional (style) |
| Kurung kurawal | Tidak ada | Wajib `{}` |
| Semicolon | Tidak perlu | Auto-insert |
| Unused variable | OK | **Error!** |
| Unused import | OK | **Error!** |
| Loop | for, while | Hanya `for` |
| Class | Ada | Tidak ada (pakai struct) |
| Inheritance | Ada | Tidak ada (pakai embedding & interface) |
| Exception | try-except | Error return value |
| Generics | Built-in | Ada (Go 1.18+) |
| List/Dict comp | Ada | Tidak ada |
| abs(int) | Built-in | Buat sendiri (atau Go 1.21+) |
| min/max(int) | Built-in | Buat sendiri (atau Go 1.21+) |

### Template Competitive Programming (Go)
```go
package main

import (
    "bufio"
    "fmt"
    "os"
    "sort"
)

var reader *bufio.Reader
var writer *bufio.Writer

func main() {
    reader = bufio.NewReader(os.Stdin)
    writer = bufio.NewWriter(os.Stdout)
    defer writer.Flush()

    var t int
    fmt.Fscan(reader, &t)

    for t > 0 {
        solve()
        t--
    }
}

func solve() {
    var n int
    fmt.Fscan(reader, &n)

    arr := make([]int, n)
    for i := 0; i < n; i++ {
        fmt.Fscan(reader, &arr[i])
    }

    // ... logika solusi ...

    fmt.Fprintln(writer, "answer")
}

// Helper functions
func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func gcd(a, b int) int {
    for b != 0 {
        a, b = b, a%b
    }
    return a
}
```
