## MODE DEVELOPPEMENT
sur la branche dev

## module externe
#### Avec un référenciel VCS:  
```
npm install git+https://github.com/marmits/jsonresume-theme-stackoverflowmar
npx resumed render resume.json --theme jsonresume-theme-stackoverflowmar
```

#### Avec un référenciel LOCAL:
1. Dans le module jsonresume-theme-stackoverflowmar :  
   `npm link`

   >Exécuter cette commande crée un lien symbolique pour votre paquet,   
   à l’intérieur du dossier global node_modules (le même dossier où vos dépendances npm globales ont été installées).  
   Donc maintenant vous pouvez utiliser le paquet npm que vous avez créé à l’intérieur de n’importe quel projet.

2. Dans le projet:  
   `npm link jsonresume-theme-stackoverflowmar`
   >Lorsque vous exécutez npm link <module_name> dans le répertoire d'un projet, npm crée un lien symbolique de ./node_modules/<module_name> vers <global_node_modules>/<module_name>.

3. Installation  
   `npm install`

4. Check  
   `npm ls`

5. Exécution  
   `npx resumed render resume.json --theme jsonresume-theme-stackoverflowmar`



#### Bonus delete:
1. Dans le module:  
   `npm unlink jsonresume-theme-stackoverflowmar`
2. Dans le projet:  
   `npm unlink --no-save jsonresume-theme-stackoverflowmar`
details:
>- npm unlink --no-save <module_name> on your project’s directory to remove the local symlink, and run
>- npm unlink <module_name> on the module’s directory to remove the global symlink.



#### package.json (old version):
```
{
    "dependencies": {
        "jsonresume-theme-stackoverflowmar": "file:../Bundles/Marmits/jsonresume-theme-stackoverflowmar",
        "resumed": "^3.0.1"
    }
}
```

#### JSON Resume PDF (old version):
```
npm install -g resume-cli
resume export resume.pdf --theme=jsonresume-theme-stackoverflowmar
```

#### source
[@alexishevia](https://medium.com/@alexishevia/the-magic-behind-npm-link-d94dcb3a81af)