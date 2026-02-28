# Leçon 3 : Les Permissions de Fichiers

## Introduction

Sous Linux, chaque fichier et dossier possède des permissions qui déterminent qui peut le lire, le modifier ou l'exécuter. Comprendre ces permissions est essentiel pour la sécurité et l'administration système.

## Comprendre les Permissions

### Les trois types de permissions

- **r** (read) : Lire le contenu
- **w** (write) : Modifier le contenu
- **x** (execute) : Exécuter le fichier (ou entrer dans un dossier)

### Les trois groupes

- **Propriétaire** (owner) : L'utilisateur qui a créé le fichier
- **Groupe** (group) : Le groupe associé au fichier
- **Autres** (others) : Tous les autres utilisateurs

### Visualiser les permissions

Utilisez la commande `ls -l` pour voir les permissions :

```bash
ls -l /home/utilisateur/fichier.txt
```

Résultat : `-rw-r--r-- 1 utilisateur groupe 1024 Jan 15 10:00 fichier.txt`

| Position | Signification |
|----------|---------------|
| 1er caractère | Type de fichier (- = régulier, d = dossier, l = lien) |
| Caractères 2-4 | Permissions du propriétaire (rw-) |
| Caractères 5-7 | Permissions du groupe (r--) |
| Caractères 8-10 | Permissions des autres (r--) |

## Modifier les Permissions

### Avec chmod en mode symbolique

```bash
# Ajouter la permission d'exécution au propriétaire
chmod u+x script.sh

# Retirer la permission d'écriture au groupe
chmod g-w fichier.txt

# Ajouter la permission de lecture aux autres
chmod o+r document.txt
```

### Avec chmod en mode numérique

Chaque permission reçoit une valeur :
- **r (read)** = 4
- **w (write)** = 2
- **x (execute)** = 1

```bash
# rw-r--r-- (propriétaire:读写, groupe:读, autres:读)
chmod 644 fichier.txt

# rwxr-xr-x (propriétaire:读写执行, groupe:读执行, autres:读执行)
chmod 755 script.sh

# rw------- ( uniquement le propriétaire)
chmod 600 secret.txt
```

## Le Propriétaire et le Groupe

### Changer le propriétaire

```bash
sudo chown utilisateur fichier.txt
```

### Changer le groupe

```bash
sudo chgrp groupe fichier.txt
```

### Changer les deux

```bash
sudo chown utilisateur:groupe fichier.txt
```

## Permissions Spéciales

### Setuid (4000)

Permet à un utilisateur d'exécuter droits un fichier avec les du propriétaire.

```bash
chmod 4755 programme
```

### Setgid (2000)

Les fichiers créés héritent du groupe du dossier.

```bash
chmod 2755 dossier
```

### Sticky Bit (1000)

Sur un dossier, seuls les propriétaires peuvent supprimer leurs fichiers (ex: /tmp).

```bash
chmod 1777 /tmp
```

## Exercice Pratique

1. Créez un fichier nommé `test.txt`
2. Vérifiez ses permissions avec `ls -l`
3. Ajoutez la permission d'exécution au propriétaire
4. Changez les permissions à `rw-r-----` (640)
5. Créez un dossier nommé `mon_dossier` et vérifiez ses permissions

```bash
# Commandes de l'exercice
touch test.txt
ls -l test.txt
chmod u+x test.txt
chmod 640 test.txt
mkdir mon_dossier
ls -ld mon_dossier
```

## Résumé

| Commande | Description |
|----------|-------------|
| `ls -l` | Affiche les permissions |
| `chmod` | Modifie les permissions |
| `chown` | Change le propriétaire |
| `chgrp` | Change le groupe |

Les permissions Linux suivent le principe du moindre privilège : n'accorder que les droits nécessaires pour des raisons de sécurité.
