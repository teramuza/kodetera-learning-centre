# Modul 1.1: JavaScript Dasar

## 🎯 Tujuan Pembelajaran

Setelah modul ini, peserta dapat:
- Memahami variabel dan tipe data JavaScript
- Menggunakan fungsi dan arrow functions
- Memanipulasi array dengan berbagai methods
- Bekerja dengan object dan destructuring
- Menangani asynchronous code dengan async/await
- Menggunakan modules untuk mengorganisir kode

## ⏱️ Durasi: 2-3 Sesi

---

## 📚 Materi 1.1: Variabel dan Tipe Data

### Penjelasan untuk Instruktur

**Konsep Penting:**
- JavaScript memiliki 3 cara deklarasi variabel: `var`, `let`, `const`
- Fokus pada `let` dan `const` (ES6+)
- JavaScript adalah dynamically typed language
- Tipe data primitif: string, number, boolean, null, undefined, symbol

### Materi Pembelajaran

#### 1. Deklarasi Variabel

```javascript
// ❌ Jangan gunakan var (old way)
var namaLama = "Ini cara lama";

// ✅ Gunakan let untuk variabel yang bisa diubah
let nama = "John";
nama = "Jane"; // Bisa diubah

// ✅ Gunakan const untuk nilai yang tidak berubah
const umur = 25;
// umur = 26; // Error! const tidak bisa diubah
```

**Poin Penting:**
- `let`: untuk variabel yang nilainya bisa berubah
- `const`: untuk nilai yang tetap (lebih aman, gunakan sebanyak mungkin)
- `var`: hindari, ada masalah dengan scope

#### 2. Tipe Data Primitif

```javascript
// String
const nama = "John Doe";
const pesan = 'Hello World';
const template = `Halo, ${nama}!`; // Template literal

// Number
const umur = 25;
const harga = 99.99;
const jumlah = 1000;

// Boolean
const isActive = true;
const isComplete = false;

// Null dan Undefined
const data = null;        // Sengaja dikosongkan
let belumDiisi;          // undefined - belum diberi nilai
```

#### 3. Tipe Data Object dan Array

```javascript
// Object
const user = {
  nama: "John",
  umur: 25,
  email: "john@example.com"
};

// Array
const fruits = ["apple", "banana", "orange"];
const numbers = [1, 2, 3, 4, 5];
```

### Latihan 1.1

```javascript
// Buat variabel dengan tipe data berikut:
// 1. Nama lengkap (string)
// 2. Umur (number)
// 3. Status aktif (boolean)
// 4. Array berisi 3 hobi
// 5. Object berisi informasi diri (nama, umur, email)

// Tulis kode di bawah ini:
```

---

## 📚 Materi 1.2: Fungsi dan Arrow Functions

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Function adalah blok kode yang bisa dipanggil berulang kali
- Arrow function adalah sintaks modern ES6+
- Function bisa menerima parameter dan mengembalikan nilai
- Function adalah first-class citizen di JavaScript

### Materi Pembelajaran

#### 1. Function Declaration

```javascript
// Function biasa
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet("John")); // "Hello, John!"
```

#### 2. Arrow Function

```javascript
// Arrow function (cara modern)
const greet = (name) => {
  return `Hello, ${name}!`;
};

// Arrow function dengan satu baris (implisit return)
const greetShort = (name) => `Hello, ${name}!`;

// Arrow function dengan satu parameter (bisa tanpa kurung)
const greetShorter = name => `Hello, ${name}!`;

console.log(greet("John")); // "Hello, John!"
```

#### 3. Function dengan Multiple Parameters

```javascript
// Function biasa
function add(a, b) {
  return a + b;
}

// Arrow function
const add = (a, b) => a + b;

// Function tanpa parameter
const getCurrentTime = () => new Date().toISOString();

// Function dengan default parameter
const greet = (name = "Guest") => `Hello, ${name}!`;

console.log(greet());        // "Hello, Guest!"
console.log(greet("John"));  // "Hello, John!"
```

### Latihan 1.2

