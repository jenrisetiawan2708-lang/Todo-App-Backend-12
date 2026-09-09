# Todo App — Backend

RESTful API untuk aplikasi Todo, dibangun dengan Express.js dan TypeScript, menggunakan autentikasi JWT dan database MySQL.

## Tech Stack
- Express.js
- TypeScript
- MySQL (mysql2)
- JWT (jsonwebtoken)
- bcrypt

## Setup Project

1. Clone repository ini
```bash
git clone https://github.com/username/todo-app-backend.git
cd todo-app-backend
```

2. Install dependencies
```bash
npm install
```

3. Buat file `.env` di root folder, isi sesuai contoh di `.env.example`:
```
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=
DB_NAME=todo_db
PORT=5000
JWT_SECRET=your_jwt_secret_key
```

## Import Database

1. Buka phpMyAdmin (via XAMPP)
2. Buat database baru `todo_db`, atau jalankan query berikut di tab SQL:
```sql
CREATE DATABASE todo_db;
USE todo_db;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(100) UNIQUE NOT NULL,
  email VARCHAR(150) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL
);

CREATE TABLE todos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  task VARCHAR(255) NOT NULL,
  is_completed BOOLEAN DEFAULT FALSE,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

## Menjalankan Server

```bash
npm run dev
```

Server berjalan di `http://localhost:5000` (atau sesuai `PORT` di `.env`).

## API Endpoints

| Method | Endpoint             | Deskripsi                          | Middleware              |
|--------|-----------------------|-------------------------------------|--------------------------|
| POST   | `/api/auth/register`  | Mendaftarkan pengguna baru          | Validator                |
| POST   | `/api/auth/login`     | Login, mengembalikan token JWT      | Validator                |
| GET    | `/api/todos`          | Ambil semua todo milik user login   | Auth                     |
| POST   | `/api/todos`          | Tambah todo baru                    | Auth, Validator          |

### Contoh Request

**Register**
```json
POST /api/auth/register
{
  "username": "firo",
  "email": "firo@example.com",
  "password": "password123"
}
```

**Login**
```json
POST /api/auth/login
{
  "username": "firo",
  "password": "password123"
}
```

**Tambah Todo** (perlu header `Authorization: Bearer <token>`)
```json
POST /api/todos
{
  "task": "Belajar TypeScript"
}
```