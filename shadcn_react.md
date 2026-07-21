# Panduan Instalasi

Tentu! Saya akan jelaskan **dari nol** seolah-olah ini pertama kali Anda menggunakan React + Vite + shadcn/ui.

Panduan ini mengikuti dokumentasi resmi shadcn untuk Vite. ([ui.shadcn.com][1])

---

# 1. Install Node.js

Pastikan Node.js sudah terinstall.

Cek dengan:

```bash
node -v
```

dan

```bash
npm -v
```

Kalau muncul versinya, misalnya:

```
v22.x.x
10.x.x
```

berarti sudah siap.

Kalau belum ada, install Node.js terlebih dahulu.

---

# 2. Buat Project React Vite

Buka Terminal atau CMD.

Jalankan:

```bash
npm create vite@latest
```

Lalu akan muncul pertanyaan:

```
Project name:
```

Misalnya isi:

```
my-app
```

Lalu pilih

```
React
```

Kemudian pilih

```
TypeScript
```

> **Kenapa TypeScript?** Karena shadcn/ui saat ini menggunakan template React + TypeScript sebagai rekomendasi. ([ui.shadcn.com][1])

Masuk ke folder project:

```bash
cd my-app
```

---

# 3. Install Dependencies

Jalankan:

```bash
npm install
```

Tunggu sampai selesai.

---

# 4. Install Tailwind CSS

Install Tailwind versi terbaru:

```bash
npm install tailwindcss @tailwindcss/vite
```

([ui.shadcn.com][1])

---

# 5. Edit `src/index.css`

Hapus semua isi file:

```
src/index.css
```

Ganti menjadi:

```css
@import "tailwindcss";
```

---

# 6. Tambahkan Alias "@"

Buka file:

```
tsconfig.json
```

Di bagian:

```json
"compilerOptions"
```

Tambahkan:

```json
"baseUrl": ".",
"paths": {
  "@/*": ["./src/*"]
}
```

---

Lalu buka

```
tsconfig.app.json
```

Tambahkan juga:

```json
"baseUrl": ".",
"paths": {
  "@/*": ["./src/*"]
}
```

---

# 7. Install @types/node

Jalankan:

```bash
npm install -D @types/node
```

---

# 8. Edit `vite.config.ts`

Ubah menjadi seperti ini:

```ts
import path from "path"
import tailwindcss from "@tailwindcss/vite"
import react from "@vitejs/plugin-react"
import { defineConfig } from "vite"

export default defineConfig({
  plugins: [
    react(),
    tailwindcss(),
  ],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
})
```

([ui.shadcn.com][1])

---

# 9. Install shadcn/ui

Sekarang jalankan:

```bash
npx shadcn@latest init
```

Nanti akan muncul beberapa pertanyaan.

Biasanya cukup tekan **Enter** untuk pilihan default, atau pilih sesuai kebutuhan.

Jika berhasil akan muncul file seperti:

```
components.json
```

dan folder:

```
src/components
```

---

# 10. Install Component Pertama

Misalnya install tombol (Button):

```bash
npx shadcn@latest add button
```

Kalau berhasil, akan muncul file:

```
src/components/ui/button.tsx
```

---

# 11. Coba Gunakan Button

Buka:

```
src/App.tsx
```

Ganti isinya menjadi:

```tsx
import { Button } from "@/components/ui/button"

function App() {
  return (
    <div className="flex min-h-screen items-center justify-center">
      <Button>Halo Shadcn!</Button>
    </div>
  )
}

export default App
```

---

# 12. Jalankan Project

```bash
npm run dev
```

Biasanya akan muncul:

```
Local:

http://localhost:5173
```

Buka alamat tersebut di browser.

Kalau semuanya benar, Anda akan melihat sebuah tombol **"Halo Shadcn!"**.

---

# Struktur Folder Setelah Selesai

Kurang lebih akan menjadi:

```
my-app/
│
├── src/
│   ├── components/
│   │    └── ui/
│   │         └── button.tsx
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
│
├── components.json
├── vite.config.ts
├── tsconfig.json
├── tsconfig.app.json
└── package.json
```

---

## Ringkasan Perintah

```bash
npm create vite@latest
cd my-app
npm install

npm install tailwindcss @tailwindcss/vite
npm install -D @types/node

npx shadcn@latest init

npx shadcn@latest add button

npm run dev
```

Panduan ini mengikuti alur instalasi resmi untuk **React + Vite + shadcn/ui**, termasuk konfigurasi Tailwind CSS, alias `@`, dan inisialisasi CLI shadcn. ([ui.shadcn.com][1])

Jika Anda masih benar-benar pemula, saya juga bisa membuat versi yang lebih detail dengan **gambar alur**, menjelaskan fungsi setiap file (`App.tsx`, `main.tsx`, `vite.config.ts`, `components.json`, dll.) sehingga lebih mudah dipahami.

[1]: https://ui.shadcn.com/docs/installation/vite?utm_source=chatgpt.com "Vite - shadcn/ui"
