# Compte-rendu TP2

## Exercice 1 : Commandes de base

### mise à jour de ma distribution

```console
ubuntu@ubuntu-VirtualBox:~/script$ sudo apt full-upgrade
```

### 1. Quels sont les 5 derniers paquets installés sur votre machine ?

```console
ubuntu@ubuntu-VirtualBox:~$ cat /var/log/dpkg.log  | tail -5
2019-09-23 14:38:36 status half-configured install-info:amd64 6.5.0.dfsg.1-4build1
2019-09-23 14:38:36 status installed install-info:amd64 6.5.0.dfsg.1-4build1
2019-09-23 14:38:36 trigproc initramfs-tools:all 0.131ubuntu19.1 <aucune>
2019-09-23 14:38:36 status half-configured initramfs-tools:all 0.131ubuntu19.1
2019-09-23 14:38:57 status installed initramfs-tools:all 0.131ubuntu19.1
ubuntu@ubuntu-VirtualBox:~$
```

### 2. Utiliser dpkg et apt pour compter le nombre de paquets installés (ne pas hésiter à consulter le manuel !). Comment explique-t-on la (petite) différence de comptage ?

```console
ubuntu@ubuntu-VirtualBox:~$ dpkg -l | grep "^ii" | wc -l
1656
ubuntu@ubuntu-VirtualBox:~$
```

```console
ubuntu@ubuntu-VirtualBox:~$ apt list --installed | wc -l

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

1657
ubuntu@ubuntu-VirtualBox:~$
```

### 3. Combien de paquets sont disponibles en téléchargement ?

```console
ubuntu@ubuntu-VirtualBox:~$ apt list | wc -l

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

95321
ubuntu@ubuntu-VirtualBox:~$
```

### 4. Créer un alias “maj” qui met à jour le système

```console
ubuntu@ubuntu-VirtualBox:~$ alias maj="sudo apt full-upgrade"
ubuntu@ubuntu-VirtualBox:~$
```

### 5. A quoi sert le paquet fortunes ? Installez-le.

```console
ubuntu@ubuntu-VirtualBox:~$ apt search "fortunes"
fortunes/disco,disco,now 1:1.99.1-7build1 all
  Fichiers de données contenant des biscuits chinois
ubuntu@ubuntu-VirtualBox:~$ sudo apt install "fortunes"
.
.
ubuntu@ubuntu-VirtualBox:~$ fortune fortunes
Don't Worry, Be Happy.
    -- Meher Baba
ubuntu@ubuntu-VirtualBox:~$
```

### 6. Quels paquets proposent de jouer au sudoku ?

```console
ubuntu@ubuntu-VirtualBox:~$ apt search "sudoku"
En train de trier... Fait
Recherche en texte intégral... Fait
fltk1.1-games/disco 1.1.10-26ubuntu1 amd64
  Boîte à outils Fast Light - exemples de jeux : jeux de dames, sudoku

fltk1.3-games/disco 1.3.4-9ubuntu1 amd64
  Boîte à outils Fast Light - exemples de jeux : jeux de dames, sudoku

gnome-sudoku/disco,now 1:3.32.0-1 amd64  [installé, automatique]
  Casse-tête Sudoku pour GNOME

hitori/disco 3.31.0-1 amd64
  Jeu de puzzle logique similaire au sudoku

ksudoku/disco 4:18.12.3-0ubuntu1 amd64
  Jeu et Solveur de Sudoku

libqqwing-dev/disco 1.3.4-1.1 amd64
  outil pour générer et résoudre des casse-tête Sudoku (développement)

libqqwing2v5/disco,now 1.3.4-1.1 amd64  [installé, automatique]
  outil pour générer et résoudre des casse-tête Sudoku (bibliothèque)

nudoku/disco 1.0.0-1 amd64
  ncurses based sudoku games

qqwing/disco 1.3.4-1.1 amd64
  tool for generating and solving Sudoku puzzles (application)

sudoku/disco 1.0.5-2build3 amd64
  Sudoku en mode console

texlive-games/disco,disco 2018.20190227-1 all
  TeX Live : Composition de jeux

ubuntu@ubuntu-VirtualBox:~$ apt install sudoku
.
.
ubuntu@ubuntu-VirtualBox:~$
```

