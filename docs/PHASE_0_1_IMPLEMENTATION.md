# Generic Business Website Platform

## Phase 0 and Phase 1 Implementation Plan

This document defines the internship project for evolving the existing Aroma restaurant website into the first production-ready implementation of a reusable business website platform.

The work must be completed incrementally on top of the current repository. The existing customer-facing design should be preserved unless a task explicitly requires a visual change.

## 1. Project objective

Build a safe, configurable, production-ready restaurant website that demonstrates a reusable core for future business templates.

At the end of Phase 1, a developer must be able to launch a new restaurant website by changing configuration, content, images, and environment variables without modifying shared React components.

## 2. Product boundary

### Included

- One production-ready restaurant template
- Central client configuration
- Reusable page sections
- Theme tokens
- Complete restaurant menu
- Real enquiry/reservation delivery
- SEO and local-business metadata
- Responsive and accessible customer experience
- Repeatable build, test, and deployment process
- Documentation for creating a new restaurant client

### Excluded

- Client-facing CMS
- Drag-and-drop page builder
- Multi-tenant architecture
- Subscription billing
- Native ordering or payment processing
- Multiple business industries
- Unlimited design customization
- Complex roles and permissions

These belong to later phases. Phase 0 and Phase 1 should establish interfaces that allow those capabilities to be added without implementing them prematurely.

## 3. Existing implementation

The repository currently contains:

- React 19 and Vite
- React Router
- Supabase authentication for the admin routes
- Customer sections for hero, about, menu, gallery, contact, and footer
- Admin editors backed by browser `localStorage`
- CSS files organized by section
- Static defaults in `src/data/restaurantData.js`

Known problems that must be addressed include committed credentials, simulated form submission, browser-only content persistence, incomplete admin import/export, unsafe empty data handling, case-sensitive imports, lint failures, and unused admin fields.

## 4. Target architecture

Phase 1 should move toward the following structure without requiring a complete rewrite:

```text
src/
├── app/                         # Application composition and routes
├── core/
│   ├── config/                  # Configuration loading and validation
│   ├── forms/                   # Form validation and submission adapters
│   ├── seo/                     # Metadata and structured data
│   └── theme/                   # Theme tokens and provider
├── blocks/
│   └── shared/                  # Reusable business-neutral sections
├── industries/
│   └── restaurant/
│       ├── components/          # Menu and restaurant-specific UI
│       ├── data/                # Default restaurant content
│       └── schema/              # Restaurant configuration contracts
├── client/
│   └── siteConfig.js            # Active client configuration
└── assets/
```

The exact migration can be gradual. Do not move files solely to match this diagram; move them when a task creates a clear boundary.

## 5. Engineering principles

1. Preserve working behavior while refactoring.
2. Keep client-specific values outside reusable components.
3. Prefer small, reviewable pull requests.
4. Add tests when moving business logic.
5. Never commit secrets or real client data.
6. Validate configuration before rendering it.
7. Provide safe loading, empty, success, and failure states.
8. Keep accessibility and mobile behavior part of each feature rather than a final patch.
9. Avoid abstractions that are not needed by the restaurant implementation.
10. A completed ticket must satisfy its acceptance criteria and pass all quality checks.

## 6. Phase 0: stabilization

### Goal

Make the current repository safe to collaborate on and establish a reliable quality baseline.

### Estimated duration

One working week for one intern with mentor support.

### P0-01: Secure repository credentials

**Purpose:** Remove exposed credentials and prevent recurrence.

**Work:**

- Remove `adminuser.txt` from the repository.
- Rotate the associated Supabase password before treating the repository as safe.
- Remove the file from published Git history if it has been pushed.
- Confirm `.env`, `.env.local`, and environment-specific files remain ignored.
- Add a sanitized `.env.example` containing only variable names and placeholders.
- Add a short security note to the README.

**Acceptance criteria:**

- No credentials or tokens are present in tracked files or current Git history.
- The application starts using values copied from `.env.example` into `.env.local`.
- A repository search does not reveal the retired password.

### P0-02: Restore a clean quality baseline

**Purpose:** Make automated checks trustworthy.

**Work:**

- Fix all existing ESLint errors and hook warnings.
- Resolve the reported vulnerable development dependency without breaking the build.
- Add a `check` script that runs lint, tests, and the production build.
- Document the supported Node.js version.

