# Modul 6: Testing API

## 🎯 Tujuan Pembelajaran

Setelah modul ini, peserta dapat:
- Memahami pentingnya testing dalam development
- Menggunakan Postman untuk manual testing API
- Membuat dan mengorganisir collection di Postman
- Menulis unit test untuk API endpoints
- Menulis integration test
- Menerapkan best practices untuk testing

## ⏱️ Durasi: 2-3 Sesi

---

## 📚 Materi 6.1: Pengenalan Testing API

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Testing memastikan kode bekerja dengan benar
- Manual testing vs automated testing
- Unit test vs integration test
- Testing sangat penting untuk production apps
- Testing membantu catch bugs sebelum production

### Materi Pembelajaran

#### 1. Mengapa Testing Penting?

**Testing membantu:**
- ✅ Memastikan kode bekerja dengan benar
- ✅ Catch bugs sebelum production
- ✅ Dokumentasi bagaimana kode seharusnya bekerja
- ✅ Memudahkan refactoring
- ✅ Meningkatkan confidence saat deploy

**Tanpa Testing:**
```
Develop → Deploy → Bug ditemukan user → Fix → Deploy lagi
```
😱 Cycle yang tidak efisien dan berisiko!

**Dengan Testing:**
```
Develop → Test → Fix → Test lagi → Deploy
```
✅ Lebih aman dan efisien!

#### 2. Jenis-jenis Testing

**Manual Testing:**
- Testing dengan tangan (manual)
- Menggunakan tools seperti Postman
- Baik untuk exploratory testing
- Lambat tapi fleksibel

**Automated Testing:**
- Testing dengan code (automated)
- Menggunakan testing framework seperti Jest
- Cepat dan bisa dijalankan berkali-kali
- Perlu maintenance

**Unit Test:**
- Test satu fungsi/unit secara terpisah
- Cepat dan isolated
- Contoh: test function `calculateTotal()`

**Integration Test:**
- Test beberapa komponen bekerja bersama
- Lebih lambat tapi lebih realistis
- Contoh: test endpoint `/users` dengan database

**E2E Test (End-to-End):**
- Test seluruh flow aplikasi
- Paling lambat tapi paling lengkap
- Contoh: test flow register → login → create order

#### 3. Testing Pyramid

```
        /\
       /E2E\        ← Sedikit (mahal, lambat)
      /------\
     /Integration\  ← Sedang
    /------------\
   /   Unit Test   \ ← Banyak (murah, cepat)
  /----------------\
```

**Prinsip:**
- Banyak unit test (murah, cepat)
- Sedang integration test
- Sedikit E2E test (mahal, lambat)

### Latihan 6.1

```
Jawab pertanyaan berikut:
1. Apa perbedaan antara manual testing dan automated testing?
2. Kapan sebaiknya menggunakan unit test vs integration test?
3. Mengapa testing penting untuk production apps?

Tulis jawaban Anda:
```

---

## 📚 Materi 6.2: Testing dengan Postman

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Postman adalah tool untuk testing API secara manual
- Collection untuk mengorganisir requests
- Environment untuk manage variables
- Tests untuk automated assertions
- Pre-request scripts untuk setup

### Materi Pembelajaran

#### 1. Install dan Setup Postman

**Download:**
- Windows/Mac/Linux: https://www.postman.com/downloads/
- Atau gunakan web version: https://web.postman.com/

**Setup:**
1. Install Postman
2. Buat akun (optional tapi recommended)
3. Selesai!

#### 2. Membuat Request Pertama

**Membuat GET Request:**
1. Buka Postman
2. Klik **"New"** → **"HTTP Request"**
3. Pilih method: **GET**
4. Masukkan URL: `http://localhost:3000/health`
5. Klik **"Send"**
6. Lihat response di bawah

