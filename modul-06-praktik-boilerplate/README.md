# Modul 6: Praktik dengan Boilerplate

## 🎯 Tujuan Pembelajaran

Setelah modul ini, peserta dapat:
- Memahami struktur project boilerplate Express TypeScript
- Setup dan menjalankan boilerplate
- Memahami arsitektur dan best practices yang digunakan
- Menambahkan fitur baru ke boilerplate
- Menerapkan pengetahuan dari modul sebelumnya ke project nyata

## ⏱️ Durasi: 2-3 Sesi

---

## 📚 Materi 5.1: Setup Boilerplate

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Setup project dari repository GitHub
- Install dependencies
- Konfigurasi environment variables
- Setup database
- Menjalankan project

### Materi Pembelajaran

#### 1. Clone Repository

```bash
# Clone repository
git clone https://github.com/teramuza/boilerplate-express-ts.git
cd boilerplate-express-ts

# Atau jika sudah ada, pull latest changes
git pull origin main
```

#### 2. Install Dependencies

```bash
# Install semua dependencies
npm install

# Atau dengan yarn
yarn install
```

#### 3. Setup Environment Variables

```bash
# Copy file contoh environment
cp env.example .env

# Edit .env file dengan konfigurasi yang sesuai
# Minimal yang perlu diisi:
# - DB_HOST
# - DB_NAME
# - DB_USER
# - DB_PASSWORD
# - JWT_SECRET
# - PORT
```

**Contoh .env:**
```env
NODE_ENV=development
PORT=3000

# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp_dev
DB_USER=postgres
DB_PASSWORD=password

# JWT
JWT_SECRET=your-super-secret-key-change-in-production
JWT_EXPIRES_IN=7d

# CORS
CORS_ORIGIN=http://localhost:3000
```

#### 4. Setup Database

```bash
# Buat database (jika belum ada)
npm run db:create

# Run migrations
npm run db:migrate

# (Optional) Run seeders untuk data contoh
npm run db:seed
```

#### 5. Menjalankan Project

```bash
# Development mode (dengan hot reload)
npm run dev

# Build untuk production
npm run build

# Run production build
npm run serve
```

#### 6. Test API

```bash
# Test health check endpoint
curl http://localhost:3000/health

# Expected response:
# { "status": "ok", "timestamp": "2024-..." }
```

### Latihan 5.1

```bash
# Setup boilerplate di komputer Anda:
# 1. Clone repository
# 2. Install dependencies
# 3. Setup .env file
# 4. Setup database
# 5. Run migrations
# 6. Start development server
# 7. Test health check endpoint

# Catat langkah-langkah yang Anda lakukan:
```

---

## 📚 Materi 5.2: Memahami Struktur Project

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Struktur folder yang terorganisir
- Separation of concerns
- Component-based architecture
- Base classes untuk reusability
- Configuration management

### Materi Pembelajaran

#### 1. Struktur Folder Utama

```
boilerplate-express-ts/
├── src/
│   ├── components/          # Feature modules (modular architecture)
│   │   ├── example/         # Contoh CRUD tanpa database
│   │   ├── users/           # User CRUD dengan database
│   │   └── routes.ts        # Main router yang mengumpulkan semua routes
│   ├── database/            # Database related files
│   │   ├── models/         # Sequelize models
│   │   ├── migrations/     # Database migrations
│   │   └── seeders/        # Database seeders
│   ├── config/             # Configuration files
│   │   └── database.ts     # Database configuration
│   ├── lib/                # Library/base classes
│   │   └── base/          # Base classes (BaseController, BaseService)
│   ├── utils/              # Utility functions
│   ├── middlewares/        # Express middlewares
│   ├── constants/          # Constants & error codes
│   └── server.ts           # Entry point aplikasi
├── .env                    # Environment variables (gitignored)
├── package.json
├── tsconfig.json           # TypeScript configuration
└── README.md
```

#### 2. Component Structure (Feature Module)

Setiap feature di `components/` memiliki struktur:

