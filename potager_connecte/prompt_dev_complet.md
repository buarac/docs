# PROMPT DÉVELOPPEMENT - APPLICATION POTAGER CONNECTÉ

## 🎯 CONTEXTE GÉNÉRAL

Développer une application web fullstack de **potager connecté** pour un couple de jardiniers expérimentés. L'application fonctionne **uniquement en réseau local** sur un NUC Ubuntu et suit les 4 piliers : **Cultiver, Planter, Récolter, Analyser**.

**Vision UX :** "Jardin Digital Apaisant" - Interface qui respire comme un jardin avec simplicité, convivialité, efficacité et douceur.

---

## 🧱 STACK TECHNIQUE IMPOSÉE

### Framework & Core
- **Next.js 15** + **React 18** + **TypeScript**
- **App Router** (`app/`) avec Server Components + Client Components
- **Tailwind CSS** (mobile-first) + `prettier-plugin-tailwindcss`
- **shadcn/ui** (Radix UI, CLI) pour composants accessibles

### Base de données & Auth
- **Prisma ORM** + **PostgreSQL**
- **NextAuth.js** (Credentials + OAuth Google/Apple via ENV)
- **Zod** pour validation

### Qualité & Tests
- **ESLint** (`next/core-web-vitals`), **Prettier**, **Husky** + **lint-staged**
- **Vitest** (unit/intégration) + **Playwright** (E2E)
- **Conventional Commits** + **commitlint**

### DevOps & Observabilité
- **GitHub Actions** (CI/CD)
- **Sentry** (erreurs) + **Plausible** (analytics sans cookies)
- **PWA** (manifest, SW, offline)

---

## 🌱 MODÈLE DE DONNÉES JARDINAGE

### Entités principales

#### 1. **Culture** (entité centrale)
```prisma
model Culture {
  id               String    @id @default(cuid())
  name             String    // "Tomate Coeur de Boeuf"
  variety          String?   // "Coeur de Boeuf"
  species          String    // "Tomate"
  category         CultureCategory // LEGUME, FRUIT, HERBE, FLEUR
  
  // Cycle de vie
  lifecycle        CultureLifecycle[] // Semis, repiquages, plantation, récoltes
  currentStage     CultureStage       // SEED, SEEDLING, PLANTED, MATURE, HARVESTED
  
  // Localisation
  location         Location?
  
  // Métadonnées
  plantingDate     DateTime?
  harvestDate      DateTime?
  expectedHarvest  DateTime?
  
  // Relations
  seeds            SeedStock?
  harvests         Harvest[]
  tasks            Task[]
  photos           Photo[]
  notes            Note[]
  
  createdAt        DateTime @default(now())
  updatedAt        DateTime @updatedAt
}
```

#### 2. **Localisation spatiale**
```prisma
model Location {
  id          String @id @default(cuid())
  type        LocationType // GARAGE, GARDEN_SQUARE
  name        String       // "Garage", "Carré 1", "Carré 2"...
  
  // Spécifique carrés potager
  squareNumber Int?         // 1, 2, 3, 4
  sunExposure  SunExposure? // FULL_SUN, PARTIAL_SUN, SHADE
  dimensions   Json?        // {length: 8, width: 0.8} en mètres
  
  // Spécifique garage
  temperature  Float?
  humidity     Float?
  
  cultures     Culture[]
}
```

#### 3. **Stock de graines**
```prisma
model SeedStock {
  id             String    @id @default(cuid())
  species        String    // "Tomate"
  variety        String    // "Coeur de Boeuf"
  brand          String?
  purchaseDate   DateTime?
  expirationDate DateTime?
  quantity       Int
  used           Int       @default(0)
  
  cultures       Culture[]
  
  createdAt      DateTime @default(now())
  updatedAt      DateTime @updatedAt
}
```

#### 4. **Récoltes**
```prisma
model Harvest {
  id          String   @id @default(cuid())
  cultureId   String
  culture     Culture  @relation(fields: [cultureId], references: [id])
  
  date        DateTime
  quantity    Float    // en grammes
  quality     HarvestQuality? // EXCELLENT, GOOD, AVERAGE, POOR
  notes       String?
  
  // Métadonnées
  weather     Json?    // conditions météo du jour
  userId      String   // qui a récolté
  
  photos      Photo[]
  
  createdAt   DateTime @default(now())
}
```

