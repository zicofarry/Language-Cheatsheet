# ⚡ JAVASCRIPT CHEATSHEET LENGKAP
## Untuk Tes Coding & Problem Solving

---

## Daftar Isi
1. [Dasar-Dasar JS](#1-dasar-dasar-js)
2. [Tipe Data & Variabel](#2-tipe-data--variabel)
3. [String Operations](#3-string-operations)
4. [Array](#4-array)
5. [Object & Map](#5-object--map)
6. [Set](#6-set)
7. [Control Flow](#7-control-flow)
8. [Functions](#8-functions)
9. [Destructuring & Spread](#9-destructuring--spread)
10. [Sorting & Searching](#10-sorting--searching)
11. [Math Operations](#11-math-operations)
12. [Input/Output (HackerEarth / Node.js)](#12-inputoutput)
13. [Common Patterns untuk Problem Solving](#13-common-patterns-untuk-problem-solving)
14. [Tips & Tricks](#14-tips--tricks)

---

## 1. Dasar-Dasar JS

### Deklarasi Variabel
```javascript
// let: bisa di-reassign, block-scoped
let x = 10;
x = 20;  // OK

// const: tidak bisa di-reassign, block-scoped
const PI = 3.14;
// PI = 3;  // ERROR!

// TAPI const object/array isinya masih bisa diubah
const arr = [1, 2, 3];
arr.push(4);      // OK — isinya berubah, referensinya tidak
// arr = [5, 6];  // ERROR — reassign referensi

// var: HINDARI! function-scoped, bisa hoisted
var y = 10;

// Rekomendasi: pakai const default, let jika perlu reassign
```

### Semicolon
```javascript
// Opsional di JS (auto-inserted), tapi RECOMMENDED pakai
let x = 10;   // OK
let y = 20    // OK juga, tapi rawan bug
```

---

## 2. Tipe Data & Variabel

### Tipe Data Primitif
```javascript
// Number (integer & float jadi satu)
let n = 42;
let f = 3.14;
let big = 1e6;            // 1000000
let inf = Infinity;
let negInf = -Infinity;
let nan = NaN;             // Not a Number

// String
let s = "hello";
let s2 = 'hello';         // sama saja
let s3 = `hello ${name}`; // template literal

// Boolean
let b = true;
let b2 = false;

// Null & Undefined
let a = null;              // kosong (sengaja)
let b;                     // undefined (belum diisi)

// BigInt (angka besar)
let big = 9007199254740991n;
let big2 = BigInt("99999999999999999999");
```

### Cek Tipe Data
```javascript
typeof 42           // "number"
typeof "hello"      // "string"
typeof true         // "boolean"
typeof undefined    // "undefined"
typeof null         // "object" (bug historis JS!)
typeof []           // "object"
typeof {}           // "object"

// Cek array
Array.isArray([1,2])    // true
Array.isArray("hello")  // false

// Cek NaN
isNaN(NaN)              // true
Number.isNaN(NaN)       // true (lebih strict)

// Cek finite
Number.isFinite(42)     // true
Number.isFinite(Infinity) // false

// Cek integer
Number.isInteger(42)    // true
Number.isInteger(42.5)  // false
```

### Konversi Tipe
```javascript
// Ke Number
Number("42")           // 42
Number("3.14")         // 3.14
Number("abc")          // NaN
Number(true)           // 1
Number(false)          // 0
Number(null)           // 0
Number(undefined)      // NaN
parseInt("42")         // 42
parseInt("42.9")       // 42 (truncate)
parseInt("0b1010", 2)  // 10 (binary)
parseInt("ff", 16)     // 255 (hex)
parseFloat("3.14")     // 3.14
+"42"                  // 42 (unary plus, shortcut)

// Ke String
String(42)             // "42"
(42).toString()        // "42"
(255).toString(16)     // "ff" (hex)
(10).toString(2)       // "1010" (binary)
`${42}`                // "42" (template literal)

// Ke Boolean
Boolean(0)             // false
Boolean("")            // false
Boolean(null)          // false
Boolean(undefined)     // false
Boolean(NaN)           // false
Boolean(1)             // true
Boolean("abc")         // true
Boolean([])            // true  (⚠️ JEBAKAN! array kosong = truthy)
Boolean({})            // true  (⚠️ object kosong = truthy)
!!value                // shortcut ke boolean
```

### Falsy Values (PENTING!)
```javascript
// Hanya 6 nilai yang falsy:
false, 0, "", null, undefined, NaN

// Semua yang lain TRUTHY termasuk:
// [], {}, "0", "false", -1, Infinity → semuanya TRUTHY!
```

---

## 3. String Operations

### Operasi Dasar
```javascript
let s = "Hello World";

s.length                        // 11
s[0]                            // "H"
s[s.length - 1]                 // "d"
s.charAt(0)                     // "H"

// Slicing
s.slice(0, 5)                   // "Hello"
s.slice(6)                      // "World"
s.slice(-5)                     // "World" (dari belakang)
s.substring(0, 5)               // "Hello" (mirip slice, tapi no negatif)

// String IMMUTABLE di JS
// s[0] = "h";  // TIDAK BERUBAH! (silent fail)
```

### Transformasi
```javascript
s.toUpperCase()                 // "HELLO WORLD"
s.toLowerCase()                 // "hello world"
s.trim()                        // hapus whitespace kiri-kanan
s.trimStart()                   // hapus whitespace kiri
s.trimEnd()                     // hapus whitespace kanan
s.padStart(15, "*")             // "****Hello World"
s.padEnd(15, "*")               // "Hello World****"
s.repeat(3)                     // "Hello WorldHello WorldHello World"

// Replace
s.replace("World", "JS")       // "Hello JS" (pertama saja)
s.replaceAll("l", "L")         // "HeLLo WorLd" (semua)
```

### Find & Check
```javascript
s.includes("World")             // true
s.startsWith("Hello")           // true
s.endsWith("World")             // true
s.indexOf("World")              // 6 (-1 jika tidak ada)
s.lastIndexOf("l")              // 9

// Search (regex support)
s.search(/world/i)              // 6 (case-insensitive)
s.match(/l/g)                   // ["l", "l", "l"] (semua match)
```

### Split & Join
```javascript
"a,b,c".split(",")              // ["a", "b", "c"]
"hello".split("")               // ["h", "e", "l", "l", "o"]
"  hi  there  ".split(/\s+/)    // ["", "hi", "there", ""]
["a", "b", "c"].join("-")       // "a-b-c"
["a", "b", "c"].join("")        // "abc"
```

### Template Literals
```javascript
let name = "Alice";
let age = 25;

// Interpolasi
`Name: ${name}, Age: ${age}`    // "Name: Alice, Age: 25"

// Ekspresi
`Total: ${10 + 20}`             // "Total: 30"
`Even: ${4 % 2 === 0}`          // "Even: true"

// Multi-line
let msg = `
  Line 1
  Line 2
`;
```

### Reverse String
```javascript
let reversed = s.split("").reverse().join("");
// atau
let reversed = [...s].reverse().join("");
```

### Char Code
```javascript
"A".charCodeAt(0)               // 65
String.fromCharCode(65)         // "A"

// Cek apakah huruf/angka
let ch = "A";
ch >= 'a' && ch <= 'z'          // lowercase?
ch >= 'A' && ch <= 'Z'          // uppercase?
ch >= '0' && ch <= '9'          // digit?
/[a-zA-Z]/.test(ch)             // letter?
```

---

## 4. Array

### Deklarasi & Akses
```javascript
let arr = [1, 2, 3, 4, 5];
let arr2 = new Array(5).fill(0);     // [0, 0, 0, 0, 0]
let arr3 = Array.from({length: 5}, (_, i) => i); // [0, 1, 2, 3, 4]

arr[0]                  // 1
arr[arr.length - 1]     // 5
arr.length              // 5
```

### Modifikasi
```javascript
// Tambah/Hapus di akhir
arr.push(6)             // [1,2,3,4,5,6] — return length baru
arr.pop()               // return 6, arr = [1,2,3,4,5]

// Tambah/Hapus di awal
arr.unshift(0)          // [0,1,2,3,4,5] — return length baru
arr.shift()             // return 0, arr = [1,2,3,4,5]

// Splice (insert/delete di posisi tertentu)
arr.splice(2, 1)        // hapus 1 elemen di index 2 → return [3]
arr.splice(2, 0, 99)    // insert 99 di index 2, hapus 0
arr.splice(1, 2, 10, 20)// ganti 2 elemen dari index 1 dengan 10,20

// Concat
let merged = arr1.concat(arr2);
let merged2 = [...arr1, ...arr2];  // spread operator

// Slice (tidak mengubah array asli)
arr.slice(1, 3)         // [2, 3] — elemen index 1 sampai 2
arr.slice(-2)           // [4, 5] — 2 elemen terakhir

// Fill
arr.fill(0)             // [0, 0, 0, 0, 0]
arr.fill(9, 1, 3)       // [0, 9, 9, 0, 0] — fill index 1 sampai 2

// Flat (ratakan nested array)
[1, [2, [3, 4]]].flat()       // [1, 2, [3, 4]]
[1, [2, [3, 4]]].flat(Infinity) // [1, 2, 3, 4]

// Reverse (in-place)
arr.reverse()

// Copy
let copy = [...arr];
let copy2 = arr.slice();
let copy3 = Array.from(arr);
```

### Iterasi & Transform (SANGAT PENTING!)
```javascript
let nums = [1, 2, 3, 4, 5];

// forEach — loop tanpa return
nums.forEach((val, idx) => {
    console.log(idx, val);
});

// map — transform setiap elemen, return array baru
nums.map(x => x * 2)            // [2, 4, 6, 8, 10]
nums.map((val, i) => val + i)    // [1, 3, 5, 7, 9]

// filter — ambil elemen yang memenuhi kondisi
nums.filter(x => x > 3)         // [4, 5]
nums.filter(x => x % 2 === 0)   // [2, 4]

// reduce — akumulasi jadi satu nilai
nums.reduce((acc, val) => acc + val, 0)      // 15 (sum)
nums.reduce((acc, val) => acc * val, 1)      // 120 (product)
nums.reduce((a, b) => Math.max(a, b))        // 5 (max)

// find — cari elemen pertama yang cocok
nums.find(x => x > 3)           // 4

// findIndex — cari index pertama yang cocok
nums.findIndex(x => x > 3)      // 3

// some — ada setidaknya 1 yang cocok?
nums.some(x => x > 4)           // true

// every — semua cocok?
nums.every(x => x > 0)          // true

// includes — ada nilai tertentu?
nums.includes(3)                 // true

// indexOf — posisi pertama (-1 jika tidak ada)
nums.indexOf(3)                  // 2
nums.lastIndexOf(3)              // 2

// flatMap — map + flat level 1
[[1,2], [3,4]].flatMap(x => x)          // [1, 2, 3, 4]
[1,2,3].flatMap(x => [x, x * 2])       // [1, 2, 2, 4, 3, 6]
```

### 2D Array (Matrix)
```javascript
// Buat matrix n x m
let n = 3, m = 4;
let matrix = Array.from({length: n}, () => new Array(m).fill(0));

// ⚠️ JANGAN: new Array(n).fill(new Array(m).fill(0))
// ↑ Semua row jadi referensi yang sama!

// Akses
matrix[0][0] = 1;

// Iterate
for (let i = 0; i < n; i++) {
    for (let j = 0; j < m; j++) {
        process(matrix[i][j]);
    }
}
```

---

## 5. Object & Map

### Object (Key-Value dasar)
```javascript
let obj = {
    name: "Alice",
    age: 25,
    greet() { return `Hi, I'm ${this.name}`; }
};

// Akses
obj.name                      // "Alice"
obj["name"]                   // "Alice" (bracket notation)
obj.greet()                   // "Hi, I'm Alice"

// CRUD
obj.city = "Jakarta";         // add
obj.age = 26;                 // update
delete obj.city;              // delete

// Check key exist
"name" in obj                 // true
obj.hasOwnProperty("name")   // true

// Iterate
Object.keys(obj)              // ["name", "age", "greet"]
Object.values(obj)            // ["Alice", 25, ...]
Object.entries(obj)           // [["name","Alice"], ["age",25], ...]

for (let [key, val] of Object.entries(obj)) {
    console.log(key, val);
}

// Merge / Copy
let merged = {...obj, city: "Jakarta"};
let copy = {...obj};
let merged2 = Object.assign({}, obj, {city: "Jakarta"});
```

### Map (Key-Value canggih)
```javascript
// Map vs Object:
// - Map: key bisa tipe apa saja (number, object, dll)
// - Map: menjaga urutan insert
// - Map: punya .size
// - Map: performa lebih baik untuk frequent insert/delete

let m = new Map();

// CRUD
m.set("name", "Alice");
m.set(1, "one");
m.get("name")                // "Alice"
m.has("name")                // true
m.delete("name");
m.clear();                   // hapus semua
m.size                       // jumlah entries

// Iterate
for (let [key, val] of m) {
    console.log(key, val);
}
m.forEach((val, key) => console.log(key, val));

// Dari array ke Map
let m2 = new Map([["a", 1], ["b", 2]]);

// Frequency counter dengan Map
let freq = new Map();
for (let ch of "hello") {
    freq.set(ch, (freq.get(ch) || 0) + 1);
}
// Map { 'h' => 1, 'e' => 1, 'l' => 2, 'o' => 1 }
```

### Frequency Counter (pakai Object)
```javascript
// Pakai Object (lebih simpel untuk string key)
let freq = {};
for (let ch of "hello") {
    freq[ch] = (freq[ch] || 0) + 1;
}
// { h: 1, e: 1, l: 2, o: 1 }
```

---

## 6. Set

```javascript
let s = new Set();

// Add & Delete
s.add(1);
s.add(2);
s.add(2);               // duplikat diabaikan
s.has(1)                 // true
s.delete(1);
s.clear();
s.size                   // jumlah elemen

// Dari array (hapus duplikat!)
let unique = [...new Set([1, 2, 2, 3, 3, 3])];  // [1, 2, 3]

// Iterate
for (let val of s) {
    console.log(val);
}

// Set operations (manual)
let a = new Set([1, 2, 3]);
let b = new Set([2, 3, 4]);

// Union
let union = new Set([...a, ...b]);           // {1, 2, 3, 4}

// Intersection
let inter = new Set([...a].filter(x => b.has(x)));  // {2, 3}

// Difference
let diff = new Set([...a].filter(x => !b.has(x)));  // {1}
```

---

## 7. Control Flow

### If-Else
```javascript
if (x > 0) {
    console.log("Positive");
} else if (x < 0) {
    console.log("Negative");
} else {
    console.log("Zero");
}

// Ternary
let result = x > 0 ? "Positive" : "Negative";

// Nested ternary
let grade = x >= 90 ? "A" : x >= 80 ? "B" : x >= 70 ? "C" : "D";

// Nullish coalescing (??)
let val = null ?? "default";       // "default"
let val2 = 0 ?? "default";        // 0 (hanya null/undefined trigger ??)

// Optional chaining (?.)
let name = user?.profile?.name;    // undefined jika user/profile null

// Short-circuit
let name = user && user.name;     // undefined jika user falsy
let val = input || "default";     // "default" jika input falsy
```

### Switch
```javascript
switch (day) {
    case "Monday":
        console.log("Senin");
        break;                      // WAJIB break! (berbeda dari Go)
    case "Tuesday":
    case "Wednesday":               // fall-through
        console.log("Selasa/Rabu");
        break;
    default:
        console.log("Lainnya");
}
```

### For Loop
```javascript
// Classic
for (let i = 0; i < 10; i++) {
    console.log(i);
}

// for...of (iterate values — array, string, map, set)
for (let val of [1, 2, 3]) { console.log(val); }
for (let ch of "hello") { console.log(ch); }
for (let [k, v] of map) { console.log(k, v); }

// for...in (iterate keys — object)
for (let key in obj) { console.log(key, obj[key]); }
// ⚠️ JANGAN pakai for...in untuk array! Pakai for...of

// while
let i = 0;
while (i < 10) {
    console.log(i);
    i++;
}

// do...while (minimal 1x eksekusi)
do {
    console.log(i);
    i++;
} while (i < 10);

// Break & Continue
for (let i = 0; i < 10; i++) {
    if (i === 5) break;
    if (i % 2 === 0) continue;
    console.log(i);  // 1, 3
}
```

---

## 8. Functions

### Deklarasi
```javascript
// Function declaration (hoisted)
function add(a, b) {
    return a + b;
}

// Function expression
const add = function(a, b) {
    return a + b;
};

// Arrow function (RECOMMENDED untuk callback)
const add = (a, b) => a + b;
const square = x => x * x;         // 1 param: kurung opsional
const greet = () => "Hello";       // 0 param: kurung wajib
const multi = (a, b) => {          // multi-line: kurung kurawal
    let result = a * b;
    return result;
};

// Default parameter
function greet(name = "World") {
    return `Hello, ${name}!`;
}

// Rest parameter (variadic)
function sum(...nums) {
    return nums.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4)  // 10
```

### Callback & Higher-Order Functions
```javascript
// Function sebagai parameter
function apply(arr, fn) {
    return arr.map(fn);
}
apply([1, 2, 3], x => x * 2);  // [2, 4, 6]

// IIFE (Immediately Invoked Function Expression)
(function() {
    console.log("langsung jalan!");
})();

(() => {
    console.log("arrow IIFE");
})();
```

### Closure
```javascript
function counter() {
    let count = 0;
    return {
        increment: () => ++count,
        getCount: () => count
    };
}

const c = counter();
c.increment();  // 1
c.increment();  // 2
c.getCount();   // 2
```

---

## 9. Destructuring & Spread

### Array Destructuring
```javascript
let [a, b, c] = [1, 2, 3];       // a=1, b=2, c=3
let [first, ...rest] = [1,2,3,4]; // first=1, rest=[2,3,4]
let [x, , z] = [1, 2, 3];        // x=1, z=3 (skip elemen kedua)

// Swap
[a, b] = [b, a];

// Default values
let [p = 0, q = 0] = [1];        // p=1, q=0
```

### Object Destructuring
```javascript
let {name, age} = {name: "Alice", age: 25};
let {name: n, age: a} = {name: "Alice", age: 25}; // rename
let {name, ...rest} = {name: "Alice", age: 25, city: "JKT"};
// name = "Alice", rest = {age: 25, city: "JKT"}
```

### Spread Operator (...)
```javascript
// Array spread
let arr1 = [1, 2, 3];
let arr2 = [...arr1, 4, 5];      // [1, 2, 3, 4, 5]
Math.max(...arr1)                  // 3

// Object spread
let obj1 = {a: 1, b: 2};
let obj2 = {...obj1, c: 3};       // {a: 1, b: 2, c: 3}

// Function arguments
function sum(a, b, c) { return a + b + c; }
sum(...[1, 2, 3])                  // 6
```

---

## 10. Sorting & Searching

### Array Sort
```javascript
// ⚠️ JEBAKAN BESAR: sort() default adalah LEXICOGRAPHIC (string)!
[10, 2, 30, 1].sort()             // [1, 10, 2, 30] ← SALAH!

// Sort angka ascending — SELALU pakai comparator!
[10, 2, 30, 1].sort((a, b) => a - b)    // [1, 2, 10, 30] ✅

// Sort angka descending
[10, 2, 30, 1].sort((a, b) => b - a)    // [30, 10, 2, 1]

// Sort string (case-insensitive)
["Banana", "apple", "Cherry"].sort((a, b) =>
    a.toLowerCase().localeCompare(b.toLowerCase())
)

// Sort by string (default OK)
["banana", "apple", "cherry"].sort()     // ["apple", "banana", "cherry"]
```

### Custom Sort
```javascript
let people = [
    {name: "Alice", age: 25},
    {name: "Bob", age: 20},
    {name: "Charlie", age: 25}
];

// Sort by age ascending
people.sort((a, b) => a.age - b.age);

// Sort by age descending
people.sort((a, b) => b.age - a.age);

// Sort by multiple keys (age asc, lalu name asc)
people.sort((a, b) => {
    if (a.age !== b.age) return a.age - b.age;
    return a.name.localeCompare(b.name);
});

// Sort tuple array
let data = [[6, 55], [4, 40], [6, 45], [10, 50]];

// Elemen pertama descending, kedua ascending
data.sort((a, b) => {
    if (a[0] !== b[0]) return b[0] - a[0];
    return a[1] - b[1];
});
// [[10,50], [6,45], [6,55], [4,40]]
```

### Binary Search
```javascript
function binarySearch(arr, target) {
    let lo = 0, hi = arr.length - 1;
    while (lo <= hi) {
        let mid = Math.floor((lo + hi) / 2);
        if (arr[mid] === target) return mid;
        else if (arr[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}

// Lower bound (posisi insert)
function lowerBound(arr, target) {
    let lo = 0, hi = arr.length;
    while (lo < hi) {
        let mid = Math.floor((lo + hi) / 2);
        if (arr[mid] < target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}
```

---

## 11. Math Operations

### Built-in Math
```javascript
Math.abs(-5)             // 5
Math.sqrt(16)            // 4
Math.pow(2, 10)          // 1024
2 ** 10                  // 1024 (operator **)
Math.log(Math.E)         // 1 (ln)
Math.log2(8)             // 3
Math.log10(100)          // 2

Math.floor(3.7)          // 3
Math.ceil(3.2)           // 4
Math.round(3.5)          // 4
Math.trunc(3.7)          // 3 (buang desimal)
Math.trunc(-3.7)         // -3

Math.max(1, 2, 3)        // 3
Math.min(1, 2, 3)        // 1
Math.max(...arr)          // max dari array (spread!)
Math.min(...arr)          // min dari array

Math.PI                  // 3.141592653589793
Math.E                   // 2.718281828459045

Math.random()            // 0 <= x < 1
Math.floor(Math.random() * 10)  // random 0-9

// Modulo (remainder)
10 % 3                   // 1
-7 % 3                   // -1 (⚠️ JS modulo bisa negatif!)
((-7 % 3) + 3) % 3       // 2 (true modulo, selalu positif)
```

### Angka Besar (BigInt)
```javascript
// Number.MAX_SAFE_INTEGER = 9007199254740991

// Untuk angka lebih besar, pakai BigInt
let a = 999999999999999999n;
let b = 123456789012345678n;
a + b                    // 1123456789012345677n
a * b                    // BigInt result

// ⚠️ BigInt tidak bisa dicampur dengan Number biasa
// 1n + 2  // ERROR! harus 1n + 2n
```

### Common Math Functions
```javascript
// GCD
function gcd(a, b) {
    while (b) [a, b] = [b, a % b];
    return a;
}

// LCM
function lcm(a, b) {
    return (a / gcd(a, b)) * b;
}

// Check Prime
function isPrime(n) {
    if (n < 2) return false;
    for (let i = 2; i * i <= n; i++) {
        if (n % i === 0) return false;
    }
    return true;
}

// Sum of digits
function digitSum(n) {
    return String(Math.abs(n)).split("").reduce((a, b) => a + Number(b), 0);
}

// Factorial
function factorial(n) {
    let result = 1;
    for (let i = 2; i <= n; i++) result *= i;
    return result;
}
```

---

## 12. Input/Output

### Node.js / HackerEarth Template
```javascript
// === Cara 1: readline (paling umum di HackerEarth) ===
const readline = require('readline');
const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

let lines = [];
rl.on('line', (line) => {
    lines.push(line.trim());
});

rl.on('close', () => {
    let idx = 0;
    const nextLine = () => lines[idx++];

    let t = parseInt(nextLine());
    while (t--) {
        let n = parseInt(nextLine());
        let arr = nextLine().split(' ').map(Number);

        // ... solve ...

        console.log(answer);
    }
});
```

```javascript
// === Cara 2: process.stdin (lebih cepat) ===
let input = '';
process.stdin.on('data', d => input += d);
process.stdin.on('end', () => {
    let lines = input.trim().split('\n');
    let idx = 0;
    const next = () => lines[idx++].trim();

    let n = parseInt(next());
    let arr = next().split(' ').map(Number);

    // ... solve ...

    console.log(answer);
});
```

```javascript
// === Parse input helpers ===
// Baca 1 integer
let n = parseInt(line);

// Baca array of integers
let arr = line.split(' ').map(Number);

// Baca array of strings
let words = line.split(' ');

// Baca 2D array
let matrix = [];
for (let i = 0; i < n; i++) {
    matrix.push(nextLine().split(' ').map(Number));
}

// Output
console.log(value);              // otomatis newline
process.stdout.write(value);     // tanpa newline
console.log(arr.join(' '));      // array jadi "1 2 3"
```

---

## 13. Common Patterns untuk Problem Solving

### Two Pointers
```javascript
// Palindrome check
function isPalindrome(s) {
    let i = 0, j = s.length - 1;
    while (i < j) {
        if (s[i] !== s[j]) return false;
        i++;
        j--;
    }
    return true;
}

// Two Sum (sorted array)
function twoSum(nums, target) {
    let lo = 0, hi = nums.length - 1;
    while (lo < hi) {
        let sum = nums[lo] + nums[hi];
        if (sum === target) return [lo, hi];
        else if (sum < target) lo++;
        else hi--;
    }
    return [-1, -1];
}
```

### Sliding Window
```javascript
// Max sum subarray of size k
function maxSumSubarray(arr, k) {
    let windowSum = arr.slice(0, k).reduce((a, b) => a + b);
    let maxSum = windowSum;

    for (let i = k; i < arr.length; i++) {
        windowSum += arr[i] - arr[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

### Stack
```javascript
// JS array = natural stack
let stack = [];
stack.push(10);                  // push
stack.pop();                     // pop → return 10
stack[stack.length - 1];         // peek
stack.length === 0;              // isEmpty

// Valid Parentheses
function isValid(s) {
    let stack = [];
    let pairs = {')': '(', ']': '[', '}': '{'};

    for (let ch of s) {
        if ('([{'.includes(ch)) {
            stack.push(ch);
        } else {
            if (!stack.length || stack[stack.length-1] !== pairs[ch]) return false;
            stack.pop();
        }
    }
    return stack.length === 0;
}
```

### Queue
```javascript
// Simple queue (pakai array, shift() = O(n))
let queue = [];
queue.push(10);                  // enqueue
queue.shift();                   // dequeue → O(n)!

// Efficient queue (pakai object, O(1) dequeue)
class Queue {
    constructor() {
        this.items = {};
        this.head = 0;
        this.tail = 0;
    }
    enqueue(val) { this.items[this.tail++] = val; }
    dequeue() {
        if (this.isEmpty()) return undefined;
        let val = this.items[this.head];
        delete this.items[this.head++];
        return val;
    }
    peek() { return this.items[this.head]; }
    isEmpty() { return this.head === this.tail; }
    size() { return this.tail - this.head; }
}
```

### BFS
```javascript
function bfs(graph, start) {
    let visited = new Set([start]);
    let queue = [start];
    let result = [];

    while (queue.length > 0) {
        let node = queue.shift();
        result.push(node);

        for (let neighbor of (graph[node] || [])) {
            if (!visited.has(neighbor)) {
                visited.add(neighbor);
                queue.push(neighbor);
            }
        }
    }
    return result;
}
```

### DFS
```javascript
// Recursive
function dfs(graph, node, visited = new Set()) {
    visited.add(node);
    console.log(node);
    for (let neighbor of (graph[node] || [])) {
        if (!visited.has(neighbor)) {
            dfs(graph, neighbor, visited);
        }
    }
}

// Iterative
function dfsIterative(graph, start) {
    let visited = new Set();
    let stack = [start];

    while (stack.length > 0) {
        let node = stack.pop();
        if (visited.has(node)) continue;
        visited.add(node);
        console.log(node);
        for (let neighbor of (graph[node] || [])) {
            if (!visited.has(neighbor)) {
                stack.push(neighbor);
            }
        }
    }
}
```

### Dynamic Programming
```javascript
// Fibonacci (memoization — top-down)
function fib(n, memo = {}) {
    if (n <= 1) return n;
    if (memo[n]) return memo[n];
    memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
    return memo[n];
}

// Fibonacci (tabulation — bottom-up)
function fibTab(n) {
    if (n <= 1) return n;
    let dp = [0, 1];
    for (let i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}

// Space optimized
function fibOpt(n) {
    if (n <= 1) return n;
    let [a, b] = [0, 1];
    for (let i = 2; i <= n; i++) {
        [a, b] = [b, a + b];
    }
    return b;
}
```

### Heap / Priority Queue (Manual)
```javascript
// JS tidak punya built-in heap, harus bikin sendiri
class MinHeap {
    constructor() { this.heap = []; }

    push(val) {
        this.heap.push(val);
        this._bubbleUp(this.heap.length - 1);
    }

    pop() {
        let top = this.heap[0];
        let last = this.heap.pop();
        if (this.heap.length > 0) {
            this.heap[0] = last;
            this._sinkDown(0);
        }
        return top;
    }

    peek() { return this.heap[0]; }
    size() { return this.heap.length; }

    _bubbleUp(i) {
        while (i > 0) {
            let parent = Math.floor((i - 1) / 2);
            if (this.heap[parent] <= this.heap[i]) break;
            [this.heap[parent], this.heap[i]] = [this.heap[i], this.heap[parent]];
            i = parent;
        }
    }

    _sinkDown(i) {
        let n = this.heap.length;
        while (true) {
            let left = 2 * i + 1, right = 2 * i + 2, smallest = i;
            if (left < n && this.heap[left] < this.heap[smallest]) smallest = left;
            if (right < n && this.heap[right] < this.heap[smallest]) smallest = right;
            if (smallest === i) break;
            [this.heap[smallest], this.heap[i]] = [this.heap[i], this.heap[smallest]];
            i = smallest;
        }
    }
}
```

---

## 14. Tips & Tricks

### JS-Specific Gotchas
```javascript
// 1. == vs === (SELALU pakai ===!)
0 == ""       // true  (type coercion!)
0 === ""      // false (no coercion) ✅
null == undefined  // true
null === undefined // false

// 2. NaN !== NaN
NaN === NaN   // false!
Number.isNaN(NaN) // true ← pakai ini

// 3. Floating point precision
0.1 + 0.2 === 0.3              // false!
Math.abs(0.1 + 0.2 - 0.3) < 1e-10 // true ← pakai ini

// 4. sort() default lexicographic
[10, 2, 30].sort()              // [10, 2, 30] ← SALAH!
[10, 2, 30].sort((a,b) => a-b)  // [2, 10, 30] ✅

// 5. Array copy trap
let a = [1, 2, 3];
let b = a;           // REFERENCE! bukan copy
b.push(4);           // a juga berubah!
let c = [...a];      // COPY ✅

// 6. parseInt trap
parseInt("08")       // 8 (OK, tapi versi lama bisa jadi 0, octal)
parseInt("08", 10)   // 8 ← selalu sertakan radix!

// 7. String immutable
let s = "hello";
s[0] = "H";          // TIDAK BERUBAH! (silent fail)
s = "H" + s.slice(1); // "Hello" ← buat string baru
```

### Useful One-Liners
```javascript
// Remove duplicates
[...new Set(arr)]

// Sum array
arr.reduce((a, b) => a + b, 0)

// Max/Min array
Math.max(...arr)
Math.min(...arr)

// Flatten array
arr.flat(Infinity)

// Count occurrences
arr.reduce((acc, val) => (acc[val] = (acc[val] || 0) + 1, acc), {})

// Group by
arr.reduce((groups, item) => {
    let key = item.category;
    groups[key] = groups[key] || [];
    groups[key].push(item);
    return groups;
}, {})

// Generate range [0, 1, 2, ..., n-1]
[...Array(n).keys()]
Array.from({length: n}, (_, i) => i)

// Chunk array
function chunk(arr, size) {
    return Array.from({length: Math.ceil(arr.length/size)},
        (_, i) => arr.slice(i*size, i*size+size));
}

// Zip arrays
function zip(...arrays) {
    return arrays[0].map((_, i) => arrays.map(arr => arr[i]));
}
```

### Perbedaan Penting JS vs Python vs Go
| Aspek | JavaScript | Python | Go |
|-------|-----------|--------|-----|
| Typing | Dynamic | Dynamic | Static |
| Semicolon | Opsional | Tidak ada | Auto-insert |
| Array sort | Lexicographic ⚠️ | Numeric ✅ | `sort.Ints()` |
| Falsy | 0,"",null,undefined,NaN,false | 0,"",None,False,[],{} | Tidak ada |
| `[] == false` | ✅ true | ❌ ([] truthy) | N/A |
| Division | Float (5/2=2.5) | Float (5/2=2.5) | Int (5/2=2) |
| Floor div | `Math.floor(5/2)` | `5//2` | `5/2` |
| Power | `2**10` | `2**10` | `math.Pow(2,10)` |
| Null | null + undefined | None | nil |
| Class | `class` (ES6) | `class` | struct |
| Error handling | try-catch | try-except | error return |
| Lambda | `x => x*2` | `lambda x: x*2` | `func(x int) int {}` |
