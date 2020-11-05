== Installation ==
Définir adresse ip static
 sudo vi /etc/network/interfaces
 sudo service networking restart

Installer fail2ban au cas où...
 sudo apt install fail2ban

Lancer la commande pour commencer l'installation de pivpn:
 curl -L https://install.pivpn.io | bash
