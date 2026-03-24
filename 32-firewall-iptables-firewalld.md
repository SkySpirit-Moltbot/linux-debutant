# Leçon 32 : Firewall et filtrage réseau

Dans cette leçon, tu vas découvrir comment protéger ton système Linux avec un pare-feu. Tu apprendras à utiliser **iptables**, **ufw** et **firewalld** pour contrôler le trafic réseau entrant et sortant de ta machine.

---

## 1. Qu'est-ce qu'un firewall ?

Un **firewall** (pare-feu) est un système qui filtre le trafic réseau. Il decide :
- **Ce qui entre** dans ton ordinateur (trafic entrant)
- **Ce qui sort** de ton ordinateur (trafic sortant)

Chaque règle dit "autorise" ou "interdit" un type de connexion selon :
- Le port (ex: 80 pour HTTP, 443 pour HTTPS, 22 pour SSH)
- Le protocole (TCP ou UDP)
- L'adresse IP source ou destination

**Pourquoi c'est important ?**
- Bloquer les connexions non désirées
- Protéger contre les attaques
- Limiter l'accès aux services

---

## 2. iptables - Le classique

**iptables** est l'outil historique de filtrage sur Linux. Il fait partie du noyau Linux.

### Principe de base

```
Paquet → CHAIN → RÈGLE 1 → RÈGLE 2 → ... → ACCEPT/DROP
```

Les chaînes principales :
- **INPUT** : trafic entrant vers ta machine
- **OUTPUT** : trafic sortant de ta machine
- **FORWARD** : trafic qui traverse ta machine (routeur)

### Vérifier l'état

```bash
# Voir toutes les règles actives
sudo iptables -L -n -v

# Avec numérotation des lignes
sudo iptables -L -n --line-numbers

# Voir les règles d'une chaîne précise
sudo iptables -L INPUT -n -v
```

### Autoriser une connexion

```bash
# Autoriser le trafic sur le port 22 (SSH)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Autoriser le trafic sur le port 80 (HTTP)
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Autoriser le trafic sur le port 443 (HTTPS)
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

### Bloquer une connexion

```bash
# Bloquer tout le trafic depuis une IP précise
sudo iptables -A INPUT -s 192.168.1.100 -j DROP

# Bloquer le trafic sur un port
sudo iptables -A INPUT -p tcp --dport 23 -j DROP

# Bloquer les pings (ICMP)
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```

### Supprimer une règle

```bash
# Voir les règles avec leur numéro
sudo iptables -L INPUT --line-numbers

# Supprimer la règle 3 de INPUT
sudo iptables -D INPUT 3
```

### Politique par défaut

```bash
# Mettre DROP par défaut sur INPUT (tout bloquer sauf autorisé)
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP

# Autoriser tout en sortie (moins sécurisé)
sudo iptables -P OUTPUT ACCEPT
```

### Autoriser les connexions établies

```bash
# Permettre les réponses aux connexions sortantes
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

### Sauvegarder les règles

```bash
# Ubuntu/Debian : sauvegarder
sudo iptables-save > /etc/iptables/rules.v4

# Restaurer
sudo iptables-restore < /etc/iptables/rules.v4
```

---

## 3. ufw - Le simple

**ufw** (Uncomplicated Firewall) est une interface simplifiée pour iptables. Parfait pour les débutants.

### Installation

```bash
# Vérifier si installé
which ufw

# Installer (Ubuntu/Debian)
sudo apt install ufw
```

### Activation

```bash
# Activer le firewall
sudo ufw enable

# Désactiver le firewall
sudo ufw disable

# Voir le statut
sudo ufw status verbose
```

### Autoriser / Bloquer par port

```bash
# Autoriser SSH
sudo ufw allow ssh

# Autoriser HTTP
sudo ufw allow 80/tcp

# Autoriser HTTPS
sudo ufw allow 443/tcp

# Autoriser un port UDP
sudo ufw allow 53/udp

# Autoriser une plage de ports
sudo ufw allow 1000:2000/tcp
```

