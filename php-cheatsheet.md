# 🐘 PHP CHEATSHEET LENGKAP
## Untuk Tes Coding & Problem Solving

---

## Daftar Isi
1. [Dasar-Dasar PHP](#1-dasar-dasar-php)
2. [Tipe Data & Variabel](#2-tipe-data--variabel)
3. [String Operations](#3-string-operations)
4. [Array](#4-array)
5. [Associative Array (Map/Dictionary)](#5-associative-array)
6. [Control Flow](#6-control-flow)
7. [Functions](#7-functions)
8. [OOP (Class & Interface)](#8-oop-class--interface)
9. [Sorting & Searching](#9-sorting--searching)
10. [Math Operations](#10-math-operations)
11. [Input/Output](#11-inputoutput)
12. [Common Patterns untuk Problem Solving](#12-common-patterns-untuk-problem-solving)
13. [Tips & Tricks](#13-tips--tricks)

---

## 1. Dasar-Dasar PHP

### Struktur Program
```php
<?php
// Semua kode PHP ditulis di dalam tag <?php

echo "Hello, World!\n";

// Variabel dimulai dengan $
$name = "Alice";
echo "Hello, $name!\n";
?>
```

### Aturan Dasar
```php
<?php
// Variabel HARUS diawali $
// Case-sensitive untuk variabel, TIDAK untuk function/keyword
// Semicolon (;) WAJIB di akhir statement
// Comment: // atau # (single line), /* */ (multi line)

$x = 10;       // OK
$X = 20;       // berbeda dari $x (case-sensitive!)
echo $x;       // 10
ECHO $X;       // 20 (keyword echo case-insensitive)
?>
```

---

## 2. Tipe Data & Variabel

### Tipe Data
```php
<?php
// Integer
$n = 42;
$hex = 0xFF;         // 255
$bin = 0b1010;       // 10
$oct = 0777;         // 511

// Float
$f = 3.14;
$f2 = 1.2e3;        // 1200.0

// String
$s = "hello";
$s2 = 'hello';      // tanpa interpolasi

// Boolean
$b = true;
$b2 = false;

// Null
$n = null;

// Array
$arr = [1, 2, 3];
$assoc = ["name" => "Alice", "age" => 25];
?>
```

### Cek Tipe
```php
<?php
gettype($x);              // "integer", "string", "array", dll
var_dump($x);             // tipe + nilai (untuk debugging)

is_int($x);               // true jika integer
is_float($x);
is_string($x);
is_bool($x);
is_array($x);
is_null($x);
is_numeric($x);           // true untuk "42", 42, 3.14
isset($x);                // true jika ada dan BUKAN null
empty($x);                // true jika kosong/falsy
?>
```

### Konversi Tipe
```php
<?php
// Casting
$x = (int) "42";          // 42
$x = (float) "3.14";      // 3.14
$x = (string) 42;         // "42"
$x = (bool) 1;            // true
$x = (array) "hello";     // ["hello"]

// Functions
intval("42");              // 42
intval("42abc");           // 42 (baca sampai non-digit)
intval("abc");             // 0
intval("0b1010", 2);       // 10 (binary)
intval("ff", 16);          // 255 (hex)
floatval("3.14");          // 3.14
strval(42);                // "42"
settype($x, "integer");   // ubah tipe in-place

// Number formatting
number_format(1234567.89, 2, '.', ',');  // "1,234,567.89"
sprintf("%.2f", 3.14159);               // "3.14"

// Binary/Hex/Octal string
decbin(10);                // "1010"
decoct(8);                 // "10"
dechex(255);               // "ff"
bindec("1010");            // 10
octdec("10");              // 8
hexdec("ff");              // 255
?>
```

### Falsy Values
```php
<?php
// Nilai yang dianggap FALSE:
// false, 0, 0.0, "", "0", null, [] (array kosong)

// ⚠️ JEBAKAN: "0" adalah falsy di PHP! (berbeda dari JS/Python)
if ("0") { echo "truthy"; } else { echo "falsy"; }  // "falsy"!

// Truthy: semua selain di atas, termasuk "false", -1, [0]
?>
```

---

## 3. String Operations

### Operasi Dasar
```php
<?php
$s = "Hello World";

strlen($s);                     // 11
$s[0];                          // "H"
$s[strlen($s) - 1];            // "d"

// Concat
$s1 = "Hello" . " " . "World"; // concatenation pakai titik (.)
$s1 .= "!";                    // append

// Substring
substr($s, 0, 5);              // "Hello"
substr($s, 6);                 // "World"
substr($s, -5);                // "World" (dari belakang)

// Repeat
str_repeat("ab", 3);           // "ababab"
?>
```

### Transformasi
```php
<?php
strtoupper($s);                 // "HELLO WORLD"
strtolower($s);                 // "hello world"
ucfirst("hello");               // "Hello"
ucwords("hello world");         // "Hello World"
lcfirst("Hello");               // "hello"

trim("  hello  ");              // "hello"
ltrim("  hello  ");             // "hello  "
rtrim("  hello  ");             // "  hello"
trim("**hello**", "*");         // "hello"

str_replace("World", "PHP", $s);     // "Hello PHP"
str_ireplace("world", "PHP", $s);    // "Hello PHP" (case-insensitive)
substr_replace($s, "PHP", 6, 5);     // "Hello PHP"

str_pad("42", 5, "0", STR_PAD_LEFT);  // "00042"
str_pad("hi", 10, "*");               // "hi********"
str_pad("hi", 10, "*", STR_PAD_BOTH); // "****hi****"

strrev("hello");                // "olleh"
str_shuffle("hello");           // random order
?>
```

### Find & Check
```php
<?php
$s = "Hello World Hello";

strpos($s, "Hello");            // 0 (posisi pertama)
strrpos($s, "Hello");           // 12 (posisi terakhir)
strpos($s, "xyz");              // false (BUKAN -1!)

// ⚠️ JEBAKAN: strpos bisa return 0, yang falsy!
// SELALU pakai === untuk compare!
if (strpos($s, "Hello") !== false) {
    echo "Found!";
}

str_contains($s, "World");     // true (PHP 8.0+)
str_starts_with($s, "Hello");  // true (PHP 8.0+)
str_ends_with($s, "Hello");    // true (PHP 8.0+)

substr_count($s, "Hello");     // 2
?>
```

### Split & Join
```php
<?php
explode(",", "a,b,c");         // ["a", "b", "c"]
explode(" ", "hello world");   // ["hello", "world"]

implode("-", ["a", "b", "c"]); // "a-b-c"
implode("", ["a", "b", "c"]); // "abc"
join(",", [1, 2, 3]);          // "1,2,3" (alias implode)

str_split("hello", 2);         // ["he", "ll", "o"]
str_split("hello");            // ["h", "e", "l", "l", "o"]

// Split by regex
preg_split('/\s+/', "hello   world");  // ["hello", "world"]
?>
```

### Interpolasi & Formatting
```php
<?php
$name = "Alice";
$age = 25;

// Double quotes: interpolasi
"Name: $name, Age: $age";              // "Name: Alice, Age: 25"
"Name: {$name}, Age: {$age}";          // sama, lebih jelas

// Single quotes: tanpa interpolasi
'Name: $name';                          // "Name: $name" (literal)

// sprintf
sprintf("Name: %s, Age: %d", $name, $age);  // "Name: Alice, Age: 25"
sprintf("%05d", 42);                          // "00042"
sprintf("%.2f", 3.14159);                     // "3.14"
sprintf("%b", 10);                            // "1010" (binary)
sprintf("%x", 255);                           // "ff" (hex)

// printf (langsung print)
printf("Result: %d\n", 42);
?>
```

### Char Operations
```php
<?php
ord("A");                       // 65
chr(65);                        // "A"

ctype_alpha("Hello");           // true (semua huruf)
ctype_digit("123");             // true (semua digit)
ctype_alnum("abc123");          // true (huruf + digit)
ctype_upper("ABC");             // true
ctype_lower("abc");             // true
ctype_space(" \t\n");           // true
?>
```

---

## 4. Array

### Deklarasi & Akses
```php
<?php
$arr = [1, 2, 3, 4, 5];
$arr2 = array(1, 2, 3);        // cara lama, sama saja
$arr3 = array_fill(0, 5, 0);   // [0, 0, 0, 0, 0]
$arr4 = range(1, 5);           // [1, 2, 3, 4, 5]
$arr5 = range(0, 10, 2);       // [0, 2, 4, 6, 8, 10]
$arr6 = range('a', 'z');       // ['a', 'b', ..., 'z']

$arr[0];                        // 1
$arr[count($arr) - 1];         // 5 (elemen terakhir)
count($arr);                    // 5 (panjang)
sizeof($arr);                   // 5 (alias count)
?>
```

### Modifikasi
```php
<?php
// Tambah di akhir
$arr[] = 6;                     // push
array_push($arr, 7, 8);        // push multiple

// Tambah di awal
array_unshift($arr, 0);

// Hapus di akhir
array_pop($arr);                // return elemen terakhir

// Hapus di awal
array_shift($arr);              // return elemen pertama

// Hapus by index
unset($arr[2]);                 // hapus index 2 (⚠️ index TIDAK re-index!)
$arr = array_values($arr);     // re-index setelah unset

// Splice (insert/delete di posisi tertentu)
array_splice($arr, 2, 1);              // hapus 1 elemen di index 2
array_splice($arr, 2, 0, [99]);        // insert 99 di index 2
array_splice($arr, 1, 2, [10, 20]);    // ganti 2 elemen dari index 1

// Merge
$merged = array_merge($arr1, $arr2);
$merged = [...$arr1, ...$arr2];        // spread (PHP 7.4+)

// Slice (tidak mengubah asli)
array_slice($arr, 1, 3);       // 3 elemen dari index 1
array_slice($arr, -2);         // 2 elemen terakhir

// Reverse
$reversed = array_reverse($arr);

// Unique
$unique = array_unique([1, 2, 2, 3, 3]); // [1, 2, 3]

// Fill
$arr = array_fill(0, 5, "x"); // ["x", "x", "x", "x", "x"]

// Flatten (1 level)
$flat = array_merge(...[[1,2], [3,4], [5,6]]); // [1,2,3,4,5,6]
?>
```

### Iterasi & Transform
```php
<?php
$nums = [1, 2, 3, 4, 5];

// foreach
foreach ($nums as $val) {
    echo $val . " ";
}

// foreach with index
foreach ($nums as $idx => $val) {
    echo "$idx: $val\n";
}

// array_map (seperti map di JS)
$doubled = array_map(fn($x) => $x * 2, $nums);    // [2, 4, 6, 8, 10]

// array_filter (seperti filter di JS)
$evens = array_filter($nums, fn($x) => $x % 2 === 0);  // [2, 4]
$evens = array_values($evens);  // re-index! [2, 4]

// array_reduce (seperti reduce di JS)
$sum = array_reduce($nums, fn($carry, $x) => $carry + $x, 0);  // 15

// array_walk (modify in-place by reference)
array_walk($nums, function(&$val) { $val *= 2; });
// $nums sekarang [2, 4, 6, 8, 10]

// Check
in_array(3, $nums);            // true (ada di array?)
array_search(3, $nums);        // 2 (index, false jika tidak ada)

// Sum, Product, Min, Max
array_sum($nums);              // 15
array_product($nums);          // 120
min($nums);                    // 1
max($nums);                    // 5

// Count occurrences
array_count_values(["a","b","a","c","a"]); // ["a"=>3, "b"=>1, "c"=>1]
?>
```

### 2D Array (Matrix)
```php
<?php
// Buat matrix n x m
$n = 3; $m = 4;
$matrix = array_fill(0, $n, array_fill(0, $m, 0));

// Akses
$matrix[0][0] = 1;

// Iterate
for ($i = 0; $i < $n; $i++) {
    for ($j = 0; $j < $m; $j++) {
        echo $matrix[$i][$j] . " ";
    }
    echo "\n";
}
?>
```

---

## 5. Associative Array (Map/Dictionary)

```php
<?php
// PHP array = ordered map (gabungan array + dictionary!)
$person = [
    "name" => "Alice",
    "age" => 25,
    "city" => "Jakarta"
];

// Akses
$person["name"];                // "Alice"

// CRUD
$person["email"] = "a@b.com";  // add
$person["age"] = 26;           // update
unset($person["city"]);        // delete

// Check key exist
array_key_exists("name", $person);  // true
isset($person["name"]);             // true (tapi false jika value null!)

// Keys & Values
array_keys($person);           // ["name", "age", "email"]
array_values($person);         // ["Alice", 26, "a@b.com"]

// Iterate
foreach ($person as $key => $val) {
    echo "$key: $val\n";
}

// Merge (key bertabrakan → yang terakhir menang)
$merged = array_merge($arr1, $arr2);
$merged = $arr1 + $arr2;       // key bertabrakan → yang pertama menang

// Flip keys ↔ values
$flipped = array_flip(["a" => 1, "b" => 2]);  // [1 => "a", 2 => "b"]

// Frequency counter
$freq = array_count_values(["go", "go", "php", "go"]);
// ["go" => 3, "php" => 1]

// Sort by key
ksort($person);                // sort by key ascending
krsort($person);               // sort by key descending

// Sort by value
asort($person);                // sort by value ascending, preserve keys
arsort($person);               // sort by value descending, preserve keys
?>
```

---

## 6. Control Flow

### If-Else
```php
<?php
$x = 10;

if ($x > 0) {
    echo "Positive";
} elseif ($x < 0) {
    echo "Negative";
} else {
    echo "Zero";
}

// Ternary
$result = $x > 0 ? "Positive" : "Negative";

// Null coalescing (??)
$name = $_GET["name"] ?? "Guest";  // "Guest" jika null/undefined
$name ??= "Default";               // assign jika null (PHP 7.4+)

// Null coalescing vs ternary
$x = 0;
$a = $x ?: "default";    // "default" (0 falsy → ambil kanan)
$b = $x ?? "default";    // 0 (hanya null/undefined trigger ??)

// Spaceship operator
1 <=> 2;   // -1 (kurang dari)
2 <=> 2;   // 0  (sama)
3 <=> 2;   // 1  (lebih dari)
// Berguna untuk custom sort!

// Match expression (PHP 8.0+ — pengganti switch yang lebih baik)
$result = match($x) {
    1, 2 => "satu atau dua",
    3    => "tiga",
    default => "lainnya"
};
?>
```

### Switch
```php
<?php
switch ($day) {
    case "Monday":
        echo "Senin";
        break;                  // WAJIB break!
    case "Tuesday":
    case "Wednesday":           // fall-through
        echo "Selasa/Rabu";
        break;
    default:
        echo "Lainnya";
}
?>
```

### For Loop
```php
<?php
// Classic for
for ($i = 0; $i < 10; $i++) {
    echo $i;
}

// foreach (array)
foreach ($arr as $val) {
    echo $val;
}

// foreach with key
foreach ($arr as $key => $val) {
    echo "$key: $val";
}

// while
$i = 0;
while ($i < 10) {
    echo $i;
    $i++;
}

// do-while
do {
    echo $i;
    $i++;
} while ($i < 10);

// Break & Continue
for ($i = 0; $i < 10; $i++) {
    if ($i === 5) break;
    if ($i % 2 === 0) continue;
    echo $i;  // 1, 3
}

// Break nested loops (break N)
for ($i = 0; $i < 5; $i++) {
    for ($j = 0; $j < 5; $j++) {
        if ($j === 3) break 2;  // keluar dari KEDUA loop!
    }
}
?>
```

---

## 7. Functions

### Deklarasi
```php
<?php
function add(int $a, int $b): int {
    return $a + $b;
}

// Type hints (PHP 7+)
function greet(string $name, string $greeting = "Hello"): string {
    return "$greeting, $name!";
}

// Nullable type
function find(?string $name): ?int {
    if ($name === null) return null;
    return strlen($name);
}

// Union types (PHP 8.0+)
function format(int|string $input): string {
    return (string) $input;
}

// Return void
function log(string $msg): void {
    echo $msg;
}

// Variadic (rest parameter)
function sum(int ...$nums): int {
    return array_sum($nums);
}
sum(1, 2, 3, 4);  // 10

// Spread
$arr = [1, 2, 3];
sum(...$arr);       // 6
?>
```

### Arrow Functions (PHP 7.4+)
```php
<?php
// Short closure (auto capture variabel luar)
$double = fn($x) => $x * 2;
$double(5);  // 10

// Pakai di array_map, array_filter
$nums = [1, 2, 3, 4, 5];
$evens = array_filter($nums, fn($x) => $x % 2 === 0);
$doubled = array_map(fn($x) => $x * 2, $nums);

// Multi-line closure (pakai function + use)
$factor = 3;
$multiply = function($x) use ($factor) {
    return $x * $factor;
};
$multiply(5);  // 15

// By reference (modify outer variable)
$count = 0;
$increment = function() use (&$count) {
    $count++;
};
$increment();
echo $count;  // 1
?>
```

### Pass by Reference
```php
<?php
function swap(&$a, &$b): void {
    [$a, $b] = [$b, $a];
}

$x = 1; $y = 2;
swap($x, $y);
echo "$x $y";  // 2 1
?>
```

---

## 8. OOP (Class & Interface)

### Class
```php
<?php
class Student {
    // Properties dengan visibility
    public string $name;
    private int $age;
    protected float $grade;
    public readonly string $id;  // readonly (PHP 8.1+)

    // Constructor
    public function __construct(string $name, int $age, float $grade, string $id) {
        $this->name = $name;
        $this->age = $age;
        $this->grade = $grade;
        $this->id = $id;
    }

    // Constructor promotion shorthand (PHP 8.0+)
    // public function __construct(
    //     public string $name,
    //     private int $age,
    //     protected float $grade = 0.0
    // ) {}

    // Method
    public function info(): string {
        return "{$this->name}, age {$this->age}";
    }

    // Getter
    public function getAge(): int {
        return $this->age;
    }

    // Setter
    public function setAge(int $age): void {
        if ($age >= 0) $this->age = $age;
    }

    // Static method
    public static function create(string $name): self {
        return new self($name, 0, 0.0, uniqid());
    }

    // Magic method: toString
    public function __toString(): string {
        return $this->info();
    }
}

$s = new Student("Alice", 20, 3.5, "001");
$s->name;           // "Alice"
$s->info();          // "Alice, age 20"
$s2 = Student::create("Bob");  // static call
?>
```

### Inheritance & Interface
```php
<?php
// Interface
interface Shape {
    public function area(): float;
    public function perimeter(): float;
}

// Abstract class
abstract class BaseShape implements Shape {
    abstract public function area(): float;

    public function describe(): string {
        return "Area: " . $this->area();
    }
}

// Concrete class
class Circle extends BaseShape {
    public function __construct(private float $radius) {}

    public function area(): float {
        return M_PI * $this->radius ** 2;
    }

    public function perimeter(): float {
        return 2 * M_PI * $this->radius;
    }
}

class Rectangle extends BaseShape {
    public function __construct(
        private float $width,
        private float $height
    ) {}

    public function area(): float {
        return $this->width * $this->height;
    }

    public function perimeter(): float {
        return 2 * ($this->width + $this->height);
    }
}

// Polymorphism
function printArea(Shape $shape): void {
    echo "Area: " . $shape->area() . "\n";
}

printArea(new Circle(5));
printArea(new Rectangle(3, 4));
?>
```

### Enum (PHP 8.1+)
```php
<?php
enum Color: string {
    case Red = "red";
    case Green = "green";
    case Blue = "blue";
}

$c = Color::Red;
echo $c->value;     // "red"
echo $c->name;      // "Red"

// From value
$c = Color::from("red");        // Color::Red
$c = Color::tryFrom("xyz");     // null (tidak error)

// Pure enum (tanpa value)
enum Direction {
    case Up;
    case Down;
    case Left;
    case Right;
}
?>
```

---

## 9. Sorting & Searching

### Sort Functions
```php
<?php
$arr = [3, 1, 4, 1, 5, 9, 2, 6];

// Sort ascending (in-place, re-index)
sort($arr);                     // [1, 1, 2, 3, 4, 5, 6, 9]

// Sort descending
rsort($arr);                    // [9, 6, 5, 4, 3, 2, 1, 1]

// Sort ascending, preserve keys
asort($arr);

// Sort descending, preserve keys
arsort($arr);

// Sort by key
ksort($assoc);                  // key ascending
krsort($assoc);                 // key descending
?>
```

### Custom Sort (usort)
```php
<?php
$nums = [3, 1, 4, 1, 5];

// Custom comparator (return negatif/0/positif)
usort($nums, function($a, $b) {
    return $a - $b;             // ascending
});

usort($nums, fn($a, $b) => $b - $a);  // descending

// Spaceship operator (sangat berguna!)
usort($nums, fn($a, $b) => $a <=> $b);  // ascending
usort($nums, fn($a, $b) => $b <=> $a);  // descending

// Sort by multiple keys
$people = [
    ["name" => "Alice", "age" => 25],
    ["name" => "Bob", "age" => 20],
    ["name" => "Charlie", "age" => 25],
];

usort($people, function($a, $b) {
    // Age ascending, lalu name ascending
    if ($a["age"] !== $b["age"]) {
        return $a["age"] <=> $b["age"];
    }
    return $a["name"] <=> $b["name"];
});

// Sort tuple-like array
$data = [[6, 55], [4, 40], [6, 45], [10, 50]];

// Elemen pertama descending, kedua ascending
usort($data, function($a, $b) {
    if ($a[0] !== $b[0]) return $b[0] <=> $a[0];  // desc
    return $a[1] <=> $b[1];                         // asc
});
// [[10,50], [6,45], [6,55], [4,40]]

// Stable sort (preserve relative order)
// usort() TIDAK stable di PHP < 8.0
// PHP 8.0+: usort() sudah stable
?>
```

### Searching
```php
<?php
// Linear search
in_array(3, $arr);              // true/false
array_search(3, $arr);          // index atau false

// Binary search (manual)
function binarySearch(array $arr, int $target): int {
    $lo = 0;
    $hi = count($arr) - 1;
    while ($lo <= $hi) {
        $mid = intdiv($lo + $hi, 2);
        if ($arr[$mid] === $target) return $mid;
        elseif ($arr[$mid] < $target) $lo = $mid + 1;
        else $hi = $mid - 1;
    }
    return -1;
}
?>
```

---

## 10. Math Operations

### Built-in Math
```php
<?php
abs(-5);                // 5
sqrt(16);               // 4.0
pow(2, 10);             // 1024
2 ** 10;                // 1024

floor(3.7);             // 3
ceil(3.2);              // 4
round(3.5);             // 4
round(3.14159, 2);      // 3.14
intdiv(7, 2);           // 3 (integer division)

max(1, 2, 3);           // 3
min(1, 2, 3);           // 1
max([1, 2, 3]);         // 3 (juga bisa array!)
min([1, 2, 3]);         // 1

log(M_E);               // 1.0 (ln)
log2(8);                // 3.0
log10(100);             // 2.0

M_PI;                   // 3.141592653589793
M_E;                    // 2.718281828459045
PHP_INT_MAX;            // 9223372036854775807
PHP_INT_MIN;
PHP_FLOAT_MAX;
INF;                    // Infinity
NAN;                    // Not a Number

// Random
rand(1, 100);           // random 1-100
mt_rand(1, 100);        // better random
random_int(1, 100);     // cryptographically secure

// Modulo
10 % 3;                 // 1
fmod(10.5, 3.2);        // 0.9 (float modulo)
-7 % 3;                 // -1 (⚠️ bisa negatif!)
((-7 % 3) + 3) % 3;    // 2 (true modulo)
?>
```

### Common Math Functions
```php
<?php
// GCD
function gcd(int $a, int $b): int {
    while ($b !== 0) [$a, $b] = [$b, $a % $b];
    return $a;
}
// Atau built-in: gmp_gcd(12, 8)

// LCM
function lcm(int $a, int $b): int {
    return intdiv($a, gcd($a, $b)) * $b;
}

// Check Prime
function isPrime(int $n): bool {
    if ($n < 2) return false;
    for ($i = 2; $i * $i <= $n; $i++) {
        if ($n % $i === 0) return false;
    }
    return true;
}

// Sum of digits
function digitSum(int $n): int {
    return array_sum(str_split((string)abs($n)));
}

// Factorial
function factorial(int $n): int {
    $result = 1;
    for ($i = 2; $i <= $n; $i++) $result *= $i;
    return $result;
}
?>
```

---

## 11. Input/Output

### Standard I/O (HackerEarth / CLI)
```php
<?php
// === Baca input ===

// Baca 1 baris
$line = trim(fgets(STDIN));

// Baca 1 integer
$n = (int)trim(fgets(STDIN));

// Baca array of integers (space-separated)
$arr = array_map('intval', explode(' ', trim(fgets(STDIN))));

// Baca multiple integers
[$a, $b] = array_map('intval', explode(' ', trim(fgets(STDIN))));

// fscanf (formatted input)
fscanf(STDIN, "%d %d", $a, $b);

// === Output ===
echo "Hello\n";
echo $answer . "\n";
echo implode(" ", $arr) . "\n";  // array jadi "1 2 3"
print_r($arr);                    // debug output
var_dump($arr);                   // debug dengan tipe
?>
```

### Template Competitive Programming
```php
<?php
// Template 1: Simple
$t = (int)trim(fgets(STDIN));
while ($t--) {
    $n = (int)trim(fgets(STDIN));
    $arr = array_map('intval', explode(' ', trim(fgets(STDIN))));

    // ... solve ...

    echo $answer . "\n";
}
?>
```

```php
<?php
// Template 2: Fast I/O (baca semua sekaligus)
$input = explode("\n", trim(file_get_contents("php://stdin")));
$idx = 0;

function nextLine(): string {
    global $input, $idx;
    return trim($input[$idx++]);
}

function nextInt(): int {
    return (int)nextLine();
}

function nextInts(): array {
    return array_map('intval', explode(' ', nextLine()));
}

// Usage
$t = nextInt();
while ($t--) {
    $n = nextInt();
    $arr = nextInts();
    echo solve($n, $arr) . "\n";
}

function solve(int $n, array $arr): int {
    // ...
    return 0;
}
?>
```

---

## 12. Common Patterns untuk Problem Solving

### Two Pointers
```php
<?php
function isPalindrome(string $s): bool {
    $i = 0;
    $j = strlen($s) - 1;
    while ($i < $j) {
        if ($s[$i] !== $s[$j]) return false;
        $i++;
        $j--;
    }
    return true;
}

function twoSum(array $nums, int $target): array {
    $lo = 0;
    $hi = count($nums) - 1;
    while ($lo < $hi) {
        $sum = $nums[$lo] + $nums[$hi];
        if ($sum === $target) return [$lo, $hi];
        elseif ($sum < $target) $lo++;
        else $hi--;
    }
    return [-1, -1];
}
?>
```

### Stack
```php
<?php
// PHP array bisa langsung jadi stack
$stack = [];

array_push($stack, 10);         // push
$stack[] = 20;                   // push (lebih cepat)
$top = array_pop($stack);       // pop → 20
$top = end($stack);             // peek (tanpa hapus)
empty($stack);                   // isEmpty

// Valid Parentheses
function isValid(string $s): bool {
    $stack = [];
    $pairs = [')' => '(', ']' => '[', '}' => '{'];

    for ($i = 0; $i < strlen($s); $i++) {
        $ch = $s[$i];
        if (in_array($ch, ['(', '[', '{'])) {
            $stack[] = $ch;
        } else {
            if (empty($stack) || end($stack) !== $pairs[$ch]) return false;
            array_pop($stack);
        }
    }
    return empty($stack);
}
?>
```

### Queue
```php
<?php
// SplQueue (proper queue, O(1) operations)
$queue = new SplQueue();
$queue->enqueue(10);            // enqueue
$queue->dequeue();              // dequeue → 10
$queue->bottom();               // peek front
$queue->isEmpty();
$queue->count();

// Atau array (shift = O(n) tapi simpel)
$queue = [];
$queue[] = 10;                  // enqueue
$front = array_shift($queue);  // dequeue → O(n)
?>
```

### BFS
```php
<?php
function bfs(array $graph, int $start): array {
    $visited = [$start => true];
    $queue = new SplQueue();
    $queue->enqueue($start);
    $result = [];

    while (!$queue->isEmpty()) {
        $node = $queue->dequeue();
        $result[] = $node;

        foreach ($graph[$node] ?? [] as $neighbor) {
            if (!isset($visited[$neighbor])) {
                $visited[$neighbor] = true;
                $queue->enqueue($neighbor);
            }
        }
    }
    return $result;
}
?>
```

### DFS
```php
<?php
function dfs(array $graph, int $node, array &$visited = []): array {
    $visited[$node] = true;
    $result = [$node];

    foreach ($graph[$node] ?? [] as $neighbor) {
        if (!isset($visited[$neighbor])) {
            $result = array_merge($result, dfs($graph, $neighbor, $visited));
        }
    }
    return $result;
}
?>
```

### Dynamic Programming
```php
<?php
// Fibonacci (memoization)
function fib(int $n, array &$memo = []): int {
    if ($n <= 1) return $n;
    if (isset($memo[$n])) return $memo[$n];
    $memo[$n] = fib($n - 1, $memo) + fib($n - 2, $memo);
    return $memo[$n];
}

// Fibonacci (bottom-up)
function fibTab(int $n): int {
    if ($n <= 1) return $n;
    $dp = [0, 1];
    for ($i = 2; $i <= $n; $i++) {
        $dp[$i] = $dp[$i-1] + $dp[$i-2];
    }
    return $dp[$n];
}

// Space optimized
function fibOpt(int $n): int {
    if ($n <= 1) return $n;
    [$a, $b] = [0, 1];
    for ($i = 2; $i <= $n; $i++) {
        [$a, $b] = [$b, $a + $b];
    }
    return $b;
}
?>
```

### Heap / Priority Queue
```php
<?php
// SplPriorityQueue (built-in! Max heap by default)
$heap = new SplPriorityQueue();
$heap->insert("task1", 3);     // (value, priority)
$heap->insert("task2", 1);
$heap->insert("task3", 5);

$heap->extract();               // "task3" (priority 5, tertinggi)
$heap->extract();               // "task1" (priority 3)

// Min heap: negate priority
$minHeap = new SplPriorityQueue();
$minHeap->insert("a", -3);     // priority -3
$minHeap->insert("b", -1);     // priority -1
$minHeap->insert("c", -5);     // priority -5
$minHeap->extract();            // "b" (priority -1, terbesar = yang aslinya terkecil)

// SplMinHeap / SplMaxHeap (hanya value, tanpa separate priority)
$minHeap = new SplMinHeap();
$minHeap->insert(5);
$minHeap->insert(2);
$minHeap->insert(8);
$minHeap->extract();            // 2 (terkecil)
?>
```

---

## 13. Tips & Tricks

### PHP-Specific Tips
```php
<?php
// 1. Swap (PHP 7.1+ list destructuring)
[$a, $b] = [$b, $a];

// 2. List/destructuring
[$first, $second, $third] = [1, 2, 3];
[, , $third] = [1, 2, 3];     // skip elemen
[$a, ...$rest] = [1, 2, 3, 4]; // $a=1, $rest=[2,3,4] (PHP 7.4+... hanya di function args)

// 3. array_combine (buat assoc dari 2 array)
$keys = ["a", "b", "c"];
$vals = [1, 2, 3];
$result = array_combine($keys, $vals); // ["a"=>1, "b"=>2, "c"=>3]

// 4. array_column (ambil kolom dari array of arrays)
$users = [
    ["name" => "Alice", "age" => 25],
    ["name" => "Bob", "age" => 30],
];
array_column($users, "name");  // ["Alice", "Bob"]

// 5. Compact & Extract
$name = "Alice"; $age = 25;
$data = compact("name", "age");  // ["name" => "Alice", "age" => 25]
extract($data);                   // $name = "Alice", $age = 25

// 6. Null safe operator (PHP 8.0+)
$name = $user?->getProfile()?->getName();

// 7. Named arguments (PHP 8.0+)
array_slice($arr, offset: 2, length: 3);
str_pad("42", length: 5, pad_string: "0", pad_type: STR_PAD_LEFT);

// 8. Spread operator
$arr1 = [1, 2, 3];
$arr2 = [0, ...$arr1, 4];     // [0, 1, 2, 3, 4]
function sum(int ...$n): int { return array_sum($n); }

// 9. Check prime cepat
function isPrime(int $n): bool {
    if ($n < 2) return false;
    for ($i = 2; $i * $i <= $n; $i++) {
        if ($n % $i === 0) return false;
    }
    return true;
}

// 10. Quick frequency counter
$freq = array_count_values($arr);  // langsung jadi!
?>
```

### Perbedaan PHP vs Bahasa Lain
| Aspek | PHP | Python | JavaScript | Go |
|-------|-----|--------|------------|-----|
| Variabel | `$var` | `var` | `let/const` | `var/short` |
| Concat | `.` | `+` | `+` | `+` |
| Array | Ordered map | list + dict | Array + Object | Slice + Map |
| Falsy `"0"` | ✅ falsy! | ❌ truthy | ❌ truthy | N/A |
| Sort default | Ascending ✅ | Ascending ✅ | Lexicographic ⚠️ | `sort.Ints()` ✅ |
| `==` vs `===` | Ada keduanya | Hanya `==` | Ada keduanya | Hanya `==` |
| Spaceship `<=>` | ✅ Ada | ❌ | ❌ | ❌ |
| Null coalescing | `??` | `or`/`:=` | `??` | N/A |
| strpos return | `false` ⚠️ | `-1` | `-1` | `-1` |
| Semicolon | Wajib | Tidak ada | Opsional | Auto-insert |
| Type system | Dynamic (typed hints) | Dynamic | Dynamic | Static |
| Built-in heap | ✅ SplPriorityQueue | ✅ heapq | ❌ Manual | ❌ Manual |
