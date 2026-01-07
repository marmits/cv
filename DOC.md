Générer le CV en local.

### Prérequis
Node 24+ avec Corepack et pnpm.

### Local use
```
corepack enable
corepack prepare pnpm@latest --activate
pnpm install
```

#### Version html
```
npx resumed render resume.json --theme jsonresume-theme-stackoverflowmar
```
#### Version pdf
Fork de : [with-pdf-export](https://github.com/rbardini/resumed/tree/main/examples/with-pdf-export) => [voir index.js](index.js)
```
pnpm start
```

### Commandes rapides
```
pnpm install git+https://github.com/marmits/jsonresume-theme-stackoverflowmar  
npx resumed render resume.json --theme jsonresume-theme-stackoverflowmar
```