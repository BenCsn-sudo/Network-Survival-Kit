# 02 - Protocoles et Modèle OSI

> **Le problème fondamental :** Pour que deux machines communiquent, elles doivent parler la même langue. En réseau, cette langue s'appelle un **protocole**.

## 1. Qu'est-ce qu'un protocole ?

Une définition simple serait : **un ensemble de règles qui définissent comment se produit une communication dans un réseau.**

### L'Analogie de la Conversation Humaine 🗣️
Pour comprendre à quoi sert un protocole, prenons l'exemple d'un appel téléphonique entre Pierre et Jean :

1.  **Établissement (Handshake) :**
    * *Pierre compose le numéro...* (Demande de connexion)
    * *Jean décroche et dit "Allô ?"* (Acceptation de connexion / ACK)
    * *Pierre répond "Salut, c'est Pierre"* (Début de session)
2.  **Transfert de données :**
    * *Pierre raconte son histoire.*
    * *Jean dit "Hein ? J'ai pas compris"* (Demande de retransmission / NACK).
    * *Pierre répète.* (Retransmission).
3.  **Fermeture :**
    * *Pierre dit "Au revoir".*
    * *Jean dit "Au revoir".*
    * *Clic.* (Fin de session).

En informatique, c'est exactement pareil. Les protocoles gèrent ces étapes automatiquement.

### Les 10 Exigences d'un bon protocole 🛠️
Pour être efficace, un protocole (ou une suite de protocoles) doit gérer plusieurs fonctions vitales :

| Fonction | Description | Exemple concret |
| :--- | :--- | :--- |
| **Formatage** | Définir la structure du message (En-tête + Contenu). | *Mise en page d'une lettre.* |
| **Adressage** | Identifier qui envoie et qui reçoit. | *Adresse IP Source / Destination.* |
| **Mapping** | Faire le lien entre adresse logique et physique. | *Lien IP ↔ Adresse MAC.* |
| **Routage** | Trouver le chemin à travers le réseau. | *GPS.* |
| **Détection d'erreurs** | Vérifier si le message est arrivé intact. | *Somme de contrôle (CRC).* |
| **Accusé de réception** | Confirmer la bonne réception. | *Recommandé avec AR.* |
| **Gestion des pertes** | Renvoyer les paquets perdus (Timeout). | *Si pas de réponse après 3s, on renvoie.* |
| **Séquençage** | Remettre les paquets dans le bon ordre à l'arrivée. | *Numéroter les pages d'un livre.* |
| **Contrôle de flux** | Adapter la vitesse si l'émetteur parle trop vite. | *"Attends, tu vas trop vite !"* |
| **Direction** | Gérer qui parle quand (Simplex, Half-Duplex, Full-Duplex). | *Talkie-Walkie vs Téléphone.* |

---

## 2. Le Modèle OSI (Open Systems Interconnection)

> **Définition :** Le modèle OSI est une norme standardisée qui divise le processus de communication réseau en **7 couches** distinctes.

L'idée est de diviser pour mieux régner : chaque couche a un rôle précis et ne parle qu'à ses voisines (celle du dessus et celle du dessous).

### 🧠 Moyens Mnémotechniques
Pour retenir l'ordre des couches (c'est indispensable pour les partiels !).

**De Haut en Bas (7 → 1) :**
> **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing
> *(Tout le monde semble avoir besoin de traitement de données)*

**De Bas en Haut (1 → 7) :**
> **P**our **L**e **R**éseau, **T**out **S**e **P**asse **A**utomatiquement
> *(Physique, Liaison, Réseau, Transport, Session, Présentation, Application)*

---

## 3. Détail des 7 Couches

Voici ce qui se passe quand vous envoyez un email, de haut en bas :

### Couche 7 : Application 🖥️
* **Rôle :** C'est l'interface avec l'utilisateur. C'est la seule couche que vous "voyez". Elle donne accès aux services réseaux (Web, Mail, Transfert de fichier).
* **Protocoles :** HTTP, DNS, SMTP, FTP.

### Couche 6 : Présentation 🎨
* **Rôle :** Elle s'occupe de la syntaxe et de la forme des données. C'est le traducteur.
* **Fonctions clés :**
    * **Encodage :** Convertir ASCII en binaire.
    * **Chiffrement :** Crypter les données (SSL/TLS).
    * **Compression :** Réduire la taille des fichiers.

### Couche 5 : Session 🤝
* **Rôle :** Le chef d'orchestre du dialogue. Elle ouvre, gère et ferme la session de communication entre deux machines.
* **Fonction clé :** Si la connexion coupe, c'est elle qui tente de "reprendre" là où on s'était arrêté.

### Couche 4 : Transport 🚚
* **Rôle :** Responsable de la qualité de service et de la fiabilité. Elle découpe les grosses données en petits morceaux appelés **Segments**.
* **Protocoles clés :**
    * **TCP :** Fiable, vérifie que tout arrive (Email, Web).
    * **UDP :** Rapide, mais sans vérification (Streaming, Jeu vidéo).
