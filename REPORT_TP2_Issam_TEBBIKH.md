# Compte-rendu TP2

## Exercice 1 : Variables d'environnement

### 1. Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?

```console
ubuntu@ubuntu-VirtualBox:~$ echo $PATH
/usr/bin/ls
ubuntu@ubuntu-VirtualBox:~$ 
```

### 2. Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans votre répertoire personnel ?

```console
ubuntu@ubuntu-VirtualBox:/var/lib$ cd $HOME
ubuntu@ubuntu-VirtualBox:~$ 
```

Dans le programme ___cd___, la variable d'environnement $HOME est utilisé par défaut lorsqu'aucun argument est passé à la commande.

### 3. Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.

___LANG:___ utilisée par les différents programmes pour déterminer la langue des messages à afficher.  
___PWD:___ contient le chemin absolu vers le répertoire courant.  
___OLDPWD:___ contient le chemin absolu vers le répertoire courant précédent.  
___SHELL:___ indique l'interpréteur shell utilisé par défaut. La valeur habituelle sous linux est /bin/bash (plus rarement /bin/sh).

### 4. Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe.

```console
ubuntu@ubuntu-VirtualBox:~$ MY_VAR="hello"
ubuntu@ubuntu-VirtualBox:~$ echo $MY_VAR
hello
ubuntu@ubuntu-VirtualBox:~$ 
```

### 5. Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale.

La commande ___bash___ est un interpréteur de commande compatible sh qui exécute les commandes lues depuis l'entrée standard ou depuis un fichier.

```console
ubuntu@ubuntu-VirtualBox:~$ bash
ubuntu@ubuntu-VirtualBox:~$ echo $MY_VAR

ubuntu@ubuntu-VirtualBox:~$ 
```

La variable local ___MY_VAR___ ayant été créé dans le shell, elle n'est pas prise en charge par l'interpréteur. Donc elle n'existe pas dans le programme ___bash___.

### 6. Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.

```console
ubuntu@ubuntu-VirtualBox:~$ export MY_VAR ; printenv MY_VAR
hello
ubuntu@ubuntu-VirtualBox:~$ 
```

### 7. Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace. Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.

```console
ubuntu@ubuntu-VirtualBox:~$ export NOMS="TEBBIKH ZHOU" ; printenv NOMS
TEBBIKH ZHOU
ubuntu@ubuntu-VirtualBox:~$ 
```

### 8. Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2 sont vos deux noms) en utilisant la variable NOMS.

```console
ubuntu@ubuntu-VirtualBox:~$ echo "Bonjour à vous deux, $NOMS !"
Bonjour à vous deux, TEBBIKH ZHOU!
ubuntu@ubuntu-VirtualBox:~$ 
```

### 9. Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande unset ?

```console
ubuntu@ubuntu-VirtualBox:~$ export VIDE=""
ubuntu@ubuntu-VirtualBox:~$ env | grep "VIDE"
VIDE=
ubuntu@ubuntu-VirtualBox:~$ unset VIDE
ubuntu@ubuntu-VirtualBox:~$ env | grep "VIDE"
ubuntu@ubuntu-VirtualBox:~$ 
```

### 10. Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre dossier personnel d’après bash)

```console
ubuntu@ubuntu-VirtualBox:~$ echo '$HOME =' $HOME
$HOME = /home/ubuntu
ubuntu@ubuntu-VirtualBox:~$ 
```

## Exercice 2 : Contrôle de mot de passe

### Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur ne doit pas s’afficher.

```bash
#!/bin/bash

PASSWORD="hello"

read -p "Entrez votre mot de passe: " -s mdp

if [ $mdp = $PASSWORD ]
then
    echo -e "\nMot de passe correct"
else
    echo -e "\nMot de passe erroné"
fi
```

```console
ubuntu@ubuntu-VirtualBox:~$ bash testpwd.sh
Entrez votre mot de passe:  //mdp
Mot de passe erroné
ubuntu@ubuntu-VirtualBox:~$ bash testpwd.sh
Entrez votre mot de passe:  //hello
Mot de passe correct
ubuntu@ubuntu-VirtualBox:~$ 
```

## Exercice 3 : Expressions rationnelles

### Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre est un nombre réel :

```bash
function is_number()
{
re='^[+-]?[0-9]+([.][0-9]+)?$'
if ! [[ $1 =~ $re ]] ; then
return 1
else
return 0
fi
}
```

### Il affichera un message d’erreur dans le cas contraire.

