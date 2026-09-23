# Frontend E-commerce Development SOP & Master Blueprint

As a Senior Frontend Architect, I have designed this Standard Operating Procedure (SOP) to bridge the gap between your company’s predefined architecture and the unique requirements of your clients. Following this document ensures every project you build is scalable, maintainable, performant, and production-ready.

---

## Phase 1: Requirement Analysis Phase

Before writing a single line of code, you must deconstruct the client's vision.

### 1. Analyzing Client Requirements
- Read the Business Requirement Document (BRD) or feature list thoroughly.
- Identify the core user personas (e.g., Guest User, Logged-in Customer, Admin).
- Map out the primary user journeys (e.g., Search -> Product Detail -> Cart -> Checkout).

### 2. Studying the Reference Website
- **Visual Teardown:** Open the reference site and take full-page screenshots.
- **Inspect Element:** Look at their DOM structure, layout grids, and responsive breakpoints.
- **Behavioral Analysis:** Note interactions (hover states, sticky headers, modal behaviors, toast notifications).
- **Performance:** Run a quick Lighthouse audit on the reference site to set a baseline for your own build.

### 3. Identifying Reusable Sections
- Break the reference design into repeated blocks: 
  - *Atoms:* Buttons, Inputs, Badges, Typography.
  - *Molecules:* Product Cards, Search Bars, Form fields.
  - *Organisms:* Header, Footer, Hero Sliders, Product Grids, Cart Drawer.
- Check these against your company's existing component library. Identify what can be reused directly, what needs customization, and what must be built from scratch.

### 4. Feature Checklist Creation
- [ ] Authentication (Login, Register, Forgot Password, OTP).
- [ ] Catalog (Category pages, Filters, Sorting, Pagination/Infinite Scroll).
- [ ] Product (Image gallery, Variants, Add to Cart, Reviews).
- [ ] Cart & Checkout (Mini-cart, Promo codes, Shipping address, Payment gateway).
- [ ] User Dashboard (Order history, Profile, Addresses, Wishlist).
- [ ] Static Pages (About, Contact, T&C, Privacy Policy).

---

## Phase 2: Project Setup Phase

Aligning the new project with the company's ecosystem.

### 1. Understanding Company Structure
- Clone the base company boilerplate/template.
- Review the `package.json` to understand the tech stack (React/Next, Vite, Tailwind, Redux/Zustand, React Query).
- Locate the global configuration files (`.env.example`, `tailwind.config.js`, `vite.config.js`).

### 2. Environment Setup
- Run `npm install` (or `pnpm`/`yarn` as per company standard).
- Setup your `.env.local` with development API URLs.
- Ensure Prettier and ESLint extensions are active in your VS Code and formatting on save.

### 3. Naming Conventions & Standards
- **Folders/Directories:** `kebab-case` (e.g., `product-details`, `user-panel`).
- **Components:** `PascalCase` (e.g., `ProductCard.jsx`, `Header.jsx`).
- **Hooks:** `camelCase` with 'use' prefix (e.g., `useCart.js`).
- **Utility/Helper Functions:** `camelCase` (e.g., `formatCurrency.js`, `calculateDiscount.js`).
- **Constants:** `UPPER_SNAKE_CASE` (e.g., `MAX_CART_ITEMS`, `API_BASE_URL`).

---

## Phase 3: Architecture Planning Phase

### 1. Folder Organization (Standard Feature-Sliced)
```text
src/
├── api/          # Axios instances and endpoint services
├── assets/       # Static images, icons, fonts
├── components/   # Reusable global UI (Buttons, Modals)
├── constants/    # Global static variables, Routes
├── hooks/        # Custom React hooks
├── layouts/      # MainLayout, AuthLayout, DashboardLayout
├── pages/        # Route components (Home, Shop, Checkout)
├── store/        # Redux/Zustand state management
└── utils/        # Helper functions (date formatters, validators)
```

### 2. Route & State Planning
- Map out URLs: `/`, `/shop`, `/product/:slug`, `/cart`, `/checkout`, `/account/*`.
- Determine what goes in **Global State** (User Auth, Cart Items, Theme) vs **Local State** (Form inputs, UI toggles like modal open/close).