**Acceptance criteria:**

- `npm run lint` succeeds with no warnings.
- `npm run build` succeeds.
- `npm audit` has no unresolved high-severity findings, or an approved exception is documented.
- `npm run check` succeeds from a clean installation.

### P0-03: Fix portability and routing

**Purpose:** Ensure local success matches Linux hosting behavior.

**Work:**

- Correct import casing, including the `Footer.jsx` import.
- Standardize component filenames and imports.
- Add hosting rewrite configuration for direct navigation to React Router routes.
- Verify `/`, `/admin/login`, and `/admin` behavior from direct URLs.

**Acceptance criteria:**

- The project builds in a case-sensitive environment.
- Refreshing a nested route does not return a hosting 404.
- Public and protected routes retain their intended behavior.

### P0-04: Add defensive rendering

**Purpose:** Prevent editable or imported data from crashing the public site.

**Work:**

- Add empty states for signature dishes and gallery images.
- Prevent modulo-by-zero and undefined active-item access.
- Validate imported JSON before applying it.
- Handle missing nested fields using normalized defaults.
- Add a top-level error boundary with a customer-safe fallback.

**Acceptance criteria:**

- Zero dishes and zero gallery images do not crash the application.
- Invalid import data produces a useful error and does not replace valid content.
- A component error renders the fallback instead of a blank page.

### P0-05: Make admin controls internally consistent

**Purpose:** Ensure existing controls do what their labels promise while they remain in the repository.

**Work:**

- Include the restaurant profile in reset, export, and import operations.
- Make contact-form text read from `RestaurantContext` rather than static defaults.
- Either connect unused profile fields to the public site or remove them from the editor.
- Remove or document any other fields that have no public effect.
- Do not expand the `localStorage` admin into a CMS during Phase 0.

**Acceptance criteria:**

- Every visible editor field has an observable public-site effect.
- Export followed by import restores every editable section.
- Reset all resets every editable section.

### P0-06: Add baseline automated tests

**Purpose:** Protect the application during the Phase 1 refactor.

**Work:**

- Configure a unit/component test framework.
- Add tests for configuration normalization and persistent-state behavior.
- Add customer-page smoke tests.
- Test empty dish and gallery states.
- Add one protected-route authentication test.

**Acceptance criteria:**

- Tests run through `npm test` in non-interactive mode.
- The identified crash cases are covered.
- Tests do not depend on live Supabase credentials.

### Phase 0 completion gate

Phase 0 is complete only when:

- Secrets have been removed and rotated.
- Installation, lint, tests, and build succeed.
- Critical customer flows do not crash with empty or malformed data.
- Existing admin operations behave consistently.
- The repository can be safely assigned to an intern through GitHub issues.

## 7. Phase 1: generic-core restaurant MVP

### Goal

Deliver the first client-ready restaurant website while introducing the smallest reusable platform core needed for future industries.

### Estimated duration

Three to four working weeks after Phase 0.

### P1-01: Define and validate client configuration

**Purpose:** Create one source of truth for client-specific settings.

**Required configuration groups:**

- Identity: business name, logo, favicon, tagline
- Theme: colors, fonts, spacing preset, radius preset
- Contact: phone, email, WhatsApp, address, coordinates
- Business: locale, currency, timezone, price range, cuisine types
- Hours: regular weekly schedule and optional exceptions
- Social links
- SEO defaults
- Feature flags
- Integration settings without secrets

**Work:**

- Create a documented configuration contract.
- Create the Aroma configuration using the contract.
- Validate required values at application startup or build time.
- Normalize optional values into predictable defaults.
- Replace hard-coded business details throughout the public site.

**Acceptance criteria:**

- Changing the active client configuration updates all relevant public sections.
- Invalid required configuration produces a clear developer-facing error.
- No Aroma-specific business text remains inside reusable components.

### P1-02: Introduce theme tokens

**Purpose:** Rebrand a client without editing component styles.

**Work:**

- Define semantic CSS variables for colors, typography, spacing, radius, shadows, and content width.
- Map the active configuration to those variables.
- Replace client-specific color and font values in existing CSS.
- Keep one supported theme preset for the MVP.
- Document safe customization ranges.

**Acceptance criteria:**

- Logo, core colors, and fonts can be changed centrally.
- Text remains readable and controls remain visible using the documented ranges.
- Mobile and desktop layouts continue to match the existing quality level.

