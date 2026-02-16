# Modul 4: Express.js Dasar

## 🎯 Tujuan Pembelajaran

Setelah modul ini, peserta dapat:
- Memahami apa itu Express.js dan kegunaannya
- Membuat server Express sederhana
- Menggunakan routing dan HTTP methods
- Memahami dan menggunakan middleware
- Menangani request dan response
- Mengimplementasikan error handling dasar

## ⏱️ Durasi: 2-3 Sesi

---

## 📚 Materi 4.1: Pengenalan Express.js

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Express.js adalah framework web untuk Node.js
- Express membuat development server lebih mudah
- Express menangani routing, middleware, dll
- Express adalah foundation untuk REST API

### Materi Pembelajaran

#### 1. Apa itu Express.js?

**Express.js** adalah framework web yang minimal dan fleksibel untuk Node.js. Express menyediakan:
- Routing (menentukan bagaimana aplikasi merespons request)
- Middleware (fungsi yang dijalankan sebelum request sampai ke route handler)
- Template engines (untuk render HTML)
- Error handling

#### 2. Instalasi Express

```bash
# Buat folder project baru
mkdir my-express-app
cd my-express-app

# Inisialisasi npm project
npm init -y

# Install Express
npm install express

# Install TypeScript dan dependencies (jika pakai TypeScript)
npm install -D typescript @types/express @types/node ts-node nodemon
```

#### 3. Server Express Pertama (JavaScript)

```javascript
// server.js
const express = require('express');
const app = express();
const PORT = 3000;

// Route sederhana
app.get('/', (req, res) => {
  res.send('Hello World!');
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

**Menjalankan:**
```bash
node server.js
```

#### 4. Server Express dengan TypeScript

```typescript
// server.ts
import express, { Request, Response } from 'express';

const app = express();
const PORT = 3000;

