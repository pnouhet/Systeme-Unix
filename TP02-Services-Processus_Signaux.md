# Secure Shell SSH

## 1.1 Exercice : Connection ssh root

L'élément de configuration que j'ai dû changer afin de permettre une connexion distante au serveur depuis le terminal local de ma machine est `#PermitRootLogin`
en lui passant comme argument `yes` au lieu de `prohibit-password`, cela permet à l'utilisateur Root de se connecter en utilisant SSH.

Avantages et incovénients : https://www.malekal.com/securiser-serveur-ssh/ ### TODO à détaillé

## 1.2 Exercice : Authentification par clef / G´en´eration de clefs

### TODO
J'ai utilisé la commande `ssh-keygen` afin de générer une clef d'authentification.
(mettre capture d'écran du resultat)
https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server#faqs

Puis détaillé la suite : Copying Your Public Key Manually(url du dessus)