```javascript
// Buat fungsi-fungsi berikut menggunakan arrow function:
// 1. Fungsi untuk menghitung luas persegi panjang (panjang x lebar)
// 2. Fungsi untuk mengecek apakah angka genap atau ganjil
// 3. Fungsi untuk menggabungkan dua string dengan spasi di antaranya
// 4. Fungsi untuk mendapatkan elemen pertama dari array

// Tulis kode di bawah ini:
```

---

## 📚 Materi 1.3: Array Methods

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Array methods sangat penting untuk REST API (filtering, mapping data)
- Methods yang paling sering digunakan: `map`, `filter`, `find`, `forEach`, `reduce`
- Methods ini tidak mengubah array asli (immutable), kecuali `forEach`

### Materi Pembelajaran

#### 1. forEach - Loop melalui array

```javascript
const fruits = ["apple", "banana", "orange"];

// Cara lama (for loop)
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}

// Cara modern (forEach)
fruits.forEach((fruit) => {
  console.log(fruit);
});

// Atau lebih singkat
fruits.forEach(fruit => console.log(fruit));
```

#### 2. map - Transform setiap elemen

```javascript
const numbers = [1, 2, 3, 4, 5];

// Kalikan setiap angka dengan 2
const doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// Ubah array string menjadi uppercase
const fruits = ["apple", "banana"];
const upperFruits = fruits.map(fruit => fruit.toUpperCase());
console.log(upperFruits); // ["APPLE", "BANANA"]
```

#### 3. filter - Filter elemen berdasarkan kondisi

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Ambil hanya angka genap
const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4, 6, 8, 10]

// Filter berdasarkan panjang string
const fruits = ["apple", "banana", "kiwi", "orange"];
const longFruits = fruits.filter(fruit => fruit.length > 4);
console.log(longFruits); // ["apple", "banana", "orange"]
```

#### 4. find - Cari elemen pertama yang sesuai

```javascript
const users = [
  { id: 1, name: "John", age: 25 },
  { id: 2, name: "Jane", age: 30 },
  { id: 3, name: "Bob", age: 25 }
];

// Cari user dengan id = 2
const user = users.find(u => u.id === 2);
console.log(user); // { id: 2, name: "Jane", age: 30 }

// Cari user dengan age = 25
const youngUser = users.find(u => u.age === 25);
console.log(youngUser); // { id: 1, name: "John", age: 25 }
```

#### 5. Chaining Methods

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Filter angka genap, lalu kalikan dengan 2
const result = numbers
  .filter(num => num % 2 === 0)  // [2, 4, 6, 8, 10]
  .map(num => num * 2);            // [4, 8, 12, 16, 20]

console.log(result); // [4, 8, 12, 16, 20]
```

### Latihan 1.3

```javascript
const products = [
  { id: 1, name: "Laptop", price: 1000, category: "electronics" },
  { id: 2, name: "Phone", price: 500, category: "electronics" },
  { id: 3, name: "Book", price: 20, category: "books" },
  { id: 4, name: "Tablet", price: 300, category: "electronics" }
];

// 1. Gunakan map untuk mendapatkan array nama produk saja
// 2. Gunakan filter untuk mendapatkan produk dengan harga < 500
// 3. Gunakan find untuk mencari produk dengan id = 3
// 4. Gunakan filter dan map untuk mendapatkan nama produk elektronik dengan harga < 400

// Tulis kode di bawah ini:
```

---

## 📚 Materi 1.4: Object dan Destructuring

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Object adalah struktur data yang sangat penting untuk REST API
- Destructuring memudahkan ekstraksi data dari object/array
- Spread operator untuk menggabungkan object/array
- Ini akan sangat berguna saat bekerja dengan request/response di Express

### Materi Pembelajaran

#### 1. Object Basics

```javascript
// Membuat object
const user = {
  id: 1,
  name: "John Doe",
  email: "john@example.com",
  age: 25,
  isActive: true
};

// Mengakses property
console.log(user.name);        // "John Doe"
console.log(user["email"]);    // "john@example.com"

// Menambah property baru
user.phone = "123-456-7890";

// Menghapus property
delete user.age;
```

#### 2. Destructuring Object

