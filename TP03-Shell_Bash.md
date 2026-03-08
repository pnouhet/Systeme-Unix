# TP 03 : Shell bash

## Exercice : paramètres
`nano analyse.sh` création du script

```
#!/bin/bash

# $# -> variable contenant le nombre de paramètres passés au script
# $i -> entier entre 1 et 9, la variable i-ème
# $@ -> contient la liste de tous les paramètres séparés par espaces
# $0 -> contient le nom du script en cours

echo "Bonjour, vous avez rentré" $# "nombre de paramètres."
echo "le nom du script est :" $0
echo "Le 3ème paramètre est :" $3
echo "Voici la liste des paramètres :" $@
```
`chmod +x analyse.sh` pour ajouter les permissions d'executions du script
`./analyse.sh` pour éxécuter le script, avec `./` pour faire appel au chemin relatif du fichier.

#### OUTPUT
```
root@servlocal:~# ./analyse.sh test de script
Bonjour, vous avez rentré 3 nombre de paramètres.
le nom du script est : ./analyse.sh
Le 3ème paramètre est : script
Voici la liste des paramètres : test de script
```

## Exercice : vérification du nombre de paramètres
`nano concat.sh` création du script

```
#!/bin/bash
# script pour concaténer 2 paramètres entre eux et renvoyant une erreur si plus de 2 params
param1=$1 # assigne le parametre 1 dans la variable $param1
param2=$2 # assigne le parametre 2 dans la variable $param2

# boucle if de si le nombre de parametres est équivalent à 2 alors on affiche param1 et param2
if [ $# -eq 2 ]; then
        echo "${param1} ${param2}"
else # Sinon on renvoi une erreur
        set -e # ferme le script
        echo "Erreur : Veuillez entrer 2 paramètres"
fi
```

`chmod +x concat.sh` pour ajouter les permissions d'executions du script
`./concat.sh` pour éxécuter le script, avec `./` pour faire appel au chemin relatif du fichier.

#### OUTPUT
```
root@servlocal:~# ./concat.sh pierre nouhet
pierre nouhet
```

## Exercice : argument type et droits
`nano test-fichier.sh` création du script

```
#!/bin/bash
# Script permettant de connaître les permissions en lecture, en écriture ou executable d'un fichier ou d'un répertoire

# -f > fichier ordinaire
# -d > répertoire
# -r > est lisible
# -w > écriture possible
# -x > executable

echo "Veuillez entrer le nom du fichier : "
read file

if [ -d $file ]; then #si file est un répertoire
        echo "Le fichier" ${file} "est un répertoire"
        if [ -r $file ]; then
                echo $file "est accessible en lecture"
        fi
        if [ -w $file ]; then
                echo $file "est accessible en écriture"
        fi
        if [ -x $file ]; then
                echo $file "est exécutable"
        fi
fi
if [ -f $file ]; then #si file est un fichier ordinaire
        echo "Le fichier" ${file} "est un fichier ordinaire"
        if [ -r $file ]; then
                echo $file "est accessible en lecture"
        fi
        if [ -w $file ]; then
                echo $file "est accessible en écriture"
        fi
        if [ -x $file ]; then
                echo $file "est exécutable"
        fi
fi
```

#### OUTPUT
```
root@servlocal:~# ./test-fichier.sh
Veuillez entrer le nom du fichier :
/etc
Le fichier /etc est un répertoire
/etc est accessible en lecture
/etc est accessible en écriture
/etc est exécutable
```
```
root@servlocal:~# ./test-fichier.sh
Veuillez entrer le nom du fichier :
test-fichier.sh
Le fichier test-fichier.sh est un fichier ordinaire
test-fichier.sh est accessible en lecture
test-fichier.sh est accessible en écriture
test-fichier.sh est exécutable
```

## Exercice : Afficher le contenu d’un répertoire

`nano listedir.sh` création du script
```
#!/bin/bash
# Script affichant les fichiers et dossiers d'un répertoire

repertoire=$1 # récupération de l'argument

# filtre sur les fichiers
if [ -f $repertoire ]; then
        echo "###### fichiers dans" $repertoire
        find $repertoire -maxdepth 1 -f
fi
# filtre sur les dossiers
if [ -d $repertoire ]; then
        echo "###### repertoires dans" $repertoire
        find $repertoire -maxdepth 1 -d
else
# message en cas d'erreur
        echo "Erreur" $repertoire "n'est pas un dossier valide"
fi
```

#### OUTPUT
```
root@servlocal:~# ./listedir.sh /var
###### repertoires dans /var
find: attention : l'option -d est obsolète ; veuillez utiliser -depth à la place, parce que celle-ci est est une option conforme à POSIX.
/var/run
/var/lib
/var/lock
/var/.updated
/var/tmp
/var/mail
/var/spool
/var/cache
/var/lost+found
/var/log
/var/backups
/var/local
/var/opt
/var
```

## Exercice : Lister les utilisateurs
`nano listusers.sh` création du script  
```
#!/bin/bash
# Script pour afficher la liste des users sur le systeme et leur uid correspondant

# pour chaque ligne du fichier passwd on extrait la colonne 1 pour le login et la 3 pour le uid
for ligne in $(cat /etc/passwd | cut -d: -f1,3); do
        login=$(echo $ligne | cut -d: -f1) # coupe la variable ligne pour afficher le login
        uid=$(echo $ligne | cut -d: -f2) # colonne la variable ligne pour afficher le uid
if [ $uid -gt 100 ]; then # vérifier que la valeur uid est greater than (plus grande que) 100
        echo "User" $login "a pour uid" $uid
fi
done
```

#### OUTPUT  
```
root@servlocal:~# ./listusers.sh
User nobody a pour uid 65534
User systemd-network a pour uid 998
User systemd-timesync a pour uid 991
User messagebus a pour uid 990
User pierre a pour uid 1000
User sshd a pour uid 989
```

## Exercice : Mon utilisateur existe-t-il
`nano userexist.sh`  
```
#!/bin/bash
# Script pour afficher si un utilisateur existe

#Récupération du paramètre
param=$1

# Vérifier si le parametre est un login
searchLogin=$(grep ^$param /etc/passwd)

if [ ! -z $searchLogin ]; then #si la chaine n'est pas vide alors l'utilisateur existe
        echo $searchLogin | cut -d: -f3 # on affiche son uid
else
        searchUID=$(cat /etc/passwd | cut -d: -f3 | grep ^$param) #sinon verifier si cest un uid en e>
        if [ ! -z $searchUID ]; then # si on trouve l'uid alors on l'affiche
                echo $searchUID # affiche l'uid
        fi
fi
```

#### OUTPUT  
```
root@servlocal:~# ./userexist.sh pierre
1000
```
