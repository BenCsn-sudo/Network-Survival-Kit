# 04 - Prise en main de Cisco IOS (CLI)

> **Cisco IOS** (Internetwork Operating System) est le système d'exploitation propriétaire présent sur la majorité des routeurs et switchs Cisco.
> Il s'utilise en ligne de commande (CLI).

---

## 1. Mémo Visuel : Les Câbles Packet Tracer ⚡

Avant de configurer, il faut câbler. Voici à quoi correspondent les icônes dans la barre d'outils :

<p align="center">
<img src="./img/CONNECTION.png" alt="connection" width="600"/>
</p>

| Icône | Nom | Usage Principal |
| --- | --- | --- |
| ⚡ | **Automatique** | Choisit le câble à ta place. *À éviter pour apprendre !* |
| 🔵 | **Console** (Bleu ciel) | **PC (RS232) ↔ Routeur/Switch (Console)**. Sert à la configuration initiale (Ligne de commande). |
| ⚫ | **Droit** (Trait noir) | **Équipements DIFFÉRENTS** (PC ↔ Switch / Switch ↔ Routeur). Le standard RJ45. |
| ➖ | **Croisé** (Pointillés) | **Équipements IDENTIQUES** (PC ↔ PC / Switch ↔ Switch / PC ↔ Routeur). |
| 🟠 | **Fibre** (Orange) | **Liaisons Longue Distance**. Nécessite des ports spécifiques (GigabitEthernet ou SFP). |
| 〰️ | **Téléphone** (Gris) | Modem ↔ Prise Téléphone (RJ11). Pour l'ADSL. |
| 🟦 | **Coaxial** (Bleu zigzag) | Cloud ↔ Modem Câble. Pour l'Internet par câble TV. |
| 🟥🕓 | **Série DCE** (Éclair + Horloge) | **Routeur ↔ Routeur (WAN)**. Côté "Fournisseur" qui donne le rythme (*Clock Rate*). |
| 🟥 | **Série DTE** (Éclair simple) | **Routeur ↔ Routeur (WAN)**. Côté "Client". |

---

## 2. Accès au périphérique

Contrairement à un PC, on n'a pas d'écran branché directement. On accède à la CLI via :

