# Installation
## Définir adresse ip static
    sudo vi /etc/network/interfaces
    sudo service networking restart

## Installer fail2ban au cas où...
    sudo apt install fail2ban

## Lancer la commande pour commencer l'installation de pivpn:
    curl -L https://install.pivpn.io | bash

# Configuration
Créer le dossier où nous allons placer la configuration de chaque clients

    sudo mkdir -p /etc/openvpn/ccd

Editer le fichier server.conf
 
    sudo vi /etc/openvpn/server.conf

Ajouter:
   
    client-config-dir /etc/openvpn/ccd
    
Si on veut empêcher les clients n'ayant pas de fichier de config de se connecter, il faut ajouter:

     ccd-exclusive

Le fichier devrait ressembler à ceci:

    dev tun0
    proto udp
    port 1194
    ca /etc/openvpn/easy-rsa/pki/ca.crt
    cert /etc/openvpn/easy-rsa/pki/issued/raspberrypi_XXX.crt
    key /etc/openvpn/easy-rsa/pki/private/raspberrypi_XXX.key
    dh none
    topology subnet
    server 10.8.0.0 255.255.255.0
    route 192.168.10.0 255.255.255.0
    # Set your primary domain name server address for clients
    push "dhcp-option DNS X.X.X.X"
    push "dhcp-option DNS X.X.X.X"
    push "route 192.168.X.X 255.255.255.0"
    # Prevent DNS leaks on Windows
    push "block-outside-dns"
    # Override the Client default gateway by using 0.0.0.0/1 and
    # 128.0.0.0/1 rather than 0.0.0.0/0. This has the benefit of
    # overriding but not wiping out the original default gateway.
    push "redirect-gateway def1"
    client-to-client
    keepalive 1800 3600
    remote-cert-tls client
    tls-version-min 1.2
    tls-crypt /etc/openvpn/easy-rsa/pki/ta.key
    cipher AES-256-CBC
    auth SHA256
    user nobody
    group nogroup
    persist-key
    persist-tun
    crl-verify /etc/openvpn/crl.pem
    status /var/log/openvpn-status.log 20
    status-version 3
    syslog
    verb 3
    client-config-dir /etc/openvpn/ccd
    ccd-exclusive
    #DuplicateCNs allow access control on a less-granular, per user basis.
    #Remove # if you will manage access by user instead of device.
    #Duplicate-cn
    #Generated for use by PiVPN.io

Aller dans le fichier /etc/openvpn/ccd et créer un fichier avec le nom de l'utilisateur
    
    vi monclient

Ajouter dans le fichier

    ifconfig-push 10.8.0.42 255.255.255.0

Modifier les droits: Accordez les permissions aux dossier et fichiers précédemment créés afin que l'utilisateur faisant tourner openvpn (nobody) puisse y accéder
    
    sudo chown nobody:nogroup -R /etc/openvpn/ccd
    sudo chmod 770 -R /etc/openvpn/ccd

Redémarrez OpenVPN ou la machine au choix
    
    sudo systemctl restart openvpn.service

# Gestion
Ajouter un utilisateur: 

    pivpn -a 

Retenir le nom d'utilisateur et ajouter un fichier de config dans
    
    cd /etc/openvpn/ccd

    :::  -a, add [nopass]     Create a client ovpn profile, optional nopass
    :::  -c, clients          List any connected clients to the server
    :::  -d, debug            Start a debugging session if having trouble
    :::  -l, list             List all valid and revoked certificates
    :::  -r, revoke           Revoke a client ovpn profile
    :::  -h, help             Show this help dialog
    :::  -u, uninstall        Uninstall PiVPN from your system!
    :::  -up, update          Updates PiVPN Scripts
    :::  -bk, backup          Backup Openvpn and ovpns dir
