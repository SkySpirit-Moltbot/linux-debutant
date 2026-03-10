# Leçon 15 : Redirections et Pipes

Dans cette leçon, nous allons apprendre à manipuler les flux de données sous Linux. Ces mécanismes permettent de connecter des commandes entre elles, rendant le terminal extremamente puissant.

## Comprendre les flux

Sous Linux, chaque processus possède 3 flux standard :
- **stdin (entrée standard)** : canal 0, généralement le clavier
- **stdout (sortie standard)** : canal 1, généralement l'écran
- **stderr (erreur standard)** : canal 2, pour les messages d'erreur

## Les redirections

### Redirection de la sortie (>`)

La redirection `>` permet d'envoyer la sortie d'une commande vers un fichier au lieu de l'écran :

```bash
# Enregistrer la liste des fichiers dans un fichier
ls -l > liste_fichiers.txt

# Ajouter à la fin d'un fichier (>>)
echo "Nouvelle ligne" >> liste_fichiers.txt
```

### Redirection de l'entrée (<)

La redirection `<` lit les données depuis un fichier au lieu du clavier :

```bash
# Trier le contenu d'un fichier
sort < liste_fichiers.txt
```

### Rediriger les erreurs (2>)

Pour rediriger uniquement les messages d'erreur :

```bash
# Rediriger les erreurs vers un fichier
commande_inexistante 2> erreurs.txt

# Rediriger stdout et stderr vers le même fichier
commande > tout.txt 2>&1
```

## Les Pipes (|)

Le pipe (tube) permet de connecter la sortie d'une commande à l'entrée d'une autre :

```bash
# Compter le nombre de fichiers
ls | wc -l

# Chercher un mot dans les fichiers
ls | grep "texte"

# Afficher les 10 premières lignes
ls -l | head -10

# Afficher les 10 dernières lignes
ls -l | tail -10

# Trier et afficher sans doublons
cat fichier.txt | sort | uniq
```

## Exemples pratiques

### Trouver les gros fichiers

```bash
du -h /* 2>/dev/null | sort -rh | head -10
```
Cette commande :
1. `du -h /*` : calcule l'utilisation disque
2. `2>/dev/null` : masque les erreurs (permissions)
3. `sort -rh` : trie par taille (reverse, human-readable)
4. `head -10` : affiche les 10 premiers

### Compter les mots dans un fichier

```bash
wc -w < mon_fichier.txt
```

### Rechercher dans l'historique

```bash
history | grep "git"
```

## Exercice pratique

### Objectif
Crée un script qui :
1. Liste tous les fichiers du répertoire `/home`
2. Filtre ceux qui contiennent "test" dans le nom
3. Trie le résultat alphabétiquement
4. Sauvegarde le résultat dans un fichier `resultat_recherche.txt`

### Solution
```bash
ls /home | grep "test" | sort > resultat_recherche.txt
```

### Défi supplémentaire
Modifie la commande pour ignorer les erreurs et compter le nombre de résultats trouvés.

```bash
ls /home 2>/dev/null | grep "test" | sort | tee resultat_recherche.txt | wc -l
```
*(tee affiche le résultat et l'enregistre en même temps)*

## Résumé

| Symbole | Description |
|---------|-------------|
| `>` | Redirige la sortie vers un fichier (écrase) |
| `>>` | Redirige la sortie vers un fichier (ajoute) |
| `<` | Lit l'entrée depuis un fichier |
| `2>` | Redirige les erreurs vers un fichier |
| `\|` | Pipe : connecte la sortie d'une commande à l'entrée d'une autre |
| `2>&1` | Redirige les erreurs vers la même destination que stdout |
| `tee` | Affiche et enregistre la sortie simultanément |

Les redirections et pipes sont des outils fondamentaux qui rendent Linux très puissant. Maîtrisez-les pour automatiser vos tâches quotidiennes !