#### 5. **Tâches jardinage**
```prisma
model Task {
  id            String     @id @default(cuid())
  title         String
  description   String?
  type          TaskType   // SOWING, TRANSPLANT, WATERING, HARVEST, MAINTENANCE
  
  // Planning
  dueDate       DateTime?
  completedAt   DateTime?
  estimatedDuration Int?   // en minutes
  
  // Relations
  cultureId     String?
  culture       Culture?   @relation(fields: [cultureId], references: [id])
  assignedToId  String?    // utilisateur assigné
  
  // Contexte
  priority      Priority   @default(MEDIUM)
  weather       Json?      // conditions recommandées
  
  createdAt     DateTime   @default(now())
  updatedAt     DateTime   @updatedAt
}
```

#### 6. **Météo & Conditions**
```prisma
model WeatherData {
  id           String   @id @default(cuid())
  date         DateTime @unique
  
  temperature  Float
  humidity     Float
  rainfall     Float    // en mm
  sunHours     Float
  windSpeed    Float
  
  // Données externes (API météo)
  source       String   @default("manual")
  
  createdAt    DateTime @default(now())
}
```

### Énumérations
```prisma
enum CultureCategory {
  LEGUME
  FRUIT
  HERBE_AROMATIQUE
  FLEUR
}

enum CultureStage {
  SEED          // En graine/semis
  SEEDLING      // Plantule en garage
  PLANTED       // Planté au potager
  GROWING       // En croissance
  MATURE        // Arrivé à maturité
  HARVESTING    // En récolte
  FINISHED      // Cycle terminé
}

enum LocationType {
  GARAGE
  GARDEN_SQUARE
  GREENHOUSE
}

enum SunExposure {
  FULL_SUN      // 6+ heures
  PARTIAL_SUN   // 4-6 heures
  SHADE         // < 4 heures
}

enum TaskType {
  SOWING
  TRANSPLANT
  WATERING
  FERTILIZING
  PRUNING
  HARVEST
  MAINTENANCE
}

enum HarvestQuality {
  EXCELLENT
  GOOD
  AVERAGE
  POOR
}

enum Priority {
  LOW
  MEDIUM
  HIGH
  URGENT
}
```

---

## 🎨 DESIGN SYSTEM & UX

### Palette couleurs OKLCH
```css
:root {
  /* Couleurs principales - "Terre & Chlorophylle" */
  --sage: oklch(0.65 0.08 142);        /* Vert sauge - actions principales */
  --sienna: oklch(0.45 0.12 45);       /* Terre de Sienne - structure */
  
  /* Neutres organiques */
  --stone-50: oklch(0.98 0.01 45);
  --stone-100: oklch(0.95 0.02 45);
  --stone-200: oklch(0.90 0.02 45);
  --stone-800: oklch(0.30 0.02 45);
  --stone-900: oklch(0.25 0.02 45);
  
  /* Fonctionnels */
  --success: oklch(0.70 0.15 142);     /* Récolte réussie */
  --warning: oklch(0.75 0.15 60);      /* Attention/alerte */
  --info: oklch(0.60 0.12 200);        /* Météo/infos */
  --error: oklch(0.60 0.20 15);        /* Erreurs */
  
  /* Spécifiques jardinage */
  --seed: oklch(0.80 0.10 45);         /* Semis */
  --growth: oklch(0.70 0.12 142);      /* Croissance */
  --harvest: oklch(0.75 0.15 60);      /* Récolte */
}

/* Dark mode - "Nuit au Jardin" */
[data-theme="dark"] {
  --sage: oklch(0.55 0.08 142);
  --sienna: oklch(0.35 0.12 45);
  --bg: oklch(0.15 0.01 200);
  --surface: oklch(0.20 0.01 200);
}
```

### Animations organiques
- **Growth Animation** : Progression type croissance végétale
- **Seasonal Transitions** : Morphing fluide style time-lapse
- **Feedback Terroir** : Particules dorées (succès), ondulations (attention)

---

## 🗂️ STRUCTURE IMPOSÉE

