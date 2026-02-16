# Modul 1.5: REST API Development

## 🎯 Tujuan Pembelajaran

Setelah modul ini, peserta dapat:
- Memahami konsep dan prinsip REST API
- Membangun REST API yang lengkap dengan CRUD operations
- Mengimplementasikan validation dan error handling yang proper
- Menggunakan authentication dengan JWT
- Mengintegrasikan database dengan Sequelize ORM
- Menerapkan best practices untuk REST API

## ⏱️ Durasi: 3-4 Sesi

---

## 📚 Materi 5.1: Konsep REST API

### Penjelasan untuk Instruktur

**Konsep Penting:**
- REST (Representational State Transfer) adalah arsitektur untuk web services
- REST API menggunakan HTTP methods untuk operasi CRUD
- Resource-based URLs (nouns, bukan verbs)
- Stateless communication
- JSON sebagai format data standar

### Materi Pembelajaran

#### 1. Prinsip REST API

**REST API mengikuti prinsip-prinsip berikut:**

1. **Resource-Based URLs**
   ```
   ✅ Baik:  GET /users
   ❌ Buruk: GET /getUsers
   
   ✅ Baik:  POST /users
   ❌ Buruk: POST /createUser
   ```

2. **HTTP Methods untuk Operasi**
   ```
   GET    /users        → Ambil semua users
   GET    /users/:id    → Ambil user tertentu
   POST   /users        → Buat user baru
   PUT    /users/:id    → Update seluruh user
   PATCH  /users/:id    → Update sebagian user
   DELETE /users/:id    → Hapus user
   ```

3. **Status Codes**
   ```
   200 OK           → Success
   201 Created      → Resource created
   400 Bad Request  → Invalid input
   401 Unauthorized → Not authenticated
   403 Forbidden    → Not authorized
   404 Not Found    → Resource not found
   500 Server Error → Internal error
   ```

4. **JSON Response Format**
   ```json
   {
     "data": { ... },
     "message": "Success",
     "pagination": { ... }
   }
   ```

#### 2. Struktur URL yang Baik

```typescript
// ✅ Resource-based (baik)
GET    /users
GET    /users/:id
GET    /users/:id/posts
GET    /users/:id/posts/:postId

// ❌ Action-based (buruk)
GET    /getUsers
GET    /getUserById/:id
POST   /createUser
POST   /updateUser/:id
POST   /deleteUser/:id
```

#### 3. Contoh REST API Endpoints

```typescript
// Users Resource
GET    /api/users              → List users
GET    /api/users/:id          → Get user by ID
POST   /api/users              → Create user
PUT    /api/users/:id          → Update user
DELETE /api/users/:id          → Delete user

// Posts Resource (nested)
GET    /api/users/:userId/posts        → Get user's posts
GET    /api/users/:userId/posts/:id    → Get specific post
POST   /api/users/:userId/posts        → Create post for user
```

### Latihan 5.1

```typescript
// Rancang REST API endpoints untuk:
// 1. Products resource (CRUD lengkap)
// 2. Orders resource (CRUD lengkap)
// 3. Nested: Orders memiliki OrderItems
//    - GET /orders/:orderId/items
//    - POST /orders/:orderId/items
//    - dll

// Tulis rancangan endpoints di bawah ini:
```

---

## 📚 Materi 5.2: CRUD Operations Lengkap

### Penjelasan untuk Instruktur

**Konsep Penting:**
- CRUD = Create, Read, Update, Delete
- Setiap operasi harus mengikuti REST principles
- Proper error handling untuk setiap operasi
- Response format yang konsisten
- Validation sebelum create/update

### Materi Pembelajaran

#### 1. Struktur Project yang Terorganisir

```
src/
├── components/
│   └── users/
│       ├── user.controller.ts    # Route handlers
│       ├── user.service.ts       # Business logic
│       ├── user.routes.ts        # Route definitions
│       └── user.types.ts         # TypeScript types
├── middlewares/
│   └── validation.ts
└── server.ts
```

