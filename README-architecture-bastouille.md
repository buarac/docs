# 🌿 Architecture du projet Baštouille

## 🧱 Stack technique

- **Framework** : Next.js (avec App Router)
- **UI** : React + shadcn/ui + Tailwind CSS
- **Backend** : API routes Next.js + PostgreSQL
- **ORM** : Prisma
- **Appels externes** : Open-Meteo, Home Assistant
- **Cible** : Mobile (iPhone) + Desktop

---

## 🗂️ Structure du projet

```
app/
  mobile/
    layout.tsx              # Layout iPhone
    home/page.tsx           # Vue mobile principale (récolte rapide)

  desktop/
    layout.tsx              # Layout desktop
    dashboard/page.tsx      # Stats, cumuls
    admin/page.tsx          # Gestion des cultures
    stats/page.tsx          # Analyses poussées

  api/
    meteo/route.ts          # API météo wrapper Open-Meteo
    homeassistant/route.ts  # API proxy Home Assistant

components/
  WeatherDisplayCard.tsx    # Composant météo
  [autres composants UI]

lib/
  weather.ts                # Fonctions appel Open-Meteo
  homeassistant.ts          # Fonctions appel Home Assistant
  db.ts                     # Prisma client

prisma/
  schema.prisma             # Modèle de données

types/
  weather.ts                # Types TypeScript partagés

public/
  images/cultures/          # Illustrations des cultures
  icons/                    # Icônes Lucide

.env.local                  # Variables locales
```

---

## 🚦 Comportement mobile vs desktop

- **Middleware Next.js** détecte l'agent utilisateur
  - Redirection vers `/mobile` ou `/desktop`
- **Layouts distincts** : bottom-nav iPhone, navigation étendue desktop
- Possibilité d’un `useDevice()` hook pour des rendus conditionnels

---

## 🔌 API internes & externes

- Accès aux APIs via `/api/...`
- Sécurisation avec token d’accès (Home Assistant, etc.)
- Centralisation des appels météo (pas d’appel direct depuis le client)

---

## 📊 Base de données

- PostgreSQL + Prisma
- Modèles :
  - Culture
  - Récolte
  - Météo
  - Historique
- Génération d’UUID (via `gen_random_uuid()`)

---

## 📦 Extension possible (monorepo futur)

```
apps/
  web/           # App Next.js (mobile + desktop)
  server/        # Cron météo, tâches background

packages/
  ui/            # shadcn/ui + composants partagés
  lib/           # appels API, hooks, utilitaires
  db/            # Prisma + accès DB partagé
```

---

## 📌 Objectif à court terme

- ✅ P1 : Saisie rapide de récolte (mobile)
- 🧑‍🌾 P2 : Admin cultures (desktop)
- 📊 P3 : Stats et tableaux de bord
- 🌱 P4 : Suivi des cultures
- 🔬 P5 : Analyse croisée (météo, rendement, etc.)

---

Baštouille : un projet modulaire, scalable, et agréable à coder 🌿
