# 🌆 Milaan-AI

> 🧠 *AI-assisted civic issue reporting & resolution platform for citizens, inspectors and administrators.*

---

## 📋 Table of contents

* [✨ Overview](#-overview)
* [💡 Key Features](#-key-features)
* [📘 Milaan Project Flow (detailed)](#-milaan-project-flow-detailed)

  * [1. User Roles & Access Flow](#1-user-roles--access-flow)
  * [2. Citizen Flow 👤](#2-citizen-flow-👤)
  * [3. Inspector Flow 🔍](#3-inspector-flow-🔍)
  * [4. Admin Hierarchy Flow 👔](#4-admin-hierarchy-flow-👔)
  * [5. Technical Data Flow 🔄](#5-technical-data-flow-🔄)
  * [6. Report Lifecycle 📊](#6-report-lifecycle-📊)
  * [7. Key Features in Action 🎯](#7-key-features-in-action-🎯)
  * [🔐 Login Flow & Test Credentials](#🔐-login-flow--test-credentials)
* [🧱 Architecture & Tech Stack](#-architecture--tech-stack)
* [🚀 Getting Started](#-getting-started)
* [⚙️ Dev Setup](#️-dev-setup)
* [🌍 Deployment](#-deployment)
* [🤝 Contributing](#-contributing)
* [📄 License & Acknowledgements](#-license--acknowledgements)

---

## ✨ Overview

Milaan-AI is a smart, multilingual civic issue reporting platform that helps citizens report local problems (potholes, waste, streetlights, water, etc.), allows inspectors and field officers to act, and gives admins analytics to measure impact and SLAs. The platform aims for transparency, faster resolution and community engagement (gamification). 🌱

---

## 💡 Key Features

* 📝 Issue reporting with photos, voice → text, GPS
* 🔎 Duplicate detection via perceptual hashing (pHash)
* 🗺️ Maps + heatmaps (Google Maps + PostGIS)
* ⚡ Real-time notifications via Socket.io
* 📊 Analytics dashboards (cluster / district / state)
* 🎮 Gamification (points, badges)
* 🌐 Multilingual + PWA/offline support for low-connectivity areas

---

## 📘 Milaan Project Flow (detailed)

### 1. User Roles & Access Flow

```
LANDING PAGE (/) 
 • Hero section with platform overview
 • Impact metrics & success stories
 • CTA: "Login" or "Report Issue"
      ↓
LOGIN PAGE (/login)
 • Email/Password authentication
 • JWT token generation
      ↓
Role Detection (from JWT)
      ↓
CITIZEN  | INSPECTOR | CLERK | ADMIN
```

### 2. Citizen Flow 👤

**CITIZEN DASHBOARD** — `/citizen`

**HOME TAB**

* View all submitted reports
* Search reports
* Quick "New Report" button

**REPORT TAB (Dialog)**

* Select category (pothole, waste, streetlight, etc.)
* Title & description
* Upload photos (optional)
* Record voice description (optional)
* Auto capture GPS
* Submit → **Report created**

**Backend processing (on submit):**

* Save report to DB
* Generate image perceptual hash (pHash)
* Check duplicates (similarity threshold)
* Auto-assign to cluster/district (PostGIS `ST_Contains`)
* Notify inspectors via Socket.io
* Return Report ID

**HISTORY TAB**

* View all your reports & statuses: `reported → verified → assigned → in-progress → resolved → closed`
* Track progress

**PROFILE TAB**

* Stats: Reports, Resolved, Points
* Gamification info
* Logout

---

### 3. Inspector Flow 🔍

**INSPECTOR DASHBOARD** — `/inspector`

**LIVE FEED**

* Real-time reports in assigned cluster
* Socket.io instant notifications
* Filter by category/status/priority
* Actions: Verify report, Mark duplicate, Assign priority (1–5), Add notes

**MAP VIEW**

* Google Maps with clustered markers
* Heatmap visualization
* Filter & click marker → open report details

---

### 4. Admin Hierarchy Flow 👔

**Auto-detection of admin level based on officer assignment / JWT role**

* **State Admin** — `/state-admin`
  Oversees entire state (e.g., Karnataka). State-level analytics & heatmap; drill down to districts.

* **District Admin** — `/district-admin`
  Manages district (e.g., Bangalore Urban). District analytics, cluster drill-downs.

* **Cluster Admin** — `/cluster-admin`
  Manages local cluster (e.g., Koramangala). Assign reports, track SLAs, run cluster-level analytics.

---

### 5. Technical Data Flow 🔄

```
FRONTEND (React + Vite)
 • Landing, Login, Dashboards
 • Stores JWT in localStorage
 • Sends API requests with Authorization: Bearer <token>

  ↓ API requests

EXPRESS BACKEND
 • Middleware:
   - Authentication (JWT verify)
   - Role-based access control (RBAC)
   - Request validation (Zod schemas)
 • API routes:
   - /api/auth/* (login/register/me)
   - /api/reports/* (CRUD)
   - /api/analytics/* (stats/heatmaps)
   - /api/officers/* (assignments)
 • Socket.io: real-time notifications
   - Rooms: role-{role}, officer-{userId}, cluster-{clusterId}
   - Events: new_report, status_change, comment

  ↓ DB calls

POSTGRESQL + PostGIS
 • Tables:
   - users (citizens, inspectors, admins)
   - reports (status, location, images)
   - officers (cluster assignments)
   - states/districts/clusters (PostGIS polygons)
   - report_fingerprints (dedup)
   - audit_logs (change tracking)
 • Spatial queries: ST_Contains for auto-assign
```

---

### 6. Report Lifecycle 📊

1. **Citizen submits report** (category, GPS, photos, voice→text)
2. **Backend processing**: pHash, duplicate detection, spatial routing
3. **Inspector notified** (Socket.io + push)
4. **Inspector verifies** → status `verified`, set priority & notes
5. **Clerk/Admin assigns** → status `assigned`, set SLA & officer
6. **Field work** → `in-progress` updates
7. **Resolution** → `resolved`, upload after photos, notify citizen
8. **Verification & closure** → citizen/inspector verifies → `closed`; award points; update analytics

---

### 7. Key Features in Action 🎯

**Voice Reporting**

* Mic → Web Speech API → speech-to-text → auto-fill description

**Auto GPS Tagging**

* Browser geolocation → lat/lng → PostGIS `ST_Contains` → cluster auto-assign

**Duplicate Detection**

* Image pHash → compare with existing fingerprints → flag if similarity > 85%

**Analytics & Heatmaps**

* Admin queries geo-level data → map heatmap + charts
* Real-time updates from Socket.io

**Real-time Notifications**

* New report → emit events to cluster inspectors, assigned admins
* UI updates instantly

---

## 🔐 Login Flow & Test Credentials

**Login page:** `http://your-app-url/login`

**Test accounts**

| Role           |                Email | Password      | Dashboard URL     |
| -------------- | -------------------: | ------------- | ----------------- |
| Citizen        |   `citizen@test.com` | `password123` | `/citizen`        |
| Inspector      | `inspector@test.com` | `password123` | `/inspector`      |
| Cluster Admin  | `cluster1@admin.com` | `password123` | `/cluster-admin`  |
| District Admin | `district@admin.com` | `password123` | `/district-admin` |
| State Admin    |    `state@admin.com` | `password123` | `/state-admin`    |

**Quick test flows**

* Citizen: login → Home / Report / History / Profile
* Inspector: login → Live feed / Map view → Verify & update reports
* Admins: login → analytics, heatmaps, cluster/district drills

---

## 🧱 Architecture & Tech Stack

* **Frontend:** React + TypeScript + Vite + shadcn/ui + TailwindCSS
* **Backend:** Node.js + Express + Drizzle ORM (or preferred ORM)
* **DB:** PostgreSQL + PostGIS (spatial)
* **Realtime:** Socket.io
* **Maps:** Google Maps API (requires `VITE_GOOGLE_MAPS_API_KEY`)
* **Auth:** JWT (stored in localStorage)
* **Validation:** Zod (request schemas)
* **Other:** pHash image hashing for deduplication, Web Speech API for voice

---

## 🚀 Getting Started

1. **Clone**

```bash
git clone https://github.com/Dhanushkumar1610/milaan-ai-.git
cd milaan-ai-
```

2. **Install**

```bash
npm install
```

3. **Environment**

* Copy sample env: `cp .env.development .env.local` and set:

  * `DATABASE_URL`
  * `VITE_GOOGLE_MAPS_API_KEY`
  * `JWT_SECRET`
  * any other keys used by the repo

4. **Run**

```bash
npm run dev
```

Visit `http://localhost:3000` (or configured port)

---

## ⚙️ Dev Setup

* Use `drizzle.config.ts` to configure DB connections and migrations.
* Keep secrets out of source (use CI/CD encrypted vars).
* Recommended scripts (add to `package.json` if not present):

  * `dev`, `build`, `start`, `lint`, `format`, `migrate`, `seed`.
* Use feature branches & PR reviews.

---

## 🌍 Deployment

* Build frontend: `npm run build`
* Deploy frontend to Vercel/Netlify.
* Deploy backend to a managed service (Render, Heroku, AWS ECS) with `DATABASE_URL` & `JWT_SECRET`.
* Ensure `VITE_GOOGLE_MAPS_API_KEY` and other runtime vars are configured.
* Monitor via logs and add alerting for SLA breaches.

---

## 🤝 Contributing

We ❤️ contributions!

1. Fork → create branch (`feature/xyz`)
2. Add tests / update docs
3. Open a PR with clear description & screenshots
4. Follow the code style (Prettier / ESLint) and component guidelines in `design_guidelines.md`

---

## 📄 License & Acknowledgements

**MIT © 2025 Dhanush Kumar**

Thanks to contributors and the civic-tech community for ideas & inspiration. 💪🌍

---

If you’d like, I can:

* ✅ Generate a ready-to-paste `README.md` file (I already formatted this — want it as a single file text block?).
* 🎨 Add GitHub shields (build, license, contributors) and badges.
* 🧩 Extract and create a minimal `CONTRIBUTING.md`, `API_REFERENCE.md` or `ARCHITECTURE.md`.

Which of the above should I produce next?
