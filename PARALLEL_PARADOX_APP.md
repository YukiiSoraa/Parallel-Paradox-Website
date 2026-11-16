# PARALLEL PARADOX — Official Member Roster App Blueprint

> Complete product specification, design system, architecture, and full verbatim source code for **Parallel Paradox (`PRX`)** — an Official Call of Duty: Mobile (CODM) Clan Member Roster Showcase styled in a **Navy Blue Y2K / Y3K** aesthetic.

---

## 1. Overview & Live Features

- **Brand:** `PARALLEL PARADOX` (`PRX`) — Call of Duty: Mobile Clan X Community
- **Canonical URL / Route:** `https://www.parallelparadox.com/roster` (root `/` automatically normalizes to `/roster`)
- **Theme:** Navy Blue Y2K / Y3K liquid-ice chrome aesthetic with a faded PRX fractured-ice background (`PRX_BACKGROUND_IMAGE = '/assets/aistudio/prx_a687ed31.jpg'`), horizon grid overlay, and frosted navy glass cards.
- **Top Navbar:**
  - Upper-left PRX emblem + editable clan title (`PARALLEL PARADOX`).
  - Right zone: `LOGIN / SIGN UP` button (visually identical to the hero's `MESSAGE US TO JOIN OUR COMMUNITY` button).
  - Logged-in Member state: User initials + Username badge + `LOG OUT` button.
  - Logged-in Admin state: User initials + Username badge + `[ADMIN // OVERRIDE]` sky-blue/chrome badge + `LOG OUT` button.
- **Hero Section (`#top`):**
  - Clean Y3K navy horizon grid over the faded PRX background (no 3D star/centerpiece).
  - Large liquid-ice chrome `PARALLEL PARADOX` headline.
  - Centered `MESSAGE US TO JOIN OUR COMMUNITY` CTA linking to `https://www.facebook.com/parallelparadoxx`.
  - Centered `VIEW MEMBER ROSTER` anchor button (`#roster`) with down arrow.
- **Official Member Roster Showcase (`#roster`):**
  - Search input (`"Search members by their IGN"`) and Sort selector (`Name A → Z`, `Name Z → A`, `Member # 01 → 99`).
  - Responsive 3-column grid of member portrait cards showing Member #, Division/Region, Role, IGN, Display Name, Bio, CODM UID, and `PROFILE` modal trigger.
  - **Admin-Only Controls** (`role === 'admin'`):
    - **Admin Roster Control Bar** above the grid with `+ ADD PLAYER` and `RESET DEFAULT ROSTERS`.
    - Direct **Edit** (`Pencil`) and **Delete** (`Trash2`) buttons on every member card.
    - **Click-to-Edit Inline Text & Image Swapper**: Click any text label/heading/bio or portrait/logo to edit inline or upload/paste a replacement image URL.
    - **Add / Edit Player Modal**: Avatar Image URL + device upload, `IGN *`, `GAME DIVISION`, `ROLE`, `DISPLAY NAME`, `SOCIAL HANDLE / URL`, `CODM UID`, `REGION`, and `SHORT BIO`.
- **Footer (`#clan-info`):**
  - Upper-left PRX emblem + `PARALLEL PARADOX` + description (`Official Call of Duty: Mobile Member Roster Showcase.`).
  - External links: **DISCORD** (`https://discord.gg/nj64e3E9Gr`) and **FACEBOOK** (`https://www.facebook.com/parallelparadoxx`).
  - Bottom bar: `© 2026 PARALLEL PARADOX · CALL OF DUTY: MOBILE CLAN X COMMUNITY` and `www.parallelparadox.com/roster`.

---

## 2. Color Palette & Typography

| Token | Value | Usage |
| :--- | :--- | :--- |
| Deep Abyssal Navy | `#060E24` / `#081229` | Base canvas & background gradient |
| Frosted Navy Glass | `#0F214D` / `#172F6B` | Cards, buttons, modal surfaces, hover states |
| Electric Sky Blue | `#38BDF8` / `#7DD3FC` / `#60A5FA` | Accents, aero buttons, active outlines, badges |
| Ice Chrome Highlight | `#BAE6FD` / `#E0F2FE` / `#F8FAFC` | Liquid-ice text gradients & primary copy |
| Muted Slate | `#94A3B8` / `#CBD5E1` | Secondary labels & metadata |

- **Display Font:** `Syne` (`600`, `700`, `800`)
- **Body Font:** `Plus Jakarta Sans` (`400`, `500`, `600`, `700`)
- **Monospace HUD / Controls:** `JetBrains Mono` (`400`, `500`, `600`, `700`)

---

## 3. Persistent Browser Storage Keys (`localStorage`)

| Key | Purpose |
| :--- | :--- |
| `paradox_users_db` | Registered accounts (`id`, `username`, `email`, `password`, `role: 'admin' \| 'member'`, `createdAt`). Pre-seeds default Admin account on initial boot if missing. |
| `paradox_active_session` | Active logged-in user session (`userId`, `username`, `email`, `role`, `rememberMe`, `loggedInAt`). Restored automatically across reloads until `LOG OUT`. |
| `paradox_rosters_db` | Array of `ClanMember` objects persisted across reloads; initialized with `DEFAULT_MEMBERS`. |
| `paradox_site_settings_db` | `ClanSiteSettings` object storing editable headings, CTA labels, and logo URL. |

---

## 4. Tech Stack & Project Structure

- **Frontend:** React 19, Vite 8, TypeScript, Tailwind CSS v4, Framer Motion (`motion/react`), Lucide React
- **Backend / Server:** Express (`server.ts`) serving Vite middleware in dev (`npm run dev`) and static `dist/` SPA fallback in production (`npm run build && npm start`) on port `3000`.

```text
/
├── index.html
├── metadata.json
├── package.json
├── server.ts
├── vite.config.ts
├── public/
│   ├── favicon.ico
│   └── assets/aistudio/prx_a687ed31.jpg
└── src/
    ├── main.tsx
    ├── index.css
    ├── App.tsx
    ├── data/
    │   └── paradoxData.ts
    ├── assets/images/
    │   ├── codm_y2k_kael_1794836286890.jpg
    │   ├── codm_y2k_vesper_clean_1794836319970.jpg
    │   ├── codm_y2k_nyx_1794836288393.jpg
    │   ├── codm_y2k_zero_1794836289017.jpg
    │   ├── codm_y2k_sable_1794836289803.jpg
    │   └── codm_y2k_kuro_1794836289082.jpg
    └── components/
        ├── Footer.tsx
        ├── HeroY2KShowcase.tsx
        ├── InlineEditor.tsx
        ├── Modals.tsx
        ├── Navbar.tsx
        ├── ParadoxEmblems.tsx
        └── RosterSection.tsx
```

---

## 5. Complete Source Files

### `metadata.json`
```json
{
  "name": "Parallel Paradox",
  "description": "PARALLEL PARADOX ROSTER",
  "requestFramePermissions": [],
  "majorCapabilities": [
    "MAJOR_CAPABILITY_SERVER_SIDE_GEMINI_API"
  ]
}
```

### `package.json`
```json
{
  "name": "react-example",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "tsx server.ts",
    "start": "node server.ts",
    "build": "vite build",
    "preview": "vite preview",
    "clean": "rm -rf dist server.js",
    "lint": "tsc --noEmit"
  },
  "dependencies": {
    "@google/genai": "^3.3.0",
    "@tailwindcss/vite": "^4.3.3",
    "@types/three": "^0.187.0",
    "@vitejs/plugin-react": "^6.1.1",
    "dotenv": "^17.2.3",
    "express": "^4.21.2",
    "lucide-react": "^0.546.0",
    "motion": "^12.23.24",
    "react": "^19.0.1",
    "react-dom": "^19.0.1",
    "three": "^0.187.0",
    "vite": "^8.3.0"
  },
  "devDependencies": {
    "@types/express": "^4.17.21",
    "@types/node": "^22.14.0",
    "@types/react": "^19.3.0",
    "@types/react-dom": "^19.3.0",
    "autoprefixer": "^10.4.21",
    "esbuild": "^0.25.0",
    "tailwindcss": "^4.3.3",
    "tsx": "^4.21.0",
    "typescript": "^7.0.2"
  }
}
```

### `vite.config.ts`
```ts
import tailwindcss from '@tailwindcss/vite';
import react from '@vitejs/plugin-react';
import fs from 'fs';
import path from 'path';
import {defineConfig} from 'vite';

export default defineConfig(() => {
  return {
    plugins: [
      react(),
      tailwindcss(),
      // Copies src/assets into dist/ so root-relative URLs like
      // "/src/assets/images/..." (e.g. generate_image output) still resolve
      // in production builds. Do not remove.
      {
        name: 'copy-unbundled-src-assets',
        apply: 'build',
        closeBundle(error?: Error): void {
          if (error) {
            return;
          }
          const distDir = path.resolve(__dirname, 'dist');
          const srcAssets = path.resolve(__dirname, 'src/assets');
          const distAssets = path.resolve(distDir, 'src/assets');
          if (fs.existsSync(distDir) && fs.existsSync(srcAssets)) {
            fs.cpSync(srcAssets, distAssets, {recursive: true, force: true});
          }
        },
      },
    ],
    resolve: {
      alias: {
        '@': path.resolve(__dirname, '.'),
      },
    },
    server: {
      // HMR is disabled in AI Studio via DISABLE_HMR env var.
      // Do not modify—file watching is disabled to prevent flickering during agent edits.
      hmr: process.env.DISABLE_HMR !== 'true',
      // Disable file watching when DISABLE_HMR is true to save CPU during agent edits.
      watch: process.env.DISABLE_HMR === 'true' ? null : {},
    },
  };
});
```

### `server.ts`
```ts
import express from 'express';
import fs from 'fs';
import path from 'path';
import crypto from 'crypto';
import { fileURLToPath } from 'url';
import dotenv from 'dotenv';
import { createServer as createViteServer } from 'vite';

dotenv.config();

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const app = express();
const PORT = 3000;

app.use(express.json({ limit: '4mb' }));

// Persistent hard-drive storage directory
const DATA_DIR = path.resolve(__dirname, 'data');
const ACCOUNTS_FILE = path.join(DATA_DIR, 'accounts.json');
const SESSIONS_FILE = path.join(DATA_DIR, 'sessions.json');
const ROSTER_FILE = path.join(DATA_DIR, 'roster.json');
const TRYOUTS_FILE = path.join(DATA_DIR, 'tryouts.json');

interface StoredAccount {
  id: string;
  email: string;
  passwordHash: string;
  salt: string;
  role: 'owner' | 'member';
  createdAt: string;
}

interface StoredSession {
  token: string;
  userId: string;
  email: string;
  role: 'owner' | 'member';
  createdAt: string;
}

interface TryoutRecord {
  ign: string;
  division: string;
  rank: string;
  email: string;
  createdAt: string;
  dossierId: string;
}

function ensureDataDir() {
  if (!fs.existsSync(DATA_DIR)) {
    fs.mkdirSync(DATA_DIR, { recursive: true });
  }
}

function readJsonFile<T>(filePath: string, fallback: T): T {
  try {
    if (!fs.existsSync(filePath)) return fallback;
    const raw = fs.readFileSync(filePath, 'utf-8');
    return JSON.parse(raw) as T;
  } catch {
    return fallback;
  }
}

function writeJsonFile(filePath: string, data: unknown) {
  ensureDataDir();
  fs.writeFileSync(filePath, JSON.stringify(data, null, 2), 'utf-8');
}

function hashPassword(password: string, salt: string): string {
  return crypto.scryptSync(password, salt, 64).toString('hex');
}

function verifyPassword(password: string, salt: string, storedHash: string): boolean {
  const candidate = crypto.scryptSync(password, salt, 64);
  const existing = Buffer.from(storedHash, 'hex');
  if (candidate.length !== existing.length) return false;
  return crypto.timingSafeEqual(candidate, existing);
}

function getSessionFromRequest(req: express.Request): StoredSession | null {
  const authHeader = req.headers.authorization || '';
  const token = authHeader.startsWith('Bearer ') ? authHeader.slice(7).trim() : '';
  if (!token) return null;
  const sessions = readJsonFile<StoredSession[]>(SESSIONS_FILE, []);
  return sessions.find((s) => s.token === token) || null;
}

// Auth Status
app.get('/api/auth/status', (req, res) => {
  const accounts = readJsonFile<StoredAccount[]>(ACCOUNTS_FILE, []);
  const session = getSessionFromRequest(req);
  const hasOwner = accounts.some((a) => a.role === 'owner');
  res.json({
    hasOwner,
    totalAccounts: accounts.length,
    user: session ? { email: session.email, role: session.role } : null,
  });
});

// Sign Up (Persisted on hard drive; ONLY the very first account becomes Owner)
app.post('/api/auth/signup', (req, res) => {
  const { email, password } = req.body as { email?: string; password?: string };
  const normalizedEmail = (email || '').trim().toLowerCase();

  if (!normalizedEmail || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(normalizedEmail)) {
    res.status(400).json({ error: 'Please enter a valid email address.' });
    return;
  }
  if (!password || password.length < 6) {
    res.status(400).json({ error: 'Password must be at least 6 characters.' });
    return;
  }

  const accounts = readJsonFile<StoredAccount[]>(ACCOUNTS_FILE, []);
  if (accounts.some((a) => a.email === normalizedEmail)) {
    res.status(409).json({ error: 'An account with this email already exists. Try logging in.' });
    return;
  }

  const hasOwner = accounts.some((a) => a.role === 'owner');
  const role: 'owner' | 'member' = hasOwner ? 'member' : 'owner';

  const salt = crypto.randomBytes(16).toString('hex');
  const passwordHash = hashPassword(password, salt);

  const newAccount: StoredAccount = {
    id: crypto.randomUUID(),
    email: normalizedEmail,
    passwordHash,
    salt,
    role,
    createdAt: new Date().toISOString(),
  };

  accounts.push(newAccount);
  writeJsonFile(ACCOUNTS_FILE, accounts);

  const token = crypto.randomBytes(32).toString('hex');
  const sessions = readJsonFile<StoredSession[]>(SESSIONS_FILE, []);
  sessions.push({
    token,
    userId: newAccount.id,
    email: newAccount.email,
    role: newAccount.role,
    createdAt: new Date().toISOString(),
  });
  writeJsonFile(SESSIONS_FILE, sessions);

  res.json({
    ok: true,
    token,
    user: { email: newAccount.email, role: newAccount.role },
  });
});

// Log In (Verified against persistent hard-drive account storage)
app.post('/api/auth/login', (req, res) => {
  const { email, password } = req.body as { email?: string; password?: string };
  const normalizedEmail = (email || '').trim().toLowerCase();

  if (!normalizedEmail || !password) {
    res.status(400).json({ error: 'Enter both your email and password.' });
    return;
  }

  const accounts = readJsonFile<StoredAccount[]>(ACCOUNTS_FILE, []);
  const account = accounts.find((a) => a.email === normalizedEmail);
  if (!account || !verifyPassword(password, account.salt, account.passwordHash)) {
    res.status(401).json({ error: 'Invalid email or password.' });
    return;
  }

  const token = crypto.randomBytes(32).toString('hex');
  const sessions = readJsonFile<StoredSession[]>(SESSIONS_FILE, []);
  sessions.push({
    token,
    userId: account.id,
    email: account.email,
    role: account.role,
    createdAt: new Date().toISOString(),
  });
  writeJsonFile(SESSIONS_FILE, sessions);

  res.json({
    ok: true,
    token,
    user: { email: account.email, role: account.role },
  });
});

// Log Out
app.post('/api/auth/logout', (req, res) => {
  const authHeader = req.headers.authorization || '';
  const token = authHeader.startsWith('Bearer ') ? authHeader.slice(7).trim() : '';
  if (token) {
    const sessions = readJsonFile<StoredSession[]>(SESSIONS_FILE, []);
    const next = sessions.filter((s) => s.token !== token);
    writeJsonFile(SESSIONS_FILE, next);
  }
  res.json({ ok: true });
});

// Public Roster Read
app.get('/api/roster', (_req, res) => {
  const saved = readJsonFile<Record<string, unknown>>(ROSTER_FILE, { custom: false });
  res.json(saved);
});

// Owner-Only Roster Write
app.put('/api/roster', (req, res) => {
  const session = getSessionFromRequest(req);
  if (!session) {
    res.status(401).json({ error: 'Please log in with the Owner account to save edits.' });
    return;
  }
  if (session.role !== 'owner') {
    res.status(403).json({ error: 'Only the single Clan Owner account can edit this website.' });
    return;
  }

  const payload = req.body;
  if (!payload || !Array.isArray(payload.members)) {
    res.status(400).json({ error: 'Invalid roster format.' });
    return;
  }

  writeJsonFile(ROSTER_FILE, payload);
  res.json({ ok: true });
});

// Persistent Tryout Submissions
app.post('/api/tryout', (req, res) => {
  const { ign, division, rank, email } = req.body as {
    ign?: string;
    division?: string;
    rank?: string;
    email?: string;
  };
  if (
    !ign ||
    !ign.trim() ||
    !email ||
    !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.trim())
  ) {
    res.status(400).json({ error: 'Please fill in your IGN and a valid email.' });
    return;
  }
  const dossierId = `PRX-${Math.floor(1000 + Math.random() * 9000)}`;
  const tryouts = readJsonFile<TryoutRecord[]>(TRYOUTS_FILE, []);
  tryouts.push({
    ign: ign.trim(),
    division: division || 'Call of Duty: Mobile',
    rank: (rank || 'Applicant').trim(),
    email: email.trim().toLowerCase(),
    createdAt: new Date().toISOString(),
    dossierId,
  });
  writeJsonFile(TRYOUTS_FILE, tryouts);
  res.json({ ok: true, dossierId });
});

async function startServer() {
  if (process.env.NODE_ENV !== 'production') {
    const vite = await createViteServer({
      server: { middlewareMode: true },
      appType: 'spa',
    });
    app.use(vite.middlewares);
  } else {
    const distPath = path.resolve(__dirname, 'dist');
    app.use(express.static(distPath));
    app.get('*', (_req, res) => {
      res.sendFile(path.join(distPath, 'index.html'));
    });
  }

  app.listen(PORT, '0.0.0.0', () => {
    console.log(`Parallel Paradox server listening on http://0.0.0.0:${PORT}`);
  });
}

