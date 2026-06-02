# CLAUDE.md — Figueiredo Comércio e Serviços Website

## Project Overview

Static single-page marketing website for **Figueiredo Comércio e Serviços Ltda**, a construction and metalwork company based in Campo Grande, MS, Brazil. The entire site lives in a single self-contained HTML file.

- **Language:** Portuguese (pt-BR)
- **Stack:** Pure HTML5 + CSS3 + Vanilla JavaScript (ES6+)
- **No build tools, no package manager, no framework, no external dependencies** except Google Fonts

---

## Repository Structure

```
figueiredo/
├── index.html     # Complete website — all HTML, CSS, JS, and base64 images
└── CLAUDE.md      # This file
```

`index.html` is ~6 MB because it embeds the company logo as a base64-encoded PNG directly in the markup. Do not extract it unless specifically asked.

---

## HTML Page Sections

Navigation anchors and their purpose:

| Section ID  | Class         | Content |
|-------------|---------------|---------|
| *(header)*  | `header`      | Fixed nav bar, logo, phone, WhatsApp CTA |
| *(hero)*    | `hero`        | Hero banner, service cards, stats, CTAs |
| `#sobre`    | `dif`         | "Why Figueiredo" — 6 differentiator cards |
| `#servicos` | `servicos`    | 5 service cards (construction, renovation, metal, roofing, management) |
| `#galeria`  | `galeria`     | Photo gallery + hidden admin panel |
| `#processo` | `processo`    | 4-step process timeline |
| `#contato`  | `cta-sec`     | Contact CTA with WhatsApp button |
| *(footer)*  | `footer`      | Company name, location, CNPJ |

Hidden elements:
- `#painel-admin` — Admin photo management panel (toggled via **Alt+F** or admin button)
- `#foto-modal` — Fullscreen lightbox for gallery photos

---

## Design System

### CSS Variables (`:root`)

```css
--navy:  #0f1c35   /* primary dark */
--navy2: #1a2d4a
--navy3: #243d62
--gold:  #c8922a   /* accent */
--gold2: #e8b84b
--gold3: #a07020
--light: #f5f4ef   /* page background */
--white: #ffffff
--gray:  #6b7280   /* body text */
--lgray: #e8e6df
```

### Typography

- **Headings:** Oswald (400, 600, 700) via Google Fonts
- **Body:** Lato (300, 400, 700) via Google Fonts
- Font sizes use `clamp()` for fluid scaling

### Responsive Breakpoints

- **≤ 900px:** Grid layouts collapse to single column; desktop nav hidden
- **≤ 600px:** Gallery grid collapses to single column

### Key UI Classes

| Class | Purpose |
|-------|---------|
| `.btn-gold` | Primary gold-filled button |
| `.btn-outline` | Secondary outline button |
| `.btn-wpp-big` | Large WhatsApp green button |
| `.float-wpp` | Fixed floating WhatsApp button (pulsing animation) |
| `.r`, `.r1`–`.r4` | Scroll-triggered reveal animation (staggered delays) |
| `.dif-card` | Differentiator card with hover effect |
| `.serv-card` | Service card |
| `.gal-item` | Gallery photo item |
| `.proc-step` | Numbered process step circle |

---

## JavaScript Functionality

### Scroll Reveal Animation
`IntersectionObserver` watches all `.r` elements and adds a visible class when they enter the viewport. Classes `.r1`–`.r4` add staggered `transition-delay` for sequential appearance.

### Photo Gallery (localStorage)

Storage key: `figueiredo-fotos-v1`  
Format: JSON array of `{ src: "<base64>", caption: "<string>" }` objects

| Function | Description |
|----------|-------------|
| `carregarFotos()` | Read + JSON-parse photos from localStorage (with try/catch) |
| `salvarFotos(fotos)` | Persist photos array to localStorage |
| `renderGaleria()` | Re-render gallery grid and admin list |
| `abrirFoto(i)` | Open photo at index `i` in modal lightbox |
| `adicionarFoto()` | Read file input, base64-encode via `FileReader`, push to array |
| `removerFoto(i)` | Splice photo at index `i` after `confirm()` dialog |

### Admin Panel
- **Toggle:** keyboard shortcut **Alt+F**, or click the admin toggle button inside the gallery section
- Hidden by default (`display: none`)
- Allows adding images (any `image/*`) with an optional caption
- Shows a list of current photos with individual remove buttons

### Photo Modal / Lightbox
- Opens on gallery item click (`abrirFoto(i)`)
- Close via: ✕ button, or clicking the modal backdrop
- Image constrained to `90vw × 90vh`

---

## Business Content

| Field | Value |
|-------|-------|
| Company | Figueiredo Comércio e Serviços Ltda |
| Location | Campo Grande, MS, Brazil |
| Phone / WhatsApp | (67) 9 8432-6429 |
| Experience | 5+ years |
| Tagline | "Construindo soluções, gerando confiança" |
| WhatsApp message | "Olá! Gostaria de um orçamento gratuito para minha obra." |

---

## Development Workflow

### Editing the Site

Open `index.html` in any text editor. All CSS is in a `<style>` block near the top; all JavaScript is in a `<script>` block near the bottom. There is no build step — changes are visible immediately on browser refresh.

### Testing / Previewing

```bash
# Quick local server (Python)
python3 -m http.server 8080

# Or with Node (npx)
npx serve .
```

Then open `http://localhost:8080` in a browser.

### Accessing the Admin Panel

Open the site in a browser and press **Alt+F** to reveal the photo management panel inside the gallery section.

### Git Workflow

```bash
# Work on a feature or fix
git checkout -b your-branch-name

# Stage and commit
git add index.html
git commit -m "Short description of change"

# Push
git push -u origin your-branch-name
```

Active development branch: `claude/claude-md-docs-oFCia`  
Primary branch: `main`

---

## Conventions & Rules for AI Assistants

1. **One file rule.** All changes go into `index.html` unless the user explicitly requests a separate file.
2. **No build tooling.** Do not introduce npm, Webpack, Vite, TypeScript, or any build step unless the user asks.
3. **Portuguese content.** All user-visible text must remain in Portuguese (pt-BR). Do not translate to English.
4. **Preserve the design system.** Use the CSS variables defined in `:root`; do not introduce new hardcoded color values.
5. **No comments in code.** Follow the project's comment-free style. Only add a comment if a non-obvious constraint or workaround must be documented.
6. **localStorage gallery is client-only.** Photos added via the admin panel persist only in the visitor's browser. Do not assume or implement server-side storage unless the user requests it.
7. **No external JS libraries.** Keep the site dependency-free (Google Fonts CDN is acceptable).
8. **Mobile first, then desktop.** When adding new sections or components, ensure they work at ≤ 900px before styling the desktop view.
9. **Accessibility basics.** New images must have `alt` attributes; new interactive elements must be keyboard-accessible.
10. **Do not re-encode the logo.** The base64 PNG in the `<img>` tag is large (~6 MB). Do not regenerate or modify it unless the user provides a new logo file.