**Membuat POST Request:**
1. Pilih method: **POST**
2. Masukkan URL: `http://localhost:3000/api/users`
3. Klik tab **"Body"**
4. Pilih **"raw"** → **"JSON"**
5. Masukkan body:
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "age": 25
}
```
6. Klik **"Send"**

#### 3. Mengorganisir dengan Collection

**Membuat Collection:**
1. Klik **"New"** → **"Collection"**
2. Beri nama: "My API Tests"
3. Klik **"Create"**

**Menambahkan Request ke Collection:**
1. Buat request baru
2. Klik **"Save"**
3. Pilih collection yang sudah dibuat
4. Beri nama request
5. Klik **"Save"**

**Mengorganisir Collection:**
```
My API Tests/
├── Users/
│   ├── GET All Users
│   ├── GET User by ID
│   ├── POST Create User
│   ├── PUT Update User
│   └── DELETE User
├── Products/
│   ├── GET All Products
│   └── POST Create Product
└── Auth/
    ├── POST Register
    └── POST Login
```

#### 4. Menggunakan Environment Variables

**Membuat Environment:**
1. Klik icon **"Environments"** (kiri)
2. Klik **"+"** untuk buat baru
3. Beri nama: "Development"
4. Tambahkan variables:
   - `base_url`: `http://localhost:3000`
   - `api_url`: `{{base_url}}/api`
   - `token`: (kosongkan dulu)

**Menggunakan Variables:**
- Di URL: `{{api_url}}/users`
- Di Body: `{{token}}`
- Di Headers: `Authorization: Bearer {{token}}`

**Switch Environment:**
- Pilih environment di dropdown (kanan atas)
- Bisa buat multiple environments (Dev, Staging, Production)

#### 5. Headers dan Authentication

**Menambahkan Headers:**
1. Klik tab **"Headers"**
2. Tambahkan header:
   - Key: `Content-Type`
   - Value: `application/json`
3. Atau:
   - Key: `Authorization`
   - Value: `Bearer {{token}}`

**Authentication Types:**
- **No Auth**: Tidak perlu authentication
- **Bearer Token**: Token di header
- **Basic Auth**: Username/password
- **API Key**: Key di header atau query

#### 6. Tests di Postman

**Menambahkan Tests:**
1. Klik tab **"Tests"**
2. Tulis test script:
```javascript
// Test status code
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// Test response body
pm.test("Response has data", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('data');
});

// Test response time
pm.test("Response time is less than 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});

// Test specific value
pm.test("User name is correct", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.data.name).to.eql("John Doe");
});
```

**Menyimpan Token dari Response:**
```javascript
// Setelah login, simpan token
pm.test("Login successful", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.data).to.have.property('token');
    
    // Simpan token ke environment
    pm.environment.set("token", jsonData.data.token);
});
```

#### 7. Pre-request Scripts

**Menggunakan Pre-request Script:**
1. Klik tab **"Pre-request Script"**
2. Tulis script untuk setup sebelum request:
```javascript
// Generate random email untuk test
const randomEmail = `test${Date.now()}@example.com`;
pm.environment.set("test_email", randomEmail);

// Set timestamp
pm.environment.set("timestamp", new Date().toISOString());
```

**Use Cases:**
- Generate test data
- Setup authentication
- Calculate values
- Set timestamps

#### 8. Collection Runner

**Menjalankan Semua Tests:**
1. Klik collection
2. Klik **"Run"**
3. Pilih requests yang mau di-test
4. Klik **"Run [Collection Name]"**
5. Lihat hasil semua tests

**Useful untuk:**
- Test semua endpoints sekaligus
- Regression testing
- CI/CD integration

### Latihan 6.2

```
Praktikkan dengan Postman:
1. Install Postman
2. Buat collection "API Testing"
3. Buat environment "Development" dengan variables:
   - base_url: http://localhost:3000
   - api_url: {{base_url}}/api
4. Buat requests untuk semua CRUD endpoints:
   - GET /api/users
   - GET /api/users/:id
   - POST /api/users
   - PUT /api/users/:id
   - DELETE /api/users/:id
5. Tambahkan tests untuk setiap request
6. Test authentication flow (register → login → use token)
7. Run collection dan lihat hasil

Catat setiap langkah:
```

---

## 📚 Materi 6.3: Unit Testing dengan Jest

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Jest adalah testing framework untuk JavaScript/TypeScript
- Unit test untuk test fungsi secara terpisah
- Mock untuk isolate dependencies
- Test coverage untuk melihat bagian yang sudah di-test
- TDD (Test-Driven Development) approach