### P1-03: Create the section-rendering contract

**Purpose:** Allow sections to be enabled and ordered without hard-coded page composition.

**Work:**

- Define a section record with `id`, `type`, `enabled`, `variant`, `content`, and `settings`.
- Create a registry that maps supported types to React components.
- Migrate the home page to the registry.
- Add an unknown-section development warning and production-safe fallback.
- Support section enable/disable and ordering through configuration.

**Acceptance criteria:**

- Reordering configuration changes page order.
- Disabling a section removes it and its navigation entry where applicable.
- Unknown section types do not crash the website.
- Only one variant per section is required in the MVP.

### P1-04: Separate shared and restaurant-specific blocks

**Purpose:** Establish the boundary required for future business templates.

**Shared blocks:**

- Hero
- About
- Gallery
- Testimonials, if included
- FAQ, if included
- Contact
- Map/location
- Footer

**Restaurant-specific blocks:**

- Menu
- Featured dishes
- Opening hours presentation
- Reservation form
- Delivery and ordering links

**Acceptance criteria:**

- Shared blocks do not import restaurant-specific data.
- Restaurant-specific components live behind the industry package boundary.
- Existing public content remains visually and functionally available.

### P1-05: Implement the complete menu model

**Purpose:** Replace the signature-only carousel with a usable restaurant menu.

**Data requirements:**

- Categories
- Item name and description
- Price and optional price variants
- Image
- Featured status
- Vegetarian and vegan indicators
- Dietary/allergen labels
- Availability
- Display order

**Work:**

- Define and validate the menu data model.
- Build category navigation or filtering.
- Format prices using configured currency and locale.
- Retain featured-dish presentation as an optional restaurant block.
- Add useful empty states.

**Acceptance criteria:**

- Customers can browse all available menu categories on mobile and desktop.
- Currency is not hard-coded.
- Unavailable items are clearly represented or hidden according to configuration.
- The menu remains usable with one category, one item, missing images, and long descriptions.

### P1-06: Implement real enquiries and reservations

**Purpose:** Turn the website into a working lead-generation tool.

**MVP form fields:**

- Name
- Email or phone
- Requested date
- Requested time
- Party size
- Message or special request
- Consent acknowledgement when required

**Work:**

- Define a form-submission adapter interface.
- Implement one production adapter selected by the project owner.
- Validate fields in the browser and at the receiving endpoint.
- Add spam prevention and basic rate limiting where supported.
- Add honest sending, success, and failure states.
- Preserve form values after a failed submission.
- Make WhatsApp, phone, or external-booking actions configurable alternatives.

**Acceptance criteria:**

- A test submission reaches the configured destination.
- Failed delivery never displays a success message.
- Repeated submissions cannot accidentally be triggered while sending.
- No service secret is included in the browser bundle.

### P1-07: Complete local-business SEO

**Purpose:** Make each client deployment discoverable and shareable.

**Work:**

- Add configurable title and description.
- Add canonical URL and social-sharing metadata.
- Add configurable social preview image.
- Generate Restaurant JSON-LD from client configuration.
- Add `robots.txt` and a sitemap.
- Provide a favicon and web-app metadata.
- Add a custom 404 page.

**Acceptance criteria:**

- Metadata contains no placeholder values.
- Structured data passes a schema validation check.
- Shared links display the configured title, description, and image.
- Production URLs are derived from configuration rather than hard-coded.

### P1-08: Accessibility and responsive completion

**Purpose:** Establish the minimum quality level for all future templates.

**Work:**

- Make mobile navigation expose its expanded state and accessible label.
- Verify keyboard navigation for menu controls and the gallery lightbox.
- Trap and restore focus in dialogs.
- Associate form errors with fields.
- Verify heading order and landmarks.
- Check contrast and visible focus indicators.
- Respect reduced-motion preferences.
- Test at representative mobile, tablet, and desktop widths.

**Acceptance criteria:**

- All interactive controls are usable with a keyboard.
- Automated accessibility checks have no serious or critical violations.
- Core tasks work at 320px width without horizontal page scrolling.
- Dialog focus returns to the control that opened it.

### P1-09: Performance and asset optimization

**Purpose:** Make the template practical on mobile connections.

**Work:**