### 7. Lister les derniers paquets installés explicitement avec la commande apt install



## Exercice 2

### A partir de quel paquet est installée la commande ls ? Comment obtenir cette information en une seule commande pour n’importe quel programme (indice : la réponse est dans le poly de cours 2, dans la liste des commandes utiles) ? Utilisez la réponse pour écrire un script appelé origine-commande (sans l’extension .sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.

```console
ubuntu@ubuntu-VirtualBox:~$ which -a ls | xargs dpkg -S 2> /dev/null 
coreutils: /bin/ls
ubuntu@ubuntu-VirtualBox:~$
```

```bash
#!/bin/bash

which -a $1 | xargs dpkg -S 2> /dev/null
```

```console
ubuntu@ubuntu-VirtualBox:~$ chmod +x origine_commande 
ubuntu@ubuntu-VirtualBox:~$ cp origine_commande /usr/bin/
cp: impossible de créer le fichier standard '/usr/bin/origine_commande': Permission non accordée
ubuntu@ubuntu-VirtualBox:~$ sudo cp origine_commande /usr/bin/
ubuntu@ubuntu-VirtualBox:~$ origine_commande ls 
coreutils: /bin/ls
ubuntu@ubuntu-VirtualBox:~$
```

## Exercice 3

### Ecrire une commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package spécifié dans cette commande.

```bash
#!/bin/bash

(dpkg -l | grep "$1" ) 1>/dev/null && echo "INSTALLÉ" || echo "NON INSTALLÉ"
```

```console
ubuntu@ubuntu-VirtualBox:~$ check_installed coreutils
INSTALLÉ
ubuntu@ubuntu-VirtualBox:~$ check_installed zzuf
NON INSTALLÉ
ubuntu@ubuntu-VirtualBox:~$
```

## Exercice 4

### Lister les programmes livrés avec coreutils.

```console
ubuntu@ubuntu-VirtualBox:~$ dpkg -L coreutils
/.
/bin
/bin/cat
/bin/chgrp
/bin/chmod
/bin/chown
/bin/cp
/bin/date
/bin/dd
/bin/df
/bin/dir
/bin/echo
/bin/false
/bin/ln
/bin/ls
/bin/mkdir
/bin/mknod
.
.
ubuntu@ubuntu-VirtualBox:~$
```

### A quoi sert la commande '[' et comment afficher ce qu’elle retourne ?

/\*Je ne sais pas\*/
En se fiant au bash, '[' initie un test.
ex: if [ $i -eq $j ]

## Exercice 5: aptitude

### Installez le paquet ___emacs___ à l’aide de la version graphique d’aptitude.

1) Installer aptitude.

```console
ubuntu@ubuntu-VirtualBox:~$ sudo apt install aptitude
.
.
ubuntu@ubuntu-VirtualBox:~$
```

2) Lancer aptitude, l'interface graphique s'ouvrira.

```console
ubuntu@ubuntu-VirtualBox:~$ aptitude
```

3) Entrer dans l'onglet \<Packets non installés\>.

4) Rechercher le packet ___emas___ en tapant / , puis taper ___emacs___ dans le champs de recherche.

5) Si le packet ___emac___ n'est pas trouvé directement, appuyer sur ___n___ jusqu'à l'avoir trouver.

6) Appuyer sur ___+___ pour sélectionner les packages à installer, ici ___emacs___.

7) Appuyer deux fois sur ___g___ pour installer les packages.

## Exercice 6. Installation d’un paquet par PPA

### 1. Installer la version Oracle de Java (avec l’ajout des PPA)

```console
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-installer-local
```

### 2. Vérifiez qu’un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il ?

Dans le dossier ___/etc/apt/sources.list.d___, on rerouve le fichier ___linuxuprising-ubuntu-java-disco.list___.
Dans ce fichier, il y a la commande suivante :

```bash
#!/bin/bash

deb http://ppa.launchpad.net/linuxuprising/java/ubuntu disco main
```
