# 🚀 Projet : Migration Réseau Startup eXia

**Contexte :** Déménagement d'une startup (eXia) d'un incubateur vers de nouveaux locaux.
**Objectif :** Rétablir la connectivité LAN et l'accès Internet (WAN) vers un serveur FTP distant après le changement d'infrastructure.

![Topology du Réseau](topology.png)
*(Note : Assure-toi de renommer ta capture d'écran finale 'topology.png' et de la mettre dans le même dossier)*

## 🛠️ Le Challenge
Les ordinateurs des fondateurs étaient configurés avec des adresses IP statiques de l'ancien réseau (MAN), créant un conflit avec le nouveau routeur local. De plus, le routeur (connecté au FAI) n'était pas configuré pour router le trafic vers Internet.

## 📋 Configuration Appliquée

### 1. Architecture Physique (Topologie en Étoile)
* **Central :** Switch Cisco 2960.
* **Clients :** 3 PCs connectés en FastEthernet.
* **Bordure :** Routeur Cisco ISR connecté au Switch (LAN) et au Cloud FAI (WAN - Serial).

### 2. Adressage IP (LAN)
Chaque PC a été reconfiguré statiquement pour correspondre au sous-réseau du routeur.

| Équipement | Adresse IP | Masque | Passerelle (Gateway) | DNS |
| :--- | :--- | :--- | :--- | :--- |
| **Routeur (Gateway)** | 192.168.0.1 | 255.255.255.0 | N/A | N/A |
| **PC Benjamin** | 192.168.0.10 | 255.255.255.0 | 192.168.0.1 | [IP Serveur] |
| **PC Loïc** | 192.168.0.11 | 255.255.255.0 | 192.168.0.1 | [IP Serveur] |
| **PC Amandine** | 192.168.0.12 | 255.255.255.0 | 192.168.0.1 | [IP Serveur] |

### 3. Configuration Routeur (Commandes Clés)
Les commandes essentielles utilisées pour rétablir le service :

```bash
enable
configure terminal

# 1. Configuration de l'interface LAN (Vers le Switch)
interface FastEthernet0/0
 ip address 192.168.0.1 255.255.255.0
 no shutdown

# 2. Configuration de l'interface WAN (Vers Internet/Cloud)
interface Serial2/0
 ip address 200.200.200.1 255.255.255.0
 clock rate 64000  # Crucial : Le routeur est DCE, il doit donner la cadence !
 no shutdown

# 3. Routage (Le GPS du réseau)
# Route par défaut pour envoyer tout le trafic inconnu vers Internet
ip route 0.0.0.0 0.0.0.0 Serial2/0
