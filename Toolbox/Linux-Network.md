# 🐧 Linux Network Cheat Sheet (Modern)

> **Objectif :** Analyser et dépanner le réseau depuis un terminal Linux (Debian/Ubuntu/Mint).
> **Note :** On privilégie ici la suite moderne **iproute2** (`ip`) qui remplace les vieux outils (`ifconfig`, `route`).

## 1. Interfaces & Adressage (Couche 1 & 2)

L'équivalent du `show ip interface brief` de Cisco.

| Action | Commande Moderne (`ip`) | Ancienne Commande |
| --- | --- | --- |
| **Voir les IPs** | `ip a` (ou `ip addr show`) | `ifconfig` |
| **Voir les liens (Câble)** | `ip link show` | `ifconfig -a` |
| **Allumer une interface** | `sudo ip link set eth0 up` | `ifconfig eth0 up` |
| **Ajouter une IP** | `sudo ip addr add 192.168.1.10/24 dev eth0` | `ifconfig eth0 ...` |

> **Astuce :** `ip -c a` met de la couleur dans la sortie (plus lisible).

## 2. Table ARP (Couche 2/3)

L'équivalent du `show ip arp` ou `show mac address-table`.

| Action | Commande | Description |
| --- | --- | --- |
| **Voir les voisins** | `ip neigh` | Affiche la table ARP (IP ↔ MAC). |
| **Vider le cache** | `sudo ip neigh flush all` | Utile si on change un équipement. |

## 3. Routage (Couche 3)

L'équivalent du `show ip route`.

| Action | Commande | Description |
| --- | --- | --- |
| **Voir la table** | `ip route` | Affiche la passerelle par défaut (`default via...`). |
| **Tester le chemin** | `traceroute 8.8.8.8` | (ou `tracepath`) Affiche chaque saut (routeur). |
| **Ping** | `ping -c 4 google.fr` | `-c 4` pour s'arrêter après 4 envois (sinon infini sous Linux). |

## 4. DNS & Noms de domaine (Couche 7)

Indispensable pour savoir si "C'est le réseau ou c'est le DNS ?"

* **`dig google.fr`** : La commande pro. Donne l'IP et le temps de réponse.
* *Astuce :* `dig +short google.fr` pour avoir juste l'IP.


* **`nslookup google.fr`** : Plus ancien, mais toujours utile.
* **`cat /etc/resolv.conf`** : Pour voir quel serveur DNS ton PC utilise.

## 5. Ports & Services (Couche 4) 🛡️

C'est là que Linux est plus fort que Cisco pour le diagnostic.

### Voir ce qui tourne sur TA machine (Serveur)

L'équivalent moderne de `netstat`.

* **`ss -tulpn`** : LA commande à connaître par cœur.
* `-t` : TCP
* `-u` : UDP
* `-l` : Listening (en écoute)
* `-p` : Process (quel programme utilise le port)
* `-n` : Numérique (pas de résolution de nom)



### Tester un port distant (Client)

Comment savoir si le port 80 (Web) d'un serveur est ouvert sans navigateur ?

* **`nc -zv 192.168.1.1 80`** (Netcat)
* "Connection to 192.168.1.1 80 port [tcp/http] succeeded!" -> ✅
* Si ça bloque -> Pare-feu ou service éteint.


* **`telnet 192.168.1.1 80`** (Vieux mais efficace).

---

### Mon conseil pour l'organisation

Regarde tes screenshots : tu as actuellement `04-Projet` et `Commandes-Cisco`.

Je ferais ceci :

1. Renomme le dossier `Commandes-Cisco` en **`Toolbox`** (ou `Boite-a-Outils`).
2. Dedans, renomme ton `README.md` actuel en **`Cisco-IOS.md`**.
3. Crée le nouveau fichier **`Linux-Network.md`** avec le contenu ci-dessus.
4. Crée un petit `README.md` principal dans ce dossier qui sert juste de sommaire avec deux liens vers ces fichiers.

Qu'en penses-tu ? Ça donne une dimension beaucoup plus "Ingénieur Système & Réseau" à ton profil !