1. **Câble Console (Physique) :** Pour la première configuration (câble bleu clair). Nécessite un logiciel comme **PuTTY** (ou l'onglet "Desktop > Terminal" dans Packet Tracer).
2. **SSH / Telnet (Réseau) :** Pour l'accès à distance une fois l'IP configurée.

---

## 3. La Hiérarchie des Modes

C'est le concept le plus important. Une commande ne fonctionne que si l'on est dans le bon mode.

| Mode | Invite (Prompt) | Description | Commande pour y entrer |
| --- | --- | --- | --- |
| **Utilisateur** | `Router>` | Lecture seule limitée (Ping, quelques Show). | (Démarrage par défaut) |
| **Privilégié** | `Router#` | Mode "Admin". Accès complet (Show all, Save, Debug). | Tapez `enable` |
| **Configuration** | `Router(config)#` | Pour modifier le routeur (IP, Nom, Sécu). | Tapez `configure terminal` |
| **Sous-mode** | `Router(config-if)#` | Configurer une interface précise. | Tapez `interface g0/0/0` |

> **Astuce de navigation :**
> * `exit` : Revenir un cran en arrière.
> * `end` (ou `Ctrl+Z`) : Revenir direct au mode privilégié (`#`).
> 
> 

---

## 4. Commandes de "Survie" (Cheat Sheet) 🛠️

### ⌨️ Raccourcis Clavier indispensables

* **`TAB`** : Complète automatiquement la commande (ex: `conf` + TAB -> `configure`).
* **`?`** : Affiche l'aide contextuelle (liste des commandes possibles).
* **`Flèche Haut`** : Rappelle la commande précédente (Historique).
* **`do`** : **L'astuce ultime.** Permet de lancer une commande privilégiée (ping, show) depuis les modes de configuration.
* *Exemple :* `do show running-config` (alors qu'on est en train de configurer une interface).



### 📝 Configuration de base (Le Script type)

À lancer sur tout nouvel équipement (Switch ou Routeur).

```bash
enable
configure terminal

! 1. Nommer l'équipement
hostname R1-Paris

! 2. Sécuriser l'accès privilégié ('secret' chiffre le mdp)
enable secret MonMotDePasseFort

! 3. Sécuriser l'accès console (Physique)
line console 0
 password cisco
 login
exit

! 4. Sécuriser l'accès distant (VTY - Telnet/SSH)
! "0 15" signifie qu'on autorise 16 connexions simultanées
line vty 0 15
 password cisco
 login
exit

! 5. Mettre une bannière légale
banner motd #ACCES RESTREINT - PERSONNEL AUTORISE UNIQUEMENT#

! 6. Chiffrer les mots de passe clairs (Service)
service password-encryption

```

---

## 5. Configuration des Interfaces (IP) 🌐

C'est ici que ça change selon le matériel !

### A. Sur un SWITCH (Interface Virtuelle - SVI)

Le switch est un équipement de couche 2. On lui donne une IP juste pour pouvoir le gérer à distance.
Il lui faut aussi une passerelle (Gateway) pour répondre à un administrateur situé dans un autre réseau.

```bash
configure terminal

! Configurer l'IP de gestion
interface vlan 1
 ip address 192.168.1.10 255.255.255.0
 no shutdown
exit

! Configurer la passerelle par défaut (Mode config globale)
ip default-gateway 192.168.1.254

```

### B. Sur un ROUTEUR (Interface Physique)

Le routeur est un équipement de couche 3. Chaque port est un réseau distinct et doit être configuré individuellement.

```bash
configure terminal

! Activer le routage IPv6 (Obligatoire pour qu'il route !)
ipv6 unicast-routing

! Choisir le port physique (ex: GigabitEthernet 0/0/0)
interface g0/0/0
 ! Bonne pratique : Toujours mettre une description
 description Vers LAN-Principal
 
 ! --- Configuration IPv4 ---
 ip address 192.168.1.1 255.255.255.0
 
 ! --- Configuration IPv6 ---
 ! Adresse Unicast Globale (GUA) - Routable sur Internet
 ipv6 address 2001:db8:acad:1::1/64
 
 ! Adresse Link-Local (LLA) - Communication locale uniquement
 ! Astuce : On la fixe manuellement (fe80::1) pour qu'elle soit facile à retenir
 ! sinon le routeur en génère une longue et compliquée à partir de l'adresse MAC.
 ipv6 address fe80::1 link-local
 
 ! IMPORTANT : Les ports routeurs sont éteints par défaut !
 no shutdown
exit

```
---

## 6. Gestion de la Sauvegarde (RAM vs NVRAM) 💾

| Type | Nom Cisco | Mémoire | Volatile ? | Description |
| --- | --- | --- | --- | --- |
| **En cours** | `running-config` | **RAM** | ⚠️ OUI | La config active. Perdue si reboot. |
| **Démarrage** | `startup-config` | **NVRAM** | ✅ NON | La config stockée sur disque dur. |

```bash
! Sauvegarder (Copier la RAM vers le Disque)
copy running-config startup-config

! Raccourci (non officiel mais marche souvent)
write

```

---

## 7. Vérification et Dépannage (Show Commands) 🔍

C'est ici que tu passes 80% de ton temps.

### 👑 L'état des interfaces (L1/L2)

La commande la plus utile pour voir si les câbles sont branchés et les IP correctes.

```bash
show ip interface brief

```

* **Status (Couche 1)** :
* `Up` : Câble branché.
* `Down` : Câble débranché.
* `Administratively down` : Interface éteinte (manque le `no shutdown`).


* **Protocol (Couche 2)** : `Up` si la communication passe.

### 🔬 Détails d'une interface

Pour voir la description, l'adresse MAC (BIA) et les erreurs.

```bash
show interfaces g0/0/0

```

### 🗺️ La Table de Routage (Routeur - L3)

Pour voir les réseaux connus par le routeur.

```bash
show ip route

```

* `C` : **Connected** (Réseau directement branché).
* `L` : **Local** (L'IP de l'interface elle-même).
* `S` : **Static** (Route ajoutée manuellement).

### 🔗 La Table ARP (Routeur - L3)

Pour voir la correspondance IP ↔ MAC. Indispensable si un Ping échoue dans le LAN.

```bash
show ip arp

```

### 🔀 La Table MAC (Switch - L2)

Pour voir quelle machine est branchée sur quel port.

```bash
show mac address-table

```

### 📡 Tester la connectivité (Ping)

```bash
! Ping simple
ping 192.168.1.1

! Si ça rate (.....) ou (U.U.U) : Vérifier le routage ou les pare-feu.
! Si ça marche (!!!!!) : Tout est OK.

```
