# 📂 Étude de Cas : Mise en œuvre de l'Adressage IPv6

> **Module 12** : Ce projet est une application pratique des concepts d'adressage IPv6 (GUA et LLA) sur une topologie multi-périphériques.

## 📝 Contexte du Projet

Dans cet exercice, l'objectif est de déployer une connectivité IPv6 complète sur un réseau comprenant un routeur central (R1), des serveurs de services (Comptabilité, CAO) et des clients finaux (Ventes, Ingénierie). Ce lab met l'accent sur la cohabitation des adresses **Unicast Globales** (pour le routage) et des adresses **Link-Local** (pour la communication de voisinage).

## 🎯 Objectifs de configuration

1. **Activation du routage IPv6** : Activer le transfert de paquets sur le routeur R1.
2. **Adressage du Routeur** : Configurer les interfaces GigabitEthernet et Serial avec des adresses globales et locales de liaison.
3. **Adressage Hôtes & Serveurs** : Configuration statique des clients et serveurs avec leurs passerelles par défaut respectives.
4. **Vérification de connectivité** : Tests de navigation Web et requêtes ICMPv6 vers l'ISP.

## ⚙️ Logique Technique & Commandes Clés

Le point le plus critique de ce TP est l'activation du routage. Sans la commande suivante, le routeur ne transmettra pas les paquets IPv6, même si les adresses sont correctement configurées sur les interfaces:

```bash
R1(config)# ipv6 unicast-routing

```

Pour la vérification rapide de l'état des interfaces et des adresses configurées, on utilise:

```bash
R1# show ipv6 interface brief

```

> **Note sur la modification** : Contrairement à l'IPv4, si vous vous trompez d'adresse en IPv6, vous devez supprimer l'ancienne adresse avec `no ipv6 address [adresse]` car une interface peut posséder plusieurs adresses IPv6 simultanément.

## 📁 Fichiers du projet

* 📄 **[Instructions_IPv6.pdf](https://www.google.com/search?q=./12.6.6-packet-tracer---configure-ipv6-addressing_fr-FR.pdf)** : Guide officiel détaillé des étapes de configuration.
* 💻 **[Maquette_Initiale.pkt](https://www.google.com/search?q=./Initial_Lab_12-6-6.pkt)** : Topologie vierge fournie au début de l'étude.
* ✅ **[Maquette_Finale_TERMINEE.pkt](https://www.google.com/search?q=./Final_Lab_12-6-6.pkt)** : Résultat final avec connectivité 100% opérationnelle.
