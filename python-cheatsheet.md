# 🐍 PYTHON CHEATSHEET LENGKAP
## Untuk Tes Coding & Problem Solving

---

## Daftar Isi
1. [Tipe Data Dasar](#1-tipe-data-dasar)
2. [String Operations](#2-string-operations)
3. [List, Tuple, Set, Dictionary](#3-list-tuple-set-dictionary)
4. [Control Flow](#4-control-flow)
5. [Functions](#5-functions)
6. [List Comprehension](#6-list-comprehension)
7. [Sorting & Searching](#7-sorting--searching)
8. [Math Operations](#8-math-operations)
9. [Built-in Functions](#9-built-in-functions)
10. [Itertools & Collections](#10-itertools--collections)
11. [Pattern Problem Solving](#11-pattern-problem-solving)
12. [Common Algorithms](#12-common-algorithms)
13. [Tips & Tricks](#13-tips--tricks)

---

## 1. Tipe Data Dasar

### Tipe Data & Konversi
```python
# Integer
x = 10
x = int("10")           # String ke Integer
x = int(10.9)           # Float ke Integer (truncate) -> 10

# Float
y = 3.14
y = float("3.14")       # String ke Float
y = float(10)           # Integer ke Float -> 10.0

# String
s = "Hello"
s = str(123)            # Integer ke String
s = str(3.14)           # Float ke String

# Boolean
b = True
b = bool(1)             # Non-zero -> True
b = bool(0)             # Zero -> False
b = bool("")            # Empty string -> False
b = bool("abc")         # Non-empty -> True
b = bool([])            # Empty list -> False
b = bool([1, 2])        # Non-empty list -> True

# None
n = None
```

### Cek Tipe Data
```python
type(x)                 # <class 'int'>
isinstance(x, int)      # True
isinstance(x, (int, float))  # Multiple types check
```

---

## 2. String Operations

### Operasi Dasar
```python
s = "Hello World"

# Panjang
len(s)                  # 11

# Akses karakter
s[0]                    # 'H' (index pertama)
s[-1]                   # 'd' (index terakhir)
s[0:5]                  # 'Hello' (slice)
s[6:]                   # 'World'
s[:5]                   # 'Hello'
s[::2]                  # 'HloWrd' (setiap 2 karakter)
s[::-1]                 # 'dlroW olleH' (reverse)

# Check
s.startswith("Hello")   # True
s.endswith("World")     # True
"lo" in s               # True (substring check)
s.isalpha()             # False (ada spasi)
s.isdigit()             # False
s.isalnum()             # False
s.isupper()             # False
s.islower()             # False
```

### Transformasi String
```python
s = "  hello world  "

s.upper()               # "  HELLO WORLD  "
s.lower()               # "  hello world  "
s.capitalize()          # "  hello world  "
s.title()               # "  Hello World  "
s.strip()               # "hello world"
s.lstrip()              # "hello world  "
s.rstrip()              # "  hello world"
s.replace("world", "python")  # "  hello python  "

# Split & Join
s = "a,b,c,d"
s.split(",")            # ['a', 'b', 'c', 'd']
"-".join(['a', 'b', 'c'])  # "a-b-c"

# Format String
name = "John"
age = 25
f"Name: {name}, Age: {age}"           # f-string (recommended)
"Name: {}, Age: {}".format(name, age) # .format()
"Name: %s, Age: %d" % (name, age)     # %-formatting

# Padding
"42".zfill(5)           # "00042"
"hello".ljust(10)       # "hello     "
"hello".rjust(10)       # "     hello"
"hello".center(11)      # "   hello   "
```

### Find & Count
```python
s = "hello hello world"

s.find("hello")         # 0 (first occurrence)
s.find("xyz")           # -1 (not found)
s.rfind("hello")        # 6 (last occurrence)
s.index("hello")        # 0 (raises error if not found)
s.count("hello")        # 2
```

---

## 3. List, Tuple, Set, Dictionary

### List (Mutable, Ordered)
```python
lst = [1, 2, 3, 4, 5]

# Akses
lst[0]                  # 1
lst[-1]                 # 5
lst[1:3]                # [2, 3]

# Modifikasi
lst.append(6)           # [1, 2, 3, 4, 5, 6]
lst.insert(0, 0)        # [0, 1, 2, 3, 4, 5, 6]
lst.extend([7, 8])      # [0, 1, 2, 3, 4, 5, 6, 7, 8]
lst.pop()               # Hapus & return elemen terakhir
lst.pop(0)              # Hapus & return elemen index 0
lst.remove(3)           # Hapus elemen bernilai 3
lst.clear()             # Kosongkan list

# Copy
lst2 = lst.copy()       # Shallow copy
lst2 = lst[:]           # Shallow copy
import copy
lst2 = copy.deepcopy(lst)  # Deep copy

# Sorting
lst.sort()              # In-place sort ascending
lst.sort(reverse=True)  # In-place sort descending
lst.reverse()           # In-place reverse
sorted_lst = sorted(lst)  # Return new sorted list

# Searching
3 in lst                # True (check existence)
lst.index(3)            # Index of first 3
lst.count(3)            # Count occurrences

# Min, Max, Sum
min(lst)
max(lst)
sum(lst)
```

### Tuple (Immutable, Ordered)
```python
tup = (1, 2, 3, 4)
tup = 1, 2, 3, 4        # Parentheses optional

# Akses
tup[0]                  # 1
tup[-1]                 # 4

# Unpack
a, b, c, d = tup
a, *rest = tup          # a=1, rest=[2, 3, 4]
a, *mid, d = (1,2,3,4)  # a=1, mid=[2,3], d=4

# Methods
tup.count(2)            # 1
tup.index(3)            # 2
```

### Set (Mutable, Unordered, Unique)
```python
s = {1, 2, 3, 4, 5}
s = set([1, 2, 2, 3])   # {1, 2, 3} - removes duplicates

# Modifikasi
s.add(6)                # {1, 2, 3, 4, 5, 6}
s.remove(3)             # Error jika tidak ada
s.discard(3)            # Tidak error jika tidak ada
s.pop()                 # Hapus random element

# Set Operations
a = {1, 2, 3}
b = {2, 3, 4}

a | b                   # Union: {1, 2, 3, 4}
a & b                   # Intersection: {2, 3}
a - b                   # Difference: {1}
a ^ b                   # Symmetric difference: {1, 4}

a.union(b)
a.intersection(b)
a.difference(b)
a.symmetric_difference(b)

# Check
a.issubset(b)           # False
a.issuperset(b)         # False
a.isdisjoint(b)         # False (ada irisan)
```

### Dictionary (Mutable, Key-Value)
```python
d = {"name": "John", "age": 25}
d = dict(name="John", age=25)

# Akses
d["name"]               # "John"
d.get("name")           # "John"
d.get("city", "Unknown")  # "Unknown" (default value)

# Modifikasi
d["city"] = "Jakarta"   # Add/update
d.update({"country": "Indonesia"})
del d["age"]            # Delete key
d.pop("city")           # Delete & return value
d.pop("xyz", None)      # No error with default

# Iterasi
d.keys()                # dict_keys(['name', 'city', ...])
d.values()              # dict_values(['John', 'Jakarta', ...])
d.items()               # dict_items([('name', 'John'), ...])

for key in d:
    print(key, d[key])

for key, value in d.items():
    print(key, value)

# Check
"name" in d             # True

# Default Dict (from collections)
from collections import defaultdict
dd = defaultdict(int)   # Default value 0
dd = defaultdict(list)  # Default value []
dd["count"] += 1        # Works without initialization
```

---

## 4. Control Flow

### If-Elif-Else
```python
x = 10

if x > 0:
    print("Positive")
elif x < 0:
    print("Negative")
else:
    print("Zero")

# Ternary operator
result = "Even" if x % 2 == 0 else "Odd"

# Multiple conditions
if 0 < x < 100:         # Chained comparison
    print("Valid")

if x > 0 and x < 100:   # Explicit
    print("Valid")
```

### Pengkondisian Tanpa `if` (Trick untuk Soal yang Larang `if`)
```python
# ========================
# 1. TERNARY EXPRESSION
# ========================
x = 10
result = "Even" if x % 2 == 0 else "Odd"  # "Even"

# Nested ternary
grade = "A" if x >= 90 else "B" if x >= 80 else "C" if x >= 70 else "D"

# ========================
# 2. AND / OR SHORT-CIRCUIT
# ========================
# `and` → return value pertama yang falsy, atau value terakhir jika semua truthy
# `or`  → return value pertama yang truthy, atau value terakhir jika semua falsy

x = 5
result = x > 0 and "Positive" or "Non-positive"  # "Positive"
# Hati-hati: gagal jika value truthy-nya adalah falsy value (0, "", None, dll)

# Contoh: default value
name = "" or "Anonymous"           # "Anonymous" (karena "" falsy)
val = None or 0 or [] or "Found"   # "Found"
val = 10 and 20                    # 20 (keduanya truthy, return terakhir)

# Print kondisional tanpa if
x = 3
x > 0 and print("Positive")       # Print "Positive"
x < 0 and print("Negative")       # Tidak print (short-circuit)
x < 0 or print("Not negative")    # Print "Not negative"

# ========================
# 3. DICTIONARY MAPPING (pengganti if-elif-else)
# ========================
def get_day(n):
    return {
        1: "Senin", 2: "Selasa", 3: "Rabu",
        4: "Kamis", 5: "Jumat", 6: "Sabtu", 7: "Minggu"
    }.get(n, "Invalid")

get_day(3)  # "Rabu"
get_day(9)  # "Invalid"

# Operasi berdasarkan kondisi
operation = {'+': lambda a,b: a+b, '-': lambda a,b: a-b,
             '*': lambda a,b: a*b, '/': lambda a,b: a/b}
operation['+'](10, 5)  # 15

# ========================
# 4. LIST/TUPLE INDEXING (pengganti if-else sederhana)
# ========================
x = 10
result = ("Odd", "Even")[x % 2 == 0]           # "Even"
# x % 2 == 0 → True (1) → index 1 → "Even"

result = ("Negative", "Non-negative")[x >= 0]   # "Non-negative"

# Boolean sebagai index (True=1, False=0)
labels = ["Fail", "Pass"]
score = 75
labels[score >= 60]  # "Pass"

# ========================
# 5. MATH TRICKS (pengganti if untuk angka)
# ========================
# Absolute value tanpa if
x = -5
abs_x = (x ** 2) ** 0.5           # 5.0  (selalu positif)
abs_x = x * (-1) ** (x < 0)       # 5

# Max tanpa if
a, b = 3, 7
max_val = (a + b + abs(a - b)) // 2  # 7

# Min tanpa if
min_val = (a + b - abs(a - b)) // 2  # 3

# Clamp value (batasi antara lo dan hi) tanpa if
val, lo, hi = 15, 0, 10
clamped = max(lo, min(val, hi))    # 10
```

### For Loop
```python
# Range
for i in range(5):      # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 6):   # 1, 2, 3, 4, 5
    print(i)

for i in range(0, 10, 2):  # 0, 2, 4, 6, 8
    print(i)

for i in range(5, 0, -1):  # 5, 4, 3, 2, 1
    print(i)

# Iterate list
lst = ['a', 'b', 'c']
for item in lst:
    print(item)

# With index
for i, item in enumerate(lst):
    print(i, item)

for i, item in enumerate(lst, start=1):  # Start from 1
    print(i, item)

# Iterate dictionary
for key, value in dict.items():
    print(key, value)

# Iterate multiple lists
list1 = [1, 2, 3]
list2 = ['a', 'b', 'c']
for x, y in zip(list1, list2):
    print(x, y)  # 1 a, 2 b, 3 c
```

### While Loop
```python
i = 0
while i < 5:
    print(i)
    i += 1

# With break & continue
while True:
    x = int(input())
    if x == 0:
        break           # Exit loop
    if x < 0:
        continue        # Skip iteration
    print(x)

# While-else (executed if loop completes normally)
i = 0
while i < 5:
    i += 1
else:
    print("Loop completed")
```

---

## 5. Functions

### Function Definition
```python
def greet(name):
    return f"Hello, {name}!"

# Default parameters
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

# *args (variable positional arguments)
def sum_all(*args):
    return sum(args)

sum_all(1, 2, 3, 4)     # 10

# **kwargs (variable keyword arguments)
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="John", age=25)

# Combined
def func(a, b, *args, **kwargs):
    pass
```

### Lambda Functions
```python
# Anonymous function
square = lambda x: x ** 2
add = lambda x, y: x + y

# Usage with map, filter, sorted
lst = [1, 2, 3, 4, 5]

# Map
list(map(lambda x: x * 2, lst))  # [2, 4, 6, 8, 10]

# Filter
list(filter(lambda x: x % 2 == 0, lst))  # [2, 4]

# Sorted with key
students = [("Alice", 85), ("Bob", 92), ("Charlie", 78)]
sorted(students, key=lambda x: x[1])  # Sort by score
sorted(students, key=lambda x: x[1], reverse=True)  # Descending
```

---

## 6. List Comprehension

### Basic Syntax
```python
# [expression for item in iterable if condition]

# Basic
squares = [x**2 for x in range(5)]  # [0, 1, 4, 9, 16]

# With condition
evens = [x for x in range(10) if x % 2 == 0]  # [0, 2, 4, 6, 8]

# With if-else
result = [x if x > 0 else 0 for x in [-1, 2, -3, 4]]  # [0, 2, 0, 4]

# Nested loops
matrix = [[1, 2, 3], [4, 5, 6]]
flat = [x for row in matrix for x in row]  # [1, 2, 3, 4, 5, 6]

# Create matrix
matrix = [[0]*3 for _ in range(3)]  # [[0,0,0], [0,0,0], [0,0,0]]
# CAUTION: [[0]*3]*3 creates shared references!
```

### Dictionary Comprehension
```python
# {key: value for item in iterable if condition}

squares = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, ...}

# Swap keys and values
d = {'a': 1, 'b': 2}
swapped = {v: k for k, v in d.items()}  # {1: 'a', 2: 'b'}

# From two lists
keys = ['a', 'b', 'c']
values = [1, 2, 3]
d = {k: v for k, v in zip(keys, values)}  # {'a': 1, 'b': 2, 'c': 3}
```

### Set Comprehension
```python
{x**2 for x in range(5)}  # {0, 1, 4, 9, 16}
```

### Generator Expression
```python
# Like list comprehension but lazy (memory efficient)
gen = (x**2 for x in range(1000000))
next(gen)  # 0
next(gen)  # 1

# Use in functions
sum(x**2 for x in range(10))
```

---

## 7. Sorting & Searching

### Sorting
```python
lst = [3, 1, 4, 1, 5, 9, 2, 6]

# Built-in sort
sorted(lst)                     # Returns new list
sorted(lst, reverse=True)       # Descending
lst.sort()                      # In-place

# Custom key
words = ["apple", "fig", "banana", "date"]
sorted(words, key=len)          # Sort by length
sorted(words, key=str.lower)    # Case-insensitive

# Sort objects
class Student:
    def __init__(self, name, grade):
        self.name = name
        self.grade = grade

students = [Student("Alice", 85), Student("Bob", 92)]
sorted(students, key=lambda s: s.grade)

# Sort by multiple keys
data = [(1, 'b'), (2, 'a'), (1, 'a')]
sorted(data, key=lambda x: (x[0], x[1]))  # Sort by first, then second

# Multi-key sorting dengan kombinasi ascending/descending
data = [(6, 55), (4, 40), (6, 45), (10, 50)]

# Semua descending (pakai reverse=True)
sorted(data, key=lambda x: (x[0], x[1]), reverse=True)
# [(10, 50), (6, 55), (6, 45), (4, 40)]

# Elemen pertama descending, kedua ascending (pakai minus di elemen pertama)
sorted(data, key=lambda x: (-x[0], x[1]))
# [(10, 50), (6, 45), (6, 55), (4, 40)]

# Elemen pertama ascending, kedua descending (pakai minus di elemen kedua)
sorted(data, key=lambda x: (x[0], -x[1]))
# [(4, 40), (6, 55), (6, 45), (10, 50)]

# Keduanya descending (pakai minus di semua elemen)
sorted(data, key=lambda x: (-x[0], -x[1]))
# [(10, 50), (6, 55), (6, 45), (4, 40)]
```

### Binary Search
```python
import bisect

lst = [1, 3, 5, 7, 9]

# Find insertion point
bisect.bisect_left(lst, 5)    # 2 (index of 5)
bisect.bisect_right(lst, 5)   # 3 (after 5)

# Insert maintaining order
bisect.insort_left(lst, 4)    # [1, 3, 4, 5, 7, 9]

# Binary search function
def binary_search(lst, target):
    left, right = 0, len(lst) - 1
    while left <= right:
        mid = (left + right) // 2
        if lst[mid] == target:
            return mid
        elif lst[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

---

## 8. Math Operations

### Basic Math
```python
# Arithmetic
a + b       # Addition
a - b       # Subtraction
a * b       # Multiplication
a / b       # Division (float result)
a // b      # Floor division (integer result)
a % b       # Modulo (remainder)
a ** b      # Exponentiation

# Compound assignment
a += 1      # a = a + 1
a -= 1
a *= 2
a /= 2
a //= 2
a %= 2
a **= 2
```

### Math Module
```python
import math

# Rounding
math.floor(3.7)         # 3 (round down)
math.ceil(3.2)          # 4 (round up)
round(3.5)              # 4 (banker's rounding)
round(3.14159, 2)       # 3.14 (2 decimal places)
math.trunc(3.7)         # 3 (truncate towards zero)
math.trunc(-3.7)        # -3

# Absolute value
abs(-5)                 # 5
math.fabs(-5.0)         # 5.0 (float)

# Power & Root
math.sqrt(16)           # 4.0
math.pow(2, 3)          # 8.0
2 ** 0.5                # Square root alternative

# Logarithm
math.log(10)            # Natural log (ln)
math.log10(100)         # Log base 10 -> 2.0
math.log2(8)            # Log base 2 -> 3.0
math.log(8, 2)          # Log base 2 -> 3.0

# Constants
math.pi                 # 3.141592653589793
math.e                  # 2.718281828459045
math.inf                # Infinity
float('inf')            # Infinity
float('-inf')           # -Infinity

# GCD & LCM
math.gcd(12, 8)         # 4
math.lcm(12, 8)         # 24 (Python 3.9+)

# Factorial & Combinations
math.factorial(5)       # 120
math.comb(5, 2)         # 10 (C(5,2))
math.perm(5, 2)         # 20 (P(5,2))

# Min, Max dengan multiple
max(1, 2, 3, 4)         # 4
min(1, 2, 3, 4)         # 1
max([1, 2, 3, 4])       # 4 (list)
```

### Numeric Tricks
```python
# Check if perfect square
def is_perfect_square(n):
    root = int(math.sqrt(n))
    return root * root == n

# Check if prime
def is_prime(n):
    if n < 2:
        return False
    for i in range(2, int(math.sqrt(n)) + 1):
        if n % i == 0:
            return False
    return True

# Get digits of number
digits = [int(d) for d in str(123)]  # [1, 2, 3]

# Sum of digits
sum(int(d) for d in str(12345))  # 15

# Reverse number
int(str(123)[::-1])  # 321
```

---

## 9. Built-in Functions

### Common Built-ins
```python
# Type checking
type(x)                 # Get type
isinstance(x, int)      # Check type

# Length & Size
len([1, 2, 3])          # 3
len("hello")            # 5
len({1, 2, 3})          # 3

# Range
range(5)                # 0, 1, 2, 3, 4
range(1, 6)             # 1, 2, 3, 4, 5
range(0, 10, 2)         # 0, 2, 4, 6, 8
list(range(5))          # [0, 1, 2, 3, 4]

# Enumerate
list(enumerate(['a', 'b', 'c']))  # [(0, 'a'), (1, 'b'), (2, 'c')]

# Zip
list(zip([1, 2], ['a', 'b']))  # [(1, 'a'), (2, 'b')]

# Map, Filter, Reduce
list(map(str, [1, 2, 3]))           # ['1', '2', '3']
list(filter(lambda x: x > 0, [-1, 2, -3, 4]))  # [2, 4]

from functools import reduce
reduce(lambda x, y: x + y, [1, 2, 3, 4])  # 10

# Any, All
any([False, True, False])   # True (at least one True)
all([True, True, False])    # False (all must be True)
any([])                     # False
all([])                     # True

# Min, Max, Sum
min([1, 2, 3])              # 1
max([1, 2, 3])              # 3
sum([1, 2, 3])              # 6
sum([1, 2, 3], 10)          # 16 (start from 10)

# Sorted & Reversed
sorted([3, 1, 2])           # [1, 2, 3]
list(reversed([1, 2, 3]))   # [3, 2, 1]

# Abs, Round, Pow
abs(-5)                     # 5
round(3.14159, 2)           # 3.14
pow(2, 3)                   # 8
pow(2, 3, 5)                # 3 (2^3 % 5)

# Input/Output
x = input("Enter: ")        # String input
x = int(input())            # Integer input
print(a, b, sep=", ")       # Custom separator
print(a, end="")            # No newline

# Conversion
bin(10)                     # '0b1010'
oct(10)                     # '0o12'
hex(10)                     # '0xa'
int('1010', 2)              # 10 (binary to int)
int('12', 8)                # 10 (octal to int)
int('a', 16)                # 10 (hex to int)
chr(65)                     # 'A'
ord('A')                    # 65
```

---

## 10. Itertools & Collections

### itertools
```python
from itertools import *

# Permutations
list(permutations([1, 2, 3]))       # All permutations
list(permutations([1, 2, 3], 2))    # Permutations of length 2

# Combinations
list(combinations([1, 2, 3], 2))    # [(1,2), (1,3), (2,3)]
list(combinations_with_replacement([1, 2], 2))  # [(1,1), (1,2), (2,2)]

# Product (Cartesian product)
list(product([1, 2], ['a', 'b']))   # [(1,'a'), (1,'b'), (2,'a'), (2,'b')]
list(product([0, 1], repeat=3))     # All binary of length 3

# Count, Cycle, Repeat
count(10, 2)                        # 10, 12, 14, ... (infinite)
cycle([1, 2, 3])                    # 1, 2, 3, 1, 2, 3, ... (infinite)
repeat('x', 3)                      # 'x', 'x', 'x'

# Chain (combine iterables)
list(chain([1, 2], [3, 4]))         # [1, 2, 3, 4]

# GroupBy (consecutive groups)
data = [('a', 1), ('a', 2), ('b', 3)]
for key, group in groupby(data, key=lambda x: x[0]):
    print(key, list(group))

# Accumulate
list(accumulate([1, 2, 3, 4]))      # [1, 3, 6, 10] (cumulative sum)
list(accumulate([1, 2, 3, 4], lambda x, y: x * y))  # [1, 2, 6, 24]
```

### collections
```python
from collections import *

# Counter
c = Counter(['a', 'b', 'a', 'c', 'a', 'b'])
c.most_common(2)                    # [('a', 3), ('b', 2)]
c['a']                              # 3
c.update(['a', 'd'])                # Add counts

# defaultdict
d = defaultdict(int)                # Default 0
d = defaultdict(list)               # Default []
d['key'] += 1                       # No KeyError

# deque (double-ended queue)
dq = deque([1, 2, 3])
dq.append(4)                        # [1, 2, 3, 4]
dq.appendleft(0)                    # [0, 1, 2, 3, 4]
dq.pop()                            # Remove right
dq.popleft()                        # Remove left
dq.rotate(1)                        # Rotate right
dq.rotate(-1)                       # Rotate left

# OrderedDict (maintains insertion order)
od = OrderedDict()
od['a'] = 1
od['b'] = 2
od.move_to_end('a')                 # Move 'a' to end

# namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
p.x, p.y                            # 1, 2
```

### heapq (Priority Queue / Min Heap)
```python
import heapq

lst = [3, 1, 4, 1, 5, 9]
heapq.heapify(lst)                  # Convert to heap in-place

heapq.heappush(lst, 2)              # Push element
heapq.heappop(lst)                  # Pop smallest

heapq.nlargest(3, lst)              # 3 largest elements
heapq.nsmallest(3, lst)             # 3 smallest elements

# Max heap (negate values)
max_heap = [-x for x in lst]
heapq.heapify(max_heap)
-heapq.heappop(max_heap)            # Get max

# Heap with custom key (use tuple)
heap = []
heapq.heappush(heap, (priority, item))
```

---

## 11. Pattern Problem Solving

### Two Pointers
```python
# Find pair with target sum in sorted array
def two_sum_sorted(arr, target):
    left, right = 0, len(arr) - 1
    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return [left, right]
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    return []

# Remove duplicates from sorted array
def remove_duplicates(arr):
    if not arr:
        return 0
    write = 1
    for read in range(1, len(arr)):
        if arr[read] != arr[read - 1]:
            arr[write] = arr[read]
            write += 1
    return write
```

### Sliding Window
```python
# Maximum sum subarray of size k
def max_sum_subarray(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum
    
    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i - k]
        max_sum = max(max_sum, window_sum)
    
    return max_sum

# Longest substring without repeating characters
def longest_unique_substring(s):
    char_index = {}
    max_len = 0
    start = 0
    
    for i, char in enumerate(s):
        if char in char_index and char_index[char] >= start:
            start = char_index[char] + 1
        char_index[char] = i
        max_len = max(max_len, i - start + 1)
    
    return max_len
```

### Hash Map / Frequency Count
```python
# Two Sum (find indices)
def two_sum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []

# Find duplicate
def find_duplicate(nums):
    seen = set()
    for num in nums:
        if num in seen:
            return num
        seen.add(num)
    return -1

# Anagram check
def is_anagram(s1, s2):
    return Counter(s1) == Counter(s2)
```

### Kadane's Algorithm (Maximum Subarray)
```python
def max_subarray_sum(arr):
    max_sum = current_sum = arr[0]
    
    for num in arr[1:]:
        current_sum = max(num, current_sum + num)
        max_sum = max(max_sum, current_sum)
    
    return max_sum
```

### Prefix Sum
```python
# Build prefix sum
def build_prefix_sum(arr):
    prefix = [0] * (len(arr) + 1)
    for i, num in enumerate(arr):
        prefix[i + 1] = prefix[i] + num
    return prefix

# Range sum query
def range_sum(prefix, left, right):
    return prefix[right + 1] - prefix[left]
```

---

## 12. Common Algorithms

### Recursion Templates
```python
# Factorial
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

# Fibonacci
def fib(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 1:
        return n
    memo[n] = fib(n - 1, memo) + fib(n - 2, memo)
    return memo[n]

# Fibonacci iterative
def fib_iter(n):
    if n <= 1:
        return n
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b
```

### Graph Algorithms
```python
# BFS
from collections import deque

def bfs(graph, start):
    visited = set([start])
    queue = deque([start])
    result = []
    
    while queue:
        node = queue.popleft()
        result.append(node)
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return result

# DFS
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(start)
    result = [start]
    
    for neighbor in graph[start]:
        if neighbor not in visited:
            result.extend(dfs(graph, neighbor, visited))
    
    return result

# DFS iterative
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    result = []
    
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            result.append(node)
            stack.extend(graph[node])
    
    return result
```

### Dynamic Programming
```python
# 0/1 Knapsack
def knapsack(weights, values, capacity):
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(capacity + 1):
            if weights[i-1] <= w:
                dp[i][w] = max(dp[i-1][w], 
                              values[i-1] + dp[i-1][w - weights[i-1]])
            else:
                dp[i][w] = dp[i-1][w]
    
    return dp[n][capacity]

# Longest Common Subsequence
def lcs(s1, s2):
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]

# Coin Change (minimum coins)
def coin_change(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    
    for coin in coins:
        for x in range(coin, amount + 1):
            dp[x] = min(dp[x], dp[x - coin] + 1)
    
    return dp[amount] if dp[amount] != float('inf') else -1
```

---

## 13. Tips & Tricks

### Common Tricks
```python
# Swap variables
a, b = b, a

# Multiple assignment
a = b = c = 0

# Chained comparison
1 < x < 10

# Check all same
all(x == lst[0] for x in lst)

# Flatten 2D list
flat = [x for row in matrix for x in row]

# Transpose matrix
transposed = list(zip(*matrix))

# Dictionary from two lists
d = dict(zip(keys, values))

# Get both index and value
for i, val in enumerate(lst):
    pass

# Iterate in reverse
for x in reversed(lst):
    pass

# Infinity
float('inf')
float('-inf')

# Default dict value
d.get(key, default_value)
d.setdefault(key, default_value)

# Copy with slice
new_list = old_list[:]

# Check if list is empty
if not lst:
    print("Empty")

# Get unique values maintaining order
list(dict.fromkeys(lst))
```

### Input Parsing untuk Kompetisi
```python
# Single integer
n = int(input())

# Multiple integers on one line
a, b, c = map(int, input().split())

# List of integers
lst = list(map(int, input().split()))

# Grid/Matrix
matrix = []
for _ in range(n):
    row = list(map(int, input().split()))
    matrix.append(row)

# Or one-liner
matrix = [list(map(int, input().split())) for _ in range(n)]

# String to list of chars
chars = list(input())
```

### Useful One-liners
```python
# Generate primes (Sieve)
def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n**0.5) + 1):
        if is_prime[i]:
            for j in range(i*i, n + 1, i):
                is_prime[j] = False
    return [i for i, p in enumerate(is_prime) if p]

# Count frequency
freq = {}
for x in lst:
    freq[x] = freq.get(x, 0) + 1
# Or
from collections import Counter
freq = Counter(lst)

# Group by key
from itertools import groupby
groups = {k: list(v) for k, v in groupby(sorted(data), key=lambda x: x[0])}

# Find most common element
from collections import Counter
Counter(lst).most_common(1)[0][0]

# Rotate list
rotated = lst[k:] + lst[:k]  # Rotate left by k
rotated = lst[-k:] + lst[:-k]  # Rotate right by k
```

---

## Quick Reference Table

| Category | Function | Example |
|----------|----------|---------|
| **String** | `len`, `upper`, `lower`, `strip` | `len("hello")` → 5 |
| **List** | `append`, `pop`, `sort`, `reverse` | `lst.append(1)` |
| **Set** | `add`, `remove`, `union`, `intersection` | `a \| b` |
| **Dict** | `get`, `keys`, `values`, `items` | `d.get(k, 0)` |
| **Math** | `abs`, `round`, `floor`, `ceil` | `round(3.14, 1)` |
| **Sorting** | `sorted`, `sort`, `key` | `sorted(lst, key=len)` |
| **Search** | `in`, `find`, `index` | `3 in lst` |
| **Iteration** | `enumerate`, `zip`, `range` | `enumerate(lst)` |
| **Aggregate** | `sum`, `min`, `max`, `any`, `all` | `sum(lst)` |

---

> 💡 **Tips untuk Tes Coding:**
> 1. Selalu pertimbangkan **edge cases** (empty input, negative numbers, etc.)
> 2. Pahami **time complexity** dari solusi Anda
> 3. Gunakan **built-in functions** sebanyak mungkin
> 4. Untuk problem solving, pikirkan pattern: Two Pointers, Sliding Window, Hash Map, DP
> 5. Test dengan contoh kecil sebelum submit
