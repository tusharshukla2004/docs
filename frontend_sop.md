# 🏗️ Frontend Development SOP (Standard Operating Procedure)
### For React / Vite / Next.js — E-Commerce & Client Projects

> **Purpose:** A practical, phase-wise, industry-standard guide for every frontend project you build as a professional Frontend Developer.

---

## 📋 PROJECT KICKOFF CHECKLIST
*Run this before touching any code.*

- [ ] Client requirements document received?
- [ ] Reference website / design file (Figma/XD) received?
- [ ] Company template / boilerplate received?
- [ ] Backend API documentation received (or backend developer contact)?
- [ ] Project deadline confirmed?
- [ ] Tech stack confirmed (React/Next.js, Tailwind/MUI)?
- [ ] Hosting/Deployment platform confirmed (Vercel, Netlify, VPS)?
- [ ] Who will provide images, content, and product data?

---

## 🗓️ DAILY DEVELOPMENT WORKFLOW

```
Morning (Start of Day):
1. Pull latest code → git pull origin main
2. Check yesterday's incomplete tasks
3. Plan today's 3 main targets (write them down)

During Development:
4. Build ONE feature at a time — don't jump between features
5. Test every component immediately after building
6. Commit small, meaningful changes often

End of Day:
7. git add . → git commit -m "feat: added product card component"
8. Push code → git push
9. Write tomorrow's task list
10. Note any blockers or questions for backend team
```

---

## PHASE 1: REQUIREMENT ANALYSIS

### 1.1 How to Analyze Client Requirements

**Step 1 — Read everything once without writing anything.**
Understand the big picture first. What TYPE of website is this?
- Simple Product Showcase?
- Full E-commerce with Cart + Payment?
- E-commerce + MLM/Affiliate?
- B2B or B2C?

**Step 2 — Create a Feature List (Must-Have vs Nice-to-Have)**

| Feature | Priority | Notes |
|---|---|---|
| User Login/Register | Must-Have | OTP or Email? |
| Product Listing | Must-Have | Filters needed? |
| Cart & Checkout | Must-Have | COD or Online? |
| Wishlist | Nice-to-Have | Add later |
| Product Reviews | Nice-to-Have | Phase 2 |

**Step 3 — Ask the RIGHT questions to the client:**
- How many products will be listed? (Affects pagination strategy)
- Is there a search/filter requirement?
- Which payment gateway? (Razorpay, Stripe, etc.)
- Is there a user dashboard after login?
- Mobile-first or Desktop-first design?
- Will there be admin panel? (Separate project or same codebase?)

### 1.2 How to Study a Reference Website

Go through the reference website systematically — don't just "look" at it:

```
1. PAGES AUDIT — List every page you can find:
   / (Home)
   /shop (Product Listing)
   /product/:id (Product Detail)
   /cart
   /checkout
   /login, /register
   /user/dashboard, /user/orders ...

2. SECTION AUDIT — For each page, list sections:
   Homepage: Navbar, Hero Slider, Categories, Featured Products,
             Banner Ad, Testimonials, Newsletter, Footer

3. COMPONENT AUDIT — Find repeating UI elements:
   - Product Card (used on Home, Shop, Wishlist pages)
   - Rating Stars (used on Product Card and Product Detail)
   - Breadcrumb (used on all inner pages)

4. INTERACTION AUDIT — Note all interactions:
   - Hover effects on cards
   - Add to Cart animation
   - Sticky header on scroll
   - Mobile menu open/close
```

### 1.3 How to Identify Reusable Sections

**Rule:** If a UI element appears MORE THAN ONCE → Make it a Component.

```
REUSABLE → Make Component:
✅ Product Card (Home, Shop, Search Results, Wishlist)
✅ Section Heading (H2 + subtitle, used everywhere)
✅ Button (Primary, Secondary, Outlined variants)
✅ Input Field (Login, Checkout, Search forms)
✅ Table (Orders, Incomes, Downlines)
✅ Modal/Popup (Cart added, Confirm delete)
✅ Loader/Spinner (every page that fetches data)

NOT REUSABLE → Keep inside Page:
❌ Hero Banner (unique to homepage)
❌ Specific checkout form layout
```

