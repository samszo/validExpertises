# Guide operateur

Ce guide est destine aux personnes qui utilisent l'application pour evaluer des expertises, sans intervention technique sur le code.

Version courte imprimable: [FICHE_OPERATEUR_1_PAGE.md](FICHE_OPERATEUR_1_PAGE.md)

## Objectif

L'application permet de:

- charger une personne;
- consulter les expertises deja presentes;
- ajouter votre propre evaluation;
- conserver une trace de vos actions dans Omeka S.

## Avant de commencer

Verifier les elements suivants:

1. Vous avez l'URL de l'application ouverte dans un navigateur.
2. Vous disposez des informations Omeka S suivantes:
   - URL API;
   - key_identity;
   - key_credential;
   - ID de votre fiche evaluateur.
3. Vous connaissez l'ID de la personne a evaluer.

## Premiere configuration (une seule fois)

1. Cliquer sur Parametres.
2. Renseigner:
   - URL de base de l'API;
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

1. Saisir l'ID de la personne dans le champ de recherche.
2. Cliquer sur Charger.
3. Verifier le bandeau personne (nom et ID).
4. Parcourir les cartes de mots-cles/expertises affichees.

Si aucun resultat n'apparait:

- verifier que la personne possede bien des expertises dans la propriete attendue;
- verifier que le type de la personne correspond au type autorise.

## Modifier une valeur d'expertise

1. Sur une carte, deplacer le slider entre `-100` et `+100`.
2. Observer la mise a jour immediate de la valeur affichee.
3. Selon le cas:
   - cliquer sur Ajouter pour creer votre expertise;
   - cliquer sur Modifier si une expertise de votre part existe deja.

Recommandation de saisie:

- valeurs negatives: expertise faible;
- proche de `0`: neutre/incertain;
- valeurs positives: expertise forte.

## Bonnes pratiques operateur

- Evaluer avec une echelle coherente d'une personne a l'autre.
- Eviter les changements rapides successifs sans verification visuelle.
- Noter les cas ambigus (expertise contradictoire, donnees incompletes).
- Recontroler la presence de votre nom/evaluateur dans la carte avant validation.

## Controle qualite rapide

Avant de passer a la personne suivante:

1. Verifier que la valeur affichee est celle voulue.
2. Verifier que l'action (Ajouter/Modifier) a bien ete executee.
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
