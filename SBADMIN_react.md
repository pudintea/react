#SB Admin 2
Tentu. Untuk React, saya sarankan **jangan copy-paste HTML SB Admin 2 langsung**, karena:

* `class` → harus menjadi `className`
* `href="#"` biasanya diganti dengan React Router (`Link`)
* Bootstrap collapse lebih baik menggunakan state React
* jQuery yang dipakai SB Admin 2 sebaiknya dihindari di React

Struktur yang lebih rapi:

```txt
src/
├── layouts/
│   └── AdminLayout.jsx
├── components/
│   ├── Sidebar.jsx
│   ├── Topbar.jsx
│   ├── Footer.jsx
│   └── StatCard.jsx
├── pages/
│   └── Dashboard.jsx
├── App.jsx
└── main.jsx
```

---

## 1. Install

```bash
npm install bootstrap
npm install @fortawesome/fontawesome-free
```

Import di `main.jsx`

```jsx
import 'bootstrap/dist/css/bootstrap.min.css';
import '@fortawesome/fontawesome-free/css/all.min.css';
import './assets/css/sb-admin-2.min.css';
```

---

## 2. AdminLayout.jsx

```jsx
import Sidebar from "../components/Sidebar";
import Topbar from "../components/Topbar";
import Footer from "../components/Footer";

export default function AdminLayout({ children }) {
  return (
    <div id="wrapper">
      <Sidebar />

      <div
        id="content-wrapper"
        className="d-flex flex-column"
      >
        <div id="content">
          <Topbar />

          <div className="container-fluid">
            {children}
          </div>
        </div>

        <Footer />
      </div>
    </div>
  );
}
```

---

## 3. Sidebar.jsx

```jsx
import { useState } from "react";

export default function Sidebar() {
  const [openComponent, setOpenComponent] = useState(false);

  return (
    <ul
      className="navbar-nav bg-gradient-primary sidebar sidebar-dark accordion"
      id="accordionSidebar"
    >
      <a
        className="sidebar-brand d-flex align-items-center justify-content-center"
        href="/"
      >
        <div className="sidebar-brand-icon rotate-n-15">
          <i className="fas fa-laugh-wink"></i>
        </div>

        <div className="sidebar-brand-text mx-3">
          Admin Panel
        </div>
      </a>

      <hr className="sidebar-divider my-0" />

      <li className="nav-item active">
        <a className="nav-link" href="/">
          <i className="fas fa-fw fa-tachometer-alt"></i>
          <span> Dashboard</span>
        </a>
      </li>

      <hr className="sidebar-divider" />

      <div className="sidebar-heading">
        Menu
      </div>

      <li className="nav-item">
        <button
          className="nav-link btn btn-link text-left w-100"
          onClick={() =>
            setOpenComponent(!openComponent)
          }
        >
          <i className="fas fa-fw fa-cog"></i>
          <span> Components</span>
        </button>

        {openComponent && (
          <div className="bg-white py-2 collapse-inner rounded mx-2">
            <a
              className="collapse-item d-block"
              href="/users"
            >
              Users
            </a>

            <a
              className="collapse-item d-block"
              href="/products"
            >
              Products
            </a>
          </div>
        )}
      </li>
    </ul>
  );
}
```

---

## 4. Topbar.jsx

```jsx
export default function Topbar() {
  return (
    <nav className="navbar navbar-expand navbar-light bg-white topbar mb-4 static-top shadow">
      <form className="d-none d-sm-inline-block form-inline mr-auto">
        <div className="input-group">
          <input
            type="text"
            className="form-control bg-light border-0 small"
            placeholder="Search..."
          />

          <div className="input-group-append">
            <button
              className="btn btn-primary"
              type="button"
            >
              <i className="fas fa-search fa-sm"></i>
            </button>
          </div>
        </div>
      </form>

      <ul className="navbar-nav ml-auto">
        <li className="nav-item">
          <span className="nav-link">
            Admin
          </span>
        </li>
      </ul>
    </nav>
  );
}
```

---

## 5. Footer.jsx

```jsx
export default function Footer() {
  return (
    <footer className="sticky-footer bg-white">
      <div className="container my-auto">
        <div className="copyright text-center my-auto">
          <span>
            Copyright © Admin Panel 2026
          </span>
        </div>
      </div>
    </footer>
  );
}
```

---

## 6. StatCard.jsx

```jsx
export default function StatCard({
  title,
  value,
  color,
  icon,
}) {
  return (
    <div className="col-xl-3 col-md-6 mb-4">
      <div
        className={`card border-left-${color} shadow h-100 py-2`}
      >
        <div className="card-body">
          <div className="row no-gutters align-items-center">
            <div className="col mr-2">
              <div
                className={`text-xs font-weight-bold text-${color} text-uppercase mb-1`}
              >
                {title}
              </div>

              <div className="h5 mb-0 font-weight-bold text-gray-800">
                {value}
              </div>
            </div>

            <div className="col-auto">
              <i
                className={`${icon} fa-2x text-gray-300`}
              ></i>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}
```