### Materi Pembelajaran

#### 1. Setup Jest

**Install Dependencies:**
```bash
npm install -D jest @types/jest ts-jest
npm install -D @types/supertest supertest
```

**Jest Configuration:**
```javascript
// jest.config.js
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['<rootDir>/src'],
  testMatch: ['**/__tests__/**/*.ts', '**/?(*.)+(spec|test).ts'],
  collectCoverageFrom: [
    'src/**/*.ts',
    '!src/**/*.d.ts',
    '!src/**/__tests__/**',
  ],
};
```

**package.json scripts:**
```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

#### 2. Struktur Testing

**File Naming:**
```
src/
├── components/
│   └── users/
│       ├── user.service.ts
│       └── user.service.test.ts    ← Test file
└── __tests__/
    └── integration/
        └── users.test.ts           ← Integration test
```

**Convention:**
- `*.test.ts` atau `*.spec.ts` untuk test files
- Bisa di folder `__tests__/` atau di samping file yang di-test

#### 3. Menulis Unit Test Pertama

**Contoh: Test Service Function**
```typescript
// src/components/users/user.service.ts
export class UserService {
  calculateAge(birthYear: number): number {
    const currentYear = new Date().getFullYear();
    return currentYear - birthYear;
  }
}
```

**Test File:**
```typescript
// src/components/users/user.service.test.ts
import { UserService } from './user.service';

describe('UserService', () => {
  let userService: UserService;

  beforeEach(() => {
    userService = new UserService();
  });

  describe('calculateAge', () => {
    it('should calculate age correctly', () => {
      const birthYear = 1995;
      const age = userService.calculateAge(birthYear);
      expect(age).toBeGreaterThan(25);
    });

    it('should return 0 for current year', () => {
      const currentYear = new Date().getFullYear();
      const age = userService.calculateAge(currentYear);
      expect(age).toBe(0);
    });
  });
});
```

**Jest Matchers:**
```typescript
// Equality
expect(value).toBe(4);              // Strict equality (===)
expect(value).toEqual({a: 1});      // Deep equality

// Truthiness
expect(value).toBeTruthy();
expect(value).toBeFalsy();
expect(value).toBeNull();
expect(value).toBeUndefined();

// Numbers
expect(value).toBeGreaterThan(3);
expect(value).toBeLessThan(5);
expect(value).toBeGreaterThanOrEqual(4);
expect(value).toBeLessThanOrEqual(4);

// Strings
expect(str).toMatch(/pattern/);
expect(str).toContain('substring');

// Arrays
expect(array).toContain(item);
expect(array).toHaveLength(3);

// Objects
expect(obj).toHaveProperty('key');
expect(obj).toHaveProperty('key', 'value');
```

#### 4. Testing Async Functions

**Test Async Function:**
```typescript
// Service dengan async function
async findById(id: number): Promise<User | null> {
  // ... implementation
}

// Test
describe('findById', () => {
  it('should return user when found', async () => {
    const user = await userService.findById(1);
    expect(user).not.toBeNull();
    expect(user?.id).toBe(1);
  });

  it('should return null when not found', async () => {
    const user = await userService.findById(999);
    expect(user).toBeNull();
  });
});
```

**Test dengan Promises:**
```typescript
it('should handle errors', async () => {
  await expect(userService.findById(-1)).rejects.toThrow();
});
```

#### 5. Mocking Dependencies

**Mock External Dependencies:**
```typescript
// Mock database call
jest.mock('../../database/models/User', () => ({
  findByPk: jest.fn(),
}));

import User from '../../database/models/User';

describe('UserService', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('should return user from database', async () => {
    const mockUser = { id: 1, name: 'John' };
    (User.findByPk as jest.Mock).mockResolvedValue(mockUser);

    const result = await userService.findById(1);
    expect(result).toEqual(mockUser);
    expect(User.findByPk).toHaveBeenCalledWith(1);
  });
});
```

**Mock Functions:**
```typescript
// Mock function
const mockFunction = jest.fn();
mockFunction.mockReturnValue('value');
mockFunction.mockResolvedValue(Promise.resolve('value'));

