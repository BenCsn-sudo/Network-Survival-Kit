# 09 - Adressage IPv6 🚀

> **Pourquoi l'IPv6 ?** L'épuisement des adresses IPv4 (4,3 milliards) a rendu nécessaire un nouveau protocole offrant **340 undécillions** d'adresses (), éliminant ainsi le besoin de NAT complexe.

---

## 1. Représentation et Compression ✍️

Une adresse IPv6 fait **128 bits**, écrits en **hexadécimal** (8 segments de 4 chiffres, appelés "hextets").

### Les 2 Règles de Compression (À connaître par cœur)

Pour rendre les adresses lisibles, on applique ces règles :

1. **Omettre les zéros non significatifs :** Dans chaque segment, on peut supprimer les zéros au début.
* *Ex:* `0db8` devient `db8`.

2. **Le double deux-points (`::`) :** On peut remplacer une suite continue de segments de zéros par `::`.
* ⚠️ **Attention :** Utilisable **UNE SEULE FOIS** par adresse.

---

## 2. Types d'Adresses & Structure 🏗️

| Type | Préfixe / Format | Description |
| --- | --- | --- |
| **GUA** (Global Unicast) | `2001::/3` | Adresse routable sur Internet (équivalent IP publique). |
| **LLA** (Link-Local) | `fe80::/10` | Indispensable. Utilisée pour communiquer sur un seul lien (LAN).
| **Loopback** | `::1/128` | Test de la pile réseau locale (équivalent `127.0.0.1`). |
| **Unique Local** | `fc00::/7` | Pour les réseaux internes non routables sur Internet. |
| **Multicast** | `ff00::/8` | Envoi vers un groupe de machines. |

### Structure d'une GUA (Global Unicast Address)

Une GUA est généralement structurée ainsi :

* **Préfixe de routage global (48 bits) :** Attribué par le fournisseur (ISP).
* **ID de sous-réseau (16 bits) :** Utilisé par l'organisation pour créer des sous-réseaux.
* **ID d'interface (64 bits) :** L'identifiant de la machine sur le réseau.

---

## 3. Adressage Dynamique (SLAAC & DHCPv6) 🔄

L'IPv6 a été conçu pour que les machines s'auto-configurent via des messages ICMPv6 :

* **RS (Router Solicitation) :** Le PC demande "Y a-t-il un routeur ?".
* 
**RA (Router Advertisement) :** Le routeur répond avec les infos du réseau.

### Les 3 Méthodes d'attribution

1. 
**SLAAC uniquement :** Le PC utilise le préfixe du RA et génère lui-même son ID d'interface (EUI-64 ou aléatoire).


2. **SLAAC + DHCPv6 sans état :** Le PC prend l'adresse via SLAAC mais demande les infos DNS au serveur DHCP.
3. **DHCPv6 avec état (Stateful) :** Le PC demande toute sa configuration au serveur (similaire à l'IPv4).

### Génération de l'ID d'interface (EUI-64)

Le PC prend son adresse MAC (48 bits), insère `ff:fe` au milieu et inverse le 7ème bit.

---

## 4. Multicast IPv6 📢

Il n'y a **PAS de Broadcast** en IPv6. Tout passe par le Multicast.

* **FF02::1 (All Nodes) :** Toutes les machines du lien local.
* 
**FF02::2 (All Routers) :** Tous les routeurs du lien local.


* **Solicited-Node Multicast :** Utilisé pour la résolution d'adresse (remplace ARP).

---

## 5. Subnetting IPv6 (Simplifié) ✂️

Contrairement à l'IPv4, on ne calcule pas bit par bit. On utilise le champ **Subnet ID** de 16 bits.

* Avec 16 bits, on peut créer **65 536** sous-réseaux de taille égale (/64).
* On incrémente simplement l'hexadécimal.

**Exemple :**

1. Réseau Parent : `2001:db8:acad::/48`
2. Sous-réseau 1 : `2001:db8:acad:0001::/64`
3. Sous-réseau 2 : `2001:db8:acad:0002::/64`
4. ...
5. Sous-réseau 65536 : `2001:db8:acad:ffff::/64`

---

## 6. Synthèse de Configuration (Cisco) 🛠️

Tiré de tes études de cas.

```bash
! ⚠️ Indispensable pour que le routeur travaille en IPv6
ipv6 unicast-routing

interface g0/0/0
 ! Configurer l'adresse globale
 ipv6 address 2001:db8:acad:1::1/64
 ! Configurer manuellement la Link-Local (plus simple que EUI-64)
 ipv6 address fe80::1 link-local
 no shutdown

! Vérifier
show ipv6 interface brief

```

---