### 1.4 Feature Checklist Template (E-Commerce)

```
AUTHENTICATION:
- [ ] Register (Name, Email, Phone, Password)
- [ ] Login (Email + Password)
- [ ] Forgot Password (OTP via Email/SMS)
- [ ] Remember Me / Auto Login (redux-persist)

PRODUCT:
- [ ] Product Listing with Pagination
- [ ] Filter (by category, price, rating)
- [ ] Search
- [ ] Sort (Price Low-High, Newest)
- [ ] Product Detail Page
- [ ] Image Gallery with Thumbnails
- [ ] Related Products

CART & CHECKOUT:
- [ ] Add to Cart
- [ ] Update Quantity
- [ ] Remove from Cart
- [ ] Apply Coupon Code
- [ ] Address Form with Validation
- [ ] Payment Gateway Integration

USER DASHBOARD:
- [ ] My Orders (with status tracking)
- [ ] My Profile (Edit)
- [ ] My Addresses
- [ ] Change Password
- [ ] Logout
```

---

## PHASE 2: PROJECT SETUP

### 2.1 Understanding Company Folder Structure

When you receive the company template, spend **1 full hour** reading it:

```bash
# Run this to see the full structure at a glance:
find src -type f -name "*.jsx" | head -50
```

- Open every file in `src/api/` — understand what APIs are pre-built.
- Open every file in `src/Component/` — note what UI blocks are ready.
- Open `tailwind.config.js` — note existing brand colors and fonts.
- Open `src/store/` — understand the Redux slice structure.
- Open `RouterPages.jsx` — understand how routing is organized.

### 2.2 Environment Setup

```bash
# Step 1: Copy template to new project
cp -r company-template new-client-project
cd new-client-project

# Step 2: Install dependencies
npm install

# Step 3: Create environment file
touch .env

# Step 4: Add backend URL to .env
VITE_API_BASE_URL=https://api.newclient.com
VITE_RAZORPAY_KEY=rzp_live_xxxx

# Step 5: Run the project
npm run dev
```

> [!CAUTION]
> Never hardcode API URLs in component files. Always use `.env`. Never push `.env` to GitHub.

### 2.3 Naming Conventions (Industry Standard)

| Type | Convention | Example |
|---|---|---|
| Components | PascalCase | `ProductCard.jsx` |
| Pages | PascalCase | `HomePage.jsx` |
| API files | camelCase | `products.api.js` |
| Utility files | camelCase | `formatPrice.js` |
| Variables | camelCase | `isLoading`, `productList` |
| Constants | UPPER_CASE | `BASE_URL`, `MAX_RETRY` |
| Functions | camelCase | `handleAddToCart()` |
| Boolean vars | is/has/show prefix | `isLoggedIn`, `hasError` |

### 2.4 Coding Standards

```jsx
// ✅ GOOD — Small, focused functions
const formatPrice = (price) => `₹${Number(price).toFixed(2)}`;

// ✅ GOOD — Early return for clean code
if (!product) return <p>Product not found</p>;
if (loading) return <Loader />;
return <div>{/* actual JSX */}</div>;

// ✅ GOOD — Meaningful variable names
const isUserAuthenticated = useSelector(state => state.user.isAuthenticated);

// ❌ BAD — Confusing names
const x = useSelector(s => s.u.a);
```

---

## PHASE 3: ARCHITECTURE PLANNING

### 3.1 Standard Folder Organization

