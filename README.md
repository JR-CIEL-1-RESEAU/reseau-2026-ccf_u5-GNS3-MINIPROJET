# 🌐 TP GNS3 : Architecture Réseau & Sécurité (BTS CIEL)

Ce dépôt contient les instructions pour la mise en œuvre d'une infrastructure réseau virtualisée sous **GNS3**. L'objectif est de simuler un environnement professionnel sécurisé pour la gestion des stocks de matériel du **BTS CIEL**.

---

## 📝 Le Sujet : Système de Gestion de Stock Sécurisé

L'étudiant doit déployer une application web dynamique (LAMP) permettant la gestion du matériel. Ce système doit inclure une base de données, une authentification par compte utilisateur et des fonctionnalités de mise à jour des données. La sécurité est assurée par un Firewall filtrant les accès selon l'origine des flux.

### Composants de la simulation :
#### 📂 Accès aux ressources pour créer vos noeuds réseaux GNS3

Copiez et collez le chemin suivant dans la barre d'adresse de votre explorateur de fichiers Windows :

`\\nas1-ciel\CIEL1\Machines Virtuelles pour les étudiants\GNS3-MINIPROJET`

> [!IMPORTANT]
> **Conseil :** Copiez les fichiers sur votre disque local (**E:**) avant de les importer dans GNS3 pour éviter les ralentissements réseau.
* **Serveur LAMP :** Instance Linux (Debian/Ubuntu) configurée avec Apache2, MariaDB/MySQL, PHP et OpenSSH.
* **Application Web :** Site dynamique avec page de connexion (login), affichage de l'inventaire, ajout de matériel (Insert) et mise à jour des quantités (Update).
* **Firewall (pfSense ou Fortinet) :** Pivot de la sécurité réseau connecté directement au serveur web.
* **Infrastructure Cisco :** Un Routeur et un Switch émulés pour la segmentation et la distribution du LAN.
* **Postes Clients :** Trois profils distincts (PC_A, PC_B, PC_DEV) sous forme de conteneurs Docker ( post client gns3/webterm )

---

## 🏗️ Architecture et Flux

1.  **Segment Serveur :** Le serveur LAMP est connecté directement au Firewall.
2.  **Segment Sécurité :** Le Firewall est relié au Routeur Cisco.
3.  **Segment LAN :** Le Routeur est relié au Switch, desservant les PC clients.

### Matrice de flux (Politique de sécurité) :

| Source | Destination | Protocole/Service | Action |
| :--- | :--- | :--- | :--- |
| **PC_A** (Consultant) | Serveur LAMP | HTTP (80) | **Autoriser** |
| **PC_B** (Non autorisé) | Serveur LAMP | TOUS | **Bloquer** |
| **PC_DEV** (Développeur) | Serveur LAMP | HTTP (80) + SFTP/SSH (22) | **Autoriser** |

---

## 📍 Plan d'adressage (À compléter)

*Note : Définissez vos réseaux et adresses avant de débuter la configuration technique.*

| Équipement | Interface | Adresse IP | Masque | Passerelle |
| :--- | :--- | :--- | :--- | :--- |
| **Serveur LAMP** | eth0 | | | |
| **Firewall** | côté Serveur | | | N/A |
| **Firewall** | côté Routeur | | | |
| **Routeur Cisco** | vers Firewall | | | |
| **Routeur Cisco** | vers Switch | | | N/A |
| **PC_A** | eth0 | | | |
| **PC_B** | eth0 | | | |
| **PC_DEV** | eth0 | | | |

---

## 🚀 Étapes de réalisation

### 1. Déploiement du Serveur LAMP & BDD
* Installation de la pile **LAMP** (Linux Apache MariaDB PHP) et du service **SSH**.
* **Base de données :** Création d'une table `users` (comptes avec mots de passe) et d'une table `stock`.
* **Application PHP :** Développement de l'interface incluant :
    * Formulaire de login pour authentifier les utilisateurs.
    * Affichage dynamique du stock.
    * Fonctions de modification (ajout de matériel et incrémentation des quantités).

### 2. Configuration Réseau (Cisco)
* Adressage des interfaces du **Routeur** et mise en place du routage vers le Firewall.
* Configuration du **Switch** (VLANs ou ports d'accès) pour connecter les trois PC.

### 3. Configuration Sécurité (Firewall)
* Paramétrage des interfaces et des zones réseaux.
* Mise en place des règles de filtrage (ACL / Policies) basées sur les adresses IP sources pour respecter la matrice de flux.

### 4. Test et Maintenance
* Validation des accès web pour **PC_A** et **PC_DEV**.
* Test de mise à jour du site via SFTP depuis le poste **PC_DEV** (vérification de l'écriture des fichiers).

---

## 🏁 Fiche de Recette (Critères de validation)

| Catégorie | Point de contrôle | Validé (✅) | Note |
| :--- | :--- | :---: | :--- |
| **Réseau** | Topologie fonctionnelle et sauvegarde des confs Cisco | ☐ | |
| | Plan d'adressage complet et cohérent | ☐ | |
| **Application** | Authentification réussie avec gestion de sessions | ☐ | |
| | Ajout (Insert) et Mise à jour (Update) opérationnels | ☐ | |
| **Sécurité** | PC_A accède au site / PC_B est totalement rejeté | ☐ | |
| | Preuve de blocage visible dans les logs du Firewall | ☐ | |
| **Admin** | Mise à jour du site via SFTP depuis PC_DEV réussie | ☐ | |

---
