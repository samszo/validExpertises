# Changelog

## [0.2.6] — 2026-06-01
 
### Correctifs
 
**Migration des expertises — `updateRessource` mal appelé (`index.html`)**
La fonction `migrateExpertises` passait l'item original en `fd` et l'item modifié en `dataOri`, ce qui avait pour effet d'envoyer l'item non modifié via PUT. Corrigé : l'item modifié est passé directement en `fd` avec `data = null`.
 
**Migration des expertises — guard temporel (`index.html`)**
La migration ignorait les valeurs entre 1 et 4 en supposant qu'elles appartenaient à la nouvelle échelle. Or des anciennes notes peuvent avoir ces valeurs sur l'échelle -100/+100. Remplacé par un guard sur `o:created` : seules les expertises créées **avant le 31/05/2026** (déploiement v0.2.4) sont converties, sans exception sur la valeur.
 
**Migration des expertises — bouton "Relancer" (`index.html`)**
Ajout d'un lien "Relancer" discret à côté du message "Migration déjà effectuée" pour pouvoir réinitialiser le flag `localStorage` et relancer la migration si nécessaire.
 
**Détection des valeurs `rank` hors plage (`index.html`)**
`formatExpertise` émet désormais un `console.warn` si une expertise a un `curation:rank` supérieur à 100 ou inférieur à -100, pour faciliter la détection de données corrompues en développement.

## [0.2.5] — 2026-05-28
 
### Nouvelles fonctionnalités
 
**Protocole de validation — languette opérateur (`index.html`, `main.css`)**
Une languette "📋 Protocole de validation" fixée sur le bord droit de l'écran (couleur primaire, texte vertical) ouvre une modale plein écran avec fond assombri affichant le diagramme `assets/img/protocole.png`. Animation d'apparition fade + slide. Clic sur l'image pour zoomer/dézoomer. Fermeture via ✕, clic sur le fond ou Échap. Visible uniquement en mode opérateur.
 
**Bandeau guide EC (`index.html`, `main.css`)**
Un bandeau explicatif apparaît au-dessus de la grille de mots-clefs en mode EC (`fromUrl`). Il décrit les 4 niveaux de notation et indique comment ajouter ou suggérer des mots-clefs manquants. Réductible via chevron ▼/▶, état persisté en `localStorage`. Le bouton "Sans avis" est masqué en mode EC.
 
**Indicateur de notation par EC dans le Browse (`index.html`, `main.css`)**
Badge ○/✓ sur chaque ligne EC indiquant si au moins une expertise existe en base pour cet EC. Calculé au démarrage via une requête unique sur `valo:Expertises_all` (toutes expertises, tous opérateurs). Mis à jour en temps réel au retour d'une fiche EC.
 
**Filtre "Évalués" dans le Browse (`index.html`, `main.css`)**
Chips "Tous / Évalués" au-dessus de la liste Browse. En mode "Évalués", charge uniquement les ECs évalués par lots de 50 IDs via `id[]=` — bien plus rapide que charger tous les ECs. Résultat mis en cache, pagination client-side.
 
### Améliorations
 
**`foaf:mbox` — propriété email unifiée (`index.html`)**
`ecItemHTML` utilisait `foaf:mailbox` au lieu de `foaf:mbox`. Corrigé pour être cohérent avec la propriété effectivement utilisée en base.
 
**Labo de l'EC dans le bandeau (`index.html`, `main.css`)**
Les laboratoires de l'EC (via `dcterms:isPartOf`) sont affichés sous son nom dans le bandeau de fiche, séparés par `·`.
 
**Navigation retour sans rechargement (`index.html`)**
`loadPerson` pousse un état dans l'historique navigateur. Le bouton natif "retour" du navigateur revient au Browse sans recharger la page. Bouton "← Retour à la liste" rendu plus visible (fond semi-transparent blanc sur bandeau).
 
