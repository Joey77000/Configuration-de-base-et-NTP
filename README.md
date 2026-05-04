# Configuration-de-base-et-NTP
Mission 2 – Configuration de base et NTP

Chaque équipement réseau est configuré avec des paramètres de base.

Les configurations réalisées sont :

attribution d’un nom d’hôte ;
configuration du mot de passe privilégié ;
configuration du mot de passe console ;
création d’un utilisateur administrateur ;
activation de SSH ;
génération d’une clé RSA ;
synchronisation horaire avec NTP.

Exemple de configuration :

enable
conf t
hostname routeurCRASH
enable secret class
line console 0
password cisco
login
ip domain-name crash.local
crypto key generate rsa
username admin password toto
line vty 0 4
login local
transport input ssh
ntp server 172.16.0.100
end
wr

SSH permet d’administrer les équipements à distance de manière sécurisée.
NTP permet de synchroniser l’heure des équipements, ce qui est important pour l’analyse des journaux.

Mission 3 – Réinitialisation des mots de passe Cisco

Cette mission consiste à apprendre la procédure de récupération d’accès sur des équipements Cisco.

Sur un routeur, la récupération passe par le mode ROMMON avec la commande :

confreg 0x2142

Après modification du mot de passe, le registre est remis à sa valeur normale :

config-register 0x2102

Sur un switch, la procédure se fait au démarrage avec le bouton Mode, puis avec l’initialisation de la mémoire flash.

Cette mission permet de comprendre comment récupérer un équipement en cas d’oubli de mot de passe.

Mission 4 – Serveur Syslog

Un serveur Syslog est mis en place afin de centraliser les journaux des équipements réseau.

Configuration utilisée :

configure terminal
logging 192.168.1.100
logging on
logging trap informational
service timestamps log datetime msec
end
write memory

Syslog permet de conserver une trace des événements réseau.
Cela facilite le diagnostic, la supervision et la détection d’incidents.

Mission 5 – ACL sur le routeur

Les ACL permettent de filtrer le trafic réseau.

Dans ce projet, les ACL servent à :

autoriser le trafic HTTP et HTTPS ;
autoriser certains flux ICMP ;
bloquer les flux non autorisés ;
journaliser les tentatives bloquées.

Exemple de configuration :

ip access-list extended ACL_LAN_IN
permit tcp 172.17.0.0 0.0.255.255 any eq 80
permit tcp 172.17.0.0 0.0.255.255 any eq 443
permit icmp 172.17.0.0 0.0.255.255 any echo
deny ip any any log

interface GigabitEthernet0/0
ip access-group ACL_LAN_IN in

Les ACL permettent de renforcer la sécurité du réseau en contrôlant les communications autorisées.

Mission 6 – VLAN, VTP et routage inter-VLAN

Les VLAN permettent de séparer le réseau en plusieurs zones logiques.

Exemple de création de VLAN :

vlan 10
name ESCRIME

vlan 20
name VOLLEY

Le routage inter-VLAN est réalisé avec la méthode Router-on-a-Stick.

Exemple :

interface GigabitEthernet0/0.10
encapsulation dot1Q 10
ip address 172.17.10.254 255.255.255.0

interface GigabitEthernet0/0.20
encapsulation dot1Q 20
ip address 172.17.20.254 255.255.255.0

Chaque sous-interface correspond à un VLAN et sert de passerelle par défaut pour les postes de ce VLAN.

Mission 7 – EtherChannel LACP

EtherChannel permet de regrouper plusieurs liens physiques en un seul lien logique.

Cette technologie permet :

d’augmenter la bande passante ;
d’améliorer la disponibilité ;
de maintenir la communication si un lien tombe.

Exemple de configuration :

interface range Fa0/1 - 2
switchport mode trunk
channel-group 1 mode active

Vérification :

show etherchannel summary
Mission 8 – Sécurisation des ports

La sécurité des ports permet d’empêcher les connexions non autorisées.

Exemple de configuration :

interface range Fa0/2 - 10
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security violation protect
switchport port-security mac-address sticky
no shutdown

Cette configuration limite chaque port à une seule adresse MAC et permet de bloquer les équipements non autorisés.

Tests réalisés

Les tests suivants ont été réalisés :

ping
show ip route
show vlan brief
show interfaces trunk
show etherchannel summary
show access-lists
show port-security

Ces commandes permettent de vérifier la connectivité, les VLAN, les trunks, les ACL, l’EtherChannel et la sécurité des ports.

Résultat obtenu

À la fin du projet, l’infrastructure est fonctionnelle, organisée et sécurisée.

Les postes peuvent communiquer selon les règles définies, les VLAN sont opérationnels, le routage inter-VLAN fonctionne, les ACL filtrent le trafic, les logs sont envoyés vers Syslog et les ports des switches sont sécurisés.

Compétences mobilisées
Architecture réseau
Configuration Cisco
VLAN
VTP
Routage inter-VLAN
ACL
SSH
NTP
Syslog
EtherChannel
Port-Security
Diagnostic réseau
Cisco Packet Tracer
Conclusion

Ce projet m’a permis de comprendre comment construire une infrastructure réseau complète étape par étape.

J’ai appris à configurer les équipements Cisco, à segmenter un réseau avec des VLAN, à mettre en place du routage inter-VLAN, à filtrer le trafic avec des ACL et à sécuriser les ports des commutateurs.

Ce projet est l’un des plus complets que j’ai réalisés, car il regroupe plusieurs compétences importantes du BTS SIO option SISR.