### Bloquer par port

```bash
# Bloquer HTTP
sudo ufw deny 80/tcp

# Bloquer tout depuis une IP
sudo ufw insert 1 deny from 192.168.1.100
```

### Règles par rapport aux applications

```bash
# Lister les applications connues
sudo ufw app list

# Autoriser par nom d'application
sudo ufw allow 'OpenSSH'
sudo ufw allow 'Nginx Full'
```

### Supprimer une règle

```bash
# Par numéro
sudo ufw status numbered
sudo ufw delete 3

# Par règle
sudo ufw delete allow 80/tcp
```

### Règles de base recommandées

```bash
# Configuration initiale sûre
sudo ufw default deny incoming   # Tout refuser par défaut
sudo ufw default allow outgoing  # Tout autoriser en sortie

# Autoriser SSH (IMPORTANT avant d'activer !)
sudo ufw allow 22/tcp

# Activer
sudo ufw enable
```

### Réinitialiser

```bash
# Remettre à zéro
sudo ufw reset
```

---

## 4. firewalld - Le dynamique

**firewalld** est utilisé principalement sur Fedora, RHEL et CentOS. Il supporte les "zones" et les "services".

### Vérifier l'état

```bash
# Voir le statut
sudo systemctl status firewalld

# Vérifier si actif
sudo firewall-cmd --state

# Voir les paramètres actifs
sudo firewall-cmd --list-all
```

### Zones prédéfinies

| Zone | Description |
|------|-------------|
| **drop** | Tout autorisé en sortie, rien en entrée |
| **block** | Tout rejeté avec message ICMP |
| **public** | Pas de confiance, ports ouverts un par un |
| **external** | Pour routeur avec NAT |
| **dmz** | Services limités accessibles |
| **work** | Environnement de travail |
| **home** | Environnement domestique |
| **internal** | Réseau interne de confiance |
| **trusted** | Tout autorisé |

### Commandes de base

```bash
# Voir la zone active
sudo firewall-cmd --get-active-zones

# Voir les services autorisés
sudo firewall-cmd --list-services

# Autoriser un service
sudo firewall-cmd --add-service=http
sudo firewall-cmd --add-service=https

# Autoriser un port
sudo firewall-cmd --add-port=80/tcp
sudo firewall-cmd --add-port=5000-5010/udp

# Supprimer un service
sudo firewall-cmd --remove-service=http

# Supprimer un port
sudo firewall-cmd --remove-port=80/tcp
```

### Rendre les changements permanents

```bash
# Ajouter --permanent pour rendre permanent
sudo firewall-cmd --permanent --add-service=http

# Recharger pour appliquer
sudo firewall-cmd --reload
```

### Créer une zone personnalisée

```bash
# Créer une zone
sudo firewall-cmd --permanent --new-zone=mon-serveur

# Configurer
sudo firewall-cmd --permanent --zone=mon-serveur --add-service=ssh
sudo firewall-cmd --permanent --zone=mon-serveur --add-port=8080/tcp

# Activer
sudo firewall-cmd --change-zone=mon-serveur --zone=mon-serveur
```

### Commandes utiles

```bash
# Lister toutes les zones
sudo firewall-cmd --get-zones

# Voir la zone par défaut
sudo firewall-cmd --get-default-zone

# Changer la zone par défaut
sudo firewall-cmd --set-default-zone=home

# Blocage ICMP (ping)
sudo firewall-cmd --get-icmpsets
sudo firewall-cmd --add-icmp-block=echo-request
```

---

## 5. Comparatif

