# Étude de Cas : Segmentation et Configuration de Réseaux IPv4

## 1. Introduction

Ce projet pratique porte sur la **segmentation d'un espace d'adressage IPv4** (Subnetting) et la **configuration globale** des équipements d'un réseau local d'entreprise. L'objectif principal est d'optimiser l'utilisation des adresses IP tout en assurant une connectivité totale entre les différents sous-réseaux et l'accès vers un fournisseur d'accès Internet (FAI).

## 2. Contexte et Problématique

Le réseau client utilise l'adresse de base `192.168.0.0/24`. Les besoins spécifiques du client imposent de diviser ce réseau en respectant les contraintes suivantes :

* **LAN-A** : 50 hôtes minimum.
* **LAN-B** : 40 hôtes minimum.
* **Évolutivité** : Prévoir au moins deux sous-réseaux supplémentaires pour une expansion future.
* **Contrainte technique** : Utilisation de masques de longueur fixe (FLSM) pour l'ensemble des sous-réseaux.

## 3. Solution Technique : Le choix du masque /26

Pour répondre au besoin du plus grand sous-réseau (50 hôtes), le masque **/26 (255.255.255.192)** a été retenu.

* **Pourquoi ?** Un masque /26 offre **62 adresses hôtes utilisables** par sous-réseau (), ce qui couvre parfaitement les besoins de 50 et 40 hôtes.
* **Avantage** : Ce découpage permet de créer exactement **4 sous-réseaux**, satisfaisant ainsi les besoins immédiats (LAN-A, LAN-B) et les besoins futurs (2 sous-réseaux libres).

## 4. Réalisations techniques

Le projet a été mené en trois phases majeures :

### A. Conception du plan d'adressage

Calcul des adresses réseaux, des plages d'adresses IP utilisables et des adresses de diffusion (broadcast) pour chaque segment.

### B. Configuration des équipements (Cisco IOS)

* **CustomerRouter** : Configuration du nom d'hôte, sécurisation de l'accès (Passerelle secret/console) et activation des interfaces `G0/0` et `G0/1` avec leurs passerelles respectives.
* **Commutateurs (S1/S2)** : Configuration des interfaces VLAN 1 pour la gestion à distance et définition des passerelles par défaut.
* **Hôtes (PC-A/PC-B)** : Attribution des adresses IP statiques et configuration des passerelles par défaut.

### C. Tests et Validation

* Vérification de la connectivité locale par des requêtes **Ping**.
* Validation de la communication inter-VLAN et de l'accès vers le serveur FAI.

## 5. Contenu du dépôt

Ce dossier contient les fichiers suivants pour suivre l'évolution du projet :

* 📄 **[Sujet-TP.pdf](Sujet.pdf)** : L'énoncé complet avec la table d'adressage à compléter.
* 🌐 **[Maquette-Origine](Maquette-Origine.pka)** : Le fichier Packet Tracer initial (périphériques non configurés).
* ✅ **[Maquette-Finale](Maquette-Finale.pka)** : Le fichier Packet Tracer finalisé avec adressage complet et connectivité validée.

---