startServer();
```

### `index.html`
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Parallel Paradox — Official Member Roster</title>
    <link rel="canonical" href="https://www.parallelparadox.com/roster" />
    <meta
      name="description"
      content="Parallel Paradox — Official Call of Duty: Mobile Member Roster Showcase."
    />
    <meta property="og:title" content="Parallel Paradox — Official Member Roster" />
    <meta
      property="og:description"
      content="Parallel Paradox — Official Call of Duty: Mobile Member Roster Showcase."
    />
    <meta property="og:url" content="https://www.parallelparadox.com/roster" />
    <meta property="og:type" content="website" />
    <meta name="twitter:card" content="summary_large_image" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700&family=Plus+Jakarta+Sans:wght@400;500;600;700&family=Syne:wght@600;700;800&display=swap"
      rel="stylesheet"
    />
  </head>
  <body class="bg-[#081229] text-[#F8FAFC] antialiased selection:bg-[#60A5FA]/35 selection:text-white">
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```

### `src/main.tsx`
```tsx
import {createRoot} from 'react-dom/client';
import App from './App.tsx';
import './index.css';

createRoot(document.getElementById('root')!).render(<App />);
```

### `src/index.css`
```css
@import "tailwindcss";

@theme {
  --font-display: "Syne", system-ui, sans-serif;
  --font-sans: "Plus Jakarta Sans", system-ui, sans-serif;
  --font-mono: "JetBrains Mono", ui-monospace, SFMono-Regular, monospace;
}

@layer base {
  html {
    scroll-behavior: smooth;
    background-color: #060e24;
    color: #f8fafc;
    font-family: var(--font-sans);
  }

  ::-webkit-scrollbar {
    width: 8px;
    height: 8px;
  }

  ::-webkit-scrollbar-track {
    background: #060e24;
  }

  ::-webkit-scrollbar-thumb {
    background: rgba(147, 197, 253, 0.35);
    border-radius: 999px;
    border: 2px solid #060e24;
  }

  ::-webkit-scrollbar-thumb:hover {
    background: #60a5fa;
  }
}

/* Y3K Navy Liquid Ice-Chrome Headline */
.text-y3k-ice {
  background: linear-gradient(
    180deg,
    #ffffff 0%,
    #e0f2fe 36%,
    #7dd3fc 70%,
    #f8fafc 100%
  );
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

/* Y3K Navy Horizon Grid */
.y3k-navy-grid,
.y2k-navy-grid {
  background-image:
    linear-gradient(to right, rgba(125, 211, 252, 0.08) 1px, transparent 1px),
    linear-gradient(to bottom, rgba(125, 211, 252, 0.08) 1px, transparent 1px);
  background-size: 44px 44px;
}

/* Frosted Navy Y3K Glass Hardware Card */
.y3k-navy-card,
.y2k-navy-card {
  background: linear-gradient(
    165deg,
    rgba(14, 30, 68, 0.82) 0%,
    rgba(7, 16, 40, 0.9) 100%
  );
  backdrop-filter: blur(14px);
  box-shadow:
    inset 0 1px 0 rgba(224, 242, 254, 0.18),
    0 14px 34px -14px rgba(2, 6, 23, 0.85);
}

/* Glossy Y3K Ice-Blue Aero Button */
.y3k-aero-btn,
.y2k-aero-btn {
  background: linear-gradient(180deg, #bae6fd 0%, #38bdf8 48%, #2563eb 100%);
  box-shadow:
    inset 0 1px 1px rgba(255, 255, 255, 0.85),
    0 8px 22px -6px rgba(56, 189, 248, 0.5);
}

.y3k-aero-btn:hover,
.y2k-aero-btn:hover {
  background: linear-gradient(180deg, #e0f2fe 0%, #60a5fa 48%, #2563eb 100%);
}

@media (prefers-reduced-motion: reduce) {
  html {
    scroll-behavior: auto;
  }
}
```

### `src/data/paradoxData.ts`
```ts
/**
 * ============================================================================
 * PARALLEL PARADOX — OFFICIAL MEMBER ROSTER DATA
 * ============================================================================
 * Edit this directly on the website by logging into the Admin account and
 * using the Admin Roster Control Bar — no coding required.
 * ============================================================================
 */

export interface ClanMember {
  id: string;
  number: string;
  ign: string;
  realName: string;
  uid: string;
  role: string;
  division?: string;
  stats?: string;
  region: string;
  portraitUrl: string;
  bio: string;
  tiktok?: string;
  youtube?: string;
  x?: string;
}

export interface ClanSiteSettings {
  clanName: string;
  logoUrl: string;
  communityCtaLabel: string;
  rosterCtaLabel: string;
  rosterKicker: string;
  rosterHeading: string;
  footerTagline: string;
  footerCopyright: string;
  footerBadge: string;
  discordLabel: string;
  facebookLabel: string;
  siteUrl: string;
}

export const PRX_BACKGROUND_IMAGE = '/assets/aistudio/prx_a687ed31.jpg';

export const DEFAULT_SITE_SETTINGS: ClanSiteSettings = {
  clanName: 'PARALLEL PARADOX',
  logoUrl: PRX_BACKGROUND_IMAGE,
  communityCtaLabel: 'MESSAGE US TO JOIN OUR COMMUNITY',
  rosterCtaLabel: 'VIEW MEMBER ROSTER',
  rosterKicker: 'OFFICIAL MEMBER ROSTER SHOWCASE',
  rosterHeading: 'Official Member Roster',
  footerTagline: 'Official Call of Duty: Mobile Member Roster Showcase.',
  footerCopyright: '© 2026 PARALLEL PARADOX · CALL OF DUTY: MOBILE CLAN X COMMUNITY',
  footerBadge: 'OFFICIAL MEMBER ROSTER',
  discordLabel: 'DISCORD',
  facebookLabel: 'FACEBOOK',
  siteUrl: 'www.parallelparadox.gg',
};

export const PRESET_PORTRAITS = [
  { label: 'Portrait 01 (Sky Jacket)', url: '/src/assets/images/codm_y2k_kael_1794836286890.jpg' },
  { label: 'Portrait 02 (Blue Stripe)', url: '/src/assets/images/codm_y2k_vesper_clean_1794836319970.jpg' },
  { label: 'Portrait 03 (Ice Fleece)', url: '/src/assets/images/codm_y2k_nyx_1794836288393.jpg' },
  { label: 'Portrait 04 (Cobalt Jacket)', url: '/src/assets/images/codm_y2k_zero_1794836289017.jpg' },
  { label: 'Portrait 05 (Star Hoodie)', url: '/src/assets/images/codm_y2k_sable_1794836289803.jpg' },
  { label: 'Portrait 06 (Silver Knit)', url: '/src/assets/images/codm_y2k_kuro_1794836289082.jpg' },
];

export const DEFAULT_MEMBERS: ClanMember[] = [
  {
    id: 'member-kael',
    number: '01',
    ign: 'PRX·KAEL',
    realName: 'Ren Takahashi',
    uid: '71948204192847102',
    role: 'Co-Leader',
    region: 'JP / NA',
    portraitUrl: '/src/assets/images/codm_y2k_kael_1794836286890.jpg',
    bio: 'Leading ranked lobbies and tournament scrims for Parallel Paradox.',
    tiktok: 'https://tiktok.com',
    youtube: 'https://youtube.com',
    x: 'https://x.com',
  },
  {
    id: 'member-vesper',
    number: '02',
    ign: 'PRX·VESPER',
    realName: 'Chloe Lindqvist',
    uid: '71948204998120411',
    role: 'Clan Leader',
    region: 'SE / EU',
    portraitUrl: '/src/assets/images/codm_y2k_vesper_clean_1794836319970.jpg',
    bio: 'Clan founder and main shotcaller for Parallel Paradox Call of Duty: Mobile.',
    tiktok: 'https://tiktok.com',
    youtube: 'https://youtube.com',
    x: 'https://x.com',
  },
  {
    id: 'member-nyx',
    number: '03',
    ign: 'PRX·NYX',
    realName: 'Kasper Holm',
    uid: '71948205541092834',
    role: 'Member',
    region: 'DK / EU',
    portraitUrl: '/src/assets/images/codm_y2k_nyx_1794836288393.jpg',
    bio: 'Search & Destroy specialist known for fast opening picks.',
    tiktok: 'https://tiktok.com',
    youtube: 'https://youtube.com',
  },
  {
    id: 'member-zero',
    number: '04',
    ign: 'PRX·ZERO',
    realName: 'Devon Sterling',
    uid: '71948206120934812',
    role: 'Member',
    region: 'CA / NA',
    portraitUrl: '/src/assets/images/codm_y2k_zero_1794836289017.jpg',
    bio: 'Dependable Hardpoint and Control anchor keeping spawns locked for the squad.',
    youtube: 'https://youtube.com',
    x: 'https://x.com',
  },
  {
    id: 'member-sable',
    number: '05',
    ign: 'PRX·SABLE',
    realName: 'Camila Reyes',
    uid: '71948207442190552',
    role: 'BR Captain',
    region: 'MX / NA',
    portraitUrl: '/src/assets/images/codm_y2k_sable_1794836289803.jpg',
    bio: 'Alcatraz & Isolated custom lobby captain leading Battle Royale squads.',
    tiktok: 'https://tiktok.com',
    x: 'https://x.com',
  },
  {
    id: 'member-kuro',
    number: '06',
    ign: 'PRX·KURO',
    realName: 'Haruto Sato',
    uid: '71948208812340019',
    role: 'Member',
    region: 'JP / SEA',
    portraitUrl: '/src/assets/images/codm_y2k_kuro_1794836289082.jpg',
    bio: 'Clan Wars contributor keeping Parallel Paradox top-ranked.',
    tiktok: 'https://tiktok.com',
    youtube: 'https://youtube.com',
  },
];
```