app.get('/', (req: Request, res: Response) => {
  res.send('Hello World!');
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

**package.json scripts:**
```json
{
  "scripts": {
    "dev": "nodemon --exec ts-node server.ts",
    "build": "tsc",
    "start": "node dist/server.js"
  }
}
```

### Latihan 3.1

```typescript
// Buat server Express sederhana yang:
// 1. Menjalankan di port 3000
// 2. Memiliki route GET '/' yang return "Welcome to Express!"
// 3. Memiliki route GET '/about' yang return "About Page"
// 4. Console log saat server start

// Tulis kode di bawah ini:
```

---

## 📚 Materi 4.2: Routing dan HTTP Methods

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Routing menentukan bagaimana aplikasi merespons request ke endpoint tertentu
- HTTP methods: GET, POST, PUT, DELETE, PATCH
- Route parameters untuk dynamic routes
- Query parameters untuk filtering/searching

### Materi Pembelajaran

#### 1. HTTP Methods Dasar

```typescript
import express, { Request, Response } from 'express';

const app = express();

// GET - untuk mengambil data
app.get('/users', (req: Request, res: Response) => {
  res.json({ message: 'Get all users' });
});

// POST - untuk membuat data baru
app.post('/users', (req: Request, res: Response) => {
  res.json({ message: 'Create new user' });
});

// PUT - untuk update seluruh data
app.put('/users/:id', (req: Request, res: Response) => {
  res.json({ message: `Update user ${req.params.id}` });
});

// DELETE - untuk menghapus data
app.delete('/users/:id', (req: Request, res: Response) => {
  res.json({ message: `Delete user ${req.params.id}` });
});

// PATCH - untuk update sebagian data
app.patch('/users/:id', (req: Request, res: Response) => {
  res.json({ message: `Partially update user ${req.params.id}` });
});
```

#### 2. Route Parameters

```typescript
// Route dengan parameter
app.get('/users/:id', (req: Request, res: Response) => {
  const userId = req.params.id;
  res.json({ 
    message: `Get user with ID: ${userId}`,
    userId: userId
  });
});

// Multiple parameters
app.get('/users/:userId/posts/:postId', (req: Request, res: Response) => {
  const { userId, postId } = req.params;
  res.json({ 
    userId, 
    postId 
  });
});

// Contoh penggunaan:
// GET /users/123 -> { message: "Get user with ID: 123", userId: "123" }
// GET /users/123/posts/456 -> { userId: "123", postId: "456" }
```

#### 3. Query Parameters

```typescript
// Query parameters untuk filtering/searching
app.get('/users', (req: Request, res: Response) => {
  const { page, limit, search } = req.query;
  
  res.json({
    message: 'Get users',
    filters: {
      page: page || 1,
      limit: limit || 10,
      search: search || ''
    }
  });
});

// Contoh penggunaan:
// GET /users?page=1&limit=20&search=john
// req.query = { page: '1', limit: '20', search: 'john' }
```

#### 4. Request Body (untuk POST/PUT)

```typescript
// IMPORTANT: Perlu middleware untuk parse JSON body
app.use(express.json()); // Tambahkan ini sebelum routes

app.post('/users', (req: Request, res: Response) => {
  const { name, email, age } = req.body;
  
  res.json({
    message: 'User created',
    user: { name, email, age }
  });
});

// Contoh request body:
// POST /users
// Body: { "name": "John", "email": "john@example.com", "age": 25 }
```

### Latihan 3.2

```typescript
// Buat routes berikut:
// 1. GET /products - return semua products
// 2. GET /products/:id - return product berdasarkan ID
// 3. GET /products?category=electronics - filter by category (query param)
// 4. POST /products - create new product (dengan body: name, price, category)
// 5. PUT /products/:id - update product
// 6. DELETE /products/:id - delete product

// Tulis kode di bawah ini:
```

---

## 📚 Materi 4.3: Middleware

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Middleware adalah fungsi yang dijalankan sebelum request sampai ke route handler
- Middleware bisa mengakses request, response, dan next function
- Middleware bisa memodifikasi request/response atau menghentikan request
- Express memiliki built-in middleware dan kita bisa buat custom middleware

### Materi Pembelajaran

#### 1. Konsep Middleware

```typescript
// Middleware adalah fungsi dengan signature:
// (req, res, next) => void

// Middleware sederhana
const logger = (req: Request, res: Response, next: Function) => {
  console.log(`${req.method} ${req.path} - ${new Date().toISOString()}`);
  next(); // Penting! Panggil next() untuk lanjut ke middleware/route berikutnya
};

// Menggunakan middleware
app.use(logger); // Akan dijalankan untuk semua routes
```

#### 2. Built-in Middleware

```typescript
// express.json() - parse JSON body
app.use(express.json());

// express.urlencoded() - parse form data
app.use(express.urlencoded({ extended: true }));

// express.static() - serve static files
app.use(express.static('public'));

// Contoh: request dengan JSON body akan di-parse otomatis
app.post('/users', (req: Request, res: Response) => {
  console.log(req.body); // Sudah berupa object, bukan string
  res.json(req.body);
});
```

#### 3. Custom Middleware

```typescript
// Middleware untuk logging
const requestLogger = (req: Request, res: Response, next: Function) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.path}`);
  next();
};

// Middleware untuk authentication (contoh sederhana)
const authenticate = (req: Request, res: Response, next: Function) => {
  const token = req.headers.authorization;
  
  if (!token) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  
  // Di sini biasanya validasi token
  // Untuk sekarang, kita anggap token valid jika ada
  next();
};

// Menggunakan middleware
app.use(requestLogger); // Global middleware

app.get('/public', (req: Request, res: Response) => {
  res.json({ message: 'Public route' });
});

app.get('/private', authenticate, (req: Request, res: Response) => {
  res.json({ message: 'Private route' });
});
```

#### 4. Middleware untuk Route Specific

```typescript
// Middleware hanya untuk route tertentu
app.get('/users', authenticate, (req: Request, res: Response) => {
  res.json({ message: 'Users list' });
});

