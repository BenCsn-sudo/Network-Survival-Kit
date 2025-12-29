# 🐧 Linux Network : Le Guide de Survie

> **Philosophie :** En réseau, on dépane toujours du **bas vers le haut** (Modèle OSI). D'abord on vérifie le câble, puis l'IP, puis la route, puis le DNS.
>
> **Outil principal :** On utilise la suite moderne **`iproute2`** (commande `ip`). Oubliez `ifconfig`.

---

## 1. Étape 1 : Suis-je connecté ? (Couches 1 & 2) 🔌

Avant de pinger Internet, vérifions si la carte réseau est vivante et si elle a une IP.

### Voir l'état des interfaces
```bash
ip -c a
# (Le -c met de la couleur, c'est vital pour la lisibilité)

```

**Ce qu'il faut analyser :**

1. **L'état du lien :** Cherchez le mot `UP` dans `<BROADCAST,MULTICAST,UP,LOWER_UP>`.
* *Si c'est DOWN :* Câble débranché ou interface éteinte.


2. **L'adresse IP :** Cherchez la ligne `inet 192.168.x.x`.
* *Si pas d'IP :* Problème DHCP ou config statique manquante.



### Agir sur l'interface

```bash
# Allumer une interface éteinte
sudo ip link set dev eth0 up

# Demander une nouvelle IP au routeur (Relancer le DHCP)
sudo dhclient -v -r  # Libérer l'IP actuelle
sudo dhclient -v     # Demander une nouvelle

```

---

## 2. Étape 2 : Puis-je sortir de chez moi ? (Couche 3 - Locale) 🏠

J'ai une IP, c'est bien. Mais est-ce que je peux parler à mon routeur (la passerelle) ?

### Trouver ma passerelle (Gateway)

```bash
ip route

```

Regardez la ligne qui commence par `default via ...`.

* *Exemple :* `default via 192.168.1.1 dev eth0` -> Votre routeur est `192.168.1.1`.

### Tester la passerelle

```bash
ping -c 4 192.168.1.1

```

* ✅ **Ça répond :** Votre réseau local (LAN) fonctionne. Le problème est plus loin.
* ❌ **Ça ne répond pas :** Problème de câble, de Wi-Fi, ou le routeur est éteint.

### Qui est mon voisin ? (Table ARP)

Si le ping échoue, vérifiez si votre PC arrive à trouver l'adresse MAC du routeur.

```bash
ip neigh

```

* Si vous voyez `REACHABLE` à côté de l'IP du routeur : Tout va bien.
* Si vous voyez `FAILED` ou `INCOMPLETE` : Problème de couche 2 (Switch, Câble).

---

## 3. Étape 3 : Ai-je accès à Internet ? (Couche 3 - Wan) 🌍

Je sors de chez moi, mais est-ce que le routeur m'emmène sur Internet ?

### Le Test Ultime (Ping IP)

On ping une IP publique connue (Google DNS) pour éviter les problèmes de noms.

```bash
ping -c 4 8.8.8.8

```

* ✅ **Ça répond :** Vous avez Internet ! Si votre navigateur ne marche pas, c'est le DNS (Étape 4).
* ❌ **Ça ne répond pas :** Votre routeur a un problème avec son opérateur (Fibre coupée ?).

### Où est-ce que ça coupe ? (Traceroute)

Pour voir le chemin exact et trouver le routeur défaillant.

```bash
traceroute -n 8.8.8.8
# (ou tracepath sur certaines distros)

```

* L'option `-n` évite de perdre du temps à chercher les noms des routeurs.

---

## 4. Étape 4 : Le problème DNS (Couche 7) 📖

"J'ai Internet (ping 8.8.8.8 OK) mais je ne peux pas aller sur google.fr". C'est **toujours** le DNS.

### Tester la résolution de nom

```bash
dig google.fr
# ou
nslookup google.fr

```

* **Regardez la section `ANSWER SECTION**`.
* Si vous voyez une IP s'afficher : Le DNS marche.
* Si "Server not found" ou "Time out" : Votre serveur DNS est mort.

### Savoir quel DNS j'utilise

```bash
cat /etc/resolv.conf

```

* C'est ici que sont listés vos serveurs (ex: `nameserver 8.8.8.8`).

---

## 5. Étape 5 : Ports et Services (Couche 4) 🛡️

Le réseau marche, le DNS marche, mais l'application plante ? C'est une histoire de ports (Pare-feu ou Service planté).

### Cas A : Je suis le SERVEUR (J'héberge un site)

Est-ce que mon logiciel tourne bien ?

```bash
ss -tulpn

```

* Cherchez votre port (ex: `:80` pour Web, `:22` pour SSH).
* Si la ligne n'existe pas : Le logiciel n'est pas lancé.
* Si elle existe mais en `127.0.0.1:80` : Il n'écoute que en local (pas accessible du réseau). Il faut qu'il écoute sur `0.0.0.0` ou votre IP LAN.

### Cas B : Je suis le CLIENT (Je veux accéder à un site)

Est-ce que le serveur en face m'autorise ou y a-t-il un pare-feu ?
L'outil magique est **Netcat** (`nc`).

```bash
# Tester si le port 80 de google.fr est ouvert
nc -zv google.fr 80

```

* `Connection to google.fr 80 port [tcp/http] succeeded!` -> ✅ La route est libre.
* `Connection timed out` -> ❌ Bloqué par un Pare-feu (Firewall).

---

## 📝 Résumé des commandes vitales

| Besoin | Commande |
| --- | --- |
| **Mon IP ?** | `ip -c a` |
| **Ma passerelle ?** | `ip route` |
| **Ping LAN ?** | `ping <IP_Passerelle>` |
| **Ping WAN ?** | `ping 8.8.8.8` |
| **Test DNS ?** | `dig google.fr` |
| **Test Port ?** | `nc -zv <IP> <Port>` |