```
src/
├── api/                    # All backend API calls
│   ├── user.api.js
│   ├── products.api.js
│   ├── cart.api.js
│   └── order.api.js
│
├── assets/                 # Images, icons, fonts
│
├── Component/              # Reusable UI Components
│   ├── ui/                 # Generic: Button, Input, Modal, Table
│   ├── landing/            # Landing page specific components
│   └── common/             # Navbar, Footer, Sidebar, PageLoader
│
├── constants/              # Fixed values
│   ├── Routes.js           # All URL paths
│   └── config.js           # Base URL, API keys
│
├── hooks/                  # Custom React Hooks
│   ├── useFetch.js
│   └── useAuth.js
│
├── layout/                 # Page structure wrappers
│   ├── DashboardLayout.jsx
│   └── WebsiteLayout.jsx
│
├── store/                  # Redux store
│   ├── store.js
│   └── slice/
│       ├── userSlice.js
│       └── cartSlice.js
│
├── utils/                  # Helper functions
│   ├── formatPrice.js
│   └── dateFormatter.js
│
├── Website/                # Public pages (no login required)
│   ├── Home.jsx
│   ├── Shop.jsx
│   ├── ProductDetails.jsx
│   ├── Cart.jsx
│   └── landingSection/
│       ├── HeroSlider.jsx
│       └── FeaturedProducts.jsx
│
├── UserPanel/              # Private pages (login required)
│   └── pages/
│       ├── Dashboard/
│       ├── Orders/
│       └── Profile/
│
├── App.jsx
├── main.jsx
├── RouterPages.jsx
└── index.css
```

### 3.2 Component Hierarchy (Atomic Design)

```
ATOMS (Smallest — pure UI, no logic):
→ Button.jsx, Input.jsx, Badge.jsx, Spinner.jsx, StarRating.jsx

MOLECULES (Atoms combined):
→ SearchBar.jsx (Input + Button)
→ ProductCard.jsx (Image + Title + Price + Button)
→ FormField.jsx (Label + Input + Error message)

ORGANISMS (Full sections):
→ Navbar.jsx (Logo + SearchBar + CartIcon + UserMenu)
→ ProductGrid.jsx (multiple ProductCards + Pagination)
→ CheckoutForm.jsx (multiple FormFields + Button)

PAGES (Organisms arranged into full pages):
→ HomePage.jsx (HeroSlider + ProductGrid + Reviews)
→ CartPage.jsx (CartItems + OrderSummary)
```

### 3.3 Route Planning

```javascript
// Plan all routes BEFORE coding
const ROUTES = {
  // Public Routes
  HOME:            "/",
  SHOP:            "/shop",
  PRODUCT_DETAIL:  "/product/:id",
  CART:            "/cart",
  CHECKOUT:        "/checkout",
  LOGIN:           "/login",
  REGISTER:        "/register",

  // Protected Routes (login required)
  DASHBOARD:       "/user/dashboard",
  MY_ORDERS:       "/user/orders",
  MY_PROFILE:      "/user/profile",
  CHANGE_PASSWORD: "/user/change-password",
};
```

### 3.4 State Management Decision

| Data Type | Where to Store | Example |
|---|---|---|
| Global (shared across pages) | Redux | User info, Cart count |
| Local (single component) | useState | Is modal open? Form values |
| Server (from API) | useState in page | Product list, Order history |

---

## PHASE 4: UI/UX PLANNING

### 4.1 Design System

**Color System:**
```javascript
// tailwind.config.js
colors: {
  primary:   '#0f7d99',  // Brand color (buttons, links)
  secondary: '#172233',  // Dark (headings)
  accent:    '#f59e0b',  // Stars, sale badges
  light:     '#ebf5fb',  // Backgrounds
  danger:    '#ef4444',  // Errors, Out of Stock
  success:   '#10b981',  // In Stock, Success messages
}
```

**Typography System:**
```
Heading 1 (Page Title):    text-4xl font-bold
Heading 2 (Section Title): text-3xl font-bold
Heading 3 (Card Title):    text-xl font-semibold
Body:                      text-sm (14px)
Caption:                   text-xs (12px)
```

**Spacing System (Stick to these):**
```
4px  → p-1, m-1
8px  → p-2, m-2
16px → p-4, m-4   ← Most used
24px → p-6, m-6
32px → p-8, m-8
48px → p-12, m-12 ← Section padding
```

### 4.2 Responsive Strategy (Mobile First)

