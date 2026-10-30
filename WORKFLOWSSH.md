# Dépendance au thème et workflow

`cvmarmits` utilise le thème `v2.0.3`, installé depuis GitHub. Pour modifier le thème localement sans toucher au manifeste de l’application, utilise `npm link` depuis les deux dépôts ; les commandes sont dans [DOC.md](DOC.md). Le workflow convertit l’URL SSH du verrou en HTTPS ; aucune clé SSH n’est nécessaire.

En local, le projet utilise le conteneur NodeTools. GitHub Actions s’exécute directement sur `ubuntu-latest` avec Node.js 26 LTS, sans Docker ni pnpm. Le thème `v2.0.3` n’exécute pas de script `prepare` dépendant de pnpm.

Le workflow lance `npm ci`, puis `npm run build` pour générer `index.html` et `resume.pdf`. Il publie également `resume.json` sur GitHub Pages. Seuls les pushes vers `main` déclenchent le workflow et publient le site.
