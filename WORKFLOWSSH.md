## WorkFlow étape SSH

***

Gestion de la dépendance GitHub et authentification SSH dans GitHub Actions

### 🎯 Contexte

Ce projet utilise une dépendance installée **depuis un dépôt GitHub** :

    "jsonresume-theme-stackoverflowmar": "github:marmits/jsonresume-theme-stackoverflowmar#v2.0.2"

Lorsque GitHub Actions exécute `npm install`, il doit **cloner le dépôt Git** du thème.  
Selon que ce dépôt est **public ou privé**, Git peut nécessiter une authentification.

***

### 🔐 Pourquoi une étape SSH peut être nécessaire ?

Si la dépendance GitHub est **privée** ou si elle utilise une URL de type :

    git@github.com:...

alors GitHub Actions doit utiliser une **clé SSH privée** pour accéder au dépôt.

Dans ce cas, il faut activer l’étape suivante :

```yaml
steps:
    - name: Authenticate SSH
      uses: webfactory/ssh-agent@v0.9.0
      with:
        ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}
```

Cette étape permet à GitHub Actions de :

1.  Lancer un **agent SSH**
2.  Charger la clé privée stockée dans `secrets.SSH_PRIVATE_KEY`
3.  Autoriser npm à cloner la dépendance Git via SSH

Sans cette étape, `npm install` échouera avec une erreur du type :

    Permission denied (publickey)
    Could not read from remote repository

***

### 🟢 Quand **ne pas** activer l’étape SSH ?

#### ✔️ Si le dépôt GitHub du thème est **public**

Dans ce cas, aucune authentification n’est nécessaire.  
GitHub Actions peut cloner le dépôt sans clé privée.

#### ✔️ Si la dépendance utilise une URL **HTTPS**

Par exemple :

    https://github.com/marmits/jsonresume-theme-stackoverflowmar.git

Là encore, **pas besoin de clé SSH**.  
Si le dépôt est privé, on utilisera plutôt un token GitHub (GITHUB\_TOKEN ou PAT), pas SSH.

***

### 🔍 Résumé simple

| Situation                                     | SSH nécessaire ?                               |
| --------------------------------------------- | ---------------------------------------------- |
| Dépendance GitHub **publique**                | ❌ Non                                          |
| Dépendance GitHub **privée**                  | ✅ Oui                                          |
| Dépendance utilisant `git@github.com:`        | ✅ Oui                                          |
| Dépendance utilisant `https://github.com/...` | ❌ sauf si privé (on utilise un token, pas SSH) |

### 🎯 Résumé 

| Syntaxe dans `package.json`  | Protocole utilisé | Besoin de SSH ? |
| ---------------------------- | ----------------- | --------------- |
| `"github:user/repo"`         | HTTPS             | ❌ Non           |
| `"git+https://..."`          | HTTPS             | ❌ Non           |
| `"git@github.com:user/repo"` | SSH               | ✅ Oui           |
| `"git+ssh://git@github.com"` | SSH               | ✅ Oui           |


***

### 💡 Bonnes pratiques

*   Utiliser SSH uniquement si tu **as réellement besoin d’un accès privé**.
*   Préférer **HTTPS + token GitHub** pour simplifier la CI.
*   Stocker la clé privée uniquement dans `Settings > Secrets > Actions`.

***


### INFOS SUPPLÉMENTAIRES 
Lorsque vous exécutez un workflow GitHub Action pour préparer votre projet, exécuter des tests ou créer des images,  
vous devrez peut-être récupérer des bibliothèques ou des fournisseurs supplémentaires à partir de référentiels privés.  
Les actions GitHub n'ont accès qu'au référentiel pour lequel elles s'exécutent.  
Ainsi, afin d'accéder à des référentiels privés supplémentaires, créez une clé SSH avec des privilèges d'accès suffisants.   
Ensuite, utilisez cette action pour rendre la clé disponible avec ssh-agent sur le nœud Worker Action.   
Une fois cela configuré, les commandes git clone utilisant les URL ssh fonctionneront simplement.   
De plus, l'exécution de commandes ssh pour se connecter à d'autres serveurs pourra utiliser la clé.  
source: [github webfactory-ssh-agen](https://github.com/marketplace/actions/webfactory-ssh-agent)

1. Générer les clés
   $ ssh-keygen -t ed25519 -C "email@domain.com"

2. Copier la clé publique: dans le référenciel que l'on veut utliser:  
   settings->Deploy keys->add deploy key:
   ``` 
    ssh-ed25519 sfdsfds email@domain.com
   ```

3. Copier la clé privée dans le référenciel qui l'installe  
   settings->secrets and variables->Actions->Secrets->New repository secret
   SSH_PRIVATE_KEY =>
   ```
   -----BEGIN OPENSSH PRIVATE KEY-----
     dfdfdff
   -----END OPENSSH PRIVATE KEY-----
   ```

4. Dans le workflow
```
steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: authentification
        uses: webfactory/ssh-agent@v0.9.0
        with:
          ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

      - name: Setup Node.js
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node }}

      - name: Install
        run: npm ci

      - name: Test
        run: npm test
```