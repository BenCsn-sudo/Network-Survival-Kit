# 13 - Fondamentaux de la Sécurité Réseau 🛡️

> **Objectif :** Protéger la triade **CIA** (Confidentialité, Intégrité, Disponibilité) des données et des infrastructures contre les menaces internes et externes.

---

## 1. Menaces et Vulnérabilités ⚠️

Pour sécuriser un réseau, il faut comprendre ce qu'on combat.

### Les types de vulnérabilités

1. **Technologiques :** Faiblesses des protocoles (ex: TCP/IP), des OS ou du matériel.
2. **Configuration :** Comptes par défaut non modifiés, services inutilisés actifs.
3. **Politiques :** Absence de sensibilisation des utilisateurs, absence de plan de secours.

### Sécurité Physique (Les 4 classes de menaces)

* **Matérielles :** Dommages sur les serveurs, câbles ou switchs.
* **Environnementales :** Températures extrêmes ou humidité.
* **Électriques :** Surtensions, baisses de tension ou coupures.
* **Maintenance :** Mauvais étiquetage, manque de pièces de rechange.

---

## 2. Les Attaques Réseau : "The Dark Side" 🌑

### Programmes malveillants (Malwares)

* **Virus :** Nécessite un programme hôte et une action humaine pour se propager.
* **Ver (Worm) :** Autonome. S'auto-réplique et se propage via les failles réseau.
* **Cheval de Troie :** Logiciel utile en apparence qui cache un code malveillant.

### Types d'attaques

| Attaque | Description | Exemples |
| --- | --- | --- |
| **Reconnaissance** | Découverte non autorisée de systèmes et services. | `nslookup`, `nmap` (scans de ports). |
| **Accès** | Manipulation de données ou privilèges d'accès. | Attaques par dictionnaire (MDP), MitM (Man-in-the-Middle). |
| **Déni de Service (DoS)** | Rendre un service inaccessible en le submergeant. | Ping of Death, SYN Flood, DDoS (Distribué). |

---

## 3. Stratégies d'Atténuation 🛡️

### Défense en profondeur (Layered Defense)

On ne mise pas tout sur un seul équipement. On multiplie les couches : Pare-feu → IPS → Antivirus → Sécurité des hôtes → Politiques humaines.

### Le Modèle AAA

* **Authentification :** "Qui êtes-vous ?" (Identifiant/Mot de passe).
* **Autorisation :** "Qu'avez-vous le droit de faire ?" (Permissions).
* **Comptabilité (Accounting) :** "Qu'avez-vous fait ?" (Logs/Traçabilité).

### Pare-feu (Firewalls)

Il existe plusieurs types selon le niveau de filtrage :

1. **Filtrage de paquets :** Bloque selon l'IP ou le Port (Couche 3/4).
2. **Stateful Inspection :** Surveille l'état des connexions (vérifie si une réponse correspond à une requête sortante).
3. **Application (Next-Gen) :** Analyse le contenu des messages (Couche 7).

---

## 4. Hardening : Sécurisation des périphériques Cisco 🔒

C'est la partie "pratique" indispensable pour la configuration de tes équipements.

### Sécurisation des mots de passe

```bash
! Fixer une longueur minimale pour tous les mots de passe
(config)# security password min-length 10

! Empêcher les attaques par force brute (bloquer 3 min après 3 échecs en 60s)
(config)# login block-for 180 attempts 3 within 60

! Déconnexion automatique après 5 minutes d'inactivité sur la console
(config-line)# exec-timeout 5 0

```

### Activation de SSH (Adieu Telnet 💀)

Telnet envoie les mots de passe en clair. **SSH les chiffre.**

```bash
! 1. Le routeur doit avoir un nom et un domaine
(config)# hostname R1
(config)# ip domain-name monreseau.local

! 2. Créer un utilisateur local avec privilèges max
(config)# username admin privilege 15 secret MonMDPFort

! 3. Générer les clés de chiffrement (RSA)
(config)# crypto key generate rsa
! (Choisir 1024 ou 2048 bits)

! 4. Forcer la version 2 de SSH (plus sécurisée)
(config)# ip ssh version 2

! 5. Configurer les lignes VTY pour n'accepter QUE le SSH
(config)# line vty 0 15
(config-line)# transport input ssh
(config-line)# login local

```

### Services inutilisés

Il est conseillé de désactiver les services qui peuvent servir de vecteurs d'attaque :

* `no ip http server` (si l'interface web n'est pas utilisée).
* `no cdp run` (sur les ports connectés à des réseaux externes).
