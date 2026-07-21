Pilihan memakai struktur **`features/`** sangat bagus untuk aplikasi admin sekolah. Untuk pemula memang terlihat lebih banyak folder, tetapi keuntungan besarnya adalah ketika aplikasi membesar, kode tidak berantakan.

Saya akan jelaskan dari konsep paling dasar sampai contoh implementasi.

---

# 1. Kenapa memakai folder `features`?

Ada dua pola umum:

## Pola berdasarkan jenis file (sederhana)

Contoh:

```
src/
├── pages/
│   ├── Users.tsx
│   ├── Sekolah.tsx
│   └── Kelas.tsx
│
├── components/
│   ├── UserTable.tsx
│   └── SekolahTable.tsx
│
├── api/
│   └── userApi.ts
```

Awalnya mudah, tetapi setelah besar:

```
components/
 ├── UserTable
 ├── UserForm
 ├── SekolahTable
 ├── SekolahForm
 ├── KelasTable
 ├── KelasForm
 ├── ...
```

Akhirnya sulit mencari kode yang berhubungan.

---

## Pola Feature Based

Semua yang berhubungan dengan **Users** dikumpulkan bersama:

```
features/
└── users/
    ├── api.ts
    ├── types.ts
    ├── components/
    └── pages/
```

Artinya:

> Kalau saya ingin menghapus fitur users, saya cukup menghapus folder users.

Ini pola yang banyak digunakan pada aplikasi production.

---

# 2. Struktur lengkap project kita

Untuk aplikasi admin sekolah:

```
src/
│
├── app/
│   ├── router.tsx
│   └── providers.tsx
│
├── components/
│   ├── layout/
│   │   ├── AdminLayout.tsx
│   │   ├── Sidebar.tsx
│   │   └── Header.tsx
│   │
│   └── ui/
│       └── (shadcn components)
│
├── features/
│   │
│   ├── users/
│   │   ├── api.ts
│   │   ├── types.ts
│   │   ├── data.ts
│   │   │
│   │   ├── components/
│   │   │   ├── UserTable.tsx
│   │   │   ├── UserForm.tsx
│   │   │   └── UserCard.tsx
│   │   │
│   │   └── pages/
│   │       └── UsersPage.tsx
│   │
│   │
│   ├── sekolah/
│   │   ├── api.ts
│   │   ├── types.ts
│   │   ├── data.ts
│   │   │
│   │   ├── components/
│   │   │   ├── SekolahTable.tsx
│   │   │   └── SekolahForm.tsx
│   │   │
│   │   └── pages/
│   │       └── SekolahPage.tsx
│   │
│   │
│   └── kelas/
│       ├── api.ts
│       ├── types.ts
│       ├── data.ts
│       ├── components/
│       └── pages/
│
├── lib/
│   └── utils.ts
│
├── main.tsx
│
└── index.css
```

---

# 3. Penjelasan setiap file

## `types.ts`

Tempat mendefinisikan bentuk data.

Contoh:

```
features/users/types.ts
```

```ts
export interface User {

  id:number;

  nama:string;

  email:string;

  role:
  | "admin"
  | "operator"
  | "guru";

}
```

Kenapa penting?

Misalnya nanti API mengirim:

```json
{
"id":1,
"nama":"Budi",
"email":"budi@mail.com",
"role":"guru"
}
```

TypeScript akan tahu bentuk datanya.

---

# 4. Dummy data

Karena belum ada backend:

```
features/users/data.ts
```

```ts
import type {User} from "./types";


export const users:User[] = [

{
 id:1,
 nama:"Budi Santoso",
 email:"budi@mail.com",
 role:"admin"
},

{
 id:2,
 nama:"Siti Rahma",
 email:"siti@mail.com",
 role:"guru"
}

];
```

Nanti file ini bisa dihapus ketika API sudah ada.

---

# 5. API Layer

Sekarang kosong dulu:

```
features/users/api.ts
```

```ts
import type {User} from "./types";


export async function getUsers()
:Promise<User[]> {


const response =
await fetch(
"/api/users"
);


return response.json();

}
```

Sekarang memang belum dipakai.

Tetapi nanti:

```
Database
   |
Backend API
   |
api.ts
   |
Component
```

Alurnya jelas.

---

# 6. Component UserTable

Folder:

```
users/components/UserTable.tsx
```

Isinya hanya tabel.

```tsx
import {
Table,
TableBody,
TableCell,
TableHead,
TableHeader,
TableRow
}
from "@/components/ui/table";


import type {User} from "../types";


interface Props {

users:User[];

}


export default function UserTable({
users
}:Props){


return (

<Table>

<TableHeader>

<TableRow>

<TableHead>
Nama
</TableHead>

<TableHead>
Email
</TableHead>

<TableHead>
Role
</TableHead>

</TableRow>

</TableHeader>


<TableBody>


{
users.map(user=>(


<TableRow key={user.id}>


<TableCell>
{user.nama}
</TableCell>


<TableCell>
{user.email}
</TableCell>


<TableCell>
{user.role}
</TableCell>


</TableRow>


))
}


</TableBody>

</Table>


)

}
```

Perhatikan:

Component ini tidak tahu data dari mana.

Dia hanya menerima:

```tsx
users={users}
```

---

# 7. Halaman Users

Sekarang:

```
features/users/pages/UsersPage.tsx
```

```tsx
import UserTable 
from "../components/UserTable";


import {users}
from "../data";


export default function UsersPage(){


return (

<div>


<h1 className="
text-2xl
font-bold
mb-5
">

Users

</h1>


<UserTable
users={users}
/>


</div>


)

}
```

Tugas Page:

* mengambil data
* mengatur layout

Bukan membuat tabel.

---

# 8. Routing

Sekarang router tidak mengambil langsung:

```tsx
import Users from "@/pages/Users";
```

Tetapi:

```tsx
import UsersPage
from "@/features/users/pages/UsersPage";
```

Contoh:

```
app/router.tsx
```

```tsx
import {
createBrowserRouter
}
from "react-router-dom";


import AdminLayout
from "@/components/layout/AdminLayout";


import UsersPage
from "@/features/users/pages/UsersPage";


import SekolahPage
from "@/features/sekolah/pages/SekolahPage";


export const router =
createBrowserRouter([


{
path:"/",
element:<AdminLayout/>,

children:[


{
path:"users",
element:<UsersPage/>
},


{
path:"sekolah",
element:<SekolahPage/>
}


]


}


]);
```

---

# 9. Alur berpikirnya

Misalnya membuka:

```
localhost:5173/users
```

Maka:

```
Router
 |
 |
UsersPage.tsx
 |
 |
UserTable.tsx
 |
 |
data.ts
```

Ketika API sudah ada:

```
Router
 |
 |
UsersPage.tsx
 |
 |
api.ts
 |
 |
Backend
 |
 |
Database
```

Component tidak berubah.

---

# 10. Tambahan folder yang biasanya dipakai

Untuk aplikasi admin modern saya biasanya tambah:

```
src/
├── hooks/
│   └── useAuth.ts
│
├── services/
│   └── axios.ts
│
├── constants/
│   └── menu.ts
│
├── utils/
│
└── schemas/
    └── userSchema.ts
```

Fungsinya:

### hooks

Logic React yang bisa dipakai ulang.

Contoh:

```
useAuth()
useDebounce()
usePagination()
```

### services

Tempat konfigurasi API:

```
axios.ts
```

Misalnya:

```ts
axios.defaults.baseURL =
"https://api.sekolah.com";
```

### schemas

Validasi form dengan Zod:

```
nama wajib diisi
email harus valid
```

shadcn sangat cocok dengan:

* React Hook Form
* Zod

---

## Urutan belajar yang saya sarankan untuk kamu:

1. ✅ React + TypeScript dasar
2. ✅ Routing
3. ✅ Admin Layout
4. ✅ Feature folder structure
5. ✅ shadcn Table
6. ✅ Form + React Hook Form + Zod
7. ✅ API dengan Axios/TanStack Query
8. ✅ Authentication
9. ✅ Permission (admin/operator/guru)

Dengan struktur ini, project kamu sudah punya fondasi yang bisa berkembang dari **dummy data → backend API → aplikasi sekolah production** tanpa perlu bongkar besar-besaran.