#### 2. User Controller (Route Handlers)

```typescript
// src/components/users/user.controller.ts
import { Request, Response } from 'express';
import { UserService } from './user.service';

export class UserController {
  private userService: UserService;

  constructor() {
    this.userService = new UserService();
  }

  // GET /users
  getAllUsers = async (req: Request, res: Response) => {
    try {
      const { page = '1', limit = '10' } = req.query;
      const users = await this.userService.findAll(
        parseInt(page as string),
        parseInt(limit as string)
      );
      res.status(200).json({
        data: users,
        message: 'Users retrieved successfully'
      });
    } catch (error) {
      res.status(500).json({ error: 'Failed to retrieve users' });
    }
  };

  // GET /users/:id
  getUserById = async (req: Request, res: Response) => {
    try {
      const userId = parseInt(req.params.id);
      const user = await this.userService.findById(userId);
      
      if (!user) {
        return res.status(404).json({ error: 'User not found' });
      }
      
      res.status(200).json({
        data: user,
        message: 'User retrieved successfully'
      });
    } catch (error) {
      res.status(500).json({ error: 'Failed to retrieve user' });
    }
  };

  // POST /users
  createUser = async (req: Request, res: Response) => {
    try {
      const { name, email, age } = req.body;
      const user = await this.userService.create({ name, email, age });
      
      res.status(201).json({
        data: user,
        message: 'User created successfully'
      });
    } catch (error) {
      res.status(400).json({ error: 'Failed to create user' });
    }
  };

  // PUT /users/:id
  updateUser = async (req: Request, res: Response) => {
    try {
      const userId = parseInt(req.params.id);
      const { name, email, age } = req.body;
      
      const user = await this.userService.update(userId, { name, email, age });
      
      if (!user) {
        return res.status(404).json({ error: 'User not found' });
      }
      
      res.status(200).json({
        data: user,
        message: 'User updated successfully'
      });
    } catch (error) {
      res.status(400).json({ error: 'Failed to update user' });
    }
  };

  // DELETE /users/:id
  deleteUser = async (req: Request, res: Response) => {
    try {
      const userId = parseInt(req.params.id);
      const deleted = await this.userService.delete(userId);
      
      if (!deleted) {
        return res.status(404).json({ error: 'User not found' });
      }
      
      res.status(200).json({
        message: 'User deleted successfully'
      });
    } catch (error) {
      res.status(500).json({ error: 'Failed to delete user' });
    }
  };
}
```

#### 3. User Service (Business Logic)

```typescript
// src/components/users/user.service.ts
import { User } from './user.types';

// In-memory storage (nanti akan diganti dengan database)
let users: User[] = [];
let nextId = 1;

export class UserService {
  async findAll(page: number = 1, limit: number = 10): Promise<User[]> {
    const start = (page - 1) * limit;
    const end = start + limit;
    return users.slice(start, end);
  }

  async findById(id: number): Promise<User | null> {
    return users.find(u => u.id === id) || null;
  }

  async create(userData: Omit<User, 'id'>): Promise<User> {
    const newUser: User = {
      id: nextId++,
      ...userData,
      createdAt: new Date()
    };
    users.push(newUser);
    return newUser;
  }

  async update(id: number, userData: Partial<User>): Promise<User | null> {
    const userIndex = users.findIndex(u => u.id === id);
    if (userIndex === -1) return null;
    
    users[userIndex] = {
      ...users[userIndex],
      ...userData,
      updatedAt: new Date()
    };
    
    return users[userIndex];
  }

  async delete(id: number): Promise<boolean> {
    const userIndex = users.findIndex(u => u.id === id);
    if (userIndex === -1) return false;
    
    users.splice(userIndex, 1);
    return true;
  }
}
```

#### 4. User Routes

