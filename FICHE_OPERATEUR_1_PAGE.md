# Fiche operateur - 1 page

Usage: validation rapide des expertises dans validExpertises.

## 1) Preparation (debut de session)

- Ouvrir l'application dans le navigateur.
- Verifier les Parametres:
  - URL API Omeka S
  - Mail proprietaire / utilisateur Omeka S
  - key_identity
  - key_credential
  - ID evaluateur
  - Type personne autorise
  - Propriete des expertises
- Verifier que le nom de l'evaluateur s'affiche correctement.

## 2) Procedure standard (par personne)

1. Entrer quelques lettres du nom de la personne.
2. Choisir la personne dans l'autocompletion.
3. Verifier le bandeau (nom + ID).
4. Si besoin, ajouter un mot-clef via le champ dedie.
5. Ajuster les sliders d'expertise (de -100 a +100).
6. Cliquer sur Ajouter.
7. Verifier l'absence de message d'erreur.

## 3) Regles de notation (recommandees)

- -100 a -1: expertise faible
- 0: neutre / non tranche
- +1 a +100: expertise forte

Conseil: rester coherent d'une personne a l'autre.

## 4) Controle qualite (30 secondes)

- La valeur affichee correspond a votre decision.
- L'action Ajouter a ete executee.
- Aucun message d'erreur visible.
- Recharger la personne si doute de persistance.

Astuce: utiliser les filtres `Sans votre expertise` puis `Avec votre expertise` pour verifier l'avancement.

## 5) Incidents courants

- Erreur HTTP
  - Verifier URL API, credentials, disponibilite Omeka S.
- Type non autorise
  - Verifier le type de l'item et le parametre Type autorise.
- Enregistrement impossible
  - Verifier droits API et proprietes cibles dans Omeka S.

## 6) Fin de session

- Verifier les cas critiques une seconde fois.
- Noter les cas ambigus a revoir.
- Laisser des parametres propres pour la prochaine session.

## Aide

Guide complet: GUIDE_OPERATEUR.md
Documentation projet: README.md
