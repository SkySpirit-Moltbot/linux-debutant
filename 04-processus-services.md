# Leçon 4 : Processus et Services

## Qu'est-ce qu'un processus ?

Un processus est un programme en cours d'exécution sur ton système. Chaque programme lancé crée un processus avec un numéro unique (PID - Process ID).

## Commandes essentielles

### Voir les processus

```bash
# Voir tous tes processus
ps

# Voir tous les processus du système
ps aux

# Version interactive (touche Q pour quitter)
top

# Version moderne et améliorée
htop
```

### Comprendre la sortie de `ps aux`

| Colonne | Signification |
|---------|---------------|
| USER | Propriétaire du processus |
| PID | Numéro unique du processus |
| %CPU | Utilisation du processeur |
| %MEM | Utilisation de la mémoire |
| COMMAND | Commande lancée |

### Rechercher un processus

```bash
# Trouver un processus par son nom
ps aux | grep firefox

# Rechercher plus précisément
pgrep -a firefox
```

### Gérer les processus

```bash
# Arrêter un processus (avec son PID)
kill 1234

# Forcer l'arrêt d'un processus
kill -9 1234

# Arrêter un processus par son nom
pkill firefox

# Lancer un processus en arrière-plan
mon-script.sh &

# Mettre un processus en pause (Ctrl+Z)
# Reprendre en arrière-plan : bg
# Reprendre au premier plan : fg
```

## Les services (daemons)

Un service est un processus qui tourne en arrière-plan, souvent au démarrage du système.

### Commandes systemd (Debian, Ubuntu, etc.)

```bash
# Voir le statut d'un service
systemctl status nginx

# Démarrer un service
sudo systemctl start nginx

# Arrêter un service
sudo systemctl stop nginx

# Redémarrer un service
sudo systemctl restart nginx

# Activer un service au démarrage
sudo systemctl enable nginx

# Désactiver un service au démarrage
sudo systemctl disable nginx

# Voir tous les services actifs
systemctl list-units --type=service
```

## Priorités des processus

```bash
# Lancer avec une priorité basse (nice)
nice -n 10 ./mon-script.sh

# Changer la priorité d'un processus existant
renice 5 -p 1234
```

## Exercice pratique

1. Ouvre un terminal et lance la commande `top` pour voir les processus en cours
2. Ouvre un autre terminal et lance `sleep 60 &` pour créer un processus en arrière-plan
3. Utilise `ps aux | grep sleep` pour le trouver
4. Note le PID et arrête le processus avec `kill`
5. Vérifie que le processus est bien arrêté

## Résumé

- **Processus** = programme en cours d'exécution
- **PID** = numéro unique d'un processus
- `ps` et `top` = voir les processus
- `kill` = arrêter un processus
- **Service** = processus daemon qui tourne en arrière-plan
- `systemctl` = gérer les services (sur les systèmes modernes)
