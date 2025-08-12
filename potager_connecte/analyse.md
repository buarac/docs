# Analyse Produit : Application Potager Connecté

## 🏡 Écosystème & Personas

### Persona Principal : Jardinier Amateur Passionné
- **Contexte :** Setup technique avancé (garage équipé, 4 carrés potager)
- **Besoins :** Optimisation rendement, planification stratégique, suivi précis
- **Pain points :** Coordination timing, gestion variétés multiples, optimisation météo

### Contextes d'Usage Multi-Device
- **📱 iPhone** : Saisies terrain, quick actions, notifications
- **💻 Desktop** : Planning, analytics, gestion stock détaillée  
- **📺 TV** : Dashboard passif, consultations evening routines

## 🌱 Workflows des 3 Piliers

### CULTIVER (Garage Indoor)
```
Planification Semis
├── Vérif stock graines ✓/✗
├── Calcul timing optimal (météo + culture)
├── Setup environnemental (T°, éclairage)
└── Suivi progression
    ├── Levée
    ├── Repiquage 1 (optionnel)
    ├── Repiquage 2 (optionnel)
    └── → Prêt plantation
```

### PLANTER (Potager 4 carrés)
```
Attribution Emplacement
├── Analyse ensoleillement par carré
├── Compatibilités cultures adjacentes
├── Rotation prévue
└── Plantation
    ├── Timing météo optimal
    ├── Stratégie arrosage
    └── → Suivi croissance
```

### RÉCOLTER (Cycles variables)
```
Détection Maturité
├── Cultures continues (haricots verts)
├── Cultures ponctuelles (melons)  
├── Cultures pérennes (arbres)
└── Analytics & Optimisation
```

## 📱💻📺 Architecture Multi-Device

### Navigation Adaptive
| Mobile (iPhone) | Desktop | TV Browser |
|---|---|---|
| Quick Actions | Dashboard Central | Overview Status |
| Saisie Rapide | Planning Gantt | Météo 7j |
| Photos/Scan | Analytics Deep | Récoltes Semaine |
| Notifications | Gestion Stocks | Photos Timeline |
| Mini Dashboard | Configuration | [Navigation simple] |

### Répartition Fonctionnelle

**📱 Mobile (Context of Action)**
- Saisies terrain (récoltes, observations)
- Quick actions (arrosé ✓, semé ✓)  
- Photos progress cultures
- Notifications intelligentes (timing optimal)

**💻 Desktop (Planning & Analysis)**
- Timeline master des 3 piliers
- Drag & drop planning cultures
- Analytics comparatifs annuels
- Gestion détaillée stocks/variétés

**📺 TV (Passive Monitoring)**
- Dashboard temps réel 4 carrés
- Météo visualisée 10 jours
- Photos captures quotidiennes
- Stats saisonnières

## 🎨 Design System Adaptatif

### Palette Saisonnière
- 🌱 **Printemps** : Verts tendres (semis, plantation)
- ☀️ **Été** : Oranges/rouges (croissance, récolte)  
- 🍂 **Automne** : Ocres/bruns (fin récolte, préparation)
- ❄️ **Hiver** : Bleus/gris (planification, analyse)

### Composants Cross-Platform

**📱 Mobile Components**
- **Thumb Cards** : Actions rapides 44px min
- **Swipe Gestures** : Navigation piliers
- **Camera Integration** : Scan codes variétés
- **Location Context** : GPS garage/potager

**💻 Desktop Components**  
- **Timeline Gantt** : Planning annuel cultures
- **Drag & Drop Grid** : Attribution carrés potager
- **Multi-panel Layout** : Météo + planning + stocks
- **Keyboard Shortcuts** : Power user workflows

**📺 TV Components**
- **Card Carousels** : Navigation télécommande  
- **Large Typography** : Lisibilité distance
- **Auto-refresh Data** : Updates passives
- **Simple 4-way Nav** : Up/down/left/right

## ⚡ Fonctionnalités Core par Pilier

### 🌱 CULTIVER - Features
- **Smart Timing Calculator** : IA calcule date semis optimale (météo + variété)
- **Stock Manager** : Scanner QR sachets graines + inventory
- **Environment Controller** : Suivi T° garage, éclairage, humidité
- **Variété Comparator** : Side-by-side tomates cœur-de-bœuf vs Crimée
- **Repiquage Planner** : Alerts timing optimal pour transplantation

