# 🎯 TOPIC 2: React Router v6 Core Concepts

### 📚 SUBTOPIC 1: React Router v6 - Routes, Route, Link, NavLink

#### **THE THEORY (DEEP DIVE)**

**WHY does React Router exist?**

In the previous topic, you learned:

* ✅ SPAs need client-side routing
* ✅ `window.history.pushState()` changes URL without reload
* ✅ JavaScript manually renders components based on URL

**But imagine building this yourself:**

```javascript
// Without React Router - Manual routing
function App() {
  const [currentPath, setCurrentPath] = useState(window.location.pathname);
  
  // Listen for back/forward button
  useEffect(() => {
    const handlePopState = () => {
      setCurrentPath(window.location.pathname);
    };
    
    window.addEventListener('popstate', handlePopState);
    return () => window.removeEventListener('popstate', handlePopState);
  }, []);
  
  // Manual navigation function
  const navigateTo = (path) => {
    window.history.pushState({}, '', path);
    setCurrentPath(path);
  };
  
  // Manual route matching
  let component;
  if (currentPath === '/') {
    component = <Home />;
  } else if (currentPath === '/about') {
    component = <About />;
  } else if (currentPath === '/profile') {
    component = <Profile />;
  } else if (currentPath.startsWith('/user/')) {
    const userId = currentPath.split('/')[2];
    component = <UserProfile userId={userId} />;
  } else {
    component = <NotFound />;
  }
  
  return (
    <div>
      <nav>
        <a href="/" onClick={(e) => { e.preventDefault(); navigateTo('/'); }}>
          Home
        </a>
        <a href="/about" onClick={(e) => { e.preventDefault(); navigateTo('/about'); }}>
          About
        </a>
      </nav>
      {component}
    </div>
  );
}
```

**Problems with this approach:**

1. ❌ **Boilerplate hell**: Every link needs `onClick` with `preventDefault()`
2. ❌ **Manual pattern matching**: You're parsing URLs with string operations
3. ❌ **No nested routes**: Can't have `/dashboard/settings/profile` hierarchy
4. ❌ **No active link styling**: How do you know which link is active?
5. ❌ **No URL parameters**: Extracting `/user/123` requires manual parsing
6. ❌ **No 404 handling**: Manual else condition for every route
7. ❌ **Performance issues**: Re-rendering entire app on every navigation
8. ❌ **No code splitting**: Can't lazy load routes
9. ❌ **Back button complexity**: Must handle state restoration manually
10. ❌ **Scroll restoration**: Must manually save/restore scroll positions

**React Router solves ALL of these!**

***

#### **CORE CONCEPT: Declarative Routing**

Instead of imperative (manual if/else), React Router is **declarative**:

```javascript
// Imperative (manual)
if (path === '/about') {
  component = <About />;
}

// Declarative (React Router)
<Route path="/about" element={<About />} />
```

You **declare** what should render at each path, and React Router handles:

* URL matching
* Component rendering
* State management
* History management
* Everything!

***

### 🧩 THE BUILDING BLOCKS

#### **1. BrowserRouter - The Foundation**

```javascript
import { BrowserRouter } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      {/* All routing happens inside here */}
    </BrowserRouter>
  );
}
```

**What BrowserRouter does internally:**

```javascript
// Simplified implementation
function BrowserRouter({ children }) {
  const [location, setLocation] = useState(window.location);
  
  useEffect(() => {
    // Listen for back/forward buttons
    const handlePopState = () => {
      setLocation(window.location);
    };
    
    window.addEventListener('popstate', handlePopState);
    return () => window.removeEventListener('popstate', handlePopState);
  }, []);
  
  // Provide location and navigation to all children
  return (
    <RouterContext.Provider value={{ location, navigate }}>
      {children}
    </RouterContext.Provider>
  );
}
```

**Key points:**

* ✅ Wraps your entire app
* ✅ Uses HTML5 History API (`pushState`, `popstate`)
* ✅ Provides routing context to all child components
* ✅ Should only be used ONCE at the root of your app

**Why "Browser" Router?**

Because it uses the browser's native history API. There are other routers:

```javascript
// For testing or non-browser environments
<MemoryRouter>  // Stores history in memory (no URL change)

// For server-side rendering
<StaticRouter>  // For SSR, doesn't change

// For old browsers (uses hash in URL)
<HashRouter>  // URLs look like: myapp.com/#/about
```

**When to use HashRouter:**

