# Milaan - Civic Issue Reporting Platform

## Overview

Milaan is a civic issue reporting and resolution platform designed for modern communities. It allows citizens to report infrastructure problems (potholes, waste management, streetlights, etc.) via multiple channels (web, mobile, SMS, voice) and tracks their resolution in real-time. The platform includes AI-based classification, PostGIS spatial auto-routing, crowd validation, and hierarchical admin dashboards with auto-detection for state, district, and cluster-level management.

## Recent Changes

### Landing Page Enhancement (November 2024)
- **Language:** Converted to English-only (removed all Hindi/bilingual content)
- **Design Enhancements:**
  - Enhanced report submission form with gradient top border, improved spacing, and icon-labeled inputs
  - Upgraded feature cards with larger icons (20x20), hover gradient borders, and better descriptions
  - Replaced emoji icons with proper Lucide React icon components
  - Improved typography with larger headings and gradient text effects
  - Enhanced CTAs with better messaging and visual hierarchy
  - Added contextual badges and improved overall spacing throughout
- **Motion Graphics:** 7 custom animation components with full accessibility support (prefers-reduced-motion)
  - FloatingShapes, AnimatedCounter, ParticleField, AnimatedIllustration, WaveAnimation, GridPattern, MorphingBlob

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend

**Technology Stack:**
- React with TypeScript (Vite)
- `wouter` for routing
- Shadcn UI (on Radix UI)
- TailwindCSS
- TanStack Query for server state
- Socket.io client for real-time features
- Framer Motion for animations with accessibility considerations (`useReducedMotion` hook)
- Optional 3D rendering with React Three Fiber + Drei, with performance checks and fallbacks.

**Design System:**
- Material Design principles with Roboto font, custom HSL color system.
- Mobile-first responsive design; bottom navigation for citizens, side navigation for admins/inspectors.
- Glassmorphism, gradients, and advanced animations (e.g., `animate-gradient`, `animate-float`).
- PWA support with offline-first capabilities using IndexedDB.

**Key Features:**
- Citizen App: Report submission, history, profile.
- Inspector Dashboard: Real-time feed, map view.
- Admin Dashboards (State, District, Cluster): Analytics, heatmaps, auto-detection based on geographic assignment.
- Consistent navigation with `PageHeader` component and enhanced login experience.

### Backend

**Technology Stack:**
- Node.js with Express and TypeScript
- Drizzle ORM with Neon serverless PostgreSQL driver
- JWT authentication (access/refresh tokens, bcryptjs)
- Socket.io server for real-time notifications
- Multer for file uploads

**API Design:**
- RESTful APIs with role-based access control (RBAC).
- Authentication middleware for protected routes.

**Key Features:**
- Auto geo-tagging of reports.
- Image perceptual hashing (pHash) and text similarity for duplicate detection.
- Hierarchical admin routing.
- Real-time notifications via Socket.io rooms (role, officer, cluster, district-based).
- Voice-to-text support (Web Speech API).
- Multi-channel submission (web, mobile, planned: SMS/IVR).

**Database Schema:**
- Users (citizen, inspector, clerk, admin roles).
- Reports with a defined status workflow.
- Geographic hierarchy: States → Districts → Clusters (with PostGIS polygons).
- Officers assigned to clusters.
- Report fingerprints for deduplication.
- Audit logs and upvotes.

### Data Storage

- **Primary Database:** PostgreSQL with PostGIS extension (Replit provisioned).
- **Spatial Features:** PostGIS for polygon containment, distance calculations, and geographic coordinate storage.
- **Image Storage:** Optional Supabase Storage (production recommended), with Base64 fallback for development.
- **Offline Storage:** IndexedDB for queued reports with auto-sync.

### Real-time Architecture

- **Socket.io:** Authenticated, room-based broadcasting (`role-{role}`, `officer-{userId}`, `cluster-{clusterId}`, `district-{districtId}`).
- **Use Cases:** Instant new report notifications for inspectors, real-time dashboard updates for admins, status change notifications for citizens.

### ML/AI Features (Planned)

- **On-device Classification:** TensorFlow Lite/TensorFlow.js models for categorizing issues (e.g., road damage, waste).
- **Training Pipeline:** Colab-based model training for .tflite export.

## External Dependencies

### Required Services

- **Database:** PostgreSQL with PostGIS (Replit provisioned).
- **Authentication:** JWT_SECRET for token signing.

### Optional Services

- **Image Storage:** Supabase Storage.
- **Push Notifications:** Firebase Cloud Messaging (FCM).
- **SMS/Voice:** Twilio (for IVR and SMS).
- **Maps:** Google Maps JavaScript API (for heatmaps).
- **Monitoring:** Sentry (for error tracking).

### Third-party NPM Packages (Key Examples)

- **Core:** `express`, `drizzle-orm`, `@neondatabase/serverless`, `socket.io`, `@supabase/supabase-js`.
- **Authentication & Security:** `jsonwebtoken`, `bcryptjs`, `multer`.
- **Frontend:** `react`, `react-dom`, `@tanstack/react-query`, `wouter`, `@radix-ui/*`, `tailwindcss`, `recharts`, `@googlemaps/js-api-loader`, `framer-motion`, `@react-three/fiber`, `@react-three/drei`.
- **Image Processing:** `image-hash`, `sharp`.
- **Development:** `vite`, `typescript`, `tsx`, `esbuild`.