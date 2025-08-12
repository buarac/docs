# 📘 Git - Commandes Essentielles

## 📦 Initialisation & Configuration

```bash
git init                             # Initialise un dépôt Git local
git config --global user.name "Ton Nom"
git config --global user.email "ton@email.com"
```

---

## 🔁 Clonage d’un dépôt distant

```bash
git clone https://github.com/user/repo.git
cd repo
```

---

## 🌿 Gestion des branches

```bash
git branch                          # Liste les branches locales
git branch -r                       # Liste les branches distantes
git checkout -b ma-branche          # Crée une nouvelle branche et bascule dessus
git switch -c nouvelle-branche      # (équivalent moderne à checkout)
git checkout main                   # Revenir sur la branche main
git merge ma-branche                # Fusionne une branche dans la branche actuelle
git branch -d ma-branche            # Supprime une branche locale
```

---

## ⬆️ Pousser les modifications

```bash
git add .                            # Ajoute tous les fichiers modifiés
git commit -m "Message du commit"    # Crée un commit
git push                             # Pousse les changements sur la branche distante actuelle
git push -u origin ma-branche        # Pousse une nouvelle branche et la suit
```

---

## ⬇️ Récupération et mise à jour

```bash
git fetch                            # Récupère les branches distantes sans fusionner
git pull                             # Récupère et fusionne les changements distants
```

---

## 🧼 Nettoyage

```bash
git status                           # Affiche les fichiers modifiés ou non suivis
git clean -fd                        # Supprime les fichiers/dossiers non suivis
git reset --hard HEAD                # Annule tous les changements non commités
```

---

## 🔖 Tags et Releases

```bash
git tag                              # Liste les tags
git tag -a v1.0.0 -m "Version stable" # Crée un tag annoté
git push origin v1.0.0               # Pousse un tag
```

---

## 🕵️ Historique

```bash
git log                              # Affiche l'historique des commits
git log --oneline --graph            # Affichage simplifié avec arbre
git diff                             # Montre les changements non commités
```

---

## 🛠️ Cas utiles

```bash
git stash                            # Sauvegarde temporairement les modifications non commités
git stash pop                        # Restaure ce qui a été stashed
git remote -v                        # Affiche les URLs distantes
```

---

## 🚨 En cas de besoin

```bash
git restore .                        # Annule toutes les modifications non commitées (Git 2.23+)
git revert <hash>                    # Crée un commit qui annule un commit précédent
```

---

## 🔗 Astuces multi-machine (clone / sync)

```bash
# Cloner un dépôt depuis une autre machine :
git clone git@github.com:user/repo.git

# Passer sur une branche distante :
git checkout -b version_2 origin/version_2
```

---

## 📌 Recommandation : fichiers utiles

- `.gitignore` → pour ignorer les fichiers temporaires (node_modules, .env, etc.)
- `README.md` → documentation du projet
- `git.md` → ce fichier d’aide Git 😉