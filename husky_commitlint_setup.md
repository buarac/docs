
# 🛡️ Configuration de Husky + Commitlint

Ce document explique comment configurer **Husky** et **Commitlint** dans un projet Node.js pour forcer des messages de commit valides selon la convention `conventional commits`.

---

## 🎯 Objectif

- Bloquer les commits invalides
- Forcer un format compatible avec `semantic-release`
- S'assurer que chaque commit est interprétable automatiquement pour la release

---

## ✅ Étapes de configuration

### 1. Installer Husky et Commitlint

```bash
npm install --save-dev husky @commitlint/cli @commitlint/config-conventional
```

---

### 2. Initialiser Husky

```bash
npx husky install
```

Puis, dans ton `package.json`, ajoute :

```json
{
  "scripts": {
    "prepare": "husky install"
  }
}
```

---

### 3. Ajouter un hook pour valider les messages de commit

```bash
npx husky add .husky/commit-msg "npx --no commitlint --edit $1"
```

Cela crée `.husky/commit-msg` contenant :

```bash
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no commitlint --edit "$1"
```

---

### 4. Créer le fichier de config `commitlint.config.js`

À la racine de ton projet :

```js
// commitlint.config.js
module.exports = {
  extends: ['@commitlint/config-conventional']
};
```

---

## 🧪 Exemple de messages valides

| Type     | Exemple                                  | Effet sur semantic-release |
|----------|-------------------------------------------|----------------------------|
| `feat:`  | feat: ajoute l'écran de suivi             | Crée une version **minor** |
| `fix:`   | fix: corrige le bug de récolte vide       | Crée une version **patch** |
| `BREAKING CHANGE:` | feat: refonte totale \n\nBREAKING CHANGE: modifie les récoltes | **Major** |

---

## 🧪 Test

Pour tester :

```bash
git commit -m "invalid commit"
# => bloqué par Husky + Commitlint

git commit -m "feat: nouvelle statistique"
# => accepté
```

---

## 🔄 Résumé

| Outil         | Rôle                             |
|---------------|----------------------------------|
| **Husky**     | Exécuter des hooks Git locaux    |
| **Commitlint**| Valider le format des commits    |

---

Avec cette configuration, tu garantis que tous les commits dans ton projet sont compatibles avec `semantic-release`, ce qui permet un CI/CD totalement automatique. 🔁🚀
