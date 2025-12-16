# 03 - La Couche Application (et ses amies)

Dans le modèle TCP/IP, les couches OSI 5, 6 et 7 sont regroupées en une seule couche : **Application**. C'est la couche la plus proche de l'utilisateur.

## 1. Vue d'ensemble des couches hautes

Avant d'attaquer les protocoles, comprenons le rôle de ces trois étages.

### 🎭 Couche 6 : Présentation
Elle est le "traducteur" du réseau.
* **Formatage :** Convertir des données (ex: EBCDIC vers ASCII).
* **Compression :** Réduire la taille des fichiers (ex: ZIP).
* **Chiffrement :** Sécuriser les données (ex: SSL/TLS).

### 🤝 Couche 5 : Session
Elle est le "chef de gare" du dialogue.
* **Gestion :** Ouvrir, maintenir et fermer la session entre deux applications.
* **Modes de communication :**
    * **Simplex :** Sens unique (ex: Télévision).
    * **Half-Duplex :** Chacun son tour (ex: Talkie-Walkie).
    * **Full-Duplex :** En même temps (ex: Téléphone).

---

## 2. Étude de Cas : Le Service de Messagerie (E-mail) 📧

Le courrier électronique est l'exemple parfait pour comprendre la couche application. Il repose sur plusieurs protocoles distincts pour envoyer et recevoir.

### 📤 L'Envoi : SMTP (Simple Mail Transfer Protocol)
SMTP est un protocole "Pousseur" (Push). Il sert à **envoyer** le courrier d'un serveur à un autre.

> **Analogie Postale :**
> * **MUA (Mail User Agent) :** C'est vous qui écrivez la lettre (Outlook, Gmail).
> * **MSA (Mail Submission Agent) :** C'est le facteur qui récupère votre lettre.
> * **MTA (Mail Transfer Agent) :** C'est le centre de tri postal qui envoie la lettre à l'autre bout du monde.
> * **MDA (Mail Delivery Agent) :** C'est le facteur qui dépose la lettre dans la boîte du destinataire.

#### Le Trajet d'un E-mail
Voici ce qu'il se passe quand Pierre (Paris) écrit à André (Lyon) :

```mermaid
flowchart LR
    A["Pierre (MUA)"] -->|SMTP| B("Serveur Départ MSA/MTA")
    B -->|SMTP| C{"Internet"}
    C -->|SMTP| D("Serveur Arrivée MTA")
    D -->|Stockage| E["Boîte aux Lettres MDA"]
    E -.->|POP/IMAP| F["André (MUA)"]
    
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style F fill:#bfb,stroke:#333,stroke-width:2px
````

  * **Note :** SMTP est utilisé de bout en bout pour le transport.
  * **Mais...** SMTP ne sait pas "donner" le courrier au destinataire final. Il le dépose juste dans la boîte (le serveur).

### 📥 La Réception : POP vs IMAP

Pour récupérer le courrier stocké sur le serveur, le destinataire doit utiliser un protocole de "Retrait" (Pull). Il y a deux écoles :

| Caractéristique | 📬 POP (Post Office Protocol) | ☁️ IMAP (Internet Message Access Protocol) |
| :--- | :--- | :--- |
| **Philosophie** | "Je prends tout et je rentre chez moi." | "Je lis sur place." |
| **Fonctionnement** | Télécharge les mails sur l'ordinateur et les **efface** souvent du serveur. | Synchronise les mails. Ils restent sur le serveur. |
| **Multi-Appareils** | ❌ Mauvais. Si téléchargé sur PC, plus visible sur téléphone. | ✅ Excellent. Tout est synchro partout (lu/non lu). |
| **Connexion** | Utile si on n'a pas internet tout le temps. | Nécessite une connexion constante pour lire. |
| **Usage Moderne** | De moins en moins utilisé. | **Standard actuel.** |

### 🔒 Et la sécurité ?

SMTP, POP et IMAP transmettent le texte en clair par défaut. C'est dangereux.
Aujourd'hui, on les encapsule presque toujours dans **SSL/TLS** (Couche Présentation) :

  * SMTP devient **SMTPS** (Port 465/587)
  * IMAP devient **IMAPS** (Port 993)
  * POP devient **POP3S** (Port 995)
