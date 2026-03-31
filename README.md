🍎 Projet Architecture 3-Tiers Sécurisée - "Le Verger"
Ce projet présente une infrastructure réseau complète segmentée en trois couches (Web, Firewall, Database) permettant la gestion d'un stock de fruits via une application Node.js et une base de données MariaDB.

🏗️ Architecture du Réseau
L'infrastructure est composée de trois Machines Virtuelles (VM) isolées par un Firewall :

VM1 : Firewall (Passerelle)

Rôle : Contrôle tous les flux entrants et sortants.

Interfaces : * NAT : Accès Internet.

LAN Web : 192.168.100.1

LAN DB : 192.168.10.1

VM2 : Serveur Web

IP : 192.168.100.10

Service : Application Node.js (Express) sur le port 80.

VM3 : Base de Données

IP : 192.168.10.10

Service : MariaDB sur le port 3306.

🛡️ Configuration de la Sécurité (iptables)
Le Firewall applique une politique de Default DROP. Seuls les flux strictement nécessaires sont autorisés :

Flux HTTP (80) : Autorisé de l'extérieur vers le serveur Web.

Flux SQL (3306) : Autorisé uniquement du serveur Web vers la base de données.

Stateful Inspection : Suivi des connexions pour autoriser les paquets de retour (ESTABLISHED).

Les règles sont sauvegardées dans le fichier rules.v4.

📂 Structure du Dépôt
/vm2-web : Contient le code source de l'application (server.js) et la configuration réseau netplan.

/vm3-bd : Contient les scripts SQL de création de la base et la configuration mariadb-server.cnf.

rules.v4 : Exportation des règles de filtrage du Firewall.

🚀 Installation Rapide
1. Base de Données (VM3)
Importez le schéma de données :

Bash
sudo mariadb -u root < database_setup.sql
2. Serveur Web (VM2)
Installez les dépendances et lancez l'application :

Bash
cd vm2-web
npm install
sudo node server.js
3. Firewall (VM1)
Appliquez les règles de filtrage :

Bash
sudo iptables-restore < rules.v4
📊 Fonctionnalités
Affichage en temps réel du stock de fruits.

Ajout de nouveaux fruits via un formulaire.

Mode Démo : En cas de coupure de flux par le firewall, l'application affiche des données de secours pour garantir la disponibilité du service.
