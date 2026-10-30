# Génération du CV

## Prérequis

- Node.js 26 ou plus récent, npm et pnpm `12.16.1` (le thème utilise pnpm dans son script `prepare`).
- Le fichier `resume.json` à la racine du projet.
- Un accès Internet pour installer les dépendances et télécharger Chrome via Puppeteer.
- Le thème `jsonresume-theme-stackoverflowmar` version `2.0.2`.

## Installation

À la racine du projet :

```sh
npm ci
```

## Génération

Générer le HTML seul (`resume.html`) :

```sh
npx resumed render resume.json --theme jsonresume-theme-stackoverflowmar
```

Générer le PDF seul (`resume.pdf`) :

```sh
npm start
```

Générer les deux fichiers pour le site (`index.html` et `resume.pdf`) :

```sh
npm run build
```

Après modification de `resume.json`, relancer `npm run build`.
