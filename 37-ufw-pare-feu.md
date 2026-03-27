# Leçon 37 : Sécuriser Linux avec UFW (Pare-feu)

Dans cette leçon, tu vas découvrir comment protéger ton système Linux avec **UFW** (*Uncomplicated Firewall*), un outil simple et efficace pour gérer le pare-feu intégré au noyau Linux (iptables/nftables).

---

## 1. Qu'est-ce qu'un pare-feu ?

Un **pare-feu** (firewall) est un filtrage qui décide quelles connexions réseau sont autorisées ou bloquées. Sans lui, n'importe qui peut se connecter à ton ordinateur. Avec lui, tu contrôle le trafic :

- **Entrant** (IN) : connexions vers ta machine (serveur web, SSH...)
- **Sortant** (OUT) : connexions depuis ta machine (navigation, mises à jour...)

Linux intègre un pare-feu puissant dans le noyau : **iptables** (ou **nftables**). Mais它们的 commandes sont complexes. **UFW**简化了这个过程。

---

## 2. Installer UFW

UFW est généralement **préinstallé** sur Ubuntu. Pour les autres distributions :

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install ufw

# Fedora
sudo dnf install ufw

# Vérifier le statut
sudo ufw status
```

---

## 3. Règles de base

### Voir le statut

```bash
# Statut actuel (actif/inactif, règles en place)
sudo ufw status verbose
```

### Activer / Désactiver le pare-feu

```bash
# Activer UFW (appliquera les règles définies)
sudo ufw enable

# Désactiver UFW (tout ouvert)
sudo ufw disable
```

> ⚠️ **Attention** : si tu te connectes en SSH et que le pare-feu est inactive, assieds-toi d'abord une règle SSH AVANT d'activer UFW !

---

## 4. Autoriser ou Bloquer des Ports

### Autoriser un port (entrant)

```bash
# Autoriser le port 80 (HTTP)
sudo ufw allow 80/tcp

# Autoriser le port 443 (HTTPS)
sudo ufw allow 443/tcp

# Autoriser un service par son nom
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
```

### Bloquer un port (entrant)

```bash
# Bloquer le port 23 (Telnet — non sécurisé)
sudo ufw deny 23/tcp

# Bloquer tout le trafic entrant (très restrictif)
sudo ufw default deny incoming
```

### Autoriser depuis une IP ou un réseau

```bash
# Autoriser une IP spécifique
sudo ufw allow from 192.168.1.100

# Autoriser un réseau entier
sudo ufw allow from 192.168.1.0/24

# Autoriser SSH uniquement depuis le réseau local
sudo ufw allow from 192.168.1.0/24 to any port 22
```

---

## 5. Supprimer une règle

```bash
# Voir les règles avec leurs numéros
sudo ufw status numbered

# Supprimer la règle n°3
sudo ufw delete 3

# Supprimer une règle par sa définition
sudo ufw delete allow 80/tcp
```

---

## 6. Logging (journalisation)

```bash
# Niveau basique
sudo ufw logging low

# Niveau moyen (recommandé)
sudo ufw logging medium

# Désactiver les logs
sudo ufw logging off

# Lire les logs
sudo tail -f /var/log/ufw.log
```

---

## 7. Configuration pour un serveur SSH

C'est le cas le plus courant — on veut sécuriser l'accès SSH :

```bash
# 1. Autoriser SSH (port 22)
sudo ufw allow ssh

# Ou explicitement :
sudo ufw allow 22/tcp

# 2. Si tu te connectes depuis une IP fixe, encore mieux :
sudo ufw allow from 192.168.1.50 to any port 22

# 3. Tout le reste en entrée : refuser
sudo ufw default deny incoming

# 4. Sortie : tout autorisé (par défaut)
sudo ufw default allow outgoing

# 5. Activer le pare-feu
sudo ufw enable
```

---

## 8. Réinitialiser UFW

```bash
# Remettre à zéro (efface toutes les règles)
sudo ufw reset
```

---

## Exercice pratique

**Objectif :** Configure UFW sur ta machine pour un usage bureautique sécurisé.

1. Installe UFW si pas déjà présent
2. Affiche le statut actuel (`sudo ufw status verbose`)
3. Configure ces règles dans l'ordre :
   - `sudo ufw default deny incoming` (tout refuser par défaut)
   - `sudo ufw allow out 53/udp` (DNS sortant)
   - `sudo ufw allow out 80/tcp` (HTTP sortant)
   - `sudo ufw allow out 443/tcp` (HTTPS sortant)
   - `sudo ufw allow ssh` (si tu te connectes en SSH)
   - `sudo ufw allow 22/tcp` (si pas encore fait)
4. Active UFW : `sudo ufw enable`
5. Vérifie : `sudo ufw status numbered`
6. Teste la navigation web (elle doit fonctionner)
7. Lis les logs : `sudo tail -5 /var/log/ufw.log`

---

## Résumé

| Commande | Rôle |
|----------|------|
| `sudo ufw status` | Voir l'état du pare-feu |
| `sudo ufw enable` | Activer UFW |
| `sudo ufw disable` | Désactiver UFW |
| `sudo ufw allow <port>` | Autoriser un port |
| `sudo ufw deny <port>` | Bloquer un port |
| `sudo ufw delete <règle>` | Supprimer une règle |
| `sudo ufw status numbered` | Lister les règles avec numéros |
| `sudo ufw reset` | Réinitialiser UFW |

UFW permet de sécuriser ta machine en quelques commandes. Pour un serveur, pense toujours à autoriser **SSH en premier** avant d'activer le pare-feu.
