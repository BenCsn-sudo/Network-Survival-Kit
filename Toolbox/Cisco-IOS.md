# 04 - Prise en main de Cisco IOS (CLI)

> **Cisco IOS** (Internetwork Operating System) est le système d'exploitation propriétaire présent sur la majorité des routeurs et switchs Cisco. Il s'utilise en ligne de commande (CLI).

---

## 1. Mémo Visuel : Les Câbles Packet Tracer ⚡

Avant de configurer, il faut câbler. Voici à quoi correspondent les icônes dans la barre d'outils :

<p align="center">
<img src="./img/CONNECTION.png" alt="connection" width="600"/>
</p>

| Icône | Nom | Usage Principal |
| --- | --- | --- |
| ⚡ | **Automatique** | Choisit le câble à ta place. *À éviter !* |
| 🔵 | **Console** | **PC ↔ Switch/Routeur**. Configuration initiale via port console. |
| ⚫ | **Droit** | **Équipements DIFFÉRENTS** (PC ↔ Switch). |
| ➖ | **Croisé** | **Équipements IDENTIQUES** (Switch ↔ Switch ou PC ↔ Routeur). |
| 🟠 | **Fibre** | Liaisons longue distance à haut débit. |
| 🟥🕓 | **Série DCE** | Liaison WAN entre routeurs (fournit l'horloge). |

---

## 2. Accès au périphérique

1. **Câble Console :** Première configuration via logiciel comme **PuTTY** ou terminal Packet Tracer.
2. **SSH / Telnet :** Accès à distance via le réseau (nécessite une IP et la configuration des lignes VTY).

---

## 3. La Hiérarchie des Modes

| Mode | Invite (Prompt) | Commande pour y entrer |
| --- | --- | --- |
| **Utilisateur** | `Router>` | (Par défaut au démarrage) |
| **Privilégié** | `Router#` | `enable` |
| **Configuration** | `Router(config)#` | `configure terminal` |
| **Sous-mode** | `Router(config-if)#` | `interface <nom_interface>` |

---

## 4. Commandes de "Survie" (Cheat Sheet) 🛠️

* **`TAB`** : Complète automatiquement la commande.
* **`?`** : Aide contextuelle (affiche les options).
* **`do`** : Exécute une commande de mode privilège depuis le mode config (ex: `do show ip route`).

### 📝 Configuration de base

```bash
hostname R1-Paris
enable secret MonMotDePasseFort
service password-encryption

! Sécuriser l'accès distant (Transport Layer)
line vty 0 15
 password cisco
 login
exit

```

---

## 5. Configuration des Interfaces (IP) 🌐

### A. Sur un SWITCH (SVI)

```bash
interface vlan 1
 ip address 192.168.1.10 255.255.255.0
 no shutdown
exit
ip default-gateway 192.168.1.254

```

### B. Sur un ROUTEUR (Interface Physique)

```bash
ipv6 unicast-routing  ! [cite_start]Indispensable pour l'IPv6 [cite: 139]

interface g0/0/0
 description Vers LAN-Principal
 ip address 192.168.1.1 255.255.255.0
 ipv6 address 2001:db8:acad:1::1/64  ! [cite_start]GUA [cite: 126]
 ipv6 address fe80::1 link-local     ! [cite_start]LLA manuelle [cite: 130]
 no shutdown
exit

```

---

## 6. Gestion de la Sauvegarde 💾

* **`copy running-config startup-config`** : Sauvegarde la RAM vers la NVRAM.
* **`write`** : Équivalent rapide de la sauvegarde.

---

## 7. Vérification Cisco (Router/Switch) 🔍

| Commande | Rôle |
| --- | --- |
| **`show ip interface brief`** | État résumé des interfaces IPv4. |
| **`show ipv6 interface brief`** | État résumé des interfaces IPv6.

 |
| **`show ip route`** / **`show ipv6 route`** | Table de routage (C=Connecté, S=Statique). |
| **`show ip arp`** / **`show ipv6 neighbors`** | Correspondance IP/MAC (Voisins directs). |
| **`show mac address-table`** | (Switch uniquement) Table de commutation L2. |
| **`traceroute <IP>`** | Analyse le chemin saut par saut (utilisé sur le routeur). |

---

## 8. Outils de Diagnostic sur PC (Invite de commandes) 💻

À utiliser sur les hôtes finaux (PC-A, PC-B) pour tester la connectivité à travers les couches Transport et Application.

### 🌐 DNS & Application

* 
**`nslookup <nom_domaine>`** : Interroge le serveur DNS pour obtenir l'IP d'un nom.


* *Exemple :* `nslookup google.com`


* **`ipconfig /all`** : Affiche toute la configuration IP, y compris le serveur DNS et le bail DHCP.

### 🚂 Couche Transport

* **`netstat -n`** : Affiche toutes les connexions TCP/UDP actives (Sockets `IP:Port`).
* **`netstat -r`** : Affiche la table de routage locale du PC.

### 📡 Connectivité & ICMP

* **`ping <IP>`** : Teste l'accessibilité d'un hôte (ICMP Echo).
* **`tracert <IP>`** : Équivalent Windows de `traceroute`. Identifie chaque routeur sur le chemin.
* *Astuce :* Si le ping vers une IP fonctionne mais pas vers un nom, le problème vient du DNS.



---

*Dernière mise à jour : Janvier 2026 - Focus Couches 4 à 7.*

---

Souhaites-tu que je prépare une section spécifique sur la **capture de paquets Wireshark** pour illustrer comment ces commandes se traduisent concrètement en segments TCP ou messages DNS ?
