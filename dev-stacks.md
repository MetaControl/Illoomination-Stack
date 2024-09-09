# Project Development Guidebook

## Overview
This project consists of a dual VPS setup for a highly performant, headless eCommerce store. We utilize **Medusa** as the backend eCommerce engine and **WordPress with Bricks Builder** for the frontend. The project includes a dynamic product assembly system, tiered caching, and membership gamification. Key features include custom discount structures, geolocation-based pricing, and a sophisticated image processing workflow.

---

## Tech Stack Breakdown

### **Frontend Stack (VPS 1 - WordPress & Bricks Builder)**:
- **WordPress**: Acts as the Content Management System (CMS) for pages, blogs, and front-end content.
- **Bricks Builder**: Visual design builder for creating and managing dynamic pages.
- **GraphQL**: Facilitates efficient data communication between WordPress (frontend) and Medusa (backend).
- **Nginx**: Web server for load balancing, caching, and handling requests for both frontend and backend servers.
- **ImageMagick**: Image processing tool used to dynamically generate web-optimized product images.
- **Redis**: Caching layer used to store transient data and API responses for faster product and page loads.
  
### **Backend Stack (VPS 2 - Medusa)**:
- **Medusa**: A modular eCommerce backend that manages products, variations, and orders.
- **Node.js**: Runtime environment for Medusa's API handling and backend services.
- **PostgreSQL**: The primary database for storing relational data (e.g., products, variants, orders). **MongoDB** can be an alternative for more dynamic storage needs.
- **Redis**: Serves as the primary cache for product variations, API requests, and data retrieval across both servers.
- **Nginx**: Manages API routing and reverse proxy operations.

---

## Key Features

### 1. **Dynamic Product Assembly**
- Medusa handles dynamic product variants such as motifs, colors, and sizes.
- The frontend queries product data from Medusa via **GraphQL**, dynamically displaying combinations without bloating the database.
- Product images are generated and compressed using **ImageMagick**, with options for motif size and placement.

### 2. **Geolocation-Based Pricing**
- Pricing adjusts based on user location (detected via IP), showing either EUR or USD, with real-time currency conversion and localized pricing.
- Implemented using **GeoIP** services on the frontend, with pricing data pulled from Medusa.

### 3. **Custom Discount Structure**
- Discounts are dynamic and stackable:
  - **Timed Discounts**: Limited-time reductions on specific products.
  - **Membership Discounts**: Discounts that scale with membership tier.
  - **Seasonal or Campaign-Specific Discounts**: Automatically applied via campaign rules set in Medusa.
- Discounts are configured in Medusa and applied dynamically based on customer interaction and tier.

### 4. **Caching System in Tiers**
**Caching Strategy**:
- **Tier S (Transients)**: Frequently accessed products (e.g., during campaigns) are cached in Redis for immediate access.
- **Tier 1**: Products with high user interaction are cached with a grace period.
- **Tier 2**: Products with lower traffic have image data purged but core data retained.
- **Tier 3**: Least-accessed products retain only default images and essential data, dynamically reassembled on request.

**Temporary Transients**: Special-case products (e.g., during marketing pushes) can be manually cached at a higher tier temporarily.

### 5. **Membership and Gamification**
- **Membership Levels**:
  - **Tier I (Free)**: Access to exclusive offers, Cotton Credits, and 1 monthly giveaway ticket.
  - **Tier II (Paid, €28/year)**: Free Tee, discounts, more Cotton Credits, and 2 giveaway tickets/month.
  - **Tier III (Subscription, €28/month)**: Monthly free T-shirt (13/year), deepest discounts, maximum credits, 3 giveaway tickets/month.
- **Monthly Giveaway**: Users receive tickets based on their membership tier and can win prizes each month.
- **Cotton Credits (or Loominous Points)**: Earned from purchases, usable for discounts, free items, or additional giveaway tickets.
- **Gamification**: Achievements and loyalty tiers provide additional perks and discounts based on user engagement.

---

## Server Architecture & Setup

### **VPS 1 (Frontend: WordPress & Bricks Builder)**:
- **vCPU**: 8 Cores
- **RAM**: 32GB
- **Storage**: 400GB NVMe
- **Bandwidth**: 32TB
- **Redis**: Used for transient caching and product image caching.
- **Nginx**: For web traffic handling and load balancing.

### **VPS 2 (Backend: Medusa)**:
- **vCPU**: 8 Cores
- **RAM**: 32GB
- **Storage**: 400GB NVMe
- **Bandwidth**: 32TB
- **Redis**: Used for caching API requests and product variations.
- **PostgreSQL**: Database for storing product, user, and order data.
- **Nginx**: Manages API requests and acts as a reverse proxy.

---

## Performance Goals

- **API Response Time**: Target 10–50ms via optimized Redis caching.
- **Time to First Byte (TTFB)**: 50–100ms.
- **Full Page Load (with Caching)**: <1 second.
- **Product Variation Load**: Cached product variations should load within 0.2–0.4 seconds.

---

## DevOps, Monitoring & Maintenance

1. **CI/CD Pipelines**: 
   - Continuous integration and deployment using tools like Jenkins or GitLab CI.
   - Automated testing and staging before live deployment.
   
2. **Monitoring & Alerts**:
   - **Prometheus** or **Grafana** for server and API monitoring.
   - **Custom Alerts**: Alerts for high load, slow queries, and downtime.

3. **Security**:
   - Implement SSL certificates via **Let's Encrypt**.
   - Secure Nginx configurations with strict headers.
   - **Malware Scanner** (part of Hostinger) for periodic checks.

---

## Cost Breakdown

### VPS Hosting:
- **VPS 1 (WordPress & Bricks Builder)**: €17.99/month (Hostinger VPS)
- **VPS 2 (Medusa)**: €17.99/month (Hostinger VPS)
- **Total**: €35.98/month

### Additional Costs:
- **Bricks Builder Pro License**: €79/year
- **Custom Development** (if outsourcing for API integration or advanced features): Varies based on hourly rates.
  
---

## Conclusion
This project integrates **Medusa** and **WordPress** to create a high-performance, scalable eCommerce solution. It combines dynamic product assembly, optimized caching, advanced pricing mechanisms, and an engaging membership system to provide a seamless shopping experience. The dual VPS setup ensures no single server is overloaded, leading to fast response times and a smooth user experience.
