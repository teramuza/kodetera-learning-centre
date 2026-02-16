# Modul 1.2: Pengenalan TypeScript

## 🎯 Tujuan Pembelajaran

Setelah modul ini, peserta dapat:
- Memahami mengapa TypeScript penting untuk development
- Menggunakan types dan interfaces
- Memahami type inference
- Menggunakan generics untuk reusable code
- Migrasi kode JavaScript ke TypeScript

## ⏱️ Durasi: 2-3 Sesi

---

## 📚 Materi 2.1: Mengapa TypeScript?

### Penjelasan untuk Instruktur

**Konsep Penting:**
- TypeScript adalah JavaScript dengan type system
- TypeScript membantu catch errors sebelum runtime
- TypeScript membuat code lebih maintainable
- TypeScript compile ke JavaScript (browser/Node.js tetap jalan JavaScript)
- TypeScript memberikan autocomplete yang lebih baik di IDE

### Materi Pembelajaran

#### 1. Masalah dengan JavaScript Murni

```javascript
// JavaScript - tidak ada error sampai runtime
function add(a, b) {
  return a + b;
}

add(5, 3);        // ✅ 8
add("5", "3");    // ❌ "53" (bug! tapi tidak error)
add(5);           // ❌ NaN (bug! tapi tidak error)
```

**Masalah:**
- Tidak ada warning saat development
- Error baru ketahuan saat aplikasi jalan
- Sulit untuk maintain code yang besar

#### 2. Solusi dengan TypeScript

```typescript
// TypeScript - error langsung terlihat
function add(a: number, b: number): number {
  return a + b;
}

add(5, 3);        // ✅ 8
add("5", "3");    // ❌ Error: Argument of type 'string' is not assignable to parameter of type 'number'
add(5);           // ❌ Error: Expected 2 arguments, but got 1
```

**Keuntungan:**
- Error terdeteksi saat menulis code (compile time)
- IDE memberikan autocomplete yang lebih baik
- Code lebih mudah dipahami dan di-maintain
- Refactoring lebih aman

#### 3. TypeScript = JavaScript + Types

```typescript
// JavaScript
let name = "John";
let age = 25;

// TypeScript (dengan type annotation)
let name: string = "John";
let age: number = 25;

// TypeScript (dengan type inference - lebih umum)
let name = "John";  // TypeScript tahu ini string
let age = 25;       // TypeScript tahu ini number
```

**Poin Penting:**
- TypeScript bisa menebak type secara otomatis (type inference)
- Tidak perlu selalu menulis type secara eksplisit
- Type annotation membantu untuk clarity dan catch errors

---

## 📚 Materi 2.2: Basic Types

### Penjelasan untuk Instruktur

**Konsep Penting:**
- TypeScript memiliki types untuk semua tipe data JavaScript
- Primitif: `string`, `number`, `boolean`, `null`, `undefined`
- Khusus: `any`, `void`, `never`
- Object types dan array types

### Materi Pembelajaran

#### 1. Primitif Types

```typescript
// String
let name: string = "John";
let message: string = `Hello, ${name}!`;

// Number
let age: number = 25;
let price: number = 99.99;

// Boolean
let isActive: boolean = true;
let isComplete: boolean = false;

// Null dan Undefined
let data: null = null;
let value: undefined = undefined;
```

#### 2. Array Types

```typescript
// Array of numbers
let numbers: number[] = [1, 2, 3, 4, 5];
let fruits: string[] = ["apple", "banana"];

// Array dengan generic syntax
let numbers2: Array<number> = [1, 2, 3];
let fruits2: Array<string> = ["apple", "banana"];

// Mixed array (tidak disarankan, gunakan union type)
let mixed: (string | number)[] = ["hello", 42];
```

#### 3. Object Types

