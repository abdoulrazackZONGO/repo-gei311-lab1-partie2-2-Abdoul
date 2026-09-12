LABORATOIRE 1 - 6GEI311 - Architecture des logiciels
PARTIE 2 - SITUATION 2
Correction d'un commit erroné poussé sur la branche principale

Etudiants : Abdoul Razack Tégawindé ZONGO  ;  Compte GitHub : abdoulrazackZONGO

Coéquipier : Tounwendsida Julien ZONGO     ;  Compte GitHub : zongo-julien

Dépôt où je suis membre A :
https://github.com/abdoulrazackZONGO/repo-gei311-lab1-partie2-2-Abdoul

Dépôt où je suis membre B :
https://github.com/zongo-julien/repo-gei311-lab1-partie2-2-Zongo_Julien


ETAPES 1 A 3 - Mise en place par le membre A
   Création du dépôt sur GitHub, clonage local, création de Dossier_A
   et du fichier textA.txt contenant "labo 1, gestion de version,
   Abdoul", puis commit et synchronisation :

       git clone https://github.com/abdoulrazackZONGO/repo-gei311-lab1-partie2-2-Abdoul.git
       cd repo-gei311-lab1-partie2-2-Abdoul
       mkdir Dossier_A
       echo "labo 1, gestion de version, Abdoul" > Dossier_A\textA.txt
       git add -A
       git commit -m "Ajout de Dossier_A/Text_A"
       git push -u origin main

   Commit résultant : 99b5eef  "Ajout de Dossier_A/Text_A"


ETAPES 4 A 8 - Travail du membre B (Julien), erreur incluse
   Le membre B clone le depot, puis réalise trois commits locaux
   successifs avant de les synchroniser en une seule fois :

   Etape 4-5 : création de Dossier_B/textB.txt et commit local
       9d09004  "Ajout du Dossier_B et de son fichier textB.txt"

   Etape 6 : ajout de la ligne "erreur injectée" dans
             Dossier_A/textA.txt et commit local
       55752c7  "Injection de l'erreur dans Dossier_A/textA.txt"
       >>> COMMIT FAUTIF <<<

   Etape 7 : ajout de Dossier_B/textB2.txt et commit local
       262def1  "Ajout de Dossier_B/textB2.txt"

   Etape 8 : synchronisation des trois commits vers le dépot distant
       git push origin main

   A l'issue de cette étape, la branche principale distante contient donc
   la ligne erronée, ce qui contrevient au principe rappelé en introduction
   de l'énoncé : la branche principale doit toujours rester fonctionnelle.


ETAPE 9 - Création de l'issue par le membre A
   Une issue a été ouverte sur GitHub avec la structure suivante :

   Titre : Ligne "erreur injectée" présente dans Dossier_A/textA.txt

   - Description du problème : le fichier Dossier_A/textA.txt contient la
     ligne "erreur injectée", qui ne devrait pas s'y trouver.
   - Commit concerne : lien cliquable vers le commit fautif.
   - Comportement attendu : textA.txt ne doit contenir que la phrase
     "labo 1, gestion de version, Abdoul" et la ligne de collaboration
     légitime.
   - Comportement observe : une ligne supplémentaire "erreur injectée" a
     été ajoutée.
   - Etapes pour reproduire : cloner le depot, ouvrir Dossier_A/textA.txt,
     constater la presence de la ligne.

   Le choix a été fait de fournir un LIEN vers le commit fautif plutot
   que son seul identifiant en texte, afin que le membre B puisse
   consulter directement le changement concerne.


ETAPE 10 - Réponse du membre B a l'issue
   Le membre B a répondu en commentaire de l'issue en indiquant :
       - le commit problématique : 55752c7
       - le dernier commit fonctionnel : 9d09004


   C'est volontairement le membre B, et non le membre A, qui fournit ces
   identifiants : le membre A signale le symptôme observe, tandis que le
   membre B, auteur des commits, investigue son propre historique pour
   en identifier la cause exacte.


ETAPE 11 - Retour du membre A vers la dernière version non erronée
   Le membre A doit retirer la ligne erronée de la branche principale.
   La commande utilisée est "git revert" :

       git checkout main
       git pull origin main
       git revert 55752c7
       git push origin main

   Commit resultant :
       d476edc  Revert "Injection de l'erreur dans Dossier_A/textA.txt"

   JUSTIFICATION DU CHOIX DE "git revert" PLUTOT QUE "git reset --hard" :
   La branche principale est une branche PARTAGEE, déjà récupérée par le
   membre B. Utiliser "git reset --hard" suivi d'un "git push --force"
   réécrirait l'historique distant et désynchroniserait le depot local du
   membre B. "git revert" crée au contraire un nouveau commit qui annule
   l'effet du commit fautif, sans rien supprimer de l'historique : la
   correction s'ajoute au bout de la branche et se propage normalement
   par un simple "git pull". C'est la méthode adaptée a une branche
   publique.


ETAPE 12 - Liste de l'historique local du membre B
   Le membre B liste ses commits locaux en ligne de commande :

       git log --oneline

   Cette commande lui permet de repérer visuellement le commit fautif
   ainsi que le dernier commit dont le contenu est valide.


ETAPE 13 - Retour du membre B vers le dernier commit fonctionnel
   Le membre B revient, dans son depot LOCAL, au dernier commit
   fonctionnel, c'est-a-dire juste avant l'injection de l'erreur :

       git reset --hard 9d09004

   JUSTIFICATION DU CHOIX DE "git reset --hard" ici :
   Contrairement a l'étape 11, il s'agit de l'historique LOCAL du membre
   B, que personne d'autre n'a récupéré. "git reset --hard" est donc sans
   risque. Le pointeur de branche recule,
   retirant de la chronologie locale a la fois le commit erroné (55752c7)
   et le commit suivant (262def1).


ETAPE 14 - Reprise des changements qui n'étaient pas erronés
   Le "reset" de l'étape 13 ayant également supprime le commit 262def1
   (l'ajout de textB2.txt, dont le contenu était pourtant valide), il
   faut refaire ce changement :

       echo "autre changement de B" > Dossier_B\textB2.txt
       git add -A
       git commit -m "Reprise de l'étape 7 après le reset impliquant la creation de Dossier_B/textB2.txt"

   Commit resultant :
       9ec2b84  "Reprise de l'étape 7 après le reset impliquant la
                 creation de Dossier_B/textB2.txt"


ETAPE 15 - Commit local et synchronisation avec le depot distant
   A ce stade, les deux historiques ont divergé :
       - la branche distante a avancé avec le commit de revert du membre A
       - la branche locale du membre B a recule puis avance différemment

   Un push direct est donc rejeté par GitHub (non-fast-forward). Le
   membre B récupère d'abord la correction du membre A, puis fusionne :

       git pull origin main
       (resolution de la divergence, puis commit de fusion)
       git push origin main

   Commit résultant :
       57d57d7  "Fusion après reset local et revert distant"

   JUSTIFICATION : Le membre A corrige la branche partagée par un revert, le
   membre B nettoie son historique local par un reset : les deux
   historiques divergent nécessairement, et la fusion est la manière
   correcte de les réconcilier sans écraser le travail de l'autre. 


VERIFICATION FINALE
   git pull origin main
   type Dossier_A\textA.txt      -> ne contient plus "erreur injectée"
   dir Dossier_B                 -> contient textB.txt et textB2.txt
