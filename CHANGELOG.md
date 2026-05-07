# Changelog

## [0.2.0] — 2026-05-07

### Nouvelles fonctionnalités

**Workflow EC — lien d'auto-évaluation personnalisé (`index.html`, `authParams.js`)**
Chaque ligne de la liste Browse dispose d'un bouton 🔗 qui copie dans le presse-papier
un lien personnalisé pour l'EC concerné (`?idAuthor=XXX&idCreator=XXX&t=<token>`).
La clé API du compte EC dédié est encodée dans le paramètre `t` via un algorithme
léger (base64 → inversion → substitution URL-safe) pour éviter de l'exposer en clair
dans les logs serveur. Le compte EC dédié (`global_ECs@valo.fr`) est configuré dans
`authParams.js` (`ecKeyId`, `ecKeyCred`, `ecMail`).

**Mode EC — interface verrouillée (`index.html`)**
Quand un EC ouvre son lien personnalisé, l'app se configure entièrement depuis les
paramètres URL sans aucune saisie. La barre de recherche de personnes et le Browse
sont masqués. Le bouton Retour n'est pas affiché. La session n'est jamais sauvegardée
en `localStorage`, ce qui évite tout conflit avec la session de l'opérateur sur le
même navigateur.

**Workflow DL — lien de dispatch (`index.html`, `authParams.js`)**
L'opérateur peut envoyer au directeur de laboratoire un lien unique
(`?idLab=XXX&idCreator=YYY&t=<token>`) depuis l'encadré "Directeur(s) du laboratoire".
L'email généré contient deux liens distincts : le lien dispatch (vue liste des ECs
du labo) et le lien EC personnel du DL (pour son auto-évaluation).

**Mode DL — interface de dispatch (`index.html`)**
Quand le DL ouvre son lien, il voit directement la liste de ses ECs sans sélecteur
de laboratoire ni interface d'évaluation. Chaque ligne dispose d'un bouton 🔗 pour
copier le lien individuel de l'EC. Le booléen `dlCanEdit` dans `authParams.js`
(défaut `false`) contrôle si le DL peut modifier les expertises des ECs de son labo.
Quand `false`, les sliders sont désactivés et les boutons Ajouter/Modifier/Supprimer
ne sont pas rendus.

**Encadré "Directeur(s) du laboratoire" (`index.html`, `main.css`)**
Dès qu'un laboratoire est sélectionné dans le Browse, un encadré affiche les
directeurs du labo (via `valo:directeurs` sur l'item labo) avec pour chacun un
bouton "✉ Envoyer les liens". Si aucun directeur n'est renseigné, un message
le signale. L'encadré est automatiquement masqué en mode DL.

**Badge DL dans la liste Browse (`index.html`, `main.css`)**
Les directeurs de laboratoire sont identifiés dans la liste ECs par un badge bleu
"DL" et une bordure gauche, sans être retirés de la liste (un DL reste un EC
à part entière).

**Bouton "📋 Copier tous les liens" (`index.html`, `main.css`)**
Un bouton discret dans le header du Browse copie dans le presse-papier l'ensemble
des liens EC de la page courante, formatés en texte (`Nom : lien`).

**Filtrage par périmètre opérateur (`index.html`)**
Chaque opérateur ne voit que les ECs et laboratoires de son périmètre, déterminé
via `valo:responsableDe` sur son item Omeka S. Les DLs voient uniquement leur
propre labo (via `valo:directeurs`). Les global admins ont un accès complet.
Le rôle (opérateur ou DL) est détecté automatiquement au login.

**Anonymisation des chargées de valo en mode EC/DL (`index.html`)**
Dans les vues EC et DL, les noms des chargées de valo sont remplacés par
"Proposition du SVR". L'utilisateur connecté voit toujours son propre nom,
et les expertises ScanR gardent leur label "ScanR Top Concept".

**Suggestion de nouveau mot-clef (`index.html`, `authParams.js`)**
En mode EC, si aucun résultat exact n'est trouvé dans l'autocomplete, une option
"➕ Créer « ... »" apparaît en bas de la liste. Le nouveau concept est créé
directement dans Omeka S (`skos:Concept`) avec `dcterms:description = "suggestion"`
pour faciliter le tri par les chargées de valo. Un quota configurable
(`maxSuggestedKw: 3` dans `authParams.js`) limite le nombre de suggestions par EC.
Un indicateur "Suggestions restantes : X / 3" est affiché sous l'autocomplete.

**Masquage des paramètres techniques en mode EC/DL (`index.html`)**
La modale Paramètres masque les champs techniques (URL API, clés, propriétés)
en mode EC et DL, qui n'ont pas à les configurer.

### Correctifs

**Expertises ScanR préservées après une opération CRUD (`index.html`)**
Lors d'un create/update/delete d'expertise, la reconstruction locale du tableau
`d.expertises` supprimait par erreur les expertises ScanR. Elles sont désormais
conservées comme toutes les autres expertises tierces.


## [0.1.1] — 2026-04-29

### Nouvelles fonctionnalités

**Support multi-propriétés pour les mots-clefs (`authParams.js`, `index.html`)**
La propriété `personPropExp` accepte désormais une liste de termes séparés par des virgules
(ex. `skos:hasTopConcept,dcterms:subject`). Les concepts sont extraits depuis chacune des
propriétés listées et dédupliqués par `value_resource_id`. En cas d'ajout d'un nouveau
mot-clef, celui-ci est rattaché à la première propriété de la liste.
Le panel de configuration reflète ce changement (label, hint, placeholder mis à jour).

**Indicateur de propriété source sur chaque carte mot-clef (`index.html`, `main.css`)**
Chaque carte affiche désormais sous son titre le nom local de la propriété depuis laquelle
le concept a été extrait (ex. *· hasTopConcept*), en italique discret.

**Liste browsable des ECs par laboratoire (`index.html`, `main.css`, `omk.js`)**
Au démarrage (et en l'absence de personne chargée), l'interface affiche une liste paginée
des Enseignants-Chercheurs filtrée par laboratoire via un menu déroulant. Chaque ligne
indique le nom, l'ID et le nombre de mots-clefs déclarés. Un clic charge directement la
personne. Un bouton *Retour à la liste* est ajouté dans le bandeau personne.
Côté `omk.js` : ajout de la méthode `searchItemsPaginated()`.

**Auto-masquage des mots-clefs peu pertinents (`index.html`)**
Quand une personne a plus de 20 mots-clefs, ceux situés au-delà des 20 premiers (triés par
rang) et dont le score est strictement inférieur à 5 sont masqués par défaut. Un bandeau
indique le nombre de mots-clefs masqués avec un bouton *Tout afficher*. La barre de
recherche *Filtrer les mots-clefs* lève automatiquement ce masquage pour chercher dans
l'ensemble. Ce comportement se réinitialise à chaque changement de personne.

### Améliorations

- Divers ajustements visuels mineurs (badge source prop, bandeau auto-masquage, espacements,
  visibilité du footer sur la page d'accueil).
- **Refactoring lisibilité** (`index.html`, `omk.js`) : indentation uniformisée (4 espaces),
  sections délimitées par des bandeaux commentaires, alignement vertical des déclarations.
  Aucune logique modifiée.
  

## [0.1.0] 