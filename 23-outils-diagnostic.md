# Leçon 23 : Outils de diagnostic et dépannage

## Introduction

Lorsque votre système Linux rencontre des problèmes, il est essentiel de savoir diagnostiquer la cause du problème. Cette leçon présente les outils les plus utiles pour identifier et résoudre les problèmes courants sur un système Linux.

## Les commandes de diagnostic essentielles

### top et htop - Surveillance des processus

La commande `top` affiche les processus en cours d'exécution en temps réel :

```bash
top
```

Pour une interface plus conviviale, utilisez `htop` :

```bash
htop
```

Dans `top` ou `htop`, vous pouvez voir :
- L'utilisation CPU par processus
- L'utilisation mémoire
- Le temps d'exécution
- L'utilisateur propriétaire

### free - Mémoire vive

Affiche la mémoire RAM disponible et utilisée :

```bash
free -h
```

L'option `-h` rend l'affichage lisible (Ko, Mo, Go).

### df - Espace disque

Vérifie l'espace disque disponible :

```bash
df -h
```

L'option `-h` affiche les tailles en format lisible.

### du - Utilisation de l'espace par fichier/répertoire

```bash
du -sh /home/*        # Taille de chaque répertoire dans /home
du -h --max-depth=1  # Tailles des répertoires courants
```

### lsblk - Liste des partitions

Affiche les partitions disponibles :

```bash
lsblk
```

## Outils de réseau

### ping - Tester la connectivité

```bash
ping google.com
ping -c 4 google.com  # Arrêt après 4 paquets
```

### ip ou ifconfig - Informations réseau

```bash
ip addr show
ip route show
```

### netstat ou ss - Connexions réseau

```bash
ss -tuln    # Connexions en écoute
netstat -tuln
```

### curl et wget - Tester un serveur web

```bash
curl -I https://google.com
wget -qO- https://google.com
```

## Journalisation et logs

### journalctl - Logs systemd

```bash
journalctl -xe          # Logs récents avec détails
journalctl -u nginx     # Logs d'un service spécifique
journalctl --since "1 hour ago"
journalctl -p err       # Logs de niveau erreur
```

### /var/log - Fichiers de logs traditionnels

```bash
/var/log/syslog    # Messages système
/var/log/auth.log # Connexions et authentifications
/var/log/kern.log # Messages du noyau
```

### dmesg - Messages du noyau

```bash
dmesg | less
dmesg | grep -i error
```

## Outils de diagnostic avancés

### strace - Suivre les appels système

```bash
strace -e open ls /home
strace -p <pid>  # Suivre un processus spécifique
```

### lsof - Fichiers ouverts

```bash
lsof              # Tous les fichiers ouverts
lsof -i :80       # Fichiers utilisant le port 80
lsof -u <user>    # Fichiers d'un utilisateur
```

### ps et pgrep - Recherche de processus

```bash
ps aux | grep nginx
pgrep -a nginx
```

### kill et killall - Terminer un processus

```bash
kill <PID>           # Terminer normalement
kill -9 <PID>        # Forcer la terminaison
killall nginx        # Terminer tous les processus nginx
```

## Résumé des commandes

| Commande | Utilité |
|----------|---------|
| `top` / `htop` | Surveillance des processus en temps réel |
| `free -h` | Mémoire RAM disponible |
| `df -h` | Espace disque disponible |
| `ping` | Tester la connectivité réseau |
| `ip addr` | Informations sur les interfaces réseau |
| `ss -tuln` | Connexions réseau actives |
| `journalctl` | Logs systemd |
| `dmesg` | Messages du noyau |
| `lsof` | Fichiers ouverts |
| `strace` | Suivre les appels système |

## Exercice pratique

1. Vérifiez l'utilisation de la mémoire avec `free -h`
2. Vérifiez l'espace disque avec `df -h`
3. Testez votre connexion internet avec `ping -c 3 google.com`
4. Affichez les processus les plus consommateurs avec `top` (ou `htop`)
5. Trouvez quels processus utilisent le port 80 avec `ss -tuln | grep :80`
6. Consultez les logs récents de votre système avec `journalctl -n 20`

## Conclusion

Ces outils de diagnostic sont essentiels pour tout administrateur Linux. La maîtrise de ces commandes vous permettra d'identifier rapidement la source des problèmes et de les résoudre efficacement. N'hésitez pas à consulter les pages de manuel (`man <commande>`) pour en savoir plus sur chaque outil.