// Check calls
expect(mockFunction).toHaveBeenCalled();
expect(mockFunction).toHaveBeenCalledWith('arg');
expect(mockFunction).toHaveBeenCalledTimes(2);
```

#### 6. Test Setup dan Teardown

**Before/After Hooks:**
```typescript
describe('UserService', () => {
  beforeAll(() => {
    // Run once before all tests
    // Setup database connection, etc.
  });

  afterAll(() => {
    // Run once after all tests
    // Cleanup database, etc.
  });

  beforeEach(() => {
    // Run before each test
    // Reset state, etc.
  });

  afterEach(() => {
    // Run after each test
    // Cleanup, etc.
  });
});
```

### Latihan 6.3

```typescript
// Buat unit test untuk:
// 1. Function calculateTotal(products: Product[]): number
//    - Test dengan array kosong
//    - Test dengan satu product
//    - Test dengan multiple products
//    - Test dengan harga negatif (error handling)

// 2. Function validateEmail(email: string): boolean
//    - Test dengan email valid
//    - Test dengan email invalid
//    - Test dengan email kosong

// 3. Function formatCurrency(amount: number): string
//    - Test dengan angka positif
//    - Test dengan angka negatif
//    - Test dengan nol

// Tulis test code di bawah ini:
```

---

## 📚 Materi 6.4: Integration Testing

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Integration test test beberapa komponen bekerja bersama
- Test API endpoints dengan database
- Test authentication flow
- Test error scenarios
- Setup dan cleanup database untuk testing

### Materi Pembelajaran

#### 1. Setup Integration Test

**Install Supertest:**
```bash
npm install -D supertest @types/supertest
```

**Setup Test Database:**
```typescript
// src/__tests__/setup.ts
import { Sequelize } from 'sequelize';

const sequelize = new Sequelize({
  dialect: 'sqlite',
  storage: ':memory:', // In-memory database untuk testing
  logging: false,
});

beforeAll(async () => {
  await sequelize.sync({ force: true });
});

afterAll(async () => {
  await sequelize.close();
});
```

#### 2. Testing API Endpoints

**Test GET Endpoint:**
```typescript
// src/__tests__/integration/users.test.ts
import request from 'supertest';
import app from '../../server';

describe('GET /api/users', () => {
  it('should return all users', async () => {
    const response = await request(app)
      .get('/api/users')
      .expect(200);

    expect(response.body).toHaveProperty('data');
    expect(Array.isArray(response.body.data)).toBe(true);
  });

  it('should return paginated results', async () => {
    const response = await request(app)
      .get('/api/users?page=1&limit=10')
      .expect(200);

    expect(response.body).toHaveProperty('pagination');
    expect(response.body.pagination.page).toBe(1);
    expect(response.body.pagination.limit).toBe(10);
  });
});
```

**Test POST Endpoint:**
```typescript
describe('POST /api/users', () => {
  it('should create a new user', async () => {
    const userData = {
      name: 'John Doe',
      email: 'john@example.com',
      age: 25
    };

    const response = await request(app)
      .post('/api/users')
      .send(userData)
      .expect(201);

    expect(response.body.data).toHaveProperty('id');
    expect(response.body.data.name).toBe(userData.name);
    expect(response.body.data.email).toBe(userData.email);
  });

  it('should return 400 for invalid data', async () => {
    const invalidData = {
      name: '', // Invalid: empty name
      email: 'invalid-email', // Invalid email format
    };

    const response = await request(app)
      .post('/api/users')
      .send(invalidData)
      .expect(400);

    expect(response.body).toHaveProperty('error');
  });
});
```

**Test dengan Authentication:**
```typescript
describe('Protected Routes', () => {
  let token: string;

  beforeAll(async () => {
    // Login untuk mendapatkan token
    const loginResponse = await request(app)
      .post('/api/auth/login')
      .send({
        email: 'test@example.com',
        password: 'password123'
      });

    token = loginResponse.body.data.token;
  });

  it('should access protected route with valid token', async () => {
    const response = await request(app)
      .get('/api/users')
      .set('Authorization', `Bearer ${token}`)
      .expect(200);

    expect(response.body).toHaveProperty('data');
  });

  it('should return 401 without token', async () => {
    await request(app)
      .get('/api/users')
      .expect(401);
  });

  it('should return 401 with invalid token', async () => {
    await request(app)
      .get('/api/users')
      .set('Authorization', 'Bearer invalid-token')
      .expect(401);
  });
});
```

#### 3. Test Error Scenarios

**Test 404 Not Found:**
```typescript
describe('GET /api/users/:id', () => {
  it('should return 404 for non-existent user', async () => {
    await request(app)
      .get('/api/users/99999')
      .expect(404);
  });
});
```

**Test Validation Errors:**
```typescript
describe('POST /api/users', () => {
  it('should return validation errors', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({}) // Empty body
      .expect(400);

    expect(response.body).toHaveProperty('details');
    expect(Array.isArray(response.body.details)).toBe(true);
  });
});
```

#### 4. Database Setup dan Cleanup

**Setup Test Database:**
```typescript
// src/__tests__/helpers/database.ts
import { Sequelize } from 'sequelize';

