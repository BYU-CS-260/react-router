# React Router
You will often want to create an application with multiple views that correspond to multiple HTML pages.  This tutorial will help you to set up an application with this style.

1. Create a react application
```
npx create-react-app multi
cd multi
npm install react-router-dom
```
2. Now create a ```pages``` folder with files for each of the views
```
mkdir pages
touch pages/Layout.js
touch pages/Home.js
touch pages/Blogs.js
touch pages/Contact.js
touch pages/NoPage.js
```
3. Edit ```App.js``` and insert the following code
```
import ReactDOM from "react-dom/client";
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Layout from "./pages/Layout";
import Home from "./pages/Home";
import Blogs from "./pages/Blogs";
import Contact from "./pages/Contact";
import NoPage from "./pages/NoPage";

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route index element={<Home />} />
          <Route path="blogs" element={<Blogs />} />
          <Route path="contact" element={<Contact />} />
          <Route path="*" element={<NoPage />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```
* [```BrowserRouter```](https://reactrouter.com/en/main/router-components/browser-router) stores the current location in the browser's address bar.
* [```Routes```](https://reactrouter.com/en/main/components/routes) looks through all its child routes to find the best match and renders that branch.
* [```Route```](https://reactrouter.com/en/main/route/route) defines the behavior associated with a URL.  So when ```blogs``` is in the URL, then ```Routes``` will render the ```<Blogs />``` tag.

