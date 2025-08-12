
# 🚀 CI/CD Automatique avec semantic-release

Ce document explique comment configurer un système de versioning automatique et de release Git avec `semantic-release`, compatible avec un projet Node.js (React, Vue, etc.) et Visual Studio Code.

---

## 🎯 Objectif

Permettre à ton application de :

* Générer automatiquement un numéro de version (major, minor, patch)
* Créer un changelog
* Créer un tag Git (`v1.2.3`)
* Faire une release GitHub
* Sans intervention manuelle

---

## ✅ Prérequis

- Projet avec Git (GitHub recommandé)
- Node.js installé
- Commits respectant la convention `conventional commits` :
  - `feat:` ➝ ajout de fonctionnalité (minor)
  - `fix:` ➝ correction de bug (patch)
  - `BREAKING CHANGE:` ➝ changement majeur

---

## 🧱 Étapes de configuration

### 1. Installer les dépendances

```bash
npm install --save-dev semantic-release @semantic-release/git @semantic-release/changelog @semantic-release/npm @semantic-release/github
```

---

### 2. Ajouter un fichier `.releaserc`

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    ["@semantic-release/git", {
      "assets": ["CHANGELOG.md", "package.json"],
      "message": "chore(release): ${nextRelease.version} [skip ci]"
    }],
    "@semantic-release/npm",
    "@semantic-release/github"
  ]
}
```

---

### 3. Créer un `CHANGELOG.md`

```bash
touch CHANGELOG.md
```

---

### 4. Ajouter un workflow GitHub Actions

Créer le fichier `.github/workflows/release.yml` :

```yaml
name: Release

on:
  push:
    branches:
      - main

jobs:
  release:
    name: Semantic Release
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm ci

      - name: Run semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: npx semantic-release
```

---

### 5. Tester en local (optionnel)

```bash
npx semantic-release --dry-run
```

---

## ⚠️ Important : Respecter la convention de commits

Exemples valides :

```bash
git commit -m "feat: ajoute les statistiques par plante"
git commit -m "fix: corrige le bug de validation de date"
git commit -m "feat: refonte complète du modèle

BREAKING CHANGE: suppression de l’attribut quantité"
```

---

## ✅ Compatible Visual Studio Code ?

Oui, tout est compatible :
- Terminal intégré
- Git intégré
- Husky/commitlint (optionnel)
- semantic-release en CLI

---

## 💡 Astuce : automatiser la validation des commits

Tu peux ajouter :

- **husky** pour bloquer les mauvais commits
- **commitlint** pour vérifier la syntaxe

```bash
npm install --save-dev husky @commitlint/{cli,config-conventional}
npx husky install
npx husky add .husky/commit-msg "npx --no commitlint --edit $1"
```

Créer un fichier `commitlint.config.js` :

```js
module.exports = {
  extends: ['@commitlint/config-conventional']
};
```

---

## 🧪 Résultat final

À chaque `push` sur `main`, GitHub Actions :
- Analyse les commits
- Déduit la version à incrémenter
- Met à jour le changelog
- Crée un tag Git
- Publie une release GitHub

Plus besoin de bump de version à la main ✨

