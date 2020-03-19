# Compte-Rendu Administration Système (MOINE PINET)
# Partie 1 
##  Gestion des utilisateurs et des groupes

1.Commencez par créer deux groupes groupe1 et groupe2

Commandes :
* *sudo addgroup groupe1* => Obligation de rajouter *sudo* car nous n'avons pas les droits sinon. 
* *sudo addgroup groupe2*

**NB** : on peut vérifier la création des deux groupes à l'aide de la commande *cat/etc/group | tail 2* => affiche : groupe1 :x :1001 ; groupe2 :x :1002 
 
2.Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell 

Commandes :
* *sudo useradd u1 -m --shell /bin/bash* => création de l'utilisateur u1 
* *sudo useradd u2 -m --shell /bin/bash* => *-m* : le répertoire personnel de l'utilisateur sera créé s'il n'existe pas déjà
* *sudo useradd u3 -m --shell /bin/bash* => *--shell /bin/bash* : sélectionne le shell à utiliser pour exécuter les commandes du terminal sachant que le chemin du shell Bash est */bin/bash*. 
* *sudo useradd u4 -m --shell /bin/bash*
 
3.Ajoutez les utilisateurs dans les groupes créés : - u1, u2, u4 dans groupe1 - u2, u3, u4 dans groupe2 

Commandes :
* *sudo usermod -a -G groupe1 u1* => *-a* : ajoute l'utilisateur aux groupes supplémentaires spécifié
* *sudo usermod -a -G groupe1 u2* => *-G* : liste de groupes supplémentaires auxquels fait également partie l'utilisateur. Si l'utilisateur fait actuellement partie d'un groupe qui n'est pas listé, l'utilisateur sera supprimé du groupe.
* *sudo usermod -a -G groupe1 u4*
* *sudo usermod -a -G groupe2 u2* 
* *sudo usermod -a -G groupe2 u3* 
* *sudo usermod -a -G groupe2 u4* 
 
4.Donnez deux moyens d’afficher les membres de groupe2

Commandes :
* *cat /etc/group \| grep "groupe2"*
* *members groupe2* => ( en ayant préalablement installer le paquet members : *sudo apt install members*) 
 
5.Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4 

Commandes :
* *sudo chown :groupe1 /home/u1*
* *sudo chown :groupe1 /home/u2*
* *sudo chown :groupe2 /home/u3*
* *sudo chown :groupe2 /home/u4* 
 
6.Remplacez le groupe primaire des utilisateurs : - groupe1 pour u1 et u2 - groupe2 pour u3 et u4 

Commandes :
* *sudo usermod -g groupe1 u1* => *-g* : le nom ou le numéro du groupe de connexion initial de l'utilisateur
* *sudo usermod -g groupe1 u2* 
* *sudo usermod -g groupe2 u3*
* *sudo usermod -g groupe2 u4* 
 
7.Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé. 

Commandes :
* *mkdir /home/groupe1 && mkdir /home/groupe2*
* *chown u1:groupe1 groupe1 && chown u2:groupe2 groupe2* 
* *chmod 720 groupe1 groupe2*
 
 
8.Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ? 

