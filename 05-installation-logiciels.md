# Leçon 5 : Installation de logiciels

Dans cette leçon, tu vas apprendre comment installer, mettre à jour et supprimer des logiciels sur Linux.

## Introduction

Linux dispose de **gestionnaires de paquets** qui facilitent l'installation de logiciels. Les deux principaux sont :
- **APT** (Debian, Ubuntu, Linux Mint)
- **YUM/DNF** (Fedora, CentOS, RHEL)

Nous allonsfocuser sur APT car c'est le plus utilisé sur les distributions grand public.

## Mettre à jour la liste des paquets

Avant d'installer, il est recommandé de mettre à jour la liste des paquets disponibles :

```bash
sudo apt update
```

> **Note** : `sudo` permet d'exécuter une commande avec les droits administrateur.

## Installer un logiciel

Pour installer un paquet :

```bash
sudo apt install nom_du_paquet
```

**Exemple** : Installer le navigateur Firefox

```bash
sudo apt install firefox
```

## Mettre à jour les logiciels installés

Pour mettre à jour tous les logiciels installés sur ton système :

```bash
sudo apt upgrade
```

Pour une mise à jour complète (qui peut aussi supprimer des paquets obsolètes) :

```bash
sudo apt full-upgrade
```

## Supprimer un logiciel

- **Supprimer le paquet** (garde les fichiers de configuration) :
```bash
sudo apt remove nom_du_paquet
```

- **Supprimer complètement** (y compris la configuration) :
```bash
sudo apt purge nom_du_paquet
```

## Rechercher un paquet

Pour trouver un logiciel dans les dépôts :

```bash
apt search nom_du_paquet
```

**Exemple** : Rechercher vlc

```bash
apt search vlc
```

## Afficher les informations d'un paquet

Pour voir les détails d'un paquet (version, taille, description) :

```bash
apt show nom_du_paquet
```

## Liste des paquets installés

Pour voir tous les paquets installés :

```bash
apt list --installed
```

## Nettoyer le système

- **Supprimer les paquets téléchargés** encore en cache :
```bash
sudo apt clean
```

- **Supprimer les dépendances inutilisées** :
```bash
sudo apt autoremove
```

## Exercice pratique

1. Met à jour la liste des paquets :
   ```bash
   sudo apt update
   ```

2. Recherche le paquet `htop` (un moniteur système) :
   ```bash
   apt search htop
   ```

3. Installe `htop` :
   ```bash
   sudo apt install htop
   ```

4. Lance `htop` pour voir les processus en cours :
   ```bash
   htop
   ```
   (Appuie sur `q` pour quitter)

5. Désinstalle `htop` :
   ```bash
   sudo apt remove htop
   ```

## Résumé

| Commande | Description |
|---------|-------------|
| `sudo apt update` | Met à jour la liste des paquets |
| `sudo apt install <paquet>` | Installe un paquet |
| `sudo apt upgrade` | Met à jour les logiciels |
| `sudo apt remove <paquet>` | Supprime un paquet |
| `apt search <paquet>` | Recherche un paquet |
| `apt show <paquet>` | Affiche les infos d'un paquet |
| `sudo apt autoremove` | Nettoie les dépendances inutilisées |

Dans la prochaine leçon, nous aborderons les **scripts bash** pour automatiser des tâches.