### `src/App.tsx`
```tsx
/**
 * @license
 * SPDX-License-Identifier: Apache-2.0
 */

import React, { useEffect, useState } from 'react';
import {
  ClanMember,
  ClanSiteSettings,
  DEFAULT_MEMBERS,
  DEFAULT_SITE_SETTINGS,
  PRX_BACKGROUND_IMAGE,
} from './data/paradoxData';
import { ActiveSession, Navbar } from './components/Navbar';
import { HeroY2KShowcase } from './components/HeroY2KShowcase';
import { RosterSection } from './components/RosterSection';
import { Footer } from './components/Footer';
import { ImageSwapModal, ImageSwapTarget } from './components/InlineEditor';
import {
  ACTIVE_SESSION_KEY,
  AuthModal,
  ensureSeededUsersDb,
  MemberEditorModal,
  MemberProfileModal,
} from './components/Modals';

const ROSTERS_DB_KEY = 'paradox_rosters_db';
const SITE_SETTINGS_DB_KEY = 'paradox_site_settings_db';

function loadInitialRoster(): ClanMember[] {
  try {
    const saved = localStorage.getItem(ROSTERS_DB_KEY);
    if (saved) {
      const parsed = JSON.parse(saved);
      if (Array.isArray(parsed) && parsed.length > 0) {
        return parsed as ClanMember[];
      }
      if (parsed && Array.isArray(parsed.members) && parsed.members.length > 0) {
        return parsed.members as ClanMember[];
      }
    }
    localStorage.setItem(ROSTERS_DB_KEY, JSON.stringify(DEFAULT_MEMBERS));
  } catch {
    // Ignore storage error
  }
  return DEFAULT_MEMBERS;
}

function loadInitialSettings(): ClanSiteSettings {
  try {
    const saved = localStorage.getItem(SITE_SETTINGS_DB_KEY);
    if (saved) {
      const parsed = JSON.parse(saved) as Partial<ClanSiteSettings>;
      if (parsed?.clanName) {
        return { ...DEFAULT_SITE_SETTINGS, ...parsed };
      }
    }
  } catch {
    // Ignore storage error
  }
  return DEFAULT_SITE_SETTINGS;
}

export default function App() {
  const [members, setMembers] = useState<ClanMember[]>(loadInitialRoster);
  const [siteSettings, setSiteSettings] = useState<ClanSiteSettings>(loadInitialSettings);

  // Restore active session from localStorage on initial load
  const [session, setSession] = useState<ActiveSession | null>(() => {
    try {
      const raw = localStorage.getItem(ACTIVE_SESSION_KEY);
      if (!raw) return null;
      return JSON.parse(raw) as ActiveSession;
    } catch {
      return null;
    }
  });

  const [authOpen, setAuthOpen] = useState(false);
  const [selectedMemberId, setSelectedMemberId] = useState<string | null>(null);
  const [editingMember, setEditingMember] = useState<ClanMember | null | undefined>(undefined);
  const [imageSwapTarget, setImageSwapTarget] = useState<ImageSwapTarget | null>(null);
  const [saveFeedback, setSaveFeedback] = useState<string>('');

  // Automatically seed default Admin account in paradox_users_db and normalize path to /roster
  useEffect(() => {
    ensureSeededUsersDb();
    if (window.location.pathname === '/' || window.location.pathname === '') {
      window.history.replaceState(
        null,
        '',
        `/roster${window.location.search}${window.location.hash}`
      );
    }
  }, []);

  const isAdmin = session?.role === 'admin';
  const selectedMember = selectedMemberId
    ? members.find((m) => m.id === selectedMemberId) || null
    : null;

  const handleAuthSuccess = (nextSession: ActiveSession) => {
    setSession(nextSession);
  };

  const handleLogout = () => {
    localStorage.removeItem(ACTIVE_SESSION_KEY);
    setSession(null);
    setEditingMember(undefined);
    setImageSwapTarget(null);
  };

  const persistRosters = (nextMembers: ClanMember[]) => {
    if (!isAdmin) return;
    try {
      localStorage.setItem(ROSTERS_DB_KEY, JSON.stringify(nextMembers));
      setSaveFeedback('Roster saved.');
      setTimeout(() => setSaveFeedback(''), 2200);
    } catch {
      setSaveFeedback('Storage quota exceeded.');
    }
  };

  const persistSettings = (nextSettings: ClanSiteSettings) => {
    if (!isAdmin) return;
    try {
      localStorage.setItem(SITE_SETTINGS_DB_KEY, JSON.stringify(nextSettings));
      setSaveFeedback('Settings saved.');
      setTimeout(() => setSaveFeedback(''), 2200);
    } catch {
      setSaveFeedback('Storage quota exceeded.');
    }
  };

  const handleUpdateSetting = <K extends keyof ClanSiteSettings>(
    key: K,
    value: ClanSiteSettings[K]
  ) => {
    if (!isAdmin) return;
    const nextSettings = { ...siteSettings, [key]: value };
    setSiteSettings(nextSettings);
    persistSettings(nextSettings);
  };

  const handleUpdateMember = (id: string, patch: Partial<ClanMember>) => {
    if (!isAdmin) return;
    const nextMembers = members.map((m) => (m.id === id ? { ...m, ...patch } : m));
    setMembers(nextMembers);
    persistRosters(nextMembers);
  };

  const handleSaveMember = (savedMember: ClanMember) => {
    if (!isAdmin) return;
    const exists = members.some((m) => m.id === savedMember.id);
    const nextMembers = exists
      ? members.map((m) => (m.id === savedMember.id ? savedMember : m))
      : [...members, savedMember];
    setMembers(nextMembers);
    persistRosters(nextMembers);
    setEditingMember(undefined);
  };

  const handleDeleteMember = (id: string) => {
    if (!isAdmin) return;
    const nextMembers = members.filter((m) => m.id !== id);
    setMembers(nextMembers);
    persistRosters(nextMembers);
    if (selectedMemberId === id) {
      setSelectedMemberId(null);
    }
  };

  const handleResetDefaults = () => {
    if (!isAdmin) return;
    setMembers(DEFAULT_MEMBERS);
    setSiteSettings(DEFAULT_SITE_SETTINGS);
    persistRosters(DEFAULT_MEMBERS);
    persistSettings(DEFAULT_SITE_SETTINGS);
  };

  const handleSwapLogo = () => {
    if (!isAdmin) return;
    setImageSwapTarget({
      label: 'Clan Logo Emblem',
      currentUrl: siteSettings.logoUrl,
      defaultUrl: PRX_BACKGROUND_IMAGE,
      onApply: (nextUrl) => handleUpdateSetting('logoUrl', nextUrl),
    });
  };

  const handleSwapMemberImage = (member: ClanMember) => {
    if (!isAdmin) return;
    const defaultMatch = DEFAULT_MEMBERS.find((m) => m.id === member.id);
    setImageSwapTarget({
      label: `${member.ign} Portrait`,
      currentUrl: member.portraitUrl,
      defaultUrl: defaultMatch?.portraitUrl || member.portraitUrl,
      showPresets: true,
      onApply: (nextUrl) => handleUpdateMember(member.id, { portraitUrl: nextUrl }),
    });
  };

  const nextMemberNumber = String(members.length + 1).padStart(2, '0');

  return (
    <div className="relative min-h-screen bg-[#060E24] text-[#F8FAFC] flex flex-col">
      {/* Faded PRX Ice Logo Background using Navy Blue Y3K Aesthetic */}
      <div
        aria-hidden="true"
        className="fixed inset-0 z-0 pointer-events-none overflow-hidden"
      >
        <img
          src={PRX_BACKGROUND_IMAGE}
          alt=""
          referrerPolicy="no-referrer"
          className="w-full h-full object-cover object-center opacity-[0.14] brightness-75 contrast-125 saturate-75 scale-105"
        />
        <div className="absolute inset-0 bg-gradient-to-b from-[#060E24]/75 via-[#081229]/65 to-[#060E24]/92" />
      </div>

      <div className="relative z-10 flex flex-col min-h-screen">
        <Navbar
          settings={siteSettings}
          session={session}
          editEnabled={isAdmin}
          onUpdateSetting={handleUpdateSetting}
          onSwapLogo={handleSwapLogo}
          onOpenAuth={() => setAuthOpen(true)}
          onLogout={handleLogout}
        />

        {saveFeedback && (
          <div
            role="status"
            className="fixed bottom-5 right-5 z-50 px-4 py-2.5 bg-[#0F214D] border border-[#60A5FA] text-[#E0F2FE] font-mono text-xs rounded-xl shadow-lg"
          >
            {saveFeedback}
          </div>
        )}

        <main className="flex-1">
          <HeroY2KShowcase
            settings={siteSettings}
            editEnabled={isAdmin}
            onUpdateSetting={handleUpdateSetting}
          />

          <RosterSection
            settings={siteSettings}
            members={members}
            editEnabled={isAdmin}
            onUpdateSetting={handleUpdateSetting}
            onUpdateMember={handleUpdateMember}
            onSwapMemberImage={handleSwapMemberImage}
            onSelectMember={(m) => setSelectedMemberId(m.id)}
            onAddMember={() => setEditingMember(null)}
            onEditMember={(m) => setEditingMember(m)}
            onDeleteMember={handleDeleteMember}
            onResetDefaults={handleResetDefaults}
          />
        </main>

        <Footer clanName={siteSettings.clanName} />
      </div>

      {authOpen && (
        <AuthModal
          onAuthSuccess={handleAuthSuccess}
          onClose={() => setAuthOpen(false)}
        />
      )}

      {selectedMember && (
        <MemberProfileModal
          member={selectedMember}
          editEnabled={isAdmin}
          onUpdateMember={handleUpdateMember}
          onSwapImage={handleSwapMemberImage}
          onClose={() => setSelectedMemberId(null)}
        />
      )}

      {isAdmin && editingMember !== undefined && (
        <MemberEditorModal
          initialMember={editingMember}
          nextNumber={nextMemberNumber}
          onSave={handleSaveMember}
          onClose={() => setEditingMember(undefined)}
        />
      )}

      {isAdmin && imageSwapTarget && (
        <ImageSwapModal
          target={imageSwapTarget}
          onClose={() => setImageSwapTarget(null)}
        />
      )}
    </div>
  );
}
```

### `src/components/Navbar.tsx`
```tsx
import React from 'react';
import { LogOut } from 'lucide-react';
import { ClanSiteSettings } from '../data/paradoxData';
import { InlineEditableText } from './InlineEditor';
import { PrxLogoEmblem } from './ParadoxEmblems';

export interface AppUser {
  id: string;
  username: string;
  email: string;
  password: string;
  role: 'admin' | 'member';
  createdAt: string;
}

export interface ActiveSession {
  userId: string;
  username: string;
  email: string;
  role: 'admin' | 'member';
  rememberMe: boolean;
  loggedInAt: string;
}

interface NavbarProps {
  settings: ClanSiteSettings;
  session: ActiveSession | null;
  editEnabled: boolean;
  onUpdateSetting: <K extends keyof ClanSiteSettings>(key: K, value: ClanSiteSettings[K]) => void;
  onSwapLogo: () => void;
  onOpenAuth: () => void;
  onLogout: () => void;
}

function getInitials(name: string): string {
  const clean = name.replace(/PRX[·.\s-]*/i, '').trim() || name.trim();
  const parts = clean.split(/[\s_-]+/).filter(Boolean);
  if (parts.length >= 2) {
    return (parts[0][0] + parts[1][0]).toUpperCase();
  }
  return clean.slice(0, 2).toUpperCase();
}

export function Navbar({
  settings,
  session,
  editEnabled,
  onUpdateSetting,
  onSwapLogo,
  onOpenAuth,
  onLogout,
}: NavbarProps) {
  return (
    <header className="sticky top-0 z-40 w-full bg-[#060E24]/85 backdrop-blur-xl border-b border-[#93C5FD]/25">
      <div className="mx-auto max-w-[1200px] px-4 md:px-8 h-16 flex items-center justify-between gap-4">
        {/* Upper-left PRX Logo + Brand Wordmark */}
        <a
          href="#top"
          onClick={(e) => {
            if (editEnabled) e.preventDefault();
          }}
          className="flex items-center gap-3 whitespace-nowrap shrink-0 group focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-[#60A5FA] rounded-lg"
        >
          <PrxLogoEmblem
            className="w-8 h-8 transition-transform duration-200 group-hover:scale-105"
            src={settings.logoUrl}
            editEnabled={editEnabled}
            onSwapImage={onSwapLogo}
          />
          <InlineEditableText
            value={settings.clanName}
            onSave={(next) => onUpdateSetting('clanName', next)}
            editEnabled={editEnabled}
            as="span"
            className="font-display text-base md:text-lg font-extrabold tracking-tight text-y3k-ice"
          />
        </a>

        {/* Right Zone: LOGIN / SIGN UP (identical to hero CTA) OR Authenticated State */}
        <div className="flex items-center gap-2.5 shrink-0">
          {!session ? (
            <button
              type="button"
              onClick={onOpenAuth}
              className="inline-flex items-center justify-center gap-2 px-6 py-3 font-mono text-xs font-semibold tracking-[0.14em] text-[#F8FAFC] bg-[#0F214D]/80 hover:bg-[#172F6B] border border-[#93C5FD]/35 hover:border-[#93C5FD] rounded-xl backdrop-blur-md transition-all duration-150 whitespace-nowrap shrink-0 cursor-pointer focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-[#93C5FD]"
            >
              <span>LOGIN / SIGN UP</span>
            </button>
          ) : (
            <div className="flex items-center gap-2 sm:gap-2.5 flex-wrap justify-end">
              {/* Username / Gamertag Badge */}
              <div className="inline-flex items-center gap-2 pl-1.5 pr-3 py-1.5 bg-[#0F214D]/85 border border-[#93C5FD]/35 rounded-xl backdrop-blur-md">
                <span className="w-6 h-6 rounded-lg bg-[#1E3A8A] border border-[#93C5FD]/40 text-[#E0F2FE] font-mono text-[11px] font-bold flex items-center justify-center shrink-0">
                  {getInitials(session.username)}
                </span>
                <span className="font-mono text-xs font-semibold text-[#F8FAFC] max-w-[110px] sm:max-w-[160px] truncate">
                  {session.username}
                </span>
              </div>

              {/* Admin Sky-Blue / Chrome Override Badge */}
              {session.role === 'admin' && (
                <span className="inline-flex items-center px-2.5 py-1.5 rounded-xl font-mono text-[10px] sm:text-[11px] font-bold tracking-[0.08em] text-[#060E24] y3k-aero-btn border border-[#E0F2FE]/80 whitespace-nowrap shrink-0">
                  [ADMIN // OVERRIDE]
                </span>
              )}

              {/* LOG OUT Button */}
              <button
                type="button"
                onClick={onLogout}
                className="inline-flex items-center justify-center gap-1.5 px-3.5 py-2 font-mono text-xs font-semibold tracking-[0.1em] text-[#F8FAFC] bg-[#0F214D]/80 hover:bg-[#172F6B] border border-[#93C5FD]/35 hover:border-[#93C5FD] rounded-xl backdrop-blur-md transition-all duration-150 whitespace-nowrap shrink-0 cursor-pointer focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-[#93C5FD]"
              >
                <LogOut className="w-3.5 h-3.5 text-[#93C5FD]" />
                <span>LOG OUT</span>
              </button>
            </div>
          )}
        </div>
      </div>
    </header>
  );
}
```

