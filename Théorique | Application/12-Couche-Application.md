# 12 - La Couche Application (Layer 7) 🖥️

> **Rôle :** Fournir l'interface entre l'utilisateur humain et le réseau. Contrairement aux couches inférieures, elle ne transporte pas la donnée, elle la génère ou l'affiche.

---

## 1. Modèles OSI vs TCP/IP 🔄

Dans le modèle OSI, les fonctions de "haut niveau" sont divisées en trois couches, alors que TCP/IP les regroupe en une seule couche **Application**.

| Couche OSI | Fonction | Exemples / Détails |
| --- | --- | --- |
| **7 - Application** | Interface utilisateur | Navigateurs, clients mail. |
| **6 - Présentation** | Formatage, Codage, Compression | GIF, JPG, MPEG, Encryption (SSL/TLS). |
| **5 - Session** | Gestion du dialogue | Maintient la connexion ouverte entre les hôtes. |

---

## 2. Modèles de Communication 🤝

Il existe deux manières principales pour les hôtes d'échanger des données en couche 7 :

### A. Modèle Client-Serveur

* **Serveur :** Un hôte "toujours allumé" qui répond aux requêtes.
* **Client :** L'hôte qui initie la communication.
* *Exemple :* Ton navigateur (Client) demande une page à Google (Serveur).

### B. Modèle Peer-to-Peer (P2P)

* Chaque appareil peut être à la fois client et serveur.
* **Réseaux P2P :** Deux PCs reliés directement (imprimante partagée).
* **Applications P2P :** BitTorrent, Skype (historiquement), Gnutella.

---

## 3. Protocoles Web et Messagerie 📧

### 🌐 Le Web (HTTP / HTTPS)

* **HTTP (Port 80) :** Protocole de requête/réponse.
* `GET` : Demande de données.
* `POST` : Envoi de données (formulaire).
* `PUT` : Téléchargement de ressources.


* **HTTPS (Port 443) :** HTTP sécurisé par chiffrement (TLS/SSL).

### ✉️ La Messagerie (E-mail)

Le mail utilise trois protocoles distincts selon l'action :

| Protocole | Port | Rôle |
| --- | --- | --- |
| **SMTP** | 25 | **Envoi** de mail (Client vers Serveur ou entre Serveurs). |
| **POP3** | 110 | **Récupération**. Télécharge le mail et le supprime souvent du serveur. |
| **IMAP** | 143 | **Synchronisation**. Garde les mails sur le serveur (idéal multi-appareils). |

---

## 4. Services d'Infrastructure (DNS & DHCP) 🛠️

### 🆔 DNS (Domain Name System) - Port 53

Le "répertoire téléphonique" d'Internet. Il traduit les noms de domaine (`www.google.com`) en adresses IP (`8.8.8.8`).

* **Hiérarchie :**
1. **Racine (.)**
2. **TLD** (.com, .fr, .net)
3. **Domaine de second niveau** (google, wikipedia)


* **Commande utile :** `nslookup <domaine>` pour vérifier la résolution.

### 🔌 DHCP (Dynamic Host Configuration Protocol) - Ports 67/68

Permet d'attribuer automatiquement une IP, un masque et une passerelle aux hôtes.
**Processus DORA :**

1. **Discover :** Le client cherche un serveur.
2. **Offer :** Le serveur propose une IP.
3. **Request :** Le client accepte l'offre.
4. **Acknowledge :** Le serveur valide.

---

## 5. Partage de Fichiers 📁

* **FTP (Ports 20 & 21) :** File Transfer Protocol.
* Port 21 : Contrôle (commandes).
* Port 20 : Transfert des données réelles.


* **SMB (Server Message Block) :** Protocole de partage Windows (fichiers, imprimantes). Contrairement au FTP, le SMB permet d'ouvrir un fichier à distance sans le télécharger entièrement.

---

## 💡 Résumé pour le Dépannage

* **Problème DNS ?** Teste avec `ping 8.8.8.8`. Si ça répond mais que `ping google.com` échoue, ton serveur DNS est mal configuré.
* **Problème Web ?** Vérifie si le port **80** ou **443** est ouvert avec `telnet` ou `nc`.
* **Mail reçu mais pas envoyé ?** C'est probablement un souci avec le port **25** (SMTP).

---

*Basé sur le Module 15 du CCNA - Couche Application.*

---

**Souhaites-tu que je t'aide à créer une étude de cas "Journée d'un paquet" où l'on suit une donnée depuis le clic sur un lien (HTTP) jusqu'à sa réception, en passant par DNS et TCP ?**