**"DL" → "DU" (`index.html`)**
Toutes les occurrences visibles (badge Browse, tooltip, titre `dlZone`, libellé de rôle) remplacées par "DU" (Directeur·rice d'Unité). Variables JS internes inchangées.
 
**Filtres Browse plus visibles (`index.html`, `main.css`)**
Label "Filtrer :" devant les chips. Input de recherche repositionné après les chips. Gap entre le select labo et les chips filtre. Classes CSS `active-pos`/`active-neg` renommées `active-high`/`active-low`.
 
**Redirections Wikidata sur les mots-clefs (`index.html`, `main.css`)**
Lien `QID ↗` discret après le titre de chaque carte mot-clef si `dcterms:isReferencedBy` contient une URL Wikidata.
 
**Badge "Suggestion" remplace "Nouvelle annotation" (`index.html`, `main.css`)**
Badge violet "Suggestion" sur les mots-clefs `dcterms:description = "suggestion"`. Le texte "Nouvelle annotation" est supprimé.
 
### Correctifs
 
**`evaluatedEcIds` — requête correcte (`index.html`)**
La détection des ECs évalués utilisait `getPropId` qui retournait un ID incorrect. Remplacé par `getAllItems` + filtre JS sur `dcterms:source`, même pattern que `migrateExpertises`.
 
**`filterBrowseItems` en mode labo (`index.html`)**
Le filtre s'applique maintenant correctement en mode labo (pagination serveur) et en mode "tous les ECs" (pagination client).

## [0.2.4] — 2026-05-27
 
### Nouvelles fonctionnalités
 
**Système de notation 1-4 — remplacement du slider (`index.html`, `main.css`)**
Le slider −100/+100 est remplacé par un sélecteur à 4 niveaux explicites : Non pertinent (1) / Expertise marginale (2) / Expertise reconnue (3) / Expertise centrale (4), plus un lien discret "Sans avis" (0). Chaque bouton est coloré en permanence (rouge → ambre → vert → teal). Le niveau actif est signalé par un `outline`. Les dots colorés dans le titre de chaque carte reflètent le niveau sélectionné. La bordure gauche de la carte suit la couleur du niveau noté.
 
**Indicateur ScanR relatif par quartiles (`index.html`)**
Au chargement d'un profil EC, `computeScanrIndicator()` calcule Q1/Q3 sur l'ensemble des notes ScanR du profil (seuil minimum : 4 mots-clefs). Chaque carte affiche un indicateur en lecture seule : "Très présent / Présent / Peu présent dans les publications". La bordure gauche de la carte utilise la couleur de l'indicateur ScanR tant qu'aucun niveau n'est sélectionné.
 
**Migration des expertises v0.2.3 → v0.2.4 (`index.html`, `main.css`)**
Modale de migration accessible depuis les Paramètres avancés (admins uniquement). Affiche la table de conversion, un avertissement de sauvegarde, et lance la conversion des 168 expertises existantes (−100/−51 → 1, −50/0 → 2, 1/50 → 3, 51/100 → 4) via PUT. Rapport détaillé avec liste des erreurs par item. Flag `localStorage` après succès pour éviter une double migration.
 
**Redirections Wikidata sur les mots-clefs (`index.html`, `main.css`)**
Si un concept a un `dcterms:isReferencedBy` pointant vers Wikidata, un lien `QID ↗` discret apparaît à droite du titre de la carte, s'ouvrant dans un nouvel onglet.
 
**Labo de l'EC dans le bandeau (`index.html`, `main.css`)**
Les laboratoires de l'EC (via `dcterms:isPartOf`) sont affichés sous son nom dans le bandeau de fiche, séparés par `·`.
 
**Navigation retour sans rechargement (`index.html`)**
`loadPerson` pousse un état dans l'historique navigateur (`history.pushState`). Le bouton natif "retour" du navigateur revient désormais au Browse sans recharger la page. `showBrowse` remplace l'état courant via `history.replaceState`.
 
**Badge "Suggestion" sur les mots-clefs suggérés (`index.html`, `main.css`)**
Les mots-clefs marqués `dcterms:description = "suggestion"` affichent un badge violet "Suggestion" sur leur carte, qu'ils soient notés ou non. Remplace le texte "Nouvelle annotation" supprimé.
 
**Moyenne des notes SVR en mode EC (`index.html`)**
En mode EC/DL, au lieu d'afficher N lignes "Proposition du SVR", les notes de plusieurs opérateurs sont regroupées en une seule ligne affichant le niveau moyen (`Math.round`). Si une seule note : affichée telle quelle. Si plusieurs : "Proposition du SVR (moyenne de N évaluations)".
 
### Améliorations
 
**"DL" → "DU" (`index.html`)**
Toutes les occurrences visibles de "DL" (badge Browse, tooltip, titre `dlZone`, libellé de rôle) remplacées par "DU" (Directeur·rice d'Unité). Variables JS internes inchangées.
 
**Bouton "Retour" plus visible (`index.html`, `main.css`)**
Le bouton "← Retour à la liste" est désormais rendu en fond semi-transparent blanc sur le bandeau coloré, plus lisible et identifiable.
 
**Filtres plus visibles (`index.html`, `main.css`)**
Label "Filtrer :" ajouté devant les chips. L'input de recherche texte est repositionné après les chips. Classes CSS `active-pos`/`active-neg` renommées `active-high`/`active-low`.
 
**Suppression du cumul des notes (`index.html`)**
`c.rank` reflète uniquement la note de l'opérateur connecté (plus de somme). `formatKeyword` et `setRankForKeyword` mis à jour en conséquence. Filtre "Niveaux 3-4" (ex-Positifs) et "Niveaux 1-2" (ex-Négatifs) adaptés. Seuil d'auto-masquage : `rank <= 2`.
 
**Constantes de niveau au niveau module (`index.html`)**
`LEVEL_NAMES`, `LEVEL_COLORS`, `LEVEL_BORDER` définis une seule fois au niveau module. `renderExpertise` n'en redéclare plus localement.
 
### Correctifs
 
**`formatExpertise` — guard trop agressif (`index.html`)**
Le guard `if (!e[cfg.rankProp]) return` empêchait l'assignation de `creatorId`, rendant `hasExpert` toujours `false` → "Nouvelle annotation" sur toutes les cartes. Le guard protège maintenant uniquement la lecture du rang ; `creatorId` est toujours assigné.
 
**Gestion des erreurs CRUD (`index.html`)**
`createExpertise`, `updateExpertise` et `deleteExpertise` sont maintenant enveloppés dans un try/catch avec toast d'erreur visible en cas d'échec API.

## [0.2.3] — 2026-05-13
 
### Nouvelles fonctionnalités
 
**Liste globale des ECs sans laboratoire sélectionné (`index.html`, `main.css`)**
Nouvelle option dans les Paramètres : "Afficher tous les ECs si aucun laboratoire sélectionné". Quand activée, le Browse charge l'ensemble des ECs du périmètre au lieu d'afficher un écran vide. Le périmètre opérateur est respecté via les filtres `dcterms:isPartOf`. La pagination fonctionne normalement.
 
**Compteur d'ECs sans mot-clef par laboratoire (`index.html`, `main.css`)**
Quand un laboratoire est sélectionné dans le Browse, un bandeau discret affiche le nombre total d'ECs et le nombre sans mot-clef (`"X ECs — Y sans mot-clef (Z%)"`) en rouge si non nul, en vert sinon. Ce compteur est calculé via `getAllItems` sur l'ensemble du labo (pas uniquement la page courante) et est désactivé en mode "tous les ECs" pour éviter un freeze au chargement initial.
 
**Bouton ✉ — envoi d'email individuel aux ECs depuis la vue DL (`index.html`)**
En mode DL, chaque ligne EC dans le Browse dispose d'un bouton ✉ qui génère un `mailto` pré-rempli (corps, objet, lien de validation personnel). Le bouton est grisé si `foaf:mbox` est absent sur l'item EC. Le template du mail EC est distinct du mail DL : plus direct, sans mention du rôle de direction.
 
**Deadline configurable (`authParams.js`, `index.html`)**
Nouveau champ `deadline` dans `authParams.js` et dans les Paramètres (format JJ/MM/AA). Quand renseignée, la date limite est incluse dans les deux templates email (DL et EC). Laisser vide pour ne pas l'afficher.
 
### Améliorations
 
**Réécriture des templates email (`index.html`)**
`sendDLEmail()` entièrement réécrit : mention de ScanR (plateforme nationale), du préfiltrage SVR, ton plus naturel distinguant clairement le rôle DL et le lien EC personnel. Signature dynamique avec le nom de l'utilisateur connecté (`a.user['o:name']`). Objet du mail mis à jour. `sendEcEmail()` est une nouvelle fonction dédiée au mail DL → EC.
 
**Indicateurs de chargement (`index.html`, `main.css`)**
Spinner `⠋ Connexion en cours…` sur le bouton "Enregistrer" de la modale Paramètres pendant `saveSettings()`. Spinner inline `#browseLoading` avec `setTimeout` sur `loadBrowsePersons` et `loadBrowseAllPersons`. Message contextuel dans `#loadingState` : "Connexion en cours…" au lancement, "Chargement de [Nom]…" au chargement d'une fiche EC. Indicateur `• Recherche…` sur les deux champs d'autocomplete (recherche personne et ajout mot-clef) pendant le debounce.
 
**Modale Paramètres restructurée (`index.html`, `main.css`)**
Les 9 champs sont regroupés en deux sections ("Connexion" et "Configuration") avec grille 2 colonnes pour les paires clés/valeurs. Les 3 champs rarement modifiés (type personne, propriétés expertises, propriété rang) sont déplacés dans un accordéon "Paramètres avancés" fermé par défaut. La modale ne nécessite plus de scroll dans le cas d'usage courant.
 
### Correctifs
 
**Pagination en mode "tous les ECs" (`index.html`)**
Les boutons Précédent/Suivant appelaient toujours `loadBrowsePersons(browseLabId)` même quand `browseLabId` était nul, provoquant une erreur silencieuse. Ils aiguillent désormais vers `loadBrowseAllPersons` ou `loadBrowsePersons` selon le contexte.
 
**Paramètre `items` mort dans `sendDLEmail` (`index.html`)**
Le premier paramètre `items` de `sendDLEmail` n'était jamais utilisé dans le corps de la fonction. Supprimé de la signature et du call site.
 
**`foaf:mbox` — propriété email unifiée (`index.html`)**
`ecItemHTML` utilisait `foaf:mailbox` au lieu de `foaf:mbox` pour récupérer l'email des ECs. Corrigé pour être cohérent avec la propriété effectivement utilisée en base et avec la récupération de l'email des DLs.
 

## [0.2.2] — 2026-05-12
 
### Améliorations
 
**Navigation inter-outils (`index.html`, `main.css`)**
Le titre de l'application dans le header est désormais cliquable et ouvre un menu déroulant listant les outils disponibles (`validExpertises`, `cleanKeywords`). L'outil courant est mis en évidence et non-cliquable. Le menu affiche également le numéro de version en bas. L'ancien `<select>` natif est remplacé par un dropdown custom cohérent visuellement avec le reste de l'interface.

## [0.2.1] — 2026-05-11

### Nouvelles fonctionnalités

**Groupement des ECs par sous-équipe dans le Browse (`index.html`, `main.css`)**
Quand un laboratoire a des sous-équipes (`valo:Axe`) renseignées via `valo:hasAxe`, les ECs sont regroupés par axe dans des accordéons natifs HTML, ouverts par défaut. Chaque accordéon affiche le nom de l'axe et le nombre d'ECs. Les ECs sans axe apparaissent dans un groupe "Sans équipe". Si aucun axe n'est renseigné, la liste plate habituelle s'affiche — le comportement est rétrocompatible.

NOTE : Aucun lien n'est fait actuellement dans la BDD pour une raison inconnue. La fonctionnalité n'est donc pas constatable tant que les liens n'ont pas été rétablis entre les deux classes via `valo:hasAxe`

**Interface "Ajouter un mot-clef" toujours accessible (`index.html`)**
Quand un EC n'a aucun mot-clef, l'input d'ajout reste désormais visible. Auparavant, `showEmpty()` masquait `#kwSection` entièrement, rendant impossible l'ajout du premier mot-clef. La grille et les filtres sont masqués, mais l'input d'ajout et la barre de sauvegarde restent accessibles.

**Suggestion de mot-clef — aucun résultat (`index.html`)**
Quand l'autocomplete ne trouve aucun concept existant, la dropdown reste ouverte avec un message "Aucun mot-clef correspondant" et l'option `➕ Créer « ... »`. Un quota configurable (`maxSuggestedKw`) s'applique uniquement en mode EC. Les concepts créés sont flagués `dcterms:description = "suggestion"` pour faciliter la revue. Auparavant la dropdown se fermait sans afficher l'option de création, rendant la fonctionnalité inaccessible sans résultats existants. La navigation clavier (Entrée) déclenche aussi `suggestNewKeyword` correctement.

### Correctifs

**Expertises ScanR dupliquées après CRUD (`index.html`)**
Après un create/update/delete d'expertise, `formatKeyword` était rappelé sur le même objet et dupliquait l'annotation ScanR et le slot placeholder. Le filtre `c.expertises.filter(e => e['o:id'] && e !== c['@annotation'])` nettoie les entrées locales avant chaque appel, éliminant les doublons.

**Compteur d'expertises inexact (`index.html`)**
Le compteur "X Expertise(s) existante(s)" incluait le placeholder local dans son décompte, affichant "1 Expertise(s) existante(s)" même sans expertise réelle. Il filtre maintenant sur `e['o:id']` pour ne compter que les items Omeka S réels.

**Double toast lors d'une suggestion de mot-clef (`index.html`)**
`addKeyword` et `suggestNewKeyword` affichaient chacun un toast de confirmation pour la même action. `addKeyword` accepte maintenant un paramètre `silent` (défaut `false`) — passé à `true` depuis `suggestNewKeyword` pour n'afficher qu'un seul toast.

**Corps de l'email au DL enrichi (`index.html`)**
L'email généré par "✉ Envoyer les liens" incluait uniquement les liens sans contexte. Le corps contient maintenant une explication de la démarche, la distinction claire entre le lien de dispatch et le lien d'auto-évaluation. Le contenu du mail peut être modifié dans `index.html`, au commentaire `── Corps de l'email — modifiable ici si besoin de personnaliser le message ──`.

## [0.2.0] — 2026-05-07

### Nouvelles fonctionnalités

**Workflow EC — lien d'auto-évaluation personnalisé (`index.html`, `authParams.js`)**
Chaque ligne de la liste Browse dispose d'un bouton 🔗 qui copie dans le presse-papier un lien personnalisé pour l'EC concerné (`?idAuthor=XXX&idCreator=XXX&t=<token>`). La clé API du compte EC dédié est encodée dans le paramètre `t` via un algorithme léger (base64 → inversion → substitution URL-safe) pour éviter de l'exposer en clair dans les logs serveur. Le compte EC dédié (`global_ECs@valo.fr`) est configuré dans `authParams.js` (`ecKeyId`, `ecKeyCred`, `ecMail`).

**Mode EC — interface verrouillée (`index.html`)**
Quand un EC ouvre son lien personnalisé, l'app se configure entièrement depuis les paramètres URL sans aucune saisie. La barre de recherche de personnes et le Browse sont masqués. Le bouton Retour n'est pas affiché. La session n'est jamais sauvegardée en `localStorage`, ce qui évite tout conflit avec la session de l'opérateur sur le même navigateur.

**Workflow DL — lien de dispatch (`index.html`, `authParams.js`)**
L'opérateur peut envoyer au directeur de laboratoire un lien unique (`?idLab=XXX&idCreator=YYY&t=<token>`) depuis l'encadré "Directeur(s) du laboratoire". L'email généré contient deux liens distincts : le lien dispatch (vue liste des ECs du labo) et le lien EC personnel du DL (pour son auto-évaluation).

**Mode DL — interface de dispatch (`index.html`)**
Quand le DL ouvre son lien, il voit directement la liste de ses ECs sans sélecteur de laboratoire ni interface d'évaluation. Chaque ligne dispose d'un bouton "🔗" pour copier le lien individuel de l'EC. Le booléen `dlCanEdit` dans `authParams.js`
(défaut `false`) contrôle si le DL peut modifier les expertises des ECs de son labo. Quand `false`, les sliders sont désactivés et les boutons Ajouter/Modifier/Supprimer ne sont pas rendus.

**Encadré "Directeur(s) du laboratoire" (`index.html`, `main.css`)**
Dès qu'un laboratoire est sélectionné dans le Browse, un encadré affiche les directeurs du labo (via `valo:directeurs` sur l'item labo) avec pour chacun un bouton "✉ Envoyer les liens". Si aucun directeur n'est renseigné, un message le signale. L'encadré est automatiquement masqué en mode DL.

**Badge DL dans la liste Browse (`index.html`, `main.css`)**
Les directeurs de laboratoire sont identifiés dans la liste ECs par un badge bleu "DL" et une bordure gauche, sans être retirés de la liste (un DL reste un EC à part entière).

**Bouton "📋 Copier tous les liens" (`index.html`, `main.css`)**
Un bouton discret dans le header du Browse copie dans le presse-papier l'ensemble des liens EC de la page courante, formatés en texte (`Nom : lien`).

**Filtrage par périmètre opérateur (`index.html`)**
Chaque opérateur ne voit que les ECs et laboratoires de son périmètre, déterminé via `valo:responsableDe` sur son item Omeka S. Les DLs voient uniquement leur propre labo (via `valo:directeurs`). Les global admins ont un accès complet. Le rôle (opérateur ou DL) est détecté automatiquement au login.

**Anonymisation des chargées de valo en mode EC/DL (`index.html`)**
Dans les vues EC et DL, les noms des chargées de valo sont remplacés par "Proposition du SVR". L'utilisateur connecté voit toujours son propre nom, et les expertises ScanR gardent leur label "ScanR Top Concept".

**Suggestion de nouveau mot-clef (`index.html`, `authParams.js`)**
En mode EC, si aucun résultat exact n'est trouvé dans l'autocomplete, une option "➕ Créer « ... »" apparaît en bas de la liste. Le nouveau concept est créé directement dans Omeka S (`skos:Concept`) avec `dcterms:description = "suggestion"` pour faciliter le tri par les chargées de valo. Un quota configurable (`maxSuggestedKw: 3` dans `authParams.js`) limite le nombre de suggestions par EC. Un indicateur "Suggestions restantes : X / 3" est affiché sous l'autocomplete.

**Masquage des paramètres techniques en mode EC/DL (`index.html`)**
La modale Paramètres masque les champs techniques (URL API, clés, propriétés) en mode EC et DL, qui n'ont pas à les configurer.

### Correctifs

**Expertises ScanR préservées après une opération CRUD (`index.html`)**
Lors d'un create/update/delete d'expertise, la reconstruction locale du tableau `d.expertises` supprimait par erreur les expertises ScanR. Elles sont désormais conservées comme toutes les autres expertises tierces.


## [0.1.1] — 2026-04-29

### Nouvelles fonctionnalités

**Support multi-propriétés pour les mots-clefs (`authParams.js`, `index.html`)**
La propriété `personPropExp` accepte désormais une liste de termes séparés par des virgules (ex. `skos:hasTopConcept,dcterms:subject`). Les concepts sont extraits depuis chacune des propriétés listées et dédupliqués par `value_resource_id`. En cas d'ajout d'un nouveau mot-clef, celui-ci est rattaché à la première propriété de la liste. Le panel de configuration reflète ce changement (label, hint, placeholder mis à jour).

**Indicateur de propriété source sur chaque carte mot-clef (`index.html`, `main.css`)**
Chaque carte affiche désormais sous son titre le nom local de la propriété depuis laquelle le concept a été extrait (ex. *· hasTopConcept*), en italique discret.

**Liste browsable des ECs par laboratoire (`index.html`, `main.css`, `omk.js`)**
Au démarrage (et en l'absence de personne chargée), l'interface affiche une liste paginée des Enseignants-Chercheurs filtrée par laboratoire via un menu déroulant. Chaque ligne indique le nom, l'ID et le nombre de mots-clefs déclarés. Un clic charge directement la personne. Un bouton *Retour à la liste* est ajouté dans le bandeau personne. Côté `omk.js` : ajout de la méthode `searchItemsPaginated()`.

**Auto-masquage des mots-clefs peu pertinents (`index.html`)**
Quand une personne a plus de 20 mots-clefs, ceux situés au-delà des 20 premiers (triés par rang) et dont le score est strictement inférieur à 5 sont masqués par défaut. Un bandeau indique le nombre de mots-clefs masqués avec un bouton *Tout afficher*. La barre de recherche *Filtrer les mots-clefs* lève automatiquement ce masquage pour chercher dans l'ensemble. Ce comportement se réinitialise à chaque changement de personne.

### Améliorations

- Divers ajustements visuels mineurs (badge source prop, bandeau auto-masquage, espacements, visibilité du footer sur la page d'accueil).
- **Refactoring lisibilité** (`index.html`, `omk.js`) : indentation uniformisée (4 espaces),
  sections délimitées par des bandeaux commentaires, alignement vertical des déclarations. Aucune logique modifiée.
  

## [0.1.0] 