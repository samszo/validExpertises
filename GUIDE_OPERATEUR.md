# Guide operateur

Ce guide est destine aux personnes qui utilisent l'application pour evaluer des expertises, sans intervention technique sur le code.

Version courte imprimable: [FICHE_OPERATEUR_1_PAGE.md](FICHE_OPERATEUR_1_PAGE.md)

## Objectif

L'application permet de:

- rechercher puis charger une personne;
- consulter les expertises deja presentes;
- ajouter un mot-clef si necessaire;
- ajouter votre propre evaluation;
- conserver une trace de vos actions dans Omeka S.

## Avant de commencer

Verifier les elements suivants:

1. Vous avez l'URL de l'application ouverte dans un navigateur.
2. Vous disposez des informations Omeka S suivantes:
   - URL API;
   - mail du proprietaire Omeka S associe a la cle API;
   - key_identity;
   - key_credential;
   - ID de votre fiche evaluateur.
3. Vous connaissez le nom ou l'identifiant de la personne a evaluer.

## Premiere configuration (une seule fois)

1. Cliquer sur Parametres.
2. Renseigner:
   - URL de base de l'API;
   - mail du proprietaire / utilisateur Omeka S;
   - key_identity;
   - key_credential;
   - Type autorise pour la personne;
   - Propriete des expertises;
   - ID evaluateur;
   - Propriete de rang.
3. Cliquer sur Enregistrer.

Resultat attendu:

- un message de confirmation apparait;
- vos parametres sont conserves pour la prochaine ouverture.

## Lancer une evaluation

1. Saisir quelques lettres du nom de la personne dans le champ de recherche.
2. Selectionner la bonne personne dans la liste d'autocompletion.
3. Verifier le bandeau personne (nom et ID).
4. Parcourir les cartes de mots-cles/expertises affichees.

Alternative:

- si l'application est ouverte avec un parametre `idAuthor` dans l'URL, la personne peut etre chargee automatiquement a l'ouverture.

Si aucun resultat n'apparait:

- verifier que la personne possede bien des expertises dans la propriete attendue;
- verifier que le type de la personne correspond au type autorise.

Si la bonne personne n'apparait pas dans l'autocompletion:

- verifier que le vocabulaire de recherche est bien indexe dans Omeka S;
- essayer avec une autre partie du nom ou avec un identifiant connu.

## Ajouter un mot-clef avant evaluation

1. Utiliser le champ `Ajouter un mot-clef...`.
2. Saisir quelques lettres du mot-clef recherche.
3. Selectionner le mot-clef dans la liste d'autocompletion.
4. Verifier que la carte apparait dans la grille avant de saisir une valeur.

## Modifier une valeur d'expertise

1. Sur une carte, deplacer le slider entre `-100` et `+100`.
2. Observer la mise a jour immediate de la valeur affichee.
3. Selon le cas:
   - cliquer sur Ajouter pour creer votre expertise;
   - si une expertise existe deja, verifier d'abord l'etat affiche sur la carte, car le parcours principal actuellement documente est l'ajout.

Recommandation de saisie:

- valeurs negatives: expertise faible;
- proche de `0`: neutre/incertain;
- valeurs positives: expertise forte.

## Bonnes pratiques operateur

- Evaluer avec une echelle coherente d'une personne a l'autre.
- Eviter les changements rapides successifs sans verification visuelle.
- Noter les cas ambigus (expertise contradictoire, donnees incompletes).
- Recontroler la presence de votre nom/evaluateur dans la carte avant validation.
- Utiliser les filtres `Sans votre expertise` et `Avec votre expertise` pour avancer de maniere systematique.
- Utiliser le tri par rang pour relire rapidement les valeurs extremes.

## Controle qualite rapide

Avant de passer a la personne suivante:

1. Verifier que la valeur affichee est celle voulue.
2. Verifier que l'action `Ajouter` a bien ete executee.
3. Confirmer qu'aucun message d'erreur n'est visible.
4. Recharger la personne pour verifier la persistance des donnees.

## Messages et incidents frequents

1. Erreur HTTP lors du chargement
Action:
- verifier URL API et credentials;
- verifier l'accessibilite de l'instance Omeka S.

2. Type de personne non autorise
Action:
- verifier le type de l'item charge;
- verifier la valeur du parametre Type autorise.

3. Impossible d'enregistrer
Action:
- verifier vos droits API;
- verifier l'existence des proprietes ciblees dans Omeka S.

4. Le mot-clef recherche n'apparait pas
Action:
- verifier que le mot-clef existe bien dans Omeka S;
- verifier que la classe de mot-clef attendue est correctement chargee.

## Procedure de reprise

En cas de doute sur l'etat de sauvegarde:

1. Noter l'ID de la personne en cours.
2. Recharger la page.
3. Reouvrir Parametres et verifier les valeurs.
4. Recharger la personne.
5. Verifier les valeurs presentes avant toute nouvelle modification.

## Checklist de fin de session

1. Toutes les personnes prevues ont ete traitees.
2. Les valeurs critiques ont ete verifiees une seconde fois.
3. Aucun message d'erreur non resolu n'est present.
4. Les parametres operateur sont corrects pour la prochaine session.
