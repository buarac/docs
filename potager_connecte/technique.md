# 🎯 Contexte générale
Application fonctionne uniquement dans le réseau local de la maison. 
L'application sera deployée sur un nuc avec ubuntu.


# 🧱 Stack & outils (versions stables récentes)
- **Next.js 15**, **React 18**, **TypeScript**
- **App Router** (`app/`), **Server Components** + **Client Components**
- **Tailwind CSS** (mobile‑first) + `prettier-plugin-tailwindcss`
- **shadcn/ui** (Radix UI, CLI) — composants accessibles et personnalisables
- **Prisma ORM** + **PostgreSQL**
- **Auth** : **NextAuth.js** (Credentials + OAuth Google/Apple activables via ENV)
- **Validation** : **Zod**
- **Tests** : **Vitest** (unit/intégration) + **Playwright** (E2E)
- **Qualité** : ESLint (`next/core-web-vitals`), Prettier, **Husky** + **lint-staged**
- **Conventional Commits** + **commitlint**
- **CI/CD** : GitHub Actions + **Vercel** (preview sur PR + prod sur `main`)
- **Observabilité** : **Sentry** (front + serveur), **Plausible** (analytics sans cookie)
- **PWA** : manifest, SW, offline de base
- **Sécurité** : en‑têtes (CSP stricte, Referrer‑Policy, Permissions‑Policy), CSRF, XSS
- **Accessibilité** : ARIA, contrastes AA, audit axe

## 📁 Structure du dépôt (attendue)
```
/src
  /app
    /(marketing)/landing/page.tsx
    /(auth)/signin/page.tsx
    /(auth)/signup/page.tsx
    /(app)/layout.tsx
    /(app)/dashboard/page.tsx
    /(app)/interventions/page.tsx
    /(app)/interventions/new/page.tsx
    /(app)/interventions/[id]/page.tsx
    /(app)/taches/page.tsx
    /(app)/fournisseurs/page.tsx
    /(app)/factures/page.tsx
    /(app)/settings/profile/page.tsx
    /(app)/settings/household/page.tsx
    api/health/route.ts
  /components
    /ui            # composants shadcn/ui générés
    /forms
    /charts
    /layout
  /lib
    auth.ts
    db.ts
    logger.ts
    csp.ts
    zod-schemas.ts
    utils.ts
  /styles
    globals.css
/prisma
  schema.prisma
/tests
  /unit
  /e2e
/public
  /icons   # PWA
  /images
.github/workflows
README.md
```

> **Alias d’imports** : obligatoire `@/*` (ex. `@/components/...`, `@/lib/...`).  
> **Processus composant (3 étapes)** souhaité :  
> 1) HTML + Tailwind brut (prévisualisable), 2) composant React avec **props** typées, 3) états (loading/disabled), **accessibilité**, **tests unitaires**.

---

## 🔐 Authentification & rôles
- **NextAuth.js** : providers `Credentials`, `Google`, `Apple` (activables via ENV).  
- Cookies de session sécurisés (`Secure`, `HttpOnly`, `SameSite=Lax`).  
- **Middleware** qui protège toutes les routes `/app` et redirige vers `/signin`.  
- Autorisation basée sur `HouseholdRole` :  
  - **OWNER** : gestion complète,  
  - **EDITOR** : gestion interventions/tâches,  
  - **VIEWER** : lecture seule.  
- Pages : `/signin`, `/signup`, `/settings/profile`.

---

## 🧭 Navigation & état
- Layouts imbriqués avec `app/` (topbar).  
- Breadcrumbs, barre de recherche (Server Action), pagination.  
- **État** : colocation + petits hooks (`usePagination`, `useFilters`). Pas de grosse lib globale par défaut.

---

## 🛡️ Sécurité
- En‑têtes **strictes** via `next.config.mjs` :  
  - `Content-Security-Policy` (self, plausible, sentry, vercel‑insights),  
  - `Referrer-Policy: no-referrer`,  
  - `Permissions-Policy` minimale,  
  - `X-Content-Type-Options: nosniff`.  
- **Validation Zod** sur toutes les actions serveur.  
- **Rate limiting** basique sur login.  
- **CSRF** : protèger les mutations via session/cookies.

---

## 📱 PWA
- `public/manifest.json` (nom, icônes 512/192/180, couleurs).  
- Service Worker : cache statique minimal (public) + stratégie “NetworkFirst” pour `/app` (sans exposer de données sensibles).  
- Icônes **PNG‑24** fond transparent dans `/public/icons` ; meta iOS.

---