// Multiple middleware
const validateUser = (req: Request, res: Response, next: Function) => {
  const { name, email } = req.body;
  if (!name || !email) {
    return res.status(400).json({ error: 'Name and email required' });
  }
  next();
};

app.post('/users', authenticate, validateUser, (req: Request, res: Response) => {
  res.json({ message: 'User created', user: req.body });
});
```

#### 5. Error Handling Middleware

```typescript
// Error handling middleware (harus di akhir, setelah semua routes)
app.use((err: Error, req: Request, res: Response, next: Function) => {
  console.error(err.stack);
  res.status(500).json({ 
    error: 'Something went wrong!',
    message: err.message 
  });
});

// Contoh route yang throw error
app.get('/error', (req: Request, res: Response) => {
  throw new Error('Test error');
});
```

### Latihan 3.3

```typescript
// Buat middleware berikut:
// 1. Middleware untuk log setiap request (method, path, timestamp)
// 2. Middleware untuk validasi API key di header (header: x-api-key)
// 3. Middleware untuk validasi product data (name, price harus ada)
// 4. Error handling middleware yang return JSON error response
// 5. Gunakan middleware tersebut di routes yang sesuai

// Tulis kode di bawah ini:
```

---

## 📚 Materi 4.4: Request dan Response Objects

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Request object berisi informasi tentang HTTP request
- Response object digunakan untuk mengirim response ke client
- Penting memahami properties dan methods yang tersedia
- Ini akan sangat berguna untuk REST API development

### Materi Pembelajaran

#### 1. Request Object Properties

```typescript
app.get('/example/:id', (req: Request, res: Response) => {
  // req.params - route parameters
  console.log(req.params); // { id: '123' }
  
  // req.query - query parameters
  console.log(req.query); // { page: '1', limit: '10' }
  
  // req.body - request body (setelah di-parse oleh middleware)
  console.log(req.body); // { name: 'John', email: '...' }
  
  // req.headers - HTTP headers
  console.log(req.headers.authorization);
  console.log(req.headers['content-type']);
  
  // req.method - HTTP method
  console.log(req.method); // 'GET', 'POST', etc.
  
  // req.path - URL path
  console.log(req.path); // '/example/123'
  
  // req.url - full URL
  console.log(req.url); // '/example/123?page=1'
  
  res.json({ message: 'Check console' });
});
```

#### 2. Response Object Methods

```typescript
app.get('/response-examples', (req: Request, res: Response) => {
  // res.send() - send response (auto detect content type)
  res.send('Hello World');
  res.send({ message: 'Hello' }); // JSON
  res.send('<h1>Hello</h1>'); // HTML
  
  // res.json() - send JSON response
  res.json({ message: 'Hello', data: [1, 2, 3] });
  
  // res.status() - set status code
  res.status(201).json({ message: 'Created' });
  
  // res.sendStatus() - send status code dengan default message
  res.sendStatus(404); // Sends "Not Found"
  
  // res.redirect() - redirect to another URL
  // res.redirect('/users');
  
  // res.setHeader() - set custom header
  res.setHeader('X-Custom-Header', 'value');
  res.json({ message: 'Custom header set' });
});
```

#### 3. Status Codes

```typescript
// Success responses
app.get('/success', (req: Request, res: Response) => {
  res.status(200).json({ message: 'OK' }); // 200 - Success
  // res.status(201).json({ message: 'Created' }); // 201 - Created
});

// Client error responses
app.get('/not-found', (req: Request, res: Response) => {
  res.status(404).json({ error: 'Not Found' }); // 404 - Not Found
  // res.status(400).json({ error: 'Bad Request' }); // 400 - Bad Request
  // res.status(401).json({ error: 'Unauthorized' }); // 401 - Unauthorized
  // res.status(403).json({ error: 'Forbidden' }); // 403 - Forbidden
});