```
/src
  /app
    /(marketing)
      /page.tsx                    # Landing page
    /(auth)
      /signin/page.tsx             # Connexion
      /signup/page.tsx             # Inscription
    /(app)
      /layout.tsx                  # Layout app avec nav "Garden Compass"
      /dashboard/page.tsx          # Vue jardin temps réel
      
      /cultiver                    # PILIER 1 - Semis/Garage
        /page.tsx                  # Dashboard semis
        /stock-graines/page.tsx    # Gestion stock graines
        /nouveau-semis/page.tsx    # Formulaire nouveau semis
        /[cultureId]/page.tsx      # Détail culture en semis
      
      /planter                     # PILIER 2 - Plantation
        /page.tsx                  # Planificateur spatial 4 carrés
        /nouveau/page.tsx          # Nouvelle plantation
        /carres/[squareId]/page.tsx # Vue détail carré
        /compatibilite/page.tsx    # Matrice compatibilité
      
      /recolter                    # PILIER 3 - Récoltes
        /page.tsx                  # Dashboard productivité
        /express/page.tsx          # Mode récolte rapide (photo)
        /historique/page.tsx       # Historique récoltes
        /[harvestId]/page.tsx      # Détail récolte
      
      /analyser                    # PILIER 4 - Analyses
        /page.tsx                  # Dashboard insights
        /meteo/page.tsx            # Corrélations météo
        /rendements/page.tsx       # Analyses rendements
        /predictions/page.tsx      # Prédictions IA
      
      /taches                      # Gestion tâches
        /page.tsx                  # Liste tâches
        /nouvelle/page.tsx         # Nouvelle tâche
        /[taskId]/page.tsx         # Détail tâche
      
      /journal                     # Journal partagé couple
        /page.tsx                  # Timeline journal
        /nouvelle-note/page.tsx    # Nouvelle note
      
      /settings
        /profile/page.tsx          # Profil utilisateur
        /potager/page.tsx          # Configuration potager
        /notifications/page.tsx    # Préférences notifications
    
    /api
      /health/route.ts
      /cultures/route.ts           # CRUD cultures
      /harvests/route.ts           # CRUD récoltes
      /tasks/route.ts              # CRUD tâches
      /weather/route.ts            # Données météo
      /analytics/route.ts          # Analyses/statistiques
  
  /components
    /ui                            # shadcn/ui générés
    /garden                        # Composants spécifiques jardinage
      /culture-card.tsx
      /garden-grid.tsx             # Vue 4 carrés
      /harvest-scanner.tsx         # Scanner récolte photo
      /weather-widget.tsx
      /planting-calendar.tsx
    /forms
      /culture-form.tsx
      /harvest-form.tsx
      /task-form.tsx
    /charts                        # Graphiques analyses
      /harvest-chart.tsx
      /weather-correlation.tsx
    /layout
      /garden-compass.tsx          # Navigation principale
      /user-menu.tsx
  
  /lib
    /auth.ts
    /db.ts
    /logger.ts
    /zod-schemas.ts              # Schémas validation jardinage
    /weather-api.ts              # Intégration API météo
    /ai-analytics.ts             # Logique IA analyses
    /garden-calculator.ts        # Calculs jardinage
    /utils.ts
  
  /styles
    /globals.css                 # Tokens OKLCH + animations organiques
```

---

## 🎭 PAGES PRINCIPALES & MAQUETTES

### 1. Dashboard Principal - "Vue Jardin Vivant"
```
┌─────────────────────────────────────────────────────────┐
│ 🌤️ 18°C ☀️  ⏰ 3 tâches  👥 Marie & Sacha  🔔         │
├─────────────────────────────────────────────────────────┤
│           🌱 VUE JARDIN TEMPS RÉEL                       │
│                                                         │
│  GARAGE 🏠              POTAGER 🌻                      │
│  🌡️ 22°C 💧 65%         ┌─Carré 1─┐ ┌─Carré 2─┐       │
│  ┌─────────────┐        │🥬 Salade │ │🍅 Tomates│       │
│  │🍅 Tomates   │        │  →Prêt   │ │  →15j    │       │
│  │   J+12      │        └─────────┘ └─────────┘       │
│  │🥒 Concombres│        ┌─Carré 3─┐ ┌─Carré 4─┐       │
│  │   J+5       │        │🥒 Concomb│ │⚡ Libre  │       │
│  └─────────────┘        │  →7j     │ │         │       │
│                         └─────────┘ └─────────┘       │
├─────────────────────────────────────────────────────────┤
│ AUJOURD'HUI - Mercredi 12 Août                          │
│ • 🚿 Arroser courgettes (météo sèche prévue)           │
│ • 🥕 Récolter radis (carré 1) - Estimation: 500g       │
│ • 🌱 Semer basilic (conditions garage optimales)        │
│                                           [Tout voir]   │
├─────────────────────────────────────────────────────────┤
│ [🌱 CULTIVER] [🌿 PLANTER] [🍅 RÉCOLTER] [📊 ANALYSER] │
└─────────────────────────────────────────────────────────┘
```

### 2. Navigation "Garden Compass"
```
          🌱 CULTIVER
        (Garage/Semis)
             │
             │
🏠 SETTINGS──┼──🌿 PLANTER
             │   (Potager)
             │
       🍅 RÉCOLTER
      (Productivité)
    
    Swipe ↕️ pour ANALYSER
      (Insights/Stats)
```

