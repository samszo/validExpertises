# validExpertises

Application web front-end pour visualiser, comparer et valider des expertises autour d'une personne dans Omeka S.

L'interface permet de rechercher une personne Omeka S par autocompletion, de charger ses concepts d'expertise, d'afficher les évaluations existantes et d'ajouter une expertise pour l'évaluateur courant via l'API Omeka S.

## Guide operateur

Pour un mode d'emploi non technique, voir [GUIDE_OPERATEUR.md](GUIDE_OPERATEUR.md).
Pour une version courte imprimable, voir [FICHE_OPERATEUR_1_PAGE.md](FICHE_OPERATEUR_1_PAGE.md).

## Fonctionnalites

- Recherche d'une personne Omeka S par autocompletion.
- Chargement direct d'une personne via le parametre d'URL `idAuthor`.
- Verification du type de la personne (type autorise configurable).
- Recuperation des concepts d'expertise relies a la personne.
- Affichage des expertises existantes (annotations et items Expertise lies a la personne).
- Ajustement de la valeur d'expertise via slider (`-100` a `+100`).
- Ajout d'un mot-clef par autocompletion avant evaluation.
- Tri des mots-clefs par titre ou par rang.
- Filtrage des mots-clefs par texte, etat de saisie, polarite positive ou negative.
- Creation d'un item Expertise pour l'evaluateur courant.
- Parametrage sauvegarde en local (`localStorage`).

## Architecture

```text
validExpertises/
	index.html
	assets/
		main.css
		data/
			expertise.json
		js/
			appUrl.js
			auth.js
			authParams.js
			d3.min.js
			omk.js
```

## Prerequis

- Un serveur web local (Apache, Nginx, `python -m http.server`, etc.).
- Une instance Omeka S accessible en HTTP(S).
- Une cle API Omeka S (`key_identity`, `key_credential`) avec droits de lecture/ecriture sur les ressources ciblees.
- Les vocabulaires/proprietes utilises dans votre Omeka S (ex. `skos:hasTopConcept`, `curation:rank`, `dcterms:creator`, `dcterms:source`, `valo:expertise`).
- D3 charge localement via `assets/js/d3.min.js`.

## Installation rapide

1. Cloner le depot.
2. Servir le dossier `validExpertises` via un serveur web.
3. Ouvrir l'application dans le navigateur.

Exemple minimal:

```bash
cd validExpertises
python3 -m http.server 8080
```

Puis ouvrir `http://localhost:8080`.

## Configuration

La configuration est initialisee depuis `assets/js/authParams.js`, puis surchargeable depuis la fenetre Parametres dans l'UI.

Parametres utilises:

- `apiOmk` : URL de base de l'API Omeka S (ex. `http://localhost/omk_paragraphe/api/`).
- `mail` : mail du proprietaire Omeka S utilise pour identifier l'utilisateur de la cle API.
- `ident` : `key_identity` Omeka S.
- `key` : `key_credential` Omeka S.
- `creatorId` : ID de l'item Omeka S representant l'evaluateur.
- `personTypeAllow` : type d'item autorise pour la personne (ex. `valo:EnseignantChercheur`).
- `personPropExp` : propriete listant les expertises de la personne (ex. `skos:hasTopConcept`).
- `rankProp` : propriete Omeka S stockant la valeur d'expertise (par defaut `curation:rank`).

Important:

- Les parametres enregistres via l'UI sont stockes dans `localStorage` avec la cle `validExpertises_cfg`.
- Les parametres dans `authParams.js` servent de valeur par defaut.
- L'application charge aussi les classes Omeka S necessaires a la recherche de personnes et de mots-clefs.

## Utilisation

1. Ouvrir Parametres et verifier les informations API Omeka S.
2. Verifier que le nom de l'evaluateur s'affiche dans l'en-tete.
3. Rechercher une personne dans le champ principal, puis la selectionner dans la liste d'autocompletion.
4. Verifier les cartes d'expertise affichees.
5. Optionnel: ajouter un mot-clef via le champ `Ajouter un mot-clef...`.
6. Ajuster la valeur avec le slider sur chaque expertise.
7. Utiliser `Ajouter` pour creer votre expertise sur le mot-clef choisi.
8. Utiliser les outils de tri et de filtre pour controler les expertises deja traitees.

## Parametres URL

Les parametres suivants sont lus dans l'URL:

- `idCreator` : surcharge l'ID de l'evaluateur.
- `idAuthor` : charge automatiquement la personne a l'ouverture.

Exemple:

```text
http://localhost:8080/?idCreator=7457&idAuthor=123
```

## Flux API principal

- Recherche de personnes et de mots-clefs: `GET /items?...property[0][text]=...`.
- Lecture de la personne: `GET /items/{id}`.
- Recherche des items Expertise relies a la personne:
	`GET /items?...property=dcterms:source...&resource_template_id[]=...`.
- Creation d'un item Expertise: via `a.omk.createItem(...)`.
- Sauvegarde de l'annotation/rang:
	- `PATCH /value-annotations/{id}` si annotation existante.
	- `PATCH /items/{id}` (partiel) sinon.

## Etat actuel et limites

- Le parcours principal documente et visible dans l'UI est l'ajout d'expertise; les fonctions `updateExpertise` et `deleteExpertise` restent presentes mais non implementees.
- Une partie de la logique de `saveAll` semble encore en transition (usage de `savedRank`, index de statut, etc.).
- La cle API ne doit pas rester en dur dans un depot public.

## Securite

- Eviter de versionner des secrets dans `assets/js/authParams.js`.
- Preferer une injection cote serveur ou un mecanisme de configuration non versionne.
- Utiliser des cles API limitees en privileges.

## Depannage

- Erreur HTTP sur chargement: verifier `apiOmk`, CORS, et les cles API.
- Aucune expertise affichee: verifier la propriete `personPropExp` et le type `personTypeAllow`.
- Impossible de sauvegarder: verifier les droits d'ecriture de la cle API et l'existence des proprietes/vocabulaires cibles.

## Licence

Ajouter ici la licence du projet si necessaire.
