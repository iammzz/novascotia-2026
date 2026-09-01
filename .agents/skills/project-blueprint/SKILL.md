---
name: project-blueprint
description: Complete project architecture, planning workflow, and deployment guide for the Nova Scotia 2026 trip website. Use when asked about project structure, how the site is built, or how to set up a similar trip planning project.
---

# 🦞 Project Blueprint: Nova Scotia 2026

This skill documents the full architecture, planning workflow, and deployment setup for this trip planning website.

---

## 🏗️ Repository Architecture

```
├── .agents/
│   ├── AGENTS.md               # Agent behavioral rules (auto-push, sync, etc.)
│   └── skills/                 # Pi skills (review-sync, project-blueprint)
├── .github/workflows/main.yml  # CI/CD: deploys to GitHub Pages on push to main
├── docs/
│   ├── assets/
│   │   ├── data/
│   │   │   └── my_maps_import.csv  # Map pins: Lat, Lng, Day, Category, Name
│   │   └── images/             # Static map graphics or photos
│   ├── day1_*.md .. dayN_*.md  # Daily detailed itineraries
│   ├── index.md                # Site welcome page + overview table + Leaflet map
│   ├── logistics.md            # Flight/Hotel/Car bookings status
│   ├── maps.md                 # Interactive Leaflet map page
│   ├── packing_list.md         # All gear & clothing checklists
│   ├── potential_activities.md # Back-pocket activities vault
│   ├── research.md             # Cost-benefit analyses (e.g., Cabot Trail vs ferry routes)
│   ├── tips.md                 # Travel tips & etiquette
│   └── todo.md                 # Outstanding & completed tasks
├── receipts/                   # Booking confirmations (PDFs, images, text extracts)
├── docs/CNAME                  # Custom domain: novascotia.teaham.ca (copied to site root by gh-deploy)
└── mkdocs.yml                  # MkDocs nav, theme, extensions config
```

### 🗺️ Map Integration

The index and maps pages use **Leaflet.js** + **PapaParse** to render pins from `docs/assets/data/my_maps_import.csv`:

```html
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>
```

Pins are color-coded: **Red** = Base, **Green** = Hike, **Orange** = Activity, **Blue** = Other.

---

## 🧳 Planning Workflow

### Phase 1: Structure
1. Define date range, day-by-day skeleton, base towns (e.g., Halifax, Lunenburg, Wolfville, Baddeck/Ingonish)
2. Set up `mkdocs.yml` navigation, create daily markdown files

### Phase 2: Logistics & Checklists
1. Populate `docs/logistics.md` with flight, hotel, car details
2. Build `docs/todo.md` with phased booking checklist
3. Create `docs/packing_list.md` with baseline gear

### Phase 3: Detail Days
1. Build per-day files with hour-by-hour schedules, drive times, trail stats
2. Add dining sections with 5-6 curated options
3. Map coordinates to `my_maps_import.csv`

### Phase 4: Deploy
1. Push to `main` → GitHub Action builds and deploys to `gh-pages`
2. Site available at `novascotia.teaham.ca`

---

## 🛠️ Development & Deployment

### Local Preview
```bash
mkdocs serve
# → http://127.0.0.1:8000
```

### Production
Any push to `main` triggers `.github/workflows/main.yml`:

```yaml
- run: pip install mkdocs-material
- run: mkdocs gh-deploy --force
```

GitHub Pages must be set to deploy from the `gh-pages` branch.

---

## 📋 Key Conventions

- **Booking privacy**: Use `Confirmed` or `Booked` instead of PNR codes. Omit card details.
- **Status indicators**: 🟢 BOOKED / 🟡 PARTIALLY BOOKED / 🔴 NOT BOOKED
- **Costs**: Show per-person ranges, not exact totals with fees.
- **Auto-push**: All doc edits are committed and pushed immediately to keep the live site in sync.
- **Nova Scotia specifics**: Drives are long (Halifax→Cape Breton is a full day); check ferry schedules (North Sydney–Port aux Basques, Digby–Saint John); Cabot Trail is best done counter-clockwise from Baddeck.