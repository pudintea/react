# Install React Router Dom
## Perintah Install
```
npm install react-router-dom
```
Pastikan di package.json sudah bertambah
---
## Buat Component Header dan Footer
```
react-project/  
├── src/
│   │
│   ├── components/
│   │   ├── HeaderComponent.jsx
│   │   ├── FooterComponent.jsx
```
Untuk File HeaderComponent.jsx
```
const HeaderComponent = () => {
  return (
    <div>HeaderComponent</div>
  )
}

export default HeaderComponent

```
Untuk File FooterComponent.jsx
```
const FooterComponent = () => {
  return (
    <div>FooterComponent</div>
  )
}

export default FooterComponent
```
---
## Buat Pages Home dan About
Berikut struktur foldernya
```
react-project/  
├── src/
│   │
│   ├── pages/
│   │   ├── HomePage.jsx
│   │   ├── AboutPage.jsx
```
Untuk Halaman HomePage.jsx
```
const HomePage = () => {
  return (
    <div>
        <h1>
          Hallo Ddi Home
        </h1>
    </div>
  )
}

export default HomePage
```
Untuk Halaman AboutPage.jsx
```
const AboutPage = () => {
  return (
    <div>AboutPage</div>
  )
}

export default AboutPage
```
---
## BrowserRouter
Kita panggil Browser Router di main.jsx,
Berikut cara Penggunaanya :
```
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App.jsx'
import './index.css'

import {BrowserRouter} from "react-router-dom"

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>,
)
```
---
## Routes dan Route
kita gunakan di app.jsx,
Perhatikan kode dibawah ini :
```
import {Routes, Route} from "react-router-dom"

import HeaderComponent from "./components/HeaderComponent"
import FooterComponent from "./components/FooterComponent"

import HomePage from "./pages/HomePage"
import AboutPage from "./pages/AboutPage"

function App() {
  return <div>
    <HeaderComponent />
      <Routes>
          <Route path="/" Component={HomePage} />
          <Route path="/about" Component={AboutPage}/>
      </Routes>
    <FooterComponent />
  </div>
}

export default App

```


## By Pudin Saepudin