### `src/components/HeroY2KShowcase.tsx`
```tsx
import React from 'react';
import { ArrowDown } from 'lucide-react';
import { ClanSiteSettings } from '../data/paradoxData';
import { InlineEditableText } from './InlineEditor';

interface HeroY2KShowcaseProps {
  settings: ClanSiteSettings;
  editEnabled: boolean;
  onUpdateSetting: <K extends keyof ClanSiteSettings>(key: K, value: ClanSiteSettings[K]) => void;
}

export function HeroY2KShowcase({
  settings,
  editEnabled,
  onUpdateSetting,
}: HeroY2KShowcaseProps) {
  return (
    <section
      id="top"
      className="relative overflow-hidden border-b border-[#93C5FD]/20 y2k-navy-grid"
    >
      <div className="relative z-10 mx-auto max-w-[1200px] px-4 md:px-8 py-24 md:py-32 flex flex-col items-center text-center">
        <InlineEditableText
          value={settings.clanName}
          onSave={(next) => onUpdateSetting('clanName', next)}
          editEnabled={editEnabled}
          as="h1"
          className="font-display font-extrabold tracking-tight text-y3k-ice uppercase leading-[0.95] max-w-4xl"
          style={{
            fontSize: 'clamp(2.5rem, 6vw, 4.75rem)',
            textWrap: 'balance',
          }}
        />

        {/* Centered Join Community Facebook CTA + Roster Anchor */}
        <div className="mt-9 flex flex-col items-center justify-center gap-4">
          <a
            href="https://www.facebook.com/parallelparadoxx"
            target="_blank"
            rel="noreferrer"
            onClick={(e) => {
              if (editEnabled) e.preventDefault();
            }}
            className="inline-flex items-center justify-center gap-2 px-6 py-3 font-mono text-xs font-semibold tracking-[0.14em] text-[#F8FAFC] bg-[#0F214D]/80 hover:bg-[#172F6B] border border-[#93C5FD]/35 hover:border-[#93C5FD] rounded-xl backdrop-blur-md transition-all duration-150 whitespace-nowrap shrink-0 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-[#93C5FD]"
          >
            <InlineEditableText
              value={settings.communityCtaLabel}
              onSave={(next) => onUpdateSetting('communityCtaLabel', next)}
              editEnabled={editEnabled}
              as="span"
            />
          </a>

          <a
            href="#roster"
            onClick={(e) => {
              if (editEnabled) e.preventDefault();
            }}
            className="inline-flex items-center justify-center gap-2 px-6 py-3 font-mono text-xs font-semibold tracking-[0.14em] text-[#F8FAFC] bg-[#0F214D]/80 hover:bg-[#172F6B] border border-[#93C5FD]/35 hover:border-[#93C5FD] rounded-xl backdrop-blur-md transition-all duration-150 whitespace-nowrap shrink-0 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-[#93C5FD]"
          >
            <InlineEditableText
              value={settings.rosterCtaLabel}
              onSave={(next) => onUpdateSetting('rosterCtaLabel', next)}
              editEnabled={editEnabled}
              as="span"
            />
            <ArrowDown className="w-4 h-4" />
          </a>
        </div>
      </div>
    </section>
  );
}
```

### `src/components/RosterSection.tsx`
```tsx
import React, { useMemo, useState } from 'react';
import { motion } from 'motion/react';
import { ArrowUpRight, Pencil, Plus, RotateCcw, Search, ShieldCheck, Trash2 } from 'lucide-react';
import { ClanMember, ClanSiteSettings } from '../data/paradoxData';
import { InlineEditableText } from './InlineEditor';

export type SortOption = 'name_asc' | 'name_desc' | 'number_asc';

interface RosterSectionProps {
  settings: ClanSiteSettings;
  members: ClanMember[];
  editEnabled: boolean;
  onUpdateSetting: <K extends keyof ClanSiteSettings>(key: K, value: ClanSiteSettings[K]) => void;
  onUpdateMember: (id: string, patch: Partial<ClanMember>) => void;
  onSwapMemberImage: (member: ClanMember) => void;
  onSelectMember: (member: ClanMember) => void;
  onAddMember?: () => void;
  onEditMember?: (member: ClanMember) => void;
  onDeleteMember?: (id: string) => void;
  onResetDefaults?: () => void;
}

export function RosterSection({
  settings,
  members,
  editEnabled,
  onUpdateSetting,
  onUpdateMember,
  onSwapMemberImage,
  onSelectMember,
  onAddMember,
  onEditMember,
  onDeleteMember,
  onResetDefaults,
}: RosterSectionProps) {
  const [query, setQuery] = useState('');
  const [sortBy, setSortBy] = useState<SortOption>('name_asc');
  const [brokenImages, setBrokenImages] = useState<Record<string, boolean>>({});

  const visibleMembers = useMemo(() => {
    const q = query.trim().toLowerCase();
    const filtered = members.filter((m) => {
      if (!q) return true;
      return (
        m.ign.toLowerCase().includes(q) ||
        m.realName.toLowerCase().includes(q) ||
        m.role.toLowerCase().includes(q) ||
        (m.division && m.division.toLowerCase().includes(q)) ||
        m.region.toLowerCase().includes(q)
      );
    });

    return [...filtered].sort((a, b) => {
      if (sortBy === 'name_asc') return a.ign.localeCompare(b.ign);
      if (sortBy === 'name_desc') return b.ign.localeCompare(a.ign);
      if (sortBy === 'number_asc') {
        return (parseInt(a.number, 10) || 999) - (parseInt(b.number, 10) || 999);
      }
      return 0;
    });
  }, [members, query, sortBy]);

  return (
    <section
      id="roster"
      className="relative py-16 md:py-24 y2k-navy-grid border-b border-[#93C5FD]/20"
    >
      <div className="mx-auto max-w-[1200px] px-4 md:px-8">
        {/* Header Row */}
        <div className="flex flex-col md:flex-row md:items-end justify-between gap-6 mb-8">
          <div>
            <InlineEditableText
              value={settings.rosterKicker}
              onSave={(next) => onUpdateSetting('rosterKicker', next)}
              editEnabled={editEnabled}
              as="p"
              className="font-mono text-xs font-semibold tracking-[0.18em] text-[#93C5FD] mb-2"
            />
            <InlineEditableText
              value={settings.rosterHeading}
              onSave={(next) => onUpdateSetting('rosterHeading', next)}
              editEnabled={editEnabled}
              as="h2"
              className="font-display font-extrabold text-y3k-ice tracking-tight"
              style={{ fontSize: 'clamp(1.75rem, 3.2vw, 2.6rem)', textWrap: 'balance' }}
            />
          </div>

          <div className="flex items-center gap-3 font-mono text-xs text-[#CBD5E1]">
            <span className="tabular-nums">{visibleMembers.length} MEMBERS</span>
          </div>
        </div>

        {/* Admin Roster Control Bar */}
        {editEnabled && (
          <div className="mb-8 y2k-navy-card border border-[#7DD3FC]/55 rounded-2xl p-5 flex flex-col lg:flex-row lg:items-center justify-between gap-4">
            <div className="flex items-start gap-3">
              <ShieldCheck className="w-5 h-5 text-[#7DD3FC] shrink-0 mt-0.5" />
              <div>
                <div className="font-mono text-xs font-bold tracking-[0.08em] text-[#7DD3FC]">
                  ADMIN ROSTER CONTROL BAR
                </div>
                <p className="text-sm text-[#E2E8F0] mt-1">
                  Click any text or photo to edit inline, or use the buttons to add/edit/remove players.
                </p>
              </div>
            </div>

            <div className="flex flex-wrap items-center gap-2.5 shrink-0 font-mono text-xs">
              {onAddMember && (
                <button
                  type="button"
                  onClick={onAddMember}
                  className="inline-flex items-center gap-1.5 px-4 py-2.5 y2k-aero-btn text-[#060E24] font-bold rounded-xl cursor-pointer whitespace-nowrap"
                >
                  <Plus className="w-3.5 h-3.5" />
                  <span>+ ADD PLAYER</span>
                </button>
              )}
              {onResetDefaults && (
                <button
                  type="button"
                  onClick={onResetDefaults}
                  className="inline-flex items-center gap-1.5 px-3.5 py-2.5 bg-[#060E24] hover:bg-red-950/60 text-[#CBD5E1] hover:text-red-200 border border-[#93C5FD]/25 hover:border-red-400/50 rounded-xl cursor-pointer whitespace-nowrap"
                >
                  <RotateCcw className="w-3.5 h-3.5" />
                  <span>RESET DEFAULT ROSTERS</span>
                </button>
              )}
            </div>
          </div>
        )}

        {/* Search & Sort Directory Toolbar */}
        <div className="flex flex-col sm:flex-row items-stretch sm:items-center justify-between gap-3 mb-9">
          <div className="relative flex-1 max-w-md">
            <Search className="w-4 h-4 text-[#93C5FD] absolute left-3.5 top-1/2 -translate-y-1/2 pointer-events-none" />
            <label htmlFor="member-search" className="sr-only">
              Search members by their IGN
            </label>
            <input
              id="member-search"
              type="text"
              value={query}
              onChange={(e) => setQuery(e.target.value)}
              placeholder="Search members by their IGN"
              className="w-full bg-[#0F214D]/80 backdrop-blur-md border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none rounded-xl pl-10 pr-4 py-2.5 text-sm text-white placeholder:text-[#94A3B8]"
            />
          </div>

          <div className="flex items-center gap-2.5">
            <label htmlFor="member-sort" className="font-mono text-xs text-[#94A3B8] whitespace-nowrap">
              SORT BY:
            </label>
            <select
              id="member-sort"
              value={sortBy}
              onChange={(e) => setSortBy(e.target.value as SortOption)}
              className="bg-[#0F214D]/90 border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none rounded-xl px-3.5 py-2.5 font-mono text-xs text-[#F8FAFC] cursor-pointer"
            >
              <option value="name_asc">Name (A &rarr; Z)</option>
              <option value="name_desc">Name (Z &rarr; A)</option>
              <option value="number_asc">Member # (01 &rarr; 99)</option>
            </select>
          </div>
        </div>

        {/* Member Grid */}
        {visibleMembers.length === 0 ? (
          <div className="y2k-navy-card border border-[#93C5FD]/25 rounded-2xl p-12 text-center">
            <p className="font-display text-lg font-bold text-white">
              No matching members found
            </p>
            <p className="text-sm text-[#94A3B8] mt-1">
              Try clearing your search filter.
            </p>
            <button
              type="button"
              onClick={() => setQuery('')}
              className="mt-4 px-5 py-2 font-mono text-xs font-semibold text-[#060E24] y2k-aero-btn rounded-xl cursor-pointer"
            >
              CLEAR SEARCH
            </button>
          </div>
        ) : (
          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
            {visibleMembers.map((member) => {
              const isBroken = brokenImages[member.id];
              return (
                <motion.article
                  key={member.id}
                  layout
                  initial={{ opacity: 0, y: 8 }}
                  animate={{ opacity: 1, y: 0 }}
                  transition={{ duration: 0.18, ease: [0.16, 1, 0.3, 1] }}
                  className="group y2k-navy-card border border-[#93C5FD]/25 hover:border-[#60A5FA] rounded-2xl overflow-hidden flex flex-col transition-all duration-200 hover:-translate-y-0.5"
                >
                  {/* Portrait Container */}
                  <div
                    className={`relative aspect-[3/4] w-full overflow-hidden bg-[#0F214D] border-b border-[#93C5FD]/20 ${
                      editEnabled
                        ? 'outline-1 outline-dashed outline-[#7DD3FC]/60 hover:outline-[#BAE6FD]'
                        : ''
                    }`}
                  >
                    {!isBroken && member.portraitUrl ? (
                      <img
                        src={member.portraitUrl}
                        alt={`${member.ign} — ${member.role}`}
                        referrerPolicy="no-referrer"
                        onClick={(e) => {
                          if (editEnabled) {
                            e.preventDefault();
                            e.stopPropagation();
                            onSwapMemberImage(member);
                          }
                        }}
                        title={editEnabled ? 'Click photo to swap image' : undefined}
                        onError={() =>
                          setBrokenImages((prev) => ({ ...prev, [member.id]: true }))
                        }
                        className={`w-full h-full object-cover object-top transition-transform duration-300 group-hover:scale-[1.03] ${
                          editEnabled ? 'cursor-pointer' : ''
                        }`}
                      />
                    ) : (
                      <div
                        onClick={() => {
                          if (editEnabled) onSwapMemberImage(member);
                        }}
                        className={`w-full h-full flex flex-col items-center justify-center p-6 text-center bg-gradient-to-br from-[#1E3A8A] to-[#060E24] ${
                          editEnabled ? 'cursor-pointer' : ''
                        }`}
                      >
                        <span className="font-display text-3xl font-extrabold text-[#93C5FD]">
                          {member.ign.slice(0, 3)}
                        </span>
                        <span className="font-mono text-xs text-[#CBD5E1] mt-2">
                          {member.ign}
                        </span>
                      </div>
                    )}

                    {/* Contrast Scrim */}
                    <div
                      aria-hidden="true"
                      className="absolute inset-x-0 bottom-0 h-32 bg-gradient-to-t from-[#060E24]/95 via-[#060E24]/55 to-transparent pointer-events-none"
                    />

                    {/* Top Member Number & Profile Action */}
                    <div className="absolute top-3 left-3 right-3 flex items-center justify-between gap-2 font-mono text-[11px] pointer-events-none">
                      <span className="pointer-events-auto bg-[#060E24]/80 backdrop-blur-md text-[#E2E8F0] border border-[#93C5FD]/35 px-2.5 py-1 rounded-lg font-semibold truncate inline-flex items-center gap-1">
                        <span>NO.</span>
                        <InlineEditableText
                          value={member.number}
                          onSave={(next) => onUpdateMember(member.id, { number: next })}
                          editEnabled={editEnabled}
                          as="span"
                        />
                        <span>·</span>
                        <InlineEditableText
                          value={member.division || member.region}
                          onSave={(next) =>
                            onUpdateMember(
                              member.id,
                              member.division ? { division: next } : { region: next }
                            )
                          }
                          editEnabled={editEnabled}
                          as="span"
                        />
                      </span>

                      <div className="pointer-events-auto flex items-center gap-1.5 shrink-0">
                        {editEnabled && onEditMember && (
                          <button
                            type="button"
                            onClick={() => onEditMember(member)}
                            aria-label={`Edit ${member.ign}`}
                            title={`Edit ${member.ign}`}
                            className="inline-flex items-center justify-center bg-[#7DD3FC] hover:bg-white text-[#060E24] p-1.5 rounded-lg transition-colors cursor-pointer"
                          >
                            <Pencil className="w-3.5 h-3.5" />
                          </button>
                        )}
                        {editEnabled && onDeleteMember && (
                          <button
                            type="button"
                            onClick={() => onDeleteMember(member.id)}
                            aria-label={`Delete ${member.ign}`}
                            title={`Delete ${member.ign}`}
                            className="inline-flex items-center justify-center bg-red-500/90 hover:bg-red-500 text-white p-1.5 rounded-lg transition-colors cursor-pointer"
                          >
                            <Trash2 className="w-3.5 h-3.5" />
                          </button>
                        )}
                        <button
                          type="button"
                          onClick={() => onSelectMember(member)}
                          className="inline-flex items-center gap-1 bg-white/90 hover:bg-[#60A5FA] text-[#060E24] px-2.5 py-1 rounded-lg font-semibold transition-colors cursor-pointer shrink-0"
                        >
                          <span>PROFILE</span>
                          <ArrowUpRight className="w-3.5 h-3.5" />
                        </button>
                      </div>
                    </div>

                    {/* Portrait Identity */}
                    <div className="absolute bottom-3.5 left-4 right-4">
                      <InlineEditableText
                        value={member.role}
                        onSave={(next) => onUpdateMember(member.id, { role: next })}
                        editEnabled={editEnabled}
                        as="div"
                        className="font-mono text-[11px] tracking-[0.12em] text-[#93C5FD] inline-block"
                      />
                      <InlineEditableText
                        value={member.ign}
                        onSave={(next) => onUpdateMember(member.id, { ign: next })}
                        editEnabled={editEnabled}
                        as="h3"
                        className="font-display text-2xl font-extrabold tracking-tight text-white"
                      />
                      <InlineEditableText
                        value={member.realName}
                        onSave={(next) => onUpdateMember(member.id, { realName: next })}
                        editEnabled={editEnabled}
                        as="div"
                        className="font-mono text-xs text-[#CBD5E1] mt-0.5 inline-block"
                      />
                    </div>
                  </div>

                  {/* Clean Bio & Profile Action Footer */}
                  <div className="p-5 flex-1 flex flex-col justify-between gap-4">
                    <div className="space-y-2">
                      <InlineEditableText
                        value={member.bio || 'Official Parallel Paradox member.'}
                        onSave={(next) => onUpdateMember(member.id, { bio: next })}
                        editEnabled={editEnabled}
                        as="p"
                        multiline
                        className="text-xs text-[#CBD5E1] leading-relaxed line-clamp-2"
                      />
                      {member.stats && (
                        <InlineEditableText
                          value={member.stats}
                          onSave={(next) => onUpdateMember(member.id, { stats: next })}
                          editEnabled={editEnabled}
                          as="div"
                          className="font-mono text-[11px] text-[#7DD3FC]"
                        />
                      )}
                    </div>

                    <div className="flex items-center justify-between gap-3 pt-3 border-t border-[#93C5FD]/15 font-mono text-[11px]">
                      <span className="text-[#94A3B8] truncate inline-flex items-center gap-1">
                        {member.uid ? (
                          <>
                            <span>UID</span>
                            <InlineEditableText
                              value={member.uid}
                              onSave={(next) => onUpdateMember(member.id, { uid: next })}
                              editEnabled={editEnabled}
                              as="span"
                            />
                          </>
                        ) : (
                          <InlineEditableText
                            value={member.region}
                            onSave={(next) => onUpdateMember(member.id, { region: next })}
                            editEnabled={editEnabled}
                            as="span"
                          />
                        )}
                      </span>
                      <button
                        type="button"
                        onClick={() => onSelectMember(member)}
                        className="font-semibold text-[#93C5FD] hover:text-white whitespace-nowrap shrink-0 cursor-pointer"
                      >
                        VIEW PROFILE &rarr;
                      </button>
                    </div>
                  </div>
                </motion.article>
              );
            })}
          </div>
        )}
      </div>
    </section>
  );
}
```