```
components/
└── users/
    ├── user.controller.ts    # Route handlers
    ├── user.service.ts        # Business logic
    ├── user.routes.ts         # Route definitions
    ├── user.types.ts          # TypeScript types/interfaces
    └── user.validator.ts      # Validation logic (optional)
```

**Penjelasan:**
- **Controller**: Menangani HTTP request/response
- **Service**: Business logic (bebas dari HTTP concerns)
- **Routes**: Definisi routes dan middleware
- **Types**: TypeScript interfaces untuk type safety
- **Validator**: Validation logic (bisa juga di middleware)

#### 3. Base Classes

```typescript
// src/lib/base/BaseController.ts
// Base class untuk semua controllers
// Menyediakan helper methods untuk response handling

// src/lib/base/BaseService.ts
// Base class untuk semua services
// Menyediakan common CRUD operations
```

**Keuntungan Base Classes:**
- Code reusability
- Consistent response format
- Less boilerplate code
- Easier maintenance

#### 4. Configuration Management

```typescript
// src/config/database.ts
// Centralized database configuration
// Menggunakan environment variables

// src/config/app.ts
// Application configuration
// Port, environment, dll
```

#### 5. Middleware Organization

```
middlewares/
├── auth.ts              # Authentication middleware
├── validation.ts        # Request validation
├── errorHandler.ts     # Error handling middleware
└── rateLimiter.ts      # Rate limiting
```

### Latihan 5.2

```typescript
// Jelajahi struktur project boilerplate:
// 1. Baca file src/server.ts - bagaimana server diinisialisasi?
// 2. Baca file src/components/routes.ts - bagaimana routes diorganisir?
// 3. Baca salah satu component (misalnya users/) - bagaimana strukturnya?
// 4. Baca BaseController - apa saja helper methods yang tersedia?
// 5. Baca errorHandler middleware - bagaimana error handling dilakukan?

// Tuliskan temuan Anda:
```

---

## 📚 Materi 5.3: Memahami Arsitektur

### Penjelasan untuk Instruktur

**Konsep Penting:**
- MVC-like pattern (Model-View-Controller, tapi tanpa View)
- Service layer pattern
- Dependency injection (simple)
- Error handling strategy
- Response standardization

### Materi Pembelajaran

#### 1. Request Flow

```
Client Request
    ↓
Express Middleware (auth, validation, dll)
    ↓
Route Handler (Controller)
    ↓
Service Layer (Business Logic)
    ↓
Database (via Sequelize Model)
    ↓
Response (Controller)
    ↓
Client Response
```

#### 2. Controller Pattern

```typescript
// Controller hanya menangani HTTP concerns
export class UserController {
  private userService: UserService;

  constructor() {
    this.userService = new UserService(); // Dependency injection
  }

  getAllUsers = async (req: Request, res: Response) => {
    // 1. Extract data dari request (query params, dll)
    // 2. Panggil service
    // 3. Return response
  };
}
```

**Prinsip:**
- Controller tidak boleh berisi business logic
- Controller hanya mengatur HTTP request/response
- Business logic ada di Service layer

#### 3. Service Pattern

```typescript
// Service berisi business logic
export class UserService {
  async findAll(page: number, limit: number) {
    // Business logic di sini
    // Bisa panggil multiple models
    // Bisa melakukan validasi business rules
    // Return data (bukan HTTP response)
  }
}
```

**Prinsip:**
- Service tidak tahu tentang HTTP
- Service bisa digunakan oleh controller, CLI, dll
- Service bisa memanggil service lain

#### 4. Error Handling Strategy

```typescript
// Custom error classes
class AppError extends Error {
  statusCode: number;
}

// Di controller/service, throw error
throw new AppError('User not found', 404);

// Error handler middleware menangkap semua error
app.use(errorHandler);
```

#### 5. Response Standardization

```typescript
// Semua response mengikuti format yang sama
{
  data: { ... },           // Data yang di-return
  message: "Success",      // Message untuk user
  pagination: { ... }      // Jika ada pagination
}

// Error response
{
  error: "Error message",
  details: [ ... ]         // Jika ada validation errors
}
```