- Replace oversized source images with appropriately sized WebP or AVIF assets.
- Provide responsive image sources where valuable.
- Add explicit dimensions to reduce layout shift.
- Lazy-load below-the-fold images.
- Optimize font loading.
- Remove unused starter assets and components.
- Document recommended upload dimensions and size limits.

**Acceptance criteria:**

- The multi-megabyte hero source is replaced with an optimized production asset.
- The initial page does not download the full gallery immediately.
- A production performance audit records the agreed baseline metrics.
- Broken or missing images have a deliberate fallback.

### P1-10: Deployment and client creation workflow

**Purpose:** Make the template repeatable rather than dependent on its original author.

**Work:**

- Document clean installation and environment setup.
- Document how to create a new restaurant client configuration.
- Document how to replace branding, images, menu, contact details, and integrations.
- Add deployment instructions for the selected host.
- Add post-deployment checks.
- Add a client handover checklist.

**Acceptance criteria:**

- A second developer can create a sample restaurant deployment by following the documentation.
- No source component edits are needed for standard branding and content.
- Direct URLs work in production.
- A real form submission succeeds after deployment.

### P1-11: End-to-end MVP verification

**Purpose:** Verify the release as a customer and as the delivery team.

**Required scenarios:**

- Load the home page on mobile and desktop.
- Navigate to every enabled section.
- Browse menu categories and prices.
- Open and close gallery content using pointer and keyboard.
- Submit a valid reservation.
- Handle an invalid and a failed reservation.
- Use call, directions, and social links.
- Open a direct route after deployment.
- Verify metadata and structured data.
- Build a second sample configuration without component changes.

**Acceptance criteria:**

- Automated checks pass.
- Manual release checklist is signed off.
- Known limitations are documented.
- The repository is tagged as the first MVP release.

### Phase 1 completion gate

Phase 1 is complete only when:

- A pilot restaurant can use the deployed website.
- The website receives real enquiries or reservations.
- Standard branding and content changes require no shared-component edits.
- Menu, contact, SEO, accessibility, and responsive requirements pass.
- A second sample restaurant configuration proves repeatability.
- Setup, deployment, and handover documentation are complete.

## 8. Recommended implementation sequence

```text
P0-01 Security
    ↓
P0-02 Quality baseline ── P0-03 Portability
    ↓                         ↓
P0-04 Defensive rendering ─ P0-05 Admin consistency
    ↓
P0-06 Baseline tests
    ↓
P1-01 Client configuration
    ↓
P1-02 Theme tokens ──────── P1-03 Section renderer
    ↓                         ↓
P1-04 Package boundaries
    ↓
P1-05 Menu ──────────────── P1-06 Reservations
    ↓                         ↓
P1-07 SEO ─ P1-08 Accessibility ─ P1-09 Performance
    ↓
P1-10 Delivery workflow
    ↓
P1-11 MVP verification
```

## 9. Working agreement for the internship

- One issue should normally produce one pull request.
- Branch names should follow `codex/<issue-number>-short-description` or the team's selected equivalent.
- Pull requests should be small enough to review in one sitting.
- Each pull request must describe the change, screenshots where visual behavior changed, testing performed, and remaining limitations.
- The intern should not merge their own pull request unless explicitly authorized.
- Architecture changes require a short written proposal in the issue before implementation.
- New dependencies require a reason and mentor approval.
- Secrets and client data must never be placed in issues, commits, screenshots, or test fixtures.

## 10. Definition of done

A task is done only when:

- Its acceptance criteria are satisfied.
- Relevant tests are added or updated.
- Lint, tests, and build pass.
- Mobile and keyboard behavior were checked when applicable.
- Documentation is updated.
- No secrets or personal data were introduced.
- The pull request has been reviewed and merged.

## 11. Inputs required from the project owner

Before Phase 1 begins, provide:

- Approved MVP feature list
- Final Aroma logo and favicon
- Brand colors and font preferences
- Restaurant description and contact details
- Address, coordinates, and opening hours
- Complete menu data with prices and dietary information
- Approved photographs with usage rights
- Reservation delivery choice and destination
- WhatsApp, ordering, and social links
- Production domain or temporary deployment domain
- Analytics preference
- Privacy-policy and consent requirements
- Selected hosting provider
- Browser and device support expectations

Use placeholders only during development. Client launch must not contain fabricated contact, pricing, legal, or location information.