export const setupTestDatabase = async () => {
  const sequelize = new Sequelize({
    dialect: 'sqlite',
    storage: ':memory:',
    logging: false,
  });

  await sequelize.sync({ force: true });
  return sequelize;
};

export const cleanupTestDatabase = async (sequelize: Sequelize) => {
  await sequelize.close();
};
```

**Menggunakan di Test:**
```typescript
import { setupTestDatabase, cleanupTestDatabase } from '../helpers/database';

describe('User Integration Tests', () => {
  let sequelize: Sequelize;

  beforeAll(async () => {
    sequelize = await setupTestDatabase();
  });

  afterAll(async () => {
    await cleanupTestDatabase(sequelize);
  });

  beforeEach(async () => {
    // Clean database before each test
    await User.destroy({ where: {}, force: true });
  });
});
```

### Latihan 6.4

```typescript
// Buat integration test untuk:
// 1. GET /api/products
//    - Test return semua products
//    - Test pagination
//    - Test filter by category

// 2. POST /api/products
//    - Test create product berhasil
//    - Test validation errors
//    - Test dengan authentication

// 3. PUT /api/products/:id
//    - Test update berhasil
//    - Test 404 jika tidak ditemukan
//    - Test dengan authentication

// 4. DELETE /api/products/:id
//    - Test delete berhasil
//    - Test 404 jika tidak ditemukan
//    - Test dengan authentication

// Tulis test code di bawah ini:
```

---

## 📚 Materi 6.5: Test Coverage dan Best Practices

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Test coverage menunjukkan bagian kode yang sudah di-test
- Target coverage yang realistis (80-90%)
- Best practices untuk menulis test yang baik
- Maintainable tests
- CI/CD integration

### Materi Pembelajaran

#### 1. Test Coverage

**Menjalankan Coverage:**
```bash
npm run test:coverage
```

**Coverage Report:**
```
File                | % Stmts | % Branch | % Funcs | % Lines
--------------------|---------|----------|---------|--------
user.service.ts     |   85.71 |    80.00 |   83.33 |   85.71
user.controller.ts  |   90.00 |    75.00 |   88.89 |   90.00
```

**Target Coverage:**
- **80-90%** adalah target yang baik
- Jangan terlalu fokus ke 100% (bisa jadi over-testing)
- Fokus ke critical paths dan business logic

**Meningkatkan Coverage:**
- Test semua branches (if/else)
- Test error cases
- Test edge cases
- Test boundary conditions

#### 2. Best Practices untuk Testing

**1. Test Naming:**
```typescript
// ✅ Baik - jelas dan deskriptif
it('should return user when valid ID is provided', () => {});
it('should return 404 when user does not exist', () => {});
it('should return validation error when email is invalid', () => {});

