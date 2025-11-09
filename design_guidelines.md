# Milaan Civic Platform Design Guidelines

## Design Approach
**Enhanced Material Design** - Building on Material's solid foundation with modern visual treatments: glassmorphism, vibrant gradients, 3D depth, and sophisticated animations. This creates a premium civic platform that feels trustworthy yet cutting-edge.

## Visual Design System

### Glassmorphism & Depth
- **Glass Cards**: backdrop-blur-xl bg-white/10 (dark) or bg-white/70 (light), border border-white/20, shadow-2xl
- **Floating Elements**: transform translate-z for 3D depth, shadow-xl with subtle rotation
- **Layering**: Use z-index hierarchy (z-10, z-20, z-30) with drop-shadow-2xl for elevation
- **Blur Overlays**: backdrop-blur-md for modals, backdrop-blur-sm for image overlays

### Gradient System
**Primary Gradients** (bg-gradient-to-br):
- Hero: from-blue-600 via-purple-600 to-pink-600
- CTA: from-green-500 to-emerald-600
- Stats: from-orange-400 to-red-500
- Success: from-teal-400 to-cyan-500

**Mesh Gradients**: Multi-stop gradients with radial-gradient for backgrounds (3-4 color stops)
**Animated Gradients**: background-size-200 bg-gradient-to-r with animate-gradient for premium feel

### Dark/Light Mode
**Light Mode Base**: bg-gray-50, cards bg-white/90
**Dark Mode Base**: bg-slate-900, cards bg-slate-800/50
**Transitions**: transition-colors duration-300 on all color-changing elements
**Mode Toggle**: Floating button with sun/moon icon, smooth transform rotation

## Typography System
**Fonts**: Roboto (primary), Roboto Condensed (data-dense)

- Display: text-5xl md:text-7xl font-bold tracking-tight with gradient text (bg-clip-text text-transparent)
- Titles: text-3xl md:text-5xl font-semibold
- Headings: text-2xl md:text-3xl font-medium
- Body Large: text-lg font-normal
- Body: text-base
- Captions: text-sm text-gray-600 dark:text-gray-400

## Layout & Spacing
**Scale**: 1, 2, 4, 6, 8, 12, 16, 24, 32 (Tailwind units)
**Containers**: max-w-7xl mx-auto px-4 md:px-8
**Section Padding**: py-16 md:py-24 lg:py-32
**Card Spacing**: p-6 md:p-8
**Grids**: grid-cols-1 md:grid-cols-2 lg:grid-cols-3 with gap-6 md:gap-8

## Component Library

### Navigation
**Top Bar**: Glass effect fixed top, backdrop-blur-lg, shadow-lg with smooth scroll-triggered background opacity change
**Bottom Nav (Mobile)**: Floating glass bar with rounded-t-3xl, icon active states with scale-110 transform
**Sidebar (Desktop)**: Glass panel w-64 with hierarchical menu, hover states lift items (translate-x-1)

### Buttons & CTAs
**Primary**: Gradient background, rounded-xl px-8 py-4, font-semibold, shadow-lg, hover:shadow-2xl hover:scale-105 transform transition-all
**Secondary**: Glass style with border-2, hover:bg-white/20
**FAB**: Fixed bottom-right, rounded-full w-16 h-16, gradient shadow-2xl, pulse animation on idle
**Icon Buttons**: Rounded-full p-3, hover:bg-white/10, transition-all duration-200

### Cards
**Report Cards**: 
- Glass container with rounded-2xl
- Image with gradient overlay (bottom fade)
- Floating status badge (top-right, glass effect)
- Hover: lift (translate-y-2), shadow enhancement, scale-102
- Category chip with gradient background
- Interactive 3D tilt effect on hover (subtle)

**Stat Cards**:
- Gradient border (border-2 border-gradient-to-r)
- Large number with counting animation on view
- Icon with subtle float animation (animate-bounce-slow)
- Trend arrow with color-coded gradient

### Forms & Inputs
- Glass-style outlined fields with backdrop-blur
- Floating labels with smooth transition
- Focus state: gradient border, glow effect (shadow-lg shadow-blue-500/50)
- Error states: shake animation, red gradient glow
- Success states: green glow with checkmark icon animation
- File upload: Drag zone with dashed gradient border, preview grid with zoom hover