```typescript
// Object dengan type annotation
let user: {
  name: string;
  age: number;
  email: string;
} = {
  name: "John",
  age: 25,
  email: "john@example.com"
};

// Object dengan optional property
let user2: {
  name: string;
  age: number;
  email?: string;  // Optional (bisa ada atau tidak)
} = {
  name: "John",
  age: 25
  // email tidak wajib
};
```

#### 4. Special Types

```typescript
// any - bisa apa saja (hindari jika mungkin)
let anything: any = "hello";
anything = 42;
anything = true;

// void - untuk function yang tidak return apa-apa
function logMessage(message: string): void {
  console.log(message);
  // Tidak ada return statement
}

// never - untuk function yang tidak pernah selesai
function throwError(message: string): never {
  throw new Error(message);
  // Function ini tidak pernah return
}
```

#### 5. Union Types

```typescript
// Bisa string atau number
let id: string | number = "123";
id = 123;  // ✅ Boleh

// Bisa beberapa types
let status: "pending" | "approved" | "rejected" = "pending";
status = "approved";  // ✅
// status = "invalid";  // ❌ Error
```

### Latihan 2.2

```typescript
// Buat type annotations untuk:
// 1. Variabel untuk menyimpan nama produk (string)
// 2. Variabel untuk menyimpan harga (number)
// 3. Array untuk menyimpan ID produk (number[])
// 4. Object untuk menyimpan informasi produk dengan properties:
//    - id (number)
//    - name (string)
//    - price (number)
//    - inStock (boolean)
//    - category (optional string)

// Tulis kode di bawah ini:
```

---

## 📚 Materi 2.3: Functions dengan Types

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Function parameters harus ada type-nya
- Return type bisa di-spesifikasikan
- Optional parameters dengan `?`
- Default parameters
- Function overloads (advanced)

### Materi Pembelajaran

#### 1. Function dengan Type Annotations

```typescript
// Function dengan parameter types dan return type
function add(a: number, b: number): number {
  return a + b;
}

// Arrow function dengan types
const multiply = (a: number, b: number): number => {
  return a * b;
};

// Arrow function satu baris
const subtract = (a: number, b: number): number => a - b;
```

#### 2. Optional Parameters

```typescript
// Parameter optional dengan ?
function greet(name: string, title?: string): string {
  if (title) {
    return `Hello, ${title} ${name}!`;
  }
  return `Hello, ${name}!`;
}

greet("John");              // "Hello, John!"
greet("John", "Mr");        // "Hello, Mr John!"
```

#### 3. Default Parameters

```typescript
// Default parameter value
function createUser(
  name: string,
  age: number = 18,
  isActive: boolean = true
): object {
  return { name, age, isActive };
}

createUser("John");                    // { name: "John", age: 18, isActive: true }
createUser("Jane", 25);                // { name: "Jane", age: 25, isActive: true }
createUser("Bob", 30, false);          // { name: "Bob", age: 30, isActive: false }
```

#### 4. Function sebagai Parameter

```typescript
// Function sebagai parameter (callback)
function processNumbers(
  numbers: number[],
  callback: (num: number) => number
): number[] {
  return numbers.map(callback);
}

// Menggunakan function tersebut
const doubled = processNumbers([1, 2, 3], (num) => num * 2);
console.log(doubled); // [2, 4, 6]
```

### Latihan 2.3

```typescript
// Buat fungsi-fungsi berikut dengan type annotations:
// 1. Fungsi untuk menghitung total harga (array of numbers) -> number
// 2. Fungsi untuk mencari user by id (users: array, id: number) -> user object atau null
// 3. Fungsi untuk filter products by category (products: array, category: string) -> array
// 4. Fungsi dengan optional parameter untuk format nama (name: string, title?: string) -> string

// Tulis kode di bawah ini:
```

---

## 📚 Materi 2.4: Interfaces

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Interface mendefinisikan struktur object
- Interface bisa digunakan untuk type checking
- Interface bisa di-extend (inheritance)
- Interface sangat penting untuk REST API (request/response types)

