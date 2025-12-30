# Dépannage de passerelle par défaut

> Ce dépôt contient la résolution de l'étude de cas **10.3.5** du cursus **CCNA : Introduction to Networks (ITN)**. L'objectif de ce laboratoire est d'analyser une architecture réseau documentée mais dysfonctionnelle,
> d'identifier les erreurs de configuration et de rétablir la connectivité de bout en bout.

## 📄 Contenu du dépôt

* **`10.3.5-troubleshoot-gateway-initial.pkt`** : Le fichier Packet Tracer original (maquette de base avec les pannes).
* **`10.3.5-troubleshoot-gateway-fixed.pkt`** : Le fichier Packet Tracer final (maquette corrigée et fonctionnelle).
* **`10.3.5-packet-tracer---troubleshoot-default-gateway-issues_fr-FR.pdf`** : Les instructions officielles et la table d'adressage de l'exercice.

## 📝 Contexte et Problématique

Pour qu'un périphérique puisse communiquer au-delà de son réseau local, il doit disposer d'une configuration IP correcte incluant une adresse IP, un masque de sous-réseau et surtout une **passerelle par défaut**. Cette passerelle correspond généralement à l'interface du routeur connectée au réseau local.

Dans ce scénario, le réseau souffre de problèmes de connectivité. Bien que les voyants de liaison (Liaison verte) soient actifs sur les commutateurs, les communications entre les différents VLANs et réseaux sont impossibles. Le défi consiste à compléter la documentation réseau manquante et à dépanner méthodiquement chaque équipement.

## 🛠️ Actions réalisées

La résolution s'est déroulée en deux phases principales : l'analyse de la documentation et la mise en œuvre des corrections.

### 1. Analyse et Diagnostic

* **Audit de la documentation :** Comparaison de la topologie logique avec la table d'adressage fournie. Identification des informations manquantes concernant les passerelles par défaut pour les commutateurs et les PC.
* **Tests de connectivité :** Réalisation de tests (Ping) locaux (ex: PC1 vers PC2) et distants (ex: PC1 vers PC4) pour isoler les points de défaillance.
* **Vérification des configurations :** Inspection des configurations IP des hôtes et des commutateurs.

### 2. Corrections Apportées

* **Correction de l'adressage des hôtes :** Identification et rectification de l'adresse IP erronée sur **PC1**, qui ne correspondait pas à la documentation réseau.
* **Configuration des PC :** Ajout des passerelles par défaut manquantes sur les postes de travail (PC1, PC2, PC3, PC4) pour permettre le routage des paquets vers les réseaux distants.
* **Configuration des Commutateurs (S1 et S2) :** Configuration de la passerelle par défaut sur les commutateurs via la commande globale `ip default-gateway`. Cette étape, bien que non critique pour le trafic utilisateur, est essentielle pour la gestion à distance du commutateur depuis un autre réseau.

## 🚀 Compétences validées

* Compréhension du rôle de la passerelle par défaut (Default Gateway).
* Dépannage de la connectivité réseau (Couche 3).
* Analyse de tables d'adressage et documentation.
* Configuration de base des équipements Cisco (IOS).

---