```jsx
// ALWAYS write mobile first, then scale up:
<div className="
  w-full        /* Mobile: full width */
  sm:w-1/2      /* Tablet: half width */
  lg:w-1/4      /* Desktop: 4 columns */
">
```

**Standard Breakpoints:**
```
sm:  640px  → Large phones
md:  768px  → Tablets
lg:  1024px → Laptops
xl:  1280px → Desktops
```

### 4.3 Accessibility Checklist
- [ ] Every `<img>` has `alt` attribute
- [ ] Every icon-only `<button>` has `aria-label`
- [ ] Forms have `<label>` tags linked to inputs
- [ ] Color contrast ratio is minimum 4.5:1
- [ ] Interactive elements reachable via Tab key

---

## PHASE 5: COMPONENT DEVELOPMENT

### 5.1 Props Design

```jsx
// Plan Props BEFORE writing the component:
const ProductCard = ({
  image,
  title,
  price,
  originalPrice = null,    // Optional with default
  onAddToCart,
}) => {
  const discount = originalPrice
    ? Math.round(((originalPrice - price) / originalPrice) * 100)
    : null;

  return (
    <div className="bg-white rounded-xl shadow-sm p-4">
      <img src={image} alt={title} className="w-full h-48 object-cover" />
      <h3 className="font-semibold mt-2">{title}</h3>
      <div className="flex items-center gap-2 mt-1">
        <span className="font-bold">₹{price}</span>
        {originalPrice && (
          <span className="text-sm text-gray-400 line-through">₹{originalPrice}</span>
        )}
        {discount && (
          <span className="text-xs text-red-600 bg-red-50 px-2 py-0.5 rounded">
            {discount}% OFF
          </span>
        )}
      </div>
      <button onClick={onAddToCart} className="w-full mt-3 bg-primary text-white py-2 rounded-lg">
        Add to Cart
      </button>
    </div>
  );
};
```

### 5.2 Common Component Checklist

```
UI COMPONENTS:
- [ ] Button (variants: primary, secondary, outlined, danger)
- [ ] Input (with label, error state, helper text)
- [ ] Modal (with overlay and close button)
- [ ] Spinner (small inline + large full page)
- [ ] Breadcrumb
- [ ] Pagination
- [ ] Empty State (when no data)
- [ ] Toast/Alert Notification

E-COMMERCE SPECIFIC:
- [ ] ProductCard
- [ ] CartItem row
- [ ] StarRating
- [ ] PriceTag (with discount logic)
- [ ] QuantitySelector (+ / -)
- [ ] ImageGallery (with thumbnails)
- [ ] OrderStatusBadge
```

---

## PHASE 6: PAGE DEVELOPMENT

### 6.1 Homepage Workflow
```
1. Navbar (Logo, Menu, Cart Icon, Search)
2. Hero Slider / Banner
3. Featured Categories section
4. Product Grid (reuse ProductCard)
5. Promotional Banner
6. Testimonials / Reviews
7. Newsletter CTA
8. Footer
```

### 6.2 Product Listing Page Workflow
```
1. Page layout (Sidebar filters + Product Grid)
2. Fetch products from API on mount (useEffect)
3. Show loading state while fetching
4. Map products → <ProductCard />
5. Implement Pagination
6. Add Filter (Category, Price Range)
7. Add Sort dropdown
8. Add Search (with debounce — wait 500ms before calling API)
```

### 6.3 Product Detail Page Workflow
```
1. Get product ID from URL → useParams()
2. Scroll to top on mount → window.scrollTo(0,0)
3. Fetch product from API
4. Image Gallery with thumbnail selector
5. Title, Price, Discount, Stock status
6. Quantity Selector
7. Add to Cart / Go to Cart button
8. Product Description
9. Related Products
```

### 6.4 Cart Page Workflow
```
1. Fetch cart items from API or Redux
2. Each item: Image, Title, Price, Quantity, Remove button
3. Quantity update → API call → refresh display
4. Calculate: Subtotal, Discount, Delivery, Total
5. Apply Coupon Code section
6. Proceed to Checkout (protected — check login first)
```