// ❌ Buruk - tidak jelas
it('test user', () => {});
it('works', () => {});
```

**2. Arrange-Act-Assert Pattern:**
```typescript
it('should calculate total correctly', () => {
  // Arrange: Setup test data
  const products = [
    { price: 10 },
    { price: 20 },
    { price: 30 }
  ];

  // Act: Execute function
  const total = calculateTotal(products);

  // Assert: Verify result
  expect(total).toBe(60);
});
```

**3. One Assertion per Test:**
```typescript
// ✅ Baik - satu konsep per test
it('should return user with correct ID', () => {
  const user = getUserById(1);
  expect(user.id).toBe(1);
});

it('should return user with correct name', () => {
  const user = getUserById(1);
  expect(user.name).toBe('John');
});

// ❌ Buruk - terlalu banyak assertions
it('should return correct user', () => {
  const user = getUserById(1);
  expect(user.id).toBe(1);
  expect(user.name).toBe('John');
  expect(user.email).toBe('john@example.com');
  // ... terlalu banyak
});
```

**4. Test Independence:**
```typescript
// ✅ Baik - setiap test independent
beforeEach(() => {
  // Reset state sebelum setiap test
  clearDatabase();
});

// ❌ Buruk - test bergantung pada test lain
it('test 1: create user', () => {
  // Creates user
});

it('test 2: update user', () => {
  // Depends on test 1
});
```

**5. Mock External Dependencies:**
```typescript
// ✅ Baik - mock database calls
jest.mock('../database/models/User');

// ❌ Buruk - menggunakan real database di unit test
// (Bisa lambat dan tidak reliable)
```

#### 3. Test Organization

**Struktur yang Baik:**
```typescript
describe('UserService', () => {
  describe('findById', () => {
    it('should return user when found', () => {});
    it('should return null when not found', () => {});
  });

  describe('create', () => {
    it('should create user successfully', () => {});
    it('should return error for duplicate email', () => {});
  });
});
```

**Grouping Tests:**
- Group by function/method
- Group by feature
- Group by scenario (happy path, error cases)

#### 4. CI/CD Integration

**GitHub Actions Example:**
```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v2
      
      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '20'
      
      - name: Install dependencies
        run: npm install
      
      - name: Run tests
        run: npm test
      
      - name: Generate coverage
        run: npm run test:coverage
```

**Benefits:**
- Automatic testing pada setiap push
- Catch bugs sebelum merge
- Confidence saat deploy

### Latihan 6.5

```
Review test code Anda:
1. Apakah test names jelas dan deskriptif?
2. Apakah menggunakan Arrange-Act-Assert pattern?
3. Apakah test independent (tidak bergantung pada test lain)?
4. Apakah external dependencies di-mock?
5. Apakah coverage sudah mencapai target (80%+)?
6. Apakah ada test untuk error cases?

Identifikasi area yang perlu diperbaiki:
```

---

## 📚 Materi 6.6: Advanced Testing Techniques

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Test data factories untuk generate test data
- Test helpers untuk reusable test utilities
- Snapshot testing untuk UI/response comparison
- Performance testing
- Contract testing

### Materi Pembelajaran

#### 1. Test Data Factories

**Factory Pattern untuk Test Data:**
```typescript
// src/__tests__/factories/user.factory.ts
import { User } from '../../components/users/user.types';

export const createUserFactory = (overrides?: Partial<User>): User => {
  return {
    id: 1,
    name: 'John Doe',
    email: 'john@example.com',
    age: 25,
    createdAt: new Date(),
    ...overrides
  };
};

// Usage
const user = createUserFactory({ name: 'Jane Doe' });
const adminUser = createUserFactory({ 
  name: 'Admin',
  role: 'admin' 
});
```

**Benefits:**
- Reusable test data
- Easy to modify
- Consistent test data

#### 2. Test Helpers

**Reusable Test Helpers:**
```typescript
// src/__tests__/helpers/api.helper.ts
import request from 'supertest';
import app from '../../server';

export const createTestUser = async (userData?: Partial<User>) => {
  const defaultData = {
    name: 'Test User',
    email: `test${Date.now()}@example.com`,
    age: 25
  };

  const response = await request(app)
    .post('/api/users')
    .send({ ...defaultData, ...userData });

  return response.body.data;
};

