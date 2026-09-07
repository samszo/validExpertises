# Changelog

## [0.2.12] — 2026-09-07 — correctif urgent

### Correctifs critiques

**Expertises tronquées à 25 par l'API — notes invisibles et doublons (`index.html`)**
La requête chargeant les expertises d'une personne dans `loadPerson` n'indiquait
pas de `per_page`. L'API Omeka S plafonne alors la réponse à 25 résultats. Au-delà
de ce seuil, une partie des expertises n'était plus chargée : l'application ne
voyait plus la note déjà saisie par l'EC sur un mot-clef, affichait « Ajouter » au
lieu de « Modifier », et chaque validation créait une expertise supplémentaire au
lieu de mettre à jour l'existante. Le bug n'apparaissait qu'une fois le seuil
franchi, ce qui explique qu'il ait surgi progressivement à mesure que les
notations s'accumulaient. La requête utilise désormais `getAllItems`, qui pagine
jusqu'à épuisement et garantit un chargement complet quel que soit le volume.

**Expertise créée puis perdue en mémoire (`index.html`)**
`showNewExpertise` ne réinsérait l'expertise créée que si elle trouvait une entrée
existante du même créateur à remplacer. En l'absence de correspondance, l'objet
retourné par l'API était silencieusement écarté — l'interface restait sur
« Ajouter » et le clic suivant produisait un doublon. La fonction garantit
maintenant que l'expertise créée est toujours conservée, et la comparaison des
identifiants de créateur se fait en chaîne de caractères (l'identifiant est une
chaîne en mode EC, un nombre côté API).

**Échecs d'écriture silencieux (`index.html`)**
`postData` (dans `omk.js`) renvoie le corps JSON sans vérifier le statut HTTP :
une erreur de l'API était donc traitée comme un succès. En création, l'expertise
n'était pas enregistrée sans que l'opérateur en soit averti, et il recliquait ;
en modification, l'interface continuait d'afficher l'ancienne note. Les réponses
de création et de modification sont désormais validées (présence de `o:id`) et le
message d'erreur renvoyé par Omeka S est remonté à l'opérateur. `updateExpertise`
et `deleteExpertise` vérifient également que l'expertise ciblée existe réellement
avant d'agir.

**Doublons recopiés en mémoire (`index.html`)**
Sur une fiche déjà affectée par le bug de pagination — donc contenant plusieurs
expertises du même créateur pour un mot-clef — `showNewExpertise` insérait
l'expertise mise à jour autant de fois qu'il trouvait de correspondances. Elle
n'est désormais insérée qu'une seule fois, ce qui absorbe au passage les doublons
présents en base lors de la première modification du mot-clef concerné.

**Détail des notifications jamais affiché (`index.html`, `main.css`)**
La fonction `toast` n'acceptait que deux arguments : le troisième, utilisé pour
transmettre le message d'erreur de l'API ou une marche à suivre, était
silencieusement ignoré — y compris pour l'avertissement sur les items Expertise
mal formés. `toast` accepte désormais une ligne de détail, affichée sous le
message principal. Les notifications d'erreur restent affichées 8 secondes au
lieu de 3,2 : elles portent une consigne à lire. Les messages d'échec d'écriture
(création, modification, suppression, suggestion de mot-clef) indiquent
maintenant de contacter la chargée de valorisation si le problème persiste.

**Chargement des expertises : échec silencieux (`index.html`)**
Si la requête chargeant les expertises échouait (réseau, timeout), l'erreur était
seulement consignée en console et la fiche s'affichait avec tous les mots-clefs
apparemment non notés — conduisant l'opérateur à recréer des expertises en double.
L'échec interrompt désormais le chargement et affiche une erreur explicite, plutôt
que de présenter une fiche trompeuse.

