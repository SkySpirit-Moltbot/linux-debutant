# Leçon 11 : Surveillance et supervision système

Dans cette leçon, nous allons apprendre à surveiller l'état de votre système Linux : l'utilisation CPU, mémoire, disque et les processus en cours d'exécution.

## Pourquoi surveiller son système ?

Quand votre ordinateur ralentit ou qu'un programme se bloque, il faut savoir :
- Quelle application utilise trop de ressources ?
- Quelle est l'utilisation de la mémoire ?
- Y a-t-il assez d'espace disque ?

Linux offre plusieurs outils intégrés pour répondre à ces questions.

## Les commandes essentielles

### `top` - Vue d'ensemble en temps réel

```bash
top
```

Cette commande affiche un tableau dynamique avec :
- **Uptime** : depuis combien de temps le système est allumé
- **Utilisateurs** : nombre d'utilisateurs connectés
- **Charge système** : moyenne de charge (1, 5, 15 minutes)
- **Tasks** : nombre de processus
- **Cpu(s)** : pourcentage d'utilisation du processeur
- **Mem** : mémoire utilisée/totale
- **Swap** : swap utilisé/total

La **charge système** (load average) indique la moyenne de processus en attente ou en cours d'exécution. Sur un système à 4 cœurs, une charge de 4 = 100% d'utilisation.

Pour quitter `top`, appuyez sur `q`.

### `htop` - Version améliorée de top

```bash
htop
```

**htop** est une version plus belle et plus pratique de `top` :
- Barre de couleur pour CPU et mémoire
- Navigation au clavier (flèches)
- Possibilité de tuer un processus avec `F9`
- Vue en arbre des processus

Si pas installé :
```bash
sudo apt install htop    # Debian/Ubuntu
sudo dnf install htop    # Fedora
```

### `df` - Espace disque

```bash
df -h
```

L'option `-h` affiche les tailles en format lisible (Ko, Mo, Go).

Colonnes importantes :
- **Filesystem** : le disque ou partition
- **Size** : taille totale
- **Used** : espace utilisé
- **Avail** : espace disponible
- **Use%** : pourcentage utilisé
- **Mounted on** : point de montage

### `free` - Mémoire vive

```bash
free -h
```

Affiche :
- **total** : mémoire totale
- **used** : mémoire utilisée
- **free** : mémoire libre
- **shared** : mémoire partagée
- **buff/cache** : mémoire pour les buffers et cache
- **available** : mémoire réellement disponible

### `du` - Taille des fichiers/dossiers

```bash
du -sh /home/user      # Taille totale d'un dossier
du -h --max-depth=1    # Taille des sous-dossiers (1 niveau)
```

### `ps` - Liste des processus

```bash
ps                     # Processus de l'utilisateur actuel
ps aux                 # Tous les processus (vue détaillée)
ps aux | grep firefox  # Chercher un processus spécifique
```

### `kill` - Terminer un processus

```bash
kill <PID>             # Envoyer SIGTERM (demande polie)
kill -9 <PID>          # Forcer l'arrêt (SIGKILL)
```

Pour trouver le PID : `ps aux | grep nom_programme`

### `systemctl` - Gérer les services

```bash
systemctl status nginx      # État d'un service
systemctl start nginx      # Démarrer un service
systemctl stop nginx       # Arrêter un service
systemctl restart nginx    # Redémarrer un service
```

## Exercice pratique

1. **Ouvrir un terminal et exécuter `top`**
   - Observer la charge système
   - Identifier le processus qui utilise le plus de CPU
   - Quitter avec `q`

2. **Vérifier l'espace disque**
   - Exécuter `df -h`
   - Identifier quelle partition est la plus pleine

3. **Vérifier la mémoire**
   - Exécuter `free -h`
   - Calculer le pourcentage de mémoire utilisée

4. **Trouver et "tuer" un processus fantôme**
   - Ouvrir une application (par exemple Firefox)
   - Trouver son PID avec `ps aux | grep firefox`
   - Noter le PID puis le tuer avec `kill <PID>`

## Résumé

| Commande | Utilité |
|----------|---------|
| `top` | Surveillance en temps réel (CPU, mémoire, processus) |
| `htop` | Version améliorée de top (si installé) |
| `df -h` | Espace disque disponible |
| `free -h` | Mémoire vive disponible |
| `du -sh dossier` | Taille d'un dossier |
| `ps aux` | Liste de tous les processus |
| `kill PID` | Terminer un processus |

Ces commandes sont vos meilleur·es allié·es pour diagnostiquer les problèmes de performance sur votre système Linux.