### Materi Pembelajaran

#### 1. Basic Interface

```typescript
// Mendefinisikan interface
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}

// Menggunakan interface sebagai type
const user: User = {
  id: 1,
  name: "John Doe",
  email: "john@example.com",
  age: 25
};

// Function dengan interface
function getUser(id: number): User {
  return {
    id,
    name: "John Doe",
    email: "john@example.com",
    age: 25
  };
}
```

#### 2. Optional Properties

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  description?: string;  // Optional
  inStock?: boolean;     // Optional
}

const product1: Product = {
  id: 1,
  name: "Laptop",
  price: 1000
  // description dan inStock tidak wajib
};

const product2: Product = {
  id: 2,
  name: "Phone",
  price: 500,
  description: "Smartphone",
  inStock: true
};
```

#### 3. Readonly Properties

```typescript
interface User {
  readonly id: number;  // Tidak bisa diubah setelah dibuat
  name: string;
  email: string;
}

const user: User = {
  id: 1,
  name: "John",
  email: "john@example.com"
};

// user.id = 2;  // ❌ Error: Cannot assign to 'id' because it is a read-only property
user.name = "Jane";  // ✅ Boleh
```

#### 4. Extending Interfaces

```typescript
// Base interface
interface Person {
  name: string;
  age: number;
}

// Extended interface
interface Employee extends Person {
  employeeId: number;
  department: string;
}

const employee: Employee = {
  name: "John",
  age: 25,
  employeeId: 123,
  department: "Engineering"
};
```

#### 5. Interface untuk Function

```typescript
// Interface untuk function signature
interface MathOperation {
  (a: number, b: number): number;
}

const add: MathOperation = (a, b) => a + b;
const multiply: MathOperation = (a, b) => a * b;
```

### Latihan 2.4

```typescript
// Buat interfaces berikut:
// 1. Interface Order dengan properties:
//    - id (number, readonly)
//    - customerName (string)
//    - total (number)
//    - status ("pending" | "completed" | "cancelled")
//    - createdAt (Date, optional)
//
// 2. Interface OrderItem extends dari interface Product (buat dulu Product interface)
//    - quantity (number)
//
// 3. Interface untuk function calculateTotal yang menerima array OrderItem dan return number

// Tulis kode di bawah ini:
```

---

## 📚 Materi 2.5: Generics

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Generics membuat code reusable untuk berbagai types
- Generics sangat berguna untuk functions, interfaces, dan classes
- Syntax: `<T>` dimana T adalah type parameter
- Bisa digunakan untuk membuat utility functions yang flexible

### Materi Pembelajaran

#### 1. Generic Functions

```typescript
// Function tanpa generic (hanya untuk number)
function getFirstNumber(arr: number[]): number {
  return arr[0];
}

// Function dengan generic (bisa untuk berbagai types)
function getFirst<T>(arr: T[]): T {
  return arr[0];
}

// Menggunakan generic function
const firstNumber = getFirst<number>([1, 2, 3]);        // number
const firstString = getFirst<string>(["a", "b", "c"]);  // string
const firstUser = getFirst<User>([user1, user2]);        // User

// TypeScript bisa infer type-nya
const first = getFirst([1, 2, 3]);  // TypeScript tahu ini number
```

#### 2. Multiple Generic Types

```typescript
// Function dengan multiple generics
function merge<T, U>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}

const merged = merge(
  { name: "John" },
  { age: 25 }
);
// merged: { name: string; age: number }
```

#### 3. Generic Constraints

```typescript
// Generic dengan constraint (harus punya property length)
function getLength<T extends { length: number }>(item: T): number {
  return item.length;
}

getLength("hello");     // ✅ string punya length
getLength([1, 2, 3]);   // ✅ array punya length
// getLength(123);      // ❌ number tidak punya length
```

#### 4. Generic Interfaces

```typescript
// Interface dengan generic
interface Repository<T> {
  findById(id: number): T | null;
  findAll(): T[];
  save(entity: T): T;
}