### 3. Pilier CULTIVER - "Serre Digitale"
```
┌─────────────────────────────────────────────────────────┐
│ 🌱 CULTIVER - Garage                           [+ SEMIS]│
├─────────────────────────────────────────────────────────┤
│ 🌡️ 22°C ✅  💧 65% ✅  💡 14h/jour ✅                  │
├─────────────────────────────────────────────────────────┤
│ SEMIS EN COURS                                          │
│                                                         │
│ 🍅 Tomates Coeur de Boeuf   [📸]     J+12              │
│ Repiquage prévu dans 3 jours  ┌─────────┐              │
│ État: Excellente croissance    │ [Photo] │              │
│                               └─────────┘              │
│                                                         │
│ 🥒 Concombres Long Vert        [📸]     J+5               │
│ Germination en cours          ┌─────────┐              │
│ Température idéale            │ [Photo] │              │
│                               └─────────┘              │
│                                                         │
│ 🌶️ Piments Doux              [📸]     J+20              │
│ ⚠️ Repiquage urgent          ┌─────────┐              │
│ Trop à l'étroit               │ [Photo] │              │
│                               └─────────┘              │
├─────────────────────────────────────────────────────────┤
│ STOCK GRAINES          [📦 Voir tout]                   │
│ • Basilic Grand Vert (exp: 2026) ✅                    │
│ • Radis Rond Rouge (exp: 2025) ⚠️ Bientôt périmé      │
└─────────────────────────────────────────────────────────┘
```

---

## 🚀 FONCTIONNALITÉS CLÉS À IMPLÉMENTER

### Dashboard Intelligence
- **Météo comportementale** : Interface adaptée aux conditions
- **Recommandations contextuelles** : Actions prioritaires selon saison/météo
- **Vue temps réel** : État live des cultures et environnement

### Pilier CULTIVER (Garage/Semis)
- **Contrôle climat** : Monitoring température/humidité
- **Planificateur semis** : Calcul dates optimales avec météo
- **Stock graines intelligent** : Alertes expiration, suggestions achat
- **Templates culture** : Paramètres pré-configurés par espèce

### Pilier PLANTER (Potager)
- **Vue spatiale 4 carrés** : Drag & drop cultures avec contraintes
- **Matrice compatibilité** : Associations bénéfiques/néfastes
- **Optimiseur ensoleillement** : Placement selon exposition
- **Rotation automatique** : Suggestions rotation année N+1

### Pilier RÉCOLTER (Productivité)
- **Mode express** : Scan photo avec reconnaissance automatique
- **Indicateurs maturité** : ML prédictif + comparaisons visuelles
- **Tracker continu vs ponctuel** : Selon type culture
- **Statistiques rendement** : Par m², culture, saison

### Pilier ANALYSER (Insights)
- **Corrélations météo-rendement** : Graphiques interactifs
- **Prédictions IA** : Recommandations saison suivante
- **Détection patterns** : Facteurs succès/échec
- **Benchmarking personnel** : Évolution performances

### Collaboration Couple
- **Journal partagé** : Timeline avec notes/photos
- **Assignation tâches** : Notifications croisées
- **Handoff invisible** : Synchronisation temps réel
- **Historique actions** : Traçabilité qui/quoi/quand

---

## 🔒 SÉCURITÉ & PERFORMANCE

### Authentification
- **Middleware protection** : Toutes routes `/app` protégées
- **Session sécurisée** : Cookies HttpOnly, Secure, SameSite
- **Gestion couple** : Partage household avec rôles OWNER/EDITOR

### Headers sécurité
```javascript
// next.config.mjs
const securityHeaders = [
  { key: 'Content-Security-Policy', value: cspPolicy },
  { key: 'Referrer-Policy', value: 'no-referrer' },
  { key: 'X-Content-Type-Options', value: 'nosniff' },
  { key: 'Permissions-Policy', value: 'geolocation=(), microphone=(), camera=(self)' }
];
```

### Performance
- **Server Components** par défaut, Client Components seulement si nécessaire
- **Suspense + Loading states** : Skeletons animation croissance
- **Image optimization** : Next.js Image avec formats WebP
- **PWA offline** : Cache intelligent données critiques

---

## 📱 PWA & RESPONSIVE

### Mobile-first
- **Touch targets** ≥ 44px avec espacement généreux
- **Mode terrain** : Interface utilisable avec gants
- **Scanner photo** : Capture optimisée récoltes
- **Navigation gesture** : Swipe pour piliers

