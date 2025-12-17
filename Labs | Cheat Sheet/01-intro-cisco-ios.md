# 04 - Prise en main de Cisco IOS (CLI)

> **Cisco IOS** (Internetwork Operating System) est le système d'exploitation propriétaire présent sur la majorité des routeurs et switchs Cisco. Il s'utilise en ligne de commande (CLI).

## 1. Accès au périphérique

Contrairement à un PC, on n'a pas d'écran branché directement. On accède à la CLI via :
1.  **Câble Console (Physique) :** Pour la première configuration (câble bleu clair "Rollover"). Nécessite un logiciel comme **PuTTY** ou dans notre cas en entrainement sur **Packet Tracer**.
2.  **SSH / Telnet (Réseau) :** Pour l'accès à distance une fois l'IP configurée.

## 2. La Hiérarchie des Modes hierarchy

C'est le concept le plus important. Une commande ne fonctionne que si l'on est dans le bon mode.



| Mode | Invite (Prompt) | Description | Commande pour y entrer |
| :--- | :--- | :--- | :--- |
| **Utilisateur** | `Switch>` | Mode lecture seule très limité (Ping, Show basique). | (Démarrage par défaut) |
| **Privilégié** | `Switch#` | Mode "Admin". Accès complet pour voir et débugger. | Tapez `enable` |
| **Configuration**| `Switch(config)#` | Pour modifier le routeur (IP, Nom, Sécu). | Tapez `configure terminal` |
| **Sous-mode** | `Switch(config-if)#`| Configurer une interface précise (ex: port Ethernet). | Tapez `interface fastEthernet 0/1` |

> **Astuce :** Pour revenir en arrière d'un cran, tapez `exit`. Pour revenir directement au mode privilège (`#`), tapez `end` ou faites `Ctrl+Z`.

## 3. Commandes de "Survie" (Cheat Sheet)

### ⌨️ Raccourcis Clavier indispensables
* **`TAB`** : Complète automatiquement la commande (ex: `conf` + TAB -> `configure`).
* **`?`** : Affiche l'aide contextuelle (liste des commandes possibles).
* **`Flèche Haut`** : Rappelle la commande précédente.

### 🛠️ Configuration de base (Le Script type)
Voici les premières commandes à taper sur tout nouvel équipement :

Passer en mode privilégié
```bash
enable
```
---
Passer en mode configuration
```bash
configure terminal
```
---
Changer le nom (Bonne pratique)
```bash
hostname R1-Paris
```
---
Sécuriser l'accès privilégié (Le plus important !)
> **'secret'** chiffre le mdp, **'password'** le laisse en clair (à éviter)
```bash
enable secret MonMotDePasseFort
```
---
Sécuriser l'accès console (Physique)
```bash
line console 0
 password cisco
 login
exit
```
---
Mettre une bannière légale (Dissuasion)
```bash
banner motd #ACCES RESTREINT - PERSONNEL AUTORISE UNIQUEMENT#
```
---
Chiffrer tous les mots de passe clairs (Service)
```bash
service password-encryption
```
---
## 4. Gestion de la Configuration (RAM vs NVRAM) 💾

C'est vital de comprendre où sont stockées vos modifications. Si vous éteignez le switch sans sauvegarder, vous perdez tout ce qui est dans la RAM.

| Type | Nom Cisco | Mémoire | Volatile ? | Description |
| :--- | :--- | :--- | :---: | :--- |
| **En cours** | `running-config` | **RAM** | ⚠️ OUI | La config active actuellement. |
| **Démarrage**| `startup-config` | **NVRAM** | ✅ NON | La config chargée au démarrage. |

### Commandes de sauvegarde
Une fois votre configuration terminée, il faut "copier" ce qui tourne (RAM) vers le disque dur (NVRAM).

Sauvegarder (Copier RAM vers NVRAM)
```bash
copy running-config startup-config
```
---
Voir la configuration actuelle
```bash
show running-config
```
---
Voir la configuration sauvegardée
```bash
show startup-config
```
---

## 5. Connectivité et Adressage IP 🌐

Un switch est un équipement de couche 2 (il comprend les adresses MAC), mais il a besoin d'une **adresse IP** pour être administré à distance (via SSH/Telnet). Sans IP, on ne peut le configurer qu'avec le câble console bleu.

On ne met pas l'IP sur un port physique (comme sur un PC), mais sur une **Interface Virtuelle (SVI)**.

### Configurer l'interface de gestion (SVI)

Par défaut, on utilise l'interface virtuelle du VLAN 1.

```bash
configure terminal
```

Entrer dans l'interface virtuelle
```bash
interface vlan 1
```

Attribuer l'IP et le Masque de sous-réseau
```bash
ip address 192.168.1.10 255.255.255.0
```

IMPORTANT : Allumer l'interface (elle est éteinte par défaut)
```bash
no shutdown
exit
```

### Configurer la passerelle par défaut (Gateway)

Pour que le switch puisse répondre à quelqu'un qui n'est pas dans son réseau local (ex: Internet ou un autre bâtiment).

```bash
ip default-gateway 192.168.1.254

```

---

## 6. Vérification et Dépannage 🔍

C'est bien de configurer, mais c'est mieux de vérifier que ça marche.

### La commande "Reine" 👑

C'est LA commande la plus utile pour voir l'état de vos ports en un coup d'œil.

```bash
show ip interface brief

```

* **Status "Up"** : La couche 1 (Physique) est OK (Câble branché).
* **Protocol "Up"** : La couche 2 (Liaison) est OK.

### Tester la connectivité (Ping)

Pour vérifier si on peut joindre un autre appareil. À faire en mode privilège (`#`).

```bash
ping 192.168.1.25
! "!!!!!" = Succès (5/5 reçus)
! "....." = Échec (Time out)
```