---

## 7. Dashboard.jsx

```jsx
import AdminLayout from "../layouts/AdminLayout";
import StatCard from "../components/StatCard";

export default function Dashboard() {
  return (
    <AdminLayout>
      <div className="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 className="h3 mb-0 text-gray-800">
          Dashboard
        </h1>
      </div>

      <div className="row">
        <StatCard
          title="Users"
          value="1,245"
          color="primary"
          icon="fas fa-users"
        />

        <StatCard
          title="Orders"
          value="320"
          color="success"
          icon="fas fa-shopping-cart"
        />

        <StatCard
          title="Products"
          value="890"
          color="info"
          icon="fas fa-box"
        />

        <StatCard
          title="Revenue"
          value="Rp 25.000.000"
          color="warning"
          icon="fas fa-wallet"
        />
      </div>

      <div className="card shadow mb-4">
        <div className="card-header py-3">
          <h6 className="m-0 font-weight-bold text-primary">
            Overview
          </h6>
        </div>

        <div className="card-body">
          Selamat datang di halaman admin.
        </div>
      </div>
    </AdminLayout>
  );
}
```

---

## 8. App.jsx

```jsx
import Dashboard from "./pages/Dashboard";

function App() {
  return <Dashboard />;
}

export default App;
```

---

Kalau kamu menggunakan **React Router v6**, saya bisa bantu membuat versi SB Admin 2 yang sudah lengkap dengan:

* Sidebar collapse
* React Router
* Login page
* Dashboard
* CRUD Users
* CRUD Produk
* DataTable
* Chart.js
* Dark mode

dalam struktur project production-ready.

## Penyesuaian
Kalau targetnya **semua fitur SB Admin 2 tersedia di React**, saya sarankan jangan membuat satu file besar. Kita buat arsitektur yang scalable dan seluruh halaman bawaan SB Admin 2 dipetakan ke React Router v6.

## Struktur Project

```txt
src/
├── assets/
│   ├── css/
│   │   └── sb-admin-2.min.css
│   ├── img/
│   └── vendor/
│
├── layouts/
│   ├── AdminLayout.jsx
│   └── AuthLayout.jsx
│
├── components/
│   ├── sidebar/
│   │   └── Sidebar.jsx
│   ├── topbar/
│   │   └── Topbar.jsx
│   ├── footer/
│   │   └── Footer.jsx
│   ├── cards/
│   ├── charts/
│   └── tables/
│
├── pages/
│   ├── dashboard/
│   │   └── Dashboard.jsx
│   │
│   ├── auth/
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   └── ForgotPassword.jsx
│   │
│   ├── components/
│   │   ├── Buttons.jsx
│   │   └── Cards.jsx
│   │
│   ├── utilities/
│   │   ├── Colors.jsx
│   │   ├── Borders.jsx
│   │   ├── Animation.jsx
│   │   └── Other.jsx
│   │
│   ├── charts/
│   │   └── Charts.jsx
│   │
│   ├── tables/
│   │   └── Tables.jsx
│   │
│   ├── pages/
│   │   ├── Blank.jsx
│   │   └── NotFound.jsx
│
├── routes/
│   └── AppRoutes.jsx
│
├── App.jsx
└── main.jsx
```

---

# Install Package

```bash
npm install react-router-dom
npm install bootstrap
npm install @fortawesome/fontawesome-free
npm install chart.js react-chartjs-2
npm install react-bootstrap
npm install react-data-table-component
npm install clsx
```

---

# main.jsx

```jsx
import React from "react";
import ReactDOM from "react-dom/client";

import App from "./App";

import "bootstrap/dist/css/bootstrap.min.css";
import "@fortawesome/fontawesome-free/css/all.min.css";

import "./assets/css/sb-admin-2.min.css";

ReactDOM.createRoot(
  document.getElementById("root")
).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

# App.jsx

```jsx
import { BrowserRouter } from "react-router-dom";
import AppRoutes from "./routes/AppRoutes";

export default function App() {
  return (
    <BrowserRouter>
      <AppRoutes />
    </BrowserRouter>
  );
}
```

---

# AppRoutes.jsx

```jsx
import { Routes, Route } from "react-router-dom";

import Dashboard from "../pages/dashboard/Dashboard";

import Login from "../pages/auth/Login";
import Register from "../pages/auth/Register";
import ForgotPassword from "../pages/auth/ForgotPassword";

import Buttons from "../pages/components/Buttons";
import Cards from "../pages/components/Cards";

import Colors from "../pages/utilities/Colors";
import Borders from "../pages/utilities/Borders";
import Animation from "../pages/utilities/Animation";
import Other from "../pages/utilities/Other";