```typescript
// src/components/users/user.routes.ts
import { Router } from 'express';
import { UserController } from './user.controller';

const router = Router();
const userController = new UserController();

router.get('/', userController.getAllUsers);
router.get('/:id', userController.getUserById);
router.post('/', userController.createUser);
router.put('/:id', userController.updateUser);
router.delete('/:id', userController.deleteUser);

export default router;
```

#### 5. User Types

```typescript
// src/components/users/user.types.ts
export interface User {
  id: number;
  name: string;
  email: string;
  age: number;
  createdAt: Date;
  updatedAt?: Date;
}

export interface CreateUserDto {
  name: string;
  email: string;
  age: number;
}

export interface UpdateUserDto {
  name?: string;
  email?: string;
  age?: number;
}
```

#### 6. Register Routes di Server

```typescript
// src/server.ts
import express from 'express';
import userRoutes from './components/users/user.routes';

const app = express();
app.use(express.json());

// Register routes
app.use('/api/users', userRoutes);

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

### Latihan 5.2

```typescript
// Implementasikan CRUD lengkap untuk Products dengan struktur:
// 1. ProductController dengan semua CRUD methods
// 2. ProductService dengan business logic
// 3. Product routes
// 4. Product types/interfaces
// 5. Register routes di server

// Product interface:
// - id: number
// - name: string
// - price: number
// - category: string
// - inStock: boolean
// - createdAt: Date

// Tulis kode di bawah ini:
```

---

## 📚 Materi 5.3: Validation dan Error Handling

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Validation sangat penting untuk data integrity
- Validasi input sebelum masuk ke database
- Error messages yang jelas untuk client
- Consistent error response format
- Middleware untuk validation

### Materi Pembelajaran

#### 1. Manual Validation

```typescript
// src/middlewares/validation.ts
import { Request, Response, NextFunction } from 'express';

export const validateCreateUser = (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  const { name, email, age } = req.body;
  const errors: string[] = [];

  // Validate name
  if (!name || typeof name !== 'string' || name.trim().length === 0) {
    errors.push('Name is required and must be a non-empty string');
  }

  // Validate email
  if (!email || typeof email !== 'string') {
    errors.push('Email is required');
  } else {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
      errors.push('Email must be a valid email address');
    }
  }

  // Validate age
  if (age === undefined || typeof age !== 'number') {
    errors.push('Age is required and must be a number');
  } else if (age < 0 || age > 150) {
    errors.push('Age must be between 0 and 150');
  }

  if (errors.length > 0) {
    return res.status(400).json({
      error: 'Validation failed',
      details: errors
    });
  }

  next();
};
```

#### 2. Menggunakan Validation Middleware

```typescript
// src/components/users/user.routes.ts
import { Router } from 'express';
import { UserController } from './user.controller';
import { validateCreateUser } from '../../middlewares/validation';

const router = Router();
const userController = new UserController();

router.get('/', userController.getAllUsers);
router.get('/:id', userController.getUserById);
router.post('/', validateCreateUser, userController.createUser); // ← Tambahkan middleware
router.put('/:id', userController.updateUser);
router.delete('/:id', userController.deleteUser);

export default router;
```

#### 3. Reusable Validation Function

```typescript
// src/utils/validators.ts
export const validators = {
  required: (value: any, fieldName: string): string | null => {
    if (value === undefined || value === null || value === '') {
      return `${fieldName} is required`;
    }
    return null;
  },

  isString: (value: any, fieldName: string): string | null => {
    if (typeof value !== 'string') {
      return `${fieldName} must be a string`;
    }
    return null;
  },

  isNumber: (value: any, fieldName: string): string | null => {
    if (typeof value !== 'number' || isNaN(value)) {
      return `${fieldName} must be a number`;
    }
    return null;
  },

  minLength: (value: string, min: number, fieldName: string): string | null => {
    if (value.length < min) {
      return `${fieldName} must be at least ${min} characters`;
    }
    return null;
  },

  isEmail: (value: string, fieldName: string): string | null => {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(value)) {
      return `${fieldName} must be a valid email address`;
    }
    return null;
  },

  min: (value: number, min: number, fieldName: string): string | null => {
    if (value < min) {
      return `${fieldName} must be at least ${min}`;
    }
    return null;
  },

  max: (value: number, max: number, fieldName: string): string | null => {
    if (value > max) {
      return `${fieldName} must be at most ${max}`;
    }
    return null;
  }
};

