install i2p on a debian live dvd r like tails ...





Yes, Debian Live + i2pd can run from a DVD-R, but there are some limitations.
How it works
You can create:
DVD-R → Debian Live → i2pd → I2P network
The computer boots from the DVD, runs Debian in RAM, and i2pd runs during that session.
The good points
✅ No internal disk needed
✅ No persistence (everything disappears after reboot)
✅ Can run offline/air-gapped except the network connection you allow
✅ More flexible than Tails because you can install normal Debian packages
The limitations
❌ A normal Debian Live DVD is read-only
You cannot permanently install i2pd on the DVD.
Any package installation disappears after shutdown.



Option 1 
— Install i2pd every boot

Boot Debian Live, then:

- sudo apt update
 - sudo apt install i2pd
 - sudo systemctl start i2pd
 -
 -It works, but:
 -slower
 -you repeat it every session
 -requires internet access to download packages






DEBIAN + i2PD - CONFIGURATION SECURISEE

Objectif:
Configurer un système Debian avec i2pd de manière plus sécurisée en réduisant les services inutiles, en activant un pare-feu, en renforçant le système et en limitant l'exposition du routeur I2P.

==================================================

1. INSTALLATION DE DEBIAN
   ==================================================

Pendant l'installation:

- Utiliser une installation minimale de Debian.
- Activer le chiffrement complet du disque avec LUKS si un disque est utilisé.
- Ne pas installer les services inutiles:
  - serveur SSH si non nécessaire
  - Bluetooth
  - impression
  - logiciels inutiles

Créer un utilisateur normal et éviter d'utiliser root pour les tâches quotidiennes.

==================================================
2. METTRE DEBIAN A JOUR

Après l'installation:

sudo apt update

sudo apt full-upgrade

Installer des outils de sécurité:

sudo apt install unattended-upgrades apt-listchanges lynis fail2ban ufw apparmor

Activer les mises à jour de sécurité automatiques:

sudo dpkg-reconfigure unattended-upgrades

==================================================
3. CONFIGURATION DU PARE-FEU UFW

Installer UFW:

sudo apt install ufw

Bloquer par défaut:

sudo ufw default deny incoming

sudo ufw default deny outgoing

Autoriser les connexions nécessaires:

DNS:

sudo ufw allow out 53

Mises à jour:

sudo ufw allow out 80

sudo ufw allow out 443

I2P:

sudo ufw allow out 4444

sudo ufw allow out 4447

Activer:

sudo ufw enable

Vérifier:

sudo ufw status

==================================================
4. RENFORCEMENT DU KERNEL LINUX

Modifier:

sudo nano /etc/sysctl.conf

Ajouter:

kernel.kptr_restrict=2

kernel.dmesg_restrict=1

kernel.randomize_va_space=2

kernel.sysrq=0

net.ipv4.conf.all.accept_redirects=0

net.ipv4.conf.default.accept_redirects=0

net.ipv4.conf.all.accept_source_route=0

net.ipv4.icmp_echo_ignore_broadcasts=1

fs.suid_dumpable=0

Appliquer:

sudo sysctl -p

==================================================
5. ACTIVER APPARMOR

Vérifier:

sudo aa-status

Activer:

sudo systemctl enable apparmor

sudo systemctl start apparmor

==================================================
6. INSTALLER i2pd

Installer:

sudo apt install i2pd

Les fichiers de configuration sont dans:

/etc/i2pd/

Fichiers importants:

i2pd.conf

tunnels.conf

==================================================
7. CONFIGURATION SECURISEE DE i2pd

Modifier:

sudo nano /etc/i2pd/i2pd.conf

Configuration recommandée:

ipv6=false

bandwidth=B

notransit=true

floodfill=false

Activer uniquement le proxy HTTP local:

httpproxy.enabled=true

httpproxy.address=127.0.0.1

httpproxy.port=4444

Désactiver SOCKS si inutile:

socksproxy.enabled=false

Explication:

ipv6=false

- réduit les risques liés aux fuites IPv6.

notransit=true

- empêche votre machine de servir de relais pour d'autres utilisateurs I2P.

floodfill=false

- empêche votre nœud de devenir un serveur d'annuaire I2P.

==================================================
8. ACTIVER LE SERVICE i2pd

Démarrer automatiquement:

sudo systemctl enable i2pd

Redémarrer:

sudo systemctl restart i2pd

Vérifier:

systemctl status i2pd

==================================================
9. CONFIGURATION DU NAVIGATEUR

Utiliser un profil Firefox séparé pour I2P.

Configurer le proxy:

HTTP Proxy:

127.0.0.1

Port:

4444

Éviter:

- comptes personnels
- Google
- Facebook
- email personnel
- téléchargement de fichiers inconnus

Ne pas mélanger navigation normale et navigation I2P.

==================================================
10. ISOLATION SUPPLEMENTAIRE

Installer Firejail:

sudo apt install firejail

Possibilités:

- lancer le navigateur dans Firejail
- isoler i2pd
- utiliser un conteneur Podman

==================================================
11. AUDIT DE SECURITE

Lancer:

sudo lynis audit system

Vérifier:

- ports ouverts
- permissions
- services actifs
- mises à jour manquantes

==================================================
12. ARCHITECTURE RECOMMANDEE

Internet

|

Routeur pare-feu

|

Debian minimal sécurisé

|

i2pd

|

Firefox avec profil I2P

Version plus forte:

Debian Live

+ 

i2pd

+ 

pas de persistence

+ 

DVD-R ou média en lecture seule

+ 

pas de WiFi

+ 

pas de Bluetooth

+ 

pare-feu externe

Cette configuration réduit la surface d'attaque et limite les risques, mais aucun système connecté à Internet ne peut garantir une sécurité parfaite.