* **Concept clé :** Les **Ports** (ex: port 80 pour le Web).

### Couche 3 : Réseau (Network) 🌐
* **Rôle :** L'adressage logique et le **Routage**. C'est ici qu'on décide par quel chemin passer pour traverser Internet.
* **Unité de données :** Le **Paquet**.
* **Protocole roi :** **IP** (Internet Protocol).

### Couche 2 : Liaison de Données (Data Link) 🔗
* **Rôle :** La communication dans le réseau local (LAN). Elle gère l'accès au média physique et l'adressage physique (**MAC**).
* **Unité de données :** La **Trame** (Frame).
* **Matériel :** Switch.

### Couche 1 : Physique 🔌
* **Rôle :** La transmission pure et dure. On ne parle plus d'informatique mais d'électronique ou d'optique.
* **Unité de données :** Le **Bit** (0 ou 1).
* **Matériel :** Câbles, Hub, Ondes Wi-Fi.

---

## 📝 Résumé Synoptique

| # | Couche | Unité (PDU) | Rôle Principal | Matériel / Protocole |
|:---:| :--- | :--- | :--- | :--- |
| **7** | **Application** | Donnée | Interface utilisateur | HTTP, SMTP |
| **6** | **Présentation** | Donnée | Formatage, Chiffrement | ASCII, JPEG, SSL |
| **5** | **Session** | Donnée | Ouverture/Fermeture dialogue | RPC, NetBIOS |
| **4** | **Transport** | **Segment** | Fiabilité, Ports | TCP, UDP |
| **3** | **Réseau** | **Paquet** | Adressage IP, Routage | Routeur, IP |
| **2** | **Liaison** | **Trame** | Adressage MAC, Accès média | Switch, Ethernet |
| **1** | **Physique** | **Bit** | Signal électrique/optique | Câble, Carte réseau |

---

## 4. Le "Vrai" modèle : TCP/IP 🌍

Le modèle OSI est beau et théorique, mais Internet fonctionne sur **TCP/IP**.

### ⚔️ OSI vs TCP/IP : Le match
* **OSI (7 couches) :** Modèle **théorique** créé par l'ISO (norme). Parfait pour apprendre et comprendre.
* **TCP/IP (4 couches) :** Modèle **pratique** créé par le DoD (Département de la Défense US). C'est celui qui est implémenté dans votre ordinateur.

> **Le saviez-vous ?** On appelle souvent TCP/IP "La suite de protocoles Internet". Ce n'est pas un seul protocole, mais une **pile** (stack) de protocoles qui travaillent ensemble.

### 📐 Comparaison des Architectures

Le modèle TCP/IP regroupe certaines couches OSI pour simplifier :

| Modèle OSI (7) | Modèle TCP/IP (4) | Rôle dans TCP/IP |
| :--- | :--- | :--- |
| **7. Application** | **4. Application** | Regroupe tout ce qui touche aux données utilisateur (HTTP, FTP, SSH...). |
| **6. Présentation** | ^ | (Inclut encodage et chiffrement) |
| **5. Session** | ^ | (Inclut gestion de session) |
| **4. Transport** | **3. Transport** | Identique (TCP / UDP). Gère la fiabilité et les ports. |
| **3. Réseau** | **2. Internet** | Routage et Adressage IP (Le cœur du système). |
| **2. Liaison** | **1. Accès Réseau** | Regroupe tout ce qui touche au matériel (Driver, Carte Réseau, Câble). |
| **1. Physique** | ^ | |

---

## 5. L'Encapsulation : Les Poupées Russes 🪆

C'est **LE** concept le plus important pour comprendre comment une donnée traverse un réseau.

### 📦 Le Principe
Quand vous envoyez une donnée, elle descend les couches. À chaque étape, la couche ajoute son propre "en-tête" (Header) pour donner des instructions à la couche équivalente chez le destinataire.

