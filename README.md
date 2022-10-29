# React Router
You will often want to create an application with multiple views that correspond to multiple HTML pages.  This tutorial will help you to set up an application with this style.  It is roughly patterned after the [w3schools tutorial](https://www.w3schools.com/react/react_router.asp).

1. Create a react application
```
npx create-react-app multi
cd multi
npm install react-router-dom
```
2. Now create a ```pages``` folder inside of the ```src``` folder with files for each of the views
```
cd src
mkdir pages
touch pages/Layout.js
touch pages/Home.js
touch pages/Blogs.js
touch pages/Contact.js
```
3. Edit ```src/App.js``` and insert the following code
```
import ReactDOM from "react-dom/client";
import { BrowserRouter, Routes, Route, Navigate } from "react-router-dom";
import Layout from "./pages/Layout";
import Home from "./pages/Home";
import Blogs from "./pages/Blogs";
import Contact from "./pages/Contact";

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route index element={<Home />} />
          <Route path="blogs" element={<Blogs />} />
          <Route path="contact" element={<Contact />} />
          <Route path="*" element={<Navigate to="/" />}  />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```
* [```BrowserRouter```](https://reactrouter.com/en/main/router-components/browser-router) stores the current location in the browser's address bar.
* [```Routes```](https://reactrouter.com/en/main/components/routes) looks through all its child routes to find the best match and renders that branch.
* [```Route```](https://reactrouter.com/en/main/route/route) defines the behavior associated with a URL.  So when ```blogs``` is in the URL, then ```Routes``` will render the ```<Blogs />``` tag.
4. Now fill in each of the views in the ```pages``` directory
* pages/Layout.js
```
import { Outlet, Link } from "react-router-dom";

const Layout = () => {
  return (
    <>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/blogs">Blogs</Link>
          </li>
          <li>
            <Link to="/contact">Contact</Link>
          </li>
        </ul>
      </nav>

      <Outlet />
    </>
  )
};

export default Layout;
```
The ```<Outlet>``` renders the current route selected.  So when you select the ```Blogs``` Link, the ```blogs``` route will render the ```<Blogs />``` tag.
```<Link>``` is used like an ```<a>``` tag.  Anytime we link to an internal path, we will use ```<Link>``` instead of ```<a href="">```.
The "Layout route" is a shared component that inserts a navigation menu into all pages.  Notice that all of the other routes are inside of the opening and closing ```Layout``` tags.
* pages/Home.js
```
const Home = () => {
  return <h1>Home</h1>;
};

export default Home;
```
* pages/Blogs.js
```
const Blogs = () => {
  return <h1>Blog Articles</h1>;
};

export default Blogs;
```
* pages/Contact.js
```
const Contact = () => {
  return <h1>Contact Me</h1>;
};

export default Contact;
```
If none of the routes match, then the ```<Navigate>``` tag will redirect to the home page.

5. Run your code and experiment with creating different routes
```
npm start
```

6. Deploy your code with Caddy
Once you have your code working, you will want to deploy it to Caddy so it can be served even after you have stopped your ```npm start``.

Change your "package.json" file to allow your application to be served from any subdirectory in your domain.  
Add the following line to the top of your "package.json" file
```
  "homepage": ".",
```
So, the top 5 lines of your package.json file should be:
```
{
  "name": "my-app",
  "version": "0.1.0",
  "private": true,
  "homepage": ".",
```
Now deploy your application with the command
```
npm run build
```
This should create a "build" directory with all of the files needed to run your application.  Notice that there is an "index.html" file in this directory that is a compressed version of your application.  If you have created ```multi``` in your "public_html/react" directory, then you can access your React CLI application by going to ```https://mydomain/react/multi/build/```

7. Fix the Routes
By default, the router will route pages from the root of your server. If your reactCLI project is in a subdirectory, you might have noticed that clicking on a router link (like the link to the contact page) sends you to "https://mydomain/contact" instead of "https://mydomain/react/multi/build/contact"
 
To remedy this, you can use the basename property to specify which directory you would like to route your links from.
 
Try editing your router (in src/App.js) to say ```<BrowserRouter basename="/react/multi/build">```
 
Now, navigating to the contact page with the router will correctly send you to "https://mydomain/react/multi/build/contact"

If you want to run your development server with ```npm start```, you will have to put this route into the URL ```http://mydomain:8080/react/multi/build```

8. CSS
You may want to add bootstrap CSS to your navigation bar. First add the bootstrap includes after the ```<title>``` in ```public/index.html```
```
    <title>React App</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/js/bootstrap.bundle.min.js"></script>
    
```

Next, add the bootstrap navbar tags to the ```src/pages/Layout.js```
```
  return (
    <>
    <nav class="navbar navbar-expand-sm bg-light">
      <div class="container-fluid">
        <ul class="navbar-nav">
          <li class="nav-item">
            <Link class="nav-link" to="/">Home</Link>
          </li>
          <li class="nav-item">
            <Link class="nav-link" to="/blogs">Blogs</Link>
          </li>
          <li class="nav-item">
            <Link class="nav-link" to="/contact">Contact</Link>
          </li>
        </ul>
      </div>
    </nav>

    <Outlet />
    </>
```
9. Refreshing

There will still be a problem if you try to refresh your "contact" page.  If you want to allow users to refresh the route "contact", you will need to create a folder "build/contact" with the following redirect in an "index.html" file in the folder.
```
<!doctype html>
<html>
<head>
<meta http-equiv="refresh" content="0; URL=/react/multi/build/" />    
</head>
</html>
```
