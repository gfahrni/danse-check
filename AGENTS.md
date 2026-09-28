# AGENTS.md — danse-check

## But du projet
App **« Ma danse »** pour Ambre sur **iPad mini 4** (Safari ancien, max iPadOS 15.x).
Clone simplifié de `instruments-check` : **1 seule colonne peach full screen** (pas de dual screen),
qui défile verticalement (`scroll-snap`) entre **2 panneaux** : panneau 1 = danse/étoile,
panneau 2 = cadeau/carte.
Bouton **Fait** → étoile débloquée **immédiatement**. Bouton **🎁 Cadeau Bonus** (popup de
confirmation) → débloque le panneau 2 (TP automatique dessus), qui affiche une **carte du jour**
Minecraft (image seule). Bouton **↺ Recommencer**.
Déployé sur **GitHub Pages** (repo `gfahrni/danse-check`).

## Stack
- HTML/CSS/JS statique uniquement, sans build, sans dépendance, sans framework.
- Déploiement : push sur `main` → GitHub Pages.

## Structure
- `index.html` : layout 1 colonne + logique (JS simple, tout inline, ES5).
- `danse-v1.css` : thème rose-peach. **Renommer à chaque changement visuel**
  (ex: `danse-v2.css`) pour forcer le cache Safari.
- `cartes.json` : `{ ambre: {...cartes} }` — 236 cartes Panini Minecraft Adventure.
  Chaque carte = `{id, num, nom, type, img}`.
- `cartes/minecraft/<n>.jpg`, `cartes/minecraft/le<n>.jpg` : images locales (offline).
  236 images = 230 Panini Minecraft Adventure (2021) + 6 Limited Edition.
- `telecharger-cartes.py` : télécharge les images manquantes listées dans `cartes.json`.

## Données / set
- **Ambre (danse 💃)** = Panini Minecraft Adventure 2021, collection laststicker **5904**.
- Motif : `https://www.laststicker.com/i/cards/5904/<n>.jpg` (1-230) + `le<n>.jpg` (le1-le6, minuscules).
- Checklist scrapée depuis `laststicker.ru` (le .com est derrière Cloudflare).
- Images hotlinkées une seule fois puis **stockées dans le repo**.

## Contraintes iPad mini 4 (importantes)
- Compatible Safari iOS 15 : JS ES5 (pas de modules ES, optional chaining, etc.).
- Garder les meta iOS : `viewport-fit=cover`, `apple-mobile-web-app-capable`, `apple-mobile-web-app-status-bar-style`.
- Tactile first : grosses zones de toucher, pas de hover-only.
- Tester en conditions réelles : ajout à l'écran d'accueil + mode plein écran.

## Conventions de travail
- Modifier `index.html` directement, garder le fichier petit et lisible.
- **Reset quotidien** : `resetIfNewDay()` / `clearAll()` efface tout à chaque nouveau jour
  (clé `danse-v1-day`). Recharge aussi au retour d'app (visibilitychange/pageshow).
- **Carte du jour** : sélection **stable toute la journée** (`hash(date|ambre) % nbCartes`),
  mémorisée sous `danse-v1-carte-ambre` ; change uniquement le lendemain.
- **Bouton ↺ Recommencer** (`clearKid`) : efface mission + cadeau, **sans** toucher à la
  sélection de la carte du jour. Remonte la colonne sur le panneau 1.
- État sous préfixe `danse-v1-` (`fait-ambre`, `bonus-ambre`, `carte-ambre`, `day`) —
  isolé de `instruments-v1-` pour ne pas interférer.
- Un test = un commit clair.
- Ne pas ajouter de tooling/build sans demande explicite.
- Langue UI : français.
