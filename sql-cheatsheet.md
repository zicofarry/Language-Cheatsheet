# 📚 SQL CHEATSHEET LENGKAP

## Daftar Isi
1. [Dasar SQL (SELECT, FROM, WHERE)](#1-dasar-sql)
2. [Filtering Data](#2-filtering-data)
3. [Sorting & Limiting](#3-sorting--limiting)
4. [Aggregate Functions](#4-aggregate-functions)
5. [GROUP BY & HAVING](#5-group-by--having)
6. [JOIN Operations](#6-join-operations)
7. [Subqueries](#7-subqueries)
8. [UNION & Set Operations](#8-union--set-operations)
9. [String Functions](#9-string-functions)
10. [Date Functions](#10-date-functions)
11. [Casting (Konversi Tipe Data)](#11-casting-konversi-tipe-data)
12. [Rounding (Pembulatan)](#12-rounding-pembulatan)
13. [CASE WHEN](#13-case-when)
14. [Window Functions](#14-window-functions)
15. [Data Manipulation (INSERT, UPDATE, DELETE)](#15-data-manipulation)
16. [Table Operations (CREATE, ALTER, DROP)](#16-table-operations)
17. [Constraints](#17-constraints)
18. [Views](#18-views)
19. [Indexes](#19-indexes)
20. [Common Table Expressions (CTE)](#20-common-table-expressions-cte)
21. [Contoh Kasus Real-World](#21-contoh-kasus-real-world)

---

## 1. Dasar SQL

### SELECT - Mengambil Data
```sql
-- Mengambil semua kolom
SELECT * FROM employees;

-- Mengambil kolom tertentu
SELECT first_name, last_name, salary FROM employees;

-- Mengambil dengan alias (AS)
SELECT first_name AS nama_depan, last_name AS nama_belakang FROM employees;

-- Mengambil nilai unik (tanpa duplikat)
SELECT DISTINCT department FROM employees;

-- Menghitung jumlah baris
SELECT COUNT(*) FROM employees;
```

---

## 2. Filtering Data

### WHERE - Filter Kondisi Dasar
```sql
-- Filter dengan nilai tertentu
SELECT * FROM employees WHERE department = 'IT';

-- Filter dengan angka
SELECT * FROM employees WHERE salary > 5000000;

-- Filter dengan tanggal
SELECT * FROM employees WHERE hire_date = '2024-01-15';
```

### Operator Perbandingan
| Operator | Deskripsi | Contoh |
|----------|-----------|--------|
| `=` | Sama dengan | `WHERE age = 25` |
| `<>` atau `!=` | Tidak sama dengan | `WHERE status <> 'inactive'` |
| `>` | Lebih besar | `WHERE salary > 5000000` |
| `<` | Lebih kecil | `WHERE age < 30` |
| `>=` | Lebih besar sama dengan | `WHERE score >= 80` |
| `<=` | Lebih kecil sama dengan | `WHERE price <= 100000` |

### Operator Logika (AND, OR, NOT)
```sql
-- AND - Semua kondisi harus benar
SELECT * FROM employees 
WHERE department = 'IT' AND salary > 5000000;

-- OR - Salah satu kondisi benar
SELECT * FROM employees 
WHERE department = 'IT' OR department = 'HR';

-- NOT - Kebalikan kondisi
SELECT * FROM employees 
WHERE NOT department = 'IT';

-- Kombinasi AND dan OR (gunakan parentheses)
SELECT * FROM employees 
WHERE (department = 'IT' OR department = 'HR') AND salary > 5000000;
```

### BETWEEN - Range Nilai
```sql
-- Angka dalam range
SELECT * FROM employees 
WHERE salary BETWEEN 3000000 AND 7000000;

-- Tanggal dalam range
SELECT * FROM orders 
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';

-- NOT BETWEEN
SELECT * FROM products 
WHERE price NOT BETWEEN 10000 AND 50000;
```

### IN - Multiple Values
```sql
-- Nilai dalam list
SELECT * FROM employees 
WHERE department IN ('IT', 'HR', 'Finance');

-- NOT IN
SELECT * FROM employees 
WHERE department NOT IN ('IT', 'HR');

-- IN dengan subquery
SELECT * FROM employees 
WHERE department_id IN (SELECT id FROM departments WHERE location = 'Jakarta');
```

### LIKE - Pattern Matching
```sql
-- Dimulai dengan 'A'
SELECT * FROM employees WHERE first_name LIKE 'A%';

-- Diakhiri dengan 'son'
SELECT * FROM employees WHERE last_name LIKE '%son';

-- Mengandung 'mar'
SELECT * FROM employees WHERE first_name LIKE '%mar%';

-- Karakter tunggal dengan underscore (_)
SELECT * FROM employees WHERE first_name LIKE 'J_n';  -- Jon, Jan, Jin

-- Kombinasi
SELECT * FROM employees WHERE email LIKE '%@gmail.com';
```

| Pattern | Deskripsi |
|---------|-----------|
| `%` | Nol atau lebih karakter |
| `_` | Tepat satu karakter |
| `[abc]` | Karakter dalam bracket (SQL Server) |
| `[^abc]` | Bukan karakter dalam bracket (SQL Server) |

### IS NULL / IS NOT NULL
```sql
-- Cek nilai NULL
SELECT * FROM employees WHERE manager_id IS NULL;

-- Cek bukan NULL
SELECT * FROM employees WHERE phone_number IS NOT NULL;
```

---

## 3. Sorting & Limiting

### ORDER BY - Mengurutkan Data
```sql
-- Ascending (default, A-Z, kecil ke besar)
SELECT * FROM employees ORDER BY first_name ASC;

-- Descending (Z-A, besar ke kecil)
SELECT * FROM employees ORDER BY salary DESC;

-- Multiple columns
SELECT * FROM employees 
ORDER BY department ASC, salary DESC;

-- Order by column position
SELECT first_name, salary FROM employees 
ORDER BY 2 DESC;  -- Order by kolom ke-2 (salary)
```

### LIMIT / TOP - Membatasi Hasil
```sql
-- MySQL, PostgreSQL: LIMIT
SELECT * FROM employees ORDER BY salary DESC LIMIT 10;

-- SQL Server: TOP
SELECT TOP 10 * FROM employees ORDER BY salary DESC;

-- Oracle: FETCH FIRST
SELECT * FROM employees ORDER BY salary DESC FETCH FIRST 10 ROWS ONLY;

-- LIMIT dengan OFFSET (skip rows)
SELECT * FROM employees ORDER BY id LIMIT 10 OFFSET 20;  -- Skip 20, ambil 10
-- atau
SELECT * FROM employees ORDER BY id LIMIT 20, 10;  -- Skip 20, ambil 10 (MySQL)
```

---

## 4. Aggregate Functions

### Fungsi Agregat Dasar
```sql
-- COUNT - Menghitung jumlah baris
SELECT COUNT(*) FROM employees;                    -- Semua baris
SELECT COUNT(phone_number) FROM employees;         -- Baris yang tidak NULL
SELECT COUNT(DISTINCT department) FROM employees;  -- Nilai unik

-- SUM - Menjumlahkan nilai
SELECT SUM(salary) FROM employees;

-- AVG - Rata-rata
SELECT AVG(salary) FROM employees;

-- MAX - Nilai maksimum
SELECT MAX(salary) FROM employees;

-- MIN - Nilai minimum
SELECT MIN(salary) FROM employees;
```

### Kombinasi Aggregate
```sql
SELECT 
    COUNT(*) AS jumlah_karyawan,
    SUM(salary) AS total_gaji,
    AVG(salary) AS rata_rata_gaji,
    MAX(salary) AS gaji_tertinggi,
    MIN(salary) AS gaji_terendah
FROM employees;
```

---

## 5. GROUP BY & HAVING

### GROUP BY - Mengelompokkan Data
```sql
-- Jumlah karyawan per department
SELECT department, COUNT(*) AS jumlah 
FROM employees 
GROUP BY department;

-- Total gaji per department
SELECT department, SUM(salary) AS total_gaji 
FROM employees 
GROUP BY department;

-- Multiple grouping
SELECT department, job_title, COUNT(*) AS jumlah 
FROM employees 
GROUP BY department, job_title;

-- Dengan ORDER BY
SELECT department, AVG(salary) AS avg_salary 
FROM employees 
GROUP BY department 
ORDER BY avg_salary DESC;
```

### HAVING - Filter Setelah GROUP BY
```sql
-- Department dengan lebih dari 5 karyawan
SELECT department, COUNT(*) AS jumlah 
FROM employees 
GROUP BY department 
HAVING COUNT(*) > 5;

-- Department dengan rata-rata gaji > 5 juta
SELECT department, AVG(salary) AS avg_salary 
FROM employees 
GROUP BY department 
HAVING AVG(salary) > 5000000;

-- Kombinasi WHERE dan HAVING
SELECT department, COUNT(*) AS jumlah, AVG(salary) AS avg_salary 
FROM employees 
WHERE status = 'active'           -- Filter SEBELUM grouping
GROUP BY department 
HAVING COUNT(*) >= 3              -- Filter SETELAH grouping
ORDER BY avg_salary DESC;
```

> **Perbedaan WHERE vs HAVING:**
> - `WHERE` filter SEBELUM grouping (row-level)
> - `HAVING` filter SETELAH grouping (group-level)

---

## 6. JOIN Operations

### Tabel Contoh
```
employees                      departments
+----+----------+--------+     +----+-----------+
| id | name     | dept_id|     | id | dept_name |
+----+----------+--------+     +----+-----------+
| 1  | Alice    | 1      |     | 1  | IT        |
| 2  | Bob      | 2      |     | 2  | HR        |
| 3  | Charlie  | NULL   |     | 3  | Finance   |
| 4  | David    | 1      |     +----+-----------+
+----+----------+--------+
```

### INNER JOIN - Data yang Cocok di Kedua Tabel
```sql
SELECT e.name, d.dept_name 
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;

-- Hasil: Alice-IT, Bob-HR, David-IT (Charlie tidak muncul karena dept_id NULL)
```

### LEFT JOIN - Semua Data dari Tabel Kiri
```sql
SELECT e.name, d.dept_name 
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;

-- Hasil: Alice-IT, Bob-HR, Charlie-NULL, David-IT
```

### RIGHT JOIN - Semua Data dari Tabel Kanan
```sql
SELECT e.name, d.dept_name 
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;

-- Hasil: Alice-IT, David-IT, Bob-HR, NULL-Finance
```

### FULL OUTER JOIN - Semua Data dari Kedua Tabel
```sql
SELECT e.name, d.dept_name 
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.id;

-- Hasil: Semua kombinasi + NULL untuk yang tidak match
```

### CROSS JOIN - Setiap Kombinasi
```sql
SELECT e.name, d.dept_name 
FROM employees e
CROSS JOIN departments d;

-- Hasil: 4 employees × 3 departments = 12 rows
```

### SELF JOIN - Join dengan Tabel Sendiri
```sql
-- Mencari manager dari setiap karyawan
SELECT 
    e.name AS employee, 
    m.name AS manager 
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;
```

### Multiple JOINs
```sql
SELECT 
    e.name,
    d.dept_name,
    p.project_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id
INNER JOIN projects p ON e.id = p.employee_id;
```

### JOIN dengan Kondisi Tambahan
```sql
SELECT e.name, d.dept_name 
FROM employees e
INNER JOIN departments d 
    ON e.dept_id = d.id 
    AND d.is_active = 1;
```

---

## 7. Subqueries

### Subquery di WHERE
```sql
-- Karyawan dengan gaji di atas rata-rata
SELECT * FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Karyawan di department yang punya lebih dari 5 orang
SELECT * FROM employees 
WHERE department_id IN (
    SELECT department_id 
    FROM employees 
    GROUP BY department_id 
    HAVING COUNT(*) > 5
);
```

### Subquery di SELECT
```sql
SELECT 
    name,
    salary,
    (SELECT AVG(salary) FROM employees) AS avg_company_salary,
    salary - (SELECT AVG(salary) FROM employees) AS diff_from_avg
FROM employees;
```

### Subquery di FROM (Derived Table)
```sql
SELECT dept_name, avg_salary
FROM (
    SELECT 
        d.dept_name,
        AVG(e.salary) AS avg_salary
    FROM employees e
    JOIN departments d ON e.dept_id = d.id
    GROUP BY d.dept_name
) AS dept_stats
WHERE avg_salary > 5000000;
```

### EXISTS / NOT EXISTS
```sql
-- Karyawan yang punya order
SELECT * FROM employees e
WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.employee_id = e.id
);

-- Karyawan yang tidak punya order
SELECT * FROM employees e
WHERE NOT EXISTS (
    SELECT 1 FROM orders o WHERE o.employee_id = e.id
);
```

### ANY / ALL
```sql
-- Gaji lebih besar dari SALAH SATU gaji di IT
SELECT * FROM employees 
WHERE salary > ANY (
    SELECT salary FROM employees WHERE department = 'IT'
);

-- Gaji lebih besar dari SEMUA gaji di IT
SELECT * FROM employees 
WHERE salary > ALL (
    SELECT salary FROM employees WHERE department = 'IT'
);
```

---

## 8. UNION & Set Operations

### UNION - Menggabungkan Hasil (Tanpa Duplikat)
```sql
SELECT name, 'Employee' AS type FROM employees
UNION
SELECT name, 'Customer' AS type FROM customers;
```

### UNION ALL - Menggabungkan Hasil (Dengan Duplikat)
```sql
SELECT product_name FROM products_2023
UNION ALL
SELECT product_name FROM products_2024;
```

### INTERSECT - Data yang Ada di Kedua Query
```sql
SELECT customer_id FROM orders_2023
INTERSECT
SELECT customer_id FROM orders_2024;
-- Customer yang order di 2023 DAN 2024
```

### EXCEPT / MINUS - Data yang Hanya Ada di Query Pertama
```sql
SELECT customer_id FROM orders_2023
EXCEPT
SELECT customer_id FROM orders_2024;
-- Customer yang order di 2023 TAPI TIDAK di 2024
```

---

## 9. String Functions

### Fungsi String Umum
```sql
-- Panjang string
SELECT LENGTH('Hello');                    -- 5 (MySQL, PostgreSQL)
SELECT LEN('Hello');                       -- 5 (SQL Server)

-- Uppercase dan Lowercase
SELECT UPPER('hello');                     -- HELLO
SELECT LOWER('HELLO');                     -- hello

-- Substring
SELECT SUBSTRING('Hello World', 1, 5);     -- Hello
SELECT SUBSTR('Hello World', 7);           -- World

-- Concatenation
SELECT CONCAT('Hello', ' ', 'World');      -- Hello World
SELECT 'Hello' || ' ' || 'World';          -- Hello World (PostgreSQL, Oracle)
SELECT 'Hello' + ' ' + 'World';            -- Hello World (SQL Server)

-- Trim (hapus spasi)
SELECT TRIM('  Hello  ');                  -- Hello
SELECT LTRIM('  Hello');                   -- Hello (left trim)
SELECT RTRIM('Hello  ');                   -- Hello (right trim)

-- Replace
SELECT REPLACE('Hello World', 'World', 'SQL');  -- Hello SQL

-- Position
SELECT POSITION('o' IN 'Hello');           -- 5 (PostgreSQL)
SELECT CHARINDEX('o', 'Hello');            -- 5 (SQL Server)
SELECT INSTR('Hello', 'o');                -- 5 (MySQL, Oracle)

-- Left dan Right
SELECT LEFT('Hello', 3);                   -- Hel
SELECT RIGHT('Hello', 2);                  -- lo

-- Reverse
SELECT REVERSE('Hello');                   -- olleH

-- COALESCE - Nilai pertama yang tidak NULL
SELECT COALESCE(phone, email, 'No Contact') FROM customers;

-- NULLIF - Return NULL jika dua nilai sama
SELECT NULLIF(value1, value2);
```

---

## 10. Date Functions

### Mendapatkan Tanggal/Waktu Sekarang
```sql
-- Tanggal sekarang
SELECT CURRENT_DATE;                       -- PostgreSQL
SELECT CURDATE();                          -- MySQL
SELECT GETDATE();                          -- SQL Server
SELECT SYSDATE FROM dual;                  -- Oracle

-- Tanggal dan waktu
SELECT NOW();                              -- MySQL, PostgreSQL
SELECT CURRENT_TIMESTAMP;                  -- Standard SQL
SELECT GETDATE();                          -- SQL Server
```

### Ekstrak Bagian Tanggal
```sql
-- MySQL
SELECT YEAR('2024-03-15');                 -- 2024
SELECT MONTH('2024-03-15');                -- 3
SELECT DAY('2024-03-15');                  -- 15
SELECT DAYNAME('2024-03-15');              -- Friday
SELECT MONTHNAME('2024-03-15');            -- March

-- PostgreSQL
SELECT EXTRACT(YEAR FROM '2024-03-15'::date);
SELECT EXTRACT(MONTH FROM '2024-03-15'::date);
SELECT EXTRACT(DOW FROM '2024-03-15'::date);    -- Day of week (0=Sunday)

-- SQL Server
SELECT DATEPART(year, '2024-03-15');
SELECT DATEPART(month, '2024-03-15');
SELECT DATENAME(weekday, '2024-03-15');
```

### Manipulasi Tanggal
```sql
-- Tambah/Kurang Tanggal - MySQL
SELECT DATE_ADD('2024-03-15', INTERVAL 7 DAY);      -- +7 hari
SELECT DATE_SUB('2024-03-15', INTERVAL 1 MONTH);    -- -1 bulan

-- PostgreSQL
SELECT '2024-03-15'::date + INTERVAL '7 days';
SELECT '2024-03-15'::date - INTERVAL '1 month';

-- SQL Server
SELECT DATEADD(day, 7, '2024-03-15');
SELECT DATEADD(month, -1, '2024-03-15');

-- Selisih Tanggal
SELECT DATEDIFF('2024-03-15', '2024-03-01');        -- 14 (MySQL)
SELECT DATEDIFF(day, '2024-03-01', '2024-03-15');   -- 14 (SQL Server)
SELECT '2024-03-15'::date - '2024-03-01'::date;     -- 14 (PostgreSQL)
```

### Format Tanggal
```sql
-- MySQL
SELECT DATE_FORMAT('2024-03-15', '%d/%m/%Y');       -- 15/03/2024
SELECT DATE_FORMAT('2024-03-15', '%W, %d %M %Y');   -- Friday, 15 March 2024

-- PostgreSQL
SELECT TO_CHAR('2024-03-15'::date, 'DD/MM/YYYY');

-- SQL Server
SELECT FORMAT(GETDATE(), 'dd/MM/yyyy');
SELECT CONVERT(VARCHAR, GETDATE(), 103);            -- 15/03/2024
```

### Filter Berdasarkan Tanggal
```sql
-- Order bulan ini
SELECT * FROM orders 
WHERE MONTH(order_date) = MONTH(CURRENT_DATE) 
AND YEAR(order_date) = YEAR(CURRENT_DATE);

-- Order 7 hari terakhir
SELECT * FROM orders 
WHERE order_date >= DATE_SUB(CURRENT_DATE, INTERVAL 7 DAY);

-- Order tahun 2024
SELECT * FROM orders 
WHERE YEAR(order_date) = 2024;
-- atau
SELECT * FROM orders 
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';
```

---

## 11. Casting (Konversi Tipe Data)

### CAST - Konversi Standard SQL
```sql
-- String ke Integer
SELECT CAST('123' AS INT);
SELECT CAST('123' AS INTEGER);

-- Integer ke String
SELECT CAST(123 AS VARCHAR(10));
SELECT CAST(123 AS CHAR(5));

-- String ke Decimal
SELECT CAST('123.45' AS DECIMAL(10, 2));

-- String ke Date
SELECT CAST('2024-03-15' AS DATE);

-- String ke Timestamp/DateTime
SELECT CAST('2024-03-15 10:30:00' AS TIMESTAMP);
SELECT CAST('2024-03-15 10:30:00' AS DATETIME);  -- SQL Server, MySQL

-- Integer ke Boolean (PostgreSQL)
SELECT CAST(1 AS BOOLEAN);  -- true
SELECT CAST(0 AS BOOLEAN);  -- false
```

### CONVERT - Konversi dengan Format (SQL Server, MySQL)
```sql
-- SQL Server
SELECT CONVERT(VARCHAR, GETDATE(), 103);      -- dd/mm/yyyy
SELECT CONVERT(VARCHAR, GETDATE(), 108);      -- hh:mm:ss
SELECT CONVERT(INT, '123');
SELECT CONVERT(DECIMAL(10,2), '123.45');

-- MySQL
SELECT CONVERT('123', SIGNED);                -- String ke Integer
SELECT CONVERT('123.45', DECIMAL(10, 2));     -- String ke Decimal
SELECT CONVERT('2024-03-15', DATE);           -- String ke Date
```

### Type Casting Shorthand

#### PostgreSQL (::)
```sql
SELECT '123'::INTEGER;
SELECT 123::VARCHAR;
SELECT '2024-03-15'::DATE;
SELECT '123.45'::NUMERIC(10,2);
SELECT 'true'::BOOLEAN;
SELECT '{"name":"John"}'::JSON;
```

#### MySQL (Implicit atau CAST)
```sql
SELECT '123' + 0;                             -- String ke Integer (implicit)
SELECT 123 + '';                              -- Integer ke String (implicit)
SELECT '123' * 1;                             -- String ke Number
```

### Contoh Penggunaan Praktis
```sql
-- Menggabungkan angka dengan string
SELECT 'Total: ' || CAST(total_amount AS VARCHAR) FROM orders;
SELECT CONCAT('Total: ', CAST(total_amount AS VARCHAR)) FROM orders;

-- Konversi untuk kalkulasi
SELECT CAST(price AS DECIMAL(10,2)) * quantity AS total FROM order_items;

-- Konversi tanggal untuk filtering
SELECT * FROM orders 
WHERE CAST(order_date AS DATE) = '2024-03-15';

-- Konversi NULL dengan COALESCE
SELECT CAST(COALESCE(discount, 0) AS DECIMAL(10,2)) FROM orders;

-- Konversi untuk JOIN
SELECT * FROM orders o
JOIN customers c ON CAST(o.customer_id AS VARCHAR) = c.external_id;
```

### TRY_CAST / TRY_CONVERT (SQL Server) - Safe Conversion
```sql
-- Mengembalikan NULL jika konversi gagal (bukan error)
SELECT TRY_CAST('abc' AS INT);       -- NULL (tidak error)
SELECT TRY_CAST('123' AS INT);       -- 123

SELECT TRY_CONVERT(INT, 'abc');      -- NULL
SELECT TRY_CONVERT(DATE, '2024-13-45');  -- NULL (tanggal invalid)
```

### Tabel Tipe Data Umum
| Tipe Data | MySQL | PostgreSQL | SQL Server |
|-----------|-------|------------|------------|
| Integer | INT, SIGNED | INTEGER, INT | INT |
| Decimal | DECIMAL(p,s) | NUMERIC(p,s) | DECIMAL(p,s) |
| String | CHAR, VARCHAR | VARCHAR, TEXT | VARCHAR, NVARCHAR |
| Date | DATE | DATE | DATE |
| DateTime | DATETIME | TIMESTAMP | DATETIME |
| Boolean | TINYINT(1) | BOOLEAN | BIT |

---

## 12. Rounding (Pembulatan)

### ROUND - Pembulatan Standard
```sql
-- Pembulatan ke desimal terdekat
SELECT ROUND(123.456, 2);        -- 123.46 (2 desimal)
SELECT ROUND(123.456, 1);        -- 123.5 (1 desimal)
SELECT ROUND(123.456, 0);        -- 123 (tanpa desimal)

-- Pembulatan ke puluhan/ratusan
SELECT ROUND(1234.56, -1);       -- 1230 (pembulatan ke puluhan)
SELECT ROUND(1234.56, -2);       -- 1200 (pembulatan ke ratusan)
SELECT ROUND(1234.56, -3);       -- 1000 (pembulatan ke ribuan)

-- Pembulatan dengan mode (MySQL - opsional)
SELECT ROUND(2.5);               -- 2 atau 3 (tergantung database)
```

### FLOOR - Pembulatan ke Bawah
```sql
-- Selalu membulatkan ke bawah (menuju negatif tak hingga)
SELECT FLOOR(123.9);             -- 123
SELECT FLOOR(123.1);             -- 123
SELECT FLOOR(-123.1);            -- -124 (ke arah negatif)
SELECT FLOOR(-123.9);            -- -124
```

### CEIL / CEILING - Pembulatan ke Atas
```sql
-- Selalu membulatkan ke atas (menuju positif tak hingga)
SELECT CEIL(123.1);              -- 124
SELECT CEILING(123.9);           -- 124
SELECT CEIL(-123.1);             -- -123 (ke arah positif)
SELECT CEILING(-123.9);          -- -123
```

### TRUNCATE / TRUNC - Memotong Desimal (Tanpa Pembulatan)
```sql
-- MySQL: TRUNCATE
SELECT TRUNCATE(123.456, 2);     -- 123.45 (potong, bukan bulatkan)
SELECT TRUNCATE(123.999, 2);     -- 123.99
SELECT TRUNCATE(123.456, 0);     -- 123
SELECT TRUNCATE(1234, -2);       -- 1200

-- PostgreSQL, Oracle: TRUNC
SELECT TRUNC(123.456, 2);        -- 123.45
SELECT TRUNC(123.999);           -- 123 (default 0 desimal)

-- SQL Server: Gunakan ROUND dengan parameter ke-3
SELECT ROUND(123.456, 2, 1);     -- 123.45 (1 = truncate)
```

### Perbandingan Fungsi Pembulatan
| Input | ROUND | FLOOR | CEIL | TRUNCATE |
|-------|-------|-------|------|----------|
| 3.7 | 4 | 3 | 4 | 3 |
| 3.2 | 3 | 3 | 4 | 3 |
| -3.7 | -4 | -4 | -3 | -3 |
| -3.2 | -3 | -4 | -3 | -3 |

### ABS - Nilai Absolut
```sql
SELECT ABS(-123.45);             -- 123.45
SELECT ABS(123.45);              -- 123.45
```

### SIGN - Tanda Bilangan
```sql
SELECT SIGN(-50);                -- -1 (negatif)
SELECT SIGN(0);                  -- 0
SELECT SIGN(50);                 -- 1 (positif)
```

### MOD / % - Sisa Pembagian
```sql
-- Modulo
SELECT MOD(10, 3);               -- 1 (10 dibagi 3 sisa 1)
SELECT 10 % 3;                   -- 1 (alternatif di MySQL, SQL Server)

-- Contoh: Cek bilangan genap/ganjil
SELECT number,
    CASE WHEN MOD(number, 2) = 0 THEN 'Genap' ELSE 'Ganjil' END
FROM numbers;
```

### POWER dan SQRT
```sql
-- Pangkat
SELECT POWER(2, 3);              -- 8 (2^3)
SELECT POWER(10, 2);             -- 100 (10^2)

-- Akar kuadrat
SELECT SQRT(16);                 -- 4
SELECT SQRT(2);                  -- 1.414...
```

### Contoh Penggunaan Praktis
```sql
-- Format harga (2 desimal)
SELECT product_name, ROUND(price, 2) AS formatted_price FROM products;

-- Hitung diskon (bulatkan ke bawah)
SELECT order_total, FLOOR(order_total * 0.1) AS discount FROM orders;

-- Hitung halaman (bulatkan ke atas)
SELECT CEIL(COUNT(*) / 10.0) AS total_pages FROM products;

-- Hitung rata-rata dengan pembulatan
SELECT department, ROUND(AVG(salary), 0) AS avg_salary
FROM employees
GROUP BY department;

-- Pembulatan untuk laporan keuangan
SELECT 
    SUM(amount) AS total_exact,
    ROUND(SUM(amount), 2) AS total_rounded,
    FLOOR(SUM(amount)) AS total_floor,
    CEILING(SUM(amount)) AS total_ceiling
FROM transactions;

-- Konversi jam ke hari (bulatkan kebawah)
SELECT employee_id, FLOOR(total_hours / 8) AS total_days FROM timesheets;
```

---

## 13. CASE WHEN

### CASE WHEN Sederhana
```sql
SELECT 
    name,
    salary,
    CASE 
        WHEN salary >= 10000000 THEN 'High'
        WHEN salary >= 5000000 THEN 'Medium'
        ELSE 'Low'
    END AS salary_category
FROM employees;
```

### CASE WHEN dengan Agregat
```sql
SELECT 
    department,
    COUNT(CASE WHEN gender = 'M' THEN 1 END) AS male_count,
    COUNT(CASE WHEN gender = 'F' THEN 1 END) AS female_count
FROM employees
GROUP BY department;
```

### CASE WHEN di ORDER BY
```sql
SELECT * FROM employees
ORDER BY 
    CASE department
        WHEN 'Executive' THEN 1
        WHEN 'IT' THEN 2
        WHEN 'HR' THEN 3
        ELSE 4
    END;
```

### CASE WHEN untuk Update
```sql
UPDATE products
SET stock_status = 
    CASE 
        WHEN stock = 0 THEN 'Out of Stock'
        WHEN stock < 10 THEN 'Low Stock'
        ELSE 'In Stock'
    END;
```

### Nested CASE WHEN
```sql
SELECT 
    name,
    CASE 
        WHEN department = 'IT' THEN
            CASE 
                WHEN salary >= 10000000 THEN 'Senior IT'
                ELSE 'Junior IT'
            END
        WHEN department = 'HR' THEN 'HR Staff'
        ELSE 'Other'
    END AS role_category
FROM employees;
```

---

## 12. Window Functions

### ROW_NUMBER - Nomor Urut
```sql
-- Nomor urut per department berdasarkan gaji
SELECT 
    name,
    department,
    salary,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS rank_in_dept
FROM employees;
```

### RANK dan DENSE_RANK
```sql
SELECT 
    name,
    salary,
    RANK() OVER (ORDER BY salary DESC) AS rank,          -- Ada gap jika tie
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank  -- Tanpa gap
FROM employees;

-- Contoh perbedaan:
-- salary: 100, 100, 90
-- RANK:   1,   1,   3  (skip 2)
-- DENSE_RANK: 1, 1, 2  (tidak skip)
```

### NTILE - Bagi Menjadi N Kelompok
```sql
-- Bagi karyawan menjadi 4 quartile berdasarkan gaji
SELECT 
    name,
    salary,
    NTILE(4) OVER (ORDER BY salary DESC) AS salary_quartile
FROM employees;
```

### LAG dan LEAD - Nilai Baris Sebelum/Sesudah
```sql
SELECT 
    order_date,
    total_amount,
    LAG(total_amount, 1) OVER (ORDER BY order_date) AS prev_amount,
    LEAD(total_amount, 1) OVER (ORDER BY order_date) AS next_amount,
    total_amount - LAG(total_amount, 1) OVER (ORDER BY order_date) AS diff_from_prev
FROM orders;
```

### SUM, AVG, COUNT sebagai Window Function
```sql
SELECT 
    name,
    department,
    salary,
    SUM(salary) OVER (PARTITION BY department) AS dept_total,
    AVG(salary) OVER (PARTITION BY department) AS dept_avg,
    COUNT(*) OVER (PARTITION BY department) AS dept_count,
    salary * 100.0 / SUM(salary) OVER (PARTITION BY department) AS pct_of_dept
FROM employees;
```

### Running Total / Cumulative Sum
```sql
SELECT 
    order_date,
    total_amount,
    SUM(total_amount) OVER (ORDER BY order_date) AS running_total
FROM orders;

-- Running total per bulan
SELECT 
    order_date,
    total_amount,
    SUM(total_amount) OVER (
        PARTITION BY DATE_TRUNC('month', order_date) 
        ORDER BY order_date
    ) AS monthly_running_total
FROM orders;
```

### FIRST_VALUE dan LAST_VALUE
```sql
SELECT 
    name,
    department,
    salary,
    FIRST_VALUE(name) OVER (PARTITION BY department ORDER BY salary DESC) AS highest_paid,
    LAST_VALUE(name) OVER (
        PARTITION BY department 
        ORDER BY salary DESC 
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS lowest_paid
FROM employees;
```

---

## 13. Data Manipulation

### INSERT - Menambah Data
```sql
-- Insert satu baris
INSERT INTO employees (first_name, last_name, department, salary)
VALUES ('John', 'Doe', 'IT', 5000000);

-- Insert multiple baris
INSERT INTO employees (first_name, last_name, department, salary)
VALUES 
    ('John', 'Doe', 'IT', 5000000),
    ('Jane', 'Smith', 'HR', 4500000),
    ('Bob', 'Johnson', 'Finance', 5500000);

-- Insert dari SELECT
INSERT INTO employees_backup
SELECT * FROM employees WHERE department = 'IT';

-- Insert dengan DEFAULT
INSERT INTO employees (first_name, last_name)
VALUES ('John', 'Doe');  -- Kolom lain pakai DEFAULT
```

### UPDATE - Mengubah Data
```sql
-- Update satu kolom
UPDATE employees 
SET salary = 6000000 
WHERE id = 1;

-- Update multiple kolom
UPDATE employees 
SET salary = 6000000, department = 'Senior IT' 
WHERE id = 1;

-- Update dengan perhitungan
UPDATE employees 
SET salary = salary * 1.1  -- Naik 10%
WHERE department = 'IT';

-- Update dengan JOIN
UPDATE employees e
SET e.department_id = d.id
FROM departments d
WHERE e.department_name = d.name;

-- Update dengan subquery
UPDATE employees 
SET salary = (SELECT AVG(salary) FROM employees)
WHERE performance_rating < 3;
```

### DELETE - Menghapus Data
```sql
-- Hapus dengan kondisi
DELETE FROM employees WHERE id = 1;

-- Hapus dengan multiple kondisi
DELETE FROM employees 
WHERE department = 'IT' AND status = 'inactive';

-- Hapus dengan subquery
DELETE FROM employees 
WHERE department_id IN (
    SELECT id FROM departments WHERE is_closed = 1
);

-- Hapus semua data (HATI-HATI!)
DELETE FROM employees;

-- TRUNCATE - Hapus semua data (lebih cepat)
TRUNCATE TABLE employees;
```

### UPSERT - Insert atau Update
```sql
-- MySQL: ON DUPLICATE KEY UPDATE
INSERT INTO products (id, name, stock)
VALUES (1, 'Product A', 100)
ON DUPLICATE KEY UPDATE stock = stock + 100;

-- PostgreSQL: ON CONFLICT
INSERT INTO products (id, name, stock)
VALUES (1, 'Product A', 100)
ON CONFLICT (id) DO UPDATE SET stock = products.stock + 100;

-- SQL Server: MERGE
MERGE INTO products AS target
USING (SELECT 1 AS id, 'Product A' AS name, 100 AS stock) AS source
ON target.id = source.id
WHEN MATCHED THEN UPDATE SET stock = target.stock + source.stock
WHEN NOT MATCHED THEN INSERT (id, name, stock) VALUES (source.id, source.name, source.stock);
```

---

## 14. Table Operations

### CREATE TABLE
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    department VARCHAR(50) DEFAULT 'Unassigned',
    salary DECIMAL(15, 2),
    hire_date DATE,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Create table dari tabel lain
CREATE TABLE employees_backup AS
SELECT * FROM employees WHERE department = 'IT';

-- Create table dengan foreign key
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(15, 2),
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

### ALTER TABLE
```sql
-- Tambah kolom
ALTER TABLE employees ADD COLUMN phone VARCHAR(20);

-- Hapus kolom
ALTER TABLE employees DROP COLUMN phone;

-- Ubah tipe data kolom
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(18, 2);  -- MySQL
ALTER TABLE employees ALTER COLUMN salary TYPE DECIMAL(18, 2);  -- PostgreSQL

-- Rename kolom
ALTER TABLE employees RENAME COLUMN first_name TO fname;  -- PostgreSQL, MySQL 8+
EXEC sp_rename 'employees.first_name', 'fname', 'COLUMN';  -- SQL Server

-- Rename tabel
ALTER TABLE employees RENAME TO staff;
EXEC sp_rename 'employees', 'staff';  -- SQL Server

-- Tambah constraint
ALTER TABLE employees ADD CONSTRAINT chk_salary CHECK (salary >= 0);

-- Hapus constraint
ALTER TABLE employees DROP CONSTRAINT chk_salary;
```

### DROP TABLE
```sql
-- Hapus tabel
DROP TABLE employees;

-- Hapus tabel jika ada
DROP TABLE IF EXISTS employees;

-- Hapus multiple tabel
DROP TABLE table1, table2, table3;
```

---

## 15. Constraints

### Jenis Constraint
```sql
CREATE TABLE products (
    -- PRIMARY KEY - Unik dan Tidak NULL
    id INT PRIMARY KEY,
    
    -- UNIQUE - Nilai harus unik
    sku VARCHAR(50) UNIQUE,
    
    -- NOT NULL - Tidak boleh NULL
    name VARCHAR(100) NOT NULL,
    
    -- DEFAULT - Nilai default
    status VARCHAR(20) DEFAULT 'active',
    
    -- CHECK - Validasi nilai
    price DECIMAL(15, 2) CHECK (price >= 0),
    stock INT CHECK (stock >= 0),
    
    -- FOREIGN KEY - Referensi ke tabel lain
    category_id INT,
    FOREIGN KEY (category_id) REFERENCES categories(id)
        ON DELETE SET NULL
        ON UPDATE CASCADE
);

-- Composite Primary Key
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```

### Foreign Key Actions
| Action | ON DELETE | ON UPDATE |
|--------|-----------|-----------|
| `CASCADE` | Hapus child juga | Update child juga |
| `SET NULL` | Set child ke NULL | Set child ke NULL |
| `SET DEFAULT` | Set child ke default | Set child ke default |
| `RESTRICT` | Tolak hapus jika ada child | Tolak update jika ada child |
| `NO ACTION` | Sama dengan RESTRICT | Sama dengan RESTRICT |

---

## 16. Views

### Create View
```sql
-- View sederhana
CREATE VIEW active_employees AS
SELECT id, first_name, last_name, department, salary
FROM employees
WHERE status = 'active';

-- View dengan JOIN
CREATE VIEW employee_details AS
SELECT 
    e.id,
    e.first_name,
    e.last_name,
    d.dept_name,
    e.salary
FROM employees e
JOIN departments d ON e.dept_id = d.id;

-- View dengan agregasi
CREATE VIEW department_summary AS
SELECT 
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary,
    SUM(salary) AS total_salary
FROM employees
GROUP BY department;
```

### Menggunakan View
```sql
-- Query seperti tabel biasa
SELECT * FROM active_employees WHERE department = 'IT';

-- Join dengan view
SELECT v.*, o.order_count
FROM active_employees v
JOIN (
    SELECT employee_id, COUNT(*) AS order_count
    FROM orders
    GROUP BY employee_id
) o ON v.id = o.employee_id;
```

### Modify View
```sql
-- Update view
CREATE OR REPLACE VIEW active_employees AS
SELECT id, first_name, last_name, department, salary, hire_date
FROM employees
WHERE status = 'active';

-- Drop view
DROP VIEW active_employees;
DROP VIEW IF EXISTS active_employees;
```

---

## 17. Indexes

### Create Index
```sql
-- Index pada satu kolom
CREATE INDEX idx_employees_department ON employees(department);

-- Index pada multiple kolom (composite)
CREATE INDEX idx_employees_dept_salary ON employees(department, salary);

-- Unique index
CREATE UNIQUE INDEX idx_employees_email ON employees(email);

-- Index dengan kondisi (PostgreSQL)
CREATE INDEX idx_active_employees ON employees(department) WHERE status = 'active';
```

### Drop Index
```sql
DROP INDEX idx_employees_department ON employees;  -- MySQL
DROP INDEX idx_employees_department;  -- PostgreSQL
```

### Kapan Menggunakan Index?
- Kolom yang sering digunakan di WHERE
- Kolom yang sering digunakan di JOIN
- Kolom yang sering digunakan di ORDER BY
- Kolom dengan kardinalitas tinggi (banyak nilai unik)

---

## 18. Common Table Expressions (CTE)

### CTE Dasar
```sql
WITH high_earners AS (
    SELECT * FROM employees WHERE salary > 10000000
)
SELECT department, COUNT(*) AS count
FROM high_earners
GROUP BY department;
```

### Multiple CTEs
```sql
WITH 
dept_stats AS (
    SELECT 
        department,
        AVG(salary) AS avg_salary,
        COUNT(*) AS emp_count
    FROM employees
    GROUP BY department
),
high_avg_depts AS (
    SELECT department
    FROM dept_stats
    WHERE avg_salary > 7000000
)
SELECT e.*
FROM employees e
JOIN high_avg_depts h ON e.department = h.department;
```

### Recursive CTE (Untuk Hierarki)
```sql
-- Mencari semua subordinate dari seorang manager
WITH RECURSIVE subordinates AS (
    -- Base case: manager langsung
    SELECT id, name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id = 1
    
    UNION ALL
    
    -- Recursive case: subordinate dari subordinate
    SELECT e.id, e.name, e.manager_id, s.level + 1
    FROM employees e
    JOIN subordinates s ON e.manager_id = s.id
)
SELECT * FROM subordinates ORDER BY level;
```

---

## 19. Contoh Kasus Real-World

### 🛒 E-Commerce

#### Top 10 Produk Terlaris
```sql
SELECT 
    p.product_name,
    SUM(oi.quantity) AS total_sold,
    SUM(oi.quantity * oi.price) AS total_revenue
FROM order_items oi
JOIN products p ON oi.product_id = p.id
JOIN orders o ON oi.order_id = o.id
WHERE o.order_date >= DATE_SUB(CURRENT_DATE, INTERVAL 30 DAY)
GROUP BY p.id, p.product_name
ORDER BY total_sold DESC
LIMIT 10;
```

#### Customer yang Belum Order 30 Hari
```sql
SELECT c.*
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id 
    AND o.order_date >= DATE_SUB(CURRENT_DATE, INTERVAL 30 DAY)
WHERE o.id IS NULL;
```

#### Year-over-Year Growth
```sql
WITH monthly_sales AS (
    SELECT 
        DATE_FORMAT(order_date, '%Y-%m') AS month,
        SUM(total_amount) AS revenue
    FROM orders
    GROUP BY DATE_FORMAT(order_date, '%Y-%m')
)
SELECT 
    curr.month,
    curr.revenue AS current_revenue,
    prev.revenue AS prev_year_revenue,
    ROUND((curr.revenue - prev.revenue) / prev.revenue * 100, 2) AS yoy_growth_pct
FROM monthly_sales curr
LEFT JOIN monthly_sales prev 
    ON DATE_FORMAT(DATE_SUB(STR_TO_DATE(CONCAT(curr.month, '-01'), '%Y-%m-%d'), INTERVAL 1 YEAR), '%Y-%m') = prev.month
ORDER BY curr.month;
```

### 👥 HR / Karyawan

#### Karyawan dengan Gaji di Atas Rata-rata Department
```sql
SELECT e.*, d.avg_salary AS dept_avg_salary
FROM employees e
JOIN (
    SELECT department, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
) d ON e.department = d.department
WHERE e.salary > d.avg_salary
ORDER BY e.department, e.salary DESC;
```

#### Ranking Karyawan per Department
```sql
SELECT 
    name,
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank_in_dept,
    CASE 
        WHEN RANK() OVER (PARTITION BY department ORDER BY salary DESC) <= 3 
        THEN 'Top Performer' 
        ELSE 'Regular' 
    END AS performance_category
FROM employees;
```

#### Tenure Analysis
```sql
SELECT 
    name,
    hire_date,
    DATEDIFF(CURRENT_DATE, hire_date) AS days_employed,
    FLOOR(DATEDIFF(CURRENT_DATE, hire_date) / 365) AS years_employed,
    CASE 
        WHEN DATEDIFF(CURRENT_DATE, hire_date) < 365 THEN 'New Hire'
        WHEN DATEDIFF(CURRENT_DATE, hire_date) < 1095 THEN 'Mid-Level'
        ELSE 'Senior'
    END AS tenure_category
FROM employees
ORDER BY days_employed DESC;
```

### 📊 Analytics

#### Cohort Analysis (Retention)
```sql
WITH first_orders AS (
    SELECT 
        customer_id,
        DATE_FORMAT(MIN(order_date), '%Y-%m') AS cohort_month
    FROM orders
    GROUP BY customer_id
),
monthly_activity AS (
    SELECT 
        o.customer_id,
        f.cohort_month,
        DATE_FORMAT(o.order_date, '%Y-%m') AS activity_month,
        PERIOD_DIFF(
            DATE_FORMAT(o.order_date, '%Y%m'),
            DATE_FORMAT(STR_TO_DATE(CONCAT(f.cohort_month, '-01'), '%Y-%m-%d'), '%Y%m')
        ) AS month_number
    FROM orders o
    JOIN first_orders f ON o.customer_id = f.customer_id
)
SELECT 
    cohort_month,
    month_number,
    COUNT(DISTINCT customer_id) AS customer_count
FROM monthly_activity
GROUP BY cohort_month, month_number
ORDER BY cohort_month, month_number;
```

#### Moving Average (7 Hari)
```sql
SELECT 
    order_date,
    daily_revenue,
    AVG(daily_revenue) OVER (
        ORDER BY order_date 
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS moving_avg_7_days
FROM (
    SELECT 
        order_date,
        SUM(total_amount) AS daily_revenue
    FROM orders
    GROUP BY order_date
) daily_sales
ORDER BY order_date;
```

#### Percentile (Top 10% Customers)
```sql
WITH customer_spending AS (
    SELECT 
        customer_id,
        SUM(total_amount) AS total_spent,
        NTILE(10) OVER (ORDER BY SUM(total_amount) DESC) AS spending_decile
    FROM orders
    GROUP BY customer_id
)
SELECT c.*, cs.total_spent
FROM customers c
JOIN customer_spending cs ON c.id = cs.customer_id
WHERE cs.spending_decile = 1;  -- Top 10%
```

### 🔍 Deteksi Anomali

#### Transaksi Mencurigakan
```sql
WITH customer_stats AS (
    SELECT 
        customer_id,
        AVG(total_amount) AS avg_order,
        STDDEV(total_amount) AS stddev_order
    FROM orders
    GROUP BY customer_id
)
SELECT o.*, cs.avg_order, cs.stddev_order
FROM orders o
JOIN customer_stats cs ON o.customer_id = cs.customer_id
WHERE o.total_amount > cs.avg_order + (3 * cs.stddev_order);  -- 3 std deviations
```

#### Duplikat Data
```sql
-- Menemukan duplikat
SELECT email, COUNT(*) AS count
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;

-- Menampilkan semua row duplikat
SELECT *
FROM customers
WHERE email IN (
    SELECT email
    FROM customers
    GROUP BY email
    HAVING COUNT(*) > 1
)
ORDER BY email;

-- Hapus duplikat, simpan ID terkecil
DELETE FROM customers
WHERE id NOT IN (
    SELECT MIN(id)
    FROM customers
    GROUP BY email
);
```

---

## 📝 Tips Tes SQL

### 1. Urutan Eksekusi Query
```
FROM → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

### 2. Optimasi Query
- Gunakan `WHERE` untuk filter sebelum `JOIN` jika memungkinkan
- Hindari `SELECT *`, pilih kolom yang dibutuhkan saja
- Gunakan `EXPLAIN` untuk melihat query plan
- Buat index pada kolom yang sering di-filter atau di-join

### 3. Common Mistakes
- `NULL` tidak bisa dibandingkan dengan `=`, gunakan `IS NULL`
- `GROUP BY` harus mencakup semua non-aggregate columns di `SELECT`
- `HAVING` hanya untuk kondisi aggregate, bukan row-level

### 4. Shortcut Penting
```sql
-- Menghitung persentase
column_value * 100.0 / SUM(column_value) OVER () AS percentage

-- Menemukan nilai ke-N tertinggi
SELECT * FROM employees 
ORDER BY salary DESC 
LIMIT 1 OFFSET N-1;  -- Untuk nilai ke-N

-- Menemukan median
SELECT AVG(salary) AS median
FROM (
    SELECT salary
    FROM employees
    ORDER BY salary
    LIMIT 2 - (SELECT COUNT(*) FROM employees) % 2
    OFFSET (SELECT (COUNT(*) - 1) / 2 FROM employees)
) AS median_values;
```

---

## 📋 Quick Reference Table

| Kondisi | Query |
|---------|-------|
| Ambil semua data | `SELECT * FROM table` |
| Filter nilai tertentu | `WHERE column = 'value'` |
| Filter range | `WHERE column BETWEEN a AND b` |
| Filter multiple values | `WHERE column IN ('a', 'b', 'c')` |
| Filter pattern | `WHERE column LIKE '%pattern%'` |
| Filter NULL | `WHERE column IS NULL` |
| Gabungkan kondisi | `WHERE cond1 AND cond2 OR cond3` |
| Mengurutkan | `ORDER BY column ASC/DESC` |
| Membatasi hasil | `LIMIT n` atau `TOP n` |
| Menghitung jumlah | `COUNT(*)` atau `COUNT(column)` |
| Menjumlahkan | `SUM(column)` |
| Rata-rata | `AVG(column)` |
| Nilai maksimum | `MAX(column)` |
| Nilai minimum | `MIN(column)` |
| Mengelompokkan | `GROUP BY column` |
| Filter grup | `HAVING COUNT(*) > n` |
| Data matching di 2 tabel | `INNER JOIN table2 ON condition` |
| Semua dari tabel kiri | `LEFT JOIN table2 ON condition` |
| Subquery di WHERE | `WHERE col IN (SELECT ...)` |
| Kondisional | `CASE WHEN cond THEN x ELSE y END` |
| Ranking | `ROW_NUMBER() OVER (ORDER BY col)` |
| Ranking per grup | `ROW_NUMBER() OVER (PARTITION BY grp ORDER BY col)` |
| Running total | `SUM(col) OVER (ORDER BY col2)` |

---

**Selamat mengerjakan tes SQL! 🍀**