// Server error responses
app.get('/server-error', (req: Request, res: Response) => {
  res.status(500).json({ error: 'Internal Server Error' }); // 500 - Server Error
});
```

#### 4. Contoh Lengkap: CRUD dengan Request/Response

```typescript
// In-memory data store (untuk contoh saja)
let users: Array<{ id: number; name: string; email: string }> = [];

// GET all users
app.get('/users', (req: Request, res: Response) => {
  const { page = '1', limit = '10' } = req.query;
  const pageNum = parseInt(page as string);
  const limitNum = parseInt(limit as string);
  
  const start = (pageNum - 1) * limitNum;
  const end = start + limitNum;
  const paginatedUsers = users.slice(start, end);
  
  res.status(200).json({
    data: paginatedUsers,
    pagination: {
      page: pageNum,
      limit: limitNum,
      total: users.length
    }
  });
});

// GET user by ID
app.get('/users/:id', (req: Request, res: Response) => {
  const userId = parseInt(req.params.id);
  const user = users.find(u => u.id === userId);
  
  if (!user) {
    return res.status(404).json({ error: 'User not found' });
  }
  
  res.status(200).json({ data: user });
});

// POST create user
app.post('/users', (req: Request, res: Response) => {
  const { name, email } = req.body;
  
  if (!name || !email) {
    return res.status(400).json({ error: 'Name and email required' });
  }
  
  const newUser = {
    id: users.length + 1,
    name,
    email
  };
  
  users.push(newUser);
  res.status(201).json({ data: newUser });
});
```

### Latihan 3.4

```typescript
// Buat endpoints berikut dengan proper request/response handling:
// 1. GET /products - return products dengan pagination (query: page, limit)
// 2. GET /products/:id - return product by ID (404 jika tidak ditemukan)
// 3. POST /products - create product (400 jika data tidak valid, 201 jika berhasil)
// 4. PUT /products/:id - update product (404 jika tidak ditemukan)
// 5. DELETE /products/:id - delete product (404 jika tidak ditemukan)

// Gunakan in-memory array untuk menyimpan data products

