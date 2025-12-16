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
Passer en mode configuration
```bash
configure terminal
```
1. Changer le nom (Bonne pratique)
```bash
hostname R1-Paris
```
2. Sécuriser l'accès privilégié (Le plus important !)
> **'secret'** chiffre le mdp, **'password'** le laisse en clair (à éviter)
```bash
enable secret MonMotDePasseFort
```
3. Sécuriser l'accès console (Physique)
```bash
line console 0
 password cisco
 login
exit
```
4. Mettre une bannière légale (Dissuasion)
```bash
banner motd #ACCES RESTREINT - PERSONNEL AUTORISE UNIQUEMENT#
```
5. Chiffrer tous les mots de passe clairs (Service)
```bash
service password-encryption