### `src/components/Footer.tsx`
```tsx
import React from 'react';
import { ExternalLink } from 'lucide-react';
import { PrxLogoEmblem } from './ParadoxEmblems';

interface FooterProps {
  clanName: string;
}

const SOCIAL_LINKS = [
  { label: 'DISCORD', href: 'https://discord.gg/nj64e3E9Gr' },
  { label: 'FACEBOOK', href: 'https://www.facebook.com/parallelparadoxx' },
];

export function Footer({ clanName }: FooterProps) {
  return (
    <footer
      id="clan-info"
      className="bg-[#050C1D]/90 backdrop-blur-md border-t border-[#93C5FD]/25 pt-14 pb-10"
    >
      <div className="mx-auto max-w-[1200px] px-4 md:px-8">
        <div className="flex flex-col md:flex-row md:items-center justify-between gap-8 pb-10 border-b border-[#93C5FD]/20">
          <div className="space-y-3 max-w-md">
            <div className="flex items-center gap-3">
              <PrxLogoEmblem className="w-8 h-8" />
              <span className="font-display text-lg font-extrabold tracking-tight text-y3k-ice">
                {clanName}
              </span>
            </div>
            <p className="text-sm text-[#94A3B8] leading-relaxed">
              Official Call of Duty: Mobile Member Roster Showcase.
            </p>
          </div>

          <div className="flex flex-wrap items-center gap-6 font-mono text-xs text-[#CBD5E1]">
            {SOCIAL_LINKS.map((s) => (
              <a
                key={s.label}
                href={s.href}
                target="_blank"
                rel="noreferrer"
                className="inline-flex items-center gap-1.5 hover:text-[#93C5FD] transition-colors"
              >
                <span>{s.label}</span>
                <ExternalLink className="w-3.5 h-3.5" />
              </a>
            ))}
          </div>
        </div>

        <div className="pt-6 flex flex-col sm:flex-row items-center justify-between gap-2 font-mono text-[11px] text-[#94A3B8]">
          <span>&copy; 2026 {clanName} · CALL OF DUTY: MOBILE CLAN X COMMUNITY</span>
          <span>www.parallelparadox.com/roster</span>
        </div>
      </div>
    </footer>
  );
}
```

### `src/components/InlineEditor.tsx`
```tsx
import React, { useEffect, useRef, useState } from 'react';
import { Check, Link2, RotateCcw, Upload, X } from 'lucide-react';

interface InlineEditableTextProps {
  value: string;
  onSave: (next: string) => void;
  editEnabled: boolean;
  as?: 'span' | 'p' | 'h1' | 'h2' | 'h3' | 'div';
  multiline?: boolean;
  className?: string;
  style?: React.CSSProperties;
  placeholder?: string;
}

export function InlineEditableText({
  value,
  onSave,
  editEnabled,
  as: Tag = 'span',
  multiline = false,
  className = '',
  style,
  placeholder = 'Click to type...',
}: InlineEditableTextProps) {
  const [editing, setEditing] = useState(false);
  const [draft, setDraft] = useState(value);
  const initialValueRef = useRef(value);
  const inputRef = useRef<HTMLInputElement>(null);
  const textareaRef = useRef<HTMLTextAreaElement>(null);

  useEffect(() => {
    if (!editing) {
      setDraft(value);
    }
  }, [value, editing]);

  useEffect(() => {
    if (editing) {
      const el = multiline ? textareaRef.current : inputRef.current;
      if (el) {
        el.focus();
        el.select();
      }
    }
  }, [editing, multiline]);

  if (!editEnabled) {
    return (
      <Tag className={className} style={style}>
        {value}
      </Tag>
    );
  }

  const startEditing = (e: React.MouseEvent) => {
    e.preventDefault();
    e.stopPropagation();
    initialValueRef.current = value;
    setDraft(value);
    setEditing(true);
  };

  const commit = (nextVal: string) => {
    const cleaned = nextVal.trim() ? nextVal : initialValueRef.current;
    onSave(cleaned);
    setEditing(false);
  };

  const cancel = () => {
    onSave(initialValueRef.current);
    setDraft(initialValueRef.current);
    setEditing(false);
  };

  const handleChange = (nextVal: string) => {
    setDraft(nextVal);
    if (nextVal.trim()) {
      onSave(nextVal);
    }
  };

  if (editing) {
    const cleanClasses = className
      .replace(/\btext-y3k-ice\b/g, 'text-[#E0F2FE]')
      .replace(/\bline-clamp-\d+\b/g, '');

    if (multiline) {
      return (
        <textarea
          ref={textareaRef}
          rows={3}
          value={draft}
          onChange={(e) => handleChange(e.target.value)}
          onBlur={() => commit(draft)}
          onClick={(e) => {
            e.preventDefault();
            e.stopPropagation();
          }}
          onKeyDown={(e) => {
            if (e.key === 'Escape') {
              e.preventDefault();
              cancel();
            }
          }}
          placeholder={placeholder}
          style={style}
          className={`${cleanClasses} w-full bg-[#060E24]/95 text-[#E0F2FE] border border-dashed border-[#7DD3FC] rounded-md px-2 py-1 focus:outline-none`}
        />
      );
    }

    return (
      <input
        ref={inputRef}
        type="text"
        value={draft}
        size={Math.max(draft.length + 1, 6)}
        onChange={(e) => handleChange(e.target.value)}
        onBlur={() => commit(draft)}
        onClick={(e) => {
          e.preventDefault();
          e.stopPropagation();
        }}
        onKeyDown={(e) => {
          if (e.key === 'Enter') {
            e.preventDefault();
            commit(draft);
          } else if (e.key === 'Escape') {
            e.preventDefault();
            cancel();
          }
        }}
        placeholder={placeholder}
        style={style}
        className={`${cleanClasses} max-w-full bg-[#060E24]/95 text-[#E0F2FE] border border-dashed border-[#7DD3FC] rounded-md px-2 py-0.5 focus:outline-none`}
      />
    );
  }

  return (
    <Tag
      onClick={startEditing}
      title="Click to edit text"
      style={style}
      className={`${className} outline-1 outline-dashed outline-[#7DD3FC]/55 hover:outline-[#BAE6FD] rounded-sm cursor-text transition-[outline-color]`}
    >
      {value || placeholder}
    </Tag>
  );
}

export interface ImageSwapTarget {
  label: string;
  currentUrl: string;
  defaultUrl: string;
  showPresets?: boolean;
  onApply: (nextUrl: string) => void;
}