### 🪴 PLANTER - Features  
- **4-Square Optimizer** : IA suggère meilleur carré selon ensoleillement
- **Companion Matrix** : Visualisation compatibilités cultures adjacentes
- **Weather Integration** : Recommandations plantation selon prévisions 10j
- **Rotation Tracker** : Historique 3 ans par carré pour éviter appauvrissement
- **Irrigation Strategy** : Fréquence arrosage selon météo + type culture

### 🥕 RÉCOLTER - Features
- **Maturity Detector** : Photo-analysis IA pour détection maturité
- **Harvest Tracker** : Saisie rapide poids/quantité par culture  
- **Yield Analytics** : Comparatifs rendement par conditions météo
- **Storage Advisor** : Conseils conservation selon culture récoltée
- **Season Optimizer** : Insights pour optimiser saison suivante

## 🛠️ Stack Technique Multi-Plateforme

### Architecture Recommandée

**Frontend Multi-Device :**
- **React Native** (iPhone app native)
- **Next.js 14** (Web desktop + TV browser)  
- **Tailwind CSS** (design system unifié)
- **Framer Motion** (animations fluides)

**Backend & Services :**
- **Supabase** (BaaS + real-time sync)
- **Cloudinary** (stockage photos optimisé)
- **OpenWeatherMap API** (météo précise)
- **OpenAI API** (analyses IA cultures)

**Déploiement :**
- **Vercel** (web apps)
- **App Store** (iOS)
- **Expo** (React Native toolchain)

**Avantages Stack :**
- **Sync temps réel** entre tous devices
- **Offline-first** pour usage terrain  
- **Scalable** pour features IA futures
- **Cost-efficient** avec BaaS

## 🤖 Analyses IA & Insights

### Moteurs d'Intelligence

**🌦️ Weather-Yield Correlator**
- Analyse 3 ans données : rendement tomates vs pluviométrie
- Prédiction : "Août sec = -30% rendement courgettes"  
- Recommandation : "Arrosage +40% prévus cette semaine"

**📸 Plant Health Detector**  
- Computer vision sur photos quotidiennes
- Détection précoce maladies/parasites
- Alert : "Mildiou suspecté carré 2 - traiter avant propagation"

**⏰ Optimal Timing Predictor**
- ML sur historique semis/récoltes/météo
- Suggestion : "Semez basilic dans 5j (T° optimale prévue)"
- Personnalisé selon micro-climat garage/potager

**🎯 Space Optimizer**  
- Algorithme rotation cultures 4 carrés
- Maximisation rendement/m² selon associations
- Planning 2 saisons à l'avance avec rotations optimales

**📊 Season Performance Analytics**
- Comparatif multi-années par culture
- ROI par variété (coût graines vs kg récoltés)
- Insights : "Haricots verts Mangetout +15% vs autres variétés"

## 🚀 Synthèse & Roadmap

### MVP Recommandé (Phase 1)
1. **Core tracking** des 3 piliers sur mobile + desktop
2. **Basic analytics** rendement par culture  
3. **Weather integration** simple
4. **Photo storage** et timeline visuelle

### V2 (Phase 2)  
- **TV app** optimisée  
- **IA analyses** weather-yield + plant health
- **Advanced planning** avec optimiseur espace

### V3 (Phase 3)
- **IoT sensors** garage (température, humidité)
- **Community features** (partage variétés, tips)
- **Marketplace** integration achats graines

---

# 👨‍💻 ANALYSE TECHNIQUE DÉVELOPPEUR

## 🏗️ Recommandations d'Architecture

### Stack Technique Optimisée

**Frontend Multi-Device (Amélioré) :**
- **Expo Router** (au lieu React Native pur) - développement unifié iOS/Android
- **Next.js 14** avec App Router (Web desktop + TV browser)  
- **Tailwind CSS + Shadcn/ui** (design system cohérent)
- **Framer Motion** (animations cross-platform)

**Backend & Services (Renforcé) :**
- **Supabase** (Auth + PostgreSQL + Storage + Edge Functions)
- **Prisma** (ORM avec type-safety complète)
- **tRPC** (API type-safe end-to-end)
- **Zustand** (state management léger)
- **Cloudinary** (images avec transformations automatiques)
- **OpenWeatherMap + Météo France APIs** (redondance météo)

**Outils Développement :**
- **TypeScript strict** (zero runtime errors)
- **ESLint + Prettier** (code consistency)
- **Vitest** (testing moderne)
- **Storybook** (composants documentés)

