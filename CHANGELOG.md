# Changelog

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