function compressImageFile(file: File): Promise<string> {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = () => {
      const img = new Image();
      img.onload = () => {
        const maxW = 720;
        const scale = Math.min(1, maxW / img.width);
        const canvas = document.createElement('canvas');
        canvas.width = Math.round(img.width * scale);
        canvas.height = Math.round(img.height * scale);
        const ctx = canvas.getContext('2d');
        if (!ctx) {
          resolve(String(reader.result));
          return;
        }
        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
        resolve(canvas.toDataURL('image/jpeg', 0.84));
      };
      img.onerror = reject;
      img.src = String(reader.result);
    };
    reader.onerror = reject;
    reader.readAsDataURL(file);
  });
}

export function ImageSwapModal({
  target,
  onClose,
}: {
  target: ImageSwapTarget;
  onClose: () => void;
}) {
  const [urlInput, setUrlInput] = useState(target.currentUrl);
  const [error, setError] = useState('');

  useEffect(() => {
    const onKey = (e: KeyboardEvent) => {
      if (e.key === 'Escape') onClose();
    };
    window.addEventListener('keydown', onKey);
    return () => window.removeEventListener('keydown', onKey);
  }, [onClose]);

  const handleUpload = async (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (!file) return;
    setError('');
    try {
      const dataUrl = await compressImageFile(file);
      setUrlInput(dataUrl);
      target.onApply(dataUrl);
      onClose();
    } catch {
      setError('Could not load that image file. Try a JPG, PNG, or WebP.');
    }
  };

  const handleSubmitUrl = (e: React.FormEvent) => {
    e.preventDefault();
    const cleaned = urlInput.trim();
    if (!cleaned) {
      setError('Paste an image URL or upload a photo.');
      return;
    }
    target.onApply(cleaned);
    onClose();
  };

  return (
    <div
      className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-[#040917]/80 backdrop-blur-sm"
      role="dialog"
      aria-modal="true"
      onClick={onClose}
    >
      <div
        className="relative w-full max-w-md y2k-navy-card border border-[#93C5FD]/40 rounded-2xl p-6 shadow-2xl"
        onClick={(e) => e.stopPropagation()}
      >
        <div className="flex items-start justify-between gap-4 pb-3 border-b border-[#93C5FD]/20">
          <div>
            <div className="font-mono text-[10px] tracking-[0.14em] text-[#7DD3FC]">
              CLICK-TO-SWAP IMAGE
            </div>
            <h3 className="font-display text-lg font-extrabold text-white mt-0.5">
              {target.label}
            </h3>
          </div>
          <button
            type="button"
            onClick={onClose}
            aria-label="Close image swapper"
            className="w-8 h-8 inline-flex items-center justify-center border border-[#93C5FD]/30 bg-[#060E24] hover:bg-[#172F6B] text-[#CBD5E1] hover:text-white rounded-lg cursor-pointer"
          >
            <X className="w-4 h-4" />
          </button>
        </div>

        <div className="mt-4 space-y-4 font-mono text-xs">
          <div className="flex items-center gap-4">
            <img
              src={urlInput || target.defaultUrl}
              alt="Preview"
              className="w-16 h-20 object-cover object-top rounded-xl border border-[#93C5FD]/40 shrink-0 bg-[#060E24]"
            />
            <div className="flex-1 space-y-2">
              <label className="w-full inline-flex items-center justify-center gap-2 px-3.5 py-2.5 y3k-aero-btn text-[#060E24] font-bold rounded-xl cursor-pointer">
                <Upload className="w-3.5 h-3.5" />
                <span>UPLOAD FROM COMPUTER</span>
                <input
                  type="file"
                  accept="image/*"
                  onChange={handleUpload}
                  className="hidden"
                />
              </label>

              <button
                type="button"
                onClick={() => {
                  target.onApply(target.defaultUrl);
                  onClose();
                }}
                className="w-full inline-flex items-center justify-center gap-1.5 px-3 py-1.5 bg-[#060E24] hover:bg-[#172F6B] text-[#CBD5E1] border border-[#93C5FD]/25 rounded-xl cursor-pointer text-[11px]"
              >
                <RotateCcw className="w-3 h-3" />
                <span>Restore Default Image</span>
              </button>
            </div>
          </div>

          <form onSubmit={handleSubmitUrl} className="space-y-2.5">
            <label className="block text-[11px] text-[#94A3B8]">
              OR PASTE IMAGE LINK (URL)
            </label>
            <div className="flex gap-2">
              <div className="relative flex-1">
                <Link2 className="w-3.5 h-3.5 text-[#93C5FD] absolute left-3 top-1/2 -translate-y-1/2 pointer-events-none" />
                <input
                  type="text"
                  value={urlInput}
                  onChange={(e) => setUrlInput(e.target.value)}
                  placeholder="https://example.com/photo.jpg"
                  className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none pl-8 pr-3 py-2 text-white rounded-xl"
                />
              </div>
              <button
                type="submit"
                className="inline-flex items-center gap-1 px-4 py-2 bg-[#0F214D] hover:bg-[#172F6B] text-[#E0F2FE] border border-[#93C5FD]/40 rounded-xl font-semibold cursor-pointer shrink-0"
              >
                <Check className="w-3.5 h-3.5" />
                <span>APPLY</span>
              </button>
            </div>
          </form>

          {error && <p className="text-amber-300 text-[11px]">{error}</p>}
        </div>
      </div>
    </div>
  );
}
```

### `src/components/ParadoxEmblems.tsx`
```tsx
import React from 'react';
import { PRX_BACKGROUND_IMAGE } from '../data/paradoxData';

/**
 * PRX Emblem inspired by the user's attached fractured ice PRX monogram.
 * Supports custom logo URLs and click-to-swap when Admin / Ctrl+E edit mode is active.
 */
export function PrxLogoEmblem({
  className = 'w-9 h-9',
  src = PRX_BACKGROUND_IMAGE,
  editEnabled = false,
  onSwapImage,
}: {
  className?: string;
  src?: string;
  editEnabled?: boolean;
  onSwapImage?: () => void;
}) {
  const isCustom = src !== PRX_BACKGROUND_IMAGE;

  const handleClick = (e: React.MouseEvent) => {
    if (editEnabled && onSwapImage) {
      e.preventDefault();
      e.stopPropagation();
      onSwapImage();
    }
  };

  return (
    <div
      onClick={handleClick}
      title={editEnabled ? 'Click to swap logo image' : undefined}
      className={`relative overflow-hidden rounded-xl border border-[#93C5FD]/45 bg-[#0B1736] shadow-[0_0_16px_rgba(96,165,250,0.28)] shrink-0 ${className} ${
        editEnabled
          ? 'outline-1 outline-dashed outline-[#7DD3FC]/75 hover:outline-[#BAE6FD] cursor-pointer'
          : ''
      }`}
    >
      <img
        src={src || PRX_BACKGROUND_IMAGE}
        alt="PRX Emblem"
        referrerPolicy="no-referrer"
        className={`w-full h-full object-cover ${
          isCustom ? 'object-center' : 'scale-[1.85] object-[50%_38%]'
        }`}
      />
    </div>
  );
}

/**
 * Crisp Y3K Ice-Chrome 4-point star accent
 */
export function Y3KIceStar({ className = 'w-4 h-4' }: { className?: string }) {
  return (
    <svg
      viewBox="0 0 32 32"
      fill="none"
      xmlns="http://www.w3.org/2000/svg"
      className={className}
      aria-hidden="true"
    >
      <path
        d="M16 2C17.4 11.2 20.8 14.6 30 16C20.8 17.4 17.4 20.8 16 30C14.6 20.8 11.2 17.4 2 16C11.2 14.6 14.6 11.2 16 2Z"
        fill="url(#y3k-star-grad)"
      />
      <defs>
        <linearGradient id="y3k-star-grad" x1="2" y1="2" x2="30" y2="30" gradientUnits="userSpaceOnUse">
          <stop offset="0%" stopColor="#FFFFFF" />
          <stop offset="50%" stopColor="#93C5FD" />
          <stop offset="100%" stopColor="#38BDF8" />
        </linearGradient>
      </defs>
    </svg>
  );
}
```

### `src/components/Modals.tsx`
```tsx
import React, { useEffect, useState } from 'react';
import { motion, AnimatePresence } from 'motion/react';
import {
  Check,
  Copy,
  ExternalLink,
  Eye,
  EyeOff,
  RotateCcw,
  ShieldAlert,
  Sparkles,
  Upload,
  X,
} from 'lucide-react';
import { ClanMember, ClanSiteSettings, PRESET_PORTRAITS } from '../data/paradoxData';
import { InlineEditableText } from './InlineEditor';
import { ActiveSession, AppUser } from './Navbar';

export const USERS_DB_KEY = 'paradox_users_db';
export const ACTIVE_SESSION_KEY = 'paradox_active_session';

export const DEFAULT_ADMIN_ACCOUNT: AppUser = {
  id: 'usr_admin_prx',
  username: 'admin',
  email: 'admin@parallelparadox.gg',
  password: 'paradox2026',
  role: 'admin',
  createdAt: '2026-01-01T00:00:00.000Z',
};

export function ensureSeededUsersDb(): AppUser[] {
  try {
    const raw = localStorage.getItem(USERS_DB_KEY);
    if (!raw) {
      const seeded = [DEFAULT_ADMIN_ACCOUNT];
      localStorage.setItem(USERS_DB_KEY, JSON.stringify(seeded));
      return seeded;
    }
    const parsed = JSON.parse(raw);
    if (!Array.isArray(parsed) || parsed.length === 0) {
      const seeded = [DEFAULT_ADMIN_ACCOUNT];
      localStorage.setItem(USERS_DB_KEY, JSON.stringify(seeded));
      return seeded;
    }
    return parsed as AppUser[];
  } catch {
    const seeded = [DEFAULT_ADMIN_ACCOUNT];
    try {
      localStorage.setItem(USERS_DB_KEY, JSON.stringify(seeded));
    } catch {
      // Ignore storage error
    }
    return seeded;
  }
}

function writeUsersDb(users: AppUser[]) {
  localStorage.setItem(USERS_DB_KEY, JSON.stringify(users));
}

function formatSocialHref(handleOrUrl: string): string {
  const trimmed = handleOrUrl.trim();
  if (!trimmed) return '';
  if (/^https?:\/\//i.test(trimmed)) return trimmed;
  const clean = trimmed.replace(/^@/, '');
  return `https://www.tiktok.com/@${clean}`;
}

function Y3KModalFrame({
  onClose,
  kicker,
  title,
  children,
}: {
  onClose: () => void;
  kicker: string;
  title: string;
  children: React.ReactNode;
}) {
  useEffect(() => {
    const onKey = (e: KeyboardEvent) => {
      if (e.key === 'Escape') onClose();
    };
    window.addEventListener('keydown', onKey);
    return () => window.removeEventListener('keydown', onKey);
  }, [onClose]);

  return (
    <div
      className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-[#040917]/80 backdrop-blur-sm"
      role="dialog"
      aria-modal="true"
      onClick={onClose}
    >
      <div
        className="relative w-full max-w-xl max-h-[90vh] overflow-y-auto y2k-navy-card border border-[#93C5FD]/40 rounded-2xl p-6 md:p-8 shadow-2xl"
        onClick={(e) => e.stopPropagation()}
      >
        <div className="flex items-start justify-between gap-4 pb-4 border-b border-[#93C5FD]/20">
          <div>
            <div className="font-mono text-[11px] font-semibold tracking-[0.14em] text-[#93C5FD]">
              {kicker}
            </div>
            <h3 className="font-display text-xl md:text-2xl font-extrabold text-white mt-1">
              {title}
            </h3>
          </div>
          <button
            type="button"
            onClick={onClose}
            aria-label="Close modal"
            className="w-10 h-10 inline-flex items-center justify-center border border-[#93C5FD]/30 bg-[#060E24] hover:bg-[#172F6B] text-[#CBD5E1] hover:text-white rounded-xl cursor-pointer shrink-0"
          >
            <X className="w-4 h-4" />
          </button>
        </div>
        <div className="mt-5">{children}</div>
      </div>
    </div>
  );
}

export function AuthModal({
  onAuthSuccess,
  onClose,
}: {
  onAuthSuccess: (session: ActiveSession) => void;
  onClose: () => void;
}) {
  const [tab, setTab] = useState<'login' | 'signup'>('login');

  // Login form state
  const [loginIdentifier, setLoginIdentifier] = useState('');
  const [loginPassword, setLoginPassword] = useState('');
  const [rememberMe, setRememberMe] = useState(true);
  const [showLoginPw, setShowLoginPw] = useState(false);

  // Sign-Up form state
  const [username, setUsername] = useState('');
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [confirmPassword, setConfirmPassword] = useState('');
  const [showSignupPw, setShowSignupPw] = useState(false);
  const [showConfirmPw, setShowConfirmPw] = useState(false);

  const [status, setStatus] = useState<'idle' | 'loading' | 'success' | 'error'>('idle');
  const [feedback, setFeedback] = useState('');

  useEffect(() => {
    ensureSeededUsersDb();
  }, []);

  const handleLogin = (e: React.FormEvent) => {
    e.preventDefault();
    setFeedback('');

    const idInput = loginIdentifier.trim().toLowerCase();
    if (!idInput || !loginPassword) {
      setStatus('error');
      setFeedback('Enter your email (or username) and password.');
      return;
    }

    setStatus('loading');
    const currentDb = ensureSeededUsersDb();
    const matched = currentDb.find(
      (u) => u.email.toLowerCase() === idInput || u.username.toLowerCase() === idInput
    );

    if (!matched) {
      setStatus('error');
      setFeedback('No account found with that email or username.');
      return;
    }

    if (matched.password !== loginPassword) {
      setStatus('error');
      setFeedback('Incorrect password. Please try again.');
      return;
    }

    const session: ActiveSession = {
      userId: matched.id,
      username: matched.username,
      email: matched.email,
      role: matched.role,
      rememberMe,
      loggedInAt: new Date().toISOString(),
    };

    localStorage.setItem(ACTIVE_SESSION_KEY, JSON.stringify(session));
    setStatus('success');
    setFeedback(`Welcome back, ${matched.username}!`);

    setTimeout(() => {
      onAuthSuccess(session);
      onClose();
    }, 350);
  };

  const handleSignUp = (e: React.FormEvent) => {
    e.preventDefault();
    setFeedback('');

    const cleanUsername = username.trim();
    const cleanEmail = email.trim().toLowerCase();

    if (!cleanUsername || cleanUsername.length < 2) {
      setStatus('error');
      setFeedback('Please enter a Username (at least 2 characters).');
      return;
    }
    if (!cleanEmail || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(cleanEmail)) {
      setStatus('error');
      setFeedback('Please enter a valid email address.');
      return;
    }
    if (password.length < 6) {
      setStatus('error');
      setFeedback('Password must be at least 6 characters long.');
      return;
    }
    if (password !== confirmPassword) {
      setStatus('error');
      setFeedback('Passwords do not match.');
      return;
    }

    setStatus('loading');
    const currentDb = ensureSeededUsersDb();

    if (currentDb.some((u) => u.email.toLowerCase() === cleanEmail)) {
      setStatus('error');
      setFeedback('An account with this email already exists. Please log in instead.');
      return;
    }
    if (currentDb.some((u) => u.username.toLowerCase() === cleanUsername.toLowerCase())) {
      setStatus('error');
      setFeedback('That Username is already taken.');
      return;
    }

    const newUser: AppUser = {
      id: `usr_${Date.now()}`,
      username: cleanUsername,
      email: cleanEmail,
      password,
      role: 'member',
      createdAt: new Date().toISOString(),
    };

    const nextDb = [...currentDb, newUser];
    writeUsersDb(nextDb);

    const session: ActiveSession = {
      userId: newUser.id,
      username: newUser.username,
      email: newUser.email,
      role: 'member',
      rememberMe: true,
      loggedInAt: new Date().toISOString(),
    };
    localStorage.setItem(ACTIVE_SESSION_KEY, JSON.stringify(session));

    setStatus('success');
    setFeedback(`Account created! Signed in as ${newUser.username}.`);

    setTimeout(() => {
      onAuthSuccess(session);
      onClose();
    }, 400);
  };

  const handleAutoFillAdmin = () => {
    ensureSeededUsersDb();
    setTab('login');
    setLoginIdentifier(DEFAULT_ADMIN_ACCOUNT.email);
    setLoginPassword(DEFAULT_ADMIN_ACCOUNT.password);
    setStatus('idle');
    setFeedback('');
  };

  const handleResetAccounts = () => {
    localStorage.removeItem(USERS_DB_KEY);
    localStorage.removeItem(ACTIVE_SESSION_KEY);
    ensureSeededUsersDb();
    setTab('login');
    setLoginIdentifier('');
    setLoginPassword('');
    setStatus('idle');
    setFeedback('Saved accounts reset.');
  };

  const isAiStudioEnv =
    import.meta.env.DEV ||
    (typeof window !== 'undefined' &&
      (window.location.hostname === 'localhost' ||
        window.location.hostname === '127.0.0.1' ||
        window.location.hostname.endsWith('.run.app')));

  return (
    <Y3KModalFrame
      onClose={onClose}
      kicker="PARALLEL PARADOX · ACCOUNT ACCESS"
      title={tab === 'login' ? 'Log In to Account' : 'Create Member Account'}
    >
      {/* Smooth Tab Switcher */}
      <div className="flex gap-2 p-1 bg-[#060E24] border border-[#93C5FD]/25 rounded-xl mb-5 font-mono text-xs">
        <button
          type="button"
          onClick={() => {
            setTab('login');
            setStatus('idle');
            setFeedback('');
          }}
          className={`flex-1 py-2 rounded-lg font-semibold cursor-pointer transition-all ${
            tab === 'login' ? 'y2k-aero-btn text-[#060E24]' : 'text-[#CBD5E1] hover:text-white'
          }`}
        >
          LOG IN
        </button>
        <button
          type="button"
          onClick={() => {
            setTab('signup');
            setStatus('idle');
            setFeedback('');
          }}
          className={`flex-1 py-2 rounded-lg font-semibold cursor-pointer transition-all ${
            tab === 'signup' ? 'y2k-aero-btn text-[#060E24]' : 'text-[#CBD5E1] hover:text-white'
          }`}
        >
          SIGN UP
        </button>
      </div>

      <AnimatePresence mode="wait">
        {tab === 'login' ? (
          <motion.form
            key="login-form"
            initial={{ opacity: 0, x: -8 }}
            animate={{ opacity: 1, x: 0 }}
            exit={{ opacity: 0, x: 8 }}
            transition={{ duration: 0.15 }}
            onSubmit={handleLogin}
            className="space-y-4 font-mono text-xs"
            noValidate
          >
            <div>
              <label htmlFor="login-id" className="block text-[11px] text-[#94A3B8] mb-1.5">
                EMAIL OR USERNAME
              </label>
              <input
                id="login-id"
                type="text"
                value={loginIdentifier}
                onChange={(e) => setLoginIdentifier(e.target.value)}
                placeholder="name@domain.com or PRX_KAZ"
                className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3.5 py-2.5 text-white rounded-xl"
              />
            </div>

            <div>
              <label htmlFor="login-pw" className="block text-[11px] text-[#94A3B8] mb-1.5">
                PASSWORD
              </label>
              <div className="relative">
                <input
                  id="login-pw"
                  type={showLoginPw ? 'text' : 'password'}
                  value={loginPassword}
                  onChange={(e) => setLoginPassword(e.target.value)}
                  placeholder="••••••••"
                  className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none pl-3.5 pr-10 py-2.5 text-white rounded-xl"
                />
                <button
                  type="button"
                  onClick={() => setShowLoginPw((v) => !v)}
                  aria-label={showLoginPw ? 'Hide password' : 'Show password'}
                  className="absolute right-2.5 top-1/2 -translate-y-1/2 text-[#94A3B8] hover:text-white p-1 cursor-pointer"
                >
                  {showLoginPw ? <EyeOff className="w-4 h-4" /> : <Eye className="w-4 h-4" />}
                </button>
              </div>
            </div>

            <label className="flex items-center gap-2.5 cursor-pointer select-none text-[#CBD5E1]">
              <input
                type="checkbox"
                checked={rememberMe}
                onChange={(e) => setRememberMe(e.target.checked)}
                className="w-4 h-4 accent-[#38BDF8] rounded cursor-pointer"
              />
              <span>Keep session logged in across reloads</span>
            </label>

            {feedback && (
              <div
                className={`p-3 border rounded-xl flex items-center gap-2 ${
                  status === 'success'
                    ? 'border-[#60A5FA] bg-[#0F214D] text-[#BAE6FD]'
                    : 'border-amber-400/50 bg-amber-950/40 text-amber-200'
                }`}
              >
                {status === 'success' ? (
                  <Check className="w-4 h-4 text-[#7DD3FC] shrink-0" />
                ) : (
                  <ShieldAlert className="w-4 h-4 text-amber-400 shrink-0" />
                )}
                <span>{feedback}</span>
              </div>
            )}

            <button
              type="submit"
              disabled={status === 'loading' || status === 'success'}
              className="w-full py-3 y2k-aero-btn text-[#060E24] font-semibold tracking-[0.12em] rounded-xl disabled:opacity-50 cursor-pointer"
            >
              {status === 'loading' ? 'VERIFYING...' : 'LOG IN'}
            </button>
          </motion.form>
        ) : (
          <motion.form
            key="signup-form"
            initial={{ opacity: 0, x: 8 }}
            animate={{ opacity: 1, x: 0 }}
            exit={{ opacity: 0, x: -8 }}
            transition={{ duration: 0.15 }}
            onSubmit={handleSignUp}
            className="space-y-4 font-mono text-xs"
            noValidate
          >
            <div>
              <label htmlFor="signup-user" className="block text-[11px] text-[#94A3B8] mb-1.5">
                USERNAME *
              </label>
              <input
                id="signup-user"
                type="text"
                value={username}
                onChange={(e) => setUsername(e.target.value)}
                placeholder="e.g. PRX_KAZ"
                className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3.5 py-2.5 text-white rounded-xl"
              />
            </div>

            <div>
              <label htmlFor="signup-email" className="block text-[11px] text-[#94A3B8] mb-1.5">
                EMAIL ADDRESS *
              </label>
              <input
                id="signup-email"
                type="email"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
                placeholder="name@domain.com"
                className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3.5 py-2.5 text-white rounded-xl"
              />
            </div>

            <div className="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <div>
                <label htmlFor="signup-pw" className="block text-[11px] text-[#94A3B8] mb-1.5">
                  PASSWORD (MIN. 6) *
                </label>
                <div className="relative">
                  <input
                    id="signup-pw"
                    type={showSignupPw ? 'text' : 'password'}
                    value={password}
                    onChange={(e) => setPassword(e.target.value)}
                    placeholder="••••••"
                    className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none pl-3.5 pr-10 py-2.5 text-white rounded-xl"
                  />
                  <button
                    type="button"
                    onClick={() => setShowSignupPw((v) => !v)}
                    aria-label={showSignupPw ? 'Hide password' : 'Show password'}
                    className="absolute right-2.5 top-1/2 -translate-y-1/2 text-[#94A3B8] hover:text-white p-1 cursor-pointer"
                  >
                    {showSignupPw ? <EyeOff className="w-4 h-4" /> : <Eye className="w-4 h-4" />}
                  </button>
                </div>
              </div>

              <div>
                <label htmlFor="signup-confirm" className="block text-[11px] text-[#94A3B8] mb-1.5">
                  CONFIRM PASSWORD *
                </label>
                <div className="relative">
                  <input
                    id="signup-confirm"
                    type={showConfirmPw ? 'text' : 'password'}
                    value={confirmPassword}
                    onChange={(e) => setConfirmPassword(e.target.value)}
                    placeholder="••••••"
                    className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none pl-3.5 pr-10 py-2.5 text-white rounded-xl"
                  />
                  <button
                    type="button"
                    onClick={() => setShowConfirmPw((v) => !v)}
                    aria-label={showConfirmPw ? 'Hide confirm password' : 'Show confirm password'}
                    className="absolute right-2.5 top-1/2 -translate-y-1/2 text-[#94A3B8] hover:text-white p-1 cursor-pointer"
                  >
                    {showConfirmPw ? <EyeOff className="w-4 h-4" /> : <Eye className="w-4 h-4" />}
                  </button>
                </div>
              </div>
            </div>

            {feedback && (
              <div
                className={`p-3 border rounded-xl flex items-center gap-2 ${
                  status === 'success'
                    ? 'border-[#60A5FA] bg-[#0F214D] text-[#BAE6FD]'
                    : 'border-amber-400/50 bg-amber-950/40 text-amber-200'
                }`}
              >
                {status === 'success' ? (
                  <Check className="w-4 h-4 text-[#7DD3FC] shrink-0" />
                ) : (
                  <ShieldAlert className="w-4 h-4 text-amber-400 shrink-0" />
                )}
                <span>{feedback}</span>
              </div>
            )}

            <button
              type="submit"
              disabled={status === 'loading' || status === 'success'}
              className="w-full py-3 y2k-aero-btn text-[#060E24] font-semibold tracking-[0.12em] rounded-xl disabled:opacity-50 cursor-pointer"
            >
              {status === 'loading' ? 'CREATING ACCOUNT...' : 'CREATE ACCOUNT'}
            </button>
          </motion.form>
        )}
      </AnimatePresence>

      {/* Modal Footer Utility Helpers */}
      <div className="mt-5 pt-4 border-t border-[#93C5FD]/20 flex flex-wrap items-center justify-between gap-2 font-mono text-[11px]">
        {isAiStudioEnv ? (
          <button
            type="button"
            onClick={handleAutoFillAdmin}
            className="inline-flex items-center gap-1.5 text-[#7DD3FC] hover:text-white cursor-pointer"
          >
            <Sparkles className="w-3.5 h-3.5" />
            <span>[AUTO-FILL ADMIN DEMO]</span>
          </button>
        ) : (
          <span />
        )}

        <button
          type="button"
          onClick={handleResetAccounts}
          className="inline-flex items-center gap-1.5 text-[#94A3B8] hover:text-red-300 cursor-pointer"
        >
          <RotateCcw className="w-3 h-3" />
          <span>Reset Saved Accounts</span>
        </button>
      </div>
    </Y3KModalFrame>
  );
}

export function MemberProfileModal({
  member,
  editEnabled,
  onUpdateMember,
  onSwapImage,
  onClose,
}: {
  member: ClanMember;
  editEnabled: boolean;
  onUpdateMember: (id: string, patch: Partial<ClanMember>) => void;
  onSwapImage: (member: ClanMember) => void;
  onClose: () => void;
}) {
  const [copiedUid, setCopiedUid] = useState(false);

  const handleCopyUid = async () => {
    try {
      await navigator.clipboard.writeText(member.uid);
    } catch {
      // Clipboard fallback ignored
    }
    setCopiedUid(true);
    setTimeout(() => setCopiedUid(false), 1800);
  };

  return (
    <Y3KModalFrame
      onClose={onClose}
      kicker={`MEMBER NO. ${member.number} · ${member.division || member.region}`}
      title={`${member.ign} (${member.realName})`}
    >
      <div className="grid grid-cols-1 sm:grid-cols-12 gap-6">
        <div className="sm:col-span-5 space-y-3">
          <div
            onClick={() => {
              if (editEnabled) onSwapImage(member);
            }}
            title={editEnabled ? 'Click photo to swap image' : undefined}
            className={`aspect-[3/4] rounded-xl overflow-hidden border border-[#93C5FD]/30 bg-[#0F214D] ${
              editEnabled
                ? 'outline-1 outline-dashed outline-[#7DD3FC]/70 hover:outline-[#BAE6FD] cursor-pointer'
                : ''
            }`}
          >
            <img
              src={member.portraitUrl}
              alt={member.ign}
              referrerPolicy="no-referrer"
              className="w-full h-full object-cover object-top"
            />
          </div>

          {member.uid && (
            <button
              type="button"
              onClick={handleCopyUid}
              className="w-full flex items-center justify-between px-3 py-2 bg-[#060E24] border border-[#93C5FD]/30 hover:border-[#60A5FA] rounded-xl font-mono text-[11px] text-[#E2E8F0] cursor-pointer"
            >
              <span className="truncate">UID: {member.uid}</span>
              {copiedUid ? (
                <Check className="w-3.5 h-3.5 text-[#93C5FD] shrink-0" />
              ) : (
                <Copy className="w-3.5 h-3.5 text-[#94A3B8] shrink-0" />
              )}
            </button>
          )}
        </div>

        <div className="sm:col-span-7 flex flex-col justify-between gap-4">
          <div className="space-y-4">
            <InlineEditableText
              value={member.bio}
              onSave={(next) => onUpdateMember(member.id, { bio: next })}
              editEnabled={editEnabled}
              as="p"
              multiline
              className="text-sm text-[#CBD5E1] leading-relaxed"
            />

            <div className="bg-[#060E24]/80 border border-[#93C5FD]/25 rounded-xl p-4 space-y-2.5 font-mono text-xs">
              <div className="flex justify-between gap-4">
                <span className="text-[#94A3B8]">IGN</span>
                <InlineEditableText
                  value={member.ign}
                  onSave={(next) => onUpdateMember(member.id, { ign: next })}
                  editEnabled={editEnabled}
                  as="span"
                  className="font-semibold text-white text-right"
                />
              </div>
              <div className="flex justify-between gap-4">
                <span className="text-[#94A3B8]">ROLE</span>
                <InlineEditableText
                  value={member.role}
                  onSave={(next) => onUpdateMember(member.id, { role: next })}
                  editEnabled={editEnabled}
                  as="span"
                  className="font-semibold text-[#93C5FD] text-right"
                />
              </div>
              {(member.division || editEnabled) && (
                <div className="flex justify-between gap-4">
                  <span className="text-[#94A3B8]">DIVISION</span>
                  <InlineEditableText
                    value={member.division || 'CODM Multiplayer'}
                    onSave={(next) => onUpdateMember(member.id, { division: next })}
                    editEnabled={editEnabled}
                    as="span"
                    className="text-white text-right"
                  />
                </div>
              )}
              <div className="flex justify-between gap-4">
                <span className="text-[#94A3B8]">REGION</span>
                <InlineEditableText
                  value={member.region}
                  onSave={(next) => onUpdateMember(member.id, { region: next })}
                  editEnabled={editEnabled}
                  as="span"
                  className="text-white text-right"
                />
              </div>
            </div>

            {member.tiktok && (
              <div className="flex flex-wrap gap-4 font-mono text-xs">
                <a
                  href={formatSocialHref(member.tiktok)}
                  target="_blank"
                  rel="noreferrer"
                  className="inline-flex items-center gap-1 text-[#93C5FD] hover:text-white"
                >
                  <span>SOCIAL</span>
                  <ExternalLink className="w-3 h-3" />
                </a>
              </div>
            )}
          </div>
        </div>
      </div>
    </Y3KModalFrame>
  );
}

function compressImageFile(file: File): Promise<string> {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = () => {
      const img = new Image();
      img.onload = () => {
        const maxW = 640;
        const scale = Math.min(1, maxW / img.width);
        const canvas = document.createElement('canvas');
        canvas.width = Math.round(img.width * scale);
        canvas.height = Math.round(img.height * scale);
        const ctx = canvas.getContext('2d');
        if (!ctx) {
          resolve(String(reader.result));
          return;
        }
        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
        resolve(canvas.toDataURL('image/jpeg', 0.82));
      };
      img.onerror = reject;
      img.src = String(reader.result);
    };
    reader.onerror = reject;
    reader.readAsDataURL(file);
  });
}

export function MemberEditorModal({
  initialMember,
  nextNumber,
  onSave,
  onClose,
}: {
  initialMember: ClanMember | null;
  nextNumber: string;
  onSave: (member: ClanMember) => void;
  onClose: () => void;
}) {
  const isNew = !initialMember;
  const [form, setForm] = useState<ClanMember>(
    initialMember || {
      id: `member-${Date.now()}`,
      number: nextNumber,
      ign: '',
      realName: '',
      uid: '',
      role: 'Member',
      division: 'CODM Multiplayer',
      stats: '',
      region: 'GLOBAL',
      portraitUrl: PRESET_PORTRAITS[0].url,
      bio: '',
      tiktok: '',
      youtube: '',
      x: '',
    }
  );
  const [error, setError] = useState('');

  const handlePhotoUpload = async (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (!file) return;
    try {
      const dataUrl = await compressImageFile(file);
      setForm((prev) => ({ ...prev, portraitUrl: dataUrl }));
    } catch {
      setError('Could not read that image file. Try another JPG or PNG.');
    }
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (!form.ign.trim()) {
      setError('Please enter an IGN.');
      return;
    }
    onSave({
      ...form,
      ign: form.ign.trim(),
      realName: form.realName.trim() || 'Clan Member',
      role: form.role.trim() || 'Member',
      division: form.division?.trim() || '',
      portraitUrl: form.portraitUrl.trim() || PRESET_PORTRAITS[0].url,
    });
  };

  return (
    <Y3KModalFrame
      onClose={onClose}
      kicker="ADMIN ROSTER EDITOR"
      title={isNew ? 'Add New Player' : `Edit ${initialMember.ign}`}
    >
      <form onSubmit={handleSubmit} className="space-y-4 text-xs" noValidate>
        <div className="bg-[#060E24]/70 border border-[#93C5FD]/25 rounded-xl p-4 space-y-3">
          <div className="font-mono text-[11px] font-semibold text-[#93C5FD]">
            AVATAR IMAGE URL OR DEVICE UPLOAD
          </div>
          <div className="flex flex-col sm:flex-row items-start sm:items-center gap-4">
            <img
              src={form.portraitUrl || PRESET_PORTRAITS[0].url}
              alt="Preview"
              className="w-20 h-24 object-cover object-top rounded-xl border border-[#93C5FD]/40 shrink-0"
            />
            <div className="flex-1 space-y-2.5 w-full">
              <input
                type="url"
                value={form.portraitUrl}
                onChange={(e) => setForm((p) => ({ ...p, portraitUrl: e.target.value }))}
                placeholder="https://... or upload photo below"
                className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3 py-2 font-mono text-white rounded-xl"
              />

              <label className="inline-flex items-center gap-2 px-3.5 py-2 y2k-aero-btn text-[#060E24] font-mono font-semibold rounded-xl cursor-pointer">
                <Upload className="w-3.5 h-3.5" />
                <span>UPLOAD PHOTO</span>
                <input
                  type="file"
                  accept="image/*"
                  onChange={handlePhotoUpload}
                  className="hidden"
                />
              </label>
            </div>
          </div>
        </div>

        <div className="grid grid-cols-1 sm:grid-cols-3 gap-3 font-mono">
          <div>
            <label className="block text-[11px] text-[#94A3B8] mb-1">IGN *</label>
            <input
              type="text"
              value={form.ign}
              onChange={(e) => setForm((p) => ({ ...p, ign: e.target.value }))}
              placeholder="e.g. PRX·NOVA"
              className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3 py-2 text-white rounded-xl"
            />
          </div>
          <div>
            <label className="block text-[11px] text-[#94A3B8] mb-1">GAME DIVISION</label>
            <input
              type="text"
              value={form.division || ''}
              onChange={(e) => setForm((p) => ({ ...p, division: e.target.value }))}
              placeholder="e.g. CODM Multiplayer"
              className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3 py-2 text-white rounded-xl"
            />
          </div>
          <div>
            <label className="block text-[11px] text-[#94A3B8] mb-1">ROLE</label>
            <input
              type="text"
              value={form.role}
              onChange={(e) => setForm((p) => ({ ...p, role: e.target.value }))}
              placeholder="e.g. IGL / Entry / Co-Leader"
              className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3 py-2 text-white rounded-xl"
            />
          </div>
        </div>

        <div className="grid grid-cols-1 sm:grid-cols-2 gap-3 font-mono">
          <div>
            <label className="block text-[11px] text-[#94A3B8] mb-1">DISPLAY NAME</label>
            <input
              type="text"
              value={form.realName}
              onChange={(e) => setForm((p) => ({ ...p, realName: e.target.value }))}
              placeholder="e.g. Alex Kim"
              className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3 py-2 text-white rounded-xl"
            />
          </div>
          <div>
            <label className="block text-[11px] text-[#94A3B8] mb-1">SOCIAL HANDLE / URL</label>
            <input
              type="text"
              value={form.tiktok || ''}
              onChange={(e) => setForm((p) => ({ ...p, tiktok: e.target.value }))}
              placeholder="@handle or https://..."
              className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3 py-2 text-white rounded-xl"
            />
          </div>
        </div>

        <div className="grid grid-cols-1 sm:grid-cols-2 gap-3 font-mono">
          <div>
            <label className="block text-[11px] text-[#94A3B8] mb-1">CODM UID</label>
            <input
              type="text"
              value={form.uid}
              onChange={(e) => setForm((p) => ({ ...p, uid: e.target.value }))}
              placeholder="7194820..."
              className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3 py-2 text-white rounded-xl"
            />
          </div>
          <div>
            <label className="block text-[11px] text-[#94A3B8] mb-1">REGION</label>
            <input
              type="text"
              value={form.region}
              onChange={(e) => setForm((p) => ({ ...p, region: e.target.value }))}
              placeholder="NA / SEA / EU"
              className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3 py-2 text-white rounded-xl"
            />
          </div>
        </div>

        <div className="font-mono">
          <label className="block text-[11px] text-[#94A3B8] mb-1">SHORT BIO</label>
          <textarea
            rows={2}
            value={form.bio}
            onChange={(e) => setForm((p) => ({ ...p, bio: e.target.value }))}
            placeholder="Write a quick note about this clan member..."
            className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3 py-2 text-white rounded-xl"
          />
        </div>

        {error && (
          <div className="p-3 border border-amber-400/50 bg-amber-950/40 text-amber-200 rounded-xl flex items-center gap-2 font-mono">
            <ShieldAlert className="w-4 h-4 shrink-0" />
            <span>{error}</span>
          </div>
        )}

        <div className="flex justify-end gap-3 pt-2 font-mono">
          <button
            type="button"
            onClick={onClose}
            className="px-5 py-2.5 border border-[#93C5FD]/30 text-[#CBD5E1] hover:text-white rounded-xl cursor-pointer"
          >
            CANCEL
          </button>
          <button
            type="submit"
            className="px-6 py-2.5 y2k-aero-btn text-[#060E24] font-semibold rounded-xl cursor-pointer"
          >
            {isNew ? 'ADD PLAYER' : 'SAVE CHANGES'}
          </button>
        </div>
      </form>
    </Y3KModalFrame>
  );
}

export function SiteSettingsEditorModal({
  settings,
  onSave,
  onClose,
}: {
  settings: ClanSiteSettings;
  onSave: (next: ClanSiteSettings) => void;
  onClose: () => void;
}) {
  const [form, setForm] = useState<ClanSiteSettings>(settings);

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    onSave({
      ...form,
      clanName: form.clanName.trim() || 'PARALLEL PARADOX',
      rosterHeading: form.rosterHeading.trim() || 'Official Member Roster',
    });
  };

  return (
    <Y3KModalFrame
      onClose={onClose}
      kicker="ADMIN SITE SETTINGS"
      title="Edit Clan Title & Heading"
    >
      <form onSubmit={handleSubmit} className="space-y-4 font-mono text-xs">
        <div>
          <label className="block text-[11px] text-[#94A3B8] mb-1.5">CLAN NAME</label>
          <input
            type="text"
            value={form.clanName}
            onChange={(e) => setForm((p) => ({ ...p, clanName: e.target.value }))}
            className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3.5 py-2.5 text-white rounded-xl"
          />
        </div>
        <div>
          <label className="block text-[11px] text-[#94A3B8] mb-1.5">ROSTER SECTION HEADING</label>
          <input
            type="text"
            value={form.rosterHeading}
            onChange={(e) => setForm((p) => ({ ...p, rosterHeading: e.target.value }))}
            className="w-full bg-[#060E24] border border-[#93C5FD]/30 focus:border-[#60A5FA] focus:outline-none px-3.5 py-2.5 text-white rounded-xl"
          />
        </div>

        <div className="flex justify-end gap-3 pt-2">
          <button
            type="button"
            onClick={onClose}
            className="px-5 py-2.5 border border-[#93C5FD]/30 text-[#CBD5E1] hover:text-white rounded-xl cursor-pointer"
          >
            CANCEL
          </button>
          <button
            type="submit"
            className="px-6 py-2.5 y2k-aero-btn text-[#060E24] font-semibold rounded-xl cursor-pointer"
          >
            SAVE HEADINGS
          </button>
        </div>
      </form>
    </Y3KModalFrame>
  );
}
```

---

## 6. Running & Building

```bash
# Install dependencies
npm install

# Start development server on port 3000 (serves /roster)
npm run dev

# Type-check & build for production
npm run lint
npm run build
npm start
```