### 6.5 Checkout & Payment Workflow
```
1. Show Order Summary (readonly)
2. Address Form with Validations:
   - Name, Phone (10 digits), Street, City, State, Pincode
3. Payment method selection
4. On Confirm → Call Razorpay / Payment Gateway
5. On Success → Create Order in Backend
6. Redirect to Order Confirmation page
```

### 6.6 Authentication Workflow
```
LOGIN:
1. Form: Email + Password
2. Validate on submit
3. Call loginAPI()
4. Success → dispatch(setUser(data)) → redirect Dashboard
5. Error → show SweetAlert error

REGISTER:
1. Form: Name, Email, Phone, Password, Confirm Password
2. Validate all fields
3. Call registerAPI()
4. Success → redirect to Login
```

---

## PHASE 7: API INTEGRATION

### 7.1 Service Layer (API Folder Pattern)

```javascript
// src/api/products.api.js
import axios from "axios";

const BASE_URL = import.meta.env.VITE_API_BASE_URL;

export const getAllProducts = async (filters = {}) => {
  const response = await axios.get(`${BASE_URL}/products`, { params: filters });
  return response.data;
};

export const getSingleProduct = async (id) => {
  const response = await axios.get(`${BASE_URL}/products/${id}`);
  return response.data;
};
```

### 7.2 Standard Error Handling Pattern

```javascript
const [data, setData] = useState([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);

useEffect(() => {
  const fetchData = async () => {
    try {
      setLoading(true);
      const result = await getAllProducts();
      setData(result.products);
    } catch (err) {
      setError(err?.response?.data?.message || "Something went wrong");
    } finally {
      setLoading(false); // Always runs — success OR failure
    }
  };
  fetchData();
}, []);

// In JSX:
if (loading) return <PageLoader />;
if (error) return <ErrorMessage message={error} />;
if (!data.length) return <EmptyState message="No products found" />;
return <ProductGrid data={data} />;
```

### 7.3 Axios Instance with Auth Token

```javascript
// src/api/axiosInstance.js
import axios from "axios";

const axiosInstance = axios.create({
  baseURL: import.meta.env.VITE_API_BASE_URL,
});

// Auto attach token to every request
axiosInstance.interceptors.request.use((config) => {
  const token = JSON.parse(localStorage.getItem("user"))?.token;
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// Handle 401 Unauthorized globally (token expired)
axiosInstance.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      localStorage.removeItem("user");
      window.location.href = "/login";
    }
    return Promise.reject(error);
  }
);

export default axiosInstance;
```

---

## PHASE 8: PERFORMANCE OPTIMIZATION

### 8.1 Lazy Loading (Load pages only when visited)

```javascript
// ❌ BAD: All pages load at startup
import ShopPage from "./Website/Shop";

// ✅ GOOD: Pages load only when user navigates there
import { lazy, Suspense } from "react";
const ShopPage = lazy(() => import("./Website/Shop"));
const ProductDetails = lazy(() => import("./Website/ProductDetails"));

// Wrap routes in Suspense:
<Suspense fallback={<PageLoader />}>
  <Routes>
    <Route path="/shop" element={<ShopPage />} />
  </Routes>
</Suspense>
```

### 8.2 Image Optimization

```jsx
<img
  src={product.image}
  alt={product.name}
  width={300}
  height={300}
  loading="lazy"        // Loads only when image is near viewport
  className="object-cover"
/>
```

### 8.3 Memoization

```jsx
import { useMemo, useCallback, memo } from "react";

// memo — skip re-render if props unchanged
const ProductCard = memo(({ title, price }) => <div>...</div>);

// useMemo — cache expensive calculations
const filteredProducts = useMemo(() =>
  products.filter(p => p.category === selectedCategory),
  [products, selectedCategory]
);

// useCallback — stable function reference across renders
const handleAddToCart = useCallback((productId) => {
  dispatch(addToCart(productId));
}, [dispatch]);
```

