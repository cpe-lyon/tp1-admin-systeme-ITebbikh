# Compte-rendu TP1

## Exercice 2 : Prise en main de l’interpréteur de commandes

### Manuel

#### 1. A l’aide du manuel, identifiez le rôle de la commande which

```console
serveur@serveur:~$ man which
```
Cette commande permet d'afficher des informations sur la commande ___which___ dont un synopsis, une description et ses options.  

#### 2. Quand on consulte cette page, comment peut-on rechercher, par exemple, le mot option ?

Dans le manuel, on tape la commande suivante :

```console
/option
```

#### 3. Comment quitte-t-on le manuel ?

il faut taper ___q___.  

#### 4. Chaque section du manuel a une première page, qui présente le contenu de la section. Afficher la première page de la section 6 ; de quoi parle cette section ?


```console
serveur@serveur:~$ man 6 intro
```
La section 6 du manuel décrit les jeux et les petits programmes amusants disponibles sur le système.

### Navigation dans l’arborescence des fichiers

#### 1. allez dans le dossier /var/log

```console
serveur@serveur:~$ cd /var/log
```

#### 2. remontez dans le dossier parent (/var) en utilisant un chemin relatif

```console
serveur@serveur:~$ cd ..
```

#### 3. retournez dans le dossier personnel

```console
serveur@serveur:~$ cd ~
```

#### 4. revenez au dossier précédent (/var)

```console
serveur@serveur:~$ cd -
```

#### 5. essayez d’accéder au dossier /root ; que se passe-t-il ?

Nous ne pouvons pas accéder à ce dossier car nous n'avons pas les permissions requises.

#### 6. essayez la commande sudo cd /root ; que se passe-t-il ? Expliquez

Lorsque nous exécutons cette commande, le mot de passe de l'utilisateur administrateur est demandé pour pouvoir passer en mode ___Super Utilisateur___.  
Une fois le mot de passe entré, une erreur s'affiche indiquant que  la commande ___cd___ n'existe pas.
En effet, ___sudo___ et ___cd___ sont deux programmes appartenant au shell, ainsi le programme ___sudo___ ne peut pas utiliser le programme ___cd___.
Il faut utiliser ___sudo -s___ pour pouvoir ensuite utiliser la commande ___cd___ en tant que Super Utilisateur.

#### 7. à partir de votre dossier personnel, créez l’arborescence suivante :


```console
serveur@serveur:~$ cd - mkdir Dossier1 Dossier2
serveur@serveur:~$ cd - mkdir Dossier2/Dossier2.1 Dossier2/Dossier2.2
serveur@serveur:~$ cd - touch Dossier1/Fichier1 Dossier2/Dossier2.2/Fichier2 Dossier2/Dossier2.2/Fichier3
```

#### 8. revenez dans votre dossier personnel ; à l’aide de la commande rm, essayez de supprimer Fichier1, puis Dossier1 ; que se passe-t-il ?

```console
serveur@serveur:~$ rm Dossier1/Fichier1
```
A cet instant, ___Fichier1___ est supprimé.

```console
serveur@serveur:~$ rm Dossier1
```
Cette commande ne fonctionne pas car Dossier1 est un dossier et la commande ___rm___ permet seulement de supprimer des fichiers.

#### 9. quelle commande permet de supprimer un dossier ?

```console
serveur@serveur:~$ rmdir Dossier1
```

#### 10. que se passe-t-il quand on applique cette commande à Dossier2 ?

Cette commande échoue car Dossier2 n'est pas vide.

#### 11. comment supprimer en une seule commande Dossier2 et son contenu ?

```console
serveur@serveur:~$ rm -r Dossier1
```
l'option ___-r___ permet de supprimer le dossier et son contenu de manière récursive.

### Commandes importantes

#### 1. Quelle commande permet d’afficher l’heure ? A quoi sert la commande ___time___ ?

```console
serveur@serveur:~$ date +%H:%M:%S
```
La commande ___time___ permet de déterminer le temps d'exécution d'une commande passée en paramètre.

