Berikut contoh layout admin sekolah yang modern menggunakan **React + Vite + TypeScript + shadcn/ui + Tailwind CSS**.

### Struktur Folder

```text
src/
│
├── layouts/
│   └── AdminLayout.tsx
│
├── components/
│   ├── app-sidebar.tsx
│   ├── app-header.tsx
│   └── app-footer.tsx
│
├── pages/
│   ├── Dashboard.tsx
│   ├── Students.tsx
│   ├── Teachers.tsx
│   └── Settings.tsx
│
└── App.tsx
```

---

# Install

```bash
npm install lucide-react
```

Kalau belum install komponen shadcn:

```bash
npx shadcn@latest add sidebar
npx shadcn@latest add dropdown-menu
npx shadcn@latest add avatar
npx shadcn@latest add separator
npx shadcn@latest add button
npx shadcn@latest add breadcrumb
```

---

# app-sidebar.tsx

```tsx
import {
  BookOpen,
  GraduationCap,
  Home,
  Settings,
  Users,
} from "lucide-react"

import {
  Sidebar,
  SidebarContent,
  SidebarFooter,
  SidebarHeader,
  SidebarMenu,
  SidebarMenuButton,
  SidebarMenuItem,
} from "@/components/ui/sidebar"

const menus = [
  {
    title: "Dashboard",
    icon: Home,
    url: "/",
  },
  {
    title: "Siswa",
    icon: GraduationCap,
    url: "/students",
  },
  {
    title: "Guru",
    icon: Users,
    url: "/teachers",
  },
  {
    title: "Mata Pelajaran",
    icon: BookOpen,
    url: "/subjects",
  },
  {
    title: "Pengaturan",
    icon: Settings,
    url: "/settings",
  },
]

export function AppSidebar() {
  return (
    <Sidebar collapsible="icon">
      <SidebarHeader className="text-center py-6">
        <h1 className="font-bold text-xl">
          🎓 School Admin
        </h1>
      </SidebarHeader>

      <SidebarContent>
        <SidebarMenu>
          {menus.map((menu) => (
            <SidebarMenuItem key={menu.title}>
              <SidebarMenuButton asChild>
                <a href={menu.url}>
                  <menu.icon />
                  <span>{menu.title}</span>
                </a>
              </SidebarMenuButton>
            </SidebarMenuItem>
          ))}
        </SidebarMenu>
      </SidebarContent>

      <SidebarFooter className="text-xs text-center text-muted-foreground">
        v1.0.0
      </SidebarFooter>
    </Sidebar>
  )
}
```

---

# app-header.tsx

```tsx
import { Bell } from "lucide-react"

import { Button } from "@/components/ui/button"
import {
  SidebarTrigger,
} from "@/components/ui/sidebar"

import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/components/ui/avatar"

export function AppHeader() {
  return (
    <header className="h-16 border-b bg-background flex items-center justify-between px-6">
      <div className="flex items-center gap-3">
        <SidebarTrigger />

        <div>
          <h2 className="font-semibold">
            Dashboard
          </h2>

          <p className="text-sm text-muted-foreground">
            Sistem Informasi Sekolah
          </p>
        </div>
      </div>

      <div className="flex items-center gap-4">
        <Button size="icon" variant="ghost">
          <Bell size={18} />
        </Button>

        <Avatar>
          <AvatarImage src="" />
          <AvatarFallback>AD</AvatarFallback>
        </Avatar>
      </div>
    </header>
  )
}
```

---

# app-footer.tsx

```tsx
export function AppFooter() {
  return (
    <footer className="border-t h-14 flex items-center justify-center text-sm text-muted-foreground">
      © 2026 School Admin System
    </footer>
  )
}
```

---

# AdminLayout.tsx

```tsx
import { Outlet } from "react-router-dom"

import {
  SidebarInset,
  SidebarProvider,
} from "@/components/ui/sidebar"

import { AppSidebar } from "@/components/app-sidebar"
import { AppHeader } from "@/components/app-header"
import { AppFooter } from "@/components/app-footer"

export default function AdminLayout() {
  return (
    <SidebarProvider>
      <AppSidebar />

      <SidebarInset>
        <AppHeader />

        <main className="flex-1 p-6 bg-muted/30 min-h-[calc(100vh-120px)]">
          <Outlet />
        </main>

        <AppFooter />
      </SidebarInset>
    </SidebarProvider>
  )
}
```

---

# Dashboard.tsx

```tsx
export default function Dashboard() {
  return (
    <div className="space-y-6">

      <div>
        <h1 className="text-3xl font-bold">
          Dashboard
        </h1>

        <p className="text-muted-foreground">
          Selamat datang di Admin Sekolah
        </p>
      </div>

      <div className="grid md:grid-cols-4 gap-6">

        <div className="rounded-xl border bg-card p-6">
          <h3 className="text-muted-foreground">
            Total Siswa
          </h3>

          <p className="text-3xl font-bold">
            1.250
          </p>
        </div>

        <div className="rounded-xl border bg-card p-6">
          <h3 className="text-muted-foreground">
            Guru
          </h3>

          <p className="text-3xl font-bold">
            75
          </p>
        </div>

        <div className="rounded-xl border bg-card p-6">
          <h3 className="text-muted-foreground">
            Kelas
          </h3>

          <p className="text-3xl font-bold">
            32
          </p>
        </div>

        <div className="rounded-xl border bg-card p-6">
          <h3 className="text-muted-foreground">
            Mata Pelajaran
          </h3>

          <p className="text-3xl font-bold">
            18
          </p>
        </div>

      </div>

    </div>
  )
}
```

---

# App.tsx

```tsx
import { BrowserRouter, Routes, Route } from "react-router-dom"

import AdminLayout from "./layouts/AdminLayout"
import Dashboard from "./pages/Dashboard"

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route element={<AdminLayout />}>
          <Route index element={<Dashboard />} />
        </Route>
      </Routes>
    </BrowserRouter>
  )
}
```

---

# Hasil Layout

```text
 ---------------------------------------------------------
| Sidebar | Header                                       |
|         |----------------------------------------------|
|         |                                              |
|         |   Card Statistik                             |
|         |  +-------+ +-------+ +-------+ +-------+     |
|         |                                              |
|         |   Table Siswa                               |
|         |                                              |
|         |                                              |
|         |----------------------------------------------|
|         | Footer                                       |
 ---------------------------------------------------------
```

## Agar tampil lebih premium

Beberapa sentuhan yang membuat dashboard terasa seperti aplikasi profesional (misalnya Vercel, Linear, atau GitHub) antara lain:

* Sidebar yang dapat di-collapse dengan ikon saja.
* Header yang memiliki breadcrumb, kolom pencarian, tombol notifikasi, dan avatar pengguna.
* Statistik menggunakan komponen `Card` dari shadcn dengan ikon berwarna dan indikator tren (naik/turun).
* Dukungan mode gelap (`next-themes`) dengan toggle tema.
* Warna utama biru (`blue`) atau `zinc` agar tampil bersih dan elegan.
* Tabel menggunakan `DataTable` (TanStack Table) dengan fitur pencarian, filter, sorting, dan pagination.
* Grafik menggunakan `Recharts` untuk statistik siswa, guru, dan kehadiran.
* Kalender agenda sekolah di dashboard.
* Menu sidebar bertingkat (collapsible) untuk modul seperti Akademik, Keuangan, dan Administrasi.
* Responsif di perangkat mobile dengan sidebar berbentuk drawer.

Dengan kombinasi komponen tersebut, Anda bisa mendapatkan tampilan dashboard admin sekolah yang modern, ringan, dan konsisten dengan desain shadcn/ui.
