# 🚀 Projet : Migration & Configuration LAN Startup (Exia)

**Contexte :** Déménagement d'une startup dans de nouveaux locaux.
**Objectif :** Rétablir la connectivité réseau (LAN) et l'accès aux services distants (FTP/Web) en corrigeant la topologie physique et la configuration logique des postes.

![Topologie Finale](Topologie.png)
*(Topologie validée)*

## 🛠️ Actions Réalisées

### 1. Topologie Physique (Câblage)
Mise en place d'une architecture en étoile pour centraliser le trafic.
- **Connexion des Postes :** Branchement des 3 PC (Benjamin, Loïc, Amandine) sur le **Switch** (Commutateur).
- **Connexion de l'Infrastructure :** Branchement du **Switch** vers le **Routeur** (Passerelle).

### 2. Configuration Logique (Clients)
Les ordinateurs possédaient une ancienne configuration statique incompatible. J'ai reparamétré chaque machine manuellement :

| Machine | Adresse IP | Masque (Subnet) | Passerelle (Gateway) | Serveur DNS |
| :--- | :--- | :--- | :--- | :--- |
| **PC Benjamin** | `192.168.0.10` | `255.255.255.0` | `192.168.0.1` | `208.208.208.208` |
| **PC Loïc** | `192.168.0.11` | `255.255.255.0` | `192.168.0.1` | `208.208.208.208` |
| **PC Amandine** | `192.168.0.12` | `255.255.255.0` | `192.168.0.1` | `208.208.208.208` |

> **Pourquoi ces réglages ?**
> - **Gateway :** Indispensable pour sortir du réseau local (aller sur Internet).
> - **DNS :** Indispensable pour traduire `data.exia.fr` en adresse IP.

### 3. Correction Infrastructure (Serveur)
Le serveur DNS distant avait une mauvaise adresse IP configurée, rendant la résolution de nom impossible pour les clients.
- **Action :** Changement de l'adresse IP du Serveur DNS.
- **Nouvelle IP :** `208.208.208.208` (pour correspondre à la config des clients).

---

## ⚡ Mini-Protocole de Dépannage (Cheat Sheet)

Si le réseau ne fonctionne pas, vérifier les points suivants dans cet ordre (Méthode "Couche par Couche") :

**1. Vérification Physique (Câbles)**
* *Symptôme :* Les triangles sont rouges ou pas de lumière sur le port.
* *Action :* Vérifier que les câbles sont branchés. Vérifier que l'équipement est allumé.

**2. Vérification IP (Adressage)**
* *Test :* `ping 192.168.0.10` (Ping sa propre IP ou celle du voisin).
* *Action :* Vérifier que l'IP est unique et que le masque est le même pour tout le monde (`255.255.255.0`).

**3. Vérification Routage (Passerelle)**
* *Test :* `ping 192.168.0.1` (Ping le routeur).
* *Action :* Si ça échoue, vérifier que la ligne **Default Gateway** est bien remplie dans la config IP du PC. Sans ça, impossible de sortir du bureau.

**4. Vérification Services (DNS)**
* *Test :* Le ping IP marche (`ping 208.208...`) mais le ping nom échoue (`ping google.com`).
* *Action :* Le serveur DNS est mal renseigné sur le PC, ou le serveur DNS lui-même a une mauvaise IP.