export const getAuthToken = async () => {
  const user = await createTestUser();
  
  const response = await request(app)
    .post('/api/auth/login')
    .send({
      email: user.email,
      password: 'password123'
    });

  return response.body.data.token;
};
```

#### 3. Snapshot Testing

**Snapshot untuk Response:**
```typescript
it('should return correct user structure', () => {
  const user = getUserById(1);
  
  expect(user).toMatchSnapshot();
});

// Snapshot akan dibuat di __snapshots__/
// Bisa di-update dengan: npm test -- -u
```

**Use Cases:**
- Test response structure
- Test configuration objects
- Test error messages

#### 4. Performance Testing

**Test Response Time:**
```typescript
it('should respond within acceptable time', async () => {
  const start = Date.now();
  
  await request(app)
    .get('/api/users')
    .expect(200);
  
  const duration = Date.now() - start;
  expect(duration).toBeLessThan(500); // Less than 500ms
});
```

**Load Testing:**
- Gunakan tools seperti Artillery, k6
- Test dengan banyak concurrent requests
- Monitor performance metrics

### Latihan 6.6

```typescript
// Implementasikan:
// 1. Buat factory untuk Product test data
// 2. Buat helper untuk create test product
// 3. Buat helper untuk get auth token
// 4. Test response time untuk GET /api/products
// 5. Buat snapshot test untuk product response structure

// Tulis code di bawah ini:
```

---

## ✅ Review Modul 6

### Checklist Pemahaman

Setelah modul ini, pastikan peserta memahami:

- [ ] Mengapa testing penting untuk development
- [ ] Menggunakan Postman untuk manual testing
- [ ] Membuat collection dan environment di Postman
- [ ] Menulis unit test dengan Jest
- [ ] Menulis integration test dengan Supertest
- [ ] Mocking dependencies untuk unit test
- [ ] Test coverage dan best practices
- [ ] Advanced testing techniques

### Latihan Akhir Modul

**Final Project Testing:**
1. **Postman Collection:**
   - Buat collection lengkap untuk semua endpoints
   - Setup environment dengan variables
   - Tambahkan tests untuk setiap request
   - Test authentication flow

2. **Unit Tests:**
   - Test semua service functions
   - Test utility functions
   - Mock external dependencies
   - Achieve 80%+ coverage

3. **Integration Tests:**
   - Test semua API endpoints
   - Test authentication flow
   - Test error scenarios
   - Setup dan cleanup database

**Requirements:**
- Minimal 10 unit tests
- Minimal 5 integration tests
- Postman collection dengan semua endpoints
- Test coverage minimal 80%
- Semua tests passing

---

## 🎯 Persiapan untuk Modul Berikutnya

Modul berikutnya akan membahas praktik dengan boilerplate repository. Pastikan peserta sudah nyaman dengan:
- ✅ Testing dengan Postman untuk manual testing
- ✅ Unit testing dengan Jest
- ✅ Integration testing
- ✅ Test coverage dan best practices

**Catatan Instruktur:** 
- Setelah belajar testing, peserta akan praktik dengan boilerplate
- Saat praktik, dorong peserta untuk langsung test kode yang dibuat
- Testing akan membantu memastikan kualitas kode selama development

---

## 🎯 Kesimpulan Modul Testing

Selamat! Anda telah mempelajari:

✅ **Manual Testing:**
- Menggunakan Postman untuk test API
- Collection dan environment management
- Automated tests di Postman

✅ **Automated Testing:**
- Unit testing dengan Jest
- Integration testing dengan Supertest
- Mocking dependencies

✅ **Best Practices:**
- Test organization
- Test coverage
- CI/CD integration

**Langkah Selanjutnya:**
1. Terus praktik dengan menulis tests untuk setiap fitur baru
2. Explore testing tools lainnya (Cypress untuk E2E, dll)
3. Pelajari TDD (Test-Driven Development)
4. Setup CI/CD untuk automatic testing
5. Baca dokumentasi Jest dan Supertest lebih dalam

**Sumber Belajar Tambahan:**
- [Jest Documentation](https://jestjs.io/)
- [Supertest Documentation](https://github.com/visionmedia/supertest)
- [Postman Documentation](https://learning.postman.com/)
- [Testing Best Practices](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library)

**Selamat testing! 🧪**