import Charts from "../pages/charts/Charts";
import Tables from "../pages/tables/Tables";

import Blank from "../pages/pages/Blank";
import NotFound from "../pages/pages/NotFound";

export default function AppRoutes() {
  return (
    <Routes>
      <Route path="/" element={<Dashboard />} />

      <Route path="/login" element={<Login />} />
      <Route path="/register" element={<Register />} />
      <Route
        path="/forgot-password"
        element={<ForgotPassword />}
      />

      <Route
        path="/components/buttons"
        element={<Buttons />}
      />

      <Route
        path="/components/cards"
        element={<Cards />}
      />

      <Route
        path="/utilities/colors"
        element={<Colors />}
      />

      <Route
        path="/utilities/borders"
        element={<Borders />}
      />

      <Route
        path="/utilities/animations"
        element={<Animation />}
      />

      <Route
        path="/utilities/other"
        element={<Other />}
      />

      <Route
        path="/charts"
        element={<Charts />}
      />

      <Route
        path="/tables"
        element={<Tables />}
      />

      <Route
        path="/blank"
        element={<Blank />}
      />

      <Route
        path="*"
        element={<NotFound />}
      />
    </Routes>
  );
}
```

---

# AdminLayout

Semua halaman admin akan menggunakan layout ini.

```jsx
import Sidebar from "../components/sidebar/Sidebar";
import Topbar from "../components/topbar/Topbar";
import Footer from "../components/footer/Footer";

export default function AdminLayout({
  children,
}) {
  return (
    <div id="wrapper">
      <Sidebar />

      <div
        id="content-wrapper"
        className="d-flex flex-column"
      >
        <div id="content">
          <Topbar />

          <div className="container-fluid">
            {children}
          </div>
        </div>

        <Footer />
      </div>
    </div>
  );
}
```

---

# Sidebar React Version

Tidak menggunakan jQuery.

Gunakan state:

```jsx
const [componentsOpen, setComponentsOpen] =
  useState(false);

const [utilitiesOpen, setUtilitiesOpen] =
  useState(false);

const [pagesOpen, setPagesOpen] =
  useState(false);
```

Kemudian:

```jsx
<Link
  className="nav-link"
  to="/charts"
>
  <i className="fas fa-chart-area"></i>
  <span>Charts</span>
</Link>
```

dan:

```jsx
{componentsOpen && (
  <div className="collapse-inner bg-white rounded py-2">
     ...
  </div>
)}
```

---

# Charts

SB Admin menggunakan Chart.js.

React version:

```jsx
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement
} from "chart.js";

import { Line } from "react-chartjs-2";
```

```jsx
<Line
  data={{
    labels: [
      "Jan",
      "Feb",
      "Mar",
      "Apr",
      "May",
    ],
    datasets: [
      {
        label: "Revenue",
        data: [10, 20, 30, 25, 40],
        borderColor: "#4e73df",
      },
    ],
  }}
/>
```

---

# Tables

SB Admin menggunakan DataTables.

Di React lebih baik:

```jsx
import DataTable from
"react-data-table-component";
```

```jsx
<DataTable
  columns={columns}
  data={data}
  pagination
  striped
  responsive
/>
```

---

# Login Layout

Pisahkan dari Admin Layout.

```jsx
export default function AuthLayout({
  children,
}) {
  return (
    <div className="bg-gradient-primary">
      <div className="container">
        {children}
      </div>
    </div>
  );
}
```

---

# Fitur SB Admin 2 yang Bisa Dibuat 100% di React

✅ Dashboard
✅ Sidebar Accordion
✅ Topbar Dropdown
✅ Search
✅ Alerts Dropdown
✅ Messages Dropdown
✅ User Dropdown
✅ Cards
✅ Buttons
✅ Colors
✅ Borders
✅ Animations
✅ Other Utilities
✅ Chart Area
✅ Chart Pie
✅ Data Tables
✅ Login
✅ Register
✅ Forgot Password
✅ Blank Page
✅ 404 Page
✅ Footer
✅ Scroll To Top
✅ Responsive Sidebar
✅ Collapse Menu
✅ React Router v6

---

Untuk implementasi yang benar-benar production-ready, saya biasanya membuat satu komponen `AdminLayout` dengan konfigurasi menu berbasis JSON seperti:

```js
const menu = [
  {
    title: "Dashboard",
    icon: "fa-tachometer-alt",
    path: "/",
  },
  {
    title: "Components",
    icon: "fa-cog",
    children: [
      {
        title: "Buttons",
        path: "/components/buttons",
      },
      {
        title: "Cards",
        path: "/components/cards",
      },
    ],
  },
];
```

Lalu Sidebar dirender otomatis dari konfigurasi tersebut, sehingga menambah menu baru tidak perlu mengubah JSX. Ini jauh lebih mudah dirawat dibanding menerjemahkan HTML SB Admin 2 secara langsung.
## Pudin Saepudin
