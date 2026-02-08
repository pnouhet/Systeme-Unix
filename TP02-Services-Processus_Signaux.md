# Secure Shell SSH

## 1.1 Exercice : Connection ssh root

L'élément de configuration que j'ai dû changer afin de permettre une connexion distante au serveur depuis le terminal local de ma machine est `#PermitRootLogin`
en lui passant comme argument `yes` au lieu de `prohibit-password`, cela permet à l'utilisateur Root de se connecter en utilisant SSH.

Avantages et incovénients : https://www.malekal.com/securiser-serveur-ssh/ ### TODO à détaillé

## 1.2 Exercice : Authentification par clef / Génération de clefs

### TODO
J'ai utilisé la commande `ssh-keygen` afin de générer une clef d'authentification.
![resultats ssh-keygen](./img/ssh-keygen.jpg)
Ici dans mon cas j'ai déjà effectué la génération de clef d'authentification, je met donc `n` pour ne pas overwrite ma clef déjà existante.
La suite des étapes lorsque l'on met `y` propose d'enter un mot de passe pour la clef, dans mon cas je n'en ai pas besoin, donc je ne met rien.

```
Created directory '/home/username/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```
En revanche dans un cas réel c'est une mauvaise idée de ne pas mettre de `passphrare` car cela signifie que votre clé privée est stockée directement sur le disque. Si quelqu'un de mal intentionné parvient à voler ce fichier il accèdera instantanément au serveur. La `passphrase`ajoute une couche de sécurité critique en chiffrant la clef localement ce qui garantit que même en cas de vol de fichier l'accès reste protégé par la `passphrase`.

## 1.3 Exercice : Authentification par clef / Connection serveur

Pour nous connecter sur notre serveur à l'aide de notre clef publique il faut d'abord la déposer sur le serveur.
Pour cela on va effectuer la commande `ssh-copy-id username@remote_host` si cela ne fonctionne pas, ce qui est mon cas alors il est possible d'effectuer une copie manuelle.
Voici la procédure à suivre :

Tout d'abord on execute la commande `cat ~/.ssh/id_ed25519.pub` ce qui nous affiche notre clef publique.
![resultats cat ssh-key](./img/catsshkey.jpg)

Ensuite on accède au serveur, puis on créer le dossier .ssh si celui-ci n'existe pas `mkdir -p ~/.ssh`
Puis il suffit d'écrire la commande `echo public_key_string >> ~/.ssh/authorized_keys` en remplaçant `public_key_string` par le résultat du cat.
Cela va copier notre clef dans le fichier `authorized_keys`.

Il faut maintenant désactiver l'authentification au serveur par mot de passe.
Pour cela on va dans le fichier `sshd_config` puis on décommente la ligne `PubkeyAuthentication` et on passe la ligne `PasswordAuthentication yes` à `no` afin de désactiver la connexion par mot de passe textuel.

![resultat pubkey](./img/pubkeyauthyes.jpg)
![resultat pubkey](./img/passwordauthno.jpg)

N'oublions pas de relancer le service SSH avec la commande `systemctl restart ssh`

## 1.4 Exercice : Authentification par clef : depuis la machine hote

Il est possible pour nous maintenant de se connecter depuis notre clef, en executant la commande suivante :
`ssh -i id_ed25519.pub root@192.168.56.101`
Si une passphrase avait été paramétré on aurait eu à entrer la passphrase afin de se connecter, si ce n'est pas le cas, on est alors directement connecté au serveur.

![resultat connexion sshkey](./img/connexionsshkey.jpg)

## 1.5 Exercice : S´ecurisez

Pour protéger notre serveur contre les attaques brute force ssh, consistant à tester massivement des combinaisons d'identifiants et de mots de passe il est recommandé de désactiver la connexion par mot de passe de note serveur.
Pour ce faire il suffit d'aller dans notre fichier `/etc/ssh/sshd_config` et de commenter la ligne `PasswordAuthentication no` afin de désactiver l'authentification par mot de passe.
On peut également restreindre l'accès à l'utilisateur root pour plus de sécurité, on interdit la connexion directe en tant que super-utilisateur. Pour ce faire il suffit toujours dans le fichier `sshd_config` de décommenter et changer la ligne `PermitRootLogin yes` à `PermitRootLogin no`.

# 2.Processus
## 2.1 Exercice : Etude des processus UNIX