### 3. API Integration Planning
- Define the base URL and API versioning.
- Plan API interceptors (attaching Bearer tokens to requests, handling 401 Unauthorized responses globally).

---

## Phase 4: UI/UX Planning Phase

### 1. Design System Configuration
- **Tailwind Config:** Open `tailwind.config.js` and inject the client's branding.
- **Colors:** Define `primary`, `secondary`, `accent`, `background`, `surface`, `error`, `success`.
- **Typography:** Import client fonts (Google Fonts/Custom) and define `fontFamily`.
- **Spacing:** Stick to the 4px/8px grid system (e.g., `p-2` = 8px, `p-4` = 16px).

### 2. Responsive Strategy
- Always design **Mobile-First**. 
- Base classes apply to mobile. Use `sm:`, `md:`, `lg:`, `xl:` to adjust for tablets and desktops.
- Plan the Header transition (Hamburger menu on mobile -> Standard nav on desktop).

### 3. Accessibility (a11y) Checklist
- [ ] All `<img>` tags have descriptive `alt` attributes.
- [ ] Buttons have `aria-label` if they are icon-only.
- [ ] Color contrast meets WCAG AA standards.
- [ ] Interactive elements are keyboard navigable (Tab indexing).

---

## Phase 5: Component Development Phase

### 1. Atomic Design Implementation
- Start small. Build the customized `Button`, `Input`, `Select`, and `Checkbox` components first. Ensure they accept all native HTML props (`...rest`).
- Move to Molecules: `SearchBar`, `QuantitySelector`, `PriceDisplay`.

### 2. Props Design
- Keep props predictable. E.g., a `ProductCard` should ideally take a single `product` object prop rather than 10 individual props (`title`, `price`, `image`, etc.).

### 3. Component Optimization
- Use `React.memo` only for heavy components that receive the exact same props frequently.
- Extract complex UI logic into custom hooks (e.g., `useProductFilter()`) to keep the JSX clean.

---

## Phase 6: Page Development Phase

### 1. Homepage Workflow
- Setup the Hero Banner (optimizing LCP image).
- Implement Category Grids.
- Implement Product Carousels (using Swiper/Slick). 
- *Pro Tip:* Fetch homepage data concurrently if using multiple endpoints.

### 2. PLP (Product Listing Page) Workflow
- Implement Sidebar Filters and Sorting Dropdowns.
- Connect URL query parameters to state (so users can share URLs with filters applied).
- Implement Pagination or Infinite Scroll.
- Add Skeleton loaders for fetching states.

### 3. PDP (Product Detail Page) Workflow
- Image Gallery with zoom capability.
- Variant selector (Size, Color) – ensure selecting a variant updates the price/SKU/stock.
- "Add to Cart" with Optimistic UI (show success immediately, revert if API fails).

### 4. Checkout & Cart Workflow
- Ensure Cart state persists (localStorage or Redux Persist).
- Build a robust multi-step form for checkout (Address -> Shipping Method -> Payment).
- Validate all inputs strictly before hitting the payment gateway.

---

## Phase 7: API Integration Phase

### 1. Service Layer Architecture
Never write `axios.get` directly inside components. Use the `api/` folder.
```javascript
// api/products.api.js
import axiosInstance from './axiosInstance';

export const getProducts = async (params) => {
  return axiosInstance.get('/products', { params });
};
```

### 2. Error Handling & Loaders
- Centralize error toasts in the Axios interceptor.
- Keep components clean by using `try/catch` wrappers or React Query/RTK Query which handles `isLoading` and `isError` automatically.

---

## Phase 8: Performance Optimization Phase

### 1. Optimization Checklist
- [ ] **Code Splitting:** Use `React.lazy()` for heavy, non-critical routes (like `/account` or `/checkout`).
- [ ] **Images:** Always compress images. Use WebP formats. Use `loading="lazy"` for images below the fold. (If Next.js, use `next/image`).
- [ ] **Bundle Size:** Remove heavy unused libraries (e.g., swap Moment.js for date-fns or dayjs).
- [ ] **Debouncing:** Apply debouncing on Search inputs to prevent API spam.

