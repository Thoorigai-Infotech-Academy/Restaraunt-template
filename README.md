# Aroma Restaurant 🍽️

A production-grade restaurant website with a **frontend admin CMS**, built with React 19, React Router, and Supabase Auth.

**Live Demo:** [your-vercel-url.vercel.app](https://your-vercel-url.vercel.app) *(coming soon)*  
**Admin Demo:** [/admin/login](https://your-vercel-url.vercel.app/admin/login)

---

## ✨ Features

### Customer Site
- **Editorial luxury design** — warm cream palette, serif typography, generous whitespace
- **Hero section** — animated background, call-to-action
- **About section** — editorial layout with spice-bowl decorations
- **Signature dishes** — auto-playing carousel with arrows, dots, and keyboard navigation
- **Gallery** — auto-scrolling image strip with drag-to-scroll and full-screen lightbox
- **Contact** — Google Maps embed + reservation form
- **Footer** — 4-column layout with quick links, contact info, hours, socials
- **Fully responsive** — mobile-first CSS with fluid typography (`clamp()`)
- **Accessible** — semantic HTML, ARIA labels, keyboard navigation, `prefers-reduced-motion`

### Admin CMS (`/admin`)
- **Supabase-authenticated login** — real backend auth, not client-side theatre
- **Protected routes** — `/admin/*` requires a valid JWT
- **8 editors** — Profile, Hero, About, Signature, Gallery, Contact, Footer + Dashboard
- **Live editing** — changes reflect on the customer site instantly
- **Save / Discard** with `localStorage` persistence
- **Unsaved-changes indicator** — "● Unsaved changes" badge
- **Toast notifications** — "Saved ✓" confirmation on every save
- **Last-saved timestamp** — "Saved just now" / "Saved 5 min ago"
- **Beforeunload warning** — browser warns if you try to leave with unsaved edits
- **Export / Import JSON** — download the entire site's content, restore from file
- **Reset all** — restore everything to defaults in one click

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | React 19 |
| Routing | React Router 7 |
| Auth | Supabase Auth (JWT, session persistence) |
| State | React Context + custom hooks |
| Persistence | `localStorage` (temporary, until backend migration) |
| Build Tool | Vite 8 |
| Styling | Plain CSS with CSS variables and `clamp()` |
| Linting | ESLint 10 |

---

## 📁 Project Structure

```
src/
├── admin/                     # Admin dashboard
│   ├── AdminLayout.jsx        # Sidebar + content frame
│   ├── AdminLogin.jsx         # Login page
│   ├── ProtectedRoute.jsx     # Auth guard for /admin/*
│   ├── AdminDashboard.jsx     # Overview + bulk tools
│   ├── AdminProfile.jsx       # Edit restaurant name, tagline, logo
│   ├── AdminHero.jsx          # Edit hero section
│   ├── AdminAbout.jsx         # Edit about section
│   ├── AdminSignature.jsx     # Edit signature dishes
│   ├── AdminGallery.jsx       # Edit gallery images
│   ├── AdminContact.jsx       # Edit contact info + map
│   └── AdminFooter.jsx        # Edit footer content
│
├── components/                # Reusable UI
│   ├── AdminField.jsx         # Single-line input + label
│   ├── AdminTextArea.jsx      # Multi-line input + label
│   ├── AdminSaveBar.jsx       # Save/Discard bar with dirty tracking
│   ├── Toast.jsx              # Toast notification
│   └── footer.jsx             # Customer-facing footer
│
├── context/                   # Shared state
│   ├── AuthContext.jsx        # Supabase auth provider
│   └── RestaurantContext.jsx  # Restaurant content state
│
├── data/
│   └── restaurantData.js      # Default content (source of truth)
│
├── hooks/                     # Custom hooks
│   ├── usePersistentState.js  # useState + localStorage + save/reset
│   └── useToast.js            # Toast state manager
│
├── lib/
│   └── supabase.js            # Supabase client singleton
│
├── sections/                  # Customer-facing sections
│   ├── Navbar.jsx
│   ├── Hero.jsx
│   ├── About.jsx
│   ├── menu.jsx               # Signature dishes carousel
│   ├── gallery.jsx
│   ├── contact.jsx
│   └── ...
│
├── styles/                    # Per-section CSS
│   ├── admin.css
│   ├── about.css
│   ├── menu.css
│   ├── gallery.css
│   ├── contact.css
│   ├── footer.css
│   └── ...
│
├── App.jsx                    # Routes + CustomerSite layout
├── App.css
├── main.jsx                   # Provider nesting
└── index.css
```

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/[your-username]/aroma-restaurant.git
cd aroma-restaurant
```

### 2. Install dependencies

```bash
npm install
```

### 3. Set up Supabase

You'll need a free Supabase project to run the admin login.

1. Go to **[supabase.com](https://supabase.com)** and create a new project
2. In **Project Settings → API**, copy:
   - **Project URL**
   - **Publishable key** (or `anon public` key)
3. In **Authentication → Providers → Email**, enable the Email provider and turn **off** "Confirm email"
4. In **Authentication → Users**, create an admin user (enable "Auto Confirm User")

### 4. Configure environment variables

Copy the example env file:

```bash
cp .env.example .env.local
```

Edit `.env.local` and paste your Supabase values:

```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=your-publishable-key-here
```

> ⚠️ **Never commit `.env.local`.** It's already in `.gitignore`.

### 5. Run the dev server

```bash
npm run dev
```

Open [http://localhost:5173](http://localhost:5173).

---

## 🔐 Admin Access

1. Visit `/admin/login`
2. Sign in with the email + password you created in Supabase
3. You'll be redirected to `/admin` (dashboard)

The admin uses **real Supabase Auth** — passwords are hashed server-side, sessions use JWTs, and routes are protected by a `ProtectedRoute` guard.

---

## 🎨 Editing Content

### From the Admin UI
1. Log in at `/admin/login`
2. Pick a section from the sidebar
3. Edit fields — changes preview live
4. Click **Save** to persist to `localStorage`
5. Click **Discard** to revert to the last saved state

### Resetting
- **Per section**: click "Discard" in that editor
- **All at once**: go to Dashboard → "Reset all"

### Backup / Restore
- **Export**: Dashboard → "Export JSON" → downloads a `.json` file
- **Import**: Dashboard → "Import JSON" → upload a saved file

---

## 🏗️ Architecture Notes

### State Management
All restaurant content lives in `RestaurantContext`, backed by `usePersistentState` (a `useState` wrapper with `localStorage` sync). Both the customer site and admin editors read/write to the same context — no prop drilling, no duplicated state.

### Auth
`AuthContext` wraps Supabase's session API. `ProtectedRoute` redirects to `/admin/login` if there's no valid session, and preserves the intended destination so login returns the user there.

### Current Limitation
Content is stored per-browser in `localStorage`. Edits do **not** sync across devices yet. Migrating to Supabase Postgres with Row Level Security is the next planned step.

---

## 🗺️ Roadmap

- [x] Customer site with 6 sections
- [x] Admin CMS with 8 editors
- [x] Supabase Auth + protected routes
- [x] Save / Discard / Export / Import
- [ ] Migrate content to Supabase Postgres (RLS-secured)
- [ ] Real-time sync across devices (Supabase Realtime)
- [ ] Image uploads via Supabase Storage
- [ ] Multi-user roles (owner / editor / viewer)
- [ ] Automated tests (Vitest + Playwright)
- [ ] Deploy to Vercel

### Implementation project

The repository includes a structured internship plan for turning this prototype into the first release of a reusable business website platform:

- [Phase 0 and Phase 1 Implementation Plan](docs/PHASE_0_1_IMPLEMENTATION.md)
- [GitHub Project Setup and Tracking Guide](docs/GITHUB_PROJECT_TRACKING.md)
- [Contribution Guide](CONTRIBUTING.md)

---

## 📜 Scripts

| Command | Purpose |
|---------|---------|
| `npm run dev` | Start the Vite dev server |
| `npm run build` | Production build |
| `npm run preview` | Preview the production build locally |
| `npm run lint` | Run ESLint |

---

## 🤝 Contributing

This is a personal learning project, but issues and pull requests are welcome.

---

## 📄 License

MIT — see [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgements

- Food photography from [Unsplash](https://unsplash.com)
- Fonts: Playfair Display, Cormorant Garamond, Inter, Dancing Script (Google Fonts)
- Auth infrastructure by [Supabase](https://supabase.com)
- Built with [Vite](https://vitejs.dev) + [React](https://react.dev)

---

**Built with care by  DINISHA**  
[Portfolio](https://your-site.com) · [LinkedIn](https://linkedin.com/in/your-profile)