### PWA Configuration
```json
// public/manifest.json
{
  "name": "Potager Connecté",
  "short_name": "Jardin",
  "theme_color": "oklch(0.65 0.08 142)",
  "background_color": "oklch(0.98 0.01 45)",
  "icons": [
    { "src": "/icons/icon-180.png", "sizes": "180x180", "type": "image/png" },
    { "src": "/icons/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icons/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

---

## 🧪 TESTS & QUALITÉ

### Tests unitaires (Vitest)
```typescript
// Exemple test hook jardinage
import { renderHook, act } from '@testing-library/react';
import { useGardenCalendar } from '@/lib/hooks/use-garden-calendar';

test('calcule dates semis optimales', () => {
  const { result } = renderHook(() => useGardenCalendar());
  act(() => {
    result.current.calculateSowingDate('tomate', new Date('2025-03-15'));
  });
  expect(result.current.sowingDate).toEqual(new Date('2025-02-15'));
});
```

### Tests E2E (Playwright)
```typescript
test('cycle complet culture', async ({ page }) => {
  await page.goto('/app/cultiver');
  await page.getByRole('button', { name: 'Nouveau semis' }).click();
  await page.getByLabel('Espèce').fill('Tomate');
  await page.getByLabel('Variété').fill('Coeur de Boeuf');
  await page.getByRole('button', { name: 'Créer semis' }).click();
  await expect(page.locator('[data-testid="culture-card"]')).toContainText('Tomate');
});
```

---

## 📋 CHECKLIST DÉVELOPPEMENT

### Phase 1 - Foundation
- [ ] Initialisation Next.js 15 + TypeScript + Tailwind
- [ ] Configuration shadcn/ui avec thème OKLCH
- [ ] Modèle Prisma complet jardinage
- [ ] Authentification NextAuth couple
- [ ] Structure dossiers imposée avec alias `@/*`

### Phase 2 - Core Features
- [ ] Dashboard "Vue Jardin" temps réel
- [ ] Navigation "Garden Compass" 4 piliers
- [ ] CRUD cultures avec cycle de vie complet
- [ ] Gestion spatial 4 carrés potager
- [ ] Scanner récolte avec photo
- [ ] Journal partagé couple

### Phase 3 - Intelligence
- [ ] Intégration API météo externe
- [ ] Calculs dates optimales semis/plantation
- [ ] Recommandations compatibilité cultures
- [ ] Analytics rendements + corrélations
- [ ] Notifications contextuelles

### Phase 4 - Polish
- [ ] PWA complète (manifest, SW, offline)
- [ ] Animations organiques signature
- [ ] Dark mode "Nuit au Jardin"
- [ ] Tests E2E scénarios complets
- [ ] Documentation README exhaustive

---

## 🎯 LIVRABLES FINAUX

### 1. Code source complet
- Application Next.js 15 fonctionnelle
- Base données PostgreSQL avec seed jardinage
- Tests unitaires + E2E couvrant scénarios principaux
- CI/CD GitHub Actions prêt à l'emploi

### 2. Documentation
- **README.md** avec installation pas à pas
- Schémas ASCII des interfaces principales
- Guide utilisation pour le couple jardinier
- Checklist "Production Ready"

### 3. Déploiement
- Configuration Docker pour NUC Ubuntu
- Variables environnement avec `.env.example`
- Scripts PM2 pour production locale
- Monitoring Sentry + analytics Plausible

### 4. Validation
- [ ] `npm ci && npm run db:migrate && npm run db:seed && npm run dev`
- [ ] Tests CI verts (lint, type-check, tests, build)
- [ ] PWA installable sur mobile
- [ ] Accessible WCAG AA
- [ ] Performance Web Vitals optimales

---

## 🚦 COMMANDES D'AMORÇAGE

```bash
# 1. Initialisation
npx create-next-app@latest potager-connecte \
  --typescript --tailwind --eslint --src-dir --app --import-alias "@/*"

# 2. Dépendances core
npm i @prisma/client next-auth zod @next/bundle-analyzer

# 3. Dépendances dev
npm i -D prisma vitest @testing-library/react @testing-library/jest-dom \
  @testing-library/user-event playwright @playwright/test \
  husky lint-staged commitlint @commitlint/config-conventional

# 4. shadcn/ui setup
npx shadcn@latest init -d
npx shadcn@latest add button input label textarea dialog select \
  table toast card badge checkbox tabs alert calendar

# 5. Base Prisma
npx prisma init
npx prisma migrate dev --name init
npx prisma db seed

# 6. Tests setup
npx playwright install --with-deps
```

**OBJECTIF :** Application potager connecté complète, fonctionnelle et prête pour production locale sur NUC Ubuntu, respectant parfaitement les spécifications UX de "Jardin Digital Apaisant" et les contraintes techniques imposées.