## 🔭 Observabilité & Analytics
- **Sentry** : instrumentation front + server (dsn via ENV, sampling raisonnable).  
- **Plausible** : pageviews + events (opt‑in), script `defer`.

---

## 🧪 Tests
- **Vitest** : hooks/components + coverage.  
- **Playwright** : 1 scénario E2E (connexion → créer intervention → visible en liste).  
- **axe** (accessibility) sur quelques composants.

---

## ⚙️ CI/CD (GitHub Actions + Vercel)
- Workflow PR : **lint**, **type‑check**, **tests unitaires**, **tests E2E**, **build**.  
- Cache Next.js + npm.  
- **Preview** Vercel sur PR ; **prod** auto sur `main` (si vert).  
- Règles de protection de branche (`main`).

Exemple (à adapter) :
```yaml
name: Pull Request
on: [pull_request]
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run lint
  type-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run type-check
  unit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run test -- --coverage
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run build
```

```text
-> A PRENDRE EN COMPTE DANS LE PROMPT

Bons bonus à inclure dans le template
	•	CI/CD prêt (GitHub Actions) : lint, test, build, preview, release (semantic-release).
	•	Dependabot/Renovate config.
	•	.editorconfig, .nvmrc, .vscode (extensions recommandées).
	•	husky + lint-staged + commitlint (qualité de commits).
	•	Env example : .env.example.
	•	Sécurité : headers HTTP, CSP (starter), rate limiting pour l’API.
	•	Docs : GETTING_STARTED.md, DECISIONS.md, CONTRIBUTING.md.
	•	Jobs PM2 : pm2.config.js + scripts (tu en as l’usage).
	•	Théming tokens OKLCH prêt (ton sujet du moment), PWA on/off via flag.
```
---

## 🧹 Qualité & Git
- **Trunk‑Based** (branches courtes → PR → `main`).  
- **Conventional Commits** + **commitlint**.  
- **Husky** + **lint‑staged** : format + lint pré‑commit.  
- README vivant (checklists, captures ASCII).

---

## 🧪 Scripts npm (exemples attendus)
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint .",
    "format": "prettier --write .",
    "type-check": "tsc -b",
    "test": "vitest run",
    "test:ui": "vitest",
    "e2e": "playwright test",
    "db:push": "prisma db push",
    "db:migrate": "prisma migrate dev",
    "db:seed": "ts-node prisma/seed.ts"
  }
}
```

---

## 🔧 `.env.example` (attendu)
```
# App
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=changeme

# OAuth (optionnels)
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
APPLE_CLIENT_ID=
APPLE_CLIENT_SECRET=

# DB
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/appdb