#### 2. Dans votre dossier personnel, tapez successivement les commandes ___ls___ puis ___la___ ; que peut-on en déduire sur les fichiers commençant par un point ?

les fichiers commençant par un point sont des fichiers cachés.

#### 3. Où se situe le programme ___ls___ ?

```console
serveur@serveur:~$ which ls
/usr/bin/ls
serveur@serveur:~$ 
```
Donc, le programme ls se trouve dans le dossier ___/usr/bin/___.

#### 4. Que fait la commande ___ll___ ? (indice : la commande alias peut vous aider)

ll reviens à taper la commande suivant :

```console
serveur@serveur:~$ ls -alF
```
L'option -a permet d'afficher tous les fichiers, même les fichiers commançant par un point.
L'option -l permet d'afficher une longue liste des fichiers avec des informations détaillées sur chaque fichier.
L'option -F permet d'ajouter des indicateurs aux entrées.

#### 5. Quelle commande permet d’afficher les fichiers contenus dans le dossier /bin ?

```console
serveur@serveur:~$ which ls -a /bin
```

#### 6. Que fait la commande ___ls___ .. ?

Cette commande permet d'afficher les dossiers et les fichiers du dossier parent.

#### 7. Quelle commande donne le chemin complet du dossier courant ?

Je n'ai pas compris la question.

#### 8. Que fait la commande ___echo 'yo' > plop___ exécutée 2 fois ?

```console
serveur@serveur:~$ echo 'yo' > plop
```
A cet instant, le fichier ___plop___ est créé avec comme contenu ___yo___.

```console
serveur@serveur:~$ echo 'yo' > plop
```
A cet instant, le fichier ___plop___ a pour contenu ___yo___. Le contenu précédent est écrasé.

#### 9. Que fait la commande ___echo 'yo' >> plop___ exécutée 2 fois ?

```console
serveur@serveur:~$ rm plop
serveur@serveur:~$ echo 'yo' >> plop
```
A cet instant, le fichier ___plop___ est créé avec comme contenu ___yo___.

```console
serveur@serveur:~$ echo 'yo' >> plop
```
A cet instant, le fichier ___plop___ a pour contenu :  
yo  
yo  

Ajouter ___>>___ permet donc de concater le contenant d'un fichier avec le nouveau contenu. Dans ce cas, c'est la sortie de la comande ___echo yo___.

#### 10. A quoi sert la commande ___file___ ? Essayez la sur des fichiers de types différents.

La commande ___file___ détermine le type d'un ou plusieurs fichiers selon leur contenu.

#### 11. Créez un fichier ___toto___ qui contient la chaîne ___Hello Toto !___ ; créer ensuite un lien ___titi___ vers ce fichier avec la commande ___ln toto titi___. Modifiez à présent le contenu de toto et affichez le contenu de ___titi___ : qu’observe-t-on ? Supprimez le fichier ___toto___ ; quelle conséquence cela a-t-il sur ___titi___?

Nous retrouvons les changements effectués dans ___toto___ lorsque nous affichons le contenu de ___titi___.
Une fois le fichier ___toto___ supprimé, ___titi___ conserve le même contenu.

#### 12. Créez à présent un lien symbolique ___tutu___ sur ___titi___ avec la commande ___ln -s titi tutu___. Modifiez le contenu de ___titi___ ; quelle conséquence pour ___tutu___ ? Et inversement ? Supprimez le fichier ___titi___ ; quelle conséquence cela a-t-il sur ___tutu___ ?

Lorsque nous modifions le contenu contenu de ___titi___, le contenu de ___tutu___ change également. De même inversement.
Une fois le fichier ___titi___ supprimé, il n'est plus possible d'afficher le contenu de tutu. Le message ___cat: tutu: No such file or directory___ apparait mais nous retrouvons encore ___tutu___.

#### 13. Affichez à l’écran le fichier ___/var/log/syslog___. Quels raccourcis clavier permettent d’interrompre et reprendre le défilement à l’écran ?

