# Illoomintion-Stack - Project Development Guidebook

## Overview
This guide outlines the development of a dual VPS solution, integrating **Medusa** for backend eCommerce management and **WordPress + Bricks Builder** for the frontend. Key features include a dynamic product assembly system, optimized caching, geolocation-based pricing, custom discount structures, membership, and gamification systems.

---

## Tech Stack & Infrastructure

### **VPS 1 (WordPress + Bricks Builder Frontend)**:
- **OS**: Ubuntu 20.04 LTS
- **Web Server**: Nginx
- **Database**: MySQL 8.0
- **Caching**: Redis + FastCGI
- **Image Processing**: ImageMagick
- **Plugins**:
  - Bricks Builder
  - Meta Box (Custom Fields)
  - WP GraphQL (API communication)
  - WP Super Cache (Backup caching)

### **VPS 2 (Medusa Backend)**:
- **OS**: Ubuntu 20.04 LTS
- **Database**: PostgreSQL (or MongoDB for dynamic data)
- **Caching**: Redis
- **Additional Components**:
  - Node.js (Medusa backend)
  - Medusa API
  - Docker (for containerized microservices)

### **CDN**: Integrated to globally serve assets.

---

## Key Features

### 1. **Dynamic Product Assembly**
- **Medusa Backend** handles dynamic product creation without database bloat. Products are assembled dynamically with motifs, sizes, and color options, while keeping SKU management automated via product attributes.
- **WordPress Frontend** serves the variations using API calls from Medusa.

### 2. **Caching Tiers**
- **Tier S (Transients)**: High-frequency products are cached in Redis, with ultra-low latency.
- **Tier 1**: Frequently used images/data with a grace period.
- **Tier 2**: Less-accessed products have images purged but core data cached.
- **Tier 3**: Only default images and basic data are retained for the least-used products.
- **Temporary Transients**: Special cases (e.g., promotions) temporarily added to the top tier.

### 3. **Image Processing**
- ImageMagick automatically processes and compresses images on the fly during product creation. Multiple motif sizes and image layering are supported to generate web-optimized images.

### 4. **Geolocation-Based Pricing**
- Prices dynamically adjust based on the user’s location, automatically displaying EUR or USD pricing with predefined conversion rates.

### 5. **Custom Discount Structure**
- Discounts are managed dynamically using custom rules:
  - **Tiered Discounts** based on user actions and engagement.
  - **Relative Discounts** where the percentages scale based on overall traffic.
  - **Exclusive Offer Codes** for marketing campaigns.

### 6. **Membership & Gamification**
- **Dual Website Experience**: 
  - **Main Store**: Conversion-optimized with simple user engagement.
  - **Loominatee**: Secret society style with unique product displays and hidden offers.
  
- **Membership Levels**:
  - Tier I: Free, access to basic offers, 1 monthly giveaway ticket.
  - Tier II: Paid, 1 free Tee, store discounts, access to Discord, 2 giveaway tickets/month.
  - Tier III: Subscription, monthly Tees, deeper discounts, 3 giveaway tickets/month.

- **Monthly Giveaway**:
  - Users receive tickets based on their membership tier for a monthly prize draw.
  
- **Gamification**:
  - Achievements for actions (purchases, referrals, sharing on social media) are tied to rewards.
  - **Cotton Credits** or **Loominous Points** earned for purchases can be redeemed for discounts, products, or exclusive offers.

---

## Scalability & Optimization
- **VPS Isolation** ensures no resource competition between front-end and back-end services.
- **Redis & API Caching** optimizes load times for dynamically assembled products.
- **Dynamic Image Assembly** minimizes storage usage and allows quick access to newly added variants.
- **Grace Periods** in caching tiers ensure high-traffic products remain instantly accessible, while older or less-used products are dynamically assembled when needed.

---

## Server Architecture

### **VPS 1 (Frontend)**:
- **8 vCPU Cores**
- **32GB RAM**
- **400GB NVMe Storage**
- **Dedicated IP & Bandwidth (32TB)**

### **VPS 2 (Backend)**:
- **8 vCPU Cores**
- **32GB RAM**
- **400GB NVMe Storage**
- **Dedicated IP & Bandwidth (32TB)**

---

## Cost Considerations
- **VPS Costs**: €17.99/month each, totaling **€35.98/month**.
- **Plugin Reduction**: Eliminating WooCommerce and excessive plugins reduces long-term costs.

---

## Conclusion
This project integrates **Medusa and WordPress** across two VPS servers for a **scalable, high-performance eCommerce solution**. Dynamic product assembly, tiered caching, custom image processing, and a sophisticated membership/gamification system provide flexibility and speed, with significantly reduced bloat compared to WooCommerce.

