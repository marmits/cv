## PACKAGE NPM EXTERNE

### INSTALL
npm install git+https://github.com/marmits/jsonresume-theme-stackoverflowmar  
npx resumed render resume.json --theme jsonresume-theme-stackoverflowmar

### EN MODE LOCAL
voir [branch dev](https://github.com/marmits/cv/blob/dev/DEV.md)

## GIHUB actions (workflows)
### SSH
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