### Latihan 5.3

```typescript
// Analisis arsitektur boilerplate:
// 1. Bagaimana request flow dari client sampai database?
// 2. Apa perbedaan antara Controller dan Service?
// 3. Bagaimana error handling dilakukan?
// 4. Bagaimana response di-standardize?
// 5. Apa keuntungan arsitektur seperti ini?

// Tuliskan analisis Anda:
```

---

## 📚 Materi 5.4: Menambahkan Fitur Baru

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Mengikuti struktur yang sudah ada
- Membuat component baru dengan struktur yang sama
- Menggunakan base classes jika memungkinkan
- Menambahkan routes ke main router
- Membuat migration untuk database (jika perlu)

### Materi Pembelajaran

#### 1. Langkah-langkah Menambahkan Fitur Baru

**Contoh: Menambahkan Products Feature**

**Step 1: Buat folder component**
```bash
mkdir -p src/components/products
```

**Step 2: Buat types**
```typescript
// src/components/products/product.types.ts
export interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
  inStock: boolean;
  createdAt?: Date;
  updatedAt?: Date;
}

export interface CreateProductDto {
  name: string;
  price: number;
  category: string;
  inStock?: boolean;
}

export interface UpdateProductDto {
  name?: string;
  price?: number;
  category?: string;
  inStock?: boolean;
}
```

**Step 3: Buat Service**
```typescript
// src/components/products/product.service.ts
import { CreateProductDto, UpdateProductDto, Product } from './product.types';
import ProductModel from '../../database/models/Product';

export class ProductService {
  async findAll(page: number = 1, limit: number = 10) {
    const offset = (page - 1) * limit;
    const { count, rows } = await ProductModel.findAndCountAll({
      limit,
      offset
    });

    return {
      data: rows,
      pagination: {
        page,
        limit,
        total: count,
        totalPages: Math.ceil(count / limit)
      }
    };
  }

  async findById(id: number): Promise<Product | null> {
    return ProductModel.findByPk(id);
  }

  async create(productData: CreateProductDto): Promise<Product> {
    return ProductModel.create(productData);
  }

  async update(id: number, productData: UpdateProductDto): Promise<Product | null> {
    const product = await ProductModel.findByPk(id);
    if (!product) return null;

    await product.update(productData);
    return product.reload();
  }

  async delete(id: number): Promise<boolean> {
    const product = await ProductModel.findByPk(id);
    if (!product) return false;

    await product.destroy();
    return true;
  }
}
```

**Step 4: Buat Controller**
```typescript
// src/components/products/product.controller.ts
import { Request, Response } from 'express';
import { ProductService } from './product.service';
import { NotFoundError } from '../../utils/errors';

export class ProductController {
  private productService: ProductService;

  constructor() {
    this.productService = new ProductService();
  }

  getAllProducts = async (req: Request, res: Response, next: Function) => {
    try {
      const { page = '1', limit = '10' } = req.query;
      const result = await this.productService.findAll(
        parseInt(page as string),
        parseInt(limit as string)
      );

      res.status(200).json({
        data: result.data,
        pagination: result.pagination,
        message: 'Products retrieved successfully'
      });
    } catch (error) {
      next(error);
    }
  };

  getProductById = async (req: Request, res: Response, next: Function) => {
    try {
      const productId = parseInt(req.params.id);
      const product = await this.productService.findById(productId);

      if (!product) {
        throw new NotFoundError('Product', productId);
      }

      res.status(200).json({
        data: product,
        message: 'Product retrieved successfully'
      });
    } catch (error) {
      next(error);
    }
  };

  createProduct = async (req: Request, res: Response, next: Function) => {
    try {
      const product = await this.productService.create(req.body);

      res.status(201).json({
        data: product,
        message: 'Product created successfully'
      });
    } catch (error) {
      next(error);
    }
  };

  updateProduct = async (req: Request, res: Response, next: Function) => {
    try {
      const productId = parseInt(req.params.id);
      const product = await this.productService.update(productId, req.body);

      if (!product) {
        throw new NotFoundError('Product', productId);
      }

      res.status(200).json({
        data: product,
        message: 'Product updated successfully'
      });
    } catch (error) {
      next(error);
    }
  };

  deleteProduct = async (req: Request, res: Response, next: Function) => {
    try {
      const productId = parseInt(req.params.id);
      const deleted = await this.productService.delete(productId);

      if (!deleted) {
        throw new NotFoundError('Product', productId);
      }

      res.status(200).json({
        message: 'Product deleted successfully'
      });
    } catch (error) {
      next(error);
    }
  };
}
```

