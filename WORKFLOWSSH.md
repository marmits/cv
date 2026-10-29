# Dépendance au thème et workflow

Le thème public `jsonresume-theme-stackoverflowmar` est installé depuis GitHub en HTTPS et fixé à la version `2.0.2` dans `package.json`. Aucune clé SSH n’est nécessaire.

En local, le projet utilise le container NodeTools. GitHub Actions s’exécute directement sur `ubuntu-latest` avec Node.js 24 et pnpm `12.16.1` ; il ne construit pas d’image Docker.

Le workflow lance `npm ci`, puis `npm run build` pour générer `index.html` et `resume.pdf`. Il publie également `resume.json` sur GitHub Pages. Les pushes et pull requests vers `dev` et `main` déclenchent le build ; la publication Pages a lieu uniquement depuis `main`.