### Data Visualization
**Charts**: 
- Animated entry (stagger children, fade-in-up)
- Gradient fills for area/bar charts
- Interactive tooltips with glass effect
- Smooth transitions on data updates (300ms ease)
- Hover highlights with glow effects

**Maps**:
- Custom gradient marker pins per category
- Cluster markers with pulsing ring animation
- Glass info windows with blur backdrop
- Animated marker drop on load
- Route polylines with gradient strokes

### Modals & Overlays
- Full blur backdrop (backdrop-blur-lg bg-black/40)
- Glass dialog with rounded-3xl
- Slide-up animation (mobile), scale-in (desktop)
- Dismiss gesture animation (swipe down mobile)

### Status & Feedback
**Badges**: Gradient backgrounds, rounded-full, px-4 py-1.5, text-xs font-semibold, shadow-md
**Notifications**: Toast with glass effect, slide-in-right animation, auto-dismiss with progress bar
**Progress**: Gradient bar with shimmer animation, circular with gradient stroke
**Loading**: Skeleton screens with shimmer gradient animation (animate-pulse-slow)

## Page-Specific Layouts

### Public Landing
**Hero** (80vh):
- Large civic hero image (diverse community volunteers in action) with dark gradient overlay (from-black/60 to-transparent)
- Floating glass cards with live stats (animated counters)
- Headline with gradient text effect
- CTA buttons on glass backdrop-blur panel
- Floating 3D element illustrations (optional accent)

**Trust Section**: Logo carousel with grayscale-to-color hover
**How It Works**: 3 cards with number badges, icon with gradient background, stagger-fade animation on scroll
**Impact Metrics**: 4-column grid, gradient cards, counting animation
**Showcases**: Before/after slider cards with glass overlay labels
**Download CTA**: Phone mockup with gradient screen, floating app badges
**Footer**: Multi-column glass panel

### Citizen App
- Gradient top bar with glass effect
- Large gradient FAB for "Report Issue"
- Report grid with staggered card animations on load
- Pull-to-refresh with gradient spinner
- Glass bottom nav with icon scale animations

### Inspector Dashboard
- Glass sidebar with gradient accent
- Top metrics: 4 gradient stat cards with animated numbers
- Real-time feed: Cards slide-in-left on new items
- Map/List toggle with smooth transition
- Notification bell with badge pulse animation

### Admin Portal
- Full dashboard with gradient section dividers
- KPI cards with 3D hover tilt
- Interactive charts with gradient fills
- Geographic heatmap with color intensity animation
- Data table with glass row hover, gradient action buttons

## Animations & Micro-interactions
**Page Transitions**: Fade + slide (400ms ease-out)
**Card Entrance**: Stagger by 100ms, fade-in-up
**Hover States**: Scale-105, translate-y-2, shadow enhancement (200ms)
**Active Press**: Scale-95 (100ms)
**Success Actions**: Confetti burst, checkmark draw animation
**Data Updates**: Smooth number transitions with easing
**Scroll Animations**: Parallax hero, fade-in-up sections on intersection
**Loading**: Gradient shimmer across skeletons
**Map**: Smooth zoom easing, marker bounce on add

## Icons
**Material Icons CDN**: 24px navigation, 20px cards, 16px buttons
**Animated Icons**: Spin on loading, bounce on success, shake on error

## Images
**Hero Image**: High-quality photo showing diverse community members actively reporting/fixing civic issues - warm, authentic, action-oriented. Full-width with gradient overlay.
**Report Cards**: User-submitted issue photos
**Showcase Section**: Professional before/after comparison images
**Empty States**: Gradient illustration backgrounds with friendly icons
**Map Markers**: Custom gradient pins per category type

## Accessibility
- ARIA labels with live region announcements
- Keyboard navigation with gradient focus rings (ring-2 ring-offset-2)
- Reduced motion mode (prefers-reduced-motion): Disable animations
- High contrast mode support with border reinforcement
- Touch targets 44x44px minimum
- Color contrast WCAG AAA with gradient text alternatives

## Responsive Strategy
- Mobile (base): Single column, bottom nav, large touch targets, full-width cards
- Tablet (md:768px): 2-column grids, hybrid nav
- Desktop (lg:1024px): 3-column grids, sidebar nav, enhanced animations
- Wide (xl:1280px): max-w-7xl constraint, richer visualizations