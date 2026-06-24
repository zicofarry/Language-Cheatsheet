# 🐦 SWIFT CHEATSHEET LENGKAP
## Untuk Tes Coding & Problem Solving

---

## Daftar Isi
1. [Dasar-Dasar Swift](#1-dasar-dasar-swift)
2. [Tipe Data & Variabel](#2-tipe-data--variabel)
3. [String Operations](#3-string-operations)
4. [Array, Set, Dictionary](#4-array-set-dictionary)
5. [Control Flow](#5-control-flow)
6. [Functions & Closures](#6-functions--closures)
7. [Struct, Class, Enum](#7-struct-class-enum)
8. [Optionals](#8-optionals)
9. [Sorting & Searching](#9-sorting--searching)
10. [Math Operations](#10-math-operations)
11. [Input/Output](#11-inputoutput)
12. [Common Patterns untuk Problem Solving](#12-common-patterns-untuk-problem-solving)
13. [Tips & Tricks](#13-tips--tricks)

---

## 1. Dasar-Dasar Swift

### Struktur Program
```swift
import Foundation

// Swift tidak butuh main function untuk script
// Untuk app, entry point pakai @main atau main.swift
print("Hello, World!")
```

### Import yang Sering Dipakai
```swift
import Foundation    // String, Array, Dictionary, Math, dll
// Foundation sudah include hampir semua yang dibutuhkan
// Tidak perlu import terpisah seperti bahasa lain
```

---

## 2. Tipe Data & Variabel

### Deklarasi Variabel
```swift
// var = mutable (bisa diubah)
var x: Int = 10
var y: Double = 3.14
var s: String = "hello"
var b: Bool = true

// let = immutable / constant (TIDAK bisa diubah)
let pi = 3.14159
let name = "Alice"

// Type inference (Swift otomatis deteksi tipe)
var x = 10          // Int
var y = 3.14        // Double
var s = "hello"     // String
var b = true        // Bool

// Multiple declaration
var a = 1, b = 2, c = 3
```

### Tipe Data
```swift
// Integer
Int         // 64-bit di platform modern
Int8        // -128 to 127
Int16       // -32768 to 32767
Int32       // -2^31 to 2^31-1
Int64       // -2^63 to 2^63-1

// Unsigned Integer
UInt    UInt8   UInt16   UInt32   UInt64

// Float
Float       // 32-bit
Double      // 64-bit (default, RECOMMENDED)

// String
String      // Unicode-safe, value type

// Boolean
Bool        // true / false

// Character
Character   // satu karakter Unicode
let ch: Character = "A"
```

### Konversi Tipe
```swift
// Int <-> Double
let x = 10
let y = Double(x)       // 10.0
let z = Int(3.14)       // 3 (truncate)

// Int <-> String
let s = String(42)              // "42"
let n = Int("42")               // Optional(42) — bisa nil!
let n = Int("42")!              // 42 (force unwrap, hati-hati!)

// Double <-> String
let f = Double("3.14")!         // 3.14
let s = String(format: "%.2f", 3.14)  // "3.14"

// Int -> binary/hex/octal string
String(10, radix: 2)            // "1010" (binary)
String(255, radix: 16)          // "ff" (hex)
String(8, radix: 8)             // "10" (octal)

// Char <-> Int (ASCII)
let ch: Character = "A"
let ascii = ch.asciiValue!      // 65 (UInt8)
let char = Character(UnicodeScalar(65))  // "A"
```

### Default Values
```swift
// Swift TIDAK punya zero values seperti Go
// Variabel HARUS diinisialisasi sebelum dipakai
// Kecuali Optional yang default = nil

var x: Int?         // nil (Optional)
var arr: [Int] = [] // array kosong
var dict: [String: Int] = [:] // dictionary kosong
```

---

## 3. String Operations

### Operasi Dasar
```swift
let s = "Hello World"

s.count                         // 11 (panjang)
s.isEmpty                       // false

// Akses karakter (String di Swift BUKAN subscriptable by Int!)
s.first                         // Optional("H")
s.last                          // Optional("d")
s[s.startIndex]                 // "H"
s[s.index(s.startIndex, offsetBy: 4)]  // "o"

// Slicing (pakai String.Index, bukan Int!)
let start = s.index(s.startIndex, offsetBy: 0)
let end = s.index(s.startIndex, offsetBy: 5)
String(s[start..<end])          // "Hello"

// Prefix & Suffix shortcut
String(s.prefix(5))             // "Hello"
String(s.suffix(5))             // "World"
String(s.dropFirst(6))          // "World"
String(s.dropLast(6))           // "Hello"

// Concat
let s1 = "Hello" + " " + "World"
let s2 = "\(name) is \(age) years old"  // String interpolation

// Repeat
String(repeating: "ab", count: 3)  // "ababab"
```

### Method String
```swift
let s = "Hello World"

// Check
s.contains("World")             // true
s.hasPrefix("Hello")            // true
s.hasSuffix("World")            // true
s.lowercased() == "hello world" // case-insensitive compare

// Transform
s.uppercased()                  // "HELLO WORLD"
s.lowercased()                  // "hello world"
s.capitalized                   // "Hello World"
"  hi  ".trimmingCharacters(in: .whitespaces)  // "hi"

// Replace
s.replacingOccurrences(of: "World", with: "Swift")  // "Hello Swift"

// Split & Join
"a,b,c".split(separator: ",")              // ["a", "b", "c"] (Substring array)
"a,b,c".components(separatedBy: ",")       // ["a", "b", "c"] (String array)
["a", "b", "c"].joined(separator: "-")     // "a-b-c"

// Find
s.firstIndex(of: "W")          // Optional(String.Index)
s.range(of: "World")           // Optional(Range)
s.filter { $0 != "l" }         // "Heo Word" (hapus semua 'l')
```

### Iterasi String
```swift
let s = "Hello"

// By character
for ch in s {
    print(ch, terminator: " ")  // H e l l o
}

// By index & character
for (i, ch) in s.enumerated() {
    print("\(i):\(ch)", terminator: " ")  // 0:H 1:e 2:l 3:l 4:o
}

// Reverse string
String(s.reversed())           // "olleH"

// Convert to Array of Characters (untuk akses by index)
let chars = Array(s)           // ["H", "e", "l", "l", "o"]
chars[0]                       // "H"
chars[3]                       // "l"
```

---

## 4. Array, Set, Dictionary

### Array (Dynamic Size, Ordered)
```swift
// Deklarasi
var arr: [Int] = [1, 2, 3, 4, 5]
var arr = [1, 2, 3, 4, 5]              // type inference
var arr: [Int] = []                     // array kosong
var arr = [Int]()                       // array kosong
var arr = Array(repeating: 0, count: 5) // [0, 0, 0, 0, 0]

// Akses
arr[0]                  // 1
arr[arr.count - 1]      // elemen terakhir
arr.first               // Optional(1)
arr.last                // Optional(5)
arr.count               // 5
arr.isEmpty             // false

// CRUD
arr.append(6)                   // tambah di akhir
arr.append(contentsOf: [7, 8]) // tambah banyak
arr.insert(0, at: 0)           // insert di index 0
arr.remove(at: 2)              // hapus di index 2
arr.removeLast()                // hapus terakhir
arr.removeFirst()               // hapus pertama
arr.removeAll()                 // hapus semua

// Slicing
arr[1...3]                      // elemen index 1 sampai 3
arr[1..<3]                      // elemen index 1 sampai 2
Array(arr[1...3])               // convert slice ke Array

// Contains
arr.contains(3)                 // true

// Concat
let combined = arr1 + arr2

// Copy (Array = VALUE TYPE di Swift, auto deep copy!)
var a = [1, 2, 3]
var b = a               // b adalah COPY
b[0] = 99               // a[0] tetap 1!

// Map, Filter, Reduce
let doubled = arr.map { $0 * 2 }         // [2, 4, 6, 8, 10]
let evens = arr.filter { $0 % 2 == 0 }   // [2, 4]
let sum = arr.reduce(0, +)               // 15
let sum = arr.reduce(0) { $0 + $1 }      // 15

// CompactMap (hapus nil dari Optional)
let nums = ["1", "2", "abc", "4"]
let valid = nums.compactMap { Int($0) }   // [1, 2, 4]

// FlatMap (flatten nested array)
let nested = [[1, 2], [3, 4], [5]]
let flat = nested.flatMap { $0 }          // [1, 2, 3, 4, 5]

// Enumerate
for (index, value) in arr.enumerated() {
    print("\(index): \(value)")
}

// Reverse
arr.reversed()                  // lazy reversed collection
Array(arr.reversed())           // [5, 4, 3, 2, 1]
```

### 2D Array (Matrix)
```swift
// Buat matrix n x m
let n = 3, m = 4
var matrix = Array(repeating: Array(repeating: 0, count: m), count: n)

// Akses
matrix[0][0] = 1

// Iterate
for i in 0..<n {
    for j in 0..<m {
        print(matrix[i][j], terminator: " ")
    }
    print()
}
```

### Set (Unordered, Unique)
```swift
// Deklarasi
var set: Set<Int> = [1, 2, 3, 4, 5]
var set = Set<Int>()                // set kosong

// CRUD
set.insert(6)                       // insert
set.remove(3)                       // remove (return Optional)
set.contains(2)                     // true
set.count                           // jumlah elemen

// Set Operations
let a: Set = [1, 2, 3, 4]
let b: Set = [3, 4, 5, 6]

a.union(b)                          // {1, 2, 3, 4, 5, 6}
a.intersection(b)                   // {3, 4}
a.subtracting(b)                    // {1, 2}
a.symmetricDifference(b)            // {1, 2, 5, 6}
a.isSubset(of: b)                   // false
a.isSuperset(of: b)                 // false
a.isDisjoint(with: b)               // false (ada irisan)
```

### Dictionary (Key-Value)
```swift
// Deklarasi
var dict: [String: Int] = ["alice": 90, "bob": 85]
var dict = ["alice": 90, "bob": 85]  // type inference
var dict = [String: Int]()           // dictionary kosong

// CRUD
dict["charlie"] = 78                 // insert / update
let val = dict["alice"]              // Optional(90)
let val = dict["alice", default: 0]  // 90, atau 0 jika tidak ada
dict["bob"] = nil                    // delete
dict.removeValue(forKey: "bob")      // delete (return Optional)

// Check key exist
if let val = dict["alice"] {
    print(val)                       // 90
}

// Iterate (urutan TIDAK dijamin!)
for (key, val) in dict {
    print("\(key): \(val)")
}

// Keys & Values
Array(dict.keys)                     // ["alice", "charlie"]
Array(dict.values)                   // [90, 78]

// Counting frequency
var freq = [String: Int]()
let words = ["go", "go", "python", "go"]
for w in words {
    freq[w, default: 0] += 1
}
// freq = ["go": 3, "python": 1]

// Length
dict.count
dict.isEmpty

// Merge
dict.merge(["dave": 88]) { (current, _) in current }
```

---

## 5. Control Flow

### If-Else
```swift
let x = 10

if x > 0 {
    print("Positive")
} else if x < 0 {
    print("Negative")
} else {
    print("Zero")
}

// Ternary operator
let result = x > 0 ? "Positive" : "Non-positive"

// If-let (Optional binding)
if let val = dict["key"] {
    print(val)  // hanya jika key ada
}

// Guard (early return)
func process(name: String?) {
    guard let name = name else {
        print("No name")
        return
    }
    print("Hello, \(name)")
}
```

### Switch
```swift
// Switch biasa (WAJIB exhaustive — harus cover semua case!)
let day = "Monday"
switch day {
case "Monday":
    print("Senin")
case "Tuesday", "Wednesday":     // multiple values
    print("Selasa/Rabu")
default:
    print("Lainnya")
}

// Switch dengan range
let score = 85
switch score {
case 90...100:
    print("A")
case 80..<90:
    print("B")
case 70..<80:
    print("C")
default:
    print("D")
}

// Switch dengan where clause
switch x {
case let n where n > 0:
    print("Positive: \(n)")
case let n where n < 0:
    print("Negative: \(n)")
default:
    print("Zero")
}

// Switch tuple
let point = (1, 0)
switch point {
case (0, 0):
    print("Origin")
case (_, 0):
    print("On x-axis")
case (0, _):
    print("On y-axis")
case (let x, let y):
    print("Point at \(x), \(y)")
}

// CATATAN: Swift switch TIDAK perlu break (auto break)
// Gunakan fallthrough jika ingin lanjut ke case berikutnya
```

### For Loop
```swift
// Range-based
for i in 0..<10 {               // 0 sampai 9
    print(i)
}

for i in 0...10 {               // 0 sampai 10 (inclusive)
    print(i)
}

// Reverse
for i in (0..<10).reversed() {
    print(i)                     // 9, 8, 7, ..., 0
}

// Step (stride)
for i in stride(from: 0, to: 10, by: 2) {
    print(i)                     // 0, 2, 4, 6, 8
}

for i in stride(from: 10, through: 0, by: -2) {
    print(i)                     // 10, 8, 6, 4, 2, 0
}

// Iterate collection
let nums = [1, 2, 3, 4]
for v in nums {
    print(v)
}

for (i, v) in nums.enumerated() {
    print("\(i): \(v)")
}

// Skip value
for _ in 0..<5 {
    print("repeat")
}

// Iterate dictionary
for (key, val) in dict {
    print("\(key): \(val)")
}
```

### While Loop
```swift
// While
var i = 0
while i < 10 {
    print(i)
    i += 1
}

// Repeat-while (do-while)
var j = 0
repeat {
    print(j)
    j += 1
} while j < 10
```

### Break & Continue
```swift
for i in 0..<10 {
    if i == 5 {
        break               // keluar loop
    }
    if i % 2 == 0 {
        continue            // skip iterasi
    }
    print(i)                // 1, 3
}

// Labeled statements (untuk nested loop)
outerLoop: for i in 0..<5 {
    for j in 0..<5 {
        if i + j > 5 {
            break outerLoop  // keluar dari outer loop
        }
    }
}
```

---

## 6. Functions & Closures

### Deklarasi Function
```swift
// Basic
func add(a: Int, b: Int) -> Int {
    return a + b
}
add(a: 5, b: 3)                // 8 (harus pakai label!)

// External & internal parameter names
func greet(person name: String) -> String {
    return "Hello, \(name)"
}
greet(person: "Alice")          // "Hello, Alice"

// Omit external label
func add(_ a: Int, _ b: Int) -> Int {
    return a + b
}
add(5, 3)                       // 8 (tanpa label)

// Default parameter
func greet(_ name: String, greeting: String = "Hello") -> String {
    return "\(greeting), \(name)"
}
greet("Alice")                  // "Hello, Alice"
greet("Alice", greeting: "Hi") // "Hi, Alice"

// Multiple return values (pakai tuple)
func minMax(_ arr: [Int]) -> (min: Int, max: Int) {
    return (arr.min()!, arr.max()!)
}
let result = minMax([3, 1, 4, 1, 5])
result.min  // 1
result.max  // 5

// Variadic function
func sum(_ nums: Int...) -> Int {
    return nums.reduce(0, +)
}
sum(1, 2, 3, 4)                // 10

// inout parameter (pass by reference)
func swapValues(_ a: inout Int, _ b: inout Int) {
    let temp = a; a = b; b = temp
    // atau: (a, b) = (b, a)
}
var x = 1, y = 2
swapValues(&x, &y)              // x=2, y=1
```

### Closures
```swift
// Closure expression
let square = { (x: Int) -> Int in
    return x * x
}
square(5)  // 25

// Shortened syntax
let square: (Int) -> Int = { x in x * x }
let square: (Int) -> Int = { $0 * $0 }

// Trailing closure syntax
let nums = [5, 3, 1, 4, 2]
let sorted = nums.sorted { $0 < $1 }     // [1, 2, 3, 4, 5]
let sorted = nums.sorted(by: <)          // shorthand

// Closure sebagai parameter
func apply(_ arr: [Int], transform: (Int) -> Int) -> [Int] {
    return arr.map(transform)
}
apply([1, 2, 3]) { $0 * 2 }    // [2, 4, 6]

// Capturing values (closure = reference type)
func makeCounter() -> () -> Int {
    var count = 0
    return {
        count += 1
        return count
    }
}
let counter = makeCounter()
counter()  // 1
counter()  // 2
counter()  // 3
```

---

## 7. Struct, Class, Enum

### Struct (Value Type — PREFERRED di Swift)
```swift
struct Student {
    var name: String
    var age: Int
    var grade: Double

    // Computed property
    var isHonor: Bool {
        return grade >= 3.5
    }

    // Method
    func info() -> String {
        return "\(name), age \(age)"
    }

    // Mutating method (wajib jika modify property)
    mutating func setAge(_ age: Int) {
        self.age = age
    }
}

// Inisialisasi (auto memberwise initializer)
var s1 = Student(name: "Alice", age: 20, grade: 3.5)
s1.name         // "Alice"
s1.info()       // "Alice, age 20"
s1.setAge(21)   // s1.age = 21

// Struct = VALUE TYPE (copy saat assign)
var s2 = s1     // s2 adalah COPY
s2.name = "Bob" // s1.name tetap "Alice"
```

### Class (Reference Type)
```swift
class Animal {
    var name: String
    var sound: String

    // Harus punya initializer
    init(name: String, sound: String) {
        self.name = name
        self.sound = sound
    }

    func speak() -> String {
        return "\(name) says \(sound)"
    }

    // deinit dipanggil saat object dihapus dari memory
    deinit {
        print("\(name) is being deallocated")
    }
}

// Inheritance
class Dog: Animal {
    var breed: String

    init(name: String, breed: String) {
        self.breed = breed
        super.init(name: name, sound: "Woof")
    }

    // Override method
    override func speak() -> String {
        return "\(name) the \(breed) says \(sound)!"
    }
}

let dog = Dog(name: "Buddy", breed: "Labrador")
dog.speak()  // "Buddy the Labrador says Woof!"

// Class = REFERENCE TYPE
let a = Animal(name: "Cat", sound: "Meow")
let b = a           // b REFERENSI ke objek yang sama!
b.name = "Dog"      // a.name juga berubah jadi "Dog"!
```

### Enum
```swift
// Basic enum
enum Direction {
    case north, south, east, west
}
var dir = Direction.north
dir = .south            // shorthand

// Enum dengan raw value
enum Planet: Int {
    case mercury = 1, venus, earth, mars  // auto increment
}
Planet.earth.rawValue   // 3
Planet(rawValue: 1)     // Optional(Planet.mercury)

// Enum dengan String raw value
enum Color: String {
    case red = "RED"
    case green = "GREEN"
    case blue = "BLUE"
}
Color.red.rawValue      // "RED"

// Enum dengan associated values
enum Result {
    case success(data: String)
    case failure(error: String)
}

let r = Result.success(data: "OK")
switch r {
case .success(let data):
    print("Success: \(data)")
case .failure(let error):
    print("Error: \(error)")
}

// Enum dengan method
enum Coin: Double {
    case penny = 0.01
    case nickel = 0.05
    case dime = 0.10
    case quarter = 0.25

    func description() -> String {
        return "Worth $\(self.rawValue)"
    }
}
```

### Protocol (Interface di Swift)
```swift
protocol Shape {
    var area: Double { get }
    func perimeter() -> Double
}

struct Circle: Shape {
    var radius: Double

    var area: Double {
        return .pi * radius * radius
    }

    func perimeter() -> Double {
        return 2 * .pi * radius
    }
}

struct Rectangle: Shape {
    var width: Double
    var height: Double

    var area: Double {
        return width * height
    }

    func perimeter() -> Double {
        return 2 * (width + height)
    }
}

// Polymorphism
func printArea(_ shape: Shape) {
    print("Area: \(shape.area)")
}

printArea(Circle(radius: 5))
printArea(Rectangle(width: 3, height: 4))

// Protocol extension (default implementation)
extension Shape {
    func describe() -> String {
        return "Area: \(area), Perimeter: \(perimeter())"
    }
}
```

---

## 8. Optionals

### Dasar Optional
```swift
// Optional = bisa punya value ATAU nil
var name: String? = "Alice"     // Optional("Alice")
var age: Int? = nil              // nil

// Force unwrap (BERBAHAYA — crash jika nil!)
let n: String = name!           // "Alice"

// Optional binding (SAFE — recommended)
if let name = name {
    print("Hello, \(name)")     // "Hello, Alice"
} else {
    print("No name")
}

// Guard let (early return)
func greet(_ name: String?) {
    guard let name = name else {
        print("No name")
        return
    }
    print("Hello, \(name)")
}

// Nil coalescing (default value)
let displayName = name ?? "Anonymous"  // "Alice" atau "Anonymous"

// Optional chaining
let count = name?.count         // Optional(5)
let upper = name?.uppercased()  // Optional("ALICE")

// Multiple optional binding
if let a = optA, let b = optB, a > 0 {
    print("\(a) + \(b)")
}
```

---

## 9. Sorting & Searching

### Sort Array
```swift
// Sort in-place (mutating)
var nums = [5, 3, 1, 4, 2]
nums.sort()                             // [1, 2, 3, 4, 5]
nums.sort(by: >)                        // [5, 4, 3, 2, 1] (descending)

// Sorted (return new array, non-mutating)
let sorted = nums.sorted()             // [1, 2, 3, 4, 5]
let sortedDesc = nums.sorted(by: >)    // [5, 4, 3, 2, 1]

// Sort string array
var words = ["banana", "apple", "cherry"]
words.sort()                            // ["apple", "banana", "cherry"]
```

### Custom Sort
```swift
struct Person {
    let name: String
    let age: Int
}

var people = [
    Person(name: "Alice", age: 25),
    Person(name: "Bob", age: 20),
    Person(name: "Charlie", age: 30)
]

// Sort by age ascending
people.sort { $0.age < $1.age }

// Sort by age descending
people.sort { $0.age > $1.age }

// Sort by multiple keys
people.sort {
    if $0.age != $1.age {
        return $0.age < $1.age
    }
    return $0.name < $1.name
}
```

### Binary Search
```swift
// Manual binary search (array harus sudah terurut!)
func binarySearch(_ arr: [Int], _ target: Int) -> Int {
    var lo = 0, hi = arr.count - 1
    while lo <= hi {
        let mid = lo + (hi - lo) / 2
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

// Lower bound (index pertama >= target)
func lowerBound(_ arr: [Int], _ target: Int) -> Int {
    var lo = 0, hi = arr.count
    while lo < hi {
        let mid = lo + (hi - lo) / 2
        if arr[mid] < target {
            lo = mid + 1
        } else {
            hi = mid
        }
    }
    return lo
}
```

---

## 10. Math Operations

### Arithmetic
```swift
let a = 10, b = 3

a + b       // 13
a - b       // 7
a * b       // 30
a / b       // 3 (integer division)
a % b       // 1 (modulo)

// Float division
Double(a) / Double(b)   // 3.333...

// CATATAN: Swift TIDAK punya operator ** (power)
// Pakai pow() dari Foundation
```

### Foundation Math
```swift
import Foundation

abs(-5)                 // 5 (works for Int AND Double!)
sqrt(16.0)              // 4.0
pow(2.0, 10.0)          // 1024.0
log(M_E)                // 1.0 (ln)
log2(8.0)               // 3.0
log10(100.0)            // 2.0

floor(3.7)              // 3.0
ceil(3.2)               // 4.0
round(3.5)              // 4.0

max(3, 7)               // 7 (built-in, works for any Comparable!)
min(3, 7)               // 3

Int.max                 // 9223372036854775807
Int.min                 // -9223372036854775808
Double.infinity         // +Inf
Double.pi               // 3.141592653589793
```

### Integer Helper Functions
```swift
// GCD (Greatest Common Divisor)
func gcd(_ a: Int, _ b: Int) -> Int {
    var a = a, b = b
    while b != 0 {
        (a, b) = (b, a % b)
    }
    return a
}

// LCM (Least Common Multiple)
func lcm(_ a: Int, _ b: Int) -> Int {
    return a / gcd(a, b) * b
}

// Power (integer version)
func power(_ base: Int, _ exp: Int) -> Int {
    var result = 1
    for _ in 0..<exp {
        result *= base
    }
    return result
}

// Fast Power (modular)
func fastPow(_ base: Int, _ exp: Int, _ mod: Int) -> Int {
    var result = 1
    var base = base % mod
    var exp = exp
    while exp > 0 {
        if exp % 2 == 1 {
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

### Print (Output)
```swift
print("Hello World")                    // + newline
print("Hello", terminator: "")          // tanpa newline
print("x =", 10)                        // "x = 10"
print("a", "b", "c", separator: "-")    // "a-b-c"

// String interpolation (RECOMMENDED)
let name = "Alice"
let age = 20
print("\(name) is \(age) years old")

// Format string
let formatted = String(format: "%.2f", 3.14159)  // "3.14"
let padded = String(format: "%05d", 42)           // "00042"
```

### ReadLine (Input)
```swift
// Baca input dari stdin
if let input = readLine() {
    print("You said: \(input)")
}

// Baca integer
if let line = readLine(), let n = Int(line) {
    print("Number: \(n)")
}

// Baca banyak integer dari 1 baris
if let line = readLine() {
    let nums = line.split(separator: " ").compactMap { Int($0) }
    print(nums)  // [1, 2, 3, ...]
}

// Template competitive programming
let t = Int(readLine()!)!
for _ in 0..<t {
    let n = Int(readLine()!)!
    let arr = readLine()!.split(separator: " ").map { Int($0)! }
    // ... solve ...
    print(answer)
}
```

---

## 12. Common Patterns untuk Problem Solving

### Two Pointers
```swift
// Cek palindrome
func isPalindrome(_ s: String) -> Bool {
    let chars = Array(s)
    var i = 0, j = chars.count - 1
    while i < j {
        if chars[i] != chars[j] { return false }
        i += 1; j -= 1
    }
    return true
}

// Two Sum (sorted array)
func twoSum(_ nums: [Int], _ target: Int) -> (Int, Int) {
    var lo = 0, hi = nums.count - 1
    while lo < hi {
        let sum = nums[lo] + nums[hi]
        if sum == target { return (lo, hi) }
        else if sum < target { lo += 1 }
        else { hi -= 1 }
    }
    return (-1, -1)
}
```

### Sliding Window
```swift
// Max sum subarray of size k
func maxSumSubarray(_ arr: [Int], _ k: Int) -> Int {
    var windowSum = arr[0..<k].reduce(0, +)
    var maxSum = windowSum
    for i in k..<arr.count {
        windowSum += arr[i] - arr[i - k]
        maxSum = max(maxSum, windowSum)
    }
    return maxSum
}
```

### Frequency Counter
```swift
func topKFrequent(_ words: [String], _ k: Int) -> [String] {
    var freq = [String: Int]()
    for w in words { freq[w, default: 0] += 1 }

    let sorted = freq.sorted {
        if $0.value != $1.value { return $0.value > $1.value }
        return $0.key < $1.key
    }
    return sorted.prefix(k).map { $0.key }
}
```

### Stack
```swift
// Swift tidak punya built-in stack, pakai Array
var stack = [Int]()

// Push
stack.append(10)

// Pop
let top = stack.removeLast()

// Peek
let top = stack.last!

// IsEmpty
stack.isEmpty

// Contoh: Valid Parentheses
func isValid(_ s: String) -> Bool {
    var stack = [Character]()
    let pairs: [Character: Character] = [")": "(", "]": "[", "}": "{"]
    for ch in s {
        if ch == "(" || ch == "[" || ch == "{" {
            stack.append(ch)
        } else {
            if stack.isEmpty || stack.last != pairs[ch] { return false }
            stack.removeLast()
        }
    }
    return stack.isEmpty
}
```

### Queue
```swift
// Simple queue pakai Array
var queue = [Int]()
queue.append(10)                // enqueue
let front = queue.removeFirst() // dequeue (O(n))

// Efficient queue pakai 2 stacks
struct Queue<T> {
    private var inStack = [T]()
    private var outStack = [T]()

    mutating func enqueue(_ val: T) { inStack.append(val) }

    mutating func dequeue() -> T? {
        if outStack.isEmpty {
            outStack = inStack.reversed()
            inStack.removeAll()
        }
        return outStack.popLast()
    }

    var isEmpty: Bool { inStack.isEmpty && outStack.isEmpty }
}
```

### BFS
```swift
func bfs(_ graph: [Int: [Int]], _ start: Int) -> [Int] {
    var visited = Set<Int>()
    var queue = [start]
    visited.insert(start)
    var result = [Int]()

    while !queue.isEmpty {
        let node = queue.removeFirst()
        result.append(node)
        for neighbor in graph[node, default: []] {
            if !visited.contains(neighbor) {
                visited.insert(neighbor)
                queue.append(neighbor)
            }
        }
    }
    return result
}
```

### DFS
```swift
// Recursive
func dfs(_ graph: [Int: [Int]], _ node: Int, _ visited: inout Set<Int>) {
    visited.insert(node)
    print(node)
    for neighbor in graph[node, default: []] {
        if !visited.contains(neighbor) {
            dfs(graph, neighbor, &visited)
        }
    }
}

// Iterative (pakai stack)
func dfsIterative(_ graph: [Int: [Int]], _ start: Int) {
    var visited = Set<Int>()
    var stack = [start]
    while !stack.isEmpty {
        let node = stack.removeLast()
        if visited.contains(node) { continue }
        visited.insert(node)
        print(node)
        for neighbor in graph[node, default: []] {
            if !visited.contains(neighbor) { stack.append(neighbor) }
        }
    }
}
```

### Dynamic Programming
```swift
// Fibonacci dengan memoization
func fib(_ n: Int, _ memo: inout [Int: Int]) -> Int {
    if n <= 1 { return n }
    if let val = memo[n] { return val }
    memo[n] = fib(n - 1, &memo) + fib(n - 2, &memo)
    return memo[n]!
}

// Fibonacci bottom-up (tabulation)
func fibBottomUp(_ n: Int) -> Int {
    if n <= 1 { return n }
    var dp = Array(repeating: 0, count: n + 1)
    dp[0] = 0; dp[1] = 1
    for i in 2...n { dp[i] = dp[i - 1] + dp[i - 2] }
    return dp[n]
}

// Space optimized
func fibOptimized(_ n: Int) -> Int {
    if n <= 1 { return n }
    var a = 0, b = 1
    for _ in 2...n { (a, b) = (b, a + b) }
    return b
}
```

---

## 13. Tips & Tricks

### Swift-Specific Tips
```swift
// 1. Swap
swap(&a, &b)
// atau: (a, b) = (b, a)

// 2. Nil coalescing chain
let name = firstName ?? lastName ?? "Anonymous"

// 3. Map/Filter/Reduce chain
let result = arr.filter { $0 > 0 }.map { $0 * 2 }.reduce(0, +)

// 4. Zip (gabung 2 array)
for (name, age) in zip(names, ages) {
    print("\(name): \(age)")
}

// 5. Guard let (early exit pattern)
guard let data = fetchData() else { return }

// 6. Defer (cleanup, mirip Go)
func readFile() {
    let file = openFile()
    defer { file.close() }
}

// 7. where clause di loop
for i in 0..<100 where i % 3 == 0 {
    print(i)  // 0, 3, 6, 9, ...
}

// 8. Min/Max dari array
arr.min()   // Optional(Int)
arr.max()   // Optional(Int)

// 9. Sum array
arr.reduce(0, +)

// 10. Cek apakah angka prima
func isPrime(_ n: Int) -> Bool {
    if n < 2 { return false }
    if n < 4 { return true }
    var i = 2
    while i * i <= n {
        if n % i == 0 { return false }
        i += 1
    }
    return true
}

// 11. Digit sum
func digitSum(_ n: Int) -> Int {
    var n = abs(n), sum = 0
    while n > 0 { sum += n % 10; n /= 10 }
    return sum
}

// 12. Remove duplicates (menjaga urutan)
var seen = Set<Int>()
let unique = arr.filter { seen.insert($0).inserted }
```

### Perbedaan Penting Swift vs Go
| Aspek | Go | Swift |
|-------|-----|-------|
| Typing | Static | Static |
| Null safety | Pointer nil | Optional system |
| Error handling | Error return | throws / Optional |
| OOP | Struct + Interface | Struct + Class + Protocol |
| Inheritance | Embedding | Class inheritance |
| Value/Ref types | Struct=val, Pointer=ref | Struct=val, Class=ref |
| Generics | Go 1.18+ | Built-in |
| String indexing | By int | By String.Index |
| Enum | iota constants | Full-featured enum |
| Memory | GC | ARC |
| Concurrency | Goroutines | async/await + Actor |
| Switch break | Auto break | Auto break |

### Template Competitive Programming (Swift)
```swift
import Foundation

let t = Int(readLine()!)!

for _ in 0..<t {
    solve()
}

func solve() {
    let n = Int(readLine()!)!
    let arr = readLine()!.split(separator: " ").map { Int($0)! }

    // ... logika solusi ...

    print(answer)
}

// Helper functions
func gcd(_ a: Int, _ b: Int) -> Int {
    var a = a, b = b
    while b != 0 { (a, b) = (b, a % b) }
    return a
}

func lcm(_ a: Int, _ b: Int) -> Int {
    return a / gcd(a, b) * b
}

func isPrime(_ n: Int) -> Bool {
    if n < 2 { return false }
    var i = 2
    while i * i <= n {
        if n % i == 0 { return false }
        i += 1
    }
    return true
}
```
