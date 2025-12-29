# 🛠️ Boîte à Outils (Network Toolbox)

Bienvenue dans la zone pratique. Ce dossier regroupe les **Cheat Sheets** (Fiches de triche) et les guides de survie pour manipuler les équipements et analyser le réseau.

Un ingénieur réseau doit maîtriser deux mondes : l'infrastructure (les équipements qui transportent la donnée) et les terminaux (les machines qui envoient la donnée).

---

## 📂 Contenu du dossier

### 1. [🔵 Cisco IOS (Infrastructure)](./Cisco-IOS.md)
> **Pour qui ?** Routeurs et Switchs Cisco.
> **Pour quoi ?** Configurer le réseau, le routage et la commutation.

C'est votre guide de référence pour le système d'exploitation **Cisco IOS**.
* Configuration de base (Nom, Mots de passe, SSH).
* Configuration des interfaces (IP, SVI).
* Commandes de vérification (`show ip interface brief`, `show run`, etc.).
* Câblage Packet Tracer.

👉 **[Accéder à la Cheat Sheet Cisco](./Cisco-IOS.md)**

---

### 2. [🐧 Linux Network (Analyse & Dépannage)](./Linux-Network.md)
> **Pour qui ?** PC, Serveurs, Debian/Ubuntu/Mint.
> **Pour quoi ?** Tester la connectivité, diagnostiquer une panne et vérifier les services.

C'est votre guide de workflow pour dépanner un réseau depuis un terminal **Linux**.
* Commandes modernes (`ip`, `ss`, `dig`).
* Méthodologie de dépannage couche par couche (L1 → L7).
* Outils d'audit de ports et de services.

👉 **[Accéder au Guide Linux](./Linux-Network.md)**

---

## ⚡ Quelle fiche utiliser ?

| Situation | Fiche recommandée |
| :--- | :--- |
| "Je dois configurer un routeur neuf." | **[Cisco IOS](./Cisco-IOS.md)** |
| "Mon serveur n'arrive pas à pinger Internet." | **[Linux Network](./Linux-Network.md)** |
| "Je dois changer le VLAN d'un port." | **[Cisco IOS](./Cisco-IOS.md)** |
| "Je veux savoir si le port 80 est ouvert." | **[Linux Network](./Linux-Network.md)** |
