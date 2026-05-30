Tentu. Saya akan buatkan contoh sederhana dengan stack:

* React (Frontend)
* Node.js + Express (Backend)
* MySQL (Database)
* bcrypt untuk hash password

## 1. Struktur Project

```text
my-app/
├── backend/
│   ├── server.js
│   ├── db.js
│   ├── routes/
│   │   └── users.js
│   └── package.json
│
└── frontend/
    ├── src/
    │   ├── App.jsx
    │   ├── UserForm.jsx
    │   └── UserTable.jsx
    └── package.json
```

---

# 2. Database MySQL

Buat database:

```sql
CREATE DATABASE belajar_react;
USE belajar_react;
```

Buat tabel users:

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama_lengkap VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    no_hp VARCHAR(20),
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# 3. Backend Express

Install dependency:

```bash
npm init -y

npm install express mysql2 cors bcrypt
npm install nodemon -D
```

---

## db.js

```javascript
const mysql = require("mysql2");

const db = mysql.createPool({
  host: "localhost",
  user: "root",
  password: "",
  database: "belajar_react",
});

module.exports = db.promise();
```

---

## routes/users.js

```javascript
const express = require("express");
const bcrypt = require("bcrypt");
const router = express.Router();
const db = require("../db");


// GET ALL USERS
router.get("/", async (req, res) => {
  try {
    const [rows] = await db.query(
      "SELECT id,nama_lengkap,email,no_hp,created_at FROM users"
    );

    res.json(rows);
  } catch (err) {
    res.status(500).json(err);
  }
});


// GET USER BY ID
router.get("/:id", async (req, res) => {
  try {
    const [rows] = await db.query(
      "SELECT id,nama_lengkap,email,no_hp FROM users WHERE id=?",
      [req.params.id]
    );

    res.json(rows[0]);
  } catch (err) {
    res.status(500).json(err);
  }
});


// CREATE USER
router.post("/", async (req, res) => {
  try {
    const { nama_lengkap, email, no_hp, password } = req.body;

    const hashPassword = await bcrypt.hash(password, 10);

    await db.query(
      `INSERT INTO users
      (nama_lengkap,email,no_hp,password)
      VALUES (?,?,?,?)`,
      [nama_lengkap, email, no_hp, hashPassword]
    );

    res.json({
      success: true,
      message: "User berhasil dibuat",
    });
  } catch (err) {
    res.status(500).json(err);
  }
});


// UPDATE USER
router.put("/:id", async (req, res) => {
  try {
    const { nama_lengkap, email, no_hp } = req.body;

    await db.query(
      `UPDATE users
      SET nama_lengkap=?,
          email=?,
          no_hp=?
      WHERE id=?`,
      [nama_lengkap, email, no_hp, req.params.id]
    );

    res.json({
      success: true,
      message: "User berhasil diupdate",
    });
  } catch (err) {
    res.status(500).json(err);
  }
});


// DELETE USER
router.delete("/:id", async (req, res) => {
  try {
    await db.query(
      "DELETE FROM users WHERE id=?",
      [req.params.id]
    );

    res.json({
      success: true,
      message: "User berhasil dihapus",
    });
  } catch (err) {
    res.status(500).json(err);
  }
});

module.exports = router;
```

---

## server.js

```javascript
const express = require("express");
const cors = require("cors");

const app = express();

app.use(cors());
app.use(express.json());

app.use("/api/users", require("./routes/users"));

app.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

---

Jalankan:

```bash
npm run dev
```

atau

```bash
node server.js
```

---

# 4. Frontend React

Install axios:

```bash
npm install axios
```

---

## App.jsx

```jsx
import { useEffect, useState } from "react";
import axios from "axios";
import UserForm from "./UserForm";
import UserTable from "./UserTable";

function App() {
  const [users, setUsers] = useState([]);

  const loadUsers = async () => {
    const res = await axios.get(
      "http://localhost:3000/api/users"
    );

    setUsers(res.data);
  };

  useEffect(() => {
    loadUsers();
  }, []);

  return (
    <div style={{ padding: 20 }}>
      <h2>CRUD User</h2>

      <UserForm refresh={loadUsers} />

      <hr />

      <UserTable
        users={users}
        refresh={loadUsers}
      />
    </div>
  );
}

export default App;
```

---

## UserForm.jsx

```jsx
import { useState } from "react";
import axios from "axios";

function UserForm({ refresh }) {
  const [form, setForm] = useState({
    nama_lengkap: "",
    email: "",
    no_hp: "",
    password: "",
  });

  const submit = async (e) => {
    e.preventDefault();

    await axios.post(
      "http://localhost:3000/api/users",
      form
    );

    setForm({
      nama_lengkap: "",
      email: "",
      no_hp: "",
      password: "",
    });

    refresh();
  };

  return (
    <form onSubmit={submit}>
      <input
        placeholder="Nama Lengkap"
        value={form.nama_lengkap}
        onChange={(e) =>
          setForm({
            ...form,
            nama_lengkap: e.target.value,
          })
        }
      />

      <br /><br />

      <input
        placeholder="Email"
        value={form.email}
        onChange={(e) =>
          setForm({
            ...form,
            email: e.target.value,
          })
        }
      />

      <br /><br />

      <input
        placeholder="No HP"
        value={form.no_hp}
        onChange={(e) =>
          setForm({
            ...form,
            no_hp: e.target.value,
          })
        }
      />

      <br /><br />

      <input
        type="password"
        placeholder="Password"
        value={form.password}
        onChange={(e) =>
          setForm({
            ...form,
            password: e.target.value,
          })
        }
      />

      <br /><br />

      <button type="submit">
        Simpan
      </button>
    </form>
  );
}

export default UserForm;
```

---

## UserTable.jsx

```jsx
import axios from "axios";

function UserTable({ users, refresh }) {

  const hapus = async (id) => {
    if (!window.confirm("Hapus user?"))
      return;

    await axios.delete(
      `http://localhost:3000/api/users/${id}`
    );

    refresh();
  };

  return (
    <table border="1" cellPadding="10">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nama</th>
          <th>Email</th>
          <th>No HP</th>
          <th>Aksi</th>
        </tr>
      </thead>

      <tbody>
        {users.map((user) => (
          <tr key={user.id}>
            <td>{user.id}</td>
            <td>{user.nama_lengkap}</td>
            <td>{user.email}</td>
            <td>{user.no_hp}</td>

            <td>
              <button
                onClick={() => hapus(user.id)}
              >
                Hapus
              </button>
            </td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}

export default UserTable;
```

---

# API yang tersedia

| Method | Endpoint       | Fungsi           |
| ------ | -------------- | ---------------- |
| GET    | /api/users     | Ambil semua user |
| GET    | /api/users/:id | Detail user      |
| POST   | /api/users     | Tambah user      |
| PUT    | /api/users/:id | Update user      |
| DELETE | /api/users/:id | Hapus user       |

### Pengembangan berikutnya yang biasanya dilakukan

1. Login & JWT Authentication
2. Pagination
3. Search user
4. Validasi form
5. React Router
6. ORM seperti Prisma atau Sequelize
7. UI menggunakan Bootstrap, Material UI, atau Ant Design
8. Environment variable (`.env`) untuk koneksi database
9. Struktur MVC (Controller, Service, Repository)

Kalau tujuan Anda belajar proyek yang lebih mendekati dunia kerja, saya bisa bantu refactor contoh di atas menjadi struktur **Express MVC + React + JWT Login + MySQL** yang lebih rapi dan scalable.
