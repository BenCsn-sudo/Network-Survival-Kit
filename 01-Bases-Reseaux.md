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
  <img src="https://www.tutorialspoint.com/assets/questions/media/56586/scatternet1.jpg" width="600" />
  <br>
  <i>(Exemple de structure Piconet/Scatternet)</i>  
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
*Suite du chapitre : Matériel d'interconnexion (Hub, Switch, Routeur)...*