```bash
#!/bin/bash

function is_number()
{
re='^[+-]?[0-9]+([.][0-9]+)?$'
if ! [[ $1 =~ $re ]] ; then
return 1
else
return 0
fi
}

is_number $1

if [ $? -eq 0 ]
then
        echo "$1 est un nombre entier"
else
        echo "$1 n'est pas un nombre entier"
fi
```

```console
ubuntu@ubuntu-VirtualBox:~/script$ bash is_number.sh p
p n'est pas un nombre entier
ubuntu@ubuntu-VirtualBox:~/script$ bash is_number.sh 5
5 est un nombre entier
ubuntu@ubuntu-VirtualBox:~/script$ vim is_number.sh
```

## Exercice 4 : Contrôle d’utilisateur

### Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le script est appelé sans nom d’utilisateur, il affiche le message : ”Utilisation : nom_du_script nom_utilisateur”, où nom_du_script est le nom de votre script récupéré automatiquement (si vous changez le nom de votre script, le message doit changer automatiquement)

```bash
#!/bin/bash

nb_user=$(cat /etc/passwd | wc -l)

for i in $(seq 1 $nb_user)
do
        user=$(cat /etc/passwd | cut -d : -f1 | sort | head -n $i | tail -n 1)

        if ! [ $1 ]
        then
                echo "Utilisation : $0 nom_utilisateur"
                exit
        else
                if [ "$1" = "$user" ]
                then
                        echo "L'utilisateur $user existe"
                        exit
                fi
        fi
done
echo "L'utilisateur $1 n'existe pas"
exit
```

```console
ubuntu@ubuntu-VirtualBox:~/script$ bash user.sh roo
L'utilisateur roo n'existe pas
ubuntu@ubuntu-VirtualBox:~/script$ bash user.sh root
L'utilisateur root existe
ubuntu@ubuntu-VirtualBox:~/script$
```

## Exercice 5. Factorielle

### Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre (on supposera que l’utilisateur saisit toujours un entier naturel).

```bash
#!/bin/bash

read -p  "Saisissez un nombre entier :"  ent
f=1
for i in $(seq 1 $ent)
do
        f=$(($f*$i))
done

echo " La factorielle de $ent est :"$f
```

```console
ubuntu@ubuntu-VirtualBox:~$ bash fact.sh 
Saisissez un nombre entier :5
 La factorielle de 5 est :120
ubuntu@ubuntu-VirtualBox:~$
```

## Exercice 6. Le juste prix

### Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner. Le programme écrira ”C’est plus !”, ”C’est moins !” ou ”Gagné !” selon les cas (vous utiliserez $RANDOM).

```bash
#!/bin/bash

echelle=1000
nombre=$RANDOM

let "nombre %= $echelle"

echo "Saisir un nombre (entre 1 et 1000): "
read nb

while [ $nb -ne $nombre ]
do

        if [ $nb -lt $nombre ] ; then
                echo "c'est plus + !"
        elif [ $nb -gt $nombre ] ; then
                echo "c'est moins - !"

        fi

        echo -e "\nSaisir un nombre :"
        read new
        nb=$new

done

if [ $nb -eq $nombre ] ; then
        echo "BRAVO vous avez trouvé le juste prix ! --> $nombre <--"
fi
```

## Exercice 7 : Statistiques

### 1. Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et affiche le min, le max et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres sont bien des entiers. Puis Généralisez le programme à un nombre quelconque de paramètres.

```bash
function is_number()
{
        re='^[+-]?[0-9]+([.][0-9]+)?$'
        if ! [[ $1 =~ $re ]] ; then
                return 1
        else
                return 0
        fi
}

min=$1
max=$1
moy=$1

somme=0

while (("$#"));
do
        is_number $1
        if [ $? = 0 ]; then
                if [ $1 -lt -100 ] || [ $1 -gt 100 ]; then
                        echo "$1 est un nombre mais est inférieur à -100 ou supérieur à 100"
                else

                        if [ $1 -gt $max ]; then
                                max=$1
                        fi

                        if [ $1 -lt $min ]; then
                                min=$1
                        fi

                        somme=$(($somme+$1))
                        ((i++))


                fi

        else
                echo "$1 n'est pas un nombre"

        fi

        shift
done

moy=$(($somme/$i))

echo "Le chiffre minimum est $min"
echo "Le chiffre moyen est $moy"
echo "Le chiffre maximun est $max"
```

```console
ubuntu@ubuntu-VirtualBox:~/script$ bash stats.sh 3 5 9 12
Le chiffre minimum est 3
Le chiffre moyen est 7
Le chiffre maximun est 12
ubuntu@ubuntu-VirtualBox:~/script$
```

### 3. Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et stockées au fur et à mesure dans un tableau.

