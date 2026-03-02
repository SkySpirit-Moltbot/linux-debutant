# Leçon 6 : Réseaux de base

## Introduction

Quand tu utilises Linux, il est souvent utile de comprendre comment ton ordinateur communicate sur le réseau. Cette leçon te donnera les bases pour configurer et tester ta connexion réseau.

---

## Comprendre les adresses IP

### Qu'est-ce qu'une adresse IP ?

Une **adresse IP** (Internet Protocol) c'est comme l'adresse postale de ton ordinateur sur le réseau. Elle permet aux autres appareils de communiquer avec le tien.

Il existe deux types d'adresses IP :

| Type | Description | Exemple |
|------|-------------|---------|
| **IPv4** | Version la plus courante | 192.168.1.100 |
| **IPv6** | Nouvelle version (plus d'adresses) | 2001:0db8::1 |

### IP publique vs IP privée

- **IP publique** : L'adresse de ton réseau sur Internet (celle que les sites web voient)
- **IP privée** : L'adresse de ton appareil dans ton réseau local (192.168.x.x)

---

## Commandes réseau essentielles

### Voir son adresse IP

```bash
# IP de ta carte réseau (méthode moderne)
ip addr

# IP externe (celle vue sur Internet)
curl ifconfig.me

# IP de ta carte réseau (méthode classique)
ifconfig
```

### Tester la connexion

```bash
# Tester si un serveur est joignable
ping google.com

# Nombre de sauts jusqu'à un serveur
traceroute google.com

# Voir les serveurs DNS utilisés
cat /etc/resolv.conf
```

### Portscan (voir les connexions actives)

```bash
# Voir les connexions actives
netstat -tuln

# Version moderne
ss -tuln
```

---

## Le fichier /etc/hosts

Le fichier **hosts** permet de définir des correspondances entre noms de domaines et adresses IP.

```bash
# Éditer le fichier hosts
sudo nano /etc/hosts
```

Exemple de contenu :

```
127.0.0.1       localhost
192.168.1.1     mon-routeur.local
```

---

## Le service SSH

**SSH** (Secure Shell) permet de se connecter à distance à un autre ordinateur Linux.

### Installer un serveur SSH

```bash
# Installer le serveur
sudo apt install openssh-server

# Démarrer le service
sudo systemctl start ssh

# Activer au démarrage
sudo systemctl enable ssh
```

### Se connecter en SSH

```bash
ssh utilisateur@adresse_ip
```

Exemple :
```bash
ssh pi@192.168.1.100
```

---

## Résumé des commandes

| Commande | Description |
|----------|-------------|
| `ip addr` | Voir les adresses IP |
| `ping google.com` | Tester la connexion |
| `traceroute google.com` | Voir le chemin jusqu'au serveur |
| `netstat -tuln` | Voir les ports ouverts |
| `ssh user@ip` | Se connecter à distance |
| `scp fichier user@ip:/dossier` | Copier un fichier à distance |

---

## Exercice pratique

1. Ouvre un terminal
2. Tape `ip addr` pour voir ton adresse IP
3. Tente un `ping google.com` pour tester ta connexion
4. Vérifie ton IP publique avec `curl ifconfig.me`

---

**Rendez-vous demain pour la prochaine leçon !** 🚀