```javascript
// BrowserRouter URLs (clean):
myapp.com/about
myapp.com/profile/123

// HashRouter URLs (with #):
myapp.com/#/about
myapp.com/#/profile/123
```

**Why would you use HashRouter?**

The `#` (hash) part of URL is NEVER sent to server:

```
User visits: myapp.com/#/about

Browser sends to server: GET /
                         ↑ Only this! The "#/about" stays in browser

Server responds: index.html
Browser JS reads: window.location.hash = "#/about"
React Router renders: <About /> component
```

**Use case:** When you CAN'T configure server fallback (like GitHub Pages, or legacy hosting).

***

#### **2. Routes and Route - The Matchers**

```javascript
import { Routes, Route } from 'react-router-dom';

<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
  <Route path="/contact" element={<Contact />} />
</Routes>
```

**What Routes does:**

```javascript
// Simplified implementation
function Routes({ children }) {
  const location = useLocation(); // Gets current URL
  
  // Find which <Route> matches current URL
  let matchedRoute = null;
  
  React.Children.forEach(children, (child) => {
    if (child.props.path === location.pathname) {
      matchedRoute = child;
    }
  });
  
  // Render only the matched route
  return matchedRoute ? matchedRoute.props.element : null;
}
```

**Critical behavior:**

```javascript
// Current URL: /about

<Routes>
  <Route path="/" element={<Home />} />         {/* Not rendered */}
  <Route path="/about" element={<About />} />   {/* RENDERED ✅ */}
  <Route path="/contact" element={<Contact />} /> {/* Not rendered */}
</Routes>

// Only <About /> renders! Other routes don't even mount.
```

**This is different from conditional rendering:**

```javascript
// This renders ALL components, just hides them
<div>
  {path === '/' && <Home />}
  {path === '/about' && <About />}
  {path === '/contact' && <Contact />}
</div>

// React Router only renders the matched route
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
</Routes>
// Only ONE component mounts at a time!
```

**Why this matters:**

```javascript
function ExpensiveComponent() {
  useEffect(() => {
    // This runs ONLY when component mounts
    const data = fetchHugeDataset(); // 100MB data
    const connection = openWebSocket();
    const interval = setInterval(updateStats, 1000);
    
    return () => {
      connection.close();
      clearInterval(interval);
    };
  }, []);
  
  return <div>Expensive stuff</div>;
}

// With React Router:
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/expensive" element={<ExpensiveComponent />} />
</Routes>

// When you're on /, ExpensiveComponent doesn't exist in memory!
// When you navigate to /expensive, it mounts and fetches data
// When you navigate away, it unmounts and cleans up ✅
```

***

#### **3. Link - The Navigator**

```javascript
import { Link } from 'react-router-dom';

<Link to="/about">About Us</Link>
```

**What Link does internally:**

```javascript
// Simplified implementation
function Link({ to, children }) {
  const navigate = useNavigate();
  
  return (
    <a 
      href={to}
      onClick={(e) => {
        e.preventDefault(); // Stop browser default navigation
        navigate(to);        // Use React Router navigation
      }}
    >
      {children}
    </a>
  );
}
```

**Why use Link instead of `<a>`?**

```javascript
// Regular <a> tag
<a href="/about">About</a>
// ❌ Causes full page reload
// ❌ Loses all React state
// ❌ Destroys and recreates everything

// React Router <Link>
<Link to="/about">About</Link>
// ✅ Client-side navigation
// ✅ Preserves React state
// ✅ Instant transition
// ✅ No page reload
```

**Real-world example:**

```javascript
function ShoppingCart() {
  const [items, setItems] = useState([laptop, mouse]);
  const [total, setTotal] = useState(1500);
  
  return (
    <div>
      <h1>Cart: ${total}</h1>
      
      {/* Using <a> - BAD ❌ */}
      <a href="/checkout">Checkout</a>
      // Click this → Page reloads → items and total LOST!
      
      {/* Using <Link> - GOOD ✅ */}
      <Link to="/checkout">Checkout</Link>
      // Click this → No reload → items and total PRESERVED!
      // You can pass them to checkout page via state
    </div>
  );
}
```

**Link features:**

```javascript
// Basic usage
<Link to="/about">About</Link>

// Passing state (hidden from URL)
<Link to="/profile" state={{ fromDashboard: true }}>
  View Profile
</Link>
// Access in target component: const location = useLocation();
// location.state.fromDashboard === true

// Replace instead of push (no back button entry)
<Link to="/login" replace>Login</Link>

// Relative paths (from current route)
<Link to="../settings">Settings</Link>
<Link to="./profile">Profile</Link>
```

