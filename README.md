# CV Marmits

Ce dépôt génère le CV à partir de `resume.json`, en HTML et en PDF avec le thème `jsonresume-theme-stackoverflowmar` version `v2.0.3`. La source locale du thème peut être reliée avec `npm link` pour la développer.

## Utilisation locale

Le dépôt est monté dans le conteneur NodeTools sous `/app/cvmarmits`. Depuis ce répertoire :

```sh
npm ci
npm run build
```

La commande produit `index.html` et `resume.pdf`. Les prérequis et les commandes de génération séparées sont dans [DOC.md](DOC.md).

## GitHub Actions et publication

Le workflow [`.github/workflows/npm-resume.yml`](.github/workflows/npm-resume.yml) utilise directement un runner Ubuntu avec Node.js 26 LTS ; il ne construit pas d’image Docker. Il installe les dépendances avec `npm ci`, puis lance `npm run build`.

Les pushes vers `main` déclenchent le build et la publication GitHub Pages : `index.html`, `resume.pdf` et `resume.json`.

Le thème est récupéré depuis GitHub en HTTPS ; aucune clé SSH n’est nécessaire. Voir [WORKFLOWSSH.md](WORKFLOWSSH.md) pour le détail de cette dépendance.

## JsonResume

- [jsonresume.org](https://jsonresume.org)
- [resumed sur npm](https://www.npmjs.com/package/resumed) — `npm install resumed` permet de l’ajouter à un nouveau projet. Ici, les dépendances sont installées avec `npm ci`.
- [resume-cli sur npm](https://www.npmjs.com/package/resume-cli) — non utilisé.

## Dépendances et versions

Le projet utilise Node.js 26 LTS. Les badges npm et GitHub ci-dessous indiquent les versions publiées en amont, qui peuvent différer des versions verrouillées dans `package-lock.json`.

[![Node.js 26 LTS](https://img.shields.io/badge/Node.js-26_LTS-339933?logo=node.js&logoColor=white)](https://nodejs.org/en/about/previous-releases)

[![resumed](https://img.shields.io/github/v/release/rbardini/resumed?label=rbardini%2Fresumed)](https://github.com/rbardini/resumed)

[![Puppeteer](https://img.shields.io/npm/v/puppeteer?label=puppeteer)](https://www.npmjs.com/package/puppeteer)

[![jsonresume-theme-stackoverflowmar](https://img.shields.io/github/package-json/v/marmits/jsonresume-theme-stackoverflowmar?label=jsonresume-theme-stackoverflowmar)](https://github.com/marmits/jsonresume-theme-stackoverflowmar)

## Installation et documentation

- [Documentation complète de NodeTools](https://github.com/marmits/nodetools/tree/main/app/cvmarmits)
- [Installation et commandes locales](DOC.md)

## Workflow GitHub Actions

- [Workflow de cette application](https://github.com/marmits/cv/blob/main/.github/workflows/npm-resume.yml)
- [Dépendance au thème et détails du workflow](WORKFLOWSSH.md)

Actions utilisées :

- [checkout](https://github.com/actions/checkout)
- [setup-node](https://github.com/actions/setup-node)
- [upload-pages-artifact](https://github.com/actions/upload-pages-artifact)
- [deploy-pages](https://github.com/actions/deploy-pages)
- [ssh-agent](https://github.com/webfactory/ssh-agent) — non utilisé : le thème est public et installé en HTTPS.
- Exemple : [déployer un site avec GitHub Actions et Pages](https://medium.com/@mpaternostro/how-to-deploy-pages-on-github-using-actions-a9281d03b345)

### Jobs du workflow

- `build` : installe les dépendances, lance les vérifications et génère le CV.
- `deploy` : publie sur GitHub Pages après un build réussi sur `main`.
