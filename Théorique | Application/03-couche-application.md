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

---

## 3. Le Web : HTTP et HTTPS 🌐

C'est le protocole le plus connu, celui qui vous permet de lire cette page.

* **HTTP (Hypertext Transfer Protocol) :** Port 80.
    * Fonctionne en mode **Requête / Réponse**. Le client (navigateur) demande une page, le serveur l'envoie.
    * *Problème :* Tout circule en clair (mots de passe, cartes bancaires).
* **HTTPS (Secure) :** Port 443.
    * C'est du HTTP encapsulé dans du **TLS/SSL**. Tout est chiffré.

### Les Codes de Statut (Culture G)
Quand le serveur répond, il donne un code :
* **200 OK :** Tout va bien.
* **404 Not Found :** Page introuvable (Erreur client).
* **500 Internal Server Error :** Le serveur a planté (Erreur serveur).

---

## 4. Les Services d'Infrastructure (DNS & DHCP) 🏗️

Ces deux protocoles travaillent dans l'ombre mais sont indispensables pour surfer.

### 📖 DNS (Domain Name System)
* **Le problème :** Les ordinateurs ne comprennent que les adresses IP (ex: `142.250.179.14`), mais les humains retiennent des noms (ex: `google.com`).
* **La solution :** Le DNS est l'annuaire d'Internet. Il traduit les noms en IP.
* **Fonctionnement :**
    1.  Vous tapez `www.cesi.fr`.
    2.  Votre PC demande à son serveur DNS : "C'est quelle IP cesi.fr ?"
    3.  Le DNS répond : "C'est `213.32.10.5`".
    4.  Votre PC se connecte à l'IP.

### 🎁 DHCP (Dynamic Host Configuration Protocol)
* **Le problème :** Configurer manuellement l'IP, le Masque et la Passerelle sur 500 PC est impossible.
* **La solution :** Le DHCP distribue automatiquement la configuration réseau aux appareils qui se connectent.
* **Le Processus DORA :**
    1.  **D**iscover : Le PC crie "Y'a quelqu'un ? Je veux une IP !" (Broadcast).
    2.  **O**ffer : Le Serveur DHCP répond "Tiens, je te propose la 192.168.1.10".
    3.  **R**equest : Le PC répond "Ok, je la prends !".
    4.  **A**cknowledge : Le Serveur confirme "C'est noté, elle est à toi pour 24h".

---

## 📝 Résumé des Protocoles Applicatifs

| Protocole | Port (Défaut) | Rôle |
| :--- | :--- | :--- |
| **HTTP** | 80 (TCP) | Afficher des pages web (non sécurisé). |
| **HTTPS** | 443 (TCP) | Afficher des pages web (sécurisé). |
| **SMTP** | 25 (TCP) | Envoyer des emails. |
| **POP3** | 110 (TCP) | Recevoir des emails (téléchargement). |
| **IMAP** | 143 (TCP) | Recevoir des emails (synchro serveur). |
| **DNS** | 53 (UDP/TCP) | Traduire Nom ↔ IP. |
| **DHCP** | 67/68 (UDP) | Distribuer des IP automatiquement. |
