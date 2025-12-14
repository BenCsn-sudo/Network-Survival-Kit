# 01 - Les Bases Fondamentales du Réseau

> **Définition :** Un réseau est un ensemble d'entités (ordinateurs, périphériques) interconnectées qui échangent des informations via des règles communes appelées **protocoles**.

## 1. Internet vs Réseaux Privés

Il est crucial de distinguer le réseau public mondial des réseaux d'entreprise (LAN/Intranet).

| Caractéristique | 🌍 Internet (Réseau Public) | 🏢 Réseau Privé (LAN/Intranet) |
| :--- | :--- | :--- |
| **Accès** | Ouvert à tous. | Restreint et contrôlé (Authentification requise). |
| **Principe** | **Neutralité du Net** : En théorie, toutes les données sont traitées de manière égale, sans discrimination. | **Gestion de trafic (QoS)** : L'administrateur peut prioriser certains flux (ex: la visio du PDG passe avant YouTube). |
| **Objectif** | Interconnecter les réseaux du monde entier. | Sécuriser et optimiser les échanges internes d'une organisation. |

---

## 2. Les Médias d'Accès (Couche 1 - Physique)

Pour que les informations voyagent, elles ont besoin d'un support physique. On distingue deux catégories : le filaire (Guidé) et le sans-fil (Non-guidé).

### 🔌 Les Liaisons Filaires (Câbles)

C'est le moyen le plus fiable pour transporter l'information.

#### A. Le Câble Ethernet (Paire Torsadée)
C'est le standard des réseaux locaux (LAN). Il relie les terminaux (PC) aux équipements d'interconnexion (Switch/Routeur).
* **Format :** Connecteur RJ45.
* **Types de câblage :**
    * **Câble Droit :** Pour relier des équipements *différents* (ex: PC ↔ Switch).
    * **Câble Croisé :** Pour relier des équipements *de même nature* (ex: PC ↔ PC).

> 💡 **Note Technique (Auto-MDIX) :** Sur le matériel moderne, la distinction droit/croisé n'est plus critique. Les cartes réseaux possèdent la fonction **Auto-MDIX** qui détecte le type de câble et s'adapte automatiquement.

#### B. Le Câble Téléphonique
Utilisé historiquement pour l'ADSL via le modem. Il utilise souvent des connecteurs RJ11. Il tend à disparaître au profit de la Fibre Optique (FTTH).

### 📡 Les Liaisons Sans-Fil (Wireless)

L'air est le média. L'information est transportée par des ondes électromagnétiques.

#### A. Le Bluetooth (WPAN)
Technologie radio courte distance utilisant la bande de fréquence 2.4 GHz.
* **Architecture :** Fonctionne sur un mode **Maître / Esclave**.
    * **Piconet :** Un réseau formé par un maître et jusqu'à 7 esclaves actifs.
    * **Scatternet :** Interconnexion de plusieurs Piconets (un esclave peut être maître ailleurs).

<div align="center">
  <img src="https://www.tutorialspoint.com/assets/questions/media/56586/scatternet1.jpg" width="400" />
  <br>
  <i>(Exemple de structure Piconet/Scatternet)</i>
   <br>
   <br>
</div>
  

| Classe Bluetooth | Puissance | Portée estimée | Usage typique |
| :---: | :---: | :---: | :--- |
| **Classe 1** | 100 mW | ~100 m | Industriel / PC performants |
| **Classe 2** | 2.5 mW | ~10 m | **Smartphones**, Écouteurs |
| **Classe 3** | 1 mW | < 1 m | Périphériques très courte portée |

#### B. L'Infrarouge (IrDA)
Exploite la lumière pour transmettre des données.
* **Contraintes :** Nécessite une ligne de vue directe (Line-of-sight) et une courte distance (< 1m).
* **Usage :** Télécommandes (historique), échanges de données sécurisés très courte portée.

#### C. Le Wi-Fi (WLAN)
Le standard actuel pour les réseaux locaux sans fil.
* **Portée :** ~50 à 200m selon l'environnement (obstacles).
* **Débit :** Peut atteindre plusieurs Gigabits/s (Wi-Fi 6/7).
* **Avantage :** Mobilité totale des utilisateurs.

---

---

## 3. Le Matériel d'Interconnexion

Pour relier les machines entre elles, nous avons besoin d'équipements spécifiques. Ils opèrent à différents niveaux d'intelligence et de performance.

### 💻 La Carte Réseau (NIC - Network Interface Card)
C'est le composant fondamental : sans elle, aucune communication n'est possible. Elle assure l'interface entre l'ordinateur et le câble (ou les ondes).