___CTRL + S:___ interrompt le défilement d’un résultat trop verbeux
___CTRL + Q:___ reprend le défilement

#### 14. Affichez les 5 premières lignes du fichier ___/var/log/syslog___, puis les 15 dernières, puis seulement les lignes 10 à 20.

* 5 premières lignes :
```console
serveur@serveur:~$ head -5 /var/log/syslog
```
* 15 dernières lignes:
```console
serveur@serveur:~$ tail -15 /var/log/syslog
```
* 10 à 20 lignes:
```console
serveur@serveur:~$ head -20 /var/log/syslog | tail -n +10
```
#### 15. Que fait la commande ___dmesg | less___ ?

Au lieu d'écrire un long message en entier, cette commande permet de réduire le nombre de lignes affichées avec la possibilité de dérouler.

#### 16. Affichez à l’écran le fichier ___/etc/passwd___ ; que contient-il ? Quelle commande permet d’afficher la page de manuel de ce fichier ?

Le fichier ___/etc/passwd___ contient toutes les informations relatives aux utilisateurs (login, mots de passe, ...).
Ce fichier possède un format spécial permettant de repérer chaque utilisateur, chacune de ses lignes possède le format suivant:
```text
nom_du_compte : mot_de_passe : numero_utilisateur : numero_de_groupe
: commentaire : répertoire : programme_de_demarrage
```
#### 17. Affichez seulement la première colonne triée par ordre alphabétique inverse

```console
serveur@serveur:~$ cat /etc/passwd | cut -d: -f1 | sort
```

#### 18. Quelle commande nous donne le nombre d’utilisateurs ?

```console
serveur@serveur:~$ wc -l /etc/passwd
```

#### 19. Combien de pages de manuel comportent le mot-clé ___conversion___ dans leur description ?

/*Je sais pas*/

#### 20. A l’aide de la commande ___find___, recherchez tous les fichiers se nommant ___passwd___ présents sur la machine

```console
serveur@serveur:~$ find / -name passwd
```

#### 21. Modifiez la commande précédente pour que la liste des fichiers trouvés soit enregistrée dans le fichier ___~/list_passwd_files.txt___ et que les erreurs soient redirigées vers le fichier spécial ___/dev/null___

```console
serveur@serveur:~$ find / -name passwd > lis_passwd_files.txt 2> /dev/null
```

#### 22. Dans votre dossier personnel, utilisez la commande ___grep___ pour chercher où est défini l’alias ___ll___ vu précédemment

/*Je sais pas*/

#### 23. Utilisez la commande ___locate___ pour trouver le fichier ___history.log___

```console
serveur@serveur:~$ locate history.log
/var/log/apt/history.log
serveur@serveur:~$ 
```

#### 24. Créer un fichier dans votre dossier personnel puis utilisez locate pour le trouver. Apparaît-il ? Pourquoi ?

En utilisant locate, nous ne retrouvons pas le fichier créé. 
En effet, la commande ___locate___ ne cherche pas les fichiers dans l'arborescence des fichiers systèmes mais plutôt dans une base de données contenant tous les fichiers.
Généralement, cette base de données est mise à jour à 3h du matin. Nous pouvons également la mettre à jour manuellement.
```console
serveur@serveur:~$ sudo updatedb
```
Après cela, nous retrouvons le fichier créé avec la commande ___locate___.

## Exercice 4 : Personnalisation du shell

#### 3. Modification du fichier .bashrc

L'invite de commande devient coloré, avec en vert le nom de l'utilisateur et le nom de la machine et en bleu foncé le répertoire courant.

#### 4. Modification de l’invite de commande pour qu’elle s’affiche sous la forme demandée

Remplacer la ligne de code suivante:
```bash
PS1="${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$"
```
Par cette ligne:
```bash
PS1="${debian_chroot:+($debian_chroot)}\[\033[01;35m\][\A] \[\033[00m\]- \[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;36m\]\w\[\033[00m\]\$"
```