Commandes :
* *chmod +t /home/groupe1/ && chmod +t /home/groupe2/*
 
**NB** : +t correspond au "Sticky bit", c'est-à-dire qu'il permet seulement au propriétaire du fichier, au propriétaire du dossier ou root de renommer ou supprimer des fichiers dans ce dossier. 
 
9.Pouvez-vous vous connecter en tant que u1 ? Pourquoi ? 

Tout compte créé sans mot de passe est inactif, jusqu’à l’attribution d’un mot de passe ; d'où l'impossibilité de se connecter à u1.
 
10.Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.  

Commandes :
* *sudo passwd u1*  #entrer mdp su u1 
 
 
11.Quels sont l’uid et le gid de u1 ?  

Commandes :
* *id u1* => retourne uid=1001(u1) gid=1001(groupe1) groups=1001(groupe1) 
 
12.Quel utilisateur a pour uid 1003 ? 

Commandes :
* *id 1003* => on trouve u3
 
13.Quel est l’id du groupe groupe1 ?

Commandes :
* *cat /etc/groupe \| grep groupe1* => retourne 1001 
 
14.Quel groupe a pour guid 1002 ? (Rien n’empêche d’avoir un groupe dont le nom serait 1002...) 

Commandes :
* *cat /etc/groupe \| grep 1002* => retourne groupe2 
 
 
15.Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez. 

Commandes :
* *sudo gpasswd -d u3 groupe2* => *-d* :  utilisateur groupe

**NB** : l'utilisateur u3 ne peut plus accéder à /home/groupe2 (il ne fait plus partie du groupe 2) 
 
16.Modifiez le compte de u4 de sorte que :
* il expire au 1er juin 2020 : *usermod --expiredate 06/01/2019 u4*
* il faut changer de mot de passe avant 90 jours : *chage -M 90 u4*
* il faut attendre 5 jours pour modifier un mot de passe : *sudo chage -m 5 u4* 
* l’utilisateur est averti 14 jours avant l’expiration de son mot de passe : *sudo chage -W 14 u4* 
* le compte sera bloqué 30 jours après expiration du mot de passe : *sudo chage -I 30 u4 *
 
17.Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ? 
 
C'est /bin/bash( commande : echo $SHELL) 

18.A quoi correspond l’utilisateur nobody ? 
 
nobody est une pseudo utilisateur qui représente un utilisateur n'ayant aucun privilège, n'étant propriétaire d'aucun fichier et n'appartenant à aucun groupe 
 
19.Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? 
Quelle commande permet de forcer sudo à oublier votre mot de passe ? 
 
Par défaut, le mot de passe est gardé en mémoire pendant 15 minutes. Pour forcer l'oubli : *commande sudo -k* . 

# Partie 2
## Gestion des permissions

### 2.1 Introduction relative aux droits d'administrateur (sources : https://doc.ubuntu-fr.org/droits)

Lorsque l'on manipule les commandes *ls -l* ou *stat*, on peut observer un ensemble d'informations dont certaine sont relatives aux droits d'aministrateur (ou encore droits d'accès). Elles se présentent sous la forme de 9 bits, groupés sous 3 sous-groupes différents, à savoir le premier groupe relevant de l'utilisateur propriétaire du fichier (u) (généralement le créateur du fichier), le second faisant référence au groupe propriétaire du fichier (g). En ce sens, si un utilisateur est membre d'un certain groupe qui possède la propriété d'un fichier, l'utilisateur aura aussi certaines permissions particulières sur ce fichier. Enfin, le troisième groupe s'associe aux autres, other, le reste du monde (o) (tout un chacun n'étant ni propriétaire du fichier, ni membre du groupe propriétaire du fichier).

Par la suite, comme énoncé précedemment, les 9 bits se présentent de la manière suivante. Le premier symbole (précedent la donnée composée des 9 bits) peut être « - », « d », soit « l », entres autres. Il indique la nature du fichier :

* \- : fichier classique
* d : répertoire
* l : lien symbolique

Suivent ensuite 3 groupes de 3 symboles chacun, indiquant si le fichier (ou répertoire) est autorisé en lecture, écriture ou exécution. 
Pour rappel, les 3 groupes correspondent, dans cet ordre, aux droits du propriétaire, du groupe puis du reste des utilisateurs. 
Ce sont ces lettres qui sont utilisées pour symboliser les dites permissions. Si la permission n'est pas accordée, la lettre en question est remplacé par « - ». Si l'on reprend les lettres données, dans l'ordre fourni par le système, pour lecture/écriture/exécution (read/write/execute), nous obtenons : rwx.

### 2.2 Exercice

1.Dans votre **$HOME**, créez un dossier *test*, et dans ce dossier un fichier *fichier* contenant quelques lignes de texte. Quels sont les droits sur *test* et *fichier* ?

Commandes :

* *cd $HOME* => permet de se déplacer dans le répertoire associé à la variable d'environnement **HOME**
* *mkdir test* => permet de créer le dossier *test* (qui se trouvera, en conséquence, d'après la commande ci-dessus, dans *~/*)
* *cd test* => permet de se déplacer dans le répertoire *test*
* *touch fichier* => permet de créer le fichier *fichier* dans *~/test/*
* *nano fichier* => permet d'éditer le fichier *fichier* et d'y taper du texte
* *ls -l* => permet d'afficher le contenu du répertoire courant (si la commande *ls -l* n'a pas d'argument), en l'occurence, dans le cas présent, le répertoire *test*. Cette commande affiche aussi les droits relatifs aux fichiers contenus dans le répertoire. Le résultat de la commande est donnée ci-dessous (voir ***bis_1***).
* *cd $HOME* => voir ci-dessus pour explications
* *ls -l* => permet de voir le contenu de $HOME dans le cas présent. Le résultat de la commande est donnée ci-dessous (voir ***bis_2***).

***bis_1 :***

Les droits relatifs à *test* sont **drwxrwxrwx** :

      d => répertoire 
      r => read   (droits de lectures)      => pour utilisateur/groupe_propriétaire/autres
      w => write  (droits d'écritures)      => pour utilisateur/groupe_propriétaire/autres
      x => execute (droit d'éxécution)      => pour utilisateur/groupe_propriétaire/autres

***bis_2:***

Les droits relatifs à *fichier* sont **-rw-rw-r--** :

      - => fichier 
      r => read   (droits de lectures)      => pour utilisateur/groupe_propriétaire/autres
      w => write  (droits d'écritures)      => pour utilisateur/groupe_propriétaire
      x => execute (droit d'éxécution)      => personne n'a ce droit

**NB** : Les permissions accordées par défaut sont celles déterminées par un paramètre particulier appelé le masque utilisateur (ou user mask). Il est important de noter que l'opération qui retourne les droits relatifs à un fichier est différente en fonction des types de base (fichier ou répertoire). En soit, elle réalise la complémentation entre le masque de l'utilisateur et 0666(8) ((8) siginifie base 8) sur un type *fichier*, soit, si le masque est nulle, on associe les droits **-rw-rw-rw** ; alors que pour un *répertoire* c'est 0777(8), soit les droits **drwxrwxrwx** si le masque est nulle.

Dans Ubuntu, le masque utilisateur généralement définit par défaut par l'administrateur est **umask = 022**, soit il accorde les permissions **rwxr-xr-x**:

* le propriétaire du fichier dispose des permissions de lecture, d'écriture et d'exécution ;
* le groupe propriétaire dispose des droits de lecture et d'exécution, mais pas d'écriture ;
* le reste du monde dispose des droits de lecture et d'exécution, mais pas d'écriture.

2.Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’aﬀicher en tant queroot. Conclusion ?


Commandes :

* *cd ~/test*
* *chmod u-rwx fichier* => retire tous droits sur ce fichier (en ce sens les droits de lecture, écriture, éxécution) pour nous
* *nano fichier* => impossible de voir le contenu de *fichier* ou même d'écrire dedans
* *sudo nano fichier* => possibilité d'éditer et de voir le contenu de *fichier* en étant super-utilisateur

**Conclusion :** Quelques soit le masque utilisteur ou bien les droits associés au fichier *test*, le super-utilisateur (ou *root*) a la possibilité de modifier, décrire ou bien d'éxécuter un fichier. Il possède tous les droits et ceux, quelques soit la configuration du serveur.

3.Redonnez vous les droits en écriture et exécution sur *fichier* puis exécutez la commande *echo "echo Hello" > fichier*. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?

Commandes :

* *echo "echo Hello" > fichier* => remplace le contenu de *fichier* (s’il existe déjà) par "echo Hello"

**NB** : Il est important de noter que seul le droit en écriture ne nous aurez pas suffit. En ce sens, il nécessaire d'avoir les droits d'éxécutions pour pouvoir éxécuter une commande sur le dit *fichier*. En outre le droit en écriture et éxécution nous permet d'avoir l'intégrité des droits nécessaire pour écrire sans problème. 

4.Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez.

Commandes :

* *cat fichier* => impossibilité d'éxécuter la commande *cat*
* *sudo cat fichier* => affiche : "echo Hello"

**NB**: Comme énoncé précedemment, il est impossible de réalisé l'éxécution de la commande *cat* sur *fichier*. Pour cela, il faut les droits de lecture, qui ne sont pas présent sur le dit *fichier*. Ne sont présent que les droits écriture et éxécution pour l'utilisateur (en l'occurence nous-même)

5.Placez-vous dans le répertoire *test*, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou aﬀichez le contenu du fichier *fichier*. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur *test*.

Commandes :

* *cd ~/test*
* *chmod u-r ~/test* => retire le droit de lecture à l'utilisateur (nous-même)
* *ls -l* => impossible de lire le contenu. En effet, il faut les droits de lecture du répertoire pour pouvoir en lister le contenu

6.Créez dans test un fichier *nouveau* ainsi qu’un répertoire *sstest*. Retirez au fichier *nouveau* et au répertoire *test* le droit en écriture. Tentez de modifier le fichier *nouveau*. Rétablissez ensuite le droit en écriture au répertoire *test*. Tentez de modifier le fichier *nouveau*, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations ?

Commandes :

* *cd ~/test*
* *touch nouveau* => crée le fichier *nouveau* dans le répertoire test
* *cd* => retourne dans le répertoire **HOME**
* *mkdir sstest* => crée le répertoire *sstest*
* *chmod a-w test* => retire le droit en écriture du répertoire *test* à tous le monde (u;g;o)
* *cd test*
* *chmod a-w nouveau* => retire le droit en ériture du fichier *nouveau* à tous le monde (u;g;o)
* *nano nouveau* => impossibilité d'enregistrer les modifications (soit d'écrire dans le fichier *nouveau*)
* *chmod a+w ~/test* => accorde le droit en écriture du répertoire *test* à tous le monde (u;g;o)
* *nano nouveau* => impossibilité d'enregistrer les modifications (soit d'écrire dans le fichier *nouveau*)
* *rm nouveau* => impossibilité de supprimer *nouveau*

**NB** : ainsi, il est impossible de modifier ou de supprimer un fichier sur lequel les droits en écriture ne sont pas accordés, et ceux malgrès que les droits en écriture sur le répertoire contenant ce-même fichier soit accordés.

7.Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire *test*. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire *test*, de vous y déplacer, d’en lister le contenu, etc ... Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?

Commandes :

* *cd* 
* *chmod a-x test* => retire le droit en éxécution du répertoire *test* à tous le monde (u;g;o)
* *cd test* => échec du déplacement dans le répertoire *test*
* *chmod a+x test* => octroie le droit en éxécution du répertoire *test* à tous le monde (u;g;o)
* *cd *test* => déplacement dans le répertoire *test*
* *chmod a-x ~/test* => retire le droit en éxécution du répertoire *test* à tous le monde (u;g;o)
* *touch newup* => échec de la création de fichier *newup* dans le répertoire *test* => nécessite les droits d'éxécution sur le répertoire *test*
* *rm nouveau* => échec de la suppression du fichier *nouveau* dans le répertoire => nécessite les droits d'éxécution sur le répertoire *test*
* *ls -l* => échec de la commande de listage des éléments du répertoire *test* => nécessite les droits d'éxécution sur le répertoire *test*

**NB** : Il est, en ce sens, d'après l'ensemble des commandes réalisées ci-dessus, important de déduire que quelquse soit les actions entreprises sur un répertoire, si ce dernier ne possède pas les droits d'éxécutions asociés, toutes seront vouées à échouer. Le droit d'éxécution est donc primordial pour entreprendre une quelconque action sur un répertoire.

8.Rétablissez le droit en exécution du répertoire *test*. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire *test*, de vous déplacer dans *sstest*, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd..” ? Pouvez-vous donner une explication ?

Commandes :

* *chmod a+x ~/test* => octroie le droit en éxécution du répertoire *test* à tous le monde (u;g;o)
* *cd ~/test*
* *chmod a-x ~/test* => retire le droit en éxécution du répertoire *test* à tous le monde (u;g;o)
* *rm nouveau* => échec de la suppression du fichier *nouveau* dans le répertoire => nécessite les droits d'éxécution sur le répertoire *test*
* *nano fichier* => impossibilité de créé ou éditer un fichier existant => nécessite les droits d'éxécution sur le répertoire *test*
* *touch newup* => échec de la création de fichier *newup* dans le répertoire *test* => nécessite les droits d'éxécution sur le répertoire *test*
* *ls -l ~/sstest* => liste le contenu de sstest
* *cd ..* => remonte dans le répertoire parent
* *cd ~/sstest* => se déplace dans sstest

**NB** : 
En ce sens les droits d'éxécutions n'influent que sur le répertoire courant ainsi que l'ensemble des actions (lire,écrire,supprimer) réaliser dans (ou sur)	ce dernier. Malgrès cela, il est toujours possible, depuis un répertoire sans	droit d'éxécutions de lire le contenu, écrire ou supprimer des fichiers dans des répertoires différents (où le droit d'éxécution est alloué) en passant en argument le chemin  absolu de ce dit répertoire (où le droit d'éxécution est alloué) aux commandes bash.
	
Il est possible de retourner dans le répertoire parent depuis *test* (sans droit d'éxécution) avec la commande *cd ..* . En effet cd à pour but de changer le répertoire courant en DIR sachant que le répertoire par défaut est la valeur de la  variable d'environnement **HOME**. Par la suite cd recherche le répertoire souhaité avec la variable CDPATH qui définit le chemin de recherche du répertoire contenant DIR. En ce sens, le point de départ de recherche est le répertoire **HOME**. En outre, *cd* s'éxécutera si il trouve le répertoire à partir de ce point de départ : la commande ne passe donc aucunement par le fichier *test* pour s'éxécuter, d'où la réussite dans l'éxécution de *cd ~/sstest*.

9.Rétablissez le droit en exécution du répertoire *test*. Attribuez au fichier *fichier* les droits suﬀisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.

* *chmod a+x ~/test* => octroie le droit en éxécution du répertoire *test* à tous le monde (u;g;o)
* *chmod g-w ~/test/fichier* => retire le droit d'écriture pour le groupe propriétaire du fichier (g)
* *chmod o-w ~/test/fichier* => retire le droit d'écriture pour les autres (o)

**NB** : Il nécessaire, pour le groupe propriétaire du fichier et les autres (g et o) de posséder les droits de lecture et d'éxécution pour pouvoir rentrer dans le répertoire *test* puis d'en lire le contenu comme expliqué ci-dessus.


10.Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

*umask* fonctionne de la manière suivante : sur une base de 0777(8) pour les répertoires (définissant, si le masque est nul, le droit **drwxrwxrwx**) et 0666(8) pour les fichiers (définissant, si le masque est nul, le droit **-rw-rw-rw-**), on retranche la valeur en octal spécifié en argument de la commande umask. En ce sens, on calcul la valeur en octal à retrancher, d'après la définition donnée en **2.1** (voir ci-dessus) des droits de permissions :

* *umask 077* 
* *umask -S* => affiche : (u=rwx,g=,o=) => interdit à quiconque à part à l'utilisateur (nous) l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires
* *touch newfile* => crée le fichier *newfile*
* *mkdir newrep* => crée le répertoire *newrep*
* *ls -l* =>  donne pour résultat : drwx------ newrep ; -rw------- newfile

Nous pouvons donc lire/écrire/éxécuter un dossier et lire/écrire un fichier. Tous autres individus n'y est nullement autorisés.

**NB** : ainsi, grâce à *umask*, on interdit à quiconque à part nous, l’accès en lecture ou en écriture, ainsi que la traversée de nos répertoires

11.Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.

Commandes :

* *umask 300*
* *umask -S* => affiche : (u=r,g=rwx,o=rwx) => autorise tout le monde à lire nos fichiers et traverser nos répertoires, mais ne nous autorise qu'à écrire
* *touch newupfile* => crée le fichier *newupfile*
* *mkdir newrupep* => crée le répertoire *newuprep*
* *ls -l* => donne pour résulat : dr--rwxrwx newuprep ; -r--rw-rw- newupfile

Nous pouvons ainsi que lire un dossier et lire un fichier. Tous autres individus peut  lire/écrire/éxécuter un dossier et lire/écrire un fichier.

**NB** : ainsi, grâce à *umask*, on autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais on ne s'autorise qu'à écrire

12.Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.

Commandes :

* *umask 33*
* *umask -S* => affiche : (u=rwx,g=r,o=r) => autorise l'utilisateur à lire/écrire/éxécuté mais n'autorise le reste (groupe propriétaire et les autres) qu'à lire	
* *touch wookie* => crée le fichier *wookie*
* *mkdir wookieup* => crée le répertoire *wookieup*
* *ls -l* =>  donne pour résulat : drwxr--r-- wookieup ; -rw-r--r-- wookie

Nous pouvons donc lire/écrire/éxécuter un dossier et lire/écrire un fichier. Tous autres individus ne peut que lire un dossier ou un fichier.

**NB** : ainsi, grâce à *umask*, on s'autorise un accès complet et on autorise un accès en lecture aux membres de notre groupe.

13.Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vouspourrez vous aider de la commande *stat* pour valider vos réponses) :

-chmod u=rx,g=wx,o=r fic
=> transcription : *chmod 534 fic*
-chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x---
=> transcription : *chmod u+w fic ; chmod o+w fic ; chmod g-rw fic* ou *chmod 602 fic*
-chmod 653 fic en sachant que les droits initiaux de fic sont 711
=> transcription : *chmod u-x fic ; chmod g-r fic ; chmod o-w fic* ou *chmod 653 fic*
-chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x---
=> transcription : *chmod u+x fic ; chmod g-rx fic ; chmod g+w fic ; chmod o-r fic* ou *chmod 520*

14.Aﬀichez les droits sur le programme *passwd*. Que remarquez-vous? En aﬀichant les droits du fichier */etc/passwd*, pouvez-vous justifier les permissions sur le programme *passwd* ?

