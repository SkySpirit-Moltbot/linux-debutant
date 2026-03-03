# Leçon 7 : Gestion des fichiers et dossiers

Dans cette leçon, tu vas apprendre à créer, copier, déplacer et supprimer des fichiers et dossiers sous Linux.

## Créer des fichiers et dossiers

### Créer un dossier
```bash
mkdir mondossier
```

### Créer un fichier vide
```bash
touch mondichier.txt
```

### Créer un fichier avec du texte
```bash
echo "Bonjour" > bonjour.txt
```

## Copier des fichiers

### Copier un fichier
```bash
cp source.txt destination.txt
```

### Copier un dossier et son contenu
```bash
cp -r mondossier copie_mondossier
```

L'option `-r` signifie "récursif" (pour les dossiers).

## Déplacer et renommer

### Déplacer un fichier
```bash
cp fichier.txt /autre/dossier/
```

### Renommer un fichier
```bash
mv ancien_nom.txt nouveau_nom.txt
```

La commande `mv` sert à la fois à déplacer et renommer!

## Supprimer

### Supprimer un fichier
```bash
rm fichier.txt
```

### Supprimer un dossier
```bash
rm -r mondossier
```

**Attention** : La suppression est définitive sous Linux !

### Supprimer un dossier vide
```bash
rmdir mondossier_vide
```

## Exercices pratiques

1. Crée un dossier nommé "test"
2. Dans ce dossier, crée un fichier "note.txt" avec le texte "Ma première note"
3. Copie ce fichier vers "note_copie.txt"
4. Renomme "note_copie.txt" en "backup.txt"
5. Supprime le fichier original "note.txt"
6. Liste le contenu du dossier pour vérifier

**Solution** :
```bash
mkdir test
cd test
echo "Ma première note" > note.txt
cp note.txt note_copie.txt
mv note_copie.txt backup.txt
rm note.txt
ls -la
```

## Résumé

| Commande | Action |
|----------|--------|
| `mkdir` | Créer un dossier |
| `touch` | Créer un fichier vide |
| `cp` | Copier un fichier |
| `cp -r` | Copier un dossier |
| `mv` | Déplacer ou renommer |
| `rm` | Supprimer un fichier |
| `rm -r` | Supprimer un dossier |
| `rmdir` | Supprimer un dossier vide |

Maîtrise ces commandes, elles sont essentielles au quotidien !