### 8.4 Performance Checklist
- [ ] Lazy loading on all route-level pages
- [ ] `loading="lazy"` on all product images
- [ ] `useMemo` on filter/search functions
- [ ] `memo` on ProductCard and repeated components
- [ ] No `console.log` in production build
- [ ] Bundle size analyzed before deploying (`npm run build`)

---

## PHASE 9: SCALABILITY & MAINTAINABILITY

### 9.1 Rules for Scalable Code

```
RULE 1: One File, One Job
→ ProductCard.jsx does ONE thing — show a product card.
→ Don't put cart API logic inside ProductCard.

RULE 2: Constants over Magic Values
// ❌ BAD:
if (user.role === 3) { }

// ✅ GOOD:
const ROLES = { ADMIN: 1, USER: 2, SELLER: 3 };
if (user.role === ROLES.SELLER) { }

RULE 3: Custom Hooks for Repeated Logic
// Make useFetch() instead of copy-pasting
// loading + error + try-catch in 10 different pages

RULE 4: Avoid Deep Prop Drilling
// If props go more than 2 levels deep → use Context or Redux
```

### 9.2 Custom Hook (Eliminate Repetition)

```javascript
// src/hooks/useFetch.js
import { useState, useEffect } from "react";

const useFetch = (fetchFunction, dependencies = []) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const execute = async () => {
      try {
        setLoading(true);
        const result = await fetchFunction();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    execute();
  }, dependencies);

  return { data, loading, error };
};

export default useFetch;

// Usage in ANY page — zero repetition:
const { data: products, loading, error } = useFetch(getAllProducts);
```

---

## PHASE 10: TESTING & QA

### Functional Testing Checklist
- [ ] Register with valid data → Success message?
- [ ] Register with existing email → Error shown?
- [ ] Login with wrong password → Error shown?
- [ ] Add to cart → Cart count updates in Navbar?
- [ ] Remove from cart → Item removed instantly?
- [ ] Checkout with empty address → Validation errors shown?
- [ ] Place order → Redirects to success page?

### UI Testing Checklist
- [ ] All fonts loading correctly?
- [ ] All images visible (no broken images)?
- [ ] Hover effects working?
- [ ] Loading spinners showing during API calls?
- [ ] Empty states showing when no data?
- [ ] Error states showing on API failure?

### Responsive Testing Checklist
- [ ] Mobile (375px — iPhone SE)
- [ ] Tablet (768px — iPad)
- [ ] Laptop (1280px)
- [ ] Large Screen (1920px)
- [ ] Navbar hamburger menu works on mobile?
- [ ] Product grid changes columns on mobile?
- [ ] Tables scrollable horizontally on mobile?

### Browser Compatibility Checklist
- [ ] Google Chrome
- [ ] Mozilla Firefox
- [ ] Safari (Mac/iPhone)
- [ ] Microsoft Edge

---

## PHASE 11: DEPLOYMENT

### Pre-Deployment Checklist
- [ ] All `console.log` removed from code
- [ ] `.env` file is in `.gitignore`
- [ ] `npm run build` runs without errors
- [ ] API URL points to production server (not localhost)
- [ ] Payment gateway is in LIVE mode (not test mode)
- [ ] All images load from correct URLs

### Build & Deploy (Vercel — Recommended)

```bash
# Step 1: Build production files
npm run build

# Step 2: Test production build locally
npm run preview

# Step 3: Push to GitHub
git add .
git commit -m "chore: production build ready"
git push origin main

# Step 4: Connect GitHub to Vercel → Auto deploys on push!
```

### Post-Deployment Verification
- [ ] Live URL loads successfully?
- [ ] Login works on live site?
- [ ] Add to Cart works on live site?
- [ ] Payment gateway processes a test payment?
- [ ] Mobile view correct on live URL?
- [ ] All images loading (no broken)?

---

## PHASE 12: PROJECT DELIVERY

### Code Review Checklist
- [ ] No unused imports in any file (ESLint will catch these)
- [ ] No hardcoded URLs or API keys
- [ ] All async functions have try-catch blocks
- [ ] No component longer than 200 lines (split if needed)

