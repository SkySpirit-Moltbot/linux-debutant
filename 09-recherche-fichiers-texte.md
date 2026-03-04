# Leçon 9 : Recherche de fichiers et texte

Dans cette leçon, nous allons apprendre à rechercher des fichiers et du texte dans vos fichiers Linux. Ces commandes sont essentielles pour naviguer efficacement dans votre système.

## La commande `find` - Rechercher des fichiers

La commande `find` permet de rechercher des fichiers selon différents critères.

### Rechercher par nom

```bash
find /chemin -name "nom_fichier"
find . -name "*.txt"
```

### Rechercher par type

```bash
find . -type f    # fichiers
find . -type d    # répertoires
```

### Rechercher par taille

```bash
find . -size +100M    # fichiers de plus de 100 Mo
find . -size -1k     # fichiers de moins de 1 Ko
```

## La commande `grep` - Rechercher du texte

`grep` permet de rechercher un mot ou une expression dans des fichiers.

### Recherche simple

```bash
grep "mot" fichier.txt
```

### Recherche récursive

```bash
grep -r "mot" /chemin/
```

### Options utiles

- `-i` : ignorer la casse (majuscules/minuscules)
- `-n` : afficher les numéros de ligne
- `-l` : afficher seulement les noms de fichiers

```bash
grep -inr "erreur" /var/log/
```

## La commande `locate` - Recherche rapide

`locate` utilise une base de données pour trouver rapidement des fichiers.

```bash
locate fichier.txt
```

> Note : Vous pouvez devoir exécuter `sudo updatedb` pour mettre à jour la base de données.

## Exercice pratique

1. Créez un dossier de test avec quelques fichiers :
   ```bash
   mkdir ~/test_recherche
   cd ~/test_recherche
   echo "Bonjour le monde" > bonjour.txt
   echo "Au revoir" > aurevoir.txt
   mkdir sous_dossier
   ```

2. Recherchez tous` dans votre les fichiers `.txt dossier :
   ```bash
   find ~/test_recherche -name "*.txt"
   ```

3. Recherchez le mot "bonjour" dans les fichiers :
   ```bash
   grep -r "bonjour" ~/test_recherche
   ```

4. Utilisez `locate` pour trouver un fichier (après avoir exécuté `updatedb` si nécessaire) :
   ```bash
   sudo updatedb
   locate bonjour.txt
   ```

## Résumé

| Commande | Utilité |
|----------|---------|
| `find` | Rechercher des fichiers selon critères |
| `grep` | Rechercher du texte dans des fichiers |
| `locate` | Recherche rapide via base de données |

Ces trois commandes sont vos alliés quotidiens pour trouver rapidement ce que vous cherchez sur votre système Linux. Combinez-les pour des recherches puissantes !