**Step 5: Buat Routes**
```typescript
// src/components/products/product.routes.ts
import { Router } from 'express';
import { ProductController } from './product.controller';
import { authenticate } from '../../middlewares/auth';
import { validateCreateProduct, validateUpdateProduct } from './product.validator';

const router = Router();
const productController = new ProductController();

// Public routes
router.get('/', productController.getAllProducts);
router.get('/:id', productController.getProductById);

// Protected routes
router.post('/', authenticate, validateCreateProduct, productController.createProduct);
router.put('/:id', authenticate, validateUpdateProduct, productController.updateProduct);
router.delete('/:id', authenticate, productController.deleteProduct);

export default router;
```

**Step 6: Register Routes**
```typescript
// src/components/routes.ts
import { Router } from 'express';
import userRoutes from './users/user.routes';
import productRoutes from './products/product.routes'; // ← Tambahkan

const router = Router();

router.use('/users', userRoutes);
router.use('/products', productRoutes); // ← Register

export default router;
```

**Step 7: Buat Model (jika perlu database)**
```typescript
// src/database/models/Product.ts
import { DataTypes, Model, Optional } from 'sequelize';
import sequelize from '../config';

interface ProductAttributes {
  id: number;
  name: string;
  price: number;
  category: string;
  inStock: boolean;
  createdAt?: Date;
  updatedAt?: Date;
}

class Product extends Model<ProductAttributes> implements ProductAttributes {
  public id!: number;
  public name!: string;
  public price!: number;
  public category!: string;
  public inStock!: boolean;
  public readonly createdAt!: Date;
  public readonly updatedAt!: Date;
}

Product.init(
  {
    id: {
      type: DataTypes.INTEGER,
      autoIncrement: true,
      primaryKey: true
    },
    name: {
      type: DataTypes.STRING,
      allowNull: false
    },
    price: {
      type: DataTypes.DECIMAL(10, 2),
      allowNull: false
    },
    category: {
      type: DataTypes.STRING,
      allowNull: false
    },
    inStock: {
      type: DataTypes.BOOLEAN,
      defaultValue: true
    }
  },
  {
    sequelize,
    tableName: 'products',
    timestamps: true
  }
);

export default Product;
```

**Step 8: Buat Migration**
```bash
npm run migration:generate -- create-products
```

```typescript
// Edit migration file yang di-generate
export default {
  up: async (queryInterface: QueryInterface) => {
    await queryInterface.createTable('products', {
      id: {
        type: DataTypes.INTEGER,
        autoIncrement: true,
        primaryKey: true
      },
      name: {
        type: DataTypes.STRING,
        allowNull: false
      },
      price: {
        type: DataTypes.DECIMAL(10, 2),
        allowNull: false
      },
      category: {
        type: DataTypes.STRING,
        allowNull: false
      },
      inStock: {
        type: DataTypes.BOOLEAN,
        defaultValue: true
      },
      createdAt: {
        type: DataTypes.DATE,
        allowNull: false
      },
      updatedAt: {
        type: DataTypes.DATE,
        allowNull: false
      }
    });
  },

  down: async (queryInterface: QueryInterface) => {
    await queryInterface.dropTable('products');
  }
};
```

**Step 9: Run Migration**
```bash
npm run db:migrate
```

### Latihan 5.4