### Justifications Techniques

**Expo Router vs React Native :**
- File-based routing unifié mobile/web
- Développement 40% plus rapide
- Hot reload optimisé
- Build optimisé automatique

**tRPC vs REST :**
- Type-safety complete frontend ↔ backend
- Autocomplete partout
- Refactoring sécurisé
- Pas de documentation API à maintenir

**Prisma + Supabase :**
- Schema migrations automatiques
- Type generation depuis DB
- Relations complexes simplifiées
- Real-time out of the box

## 📊 Évaluation Complexité

### Effort Estimé
- **MVP (Phase 1)** : 3-4 mois (1-2 devs)
- **V2 complet** : 6-8 mois (2-3 devs)  
- **V3 avec IA** : 10-12 mois (3-4 devs)

### Complexité par Module
| Module | Complexité | Risque | Effort |
|---|---|---|---|
| Auth + CRUD | Faible | Faible | 2 semaines |
| Mobile App | Moyenne | Faible | 4 semaines |
| Desktop Dashboard | Moyenne | Faible | 3 semaines |
| TV Interface | Faible | Moyen | 2 semaines |
| Weather Integration | Faible | Moyen | 1 semaine |
| Photo Management | Moyenne | Faible | 2 semaines |
| IA Analytics | **Élevée** | **Élevé** | 8 semaines |
| Multi-device Sync | Moyenne | Moyen | 3 semaines |

## 🗄️ Architecture de Données

### Modèle de Données Principal

```typescript
// Schema Prisma simplifié
model User {
  id       String   @id @default(uuid())
  email    String   @unique
  gardens  Garden[]
}

model Garden {
  id       String   @id @default(uuid()) 
  name     String
  userId   String
  user     User     @relation(fields: [userId], references: [id])
  squares  Square[]
}

model Square {
  id           String     @id @default(uuid())
  name         String     // "Carré Nord-Est"
  sunExposure  String     // "full-sun" | "partial-sun" | "shade"
  size         Float      // m²
  gardenId     String
  garden       Garden     @relation(fields: [gardenId], references: [id])
  plantings    Planting[]
}

model Variety {
  id            String     @id @default(uuid())
  name          String     // "Tomate Cœur de Bœuf"
  category      String     // "tomato"
  sowingPeriod  String     // "march-april"
  harvestPeriod String     // "july-september"
  companions    String[]   // ["basil", "parsley"]
  antagonists   String[]   // ["fennel"]
  plantings     Planting[]
}

model Planting {
  id          String      @id @default(uuid())
  varietyId   String
  variety     Variety     @relation(fields: [varietyId], references: [id])
  squareId    String
  square      Square      @relation(fields: [squareId], references: [id])
  sowDate     DateTime?   // Si semis
  plantDate   DateTime?   // Mise en terre
  status      String      // "seeded" | "planted" | "growing" | "harvesting"
  activities  Activity[]
  harvests    Harvest[]
}

model Activity {
  id          String   @id @default(uuid())
  type        String   // "watering" | "repotting" | "observation"
  note        String?
  photos      String[] // URLs Cloudinary
  date        DateTime @default(now())
  plantingId  String
  planting    Planting @relation(fields: [plantingId], references: [id])
}

model Harvest {
  id         String   @id @default(uuid())
  weight     Float    // kg
  quantity   Int?     // unités
  quality    String   // "excellent" | "good" | "average"
  date       DateTime @default(now())
  photos     String[]
  plantingId String
  planting   Planting @relation(fields: [plantingId], references: [id])
}
```

### API Routes Structure

```typescript
// tRPC Router structure
const appRouter = router({
  // Auth
  auth: authRouter,
  
  // Core entities
  gardens: router({
    list: publicProcedure.query(),
    create: protectedProcedure.input(createGardenSchema).mutation(),
    update: protectedProcedure.input(updateGardenSchema).mutation(),
  }),
  
  plantings: router({
    list: protectedProcedure.input(z.object({ gardenId: z.string() })).query(),
    create: protectedProcedure.input(createPlantingSchema).mutation(),
    timeline: protectedProcedure.input(timelineSchema).query(),
  }),
  
  activities: router({
    log: protectedProcedure.input(activitySchema).mutation(),
    list: protectedProcedure.input(z.object({ plantingId: z.string() })).query(),
  }),
  
  analytics: router({
    dashboard: protectedProcedure.query(),
    yieldAnalysis: protectedProcedure.input(analysisSchema).query(),
    weatherCorrelation: protectedProcedure.input(weatherSchema).query(),
  }),
  
  weather: router({
    current: protectedProcedure.query(),
    forecast: protectedProcedure.input(z.object({ days: z.number() })).query(),
  }),
});
```

