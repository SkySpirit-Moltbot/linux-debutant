# Leçon 10 : Variables d'environnement et scripts Bash

## Introduction

Les variables d'environnement sont des valeurs dynamiques que le système utilise pour configurer le comportement des programmes. Les scripts Bash permettent d'automatiser des tâches en combinant plusieurs commandes.

## Variables d'environnement

### Qu'est-ce que c'est ?

Une variable d'environnement est une paire nom=valeur que le système et les programmes utilisent pour savoir comment se comporter.

### Commandes utiles

```bash
# Voir toutes les variables d'environnement
env

# Voir une variable spécifique
echo $HOME
echo $PATH
echo $USER

# Créer une variable (session actuelle uniquement)
MA_VARIABLE="Bonjour"

# Utiliser une variable
echo $MA_VARIABLE

# Exporter une variable (disponible pour les sous-processus)
export MA_VARIABLE
```

### Variables importantes

- `$HOME` : Répertoire personnel de l'utilisateur
- `$PATH` : Liste des répertoires où chercher les commandes
- `$USER` : Nom de l'utilisateur actuel
- `$PWD` : Répertoire de travail actuel
- `$SHELL` : Interpréteur de commandes utilisé

## Scripts Bash

### Créer un script

Un script Bash est un fichier texte contenant des commandes à exécuter.

```bash
# Créer un script
nano mon_script.sh
```

### Structure de base

```bash
#!/bin/bash

# Commentaire : ce script fait quelque chose
echo "Hello World !"
```

### Rendre le script exécutable

```bash
chmod +x mon_script.sh
./mon_script.sh
```

### Variables dans les scripts

```bash
#!/bin/bash

NOM="David"
echo "Bonjour $NOM !"

# Lire une entrée utilisateur
read -p "Ton prénom : " prenom
echo "Salut $prenom !"
```

### Conditions

```bash
#!/bin/bash

if [ $1 -gt 10 ]; then
    echo "Le nombre est plus grand que 10"
else
    echo "Le nombre est 10 ou moins"
fi
```

### Boucles

```bash
# Boucle for
for i in 1 2 3 4 5; do
    echo "Compteur: $i"
done

# Boucle while
compteur=1
while [ $compteur -le 3 ]; do
    echo "Compteur: $compteur"
    compteur=$((compteur + 1))
done
```

## Exercice pratique

1. Affiche ta variable HOME
2. Crée une variable MON_REPERTOIRE contenant le chemin de ton dossier personnel
3. Crée un script `bienvenue.sh` qui :
   - Demande le nom de l'utilisateur
   - Affiche "Bienvenue, [nom] !" avec le nom entré

## Résumé

- Les variables d'environnement configurent le système
- `$NAME` permet d'accéder à une variable
- `export` rend une variable disponible aux sous-processus
- Les scripts Bash automatisent des tâches
- `chmod +x` rend un script exécutable
- `#!` indique l'interpréteur à utiliser