```typescript
// Tambahkan fitur baru ke boilerplate:
// Pilih salah satu:
// 1. Categories feature (CRUD untuk categories)
// 2. Orders feature (CRUD untuk orders)
// 3. Reviews feature (CRUD untuk product reviews)

// Ikuti langkah-langkah di atas:
// 1. Buat folder component
// 2. Buat types
// 3. Buat service
// 4. Buat controller
// 5. Buat routes
// 6. Register routes
// 7. Buat model (jika perlu)
// 8. Buat migration (jika perlu)
// 9. Test semua endpoints

// Tulis kode di bawah ini:
```

---

## 📚 Materi 5.5: Best Practices

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Mengikuti struktur yang sudah ada
- Consistent naming conventions
- Proper error handling
- Validation untuk semua input
- Documentation
- Testing (jika ada)

### Materi Pembelajaran

#### 1. Naming Conventions

```typescript
// ✅ Baik
- user.controller.ts
- user.service.ts
- user.routes.ts
- UserController (PascalCase untuk classes)
- userService (camelCase untuk instances)

// ❌ Buruk
- UserController.ts (tidak konsisten dengan file lain)
- user_controller.ts (snake_case tidak digunakan)
```

#### 2. Code Organization

```typescript
// ✅ Baik - Separation of concerns
// Controller hanya HTTP handling
class UserController {
  getAllUsers = async (req, res, next) => {
    // Extract params
    // Call service
    // Return response
  };
}

// Service hanya business logic
class UserService {
  async findAll() {
    // Business logic here
  }
}

// ❌ Buruk - Business logic di controller
class UserController {
  getAllUsers = async (req, res) => {
    // Database query langsung di controller ❌
    const users = await User.findAll();
    // Business logic di controller ❌
    const filtered = users.filter(u => u.isActive);
    res.json(filtered);
  };
}
```

#### 3. Error Handling

```typescript
// ✅ Baik - Gunakan custom error classes
throw new NotFoundError('User', userId);
throw new ValidationError('Invalid input', errors);

// ❌ Buruk - Return error langsung
res.status(404).json({ error: 'Not found' }); // Tidak konsisten
```

#### 4. Validation

```typescript
// ✅ Baik - Validasi di middleware
router.post('/', validateCreateUser, userController.createUser);

// ❌ Buruk - Validasi di controller
createUser = async (req, res) => {
  if (!req.body.name) {
    return res.status(400).json({ error: 'Name required' });
  }
  // ...
};
```

#### 5. Type Safety

```typescript
// ✅ Baik - Gunakan TypeScript types
interface CreateUserDto {
  name: string;
  email: string;
}

createUser = async (req: Request, res: Response) => {
  const userData: CreateUserDto = req.body;
  // ...
};

// ❌ Buruk - Tidak ada types
createUser = async (req, res) => {
  const userData = req.body; // any type
  // ...
};
```

#### 6. Response Format

```typescript
// ✅ Baik - Consistent response format
res.status(200).json({
  data: users,
  message: 'Users retrieved successfully'
});

// ❌ Buruk - Inconsistent format
res.json(users); // Langsung return data
res.json({ users }); // Format berbeda
```

### Latihan 5.5

```typescript
// Review kode yang sudah Anda buat:
// 1. Apakah mengikuti naming conventions?
// 2. Apakah separation of concerns sudah benar?
// 3. Apakah error handling sudah proper?
// 4. Apakah semua input sudah divalidasi?
// 5. Apakah response format sudah konsisten?
// 6. Apakah TypeScript types sudah digunakan dengan baik?

// Identifikasi area yang perlu diperbaiki:
```

---

## 📚 Materi 5.6: Troubleshooting Common Issues

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Common errors dan solusinya
- Debugging techniques
- Logging untuk troubleshooting
- Environment issues

### Materi Pembelajaran

#### 1. Database Connection Issues

**Error:**
```
Error: connect ECONNREFUSED 127.0.0.1:5432
```

**Solusi:**
- Pastikan PostgreSQL running
- Check DB_HOST dan DB_PORT di .env
- Check database credentials

#### 2. Migration Issues

**Error:**
```
Error: Migration failed
```

