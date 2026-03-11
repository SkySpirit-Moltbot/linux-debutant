# Leçon 16 : Éditeurs de texte en ligne de commande

## Introduction

Sous Linux, vous serez souvent amenés à modifier des fichiers de configuration ou des scripts. Deux éditeurs sont disponibles directement dans le terminal : **Nano** (simple) et **Vim** (puissant).

---

## Nano : l'éditeur simple

Nano est idéal pour les débutants. Il affiche les raccourcis en bas de l'écran.

### Ouvrir/créer un fichier

```bash
nano fichier.txt
```

### Raccourcis utiles

- **Ctrl + O** : Enregistrer (WriteOut)
- **Ctrl + X** : Quitter
- **Ctrl + W** : Rechercher
- **Ctrl + K** : Couper la ligne
- **Ctrl + U** : Coller
- **Ctrl + G** : Aide

---

## Vim : l'éditeur puissant

Vim est plus complexe mais très puissant. Il fonctionne avec des **modes** — c'est la principale différence avec Nano.

### Les 3 modes principaux

| Mode | Description | Comment y entrer |
|------|-------------|------------------|
| **Normal** | Navigation et commandes | Par défaut (Échap) |
| **Insertion** | Écrire du texte | Appuie sur `i` |
| **Commande** | Commandes avancées | Appuie sur `:` |

### Comprendre les modes

1. **Quand tu ouvres Vim** : Tu es en mode **Normal**
   - Tu ne peux PAS écrire directement
   - Utilise les touches de direction pour naviguer
   - Les lettres font des commandes !

2. **Pour écrire** : Appuie sur `i` → mode **Insertion**
   - Maintenant tu peux taper du texte
   - Appuie sur **Échap** pour revenir au mode Normal

3. **Pour exécuter des commandes** : En mode Normal, appuie sur `:` → mode **Commande**
   - Puis tape la commande et Entrée

### Commandes essentielles (mode Normal)

#### Naviguer
```bash
h j k l        # Gauche, Bas, Haut, Droite (ou flèches)
w              # Mot suivant
b              # Mot précédent
0              # Début de ligne
$              # Fin de ligne
gg             # Début du fichier
G              # Fin du fichier
```

#### Éditer
```bash
i              # Insérer (passer en mode insertion)
a              # Ajouter après le curseur
A              # Ajouter en fin de ligne
o              # Nouvelle ligne dessous
O              # Nouvelle ligne dessus
x              # Supprimer un caractère
dd             # Supprimer une ligne entière
dw             # Supprimer un mot
u              # Annuler (undo)
Ctrl + r       # Rétablir (redo)
```

#### Copier/Coller
```bash
yy             # Copier une ligne (yank)
yw             # Copier un mot
p              # Coller après le curseur
P              # Coller avant le curseur
```

#### Rechercher et remplacer
```bash
/string        # Rechercher "string" vers le bas
n              # Occurrence suivante
N              # Occurrence précédente
:%s/old/new/g  # Remplacer "old" par "new" partout
```

### Commandes de sauvegarde (mode Commande)

Échap pour revenir au mode Normal, puis :

```bash
:w             # Enregistrer (write)
:wq            # Enregistrer et quitter
:x             # Enregistrer et quitter (plus court)
:q             # Quitter (si pas de modification)
:q!            # Quitter sans enregistrer
:w!            # Forcer l'enregistrement
```

### Exemple pratique

```bash
# 1. Ouvrir un fichier
vim fichier.txt

# 2. Passer en mode insertion (pour écrire)
i
# Écrire du texte...

# 3. Revenir au mode normal
Échap

# 4. Enregistrer et quitter
:wq
```

### Vimtutor

Pour pratiquer Vim gratuitement :

```bash
vimtutor
```

C'est un tutorial interactif de 30 minutes прямо dans le terminal !

---

## Nano vs Vim : que choisir ?

| Situation | Éditeur recommandé |
|-----------|-------------------|
| Modifier un fichier de config simple | Nano |
| Débutant en Linux | Nano |
| Éditer de gros fichiers | Vim |
| Programmation avancée | Vim |
| Travailler sur serveur distant | Vim |

---

## Exercice pratique

### Niveau 1 : Nano
1. Crée un fichier `test.txt` avec Nano
2. Écris quelques lignes
3. Sauvegarde avec Ctrl+O, quitte avec Ctrl+X

### Niveau 2 : Vim
1. Ouvre le fichier avec Vim : `vim test.txt`
2. Appuie sur `i` pour écrire, tape du texte
3. Appuie sur Échap pour revenir au mode normal
4. Tape `:wq` pour sauvegarder et quitter
5. Lance `vimtutor` et fais les 2 premières leçons

---

## Résumé

- **Nano** : éditeur simple, parfait pour les débutants, tous les raccourcis affichés
- **Vim** : éditeur puissant avec modes, nécessite un peu d'apprentissage
- La clé de Vim : comprendre les modes Normal / Insertion / Commande
- `vimtutor` est ton ami pour apprendre Vim !