**Analogie de la Lettre Postale :**
1.  **Donnée (Contenu) :** Vous écrivez la lettre.
2.  **Encapsulation 1 (Enveloppe) :** Vous mettez la lettre dans une enveloppe (Ajout d'adresses).
3.  **Encapsulation 2 (Sac Postal) :** Le facteur met l'enveloppe dans un sac avec d'autres lettres pour la même ville.
4.  **Encapsulation 3 (Camion) :** Le sac est mis dans un camion.

À l'arrivée, on fait l'inverse : on sort le sac du camion, l'enveloppe du sac, et la lettre de l'enveloppe. C'est la **Désencapsulation**.

### 🏷️ Vocabulaire précis : PDU et SDU
En ingénierie, on ne dit pas juste "donnée", on utilise des termes précis selon l'état de la donnée.

* **PDU (Protocol Data Unit) :** C'est le paquet "fini" d'une couche, prêt à être passé à la couche du dessous.
    * *Formule :* `PDU = En-tête + Donnée`
* **SDU (Service Data Unit) :** C'est la donnée "brute" reçue de la couche du dessus, avant qu'on y touche.

**L'évolution du nom de la donnée :**
Au fur et à mesure de la descente, la donnée change de nom :

| Couche | Nom de la donnée (PDU) | Ce que ça contient |
| :--- | :--- | :--- |
| **Application** | **Donnée** | Votre message (ex: "Coucou") |
| **Transport** | **Segment** | En-tête TCP (Ports) + Donnée |
| **Internet** | **Paquet** | En-tête IP (Adresses IP) + Segment |
| **Accès Réseau** | **Trame (Frame)** | En-tête MAC + Paquet + Trailer (CRC) |
| **Physique** | **Bits** | `01011010` (Signal électrique) |

> ⚠️ **Note importante :** Une couche ne "lit" pas ce qu'il y a dans la donnée qu'elle transporte.
> Pour le Routeur (Couche 3), le Segment TCP n'est qu'une suite de données, il ne cherche pas à savoir ce qu'il y a dedans. Il regarde juste son en-tête IP.

<div align="center">
  <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiVYeIbfNZd4SdJPhlFK9ZPKDx33032JU_aCgi_egK9OFMmATz5d9-yRCXSlhInD0TYyoeHmxKm-g53D4Sv5qFbmd8D2ntzwOffgYUedwbrbDMnLl5TO6ODWJ90oJJIJ4ZNld5-mMORViWX/s1600/284768.png" width="800" />
  <br>
  <i>(Schéma visuel de l'encapsulation et des en-têtes ajoutés)</i>
  <br><br>
</div>

---

## 6. Les Modes de Communication 📢

Sur un réseau, on ne parle pas toujours à une seule personne. Il existe trois manières de transmettre un message :

| Mode | Icône | Description | Exemple |
| :--- | :---: | :--- | :--- |
| **Unicast** | 👤 ➝ 👤 | **1 vers 1**. Communication privée entre un émetteur et un destinataire unique. | Naviguer sur un site Web, envoyer un email. |
| **Multicast**| 👤 ➝ 👥 | **1 vers un Groupe**. L'émetteur envoie à un groupe d'abonnés spécifiques. | IPTV, Vidéoconférence, Protocoles de routage. |
| **Broadcast**| 👤 ➝ 🌍 | **1 vers Tous**. L'émetteur envoie à *tous* les appareils du réseau local. | DHCP (demander une IP), ARP (demander une MAC). |

---

## 7. Qui décide des règles ? (Organismes de normalisation) 🏛️

Internet n'appartient à personne, mais des organisations bénévoles s'assurent que tout le monde utilise les mêmes normes (Standards ouverts).

* **ISOC / IAB / IETF :** S'occupent des protocoles logiques (**TCP/IP**). Ce sont eux qui rédigent les **RFC** (Request for Comments).
* **IEEE (Institute of Electrical and Electronics Engineers) :** S'occupe du matériel et des ondes.
    * *Exemples :* **802.3** (Ethernet), **802.11** (Wi-Fi).
* **ISO :** A créé le modèle OSI.
* **TIA/EIA :** Standardise les câbles physiques (ex: la norme de câblage RJ45 T568B).

---

## 8. L'Accès aux Données : Le voyage de l'adresse 🚚

C'est le concept **le plus important** pour comprendre le routage (Couche 2 vs Couche 3).

Quand un paquet voyage, il utilise deux types d'adresses qui ont des rôles différents. Imaginez un voyage en train de Paris à Berlin.

1.  **Adresse Logique (IP - Couche 3) :** C'est le **Destinataire Final**.
    * *Analogie :* "Je vais à Berlin".
    * ⚠️ **Règle d'or :** L'adresse IP source et destination **ne changent JAMAIS** pendant le voyage (de bout en bout).
2.  **Adresse Physique (MAC - Couche 2) :** C'est le **Prochain saut**.
    * *Analogie :* "Je prends le train jusqu'à Strasbourg (l'escale)".
    * ⚠️ **Règle d'or :** L'adresse MAC change à **chaque routeur** traversé.

### Scénario A : Communication sur le MÊME réseau
* **Source :** Mon PC (A)
* **Destination :** Mon Imprimante (B)
* Le PC compare l'IP et voit que c'est le même réseau.
* **Trame envoyée :** MAC Source = A / MAC Destination = B.
* *Pas besoin de routeur.*

### Scénario B : Communication vers un réseau DISTANT (Internet)
* **Source :** Mon PC (A)
* **Destination :** Serveur Google (C)
* Le PC voit que Google n'est pas dans le salon. Il doit envoyer le paquet à sa **Passerelle par défaut** (Le Routeur).

**Étape 1 (PC vers Routeur) :**
* IP Destination : Google (Loin)
* MAC Destination : **MAC du Routeur** (Proche)

**Étape 2 (Routeur vers Internet) :**
* Le routeur reçoit la trame, enlève l'enveloppe MAC (Désencapsulation).
* Il regarde l'IP (Google), consulte sa table de routage.
* Il remet une nouvelle enveloppe MAC (Ré-encapsulation) vers le routeur suivant.

> **Résumé :**
> * **IP** = Où je vais à la fin (Global).
> * **MAC** = À qui je passe le ballon maintenant (Local).