// Menggunakan generic interface
interface User {
  id: number;
  name: string;
}

class UserRepository implements Repository<User> {
  findById(id: number): User | null {
    // Implementation
    return null;
  }
  
  findAll(): User[] {
    return [];
  }
  
  save(entity: User): User {
    return entity;
  }
}
```

### Latihan 2.5

```typescript
// Buat generic functions berikut:
// 1. Function untuk mendapatkan elemen terakhir dari array (generic)
// 2. Function untuk filter array berdasarkan callback (generic dengan constraint)
// 3. Generic interface untuk API Response dengan structure:
//    - data: T
//    - status: number
//    - message: string

// Tulis kode di bawah ini:
```

---

## 📚 Materi 2.6: Type Inference dan Type Assertions

### Penjelasan untuk Instruktur

**Konsep Penting:**
- TypeScript bisa menebak types secara otomatis
- Kadang kita perlu memberi tahu TypeScript type yang benar (type assertion)
- `as` keyword untuk type assertion
- Berguna saat bekerja dengan API responses atau DOM manipulation

### Materi Pembelajaran

#### 1. Type Inference

```typescript
// TypeScript menebak type secara otomatis
let name = "John";        // Type: string
let age = 25;             // Type: number
let isActive = true;      // Type: boolean

// Array inference
let numbers = [1, 2, 3];  // Type: number[]
let mixed = [1, "hello"]; // Type: (string | number)[]

// Function return type inference
function add(a: number, b: number) {
  return a + b;  // TypeScript tahu return type adalah number
}
```

#### 2. Type Assertions

```typescript
// Kadang kita tahu lebih baik daripada TypeScript
let value: unknown = "hello";

// Type assertion dengan 'as'
let str = value as string;
let num = value as number;  // ⚠️ Hati-hati! Bisa salah

// Type assertion dengan angle bracket (kurang umum)
let str2 = <string>value;
```

#### 3. Contoh Real-World: API Response

```typescript
// Saat fetch dari API, response biasanya unknown
async function fetchUser(id: number) {
  const response = await fetch(`/api/users/${id}`);
  const data = await response.json();  // Type: any
  
  // Assert ke type yang kita tahu
  return data as User;
}

// Atau lebih aman dengan type guard
function isUser(obj: any): obj is User {
  return obj && 
         typeof obj.id === 'number' &&
         typeof obj.name === 'string' &&
         typeof obj.email === 'string';
}

async function fetchUserSafe(id: number): Promise<User | null> {
  const response = await fetch(`/api/users/${id}`);
  const data = await response.json();
  
  if (isUser(data)) {
    return data;
  }
  return null;
}
```

### Latihan 2.6

```typescript
// 1. Buat function fetchProduct yang return Promise<Product>
//    Gunakan type assertion untuk response JSON
// 2. Buat type guard function isProduct untuk validasi
// 3. Buat function fetchProductSafe yang menggunakan type guard

// Tulis kode di bawah ini:
```

---

## 📚 Materi 2.7: Migrasi JavaScript ke TypeScript

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Migrasi bisa dilakukan bertahap
- Mulai dengan menambahkan type annotations
- Gunakan `any` sementara jika perlu, lalu perbaiki bertahap
- File `.ts` bisa import dari `.js` (dengan konfigurasi tertentu)

### Materi Pembelajaran

#### 1. Langkah Migrasi

**Step 1: Rename file .js ke .ts**
```typescript
// Sebelum: utils.js
export const add = (a, b) => a + b;

// Sesudah: utils.ts
export const add = (a: number, b: number): number => a + b;
```

**Step 2: Tambahkan type annotations**
```typescript
// Sebelum
function getUser(id) {
  return { id, name: "John" };
}

// Sesudah
interface User {
  id: number;
  name: string;
}

