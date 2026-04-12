# Zoé Charge

Calculateur de recharge pour **Renault Zoé (batterie 52 kWh)**, conçu comme une Progressive Web App (PWA) installable sur Android.

---

## Fonctionnalités

- Saisie du pourcentage de charge actuel via des boutons **+** / **−**
  - Appui court : ±1 %
  - Appui long : ±10 % (accélération automatique)
- Choix du mode de recharge : **3,7 kW** (prise renforcée) ou **1,8 kW** (prise lente)
- Saisie de l'**heure de fin souhaitée** → calcul automatique de l'heure à laquelle brancher le véhicule
- Alerte visuelle si le branchement doit avoir lieu **la veille**
- Affichage en temps réel de l'énergie à recharger (kWh) et de la durée estimée
- Fonctionne **hors connexion** (service worker)
- Installable sur l'écran d'accueil Android (bannière d'installation intégrée)

---

## Paramètres techniques

| Paramètre | Valeur |
|---|---|
| Capacité nominale | 52 kWh |
| SoH (état de santé) | 95 % |
| Capacité réelle | 49,4 kWh |
| SoC cible | 80 % |
| Rendement prise renforcée (3,7 kW) | 90 % |
| Rendement prise lente (1,8 kW) | 90 % |

> Le SoH et les rendements ont été estimés à partir de mesures réelles sur le véhicule. Ils peuvent être ajustés directement dans le fichier `index.html` (constantes `BATTERY` et `MODES`).

---

## Structure des fichiers

```
zoe-pwa/
├── index.html      Application principale (HTML/CSS/JS en un seul fichier)
├── manifest.json   Manifest PWA (nom, icônes, couleurs)
├── sw.js           Service worker (mise en cache hors ligne)
├── icons/
│   ├── icon-192.png
│   └── icon-512.png
└── README.md
```

---

## Installation sur Android

### Étape 1 — Héberger les fichiers

La méthode la plus simple est **Netlify Drop** (gratuit, sans compte) :

1. Rendez-vous sur [app.netlify.com/drop](https://app.netlify.com/drop)
2. Glissez le dossier `zoe-pwa/` dans la zone de dépôt
3. Netlify génère une URL publique du type `https://xxxxx.netlify.app`

### Étape 2 — Installer sur le téléphone

1. Ouvrez l'URL dans **Chrome** sur votre smartphone Android
2. Une bannière « Installer l'application » apparaît automatiquement
3. Appuyez sur **Installer** puis confirmez
4. L'icône apparaît sur votre écran d'accueil

L'application s'ouvre ensuite en plein écran, sans barre de navigation du navigateur, et fonctionne sans connexion internet.

---

## Modifier les paramètres

Pour ajuster les valeurs techniques, ouvrez `index.html` et repérez le bloc JavaScript :

```js
const MODES = [
  { power: 3.7, eff: 0.90 },  // Prise renforcée
  { power: 1.8, eff: 0.90 },  // Prise lente
];
const BATTERY = 52 * 0.95;    // Capacité réelle (SoH 95 %)
const TARGET  = 80;            // SoC cible en %
```

Après modification, incrémentez le numéro de version dans `sw.js` :

```js
const CACHE = 'zoe-charge-vXX';  // Changer XX pour forcer la mise à jour
```

---

## Stack technique

- HTML / CSS / JavaScript vanilla — aucune dépendance
- Polices : [Bebas Neue](https://fonts.google.com/specimen/Bebas+Neue) (chiffres), [DM Mono](https://fonts.google.com/specimen/DM+Mono) (texte)
- PWA : Web App Manifest + Service Worker (cache-first)
- Optimisé pour **Samsung Galaxy S21 FE** en mode portrait