```javascript
const user = {
  id: 1,
  name: "John Doe",
  email: "john@example.com",
  age: 25
};

// Destructuring (cara lama)
const name = user.name;
const email = user.email;

// Destructuring (cara modern)
const { name, email, age } = user;
console.log(name);  // "John Doe"
console.log(email); // "john@example.com"

// Destructuring dengan rename
const { name: userName, email: userEmail } = user;

// Destructuring dengan default value
const { name, email, phone = "N/A" } = user;
```

#### 3. Destructuring Array

```javascript
const fruits = ["apple", "banana", "orange"];

// Destructuring array
const [first, second, third] = fruits;
console.log(first);  // "apple"
console.log(second); // "banana"

// Skip elemen
const [first, , third] = fruits;
console.log(first); // "apple"
console.log(third); // "orange"
```

#### 4. Spread Operator

```javascript
// Spread untuk array
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// Spread untuk object
const user = { name: "John", age: 25 };
const updatedUser = { ...user, email: "john@example.com" };
console.log(updatedUser); 
// { name: "John", age: 25, email: "john@example.com" }

// Update property dengan spread
const updatedAge = { ...user, age: 26 };
console.log(updatedAge); 
// { name: "John", age: 26 }
```

### Latihan 1.4

```javascript
const order = {
  id: 123,
  customer: {
    name: "John Doe",
    email: "john@example.com"
  },
  items: [
    { product: "Laptop", price: 1000, quantity: 1 },
    { product: "Mouse", price: 25, quantity: 2 }
  ],
  total: 1050
};

// 1. Destructure untuk mendapatkan id, total, dan customer name
// 2. Gunakan spread operator untuk membuat order baru dengan total yang berbeda
// 3. Destructure items array untuk mendapatkan item pertama

// Tulis kode di bawah ini:
```

---

## 📚 Materi 1.5: Async/Await dan Promises

### Penjelasan untuk Instruktur

**Konsep Penting:**
- REST API selalu bekerja dengan asynchronous operations
- Promise adalah cara JavaScript menangani operasi async
- async/await adalah sintaks modern yang lebih mudah dibaca
- Ini sangat penting untuk database operations, API calls, dll

### Materi Pembelajaran

#### 1. Promise Basics

```javascript
// Promise adalah representasi nilai yang akan tersedia di masa depan
const fetchData = () => {
  return new Promise((resolve, reject) => {
    // Simulasi operasi async (misalnya fetch dari API)
    setTimeout(() => {
      const data = { id: 1, name: "John" };
      resolve(data); // Berhasil
      // reject("Error!"); // Gagal
    }, 1000);
  });
};

// Menggunakan Promise dengan .then()
fetchData()
  .then(data => {
    console.log("Data:", data);
  })
  .catch(error => {
    console.error("Error:", error);
  });
```

#### 2. async/await (Cara Modern)

```javascript
// Function harus async untuk menggunakan await
const fetchData = async () => {
  try {
    // Simulasi operasi async
    const response = await new Promise((resolve) => {
      setTimeout(() => {
        resolve({ id: 1, name: "John" });
      }, 1000);
    });
    
    console.log("Data:", response);
    return response;
  } catch (error) {
    console.error("Error:", error);
    throw error;
  }
};

// Memanggil async function
fetchData();
```

#### 3. Contoh Real-World (Fetch API)

```javascript
// Fetch data dari API
const fetchUser = async (userId) => {
  try {
    const response = await fetch(`https://api.example.com/users/${userId}`);
    const user = await response.json();
    return user;
  } catch (error) {
    console.error("Failed to fetch user:", error);
    throw error;
  }
};

// Menggunakan function tersebut
fetchUser(1)
  .then(user => console.log(user))
  .catch(error => console.error(error));
```

#### 4. Multiple Async Operations

```javascript
// Sequential (satu per satu)
const fetchSequential = async () => {
  const user1 = await fetchUser(1);
  const user2 = await fetchUser(2);
  return [user1, user2];
};

// Parallel (bersamaan - lebih cepat)
const fetchParallel = async () => {
  const [user1, user2] = await Promise.all([
    fetchUser(1),
    fetchUser(2)
  ]);
  return [user1, user2];
};
```

### Latihan 1.5

```javascript
// Buat fungsi async yang:
// 1. Simulasi delay 1 detik, lalu return object { message: "Hello" }
// 2. Simulasi delay 2 detik, lalu return object { data: [1, 2, 3] }
// 3. Panggil kedua fungsi secara parallel menggunakan Promise.all