---

## Phase 9: Scalability & Maintainability Phase

### 1. Avoiding Code Duplication (DRY)
- If you copy-paste JSX more than twice, it needs to be a Component.
- If you copy-paste logic (like handling a form), it needs to be a Custom Hook.

### 2. Scalable CSS
- Avoid inline styles at all costs. 
- Use Tailwind classes. For complex dynamic classes, use the `clsx` or `tailwind-merge` utility libraries.

---

## Phase 10: Testing & QA Phase

### 1. Developer QA Checklist
- [ ] **Responsive Test:** Check on iPhone SE (320px width), iPad, and 1080p Desktop.
- [ ] **Cross-browser Test:** Chrome, Safari, Firefox.
- [ ] **State Test:** Add item to cart, refresh page, check if item is still there.
- [ ] **Error Test:** Disconnect internet, click a button, ensure a graceful error message appears, not a white screen.

---

## Phase 11: Deployment Phase

### 1. Pre-Deployment Checklist
- [ ] Run `npm run build`. Ensure it finishes without warnings/errors.
- [ ] Check `.env.production` – ensure API points to the live server, not localhost.
- [ ] Ensure all console.logs and debuggers are removed.

---

## Phase 12: Project Delivery Phase

### 1. Handover Checklist
- [ ] Update `README.md` with instructions on how to run the project.
- [ ] Document any hardcoded values or client-specific hacks.
- [ ] Hand over environment variable keys securely to the DevOps team.

---
---

# Extra Resources for the Frontend Architect

## 📅 Daily Development Workflow (Agile)
1. **Sync (Morning):** Pull the latest code from the `develop` branch (`git pull origin develop`).
2. **Review:** Check Jira/Trello for assigned tasks.
3. **Branching:** Create a feature branch (`git checkout -b feature/cart-drawer`).
4. **Develop:** Write code following the SOP.
5. **Self-Review (Afternoon):** Check your own code against the company standards before committing.
6. **Commit:** Write semantic commit messages (`feat: add cart drawer UI`, `fix: header alignment on mobile`).
7. **Push & PR:** Push code and raise a Pull Request.

## 🚀 Project Kickoff Checklist (Day 1)
- [ ] Clone company template.
- [ ] Rename project in `package.json`.
- [ ] Ask backend team for API Postman Collection / Swagger.
- [ ] Set up Tailwind theme config matching client design.
- [ ] Replace template logos, fonts, and favicons.
- [ ] Create basic empty route files for all required pages.

## 🛒 Reusable E-commerce Features Checklist
- [ ] Global Cart Drawer / Mini-cart.
- [ ] Toast notification system (Success/Error).
- [ ] Image fallback component (if product image fails to load, show a placeholder).
- [ ] Number formatter utility (e.g., formatting `15000` to `₹15,000.00`).
- [ ] Address validator logic.

## ❌ Common Mistakes to Avoid
1. **Prop Drilling:** Passing props down 5 levels deep. Use Redux or Context API instead.
2. **Massive Components:** Writing a 1000-line `Checkout.jsx` file. Break it down into `<AddressForm>`, `<PaymentMethods>`, `<OrderSummary>`.
3. **Hardcoding:** Hardcoding currency symbols (`$`), tax rates, or API URLs. These should come from config files or backend.
4. **Ignoring Loading States:** Making a user click a button and showing zero visual feedback while the API takes 3 seconds. Always use button spinners.

## 🌟 Best Practices by Senior Developers
1. **Optimistic UI:** Update the UI *before* the server responds. If the server fails, revert it. It makes the app feel lightning fast.
2. **Early Returns:** Instead of massive `if/else` blocks, return early if conditions aren't met to keep code flat and readable.
3. **Magic Strings:** Don't write `if (status === 'COMPLETED')`. Create a constant `export const ORDER_STATUS = { COMPLETED: 'COMPLETED' }` and use `ORDER_STATUS.COMPLETED`.

***

*Use this blueprint as your ultimate guide. Consistency is what transforms a junior developer into a senior architect. Happy coding!*