// Helper function untuk validate
export const validate = (
  rules: Array<() => string | null>
): string[] => {
  const errors: string[] = [];
  rules.forEach(rule => {
    const error = rule();
    if (error) errors.push(error);
  });
  return errors;
};
```

#### 4. Menggunakan Reusable Validators

```typescript
// src/middlewares/validation.ts
import { Request, Response, NextFunction } from 'express';
import { validators, validate } from '../utils/validators';

export const validateCreateUser = (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  const { name, email, age } = req.body;

  const errors = validate([
    () => validators.required(name, 'Name'),
    () => validators.isString(name, 'Name'),
    () => name && validators.minLength(name, 2, 'Name'),
    () => validators.required(email, 'Email'),
    () => validators.isString(email, 'Email'),
    () => email && validators.isEmail(email, 'Email'),
    () => validators.required(age, 'Age'),
    () => validators.isNumber(age, 'Age'),
    () => age !== undefined && validators.min(age, 0, 'Age'),
    () => age !== undefined && validators.max(age, 150, 'Age')
  ]);

  if (errors.length > 0) {
    return res.status(400).json({
      error: 'Validation failed',
      details: errors
    });
  }

  next();
};
```

#### 5. Error Handling yang Lebih Baik

```typescript
// src/utils/errors.ts
export class AppError extends Error {
  statusCode: number;
  isOperational: boolean;

  constructor(message: string, statusCode: number = 500) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true;
    Error.captureStackTrace(this, this.constructor);
  }
}

export class ValidationError extends AppError {
  constructor(message: string, public details?: string[]) {
    super(message, 400);
  }
}

export class NotFoundError extends AppError {
  constructor(resource: string, id?: string | number) {
    super(
      id ? `${resource} with ID ${id} not found` : `${resource} not found`,
      404
    );
  }
}
```

#### 6. Error Handling Middleware

```typescript
// src/middlewares/errorHandler.ts
import { Request, Response, NextFunction } from 'express';
import { AppError } from '../utils/errors';

export const errorHandler = (
  err: Error,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  if (err instanceof AppError) {
    return res.status(err.statusCode).json({
      error: err.message,
      ...(err instanceof ValidationError && { details: err.details })
    });
  }

  // Unexpected errors
  console.error('Unexpected error:', err);
  res.status(500).json({
    error: 'Internal server error',
    ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
  });
};
```

#### 7. Menggunakan Error Classes di Controller

```typescript
// src/components/users/user.controller.ts
import { Request, Response } from 'express';
import { UserService } from './user.service';
import { NotFoundError } from '../../utils/errors';

export class UserController {
  // ...

  getUserById = async (req: Request, res: Response, next: Function) => {
    try {
      const userId = parseInt(req.params.id);
      const user = await this.userService.findById(userId);
      
      if (!user) {
        throw new NotFoundError('User', userId);
      }
      
      res.status(200).json({
        data: user,
        message: 'User retrieved successfully'
      });
    } catch (error) {
      next(error); // Pass ke error handler middleware
    }
  };
}
```

### Latihan 5.3

```typescript
// Implementasikan validation dan error handling untuk Products:
// 1. Buat validators untuk Product (name, price, category, inStock)
// 2. Buat validation middleware untuk create dan update
// 3. Buat custom error classes (ValidationError, NotFoundError)
// 4. Update ProductController untuk menggunakan error classes
// 5. Register error handler middleware di server