## 🚀 Stratégie de Développement

### Phase 1 - MVP Foundation (Sprints 1-4)
**Sprint 1-2 (4 semaines)**
- Setup projet (Expo + Next.js + Supabase)
- Auth système complet
- Modèle données de base + Prisma setup
- CRUD gardens/squares/plantings

**Sprint 3-4 (4 semaines)**  
- Mobile app core (Expo Router)
- Desktop dashboard basique
- Photo upload/management
- Basic timeline view

### Phase 2 - Core Features (Sprints 5-8)
**Sprint 5-6 (4 semaines)**
- Weather API integration
- Planning/calendrier fonctionnalités  
- Activities logging système
- TV interface MVP

**Sprint 7-8 (4 semaines)**
- Harvest tracking complet
- Analytics basiques (charts, stats)
- Multi-device sync optimizations
- Performance optimizations

### Phase 3 - Intelligence (Sprints 9-12)
**Sprint 9-10 (4 semaines)**
- IA recommandations timing
- Plant health detection (basic)
- Weather-yield correlations
- Companion planting suggestions

**Sprint 11-12 (4 semaines)**
- Advanced analytics dashboard
- Seasonal planning optimizer
- Notifications système intelligent
- Beta testing + optimizations

## ⚠️ Risques Techniques & Mitigations

### 1. Coûts APIs Externes
**Risque :** OpenAI + Weather + Cloudinary = €€€
**Mitigation :** 
- Cache agressif (Redis/Upstash)
- Batch processing IA
- Image compression automatique
- Rate limiting intelligent

### 2. Complexité Multi-Device Sync
**Risque :** Conflits données, état incohérent
**Mitigation :**
- Optimistic updates avec rollback
- Conflict resolution automatique
- Offline-first avec sync queues
- Supabase real-time pour push updates

### 3. Performance Mobile (Photos)
**Risque :** App lente, storage explosif  
**Mitigation :**
- Lazy loading images
- WebP/AVIF compression
- Thumbnail generation automatique
- Background sync photos

### 4. Complexité IA Features
**Risque :** Over-engineering, coûts, latence
**Mitigation :**
- Commencer par règles simples (heuristics)
- IA progressive (A/B testing)
- Fallback sur données statiques
- Cache predictions courantes

### 5. TV Browser Limitations
**Risque :** Performance, navigation, compatibility
**Mitigation :**
- Interface read-only simplifiée
- Auto-refresh polling
- Fallback vers mobile pour actions
- Focus sur smart TV modernes uniquement

## 💡 Optimisations Recommandées

### Performance
- **Code splitting** par feature/route
- **Image optimization** automatique (Next.js Image)
- **Database indexes** sur queries fréquents  
- **CDN** pour assets statiques (Vercel Edge)

### DX (Developer Experience)
- **Monorepo** structure (apps/packages)
- **Shared types** between mobile/web
- **E2E testing** avec Playwright
- **CI/CD** avec preview deployments

### Sécurité
- **Row Level Security** (Supabase RLS)
- **API rate limiting**  
- **Input validation** (Zod schemas)
- **CORS** configuration stricte

## 📊 Estimation Coûts Mensuels

### Services (Usage modéré - 100 users actifs)
- **Supabase Pro** : $25/mois
- **Vercel Pro** : $20/mois  
- **Cloudinary** : $30/mois (10GB)
- **OpenWeatherMap** : $40/mois (60k calls)
- **OpenAI API** : $50/mois (estimé)
- **App Store** : $99/an

**Total : ~$165/mois** (phase initiale)

## Conclusion

Cette analyse technique confirme la **faisabilité** du projet avec la stack recommandée. L'approche progressive (MVP → V2 → V3) permet de valider le product-market fit avant d'investir dans les features IA coûteuses.

Les points critiques sont la **gestion multi-device** et l'**optimisation des coûts IA** - tous deux gérables avec l'architecture proposée.

**Recommandation finale :** Démarrer par un MVP 4 mois avec focus mobile + desktop, puis itérer selon les retours utilisateurs.