**Solusi:**
- Check migration file syntax
- Pastikan database sudah dibuat
- Check Sequelize configuration
- Rollback migration jika perlu: `npm run db:migrate:undo`

#### 3. TypeScript Compilation Errors

**Error:**
```
Type 'X' is not assignable to type 'Y'
```

**Solusi:**
- Check type definitions
- Pastikan import types yang benar
- Gunakan type assertions jika perlu (dengan hati-hati)
- Check tsconfig.json settings

#### 4. Module Not Found

**Error:**
```
Cannot find module '@component/users'
```

**Solusi:**
- Check tsconfig.json paths configuration
- Pastikan path alias sudah benar
- Restart TypeScript server di IDE
- Check import paths

#### 5. Port Already in Use

**Error:**
```
Error: listen EADDRINUSE: address already in use :::3000
```

**Solusi:**
- Kill process yang menggunakan port tersebut
- Atau ubah PORT di .env
- Check dengan: `lsof -i :3000` (Mac/Linux)

#### 6. JWT Token Issues

**Error:**
```
Invalid token
```

**Solusi:**
- Check JWT_SECRET di .env
- Pastikan token format benar (Bearer token)
- Check token expiration
- Verify token di jwt.io untuk debugging

### Latihan 5.6

```typescript
// Jika Anda mengalami error:
// 1. Catat error message lengkap
// 2. Check error stack trace
// 3. Cari solusi di dokumentasi atau Google
// 4. Check environment variables
// 5. Check database connection
// 6. Check logs

// Buat catatan troubleshooting Anda:
```

---

## ✅ Review Modul 5

### Checklist Pemahaman

Setelah modul ini, pastikan peserta memahami:

- [ ] Bisa setup boilerplate dari awal
- [ ] Memahami struktur project boilerplate
- [ ] Memahami arsitektur yang digunakan
- [ ] Bisa menambahkan fitur baru mengikuti struktur yang ada
- [ ] Memahami best practices yang digunakan
- [ ] Bisa troubleshoot common issues

### Latihan Akhir Modul

**Final Project:**
Buat aplikasi REST API lengkap menggunakan boilerplate dengan fitur:

1. **Authentication**
   - Register
   - Login
   - Protected routes

2. **Products Management**
   - CRUD products
   - Filter by category
   - Search products

3. **Orders Management**
   - Create order
   - List orders (dengan pagination)
   - Get order by ID
   - Update order status

4. **Database**
   - Products table
   - Orders table
   - OrderItems table (relasi dengan Orders)
   - Proper migrations

5. **Validation & Error Handling**
   - Input validation untuk semua endpoints
   - Proper error messages
   - Consistent response format

**Requirements:**
- Mengikuti struktur boilerplate
- Menggunakan TypeScript dengan proper types
- Menggunakan Sequelize untuk database
- Menggunakan JWT untuk authentication
- Proper error handling
- Code yang clean dan maintainable

---

## 🎯 Kesimpulan Kursus

Selamat! Anda telah menyelesaikan semua modul pembelajaran. Sekarang Anda memiliki:

✅ **Pengetahuan Dasar:**
- JavaScript modern (ES6+)
- TypeScript untuk type safety
- Express.js untuk web framework
- REST API development

✅ **Praktik:**
- Membuat REST API dari awal
- Menggunakan database dengan Sequelize
- Implementasi authentication dengan JWT
- Bekerja dengan boilerplate project

✅ **Best Practices:**
- Struktur project yang baik
- Separation of concerns
- Error handling yang proper
- Code yang maintainable

**Langkah Selanjutnya:**
1. Terus praktik dengan membuat project-project baru
2. Explore fitur-fitur advanced (caching, rate limiting, dll)
3. Pelajari testing (unit test, integration test)
4. Pelajari deployment (Docker, cloud platforms)
5. Baca dokumentasi Express.js dan Sequelize lebih dalam

**Sumber Belajar Tambahan:**
- [Express.js Documentation](https://expressjs.com/)
- [Sequelize Documentation](https://sequelize.org/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [REST API Best Practices](https://restfulapi.net/)

**Selamat coding! 🚀**