function getUser(id: number): User {
  return { id, name: "John" };
}
```

**Step 3: Fix errors bertahap**
```typescript
// Gunakan 'any' sementara jika perlu
function processData(data: any) {
  // TODO: Add proper types
  return data;
}

// Lalu perbaiki bertahap
interface ProcessedData {
  // ...
}

function processData(data: unknown): ProcessedData {
  // Proper implementation
}
```

#### 2. Contoh Migrasi Lengkap

**File JavaScript (sebelum):**
```javascript
// userService.js
export function getUserById(id) {
  return { id, name: "John", email: "john@example.com" };
}

export function getAllUsers() {
  return [
    { id: 1, name: "John", email: "john@example.com" },
    { id: 2, name: "Jane", email: "jane@example.com" }
  ];
}

export function createUser(userData) {
  return { ...userData, id: Date.now() };
}
```

**File TypeScript (sesudah):**
```typescript
// userService.ts
interface User {
  id: number;
  name: string;
  email: string;
}

interface CreateUserData {
  name: string;
  email: string;
}

export function getUserById(id: number): User {
  return { id, name: "John", email: "john@example.com" };
}

export function getAllUsers(): User[] {
  return [
    { id: 1, name: "John", email: "john@example.com" },
    { id: 2, name: "Jane", email: "jane@example.com" }
  ];
}

export function createUser(userData: CreateUserData): User {
  return { ...userData, id: Date.now() };
}
```

### Latihan 2.7

```typescript
// Migrasikan kode JavaScript berikut ke TypeScript:
// 
// productService.js
// export function getProduct(id) {
//   return { id, name: "Laptop", price: 1000 };
// }
//
// export function calculateTotal(products) {
//   return products.reduce((sum, product) => sum + product.price, 0);
// }
//
// export function filterByPrice(products, maxPrice) {
//   return products.filter(product => product.price <= maxPrice);
// }

// Tulis kode TypeScript di bawah ini:
```

---

## ✅ Review Modul 1.2

### Checklist Pemahaman

Sebelum lanjut ke modul berikutnya, pastikan peserta memahami:

- [ ] Mengapa TypeScript penting untuk development
- [ ] Bisa menggunakan basic types (string, number, boolean, array, object)
- [ ] Bisa membuat function dengan type annotations
- [ ] Bisa membuat dan menggunakan interfaces
- [ ] Memahami konsep generics (meskipun belum expert)
- [ ] Bisa migrasi kode JavaScript sederhana ke TypeScript

### Latihan Akhir Modul

Migrasikan aplikasi dari Modul 1 ke TypeScript:

```
project/
├── utils/
│   ├── math.ts          // Dengan type annotations
│   └── string.ts        // Dengan type annotations
├── data/
│   └── users.ts         // Dengan interface User
├── services/
│   └── userService.ts   // Dengan proper types
└── app.ts               // TypeScript version
```

**Requirements:**
1. Buat interface `User` dengan properties yang sesuai
2. Semua function harus punya type annotations
3. Semua array harus punya type (User[], number[], dll)
4. Gunakan generics jika diperlukan
5. Tidak ada penggunaan `any` (kecuali benar-benar diperlukan)

---

## 🎯 Persiapan untuk Modul Berikutnya

Modul berikutnya (1.3) akan memperkenalkan Git dan GitHub. Pastikan peserta sudah nyaman dengan:
- ✅ TypeScript basics (types, interfaces)
- ✅ Function dengan type annotations
- ✅ Async/await (dari Modul 1)
- ✅ Modules import/export (dari Modul 1)

**Catatan Instruktur:** 
- Jika peserta masih kesulitan dengan TypeScript, jangan terlalu lama di sini
- Yang penting mereka paham konsep types dan interfaces
- Generics bisa dipelajari lebih dalam nanti saat praktik
- Setelah Git, peserta akan belajar Express.js dan bisa menggunakan Git untuk track semua perubahan
