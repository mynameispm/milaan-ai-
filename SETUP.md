# Milaan Platform - Setup Instructions

## Overview
Milaan is a comprehensive civic issue reporting and resolution platform with real-time processing, ML-based classification, PostGIS spatial queries, and multi-channel submission capabilities.

## Architecture
- **Backend**: Node.js/Express with TypeScript
- **Frontend**: React + Vite + TailwindCSS + Shadcn UI
- **Database**: PostgreSQL with PostGIS extension
- **Real-time**: Socket.io for live notifications
- **Storage**: Supabase Storage (optional) or local fallback
- **Auth**: JWT with access/refresh tokens

## Required Environment Variables (Replit Secrets)

### Core Application
```
JWT_SECRET=your-secret-key-change-in-production
NODE_ENV=development
PORT=5000
```

### Database (PostgreSQL with PostGIS)
The PostgreSQL database is already configured via Replit. The following environment variables are automatically set:
```
DATABASE_URL=postgresql://...
PGHOST=...
PGPORT=...
PGUSER=...
PGPASSWORD=...
PGDATABASE=...
```

### Supabase (Optional - for image storage)
If you want to use Supabase for image storage (recommended for production):
```
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

**Setup Supabase:**
1. Create a Supabase project at https://supabase.com
2. Go to Settings → API to get your URL and keys
3. Create a storage bucket named `milaan-images` (public bucket)
4. Add the credentials to Replit Secrets

**Note:** If Supabase is not configured, images will be stored as base64 (development only).

### Firebase (Optional - for push notifications)
```
FIREBASE_CREDENTIALS_JSON={"type":"service_account",...}
FIREBASE_SERVER_KEY=your-server-key
```

### Twilio (Optional - for SMS/IVR)
```
TWILIO_ACCOUNT_SID=ACxxxx
TWILIO_AUTH_TOKEN=your-auth-token
TWILIO_PHONE_NUMBER=+1234567890
TWILIO_WEBHOOK_SECRET=your-webhook-secret
```

## Database Setup

### 1. Enable PostGIS Extension
Run this SQL in your PostgreSQL database:
```sql
CREATE EXTENSION IF NOT EXISTS postgis;
CREATE EXTENSION IF NOT EXISTS postgis_topology;
```

### 2. Push Database Schema
The database schema is defined in `shared/schema.ts` using Drizzle ORM.

Run the following command to sync the schema:
```bash
npm run db:push
```

### 3. Seed Sample Data (Optional)
You can manually run the SQL in `db/supabase-schema.sql` to add:
- Sample states, districts, and clusters
- Admin user (email: admin@milaan.gov, password: admin123)

## Running the Application

### Development Mode
```bash
npm run dev
```

This starts both the Express backend and Vite frontend on port 5000.

### Production Mode
```bash
npm run build
npm start
```

## Key Features Implemented

### ✅ Sprint 1 (MVP) - COMPLETED
- [x] PostgreSQL database with PostGIS extension
- [x] Complete database schema (users, reports, officers, clusters, etc.)
- [x] JWT authentication with access/refresh tokens
- [x] Role-based access control (citizen, inspector, clerk, admin)
- [x] Report submission API with GPS auto-tagging
- [x] Image upload with compression
- [x] PostGIS spatial queries for auto-routing to clusters/officers
- [x] Deduplication service (image hash + text similarity)
- [x] Real-time Socket.io notifications
- [x] Upvote/verification system
- [x] Gamification (points, trust scores)
- [x] Audit logs
- [x] Anonymous reporting support

### ✅ Frontend Features
- [x] Voice-to-text input using Web Speech API
- [x] Auto GPS capture on form load
- [x] Offline-first utilities with IndexedDB
- [x] Material Design UI system
- [x] Landing page, citizen app, inspector dashboard, admin analytics
- [x] Real-time feed components
- [x] Status badges and category chips
- [x] Before/after cards
- [x] Interactive map view
- [x] Responsive mobile-first design

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login and get JWT tokens
- `POST /api/auth/refresh` - Refresh access token

### Reports
- `POST /api/reports` - Create new report (with auto geo-tagging, dedup, assignment)
- `GET /api/reports` - List reports (with filters)
- `POST /api/reports/:id/upvote` - Upvote/verify report
- `POST /api/reports/:id/assign` - Assign report to officer (admin only)
- `POST /api/reports/:id/update` - Add field update with photos/comments

### Analytics
- `GET /api/analytics/overview` - Get overall statistics

## Socket.io Events

### Client → Server
- `join_location` - Join location-specific room (cluster/district)
- `report_update` - Broadcast report status change

### Server → Client
- `new_report` - New report created (sent to assigned officer)
- `report_assigned` - Report assigned to officer
- `report_status_changed` - Report status updated

## Testing the System

### 1. Create a User
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "citizen@test.com",
    "password": "test123",
    "name": "Test Citizen",
    "role": "citizen"
  }'
```