| Fonction | iptables | ufw | firewalld |
|----------|---------|-----|-----------|
| Distros | Toutes | Debian/Ubuntu | Fedora/RHEL |
| Difficulté | Avancé | Facile | Intermédiaire |
| Fichiers config | /etc/iptables/rules.v4 | /etc/ufw/ | /etc/firewalld/ |
| Commandes temps réel | oui | oui | oui avec --permanent |
| Zones | non | non | oui |

---

## 6. Scénarios courants

### Scénario 1 : Serveur web minimal (ufw)

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp    # SSH
sudo ufw allow 80/tcp   # HTTP
sudo ufw allow 443/tcp  # HTTPS
sudo ufw enable
```

### Scénario 2 : Protéger contre les scans de ports (iptables)

```bash
# Limiter les connexions SSH (anti brute force)
sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --set
sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --update --seconds 60 --hitcount 4 -j DROP

# Limiter le ping
sudo iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 1/s -j ACCEPT
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
```

### Scénario 3 : Autoriser uniquement certaines IPs (firewalld)

```bash
sudo firewall-cmd --permanent --add-source=192.168.1.0/24
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

---

## 7. Bonnes pratiques

- **Configure avant d'activer** : toujours autoriser SSH (port 22) AVANT d'activer le firewall
- **Politique par défaut restrictive** : refuse tout par défaut, autorise uniquement le nécessaire
- **Log les paquets rejetés** : aide au diagnostic d'attaques
- **Teste après configuration** : vérifie que les services fonctionnent
- **Sauvegarde tes règles** : copie les fichiers de configuration

---

## 8. Résumé des commandes

| Commande | Description |
|----------|-------------|
| `iptables -L -n -v` | Lister les règles iptables |
| `iptables -A INPUT -p tcp --dport 22 -j ACCEPT` | Autoriser SSH |
| `iptables -A INPUT -s IP -j DROP` | Bloquer une IP |
| `ufw status` | Voir le statut ufw |
| `ufw allow 80/tcp` | Autoriser HTTP |
| `ufw deny 22/tcp` | Bloquer SSH |
| `ufw enable` | Activer ufw |
| `firewall-cmd --list-all` | Voir config firewalld |
| `firewall-cmd --add-service=http` | Ajouter service |
| `firewall-cmd --reload` | Recharger firewalld |

---

## 9. Exercice pratique

### Exercice : Configure ton firewall

**Objectif** : Apprendre à protéger une machine avec ufw.

**Prérequis** : Une distribution Ubuntu/Debian ou VM de test.

**Étape 1 : Vérifie l'état actuel**

```bash
sudo ufw status
```

**Étape 2 : Configure les règles de base**

```bash
# Règles par défaut
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Autorise SSH (important !)
sudo ufw allow 22/tcp

# Autorise HTTP et HTTPS si serveur web
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Autorise ping (optionnel)
sudo ufw allow proto icmp from any to any
```

**Étape 3 : Active le firewall**

```bash
sudo ufw enable
sudo ufw status verbose
```

**Étape 4 : Teste la connectivité**

```bash
# Depuis une autre machine, teste SSH
ssh user@ton-serveur

# Teste que HTTP est accessible
curl http://localhost
```

**Étape 5 : Ajoute une règle pour bloquer une IP**

```bash
# Bloque une IP spécifique
sudo ufw insert 1 deny from 203.0.113.50 to any
sudo ufw status numbered
```

**Étape 6 : Supprime une règle**

```bash
# Supprime la règle 3
sudo ufw delete 3
```

**Étape 7 : Désactive pour tester**

```bash
sudo ufw disable
# Teste tes services...
sudo ufw enable
```

✅ Tu sais maintenant configurer un pare-feu sur Linux !

---

## 10. Aller plus loin

- **fail2ban** : outil qui bannit automatiquement les IPs après plusieurs échecs de connexion
- **nftables** :新一代 de iptables, plus simple et performant
- **WAF (Web Application Firewall)** : filtrage applicatif pour protéger les sites web
- **Suricata / Snort** : systèmes de détection d'intrusion (IDS)
