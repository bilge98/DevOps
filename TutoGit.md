créer un dossier
```mkdir nomdossier
cd nomdossier```

```git config --```

```git init```

```touch README.md```

Vérifie les fichiers suivient ou non, 
```git status```

le fichier est pas stage donc on l'ajoute
```git add README.md```
( pour tous les fichiers git add .)

on vérifie que l'ajout est bien pris en compte
```git status ```

on commit les ajouts avec un mesage
```git commit -m 'Inital commit'```

on regarde l'historique de tous les commit de la branche sélectionnée
```git log```

on cré un alias de git log avec options sur git tree :
```git config --global alias.tree "log --graph --decorate --pretty=online --abbrev-commit --all --full-history"```

on le test 
```git tree```

on repertorie dans un fichier caché .gitignore les nom de fichier a ignorer par git(par exemple les config locale, les logs..)
```touch .gitignore```
*astuce : le site gitignore.io indique tous les éléments à ignorer pour chaque type de langage (maven, java etc...), il suffit
de chercher les extension de fichiers à ignorer et copier la réponse dans son gitignore*

on se met sur master au cas ou le head est détaché et on crée une branch appelé feature
```git checkout master 
git branch feature
git checkout feature
git tree```

on cré un fichier 
```touch src/feature.java```

on écris dedans
```echo 'feature' > src/feature.java```

on add et commit en une commande
```commit -am 'inital commit branch feature'```

on change de branche et on vérifie
```git checkout master
git tree```

*astuce: indexation = stage/unstage pour git*

pour selectionner les modification une par une
```git add . -i```

pour unstage tout un fichier
```git reset head .```

Pour merge (fusionner) on se met sur une banche et on récupère les modifications d'une autre, on peut merge dans les deux sens le résultat dans le code sera le même
```git checkout master
git merge feature```

Pour revenir un commit avant depuis le head
```git reset (--hard) HEAD~1```

Le rebase consiste à récupérer les modif de master sans merge ( ca place la feature au dessus de master dans l'arbre)