### 2. Create a Report
```bash
curl -X POST http://localhost:5000/api/reports \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -d '{
    "title": "Large pothole on Main Street",
    "description": "Deep pothole causing traffic issues",
    "category": "pothole",
    "latitude": 19.0760,
    "longitude": 72.8777,
    "locationText": "Main St, Andheri"
  }'
```

### 3. Test Voice Input
Open the citizen app at `/citizen` and:
1. Click "New Report"
2. Switch to "Voice Input" tab
3. Click "Start Voice Input"
4. Speak your description
5. The transcript will appear (editable)

### 4. Test Offline Mode
1. Open DevTools → Application → Service Workers
2. Check "Offline" mode
3. Submit a report
4. Report is saved to IndexedDB
5. Go back online
6. Reports sync automatically

## Next Steps (Sprint 2 & 3)

### Sprint 2 - Integrations (Pending)
- [ ] Twilio IVR webhook for phone call reporting
- [ ] SMS submission support
- [ ] Web Speech multilingual translation
- [ ] Service worker for full offline PWA
- [ ] Background sync for pending reports

### Sprint 3 - Analytics & Hardening (Pending)
- [ ] Admin dashboard with heatmaps
- [ ] SLA tracking and enforcement
- [ ] Performance dashboards (officer rankings)
- [ ] AI-powered hotspot prediction
- [ ] CI/CD pipeline (GitHub Actions)
- [ ] Comprehensive test coverage (80%+)
- [ ] Sentry error tracking

### Sprint 4 - ML & Advanced Features (Future)
- [ ] TensorFlow Lite for on-device image classification
- [ ] Flutter mobile app
- [ ] Blockchain audit trail
- [ ] Advanced analytics dashboard

## Troubleshooting

### Database Connection Issues
If you see "connection refused" errors:
1. Check that PostgreSQL is running in Replit
2. Verify DATABASE_URL environment variable
3. Run `npm run db:push` to sync schema

### Image Upload Fails
If images aren't uploading:
1. Check Supabase credentials (SUPABASE_URL, SUPABASE_ANON_KEY)
2. Verify the `milaan-images` bucket exists in Supabase
3. Ensure bucket permissions allow public uploads
4. Fallback: Images will save as base64 if Supabase is unavailable

### Socket.io Not Connected
If real-time updates don't work:
1. Check browser console for Socket.io connection errors
2. Verify JWT token is being sent in auth handshake
3. Check server logs for Socket.io initialization

### Voice Input Not Working
Web Speech API requirements:
- **Chrome/Edge**: Full support
- **Safari**: Requires HTTPS (works on localhost)
- **Firefox**: Limited support
- Must allow microphone permissions

## Production Deployment Checklist

- [ ] Change JWT_SECRET to a strong random value
- [ ] Set up Supabase production project
- [ ] Configure Twilio for SMS/IVR
- [ ] Set up Firebase for push notifications
- [ ] Enable HTTPS
- [ ] Configure CORS for production domain
- [ ] Set up Sentry for error tracking
- [ ] Enable rate limiting
- [ ] Configure database backups
- [ ] Set up monitoring and alerting

## Support & Documentation

- API Documentation: Coming soon (Postman collection)
- Architecture Diagram: See `docs/architecture.md`
- Database Schema: See `db/supabase-schema.sql`
- Frontend Components: See `client/src/components/examples/`

## License

This is a civic infrastructure project. Licensing TBD.
