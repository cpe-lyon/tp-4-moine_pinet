# Compte-Rendu Administration Système (MOINE PINET)
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

## 2.2 Exercice

1.Dans votre **$HOME**, créez un dossier *test*, et dans ce dossier un fichier *fichier* contenant quelques lignes de texte. Quels sont les droits sur *test* et *fichier* ?

Commandes :

* *cd $HOME* => permet de se déplacer dans le répertoire associé à la variable d'environnement **HOME**
* *mkdir test* => permet de créer le dossier *test* (qui se trouvera, en conséquence, d'après la commande ci-dessus, dans *~/*)
* *cd test* => permet de se déplacer dans le répertoire *test*
* *touch fichier* => permet de créer le fichier *fichier* dans *~/test/*
* *nano fichier* => permet d'éditer le fichier *fichier* et d'y taper du texte
* *ls -l* => permet d'afficher le contenu du répertoire courant (si la commande *ls -l* n'a pas d'argument), en l'occurence, dans le cas présent, le répertoire *test*. Cette commande affiche aussi les droits relatifs aux fichiers contenus dans le répertoire. Le résultat de la commande est donnée ci-dessous (voir *bis_1*).
* *cd $HOME* => voir ci-dessus pour explications
* *ls -l* => permet de voir le contenu de $HOME dans le cas présent. Le résultat de la commande est donnée ci-dessous (voir *bis_2*).

**bis_1 :**

Les droits relatifs à *test* sont **drwxrwxrwx** :

      d => répertoire 
      r => read   (droits de lectures)      => pour utilisateur/groupe_propriétaire/autres
      w => write  (droits d'écritures)      => pour utilisateur/groupe_propriétaire/autres
      x => execute (droit d'éxécution)      => pour utilisateur/groupe_propriétaire/autres

**bis_2:**

Les droits relatifs à *fichier* sont **-rw-rw-r--** :

      - => fichier 
      r => read   (droits de lectures)      => pour utilisateur/groupe_propriétaire/autres
      w => write  (droits d'écritures)      => pour utilisateur/groupe_propriétaire
      x => execute (droit d'éxécution)      => personne n'a ce droit

**NB** : Les permissions accordées par défaut sont celles déterminées par un paramètre particulier appelé le masque utilisateur (ou user mask). Il est important de noter que l'opération qui retourne les droits relatifs à un fichier est différente en fonction des types de base (fichier ou répertoire). En soit, elle réalise la complémentation entre le masque de l'utilisateur et 0666(8) ((8) siginifie base 8) sur un type *fichier*, soit, si le masque est nulle, on associe les droits **-rw-rw-rw** ; alors que pour un *répertoire* c'est 0777(8), soit les droits **drwxrwxrwx** si le masque est nulle.

Dans Ubuntu, le masque utilisateur généralement definit par défaut par l'administrateur est **umask = 022**, soit il accorde les permissions **rwxr-xr-x**:

* le propriétaire du fichier dispose des permissions de lecture, d'écriture et d'exécution ;
* le groupe propriétaire dispose des droits de lecture et d'exécution, mais pas d'écriture ;
* le reste du monde dispose des droits de lecture et d'exécution, mais pas d'écriture.

2.Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’aﬀicher entant queroot. Conclusion?

3.Redonnez vous les droits en écriture et exécution surfichierpuis exécutez la commandeecho "echoHello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’unfichier s’il existe déjà. Que peut-on dire au sujet des droits?

4.Essayez d’exécuter le fichier. Est-ce que cela fonctionne? Et avec sudo? Expliquez.

5.Placez-vous dans le répertoiretest, et retirez-vous le droit en lecture pour ce répertoire. Listez lecontenu du répertoire, puis exécutez ou aﬀichez le contenu du fichierfichier. Qu’en déduisez-vous?Rétablissez le droit en lecture surtest

6.Créez dans test un fichiernouveauainsi qu’un répertoiresstest. Retirez au fichiernouveauet aurépertoiretestle droit en écriture. Tentez de modifier le fichiernouveau. Rétablissez ensuite le droiten écriture au répertoiretest. Tentez de modifier le fichiernouveau, puis de le supprimer. Que pouvez-vous déduire de toutes ces manipulations?

7.Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoiretest.Tentez de créer, supprimer, ou modifier un fichier dans le répertoiretest, de vous y déplacer, d’enlister le contenu, etc...Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires?

8.Rétablissez le droit en exécution du répertoiretest. Positionnez vous dans ce répertoire et retirez luià nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoiretest, de vous déplacer dansssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence desdroits que l’on possède sur le répertoire courant? Peut-on retourner dans le répertoire parent avec ”cd..”? Pouvez-vous donner une explication?

9.Rétablissez le droit en exécution du répertoiretest. Attribuez au fichierfichierles droits suﬀisantspour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.

10.Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture,ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.

11.Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos réper-toires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.

12.Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture auxmembres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.

13.Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vouspourrez vous aider de la commandestatpour valider vos réponses) :
-chmod u=rx,g=wx,o=r fic
-chmod uo+w,g-rx ficen sachant que les droits initiaux de fic sont r--r-x---
-chmod 653 ficen sachant que les droits initiaux de fic sont 711
-chmod u+x,g=w,o-r ficen sachant que les droits initiaux de fic sont r--r-x---

14.Aﬀichez les droits sur le programmepasswd. Que remarquez-vous? En aﬀichant les droits du fichier/etc/passwd, pouvez-vous justifier les permissions sur le programmepasswd?