### Documentation Checklist
- [ ] `README.md` updated with setup instructions
- [ ] `.env.example` file created (placeholder values, NOT real keys)
- [ ] Folder structure explained in README

### Handover Checklist
- [ ] GitHub repository access given to client/team
- [ ] All `.env` variables shared via secure channel
- [ ] Deployment platform (Vercel) access given
- [ ] Walkthrough call done with client

---

## ⚠️ COMMON MISTAKES TO AVOID

> [!CAUTION]
> These are the most frequent mistakes junior developers make. Avoiding these separates good developers from great ones.

```
❌ Writing API calls directly inside components
✅ Always put API calls in src/api/ folder

❌ Not handling loading and error states
✅ Every API call must have: loading, success, and error handling

❌ Magic numbers and hardcoded strings in code
✅ Use constants: ROLES.ADMIN, ROUTES.HOME, etc.

❌ Building only for desktop and adding mobile at the end
✅ Mobile-first: build for mobile, then scale up to desktop

❌ One giant component with 400+ lines
✅ Break into smaller components — ideally max 150-200 lines

❌ Copy-pasting the same fetch+loading+error logic in 10 pages
✅ Create a custom hook: useFetch()

❌ Pushing .env file to GitHub
✅ Always add .env to .gitignore before first commit

❌ Not using window.scrollTo(0, 0) on page navigation
✅ Scroll to top when any new page/route loads

❌ Testing only on your own machine and browser
✅ Test on Chrome, Firefox, Safari, and mobile devices

❌ Large, vague commit messages: "updated files"
✅ Small, specific commits: "feat: add product filter by price"
```

---

## 🏆 BEST PRACTICES (Senior Developer Habits)

```
1. COMMIT OFTEN, COMMIT SMALL
   → "feat: add product card hover effect" is better than
   → "updated many files"

2. GIT BRANCH STRATEGY
   → main        (production only — never commit directly)
   → dev         (active development)
   → feature/xxx (each new feature gets its own branch)

3. FOLDER-BASED THINKING
   → Before touching code, decide WHERE the file goes.
   → Is it a Component? API? Page? Utility? Hook?

4. DRY PRINCIPLE (Don't Repeat Yourself)
   → If you write the same code twice → make it a function/component.

5. REVIEW YOUR OWN CODE BEFORE PUSHING
   → Read your own diff. Would a senior developer approve this?

6. PERFORMANCE IS A FEATURE — NOT AN AFTERTHOUGHT
   → Add lazy loading and memo from Day 1.

7. WRITE FOR THE NEXT DEVELOPER
   → Your teammate (or future you) should understand your code
     without calling you.

8. ESLint + Prettier from Day 1
   → These tools catch bugs and keep code clean automatically.
   → Setup once, benefit forever.
```

---

## 📦 REUSABLE E-COMMERCE TEMPLATE (Quick Reuse Guide)

When reusing the `parinam-frontend` template for a new project:

```
[ ] 1. Copy template → rename project in package.json
[ ] 2. npm install
[ ] 3. Update tailwind.config.js → new brand colors
[ ] 4. Replace logo in /public and /src/assets
[ ] 5. Create .env → add new backend BASE_URL
[ ] 6. Remove unneeded features (MLM? Admin panel? etc.)
[ ] 7. Update RouterPages.jsx → add/remove routes
[ ] 8. Update src/api/ files → match new backend endpoint names
[ ] 9. Update product data structure if field names differ
       (e.g., old: productName, new: title)
[ ] 10. Run project → test all pages work
[ ] 11. Replace placeholder images with client images
[ ] 12. Update all text content (headings, about section)
[ ] 13. Test full user flow: Register → Login → Add to Cart → Checkout
[ ] 14. npm run build → npm run preview → verify production build
[ ] 15. Deploy → Verify live URL
```

---

*Document Version: 1.0*
*Stack: React 19, Vite, Tailwind CSS, Redux Toolkit, Axios, React Router v7*
*Created for: Professional Frontend Development Workflow*