Commandes :

* *stats /etc/passwd* => affiche : -rw-r--r-- => autorise l'utilisateur à lire/écrire *passwd* mais n'autorise le reste qu'à lire *passwd*

La contenance du fichier *passwd* est la suivante :

* nom de l'utilisateur pour l'identification sur le système (login)
* mot de passe chiffré
* numéro d'utilisateur (uid)
* numéro de groupe d'utilisateur par défaut (gid). Tout utilisateur est affecté à un et un seul groupe de base. Il peut, par ailleurs, faire partie d'un grand nombre d'autres groupes.
* nom complet. Ce champ peut contenir de nombreuses informations, il correspond à un champ de remarque
* répertoire de base de l'utilisateur
* programme à lancer au démarrage (programme de base), généralement un interpréteur de commande (shell).

En ce sens, *passwd* permet d'accéder au mot de passe de l'utilisateur et de le modifier. Il paraît ainsi logique de n'autoriser la modification qu'à l'utilisateur, propriétaire du mot de passe, afin de le protéger de personnes mal intentionées. De plus, la lecture est tout de même possible pour le groupe propriétaire et les autres. Le mot de passe étant chiffré, cela ne pose aucunement problème d'ouvrir lecture de ce fichier si ce n'est que malgrès que le mot de passe soit chiffré, il exite des casseurs de code. Si ce mot de passe s'avère trop "simple", alors l'attaquant pourra récupérer le mot de passe en "clear" après avoirs cassé le code chiffré à l'aide d'un casseur de code. En outre, peut-être faudrait-il interdire la lecture aux reste (g;o) pour plus de sécurité.



