# 🔷 TYPESCRIPT CHEATSHEET LENGKAP
## Untuk Tes Coding & Problem Solving

> TypeScript = JavaScript + Static Type System. Cheatsheet ini fokus pada **fitur TS yang berbeda dari JS**. Untuk array methods, string operations, dan patterns (BFS/DFS/DP dll), lihat `javascript-cheatsheet.md` — semuanya berlaku di TS.

---

## Daftar Isi
1. [Setup & Dasar](#1-setup--dasar)
2. [Type Annotations](#2-type-annotations)
3. [Union, Intersection & Literal Types](#3-union-intersection--literal-types)
4. [Type Narrowing & Guards](#4-type-narrowing--guards)
5. [Interface & Type Alias](#5-interface--type-alias)
6. [Enum](#6-enum)
7. [Generics](#7-generics)
8. [Utility Types](#8-utility-types)
9. [Class (OOP)](#9-class-oop)
10. [Functions Lanjutan](#10-functions-lanjutan)
11. [Tuple & Readonly](#11-tuple--readonly)
12. [Type Assertion & Casting](#12-type-assertion--casting)
13. [Null Safety](#13-null-safety)
14. [Common Data Structures (Typed)](#14-common-data-structures-typed)
15. [I/O Template (HackerEarth)](#15-io-template-hackerearth)
16. [Tips & Tricks](#16-tips--tricks)

---

## 1. Setup & Dasar

### Menjalankan TypeScript
```bash
# Install
npm install -g typescript ts-node

# Compile & Run
tsc file.ts          # compile ke .js
node file.js         # jalankan .js

# Langsung run (tanpa compile manual)
ts-node file.ts
npx ts-node file.ts

# Init project
tsc --init           # buat tsconfig.json
```

### Struktur File
```typescript
// file.ts
function greet(name: string): string {
    return `Hello, ${name}!`;
}

console.log(greet("World"));
```

---

## 2. Type Annotations

### Basic Types
```typescript
// Primitif
let num: number = 42;
let str: string = "hello";
let bool: boolean = true;
let big: bigint = 100n;

// Any (hindari! mematikan type checking)
let anything: any = 42;
anything = "hello";         // no error

// Unknown (lebih aman dari any)
let val: unknown = 42;
// val.toFixed()            // ERROR! harus type check dulu
if (typeof val === "number") {
    val.toFixed();           // OK setelah narrowing
}

// Void (function tanpa return)
function log(msg: string): void {
    console.log(msg);
}

// Never (function yang tidak pernah return)
function throwError(msg: string): never {
    throw new Error(msg);
}

// Null & Undefined
let n: null = null;
let u: undefined = undefined;
```

### Array Types
```typescript
// 2 cara deklarasi (equivalen)
let nums: number[] = [1, 2, 3];
let nums2: Array<number> = [1, 2, 3];

// Array of strings
let words: string[] = ["hello", "world"];

// Array of mixed (pakai union)
let mixed: (number | string)[] = [1, "two", 3];

// 2D Array
let matrix: number[][] = [[1, 2], [3, 4]];

// Typed initialization
let arr: number[] = new Array<number>(5).fill(0);
let grid: number[][] = Array.from({length: 3}, () => new Array(4).fill(0));
```

### Object Types
```typescript
// Inline object type
let person: { name: string; age: number } = {
    name: "Alice",
    age: 25
};

// Optional property (?)
let config: { host: string; port?: number } = {
    host: "localhost"
    // port opsional, bisa undefined
};

// Readonly property
let point: { readonly x: number; readonly y: number } = { x: 1, y: 2 };
// point.x = 5;  // ERROR!
```

---

## 3. Union, Intersection & Literal Types

### Union Type (A ATAU B)
```typescript
// Variabel bisa berisi beberapa tipe
let id: number | string;
id = 42;        // OK
id = "abc-123"; // OK
// id = true;   // ERROR!

// Function dengan union parameter
function format(input: number | string): string {
    if (typeof input === "number") {
        return input.toFixed(2);
    }
    return input.toUpperCase();
}

// Array yang bisa null
function first(arr: number[] | null): number | undefined {
    return arr?.[0];
}
```

### Intersection Type (A DAN B)
```typescript
type HasName = { name: string };
type HasAge = { age: number };

// Gabungan kedua tipe
type Person = HasName & HasAge;

let p: Person = { name: "Alice", age: 25 };  // harus punya KEDUA property
```

### Literal Types
```typescript
// Hanya boleh nilai tertentu
let direction: "up" | "down" | "left" | "right";
direction = "up";       // OK
// direction = "north"; // ERROR!

let dice: 1 | 2 | 3 | 4 | 5 | 6;
dice = 3;               // OK
// dice = 7;            // ERROR!

// Literal dalam function
function setStatus(status: "active" | "inactive" | "pending"): void {
    console.log(status);
}
```

---

## 4. Type Narrowing & Guards

### typeof Narrowing
```typescript
function process(input: number | string) {
    if (typeof input === "string") {
        // TS tahu input adalah string di sini
        console.log(input.toUpperCase());
    } else {
        // TS tahu input adalah number di sini
        console.log(input.toFixed(2));
    }
}
```

### instanceof Narrowing
```typescript
class Dog { bark() { return "woof"; } }
class Cat { meow() { return "meow"; } }

function speak(animal: Dog | Cat) {
    if (animal instanceof Dog) {
        console.log(animal.bark());    // TS tahu ini Dog
    } else {
        console.log(animal.meow());    // TS tahu ini Cat
    }
}
```

### in Narrowing
```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
    if ("swim" in animal) {
        animal.swim();     // TS tahu ini Fish
    } else {
        animal.fly();      // TS tahu ini Bird
    }
}
```

### Custom Type Guard
```typescript
function isString(val: unknown): val is string {
    return typeof val === "string";
}

function process(val: unknown) {
    if (isString(val)) {
        console.log(val.toUpperCase());  // TS tahu val = string
    }
}
```

### Discriminated Unions (SANGAT BERGUNA!)
```typescript
type Circle = { kind: "circle"; radius: number };
type Square = { kind: "square"; side: number };
type Shape = Circle | Square;

function area(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "square":
            return shape.side ** 2;
    }
}
```

---

## 5. Interface & Type Alias

### Interface
```typescript
interface Student {
    name: string;
    age: number;
    grade?: number;              // opsional
    readonly id: string;         // tidak bisa diubah
}

let s: Student = { name: "Alice", age: 20, id: "001" };

// Extend interface
interface GradStudent extends Student {
    thesis: string;
}

// Implement di class
class University implements Student {
    constructor(
        public name: string,
        public age: number,
        public readonly id: string
    ) {}
}
```

### Type Alias
```typescript
type Point = { x: number; y: number };
type ID = number | string;
type Callback = (data: string) => void;

// Type alias bisa untuk union, intersection, dll
type Result = "success" | "error";
type Coordinate = [number, number];      // tuple
```

### Interface vs Type Alias
```typescript
// Interface:
// - Bisa di-extend dan di-merge
// - Bagus untuk object shape & OOP
// - Declaration merging (otomatis gabung jika nama sama)

// Type Alias:
// - Bisa untuk SEMUA tipe (union, tuple, primitif, dll)
// - Lebih fleksibel
// - Tidak bisa merge

// REKOMENDASI: pakai interface untuk object, type untuk yang lain
```

---

## 6. Enum

### Numeric Enum
```typescript
enum Direction {
    Up,        // 0
    Down,      // 1
    Left,      // 2
    Right      // 3
}

// Custom value
enum StatusCode {
    OK = 200,
    NotFound = 404,
    ServerError = 500
}

let dir: Direction = Direction.Up;
console.log(dir);               // 0
console.log(Direction[0]);      // "Up" (reverse mapping)
```

### String Enum
```typescript
enum Color {
    Red = "RED",
    Green = "GREEN",
    Blue = "BLUE"
}

let c: Color = Color.Red;
console.log(c);                 // "RED"
// Tidak ada reverse mapping untuk string enum
```

### Const Enum (lebih efisien, inline saat compile)
```typescript
const enum Weekday {
    Mon, Tue, Wed, Thu, Fri
}
let day = Weekday.Mon;  // compile jadi: let day = 0;
```

### Enum Alternatives (lebih umum di TS modern)
```typescript
// Pakai const + as const (lebih type-safe)
const DIRECTION = {
    Up: "UP",
    Down: "DOWN",
    Left: "LEFT",
    Right: "RIGHT"
} as const;

type Direction = typeof DIRECTION[keyof typeof DIRECTION];
// "UP" | "DOWN" | "LEFT" | "RIGHT"
```

---

## 7. Generics

### Basic Generics
```typescript
// Function generic
function identity<T>(val: T): T {
    return val;
}

identity<number>(42);     // 42
identity<string>("hi");   // "hi"
identity(42);             // inferred: T = number

// Generic array function
function first<T>(arr: T[]): T | undefined {
    return arr[0];
}

first([1, 2, 3]);         // number | undefined
first(["a", "b"]);        // string | undefined
```

### Generic dengan Constraint
```typescript
// T harus punya property length
function longest<T extends { length: number }>(a: T, b: T): T {
    return a.length >= b.length ? a : b;
}

longest("abc", "de");           // "abc"
longest([1, 2, 3], [4, 5]);    // [1, 2, 3]
// longest(10, 20);            // ERROR! number tidak punya .length
```

### Generic Interface & Type
```typescript
interface Pair<K, V> {
    key: K;
    value: V;
}

let p: Pair<string, number> = { key: "age", value: 25 };

// Generic type alias
type Result<T> = { success: true; data: T } | { success: false; error: string };

let ok: Result<number> = { success: true, data: 42 };
let fail: Result<number> = { success: false, error: "not found" };
```

### Generic Class
```typescript
class Stack<T> {
    private items: T[] = [];

    push(item: T): void {
        this.items.push(item);
    }

    pop(): T | undefined {
        return this.items.pop();
    }

    peek(): T | undefined {
        return this.items[this.items.length - 1];
    }

    isEmpty(): boolean {
        return this.items.length === 0;
    }

    size(): number {
        return this.items.length;
    }
}

let numStack = new Stack<number>();
numStack.push(1);
numStack.push(2);
numStack.pop();   // 2

let strStack = new Stack<string>();
strStack.push("hello");
```

### Multiple Generics
```typescript
function swap<A, B>(pair: [A, B]): [B, A] {
    return [pair[1], pair[0]];
}

swap([1, "hello"]);  // ["hello", 1]
```

---

## 8. Utility Types

### Built-in Utility Types (SERING DIPAKAI!)
```typescript
interface User {
    name: string;
    age: number;
    email: string;
}

// Partial<T> → semua property jadi opsional
let update: Partial<User> = { name: "Bob" };

// Required<T> → semua property jadi wajib
let full: Required<User> = { name: "A", age: 1, email: "a@b" };

// Readonly<T> → semua property jadi readonly
let frozen: Readonly<User> = { name: "A", age: 1, email: "a@b" };
// frozen.name = "B";  // ERROR!

// Pick<T, K> → ambil property tertentu saja
type UserName = Pick<User, "name" | "email">;
// { name: string; email: string }

// Omit<T, K> → buang property tertentu
type UserNoEmail = Omit<User, "email">;
// { name: string; age: number }

// Record<K, V> → buat object type dari key & value type
let scores: Record<string, number> = {
    alice: 90,
    bob: 85
};

// Exclude<T, U> → hapus tipe dari union
type T = Exclude<"a" | "b" | "c", "a">;  // "b" | "c"

// Extract<T, U> → ambil tipe dari union
type T2 = Extract<"a" | "b" | "c", "a" | "f">;  // "a"

// NonNullable<T> → hapus null & undefined
type T3 = NonNullable<string | null | undefined>;  // string

// ReturnType<T> → dapatkan return type sebuah function
function getUser() { return { name: "A", age: 1 }; }
type UserReturn = ReturnType<typeof getUser>;
// { name: string; age: number }

// Parameters<T> → dapatkan parameter types
type Params = Parameters<typeof getUser>;  // []
```

---

## 9. Class (OOP)

### Dasar Class
```typescript
class Animal {
    // Access modifiers
    public name: string;      // bisa diakses dari mana saja (default)
    private age: number;      // hanya di dalam class ini
    protected type: string;   // di class ini + child classes
    readonly id: number;      // tidak bisa diubah setelah constructor

    constructor(name: string, age: number, type: string, id: number) {
        this.name = name;
        this.age = age;
        this.type = type;
        this.id = id;
    }

    // Method
    speak(): string {
        return `${this.name} makes a sound`;
    }

    // Getter & Setter
    get animalAge(): number {
        return this.age;
    }

    set animalAge(val: number) {
        if (val >= 0) this.age = val;
    }
}

// Shorthand constructor (auto assign)
class Student {
    constructor(
        public name: string,
        public age: number,
        private grade: number = 0
    ) {}
    // Sama saja dengan deklarasi + assign manual di constructor!
}
```

### Inheritance
```typescript
class Dog extends Animal {
    constructor(name: string, age: number) {
        super(name, age, "dog", Math.random());
    }

    // Override method
    speak(): string {
        return `${this.name} barks`;
    }

    fetch(): string {
        return "fetching!";
    }
}

let dog = new Dog("Rex", 3);
dog.speak();   // "Rex barks"
dog.fetch();   // "fetching!"
```

### Abstract Class
```typescript
abstract class Shape {
    abstract area(): number;         // HARUS diimplementasi oleh child
    abstract perimeter(): number;

    describe(): string {             // implementasi biasa boleh ada
        return `Area: ${this.area()}`;
    }
}

class Circle extends Shape {
    constructor(private radius: number) { super(); }

    area(): number {
        return Math.PI * this.radius ** 2;
    }

    perimeter(): number {
        return 2 * Math.PI * this.radius;
    }
}

// let s = new Shape();  // ERROR! abstract class tidak bisa diinstansiasi
let c = new Circle(5);
c.describe();            // "Area: 78.53..."
```

### Static Members
```typescript
class Counter {
    static count = 0;

    static increment(): void {
        Counter.count++;
    }

    static getCount(): number {
        return Counter.count;
    }
}

Counter.increment();
Counter.increment();
Counter.getCount();   // 2
```

---

## 10. Functions Lanjutan

### Function Types
```typescript
// Deklarasi tipe function
type MathFn = (a: number, b: number) => number;

const add: MathFn = (a, b) => a + b;
const sub: MathFn = (a, b) => a - b;

// Callback type
function apply(arr: number[], fn: (x: number) => number): number[] {
    return arr.map(fn);
}

apply([1, 2, 3], x => x * 2);   // [2, 4, 6]
```

### Overloading
```typescript
// Function overloads (deklarasi signature tanpa body)
function format(input: number): string;
function format(input: string): string;
function format(input: number | string): string {
    if (typeof input === "number") {
        return input.toFixed(2);
    }
    return input.toUpperCase();
}

format(3.14);    // "3.14"
format("hello"); // "HELLO"
```

### Generic Functions untuk Problem Solving
```typescript
// Generic swap
function swap<T>(arr: T[], i: number, j: number): void {
    [arr[i], arr[j]] = [arr[j], arr[i]];
}

// Generic binary search
function binarySearch<T>(arr: T[], target: T, compare: (a: T, b: T) => number): number {
    let lo = 0, hi = arr.length - 1;
    while (lo <= hi) {
        const mid = Math.floor((lo + hi) / 2);
        const cmp = compare(arr[mid], target);
        if (cmp === 0) return mid;
        if (cmp < 0) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}

binarySearch([1, 3, 5, 7], 5, (a, b) => a - b);  // 2
```

---

## 11. Tuple & Readonly

### Tuple (Fixed-length Array)
```typescript
// Array dengan panjang dan tipe tertentu per posisi
let point: [number, number] = [10, 20];
let entry: [string, number] = ["alice", 90];

// Access
point[0]     // 10 (number)
point[1]     // 20 (number)

// Destructuring
let [x, y] = point;

// Named tuple (lebih readable)
type Coordinate = [x: number, y: number, z?: number];
let pos: Coordinate = [1, 2];

// Optional elements
type Range = [start: number, end?: number];
let r: Range = [0];       // OK
let r2: Range = [0, 10];  // OK

// Rest elements
type StringNumberPair = [string, ...number[]];
let data: StringNumberPair = ["scores", 90, 85, 78];
```

### Readonly
```typescript
// Readonly array
let nums: readonly number[] = [1, 2, 3];
// nums.push(4);       // ERROR!
// nums[0] = 99;       // ERROR!

// ReadonlyArray
let arr: ReadonlyArray<number> = [1, 2, 3];

// Readonly tuple
let point: readonly [number, number] = [1, 2];

// as const (deep readonly)
let config = {
    host: "localhost",
    port: 3000,
    tags: ["web", "api"]
} as const;

// config.port = 8080;         // ERROR!
// config.tags.push("new");    // ERROR!
// type: { readonly host: "localhost"; readonly port: 3000; readonly tags: readonly ["web", "api"] }
```

---

## 12. Type Assertion & Casting

### Type Assertion
```typescript
// Ketika kamu tahu tipe lebih spesifik dari TS
let val: unknown = "hello";
let len: number = (val as string).length;

// Alternatif syntax (tidak bisa di JSX/TSX)
let len2: number = (<string>val).length;

// Non-null assertion (!)
function getElement(id: string): HTMLElement {
    return document.getElementById(id)!;  // "pasti bukan null"
    // ⚠️ hati-hati, bisa runtime error!
}

// Const assertion
let x = "hello" as const;     // type: "hello" (literal), bukan string
let arr = [1, 2, 3] as const; // type: readonly [1, 2, 3]
```

---

## 13. Null Safety

### Strict Null Checks
```typescript
// Dengan strictNullChecks: true (RECOMMENDED)
let name: string = "Alice";
// name = null;     // ERROR!
// name = undefined; // ERROR!

// Boleh null/undefined harus explicit
let name2: string | null = null;        // OK
let name3: string | undefined = undefined; // OK
```

### Handling Null
```typescript
// Optional chaining (?.)
let user: { profile?: { name?: string } } | null = null;
let name = user?.profile?.name;   // undefined (tidak error)

// Nullish coalescing (??)
let val = null ?? "default";      // "default"
let val2 = 0 ?? "default";       // 0 (hanya null/undefined trigger ??)
let val3 = "" ?? "default";      // "" (berbeda dengan ||)

// Logical OR (||) — semua falsy trigger
let val4 = 0 || "default";       // "default" (0 falsy)
let val5 = "" || "default";      // "default" ("" falsy)

// Nullish assignment (??=)
let x: number | null = null;
x ??= 42;                        // x = 42 (karena null)
```

---

## 14. Common Data Structures (Typed)

### Typed Stack
```typescript
class Stack<T> {
    private items: T[] = [];
    push(item: T): void { this.items.push(item); }
    pop(): T | undefined { return this.items.pop(); }
    peek(): T | undefined { return this.items[this.items.length - 1]; }
    isEmpty(): boolean { return this.items.length === 0; }
    size(): number { return this.items.length; }
}
```

### Typed Queue
```typescript
class Queue<T> {
    private items: Record<number, T> = {};
    private head = 0;
    private tail = 0;

    enqueue(val: T): void { this.items[this.tail++] = val; }
    dequeue(): T | undefined {
        if (this.isEmpty()) return undefined;
        const val = this.items[this.head];
        delete this.items[this.head++];
        return val;
    }
    peek(): T | undefined { return this.items[this.head]; }
    isEmpty(): boolean { return this.head === this.tail; }
    size(): number { return this.tail - this.head; }
}
```

### Typed MinHeap
```typescript
class MinHeap<T> {
    private heap: T[] = [];
    constructor(private compare: (a: T, b: T) => number = (a: any, b: any) => a - b) {}

    push(val: T): void {
        this.heap.push(val);
        this._bubbleUp(this.heap.length - 1);
    }

    pop(): T | undefined {
        if (this.heap.length === 0) return undefined;
        const top = this.heap[0];
        const last = this.heap.pop()!;
        if (this.heap.length > 0) {
            this.heap[0] = last;
            this._sinkDown(0);
        }
        return top;
    }

    peek(): T | undefined { return this.heap[0]; }
    size(): number { return this.heap.length; }

    private _bubbleUp(i: number): void {
        while (i > 0) {
            const parent = Math.floor((i - 1) / 2);
            if (this.compare(this.heap[parent], this.heap[i]) <= 0) break;
            [this.heap[parent], this.heap[i]] = [this.heap[i], this.heap[parent]];
            i = parent;
        }
    }

    private _sinkDown(i: number): void {
        const n = this.heap.length;
        while (true) {
            let smallest = i;
            const left = 2 * i + 1, right = 2 * i + 2;
            if (left < n && this.compare(this.heap[left], this.heap[smallest]) < 0) smallest = left;
            if (right < n && this.compare(this.heap[right], this.heap[smallest]) < 0) smallest = right;
            if (smallest === i) break;
            [this.heap[smallest], this.heap[i]] = [this.heap[i], this.heap[smallest]];
            i = smallest;
        }
    }
}

// Usage
const numHeap = new MinHeap<number>();
numHeap.push(5);
numHeap.push(2);
numHeap.push(8);
numHeap.pop();   // 2

// Max heap
const maxHeap = new MinHeap<number>((a, b) => b - a);

// Custom object heap
const taskHeap = new MinHeap<{priority: number; name: string}>(
    (a, b) => a.priority - b.priority
);
```

### Typed Graph
```typescript
class Graph<T> {
    private adj: Map<T, T[]> = new Map();

    addVertex(v: T): void {
        if (!this.adj.has(v)) this.adj.set(v, []);
    }

    addEdge(u: T, v: T): void {
        this.addVertex(u);
        this.addVertex(v);
        this.adj.get(u)!.push(v);
        this.adj.get(v)!.push(u);  // undirected
    }

    bfs(start: T): T[] {
        const visited = new Set<T>([start]);
        const queue: T[] = [start];
        const result: T[] = [];

        while (queue.length > 0) {
            const node = queue.shift()!;
            result.push(node);
            for (const neighbor of this.adj.get(node) || []) {
                if (!visited.has(neighbor)) {
                    visited.add(neighbor);
                    queue.push(neighbor);
                }
            }
        }
        return result;
    }

    dfs(start: T, visited = new Set<T>()): T[] {
        visited.add(start);
        const result: T[] = [start];
        for (const neighbor of this.adj.get(start) || []) {
            if (!visited.has(neighbor)) {
                result.push(...this.dfs(neighbor, visited));
            }
        }
        return result;
    }
}
```

---

## 15. I/O Template (HackerEarth)

### Template Competitive Programming
```typescript
import * as readline from 'readline';

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

const lines: string[] = [];
rl.on('line', (line: string) => lines.push(line.trim()));
rl.on('close', () => {
    let idx = 0;
    const nextLine = (): string => lines[idx++];
    const nextInt = (): number => parseInt(nextLine());
    const nextInts = (): number[] => nextLine().split(' ').map(Number);

    const t = nextInt();
    for (let i = 0; i < t; i++) {
        const n = nextInt();
        const arr = nextInts();
        console.log(solve(n, arr));
    }
});

function solve(n: number, arr: number[]): string | number {
    // ... logika solusi ...
    return 0;
}
```

### Alternative (process.stdin)
```typescript
let input = '';
process.stdin.on('data', (d: Buffer) => input += d);
process.stdin.on('end', () => {
    const lines = input.trim().split('\n');
    let idx = 0;
    const next = (): string => lines[idx++].trim();

    const n: number = parseInt(next());
    const arr: number[] = next().split(' ').map(Number);

    console.log(solve(n, arr));
});

function solve(n: number, arr: number[]): number {
    // ...
    return 0;
}
```

---

## 16. Tips & Tricks

### TS-Specific Tips
```typescript
// 1. Gunakan strict mode di tsconfig.json
// "strict": true → enables strictNullChecks, noImplicitAny, dll

// 2. Prefer interface untuk objects, type untuk union/intersection
interface User { name: string; }       // object shape
type ID = string | number;             // union
type Pair = [string, number];          // tuple

// 3. Hindari any, pakai unknown + type narrowing
function safe(val: unknown): string {
    if (typeof val === "string") return val;
    return String(val);
}

// 4. Record untuk dynamic keys
const cache: Record<string, number> = {};
cache["key1"] = 42;

// 5. keyof untuk type-safe key access
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

// 6. typeof untuk mendapatkan type dari value
const config = { port: 3000, host: "localhost" };
type Config = typeof config;  // { port: number; host: string }

// 7. Indexed access type
type Users = { users: { name: string; age: number }[] };
type User = Users["users"][number];  // { name: string; age: number }

// 8. Template literal types
type EventName = `on${Capitalize<"click" | "focus">}`;
// "onClick" | "onFocus"

// 9. Satisfies operator (TS 4.9+)
const palette = {
    red: [255, 0, 0],
    green: "#00ff00"
} satisfies Record<string, string | number[]>;
// Tipe tetap spesifik, tapi dicek terhadap Record

// 10. Type dari array element
const fruits = ["apple", "banana", "cherry"] as const;
type Fruit = typeof fruits[number];  // "apple" | "banana" | "cherry"
```

### Perbedaan TS vs JS (Ringkasan)
| Aspek | JavaScript | TypeScript |
|-------|-----------|------------|
| Typing | Dynamic | Static (compile-time) |
| File extension | `.js` | `.ts` / `.tsx` |
| Compile | Langsung run | Compile dulu ke `.js` |
| Type errors | Runtime | Compile-time ✅ |
| Interface | Tidak ada | Ada ✅ |
| Enum | Tidak ada | Ada ✅ |
| Generics | Tidak ada | Ada ✅ |
| Access modifiers | Tidak ada (punya `#private`) | `public`, `private`, `protected` ✅ |
| Null safety | Manual (`?.`, `??`) | Strict null checks ✅ |
| Overloading | Tidak ada | Ada ✅ |
| Decorator | Proposal | Ada (experimental) |

