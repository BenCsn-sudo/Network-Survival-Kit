# 🔌 Lab 02 : Connexion Physique (Wired & Wireless)

> **Objectif :** Sélectionner le média de transmission approprié (Câble) pour interconnecter différents types de périphériques (Routeurs, Switchs, PC, Cloud, Modem).

## 1. Contexte du Lab

Dans cet exercice Packet Tracer, la topologie logique est déjà configurée, mais la topologie physique est inexistante. [cite_start]L'objectif est de relier les équipements en respectant les normes de la **Couche 1 du modèle OSI**[cite: 1, 3, 17].

**Compétences validées :**
* Identification des types de câbles (Série, Coaxial, Cuivre).
* Compréhension de la distinction **Droit** (Straight-Through) vs **Croisé** (Cross-Over).
* Connexion console pour l'administration.

## 2. Topologie

| État Initial | État Final (Câblé) |
| :---: | :---: |
| ![Topologie Initiale](./img/topology-before.png) | ![Topologie Finale](./img/topology-after.png) |

*(N'oublie pas de mettre tes deux screenshots dans un dossier `img` et de renommer les fichiers)*

## 3. Choix des Câbles et Justification 🛠️

Le choix du câble n'est pas aléatoire. Il dépend des interfaces et de la nature des appareils connectés.

### 🅰️ Les Connexions WAN (Réseau Étendu)
Pour sortir du réseau local, nous utilisons des technologies spécifiques.

* **Routeur 0 ↔ Cloud :** Câble **Cuivre Droit**.
    * [cite_start]*Pourquoi ?* Le Cloud simule ici une connexion Ethernet vers un fournisseur, agissant comme un commutateur[cite: 24, 25].
* **Cloud ↔ Modem Câble :** Câble **Coaxial**.
    * [cite_start]*Pourquoi ?* C'est la norme standard pour l'internet par le câble (DOCSIS)[cite: 28].
* **Routeur 0 ↔ Routeur 1 :** Câble **Série (DCE/DTE)**.
    * [cite_start]*Pourquoi ?* Utilisé pour les liaisons point-à-point longue distance historiques entre routeurs[cite: 32, 33].

### 🅱️ Les Connexions LAN (Réseau Local) - Le piège du Cuivre ⚠️
C'est ici que se joue la différence entre câble Droit et Croisé.

* **Routeur 1 ↔ Switch :** Câble **Droit**.
    * [cite_start]*Pourquoi ?* On connecte des équipements de niveaux différents (L3 vers L2)[cite: 51].
* **Switch ↔ PC :** Câble **Droit**.
    * *Pourquoi ?* Idem, niveaux différents.
* **Routeur 0 ↔ Serveur (Netacad.pka) :** Câble **CROISÉ** (Cross-Over).
    * *Pourquoi ?* **C'est le point clé du TP.** Un PC et un Routeur utilisent les mêmes broches pour émettre (1 & 2) et recevoir (3 & 6). Si on utilise un câble droit, ils émettent sur le même fil et se percutent. [cite_start]Le câble croisé inverse les fils pour que l'émission de l'un tombe en face de la réception de l'autre[cite: 36, 37, 38].

> [cite_start]**Note :** Les cartes réseaux modernes ont la fonction *Auto-MDIX* qui croise automatiquement, mais ce Lab force à connaître la théorie historique[cite: 43].

### ©️ La Connexion de Gestion
* **PC de Config ↔ Routeur (Console) :** Câble **Console** (Bleu ciel).
    * *Pourquoi ?* Ce n'est pas une connexion réseau. [cite_start]C'est une connexion série directe pour configurer l'équipement en ligne de commande (CLI) via un terminal (RS-232)[cite: 46, 47].

## 4. Vérification ✅

Une fois le câblage effectué, les voyants sont passés au vert (Link Up). La connectivité a été validée par :
1.  [cite_start]**Ping :** De *PC Familial* vers *netacad.pka* (Traversée du réseau complet)[cite: 61, 62].
2.  [cite_start]**Web :** Accès HTTP au serveur web via le navigateur[cite: 63].
3.  [cite_start]**Show ip interface brief :** Vérification sur le routeur que les statuts sont "Up/Up"[cite: 73].

---
*Lab réalisé sur Cisco Packet Tracer.*