***

#### **4. NavLink - The Smart Link**

```javascript
import { NavLink } from 'react-router-dom';

<NavLink to="/about">About</NavLink>
```

**What makes NavLink special?**

It automatically adds a class/style when the link is **active** (matches current URL):

```javascript
<NavLink 
  to="/about"
  className={({ isActive }) => isActive ? 'active-link' : 'link'}
>
  About
</NavLink>

// When you're on /about:
// <a class="active-link" href="/about">About</a>

// When you're on /:
// <a class="link" href="/about">About</a>
```

**Real-world navigation bar:**

```javascript
function Navbar() {
  return (
    <nav>
      <NavLink 
        to="/"
        className={({ isActive }) => isActive ? 'nav-active' : 'nav-link'}
      >
        Home
      </NavLink>
      
      <NavLink 
        to="/products"
        className={({ isActive }) => isActive ? 'nav-active' : 'nav-link'}
      >
        Products
      </NavLink>
      
      <NavLink 
        to="/about"
        className={({ isActive }) => isActive ? 'nav-active' : 'nav-link'}
      >
        About
      </NavLink>
    </nav>
  );
}
```

```css
/* Your CSS */
.nav-link {
  color: gray;
  text-decoration: none;
  padding: 10px;
}

.nav-active {
  color: blue;
  font-weight: bold;
  border-bottom: 2px solid blue;
}
```

**Result:**

```
Current URL: /products

Navbar renders:
[Home]  [Products]  [About]
gray    blue+bold   gray
        ↑ Automatically highlighted!
```

**Without NavLink, you'd have to do:**

```javascript
function Navbar() {
  const location = useLocation();
  
  return (
    <nav>
      <Link 
        to="/"
        className={location.pathname === '/' ? 'nav-active' : 'nav-link'}
      >
        Home
      </Link>
      
      <Link 
        to="/products"
        className={location.pathname === '/products' ? 'nav-active' : 'nav-link'}
      >
        Products
      </Link>
      
      {/* Repeat for every link... */}
    </nav>
  );
}
```

**NavLink saves you from this repetition!**

***

#### **NavLink Advanced Features:**

```javascript
// Style instead of className
<NavLink
  to="/about"
  style={({ isActive }) => ({
    color: isActive ? 'red' : 'black',
    fontWeight: isActive ? 'bold' : 'normal'
  })}
>
  About
</NavLink>

// Custom active check
<NavLink
  to="/products"
  className={({ isActive, isPending }) => {
    if (isPending) return 'loading';  // During navigation
    if (isActive) return 'active';
    return 'link';
  }}
>
  Products
</NavLink>

// End prop - only active if EXACT match
<NavLink to="/" end>
  Home
</NavLink>
// Without "end": Active on /, /about, /products (all match "/")
// With "end": Active ONLY on /
```

***

### 🎯 Complete Example - Everything Together:

```javascript
import { BrowserRouter, Routes, Route, Link, NavLink } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav className="navbar">
        <NavLink 
          to="/" 
          end
          className={({ isActive }) => isActive ? 'active' : ''}
        >
          Home
        </NavLink>
        
        <NavLink 
          to="/products"
          className={({ isActive }) => isActive ? 'active' : ''}
        >
          Products
        </NavLink>
        
        <NavLink 
          to="/about"
          className={({ isActive }) => isActive ? 'active' : ''}
        >
          About
        </NavLink>
      </nav>
      
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/products" element={<Products />} />
        <Route path="/about" element={<About />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

function Home() {
  return (
    <div>
      <h1>Home Page</h1>
      <Link to="/products">Browse Products</Link>
    </div>
  );
}

function Products() {
  return (
    <div>
      <h1>Products</h1>
      <Link to="/" state={{ from: 'products' }}>
        Back to Home
      </Link>
    </div>
  );
}

function About() {
  return <h1>About Us</h1>;
}

function NotFound() {
  return (
    <div>
      <h1>404 - Page Not Found</h1>
      <Link to="/">Go Home</Link>
    </div>
  );
}
```

***

Bhai, theory bahut ho gaya. Ab tum batao:

**Samajh aaya ki:**

1. BrowserRouter kya karta hai?
2. Link vs `<a>` tag me kya farak hai?
3. NavLink kab use karoge?

Quick answer do, then I'll give you **CODING PROBLEMS**! 🚀