// Tulis kode di bawah ini:
```

---

## 📚 Materi 5.4: Authentication dengan JWT

### Penjelasan untuk Instruktur

**Konsep Penting:**
- JWT (JSON Web Token) untuk authentication
- Token-based authentication (stateless)
- Password hashing dengan bcrypt
- Middleware untuk protect routes
- Login dan register endpoints

### Materi Pembelajaran

#### 1. Install Dependencies

```bash
npm install jsonwebtoken bcrypt
npm install -D @types/jsonwebtoken @types/bcrypt
```

#### 2. Auth Service

```typescript
// src/components/auth/auth.service.ts
import bcrypt from 'bcrypt';
import jwt from 'jsonwebtoken';

const JWT_SECRET = process.env.JWT_SECRET || 'your-secret-key';
const JWT_EXPIRES_IN = '7d';

export class AuthService {
  // Hash password
  async hashPassword(password: string): Promise<string> {
    const saltRounds = 10;
    return bcrypt.hash(password, saltRounds);
  }

  // Compare password
  async comparePassword(
    password: string,
    hashedPassword: string
  ): Promise<boolean> {
    return bcrypt.compare(password, hashedPassword);
  }

  // Generate JWT token
  generateToken(userId: number, email: string): string {
    return jwt.sign(
      { userId, email },
      JWT_SECRET,
      { expiresIn: JWT_EXPIRES_IN }
    );
  }

  // Verify JWT token
  verifyToken(token: string): { userId: number; email: string } | null {
    try {
      const decoded = jwt.verify(token, JWT_SECRET) as {
        userId: number;
        email: string;
      };
      return decoded;
    } catch (error) {
      return null;
    }
  }
}
```

#### 3. Auth Controller

```typescript
// src/components/auth/auth.controller.ts
import { Request, Response } from 'express';
import { AuthService } from './auth.service';
import { UserService } from '../users/user.service';
import { AppError } from '../../utils/errors';

export class AuthController {
  private authService: AuthService;
  private userService: UserService;

  constructor() {
    this.authService = new AuthService();
    this.userService = new UserService();
  }

  // POST /auth/register
  register = async (req: Request, res: Response, next: Function) => {
    try {
      const { name, email, password } = req.body;

      // Check if user already exists
      const existingUser = await this.userService.findByEmail(email);
      if (existingUser) {
        throw new AppError('User with this email already exists', 400);
      }

      // Hash password
      const hashedPassword = await this.authService.hashPassword(password);

      // Create user
      const user = await this.userService.create({
        name,
        email,
        password: hashedPassword
      });

      // Generate token
      const token = this.authService.generateToken(user.id, user.email);

      res.status(201).json({
        data: {
          user: {
            id: user.id,
            name: user.name,
            email: user.email
          },
          token
        },
        message: 'User registered successfully'
      });
    } catch (error) {
      next(error);
    }
  };

  // POST /auth/login
  login = async (req: Request, res: Response, next: Function) => {
    try {
      const { email, password } = req.body;

      // Find user
      const user = await this.userService.findByEmail(email);
      if (!user) {
        throw new AppError('Invalid email or password', 401);
      }

      // Check password
      const isPasswordValid = await this.authService.comparePassword(
        password,
        user.password
      );
      if (!isPasswordValid) {
        throw new AppError('Invalid email or password', 401);
      }

      // Generate token
      const token = this.authService.generateToken(user.id, user.email);

      res.status(200).json({
        data: {
          user: {
            id: user.id,
            name: user.name,
            email: user.email
          },
          token
        },
        message: 'Login successful'
      });
    } catch (error) {
      next(error);
    }
  };
}
```

#### 4. Auth Middleware (Protect Routes)

```typescript
// src/middlewares/auth.ts
import { Request, Response, NextFunction } from 'express';
import { AuthService } from '../components/auth/auth.service';
import { AppError } from '../utils/errors';

