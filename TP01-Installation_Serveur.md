# Compte rendu TP 01 - Installation Serveur

## Post Installation
### Nombre de paquets

Suite à la commande `dpkg -l | wc -l`
J'obtiens le résultat suivant
[!Texte alternatif](/img/result_dpkg.png "Résultat paquets")
Donc 335paquets.

### Configuration SSH

Pour installer le protocole SSH, il faut d'abord se connecter en root avec la commande `su root`
Puis installer le protocole SSH pour le serveur `apt install openssh-server`
Avec la commande `systemctl status ssh` il est possible de consuter le statut du protocole SSH.
Si le statut est __inactive(dead)__, alors il faut executer la `systemctl start ssh` pour lancer le protocole.

### Connexion
Pour se connecter en SSH en local depuis son terminal il faut autoriser le compte root pour cela il faut modifier à la main le fichier __sshd_config__.
Pour y accèder `nano /etc/ssh/sshd_config` puis on complète la ligne `PermitRootLogin` par `PermitRootLogin yes`

Maintenant depuis son terminal on lance `ssh nom_d'utilisateur@ip`.

Dans mon cas ici `ssh pierre@127.0.0.1`, puis on entre son mot de passe.

### Space Usage
La commande `df -h` me donne le résutat suivant :
[!Texte alternatif](/img/result_stockage.png "Résultat stockage")
J'ai bien moins de 1GB utilisé par mon serveur.

### Résultats des commandes

`echo $LANG` me renvoi la variable locale de la langue paramétré sur le serveur.
Dans mon cas : 
```
pierre@servlocal:~$ echo $LANG
fr_FR.UTF-8
```
`echo $HOSTNAME` me renvoi le nom de la machine.
```
pierre@servlocal:~$ echo $HOSTNAME
servlocal
```
D'après le manuel `man hostname` les commandes `domainname` et `dnsdomainname` nous donne le nom de domaine du système.
En revanche `domainname` me renvoi rien
```
pierre@servlocal:~$ domainname
(none)
```
Alors que `dnsdomainname` me renvoi
```
pierre@servlocal:~$ dnsdomainname
ufr-info-p6.jussieu.fr
```
La vérification de l'emplacement depots `cat /etc/apt/sources.list | grep -v -E ’^#|^$’` me renvoi
```
pierre@servlocal:~$ cat /etc/apt/sources.list | grep -v -E '^#|^$'
deb http://ftp.fr.debian.org/debian/ trixie main non-free-firmware
deb-src http://ftp.fr.debian.org/debian/ trixie main non-free-firmware
deb http://security.debian.org/debian-security trixie-security main non-free-firmware
deb-src http://security.debian.org/debian-security trixie-security main non-free-firmware
deb http://ftp.fr.debian.org/debian/ trixie-updates main non-free-firmware
deb-src http://ftp.fr.debian.org/debian/ trixie-updates main non-free-firmware
```
La commande `cat /etc/shadow | grep -vE ':*:|:!*:'` me renvoi
```
pierre@servlocal:~$ cat /etc/shadow | grep -vE ':*:|:!*:'
cat: /etc/shadow: Permission non accordée
```
En revanche si je passe en tant que __root__, j'obtiens
```
root@servlocal:/# cat /etc/shadow | grep -vE ':\*:|:!\*:'
root:$y$j9T$ycx/PSqNZyjejEzocgxy./$MxCYRl3aLOcqLsYhUEmHU4ZtQTJIKPv6YL4FK2fjknC:20481:0:99999:7:::
dhcpcd:!:20465::::::
pierre:$y$j9T$VsI.a9Odujr2.R.OTJXe30$yZjgFIjzGUg/YDzltL2jiV.oTWc/dySPXGexZHx1Q1C:20481:0:99999:7:::
```
Ce sont les mot de passe respectifs de chaque compte présent sur la machine, afficher de manière hasher afin de ne pas afficher en clair le mot de passe.

La commande `cat /etc/passwd | grep -vE 'nologin|sync'`,  me donne comme résultat
```
root@servlocal:/# cat /etc/passwd | grep -vE 'nologin|sync'
root:x:0:0:root:/root:/bin/bash
dhcpcd:x:100:65534:DHCP Client Daemon:/usr/lib/dhcpcd:/bin/false
pierre:x:1000:1000:pierre,,,:/home/pierre:/bin/bash
```
Cette commande m'affiche le contenu du fichier `/etc/passwd` qui contient la liste des utilisateurs, leurs id, et leurs répertoires personnels.

`fdisk -l`, dans un premier temps, la commande me renvoyer `commande introuvable`, afin de résoudre ce problème, j'ai effectué les commandes suivantes :
```
PATH="/sbin:$PATH"
command -v fdisk
```
Maintenant `fdisk -l` me renvoi la liste des tables de partitions présentes sur le serveur
```
root@servlocal:/home/pierre# fdisk -l
Disque /dev/sda : 20 GiB, 21474836480 octets, 41943040 secteurs
Modèle de disque : VBOX HARDDISK
Unités : secteur de 1 × 512 = 512 octets
Taille de secteur (logique / physique) : 512 octets / 512 octets
taille d'E/S (minimale / optimale) : 512 octets / 512 octets
Type d'étiquette de disque : dos
Identifiant de disque : 0x853108bc

Périphérique Amorçage    Début      Fin Secteurs Taille Id Type
/dev/sda1    *            2048 19531775 19529728   9,3G 83 Linux
/dev/sda2             19533822 41940991 22407170  10,7G  f Étendue W95 (LBA)
/dev/sda5             19533824 27344895  7811072   3,7G 83 Linux
/dev/sda6             27346944 29298687  1951744   953M 83 Linux
/dev/sda7             29300736 41940991 12640256     6G 82 partition d'échange Linux / S
```
`fdisk -x` me renvoi sensiblement la même chose, mais après une recherche dans le man, `fdisk -x` renvoi une liste identique à `fdisk -l` avec plus de détails.

## Aller plus loin
