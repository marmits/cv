# Génération du CV

## Prérequis

- Node.js 26 ou plus récent et npm.
- Le fichier `resume.json` à la racine du projet.
- Un accès Internet pour installer les dépendances et télécharger Chrome via Puppeteer.
- Le thème `jsonresume-theme-stackoverflowmar` en version `v2.0.5`, installé depuis GitHub. Le thème ne nécessite pas pnpm. Pour le développer localement, voir ci-dessous.

## Installation

À la racine du projet :

```sh
npm ci
```

Pour utiliser la source locale du thème pendant son développement, installe ses dépendances dans le dossier du thème avec `npm install --no-package-lock`, puis exécute `npm link` dans le thème et `npm link jsonresume-theme-stackoverflowmar` dans cette application. Refais le lien après `npm ci`.

Le workflow GitHub utilise Node.js 26 et le thème `v2.0.5`. Il lance `npm ci`, les vérifications du projet et `npm run build`, puis publie le site sur GitHub Pages lors d’un push sur `main`. Docker et pnpm ne sont pas nécessaires.

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
