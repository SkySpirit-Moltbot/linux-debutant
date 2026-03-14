# Leçon 19 : Sauvegardes et restauration

## Introduction

La sauvegarde (backup) est une pratique essentielle pour protéger vos données. Dans cette leçon, nous apprendrons à créer, gérer et restaurer des sauvegardes sous Linux.

## Pourquoi faire des sauvegardes ?

- Protection contre la perte de données
- Récupération après une erreur de manipulation
- Protection contre les attaques (ransomware)
- Conformité et archivage

## Les outils de sauvegarde

### 1. Commande `tar` pour archiver

La commande `tar` permet de créer des archives.

```bash
# Créer une archive compressée
tar -czvf sauvegarde.tar.gz /home/utilisateur/

# Créer une archive avec la date du jour
tar -czvf sauvegarde-$(date +%Y%m%d).tar.gz /home/utilisateur/
```

### 2. Commande `rsync` pour la synchronisation

`rsync` est idéal pour synchroniser et sauvegarder des fichiers.

```bash
# Synchroniser un répertoire vers un autre
rsync -av /home/utilisateur/ /media/backup/utilisateur/

# Synchronisation en utilisant SSH (sauvegarde distante)
rsync -avz -e ssh /home/utilisateur/ utilisateur@serveur:/backup/

# Options utiles :
# -a : mode archive (préserve les permissions, dates, etc.)
# -v : mode verbeux
# -z : compression pendant le transfert
# --delete : supprime les fichiers absents de la source
# --exclude : exclure des fichiers
rsync -avz --delete --exclude='*.log' /home/ /backup/
```

### 3. Commandes de sauvegarde compressée

```bash
# Avec gzip
tar -czvf backup.tar.gz dossier/

# Avec bzip2 (meilleure compression)
tar -cjvf backup.tar.bz2 dossier/

# Avec xz (meilleure compression)
tar -cJvf backup.tar.xz dossier/

# Lister le contenu d'une archive
tar -tvf backup.tar.gz

# Extraire une archive
tar -xzvf backup.tar.gz
```

## Sauvegarde incrémentale avec `rsync`

Les sauvegardes incrémentales ne copient que les fichiers modifiés.

```bash
# Première sauvegarde complète
rsync -av /home/ /backup/serveur1/full/

# Sauvegardes incrémentales avec horodatage
rsync -av --delete /home/ /backup/serveur1/incr-$(date +%Y%m%d)/
```

## Script de sauvegarde automatique

Voici un exemple de script de sauvegarde :

```bash
#!/bin/bash

# Variables
SOURCE="/home"
DESTINATION="/media/backup"
DATE=$(date +%Y%m%d_%H%M%S)
LOG="/var/log/sauvegarde.log"

# Fonction de journalisation
log() {
    echo "[$(date)] $1" | tee -a $LOG
}

# Début de la sauvegarde
log "Démarrage de la sauvegarde"

# Création du répertoire de sauvegarde
mkdir -p $DESTINATION/backup-$DATE

# Sauvegarde avec rsync
rsync -av --delete --exclude='.cache' $SOURCE/ $DESTINATION/backup-$DATE/ 2>&1 | tee -a $LOG

# Vérification du résultat
if [ $? -eq 0 ]; then
    log "Sauvegarde terminée avec succès"
else
    log "ERREUR lors de la sauvegarde"
fi

# Conserver seulement les 7 dernières sauvegardes
cd $DESTINATION
ls -t | tail -n +8 | xargs -r rm -rf
```

##Restauration de données

```bash
# Restaurer une archive tar
tar -xzvf sauvegarde.tar.gz -C /repertoire/cible/

# Restaurer avec rsync (sens inverse)
rsync -av /backup/utilisateur/ /home/utilisateur/

# Restaurer un fichier spécifique depuis une archive
tar -xzvf sauvegarde.tar.gz -C /tmp chemin/vers/fichier
```

## Bonnes pratiques

1. **Règle 3-2-1** : 3 copies, 2 supports différents, 1 copie hors site
2. **Automatiser** : Planifier les sauvegardes régulières
3. **Vérifier** : Tester régulièrement les restaurations
4. **Chiffrer** : Protéger les sauvegardes sensibles
5. **Surveiller** : Mettre en place des alertes

## Exercice pratique

Créez un script de sauvegarde qui :
1. Sauvegarde votre répertoire personnel vers un dossier `/tmp/backup`
2. Utilise la date dans le nom du fichier
3. Affiche un message de succès ou d'erreur
4. Nettoie les sauvegarde de plus de 7 jours

```bash
#!/bin/bash
# À vous de compléter ce script !
```

### Correction

```bash
#!/bin/bash

SOURCE="$HOME"
DEST="/tmp/backup"
DATE=$(date +%Y%m%d)

mkdir -p $DEST

# Sauvegarde
tar -czf $DEST/sauvegarde-$DATE.tar.gz $SOURCE 2>/dev/null

if [ $? -eq 0 ]; then
    echo "Sauvegarde réussie : sauvegarde-$DATE.tar.gz"
    
    # Nettoyage des anciennes sauvegardes (plus de 7 jours)
    find $DEST -name "sauvegarde-*.tar.gz" -mtime +7 -delete
    echo "Anciennes sauvegardes nettoyées"
else
    echo "ERREUR : La sauvegarde a échoué"
    exit 1
fi
```

## Résumé

- `tar` : créer et extraire des archives compressées
- `rsync` : synchroniser et copier efficacement des fichiers
- Automatiser les sauvegardes avec des scripts cron
- Suivre la règle 3-2-1 pour une protection optimale
- Tester régulièrement la restauration des sauvegardes