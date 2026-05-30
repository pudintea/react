Kalau kamu pakai **React + Vite**, cara paling mudah saat ini adalah mengikuti panduan resmi Tailwind untuk Vite. ([v3.tailwindcss.com][1])

### 1. Buat Project React Vite

```bash
npm create vite@latest my-app -- --template react
```

Masuk ke folder project:

```bash
cd my-app
```

Install dependency:

```bash
npm install
```

---

### 2. Install Tailwind CSS

Untuk Tailwind versi terbaru:

```bash
npm install tailwindcss @tailwindcss/vite
```

([tailwind.org.cn][2])

---

### 3. Edit `vite.config.js`

Buka file:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [
    react(),
    tailwindcss(),
  ],
})
```

([Tailwind CSS][3])

---

### 4. Edit `src/index.css`

Ganti isinya menjadi:

```css
@import "tailwindcss";
```

([Tailwind CSS][3])

---

### 5. Jalankan Project

```bash
npm run dev
```

---

### 6. Coba Test Tailwind

Edit `src/App.jsx`:

```jsx
export default function App() {
  return (
    <div className="h-screen flex items-center justify-center">
      <h1 className="text-4xl font-bold text-blue-500">
        Hello Tailwind CSS 🚀
      </h1>
    </div>
  )
}
```

Kalau tulisan muncul **biru besar**, berarti Tailwind sudah berhasil terpasang.

---

### Jika Tailwind Tidak Jalan

Beberapa masalah yang sering terjadi:

* Lupa import CSS di `main.jsx`
* Belum restart server Vite
* Salah konfigurasi `vite.config.js`
* Versi Tailwind lama dan tutorial yang diikuti untuk versi baru (atau sebaliknya) ([Reddit][4])

Pastikan di `main.jsx` ada:

```jsx
import './index.css'
```

---

Dokumentasi resmi:

* [Tailwind CSS Vite Guide](https://tailwindcss.com/docs/installation/using-vite?utm_source=chatgpt.com)
* [Vite Official Website](https://vitejs.dev?utm_source=chatgpt.com)
* [React Documentation](https://react.dev?utm_source=chatgpt.com)

Kalau mau, saya juga bisa buatkan **template React + Vite + Tailwind + React Router** yang sudah siap dipakai untuk belajar project nyata.

[1]: https://v3.tailwindcss.com/docs/guides/vite?utm_source=chatgpt.com "Install Tailwind CSS with Vite - Tailwind CSS"
[2]: https://tailwind.org.cn/docs/installation/using-vite?utm_source=chatgpt.com "使用 Vite 安装 Tailwind CSS - Tailwind CSS - Tailwind 框架"
[3]: https://tailwindcss.com/docs/guides/remix?utm_source=chatgpt.com "Install Tailwind CSS with React Router - Tailwind CSS"
[4]: https://www.reddit.com/r/tailwindcss/comments/1i8xajo?utm_source=chatgpt.com "tailwind v4.0 issue in installing for vite framework"
