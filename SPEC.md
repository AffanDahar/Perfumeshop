# Perfume Hub — Project Plan

## Table of Contents

1. [Technology Stack](#1-technology-stack)
2. [Overall Architecture](#2-overall-architecture)
3. [Major Application Modules](#3-major-application-modules)
4. [Development Roadmap](#4-development-roadmap)
5. [Phase 1 — Database & Authentication](#5-phase-1--database--authentication)
6. [Phase 2 — Brand, Category & Shop Management](#6-phase-2--brand-category--shop-management)
7. [Phase 3 — Product Management](#7-phase-3--product-management)
8. [Phase 4 — Public Storefront](#8-phase-4--public-storefront)
9. [Phase 5 — Search & Filtering](#9-phase-5--search--filtering)
10. [Phase 6 — Cart & Wishlist](#10-phase-6--cart--wishlist)
11. [Phase 7 — Checkout & Orders](#11-phase-7--checkout--orders)
12. [Phase 8 — Seller Dashboard](#12-phase-8--seller-dashboard)
13. [Phase 9 — Admin Dashboard](#13-phase-9--admin-dashboard)
14. [Phase 10 — Payments & Commission](#14-phase-10--payments--commission)
15. [Phase 11 — Reviews, Coupons & Promotions](#15-phase-11--reviews-coupons--promotions)
16. [Phase 12 — Production Hardening](#16-phase-12--production-hardening)
17. [Recommended Prisma Architecture](#17-recommended-prisma-architecture)
18. [Next.js Application Structure](#18-nextjs-application-structure)
19. [How Data Should Flow](#19-how-data-should-flow)
20. [Prisma Layer](#20-prisma-layer)
21. [Database Strategy](#21-database-strategy)
22. [Development Order](#22-development-order)
23. [MVP Scope](#23-mvp-scope)
24. [Suggested Milestones](#24-suggested-milestones)

---

## 1. Technology Stack

| Area                      | Technology                                                            |
| ------------------------- | --------------------------------------------------------------------- |
| Frontend                  | Next.js                                                               |
| Backend                   | Next.js Server Components + Server Actions / Route Handlers           |
| Language                  | TypeScript                                                            |
| Runtime / Package Manager | Node.js / npm                                                         |
| Styling                   | TailwindCSS                                                           |
| Database                  | Microsoft SQL Server                                                  |
| ORM                       | Prisma                                                                |
| Authentication            | Auth.js / NextAuth                                                    |
| Validation                | Zod                                                                   |
| State                     | React state + Server Actions; Zustand only where client state is necessary |
| File/Image Storage        | Cloud/object storage                                                  |
| API                       | Next.js Route Handlers                                                |
| Deployment                | Depends on hosting environment                                        |
| Version Control           | Git                                                                   |

---

## 2. Overall Architecture

The application will be a single Next.js full-stack application.

```text
                    PERFUME HUB
                        │
            ┌───────────┴───────────┐
            │                       │
       Buyer Portal            Management
            │                       │
            │              ┌────────┴────────┐
            │              │                 │
            │           Admin Portal     Seller Portal
            │
            └───────────────┬───────────────┘
                            │
                       Next.js
                            │
            ┌───────────────┼────────────────┐
            │               │                │
       Server Actions   Route Handlers   Server Components
            │               │                │
            └───────────────┼────────────────┘
                            │
                         Prisma
                            │
                       SQL Server
```

> [!IMPORTANT]
> **Key architectural decision:** Do not create a separate Node/Express backend initially.
> Next.js can handle both the UI and backend/business logic.

---

## 3. Major Application Modules

I recommend dividing the application into these modules:

1. Authentication
2. User Management
3. Seller Management
4. Shop Management
5. Brand Management
6. Category Management
7. Product Management
8. Product Variants
9. Inventory
10. Search & Filtering
11. Cart
12. Wishlist
13. Checkout
14. Orders
15. Payments
16. Shipping
17. Reviews
18. Coupons & Promotions
19. Notifications
20. Admin Dashboard
21. Seller Dashboard
22. Reports
23. Audit Logs

---

## 4. Development Roadmap

I would build this in 12 phases rather than trying to build everything simultaneously.

### Phase 0 — Project Foundation

#### Goal

Create the initial Next.js application and establish the development architecture.

#### Tasks

- [ ] Create Next.js project
- [ ] Configure TypeScript
- [ ] Configure TailwindCSS
- [ ] Configure ESLint
- [ ] Configure Prettier
- [ ] Configure environment variables
- [ ] Configure Git
- [ ] Configure SQL Server
- [ ] Install Prisma
- [ ] Initialize Prisma
- [ ] Configure Prisma → SQL Server connection
- [ ] Create initial database migration
- [ ] Establish folder architecture

#### Initial structure

```text
perfume-hub/
│
├── prisma/
│   ├── schema.prisma
│   ├── migrations/
│   └── seed.ts
│
├── public/
│
├── src/
│   ├── app/
│   ├── components/
│   ├── features/
│   ├── lib/
│   ├── services/
│   ├── actions/
│   ├── hooks/
│   ├── types/
│   ├── validations/
│   └── utils/
│
├── .env
├── package.json
├── package-lock.json
└── tsconfig.json
```

#### Deliverable

A running Next.js application connected to SQL Server through Prisma.

---

## 5. Phase 1 — Database & Authentication

### Goal

Establish users and authorization.

### Database entities

- `User`
- `Role`
- `Permission`
- `UserRole`
- `Session`
- `Account`
- `Address`

### Roles

Initially:

- `ADMIN`
- `SELLER`
- `BUYER`

Potentially later:

- `SUPER_ADMIN`
- `SUPPORT`

### Features

- Registration
- Login
- Logout
- Password hashing
- Email verification
- Forgot password
- Reset password
- Session management
- Role-based authorization
- Protected routes

### Deliverable

Users can register/login and access pages according to their role.

---

## 6. Phase 2 — Brand, Category & Shop Management

### Goal

Build the marketplace's foundational catalog.

### Entities

- `Brand`
- `Category`
- `Shop`
- `Seller`

### Relationships

```text
Seller
   │
   └── Shop

Brand
   │
   └── Product

Category
   │
   └── Product
```

### Admin features

Admin can:

- Create brand
- Update brand
- Delete/deactivate brand
- Create categories
- Manage categories
- Approve sellers
- Suspend sellers
- View shops

### Seller features

Seller can:

- Create shop profile
- Update shop information
- Upload shop logo
- Configure shop details

### Deliverable

A working marketplace structure containing sellers, shops, brands and categories.

---

## 7. Phase 3 — Product Management

This is one of the largest phases.

### Product model

A product should represent the actual perfume, independent of a particular seller.

For example:

- **Brand:** Dior
- **Product:** Sauvage Eau de Toilette

Then sellers create listings for that product:

```text
Dior Sauvage EDT

Seller A → Rs. 30,000
Seller B → Rs. 31,500
Seller C → Rs. 29,999
```

### Entities

- `Product`
- `ProductVariant`
- `ProductImage`
- `ProductNote`
- `ProductAttribute`
- `ProductListing`
- `Inventory`

### Product information

```text
Product
├── Name
├── Brand
├── Category
├── Description
├── Gender
├── Fragrance Family
├── Concentration
├── Country
├── Notes
├── Images
└── Status
```

### Variant

```text
Product
   │
   ├── 50ml
   ├── 100ml
   └── 200ml
```

### Seller listing

```text
Product
     │
     ├── Seller A
     │     ├── Price
     │     ├── Stock
     │     └── SKU
     │
     └── Seller B
           ├── Price
           ├── Stock
           └── SKU
```

### Deliverable

Admin and sellers can manage the perfume catalog.

---

## 8. Phase 4 — Public Storefront

Now build the customer-facing marketplace.

### Pages

```text
/
├── Home
├── /products
├── /products/[slug]
├── /brands
├── /brands/[slug]
├── /categories
├── /categories/[slug]
├── /shops
└── /shops/[slug]
```

### Homepage

Possible sections:

- Hero Banner
- Featured Brands
- Popular Perfumes
- New Arrivals
- Best Sellers
- Men's Perfumes
- Women's Perfumes
- Unisex Perfumes
- Featured Shops

### Product page

- Images
- Product Name
- Brand
- Rating
- Price
- Available Sizes
- Seller / Shop
- Description
- Fragrance Notes
- Specifications
- Reviews
- Related Perfumes

### Deliverable

Users can browse the complete marketplace without logging in.

---

## 9. Phase 5 — Search & Filtering

This should be treated as a separate phase because marketplace search is critical.

### Search

Example query: `sauvage`

Results:

- Dior Sauvage EDT
- Dior Sauvage EDP
- Dior Sauvage Parfum
- Dior Sauvage Elixir

### Filters

- Brand
- Category
- Gender
- Price
- Size
- Concentration
- Fragrance Family
- Fragrance Notes
- Seller
- Rating
- Availability

### Sorting

- Relevance
- Price Low → High
- Price High → Low
- Newest
- Rating
- Popularity

### Pagination

Use server-side pagination:

```text
/products?page=1&limit=20
```

### Deliverable

Fast and usable marketplace search.

---

## 10. Phase 6 — Cart & Wishlist

### Cart

Users can:

- Add product
- Remove product
- Change quantity
- Select variant
- View subtotal

Example:

```text
Cart

Dior Sauvage 100ml
Seller: ABC Perfumes
Qty: 1
Rs. 30,000

Chanel Bleu 100ml
Seller: XYZ Perfumes
Qty: 2
Rs. 70,000
```

### Wishlist

- Add to wishlist
- Remove from wishlist
- Move to cart

> [!IMPORTANT]
> The cart should support products from multiple sellers.

---

## 11. Phase 7 — Checkout & Orders

This is another major phase.

### Checkout

```text
Cart
  ↓
Address
  ↓
Shipping Method
  ↓
Payment Method
  ↓
Order Review
  ↓
Place Order
```

### Order structure

I recommend:

```text
Order
   │
   ├── Seller Order A
   │       ├── Item
   │       ├── Item
   │       └── Shipping
   │
   └── Seller Order B
           ├── Item
           └── Shipping
```

This allows one customer checkout while still maintaining seller-specific fulfillment.

### Order status

- `PENDING`
- `CONFIRMED`
- `PROCESSING`
- `SHIPPED`
- `DELIVERED`
- `CANCELLED`
- `RETURN_REQUESTED`
- `RETURNED`
- `REFUNDED`

### Deliverable

A customer can successfully place and track orders.

---

## 12. Phase 8 — Seller Dashboard

Create the seller dashboard at `/seller`.

### Dashboard

- Sales
- Orders
- Products
- Inventory
- Customers
- Reviews
- Revenue

### Product management

- My Products
- Add Product
- Edit Product
- Inventory
- Pricing

### Orders

Seller sees only their own orders.

| Order # | Customer | Products | Amount | Status | Shipping |
| ------- | -------- | -------- | ------ | ------ | -------- |

### Reports

- Today's Sales
- Weekly Sales
- Monthly Sales
- Top Products
- Revenue
- Pending Orders

### Deliverable

A seller can operate their shop independently.

---

## 13. Phase 9 — Admin Dashboard

Create the admin dashboard at `/admin`.

### Dashboard

- Total Users
- Total Sellers
- Total Products
- Total Orders
- Revenue
- Pending Seller Approvals
- Pending Product Approvals

### Management

- Users
- Sellers
- Shops
- Brands
- Categories
- Products
- Orders
- Reviews
- Coupons
- Payments
- Reports
- Settings
- Audit Logs

### Seller approval

```text
Seller Registration
       ↓
Pending
       ↓
Admin Review
       ↓
Approved / Rejected
```

### Product approval

Potentially:

```text
Seller creates product
       ↓
Pending approval
       ↓
Admin reviews
       ↓
Published
```

---

## 14. Phase 10 — Payments & Commission

If Perfume Hub is a true marketplace, introduce commissions.

### Example

| Item                | Amount     |
| ------------------- | ---------- |
| Product             | Rs. 30,000 |
| Platform commission | 10%        |
| Perfume Hub         | Rs. 3,000  |
| Seller              | Rs. 27,000 |

### Entities

- `Payment`
- `Commission`
- `SellerWallet`
- `SellerTransaction`
- `Settlement`
- `Refund`

### Seller financial flow

```text
Customer Payment
       ↓
Perfume Hub
       ↓
Commission deducted
       ↓
Seller Balance
       ↓
Settlement
```

> [!NOTE]
> This phase can be implemented after the basic order system is stable.

---

## 15. Phase 11 — Reviews, Coupons & Promotions

### Reviews

Only customers who purchased the product should be able to leave a verified review.

```text
★★★★★

Dior Sauvage

Verified Purchase
```

### Coupons

Examples:

- `WELCOME10`
- `PERFUME20`

Rules:

- Percentage discount
- Fixed discount
- Minimum order
- Maximum discount
- Expiration date
- Usage limit
- Specific products
- Specific sellers

### Promotions

- Featured Product
- Flash Sale
- Discount
- Buy X Get Y
- Free Shipping

---

## 16. Phase 12 — Production Hardening

Before deployment:

### Security

- Input validation
- Zod validation
- Authorization checks
- Password security
- Secure cookies
- Rate limiting
- File upload validation
- SQL injection protection
- XSS protection
- CSRF considerations
- API authorization
- Seller data isolation

### Performance

- Database indexes
- Query optimization
- Pagination
- Next.js caching
- Image optimization
- Lazy loading
- Server Components
- Avoid unnecessary client components

### Reliability

- Error handling
- Logging
- Audit logs
- Transaction handling
- Database backups
- Monitoring

---

## 17. Recommended Prisma Architecture

Your Prisma schema will eventually contain entities approximately like:

| Group            | Entities                                                                                         |
| ---------------- | ------------------------------------------------------------------------------------------------ |
| Users            | `User`, `Role`, `Permission`, `Address`                                                          |
| Sellers          | `Seller`, `Shop`                                                                                 |
| Catalog          | `Brand`, `Category`, `Product`, `ProductVariant`, `ProductImage`, `ProductNote`, `ProductListing`, `Inventory` |
| Shopping         | `Cart`, `CartItem`, `Wishlist`, `WishlistItem`                                                   |
| Orders           | `Order`, `OrderItem`, `SellerOrder`, `ShippingAddress`, `ShippingMethod`                         |
| Payments         | `Payment`, `Refund`                                                                              |
| Reviews          | `Review`                                                                                         |
| Marketing        | `Coupon`, `Promotion`                                                                            |
| Finance          | `Commission`, `SellerWallet`, `SellerTransaction`, `Settlement`                                  |
| System           | `Notification`, `AuditLog`                                                                       |

> [!TIP]
> Don't create all of these on day one. Build the schema incrementally with Prisma migrations.

---

## 18. Next.js Application Structure

I recommend a feature-oriented structure rather than putting everything into one huge components folder.

```text
src/
│
├── app/
│   │
│   ├── (store)/
│   │   ├── page.tsx
│   │   ├── products/
│   │   ├── brands/
│   │   ├── categories/
│   │   ├── shops/
│   │   ├── cart/
│   │   └── checkout/
│   │
│   ├── (auth)/
│   │   ├── login/
│   │   ├── register/
│   │   ├── forgot-password/
│   │   └── reset-password/
│   │
│   ├── account/
│   │   ├── profile/
│   │   ├── orders/
│   │   ├── wishlist/
│   │   └── addresses/
│   │
│   ├── seller/
│   │   ├── dashboard/
│   │   ├── products/
│   │   ├── inventory/
│   │   ├── orders/
│   │   ├── reviews/
│   │   └── reports/
│   │
│   ├── admin/
│   │   ├── dashboard/
│   │   ├── users/
│   │   ├── sellers/
│   │   ├── brands/
│   │   ├── categories/
│   │   ├── products/
│   │   ├── orders/
│   │   ├── payments/
│   │   └── reports/
│   │
│   └── api/
│
├── components/
│   ├── ui/
│   ├── forms/
│   ├── layout/
│   └── shared/
│
├── features/
│   ├── auth/
│   ├── products/
│   ├── cart/
│   ├── checkout/
│   ├── orders/
│   ├── sellers/
│   └── reviews/
│
├── actions/
│
├── services/
│
├── lib/
│   ├── prisma.ts
│   ├── auth.ts
│   └── permissions.ts
│
├── validations/
│
├── types/
│
└── utils/
```

---

## 19. How Data Should Flow

For example, adding a product:

```text
Seller UI
   ↓
Product Form
   ↓
Zod Validation
   ↓
Server Action
   ↓
Authorization Check
   ↓
Product Service
   ↓
Prisma
   ↓
SQL Server
```

**Not:**

```text
Browser
   ↓
SQL Server
```

> [!WARNING]
> The browser should never directly access SQL Server.

---

## 20. Prisma Layer

I recommend keeping Prisma access centralized.

```text
Next.js
   ↓
Server Action / Route Handler
   ↓
Service
   ↓
Prisma
   ↓
SQL Server
```

For example:

```text
actions/
    product.actions.ts

services/
    product.service.ts

lib/
    prisma.ts
```

This keeps your business logic out of React components.

---

## 21. Database Strategy

Since you're using SQL Server, I recommend a primarily relational schema.

**Don't** store your entire product as:

```text
Product
----------------
id
data JSON
```

**Instead:**

```text
Product
----------------
id
name
slug
brandId
categoryId
description
status
createdAt
updatedAt
```

and use relations:

```text
Product
   │
   ├── Brand
   ├── Category
   ├── ProductVariant
   ├── ProductImage
   ├── ProductNote
   └── ProductListing
```

JSON can still be used for genuinely flexible metadata.

---

## 22. Development Order

I would **not** build Admin first.

The recommended order is:

```text
                    FOUNDATION
                        │
                        ▼
                 Authentication
                        │
                        ▼
              Database Architecture
                        │
                        ▼
             Brand / Category / Shop
                        │
                        ▼
                   Products
                        │
                        ▼
              Public Storefront
                        │
                        ▼
                Search & Filters
                        │
                        ▼
                 Cart / Wishlist
                        │
                        ▼
                   Checkout
                        │
                        ▼
                    Orders
                        │
             ┌──────────┴──────────┐
             ▼                     ▼
        Seller Portal         Admin Portal
             │                     │
             └──────────┬──────────┘
                        ▼
              Payments / Commission
                        │
                        ▼
             Reviews / Promotions
                        │
                        ▼
              Production Hardening
```

This gives you a usable marketplace relatively early instead of spending a long time building administration screens before the shopping flow exists.

---

## 23. MVP Scope

For the first production-ready MVP, I would limit the scope to:

### Buyer

- Registration/login
- Browse products
- Search
- Filtering
- Product details
- Seller information
- Cart
- Checkout
- Address management
- COD initially
- Order placement
- Order history
- Wishlist

### Seller

- Registration
- Seller approval
- Shop profile
- Product listings
- Inventory
- Pricing
- Orders
- Basic sales dashboard

### Admin

- Dashboard
- User management
- Seller management
- Brand management
- Category management
- Product approval
- Order management
- Basic reports

### Subsequent releases

Then add:

- Payments
- Reviews
- Coupons
- Promotions
- Commission
- Seller settlements
- Advanced analytics

---

## 24. Suggested Milestones

| Milestone | Name                   | Scope                                                     |
| --------- | ---------------------- | --------------------------------------------------------- |
| **M1**    | Project Foundation     | Next.js + TypeScript + Tailwind + Prisma + SQL Server |
| **M2**    | Authentication         | Users + roles + permissions                               |
| **M3**    | Marketplace Foundation | Sellers + shops + brands + categories                     |
| **M4**    | Product Catalog        | Products + variants + images + inventory                  |
| **M5**    | Storefront             | Home + product pages + brands + categories                |
| **M6**    | Search                 | Search + filters + sorting + pagination                   |
| **M7**    | Shopping               | Cart + wishlist                                           |
| **M8**    | Checkout               | Addresses + shipping + checkout                           |
| **M9**    | Orders                 | Order lifecycle + seller orders                           |
| **M10**   | Seller Portal          | Seller dashboard + products + inventory + orders          |
| **M11**   | Admin Portal           | Administration + approvals + reports                      |
| **M12**   | Marketplace Finance    | Payments + commissions + settlements                      |
| **M13**   | Engagement             | Reviews + coupons + promotions                            |
| **M14**   | Production             | Security + performance + testing + deployment             |