export const authenticate = (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  try {
    const authHeader = req.headers.authorization;

    if (!authHeader || !authHeader.startsWith('Bearer ')) {
      throw new AppError('No token provided', 401);
    }

    const token = authHeader.substring(7); // Remove 'Bearer ' prefix
    const authService = new AuthService();
    const decoded = authService.verifyToken(token);

    if (!decoded) {
      throw new AppError('Invalid or expired token', 401);
    }

    // Attach user info to request
    (req as any).user = decoded;
    next();
  } catch (error) {
    next(error);
  }
};
```

#### 5. Menggunakan Auth Middleware

```typescript
// src/components/users/user.routes.ts
import { Router } from 'express';
import { UserController } from './user.controller';
import { authenticate } from '../../middlewares/auth';

const router = Router();
const userController = new UserController();

// Public routes
router.get('/', userController.getAllUsers);
router.get('/:id', userController.getUserById);

// Protected routes (require authentication)
router.post('/', authenticate, userController.createUser);
router.put('/:id', authenticate, userController.updateUser);
router.delete('/:id', authenticate, userController.deleteUser);

export default router;
```

### Latihan 5.4

```typescript
// Implementasikan authentication:
// 1. Buat AuthService dengan hash password dan JWT
// 2. Buat AuthController dengan register dan login
// 3. Buat authenticate middleware
// 4. Protect routes yang memerlukan authentication
// 5. Test dengan Postman/curl

// Tulis kode di bawah ini:
```

---

## 📚 Materi 5.5: Database Integration dengan Sequelize

### Penjelasan untuk Instruktur

**Konsep Penting:**
- Sequelize adalah ORM (Object-Relational Mapping) untuk Node.js
- ORM memudahkan bekerja dengan database tanpa SQL langsung
- Models mendefinisikan struktur tabel
- Migrations untuk version control database schema
- Associations untuk relasi antar tabel

### Materi Pembelajaran

#### 1. Setup Sequelize

```bash
npm install sequelize pg pg-hstore
npm install -D sequelize-cli
```

#### 2. Sequelize Configuration

```javascript
// .sequelizerc
const path = require('path');

module.exports = {
  'config': path.resolve('src', 'database', 'config.js'),
  'models-path': path.resolve('src', 'database', 'models'),
  'seeders-path': path.resolve('src', 'database', 'seeders'),
  'migrations-path': path.resolve('src', 'database', 'migrations')
};
```

```javascript
// src/database/config.js
module.exports = {
  development: {
    username: process.env.DB_USER || 'postgres',
    password: process.env.DB_PASSWORD || 'password',
    database: process.env.DB_NAME || 'myapp_dev',
    host: process.env.DB_HOST || 'localhost',
    port: process.env.DB_PORT || 5432,
    dialect: 'postgres'
  }
};
```

#### 3. User Model

```typescript
// src/database/models/User.ts
import { DataTypes, Model, Optional } from 'sequelize';
import sequelize from '../config';

interface UserAttributes {
  id: number;
  name: string;
  email: string;
  password: string;
  age?: number;
  createdAt?: Date;
  updatedAt?: Date;
}

interface UserCreationAttributes extends Optional<UserAttributes, 'id' | 'createdAt' | 'updatedAt'> {}

class User extends Model<UserAttributes, UserCreationAttributes> implements UserAttributes {
  public id!: number;
  public name!: string;
  public email!: string;
  public password!: string;
  public age?: number;
  public readonly createdAt!: Date;
  public readonly updatedAt!: Date;
}

User.init(
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
    email: {
      type: DataTypes.STRING,
      allowNull: false,
      unique: true
    },
    password: {
      type: DataTypes.STRING,
      allowNull: false
    },
    age: {
      type: DataTypes.INTEGER,
      allowNull: true
    }
  },
  {
    sequelize,
    tableName: 'users',
    timestamps: true
  }
);

