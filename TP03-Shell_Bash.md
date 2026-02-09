# TP 03 : Shell bash

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

OUTPUT:
```
root@servlocal:~# ./analyse.sh test de script
Bonjour, vous avez rentré 3 nombre de paramètres.
le nom du script est : ./analyse.sh
Le 3ème paramètre est : script
Voici la liste des paramètres : test de script
```

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

OUTPUT:
```
root@servlocal:~# ./concat.sh pierre nouhet
pierre nouhet
```