* **Fonction :** Convertir les données numériques de l'ordinateur en signaux transmissibles sur le réseau.
* **Adresse MAC (Media Access Control) :** C'est l'identifiant **physique** et unique de la carte.
    * Contrairement à l'adresse IP (qui change selon le réseau où l'on se trouve), l'adresse MAC est gravée dans le matériel en usine.
    * *Analogie :* L'adresse MAC est votre empreinte digitale (immuable), l'adresse IP est votre adresse postale (change si vous déménagez).

> 💡 **Le saviez-vous ?** Une clé Wi-Fi USB est techniquement une carte réseau externe. Qu'elle soit branchée en USB ou via un port PCI sur la carte mère, son rôle est identique.

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/7/7e/GB_Network_PCI_Card.jpg" width="400" />
  <br>
  <i>(Exemple d'une carte réseau au format PCI)</i>
  <br><br>
</div>

### 🕸️ Le Hub (Concentrateur)
Le Hub est l'ancêtre du Switch. C'est un appareil "bête" (Couche 1 du modèle OSI).

* **Fonctionnement :** Il ne sait pas qui est qui. Quand il reçoit une information sur un port, il la répète bêtement sur **tous** les autres ports. C'est de la diffusion pure (**Broadcast**).
* **Problèmes :**
    1.  **Sécurité :** Tout le monde reçoit les messages de tout le monde (manque de confidentialité).
    2.  **Performance :** Envoie beaucoup de trafic inutile, créant des **collisions**.
* **Usage :** Quasiment disparu aujourd'hui, remplacé par le Switch.

### 🔀 Le Switch (Commutateur)
Le Switch est l'évolution intelligente du Hub (Couche 2 du modèle OSI).

* **Fonctionnement :** Il est capable d'apprendre ! Il possède une **Table MAC** qui associe chaque port à l'adresse MAC de l'ordinateur connecté.
* **Avantage :** Quand il reçoit un message pour l'ordinateur A, il l'envoie **uniquement** sur le port de l'ordinateur A (**Unicast**).
* **Résultat :** Plus de sécurité et de bien meilleures performances (fin des collisions).

<div align="center">
  <img src="https://content.pearl.fr/media/cache/default/article_ultralarge_high_nocrop/shared/images/articles/T/TG3/switch-reseau-gigabit-ethernet-24-ports-gs234v2-ref_TG3134_2.jpg" width="600" />
  <br>
  <i>(Différence de flux de données : Hub vs Switch)</i>
  <br><br>
</div>

### 🌐 Le Routeur
Si le Switch crée un réseau, le Routeur relie **différents** réseaux entre eux.

* **Rôle principal :** Il agit comme une passerelle (Gateway). C'est lui qui permet à votre réseau local (LAN) de parler à Internet (WAN).
* **Fonctionnement :** Il ne se base pas sur les adresses MAC, mais sur les **adresses IP** (Couche 3 du modèle OSI). Il détermine le meilleur chemin pour acheminer les paquets vers leur destination finale.
* **À la maison :** Votre "Box" internet est en réalité un routeur qui possède aussi un switch intégré (les 4 ports derrière) et un point d'accès Wi-Fi.

<div align="center">
  <img src="https://dropinblog.net/34253577/files/featured/composants-routeur-3.png" width="600" />
  <br>
  <i>(Un routeur Wi-Fi moderne typique)</i>
  <br><br>
</div>

### 📣 Le Répéteur (Repeater)
Le signal réseau s'affaiblit avec la distance (atténuation). Le répéteur sert à contrer ce problème.

* **Rôle :** Il reçoit un signal, le "nettoie" et le **régénère** à sa puissance maximale pour l'envoyer plus loin.
* **Attention :** En Wi-Fi, l'utilisation d'un répéteur peut augmenter la latence et diviser le débit par deux (car il doit écouter puis répéter, il ne peut pas faire les deux en même temps parfaitement).

---

## 📝 Bilan du Matériel

| Équipement | Couche OSI | Intelligence | Rôle principal |
| :--- | :---: | :--- | :--- |
| **Carte Réseau (NIC)** | 1 & 2 | Moyenne | Fournir l'adresse MAC et l'accès physique au réseau. |
| **Hub** | 1 (Physique) | Nulle | Connecter plusieurs PC (envoie tout à tout le monde). |
| **Switch** | 2 (Liaison) | Moyenne | Connecter plusieurs PC intelligemment (utilise l'adresse MAC). |
| **Routeur** | 3 (Réseau) | Élevée | Connecter des réseaux différents (LAN ↔ Internet). |
| **Répéteur** | 1 (Physique) | Nulle | Étendre la portée du signal (Régénération). |

---

## 4. Échelles et Topologies

Avant de dessiner un réseau, il faut comprendre son étendue géographique et sa forme.

### 🌍 Les Types de Réseaux (Échelle)

On classe généralement les réseaux selon la distance qui sépare les équipements.

#### LAN (Local Area Network)
C'est un réseau limité à une zone géographique restreinte (une maison, un bâtiment, un campus).
* **Technologies :** Ethernet, Wi-Fi (on parle alors de **WLAN**).
* **Vitesse :** Très rapide (1 Gbps à 10 Gbps).
* **Gestion :** Privée (géré par vous ou l'entreprise).

#### WAN (Wide Area Network)
C'est un réseau étendu qui couvre de grandes distances (pays, continent). Il sert à interconnecter plusieurs LAN.
* **Technologies :** Fibre optique longue distance, 4G/5G, Satellite, MPLS.
* **Vitesse :** Variable, souvent plus lente que le LAN.
* **Gestion :** Publique ou louée à des opérateurs (FAI).

<div align="center">
  <img src="https://www.lafibrelyonnaise.fr/wp-content/uploads/2022/04/Que-signifie-et-quel-est-lobjectif-du-WAN-ou-reseau-etendu-en-informatique.jpg" width="500" />
  <br>
  <i>(LAN vs WAN : Le WAN connecte les LANs entre eux)</i>
  <br><br>
</div>

---

### 📐 Les Topologies : Physique vs Logique

C'est une distinction fondamentale à comprendre pour un ingénieur réseau.

* **Topologie Physique :** C'est le câblage réel. Comment les machines sont reliées physiquement (la forme des câbles dans les murs).
* **Topologie Logique :** C'est la manière dont le signal circule. Comment les données "voient" le réseau.

> **Exemple clé :** Si vous branchez des PC sur un **Hub**, vous avez une étoile physique (tous les câbles vont au Hub), mais un bus logique (le signal est envoyé à tout le monde comme dans un bus).

### 🚌 Topologie en Bus
Tous les ordinateurs sont connectés à un seul câble central (souvent coaxial) appelé le segment.

* **Fonctionnement :** Le signal parcourt tout le câble. Il faut des **bouchons de terminaison** (terminateurs) aux extrémités pour éviter que le signal ne rebondisse (réflexion).
* **Problème majeur :**
    * Si le câble coupe à un endroit, **tout le réseau tombe**.
    * **Collisions :** Si deux personnes parlent en même temps, les données se percutent. On utilise le protocole **CSMA/CD** pour gérer ça.

### ⭐ Topologie en Étoile (Star)
C'est le standard actuel (Ethernet moderne). Tout le monde est relié à un point central (Switch).

* **Avantages :** Si un câble casse, seul l'ordinateur concerné est déconnecté. Le reste du réseau fonctionne.
* **Inconvénient :** Le point central est un **SPOF** (Single Point of Failure). Si le Switch meurt, plus personne ne communique.

<div align="center">
  <img src="https://zestedesavoir.com/media/galleries/5382/aae26575-30b8-46e4-8fe2-13bb42d5fa96.png" width="300" />
  <br>
  <i>(Topologie en étoile : la plus courante)</i>
  <br><br>
</div>

### 💍 Topologie en Anneau (Ring)
Les machines sont reliées en boucle fermée.
* **Fonctionnement (Token Ring) :** Un jeton (Token) circule d'ordinateur en ordinateur.
    * Celui qui a le jeton a le droit de parler.
    * Quand il a fini, il passe le jeton au voisin.
* **Avantage :** Aucune collision ! C'est une méthode **déterministe**.
* **Inconvénient :** Si une machine plante (et ne passe pas le jeton), l'anneau est brisé (sauf s'il y a un double anneau de secours comme en FDDI).

### 🕸️ Topologie Maillée (Mesh)
Chaque machine est reliée à toutes les autres (Full Mesh) ou à plusieurs autres (Partial Mesh).

* **Le Full Mesh (Intégral) :**
    * **Sécurité maximale :** Si un lien casse, il y en a toujours un autre.
    * **Calcul du nombre de câbles :** Pour $n$ machines, il faut $\frac{n(n-1)}{2}$ câbles.
    * *Exemple :* Pour 500 ordis, il faudrait 124 750 câbles ! C'est impossible à gérer pour des PC.
* **Usage réel :** On l'utilise pour le "Backbone" d'Internet (les gros routeurs) pour qu'Internet ne coupe jamais, mais on fait du maillage partiel.

<div align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/36/NetworkTopologies.svg/1000px-NetworkTopologies.svg.png" width="600" />
  <br>
  <i>(Synthèse visuelle des topologies)</i>
  <br><br>
</div>

### 🧬 Topologie Hybride
C'est la réalité du terrain. Internet est un réseau hybride : des étoiles (vos maisons), reliées par des boucles ou des maillages (les opérateurs).

---
*Prochain chapitre : Protocoles, Modèle OSI et Encapsulation !*
