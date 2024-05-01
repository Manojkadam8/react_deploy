<div align="center">
    <h2>⚜️ D E P L O Y &nbsp; VITE REACT APP WITH ROUTER ⚜️</h2>
</div>

<div align="center">
    <h3>
    <a href="https://youtu.be/r6qQe1Jbc7A">youtube video</a>
        </h3>
</div>

<br />

### Follow the steps below on how to deploy a vite react app with router:

## 💻 Deployment

#### 01. Create a vite react app

```npm
npm create vite@latest
```

#### 02. Set the base on _vite.config_

```js
base: "/[REPO_NAME]/";
```

#### 03. Create ./github/workflows/deploy.yml and add the code bellow

```yml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: 16

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v2
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v2
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

#### 04. Create a new repository on GitHub and initialize GIT

```git
git init
git add .
git commit -m "add: initial files"
git branch -M main
git remote add origin https://github.com/[USER]/[REPO_NAME]
git push -u origin main
```

#### 05. Active workflow

```js
Config -> Actions -> General -> Workflow permissions -> Read and Write permissions
Actions -> failed deploy -> re-run-job failed jobs
Config -> Pages -> gh-pages -> save
```

## React Router

#### 01. Install React Router

```npm
npm i react-router-dom
```

#### 02. Create the pages

```
src
└── pages
    ├── Home.tsx
    └── Contact.tsx
```
#### 03. Create Layout.jsx File in main project folder

```js
import React from 'react'
import { Outlet } from 'react-router-dom'

const Layout = () => {
  return (
    <>
      <Outlet />
    </>
  )
}

export default Layout
```
#### 04. Configuring the routing on _main.tsx_

```js
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import { Route, RouterProvider, createBrowserRouter, createRoutesFromElements } from 'react-router-dom'
import './index.css'
import Layout from '../Layout.jsx'
import Home from './pages/Home.jsx'
import About from './pages/About.jsx'
const router=createBrowserRouter(createRoutesFromElements(
              
  
  <Route path='/react_deploy/' element={<Layout />} >
 
 <Route path='/react_deploy/' element={<Home />} />
 <Route path='/react_deploy/about' element={<About />} />

  </Route>


))
ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
      <RouterProvider router={router} />
  </React.StrictMode>,
)
```

#### 05. Specify the homepage in _package.json_

```json
{
  "homepage": "[PROJECT_URL]"
}
```

#### 06. Create the _404.html_ file in the _public_ folder

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>React Router</title>
    <script type="text/javascript">
      var pathSegmentsToKeep = 1;

      var l = window.location;
      l.replace(
        l.protocol +
          "//" +
          l.hostname +
          (l.port ? ":" + l.port : "") +
          l.pathname
            .split("/")
            .slice(0, 1 + pathSegmentsToKeep)
            .join("/") +
          "/?/" +
          l.pathname.slice(1).split("/").slice(pathSegmentsToKeep).join("/").replace(/&/g, "~and~") +
          (l.search ? "&" + l.search.slice(1).replace(/&/g, "~and~") : "") +
          l.hash
      );
    </script>
  </head>
  <body></body>
</html>
```

#### 07. Add the script below inside the head tag in _index.html_

```html
<script type="text/javascript">
  (function (l) {
    if (l.search[1] === "/") {
      var decoded = l.search
        .slice(1)
        .split("&")
        .map(function (s) {
          return s.replace(/~and~/g, "&");
        })
        .join("?");
      window.history.replaceState(null, null, l.pathname.slice(0, -1) + decoded + l.hash);
    }
  })(window.location);
</script>
```

#### 08. Push

```git
git add .
git commit -m "feat: routing"
git push
```