// Tulis kode di bawah ini:
```

---

## 📚 Materi 4.5: Error Handling

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Error handling sangat penting untuk production apps
- Try-catch untuk synchronous errors
- Error handling middleware untuk global error handling
- Custom error classes untuk error yang lebih spesifik
- Proper error responses untuk client

### Materi Pembelajaran

#### 1. Try-Catch di Route Handlers

```typescript
app.get('/users/:id', async (req: Request, res: Response) => {
  try {
    const userId = parseInt(req.params.id);
    
    if (isNaN(userId)) {
      return res.status(400).json({ error: 'Invalid user ID' });
    }
    
    // Simulasi async operation yang bisa error
    const user = await getUserById(userId);
    
    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }
    
    res.status(200).json({ data: user });
  } catch (error) {
    console.error('Error fetching user:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
});
```

#### 2. Error Handling Middleware

```typescript
// Error handling middleware (harus di akhir, setelah semua routes)
interface CustomError extends Error {
  statusCode?: number;
}

app.use((err: CustomError, req: Request, res: Response, next: Function) => {
  const statusCode = err.statusCode || 500;
  const message = err.message || 'Internal Server Error';
  
  console.error('Error:', err);
  
  res.status(statusCode).json({
    error: message,
    ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
  });
});

// Contoh route yang throw error
app.get('/error', (req: Request, res: Response) => {
  const error: CustomError = new Error('Something went wrong');
  error.statusCode = 400;
  throw error;
});
```

#### 3. Custom Error Class

```typescript
// Custom error class
class AppError extends Error {
  statusCode: number;
  
  constructor(message: string, statusCode: number = 500) {
    super(message);
    this.statusCode = statusCode;
    this.name = this.constructor.name;
    Error.captureStackTrace(this, this.constructor);
  }
}

// Menggunakan custom error
app.get('/users/:id', async (req: Request, res: Response, next: Function) => {
  try {
    const userId = parseInt(req.params.id);
    
    if (isNaN(userId)) {
      throw new AppError('Invalid user ID', 400);
    }
    
    const user = await getUserById(userId);
    
    if (!user) {
      throw new AppError('User not found', 404);
    }
    
    res.status(200).json({ data: user });
  } catch (error) {
    next(error); // Pass error ke error handling middleware
  }
});

// Error handling middleware yang handle AppError
app.use((err: AppError, req: Request, res: Response, next: Function) => {
  const statusCode = err.statusCode || 500;
  const message = err.message || 'Internal Server Error';
  
  res.status(statusCode).json({
    error: message
  });
});
```

#### 4. Async Error Wrapper

```typescript
// Wrapper untuk catch async errors secara otomatis
const asyncHandler = (fn: Function) => {
  return (req: Request, res: Response, next: Function) => {
    Promise.resolve(fn(req, res, next)).catch(next);
  };
};

// Menggunakan wrapper
app.get('/users/:id', asyncHandler(async (req: Request, res: Response) => {
  const userId = parseInt(req.params.id);
  
  if (isNaN(userId)) {
    throw new AppError('Invalid user ID', 400);
  }
  
  const user = await getUserById(userId);
  
  if (!user) {
    throw new AppError('User not found', 404);
  }
  
  res.status(200).json({ data: user });
}));
```

### Latihan 3.5

```typescript
// Implementasikan error handling untuk aplikasi Express:
// 1. Buat custom AppError class
// 2. Buat asyncHandler wrapper
// 3. Update semua routes untuk menggunakan error handling
// 4. Buat error handling middleware
// 5. Test dengan berbagai skenario error (404, 400, 500)

// Tulis kode di bawah ini:
```

---

## ✅ Review Modul 3

### Checklist Pemahaman

Sebelum lanjut ke modul berikutnya, pastikan peserta memahami:

- [ ] Bisa membuat server Express sederhana
- [ ] Memahami routing dan HTTP methods (GET, POST, PUT, DELETE)
- [ ] Bisa menggunakan route parameters dan query parameters
- [ ] Memahami konsep middleware dan bisa membuat custom middleware
- [ ] Bisa menggunakan request dan response objects
- [ ] Bisa mengimplementasikan error handling dasar

### Latihan Akhir Modul

Buat aplikasi Express sederhana dengan fitur berikut:

**Requirements:**
1. Server Express dengan TypeScript
2. CRUD endpoints untuk `/tasks`:
   - GET /tasks - list semua tasks dengan pagination
   - GET /tasks/:id - get task by ID
   - POST /tasks - create new task
   - PUT /tasks/:id - update task
   - DELETE /tasks/:id - delete task
3. Middleware:
   - Request logger
   - Error handling middleware
4. Validation:
   - Validasi input untuk POST/PUT
   - Return proper error messages
5. Error handling:
   - 400 untuk invalid input
   - 404 untuk resource not found
   - 500 untuk server errors

**Task Model:**
```typescript
interface Task {
  id: number;
  title: string;
  description?: string;
  completed: boolean;
  createdAt: Date;
}
```

---

## 🎯 Persiapan untuk Modul Berikutnya

Modul berikutnya akan membahas REST API development yang lebih advanced. Pastikan peserta sudah nyaman dengan:
- ✅ Express.js basics
- ✅ Routing dan HTTP methods
- ✅ Middleware
- ✅ Request/Response handling
- ✅ Error handling dasar
- ✅ Git untuk version control (dari Modul 3)

**Catatan Instruktur:** 
- Pastikan peserta sudah bisa membuat CRUD sederhana
- Fokus pada pemahaman konsep, bukan menghafal syntax
- Latihan praktik sangat penting di modul ini
- **Dorong peserta untuk menggunakan Git** untuk track semua perubahan kode