// Tulis kode di bawah ini:
```

---

## 📚 Materi 1.6: Modules (import/export)

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Modules membantu mengorganisir kode menjadi file-file terpisah
- ES6 modules adalah standar modern
- Ini sangat penting untuk struktur project Express yang baik
- Export untuk membuat kode bisa digunakan di file lain
- Import untuk menggunakan kode dari file lain

### Materi Pembelajaran

#### 1. Export (Named Export)

```javascript
// file: utils.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export const multiply = (a, b) => a * b;

// Atau export di akhir
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
export { add, subtract };
```

#### 2. Export Default

```javascript
// file: user.js
const createUser = (name, email) => {
  return { name, email, createdAt: new Date() };
};

export default createUser;
```

#### 3. Import

```javascript
// Import named exports
import { add, subtract } from './utils.js';
import { add as sum, subtract as diff } from './utils.js'; // Dengan alias

// Import default export
import createUser from './user.js';

// Import semua named exports
import * as utils from './utils.js';
utils.add(1, 2);

// Import default + named
import createUser, { validateEmail } from './user.js';
```

#### 4. Contoh Struktur

```
project/
├── utils/
│   ├── math.js      // Export functions matematika
│   └── string.js    // Export functions string
├── models/
│   └── user.js      // Export user model
└── app.js           // Import semua modules
```

```javascript
// file: utils/math.js
export const add = (a, b) => a + b;
export const multiply = (a, b) => a * b;

// file: app.js
import { add, multiply } from './utils/math.js';

console.log(add(2, 3));        // 5
console.log(multiply(2, 3));   // 6
```

### Latihan 1.6

```javascript
// Buat struktur berikut:
// 1. File calculator.js dengan fungsi add, subtract, multiply, divide
// 2. File app.js yang import dan menggunakan semua fungsi tersebut
// 3. Jalankan app.js dan test semua fungsi

// Tulis kode di bawah ini:
```

---

## ✅ Review Modul 1.1

### Checklist Pemahaman

Sebelum lanjut ke modul berikutnya, pastikan peserta memahami:

- [ ] Bisa membuat variabel dengan `let` dan `const`
- [ ] Bisa membuat fungsi dengan arrow function
- [ ] Bisa menggunakan `map`, `filter`, `find` pada array
- [ ] Bisa melakukan destructuring object dan array
- [ ] Bisa menggunakan `async/await` untuk operasi asynchronous
- [ ] Bisa membuat dan menggunakan modules (import/export)

### Latihan Akhir Modul

Buat aplikasi sederhana dengan struktur berikut:

```
project/
├── utils/
│   ├── math.js          // Fungsi matematika
│   └── string.js        // Fungsi manipulasi string
├── data/
│   └── users.js         // Array data users
├── services/
│   └── userService.js   // Fungsi untuk manipulasi users (async)
└── app.js               // Main file yang menggunakan semua modules
```

**Requirements:**
1. `utils/math.js`: Export fungsi `add`, `multiply`, `calculateTotal`
2. `utils/string.js`: Export fungsi `formatName`, `capitalize`
3. `data/users.js`: Export array users dengan minimal 5 user
4. `services/userService.js`: 
   - `getAllUsers()` - return semua users (async)
   - `findUserById(id)` - cari user by id (async)
   - `filterUsersByAge(minAge)` - filter users by age (async)
5. `app.js`: Import semua dan demonstrasikan penggunaan

---

## 🎯 Persiapan untuk Modul Berikutnya

Modul berikutnya (1.2) akan memperkenalkan TypeScript. Pastikan peserta sudah nyaman dengan:
- ✅ Sintaks JavaScript modern (ES6+)
- ✅ Konsep function dan arrow function
- ✅ Array methods
- ✅ Object dan destructuring
- ✅ Async/await

**Catatan Instruktur:** Jika peserta masih kesulitan dengan konsep async/await, luangkan waktu ekstra karena ini sangat penting untuk REST API development.