# Observability
SENTRY_DSN=
PLAUSIBLE_DOMAIN=localhost
```

---

## 🔚 Livrables obligatoires
1) Code complet conforme à la structure ci‑dessus  
2) **README.md** exhaustif avec :
   - Installation pas à pas (PostgreSQL local, Prisma migrate, seed)
   - Scripts utiles et workflows Git
   - Schémas ASCII (dashboard, interventions, tâches)
   - Captures textuelles (ASCII) + explications UX
   - Sécurité (CSP, sessions, rate limit) et PWA (manifest + SW)
3) **.github/workflows** prêt à l’emploi  
4) **Tests** (unitaires + 1 E2E Playwright)  
5) **.env.example** rempli  
6) **Checklist** “Prêt pour prod” dans le README

---

## ✅ Critères d’acceptation
- Lancement local : `npm ci && npm run db:migrate && npm run db:seed && npm run dev`  
- CI verte : lint, type‑check, tests, build  
- Imports internes via `@/...`; composants UI via **shadcn/ui**

---

---

## 📝 Démarrage (attendu de l’agent)
- Initialise le repo avec **create-next-app** (TypeScript, Tailwind, App Router, alias `@/*`)  
- Installe Prisma + NextAuth + shadcn/ui (Button, Input, Label, Dialog, Table, Toast…)  
- Génère la base des pages listées  
- Écrit le README et les workflows

====================================================================================================
# ANNEXES TECHNIQUES 

## 1) Commandes d’amorçage
```bash
# 1. Créer le projet
npx create-next-app@latest app-menage \
  --typescript --tailwind --eslint --src-dir --app --import-alias "@/*"
cd app-menage

# 2. Dépendances
npm i next-auth @prisma/client zod
npm i -D prisma vitest @testing-library/react @testing-library/jest-dom \
  @testing-library/user-event playwright @playwright/test \
  eslint-plugin-unicorn eslint-plugin-import eslint-plugin-playwright \
  eslint-config-prettier eslint-plugin-prettier \
  prettier prettier-plugin-tailwindcss \
  husky lint-staged commitlint @commitlint/cli @commitlint/config-conventional \
  ts-node

# 3. Prisma init
npx prisma init

# 4. shadcn/ui init + composants de base
npx shadcn@latest init -d
npx shadcn@latest add button input label textarea dialog dropdown-menu select \
  table toast card badge checkbox skeleton tabs alert tooltip

# 5. Playwright
npx playwright install --with-deps
```

> Si une dépendance n’est pas dispo, **choisir une alternative** et documenter le choix dans le README.

---

## 2) Exemples de fichiers critiques

### 2.1 `next.config.mjs` (CSP & headers)
```js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: { serverActions: { bodySizeLimit: '2mb' } },
  async headers() {
    return [
      {
        source: "/(.*)",
        headers: [
          { key: "Referrer-Policy", value: "no-referrer" },
          { key: "X-Content-Type-Options", value: "nosniff" },
          { key: "Permissions-Policy", value: "geolocation=(), microphone=()" },
          {
            key: "Content-Security-Policy",
            value: [
              "default-src 'self';",
              "script-src 'self' 'unsafe-inline' 'unsafe-eval' https://plausible.io https://*.vercel-insights.com;",
              "style-src 'self' 'unsafe-inline';",
              "img-src 'self' data: blob:;",
              "connect-src 'self' https://plausible.io https://sentry.io https://*.ingest.sentry.io;",
              "font-src 'self' data:;",
              "object-src 'none';",
              "frame-ancestors 'none';",
            ].join(" ")
          }
        ]
      }
    ]
  }
}
export default nextConfig
```

### 2.2 `tsconfig.json` — alias `@/*`
```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": { "@/*": ["./src/*"] }
  }
}
```

### 2.3 `src/lib/db.ts` — Prisma client
```ts
import { PrismaClient } from "@prisma/client";

declare global {
  // eslint-disable-next-line no-var
  var prisma: PrismaClient | undefined;
}

export const prisma =
  global.prisma ??
  new PrismaClient({
    log: process.env.NODE_ENV === "development" ? ["query", "error", "warn"] : ["error"],
  });

if (process.env.NODE_ENV !== "production") global.prisma = prisma;
```

### 2.4 `src/lib/zod-schemas.ts`
```ts
import { z } from "zod";

export const interventionCreateSchema = z.object({
  householdId: z.string().cuid(),
  date: z.string().datetime(),
  durationMin: z.number().int().positive().max(8 * 60),
  providerId: z.string().cuid().optional(),
  assignedToId: z.string().cuid().optional(),
  notes: z.string().max(1000).optional(),
  tasks: z.array(z.object({ taskId: z.string().cuid(), durationMin: z.number().int().optional() })).optional()
});
```

### 2.5 `src/middleware.ts` — protéger `/app`
```ts
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";
import { getToken } from "next-auth/jwt";

export async function middleware(req: NextRequest) {
  if (req.nextUrl.pathname.startsWith("/app")) {
    const token = await getToken({ req, secret: process.env.NEXTAUTH_SECRET });
    if (!token) {
      const url = new URL("/signin", req.url);
      url.searchParams.set("next", req.nextUrl.pathname);
      return NextResponse.redirect(url);
    }
  }
  return NextResponse.next();
}

export const config = { matcher: ["/app/:path*"] };
```

### 2.6 Server Action — création d’intervention
```ts
"use server";

import { prisma } from "@/lib/db";
import { interventionCreateSchema } from "@/lib/zod-schemas";
import { revalidatePath } from "next/cache";

export async function createIntervention(formData: FormData) {
  const parsed = interventionCreateSchema.safeParse({
    householdId: formData.get("householdId"),
    date: formData.get("date"),
    durationMin: Number(formData.get("durationMin")),
    providerId: formData.get("providerId") || undefined,
    assignedToId: formData.get("assignedToId") || undefined,
    notes: formData.get("notes") || undefined,
  });
  if (!parsed.success) {
    return { ok: false, errors: parsed.error.flatten() };
  }
  await prisma.intervention.create({ data: parsed.data });
  revalidatePath("/app/interventions");
  return { ok: true };
}
```

### 2.7 Page form — `src/app/(app)/interventions/new/page.tsx`
```tsx
import { createIntervention } from "@/lib/actions/interventions";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";

export default function NewInterventionPage() {
  return (
    <form action={createIntervention} className="space-y-4 max-w-md">
      <div>
        <Label htmlFor="date">Date & heure</Label>
        <Input id="date" name="date" type="datetime-local" required />
      </div>
      <div>
        <Label htmlFor="durationMin">Durée (min)</Label>
        <Input id="durationMin" name="durationMin" type="number" min={15} step={15} required />
      </div>
      <Button type="submit">Créer</Button>
    </form>
  );
}
```

---

## 3) API (App Router `/src/app/api/...`)

| Route | Méthode | Entrées | Sorties | Notes |
|---|---|---|---|---|
| `/api/health` | GET | — | `{status:"ok"}` | Monitoring |
| `/api/interventions` | GET | `householdId`, `q`, `status`, `page` | liste paginée | Requêtes serveur |
| `/api/interventions` | POST | `InterventionCreate` | `Intervention` | Validation Zod |
| `/api/interventions/[id]` | GET | `id` | `Intervention` |  |
| `/api/interventions/[id]` | PATCH | champs partiels | `Intervention` |  |
| `/api/interventions/[id]` | DELETE | `id` | `{ok:true}` |  |

> Favoriser **Server Actions** pour mutations simples. API routes si accès externe requis.

---

## 4) UI — Maquettes ASCII minimales

### 4.1 Dashboard
```
+─────────────────────────────────────────────────────────────+
|  Prochaines interventions   |   Statistiques (mois)         |
|  - Mar 12, 09:00 (2h)      |   - Total heures : 12h        |
|  - Ven 14, 14:00 (1h30)    |   - Tâches done : 18          |
+─────────────────────────────────────────────────────────────+
|  Tâches récurrentes                                        |
|  [ ] Nettoyer salle de bain (60m)      [Marquer fait]      |
|  [ ] Changer draps (30m)               [Planifier]         |
+─────────────────────────────────────────────────────────────+
```

### 4.2 Liste interventions
```
[ Filtre:  (Tous | Planifiés | Terminés | Annulés)   Rech: _______ ]
Date         Durée   Prestataire     Statut    Actions
2025-08-12   120m    Sophie Nettoy   PLAN      Voir  Édit  ✓Done
```

---

## 5) Accessibilité & i18n
- Langue par défaut : `fr-FR`; formatage dates/heures en **Europe/Paris**.  
- Cibles tactiles ≥ 44x44px.  
- Contraste AA minimal ; focus visible ; labels associés aux inputs.  
- Annonces ARIA (toast “Intervention créée”).

---

## 6) PWA — fichiers attendus
- `public/manifest.json` (nom, short_name, theme_color, icons 512/192/180).  
- `public/icons/*` (PNG-24, fond transparent).  
- `app/_offline` (page offline).  
- Service Worker minimal (Workbox facultatif).

---

## 7) CI/CD — Exemple PR workflow complet
```yaml
name: PR
on: [pull_request]
jobs:
  setup:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: "npm" }
      - run: npm ci
  lint:
    needs: setup
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run lint
  typecheck:
    needs: setup
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run type-check
  unit:
    needs: setup
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run test -- --coverage
  build:
    needs: [lint, typecheck, unit]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run build
```

---

## 8) Tests — exemples

### 8.1 Vitest (hook)
```ts
import { renderHook, act } from "@testing-library/react";
import { useState } from "react";

function useCounter() {
  const [n, setN] = useState(0);
  return { n, inc: () => setN((x) => x + 1) };
}

it("increments", () => {
  const { result } = renderHook(() => useCounter());
  act(() => result.current.inc());
  expect(result.current.n).toBe(1);
});
```

### 8.2 Playwright (E2E minimal)
```ts
import { test, expect } from "@playwright/test";

test("login and create intervention", async ({ page }) => {
  await page.goto("/signin");
  await page.getByLabel("Email").fill("demo@example.com");
  await page.getByLabel("Mot de passe").fill("demo1234");
  await page.getByRole("button", { name: "Se connecter" }).click();
  await expect(page).toHaveURL(/.*\/app\/dashboard/);
  await page.goto("/app/interventions/new");
  await page.getByLabel(/Date/).fill("2025-08-15T09:00");
  await page.getByLabel(/Durée/).fill("60");
  await page.getByRole("button", { name: "Créer" }).click();
  await expect(page.locator("table")).toContainText("2025-08-15");
});
```

---

## 9) README — sections exigées
- Installation (PostgreSQL local, `prisma migrate`, `seed`).  
- Variables d’environnement & sécurité (CSP, cookies).  
- Scripts npm & workflows.  
- Aperçus ASCII des écrans.  
- Roadmap / TODO.  
- Checklists “Production ready”.

---

## 10) Critères UX supplémentaires
- Temps de chargement perçu < 2s (skeletons).  
- Focus visible et navigation clavier OK.  
- Dark mode opérationnel.  
- PWA installable sur iPhone (icône 180x180).  
- Aucune erreur console en production.

# FIN DU PROMPT