export default User;
```

#### 4. Update User Service untuk Menggunakan Sequelize

```typescript
// src/components/users/user.service.ts
import User from '../../database/models/User';
import { CreateUserDto, UpdateUserDto } from './user.types';

export class UserService {
  async findAll(page: number = 1, limit: number = 10) {
    const offset = (page - 1) * limit;
    const { count, rows } = await User.findAndCountAll({
      limit,
      offset,
      attributes: { exclude: ['password'] } // Jangan return password
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

  async findById(id: number) {
    return User.findByPk(id, {
      attributes: { exclude: ['password'] }
    });
  }

  async findByEmail(email: string) {
    return User.findOne({ where: { email } });
  }

  async create(userData: CreateUserDto) {
    return User.create(userData);
  }

  async update(id: number, userData: UpdateUserDto) {
    const user = await User.findByPk(id);
    if (!user) return null;
    
    await user.update(userData);
    return user.reload();
  }

  async delete(id: number) {
    const user = await User.findByPk(id);
    if (!user) return false;
    
    await user.destroy();
    return true;
  }
}
```

#### 5. Migration untuk Create Users Table

```typescript
// src/database/migrations/XXXXXX-create-users.ts
import { QueryInterface, DataTypes } from 'sequelize';

export default {
  up: async (queryInterface: QueryInterface) => {
    await queryInterface.createTable('users', {
      id: {
        type: DataTypes.INTEGER,
        autoIncrement: true,
        primaryKey: true
      },
      name: {
        type: DataTypes.STRING,
        allowNull: false
      },
      email: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true
      },
      password: {
        type: DataTypes.STRING,
        allowNull: false
      },
      age: {
        type: DataTypes.INTEGER,
        allowNull: true
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
    await queryInterface.dropTable('users');
  }
};
```

### Latihan 5.5

```typescript
// Implementasikan database integration untuk Products:
// 1. Buat Product model dengan Sequelize
// 2. Buat migration untuk products table
// 3. Update ProductService untuk menggunakan Sequelize
// 4. Run migration dan test CRUD operations

// Tulis kode di bawah ini:
```

---

## ✅ Review Modul 1.5

### Checklist Pemahaman

Sebelum lanjut ke modul berikutnya, pastikan peserta memahami:

- [ ] Memahami konsep dan prinsip REST API
- [ ] Bisa membuat CRUD operations yang lengkap
- [ ] Bisa mengimplementasikan validation
- [ ] Bisa mengimplementasikan error handling yang proper
- [ ] Memahami authentication dengan JWT
- [ ] Bisa mengintegrasikan database dengan Sequelize

### Latihan Akhir Modul

Buat REST API lengkap dengan fitur berikut:

**Requirements:**
1. **Products Resource** dengan CRUD lengkap
2. **Authentication** dengan JWT (register, login)
3. **Protected Routes** untuk create/update/delete products
4. **Validation** untuk semua input
5. **Database Integration** dengan Sequelize
6. **Error Handling** yang proper
7. **Pagination** untuk list endpoints

**Tech Stack:**
- Express.js + TypeScript
- Sequelize + PostgreSQL
- JWT untuk authentication
- bcrypt untuk password hashing

---

## 🎯 Persiapan untuk Modul Berikutnya

Modul berikutnya (1.6) akan membahas Testing API. Pastikan peserta sudah nyaman dengan:
- ✅ REST API development
- ✅ CRUD operations
- ✅ Authentication
- ✅ Database integration
- ✅ Error handling dan validation

**Catatan Instruktur:** 
- Modul ini adalah modul terpanjang dan paling penting
- Pastikan peserta memahami konsep sebelum praktik
- Latihan praktik sangat penting di sini
- Siapkan waktu ekstra untuk troubleshooting
- Setelah ini, peserta akan belajar testing untuk memastikan API yang dibuat bekerja dengan benar