**Écritures non vérifiées dans `addKeyword` (`index.html`)**
Le rattachement du mot-clef à la fiche et la création de l'expertise associée
n'étaient pas contrôlés. Si le rattachement échouait pendant que la création
réussissait, une expertise orpheline était produite — liée à un mot-clef absent
de la fiche, donc invisible dans l'interface. Les deux écritures sont désormais
validées, et l'expertise n'est créée qu'une fois le rattachement confirmé.

**Signalement des enregistrements anormaux (`index.html`)**
Si une expertise est créée mais revient sans créateur exploitable, l'application
affiche désormais un avertissement explicite invitant à recharger la page avant
de renoter, au lieu de reproposer « Ajouter » — ce qui générait des doublons en
série sans que l'opérateur en soit informé.

**Quota de suggestions tronqué (`index.html`)**
La requête comptant les concepts suggérés par un EC subissait la même troncature
à 25 résultats. Ajout de `per_page=1000` pour un décompte exact.

### Note

Les autocomplétions (recherche de personne et de mot-clef) restent volontairement
limitées à 25 résultats : il s'agit d'un plafond d'affichage pour le menu
déroulant, pas d'une troncature de données.

## [0.2.11] — 2026-08-24

### Améliorations

**Envoi d'emails individuels aux ECs par les opérateurs (`index.html`)**
Le bouton d'envoi d'email présent sur chaque ligne de la liste Browse était réservé aux directeur·rice·s d'unité (condition `window._dlMode`). Il est désormais accessible à tous les opérateurs, dans l'ensemble du Browse — liste par laboratoire comme liste globale. Il reste masqué en mode EC (`fromUrl`), où il n'aurait pas de sens. La fonction `sendEcEmail` était déjà indépendante du rôle et n'a pas eu besoin d'être modifiée ; le bouton reste désactivé lorsque l'adresse `foaf:mbox` de l'EC n'est pas renseignée.

**Rendu de l'icône d'enveloppe (`index.html`)**
L'émoji `✉` (U+2709 seul) s'affichait mal sous Linux, rendu en glyphe monochrome dégradé. Remplacé par `✉️` (U+2709 + variation-selector U+FE0F) sur le bouton d'envoi EC et sur le bouton « Envoyer les liens » du DU, ce qui force le rendu emoji couleur. Aucun impact sur Windows, Android ou iOS.

## [0.2.10] — 2026-06-13

### Correctifs critiques

**Ajout de mot-clef ne persistait pas en base (`index.html`)**
`addKeyword` utilisait `updateRessource` en mode `data` pour rattacher le concept à l'item EC. Ce mode appelle en interne `getResource` (XHR synchrone, sans credentials), dont le comportement est déprécié — le PUT pouvait partir avec un body invalide, silencieusement ignoré par Omeka S. Le mot-clef apparaissait temporairement (l'item Expertise était créé) mais disparaissait au rechargement pour tout le monde. Corrigé : `addKeyword` récupère désormais l'item EC brut via `getItem` (async, avec credentials), ajoute le concept directement au tableau, et envoie l'item complet via `fd`.

**Comptage erroné des items Expertise mal formés (`index.html`)**
Le toast signalant les items sans `valo:expertise` répétait le même ID une fois par mot-clef affiché (ex: 13 occurrences du même ID pour 13 mots-clefs). La détection est maintenant effectuée une seule fois avant la boucle d'affichage.

### Nouvelles fonctionnalités

**Statistiques de validation (`index.html`, `main.css`)**
Nouvelle section dans les Paramètres avancés (admin uniquement). Affiche au global et par laboratoire le nombre d'ECs ayant fait leur auto-évaluation, ainsi que le nombre d'ECs distincts validés par chaque opérateur avec détail par labo. Présentation avec carte globale, barres de progression et menus dépliants. Lecture seule, déclenchement manuel. Distinction auto-évaluation/opérateur basée sur `dcterms:creator == dcterms:source`.

**Template email DU mis à jour (`index.html`)**
Nouveau texte fourni par le SVR — formule d'ouverture inclusive généraliste, description du contexte ScanR et du préfiltrage SVR, deux liens distincts (suivi labo + validation personnelle de l'EC).


 