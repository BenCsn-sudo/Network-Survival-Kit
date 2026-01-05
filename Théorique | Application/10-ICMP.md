# 10 - ICMP : Le Protocole de Contrôle et Diagnostic 🛠️

> **Rôle :** ICMP (Internet Control Message Protocol) fournit des commentaires sur les problèmes liés au traitement des paquets IP. Il permet de comprendre pourquoi un paquet n'est pas arrivé, sans pour autant rendre IP fiable.

---

## 1. Messages Communs (ICMPv4 & ICMPv6) 📨

Les messages ICMP servent à signaler l'état d'une transmission.

* **Echo Request / Reply :** Utilisés pour tester l'accessibilité d'un hôte (base de la commande **Ping**).
* **Destination Unreachable :** Indique que le paquet ne peut pas être livré (route inexistante, port fermé).
* **Time Exceeded :** Indique que le **TTL** (v4) ou **Hop Limit** (v6) est tombé à 0, arrêtant ainsi une éventuelle boucle de routage.

---

## 2. ICMPv6 : Neighbor Discovery Protocol (NDP) 📡

En IPv6, ICMP gère la découverte du réseau et remplace le protocole ARP.

### Communication Routeur ↔ Hôte (SLAAC)

* **RS (Router Solicitation) :** Le PC demande la présence d'un routeur sur le segment.
* **RA (Router Advertisement) :** Le routeur annonce le préfixe réseau et l'ID de sous-réseau. C'est ce message qui permet l'auto-configuration **SLAAC**.

### Communication Hôte ↔ Hôte (Résolution MAC)

* **NS (Neighbor Solicitation) :** "Qui a cette adresse IPv6 ?".
* **NA (Neighbor Advertisement) :** "C'est moi, voici mon adresse MAC".

---

## 3. Récapitulatif des Types et Codes ICMP 📊

Voici les codes les plus fréquents à identifier lors d'une analyse réseau ou d'une capture Wireshark.

### ICMPv4 (Types et Codes)

| Type | Code | Description | Signification |
| --- | --- | --- | --- |
| **0** | 0 | Echo Reply | Réponse au Ping (Succès). |
| **3** | 0 | Net Unreachable | Réseau inconnu dans la table de routage. |
| **3** | 1 | Host Unreachable | Machine de destination non trouvée dans le LAN. |
| **3** | 3 | Port Unreachable | L'application/service est fermé sur l'hôte. |
| **8** | 0 | Echo Request | Requête Ping envoyée. |
| **11** | 0 | Time Exceeded | TTL expiré durant le transit (boucle ou Traceroute). |

### ICMPv6 (Messages spécifiques)

| Type | Description | Rôle |
| --- | --- | --- |
| **128** | Echo Request | Demande de réponse de voisinage ou distante. |
| **129** | Echo Reply | Réponse affirmative de connectivité. |
| **133** | Router Solicitation (RS) | Recherche de routeur par un hôte. |
| **134** | Router Advertisement (RA) | Annonce de préfixe par le routeur. |
| **135** | Neighbor Solicitation (NS) | Demande d'adresse MAC (équivalent ARP). |
| **136** | Neighbor Advertisement (NA) | Réponse avec adresse MAC. |

---

## 4. Outils de Test : Ping vs Traceroute 🔍

### A. Ping (Accessibilité)

Test de connectivité de bout en bout.

1. **Loopback (`::1`) :** Vérifie la pile réseau interne.
2. **Link-Local (`fe80::1`) :** Vérifie la connectivité sur le segment local.


3. **Global Unicast :** Vérifie le routage vers l'extérieur.

### B. Traceroute (Analyse du chemin)

Le Traceroute utilise le message **Time Exceeded** pour identifier chaque saut (routeur) sur le trajet.

---

## 💡 Résumé pour le Dépannage

* **Si Ping échoue mais Traceroute fonctionne :** Un pare-feu bloque probablement les messages Echo mais laisse passer les messages d'erreur de temps.
* **Pas de RA (Router Advertisement) :** Vérifiez que `ipv6 unicast-routing` est activé sur le routeur.
* **Destination Unreachable (Port 3/3) :** Votre configuration IP et de routage est correcte, mais le service distant est arrêté ou bloqué.
