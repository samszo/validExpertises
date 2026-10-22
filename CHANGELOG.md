# Changelog

## [0.3.6] — 2026-10-22

### Modifié

**Menu des outils harmonisé entre les trois applications (`index.html`, `main.css`)**
Le survol passe du gris au rose pâle de la charte, l'entrée courante gardant un
gris neutre. Les deux fonds étaient jusqu'ici presque identiques — `#f3f4f6`
contre `#EFE8EB` — et ne se distinguaient que parce que rien ne prend la mise au
point à l'ouverture ; ce n'est pas le cas dans ValoVizu, où la confusion s'était
effectivement produite. Les derniers gris écrits en dur du menu — en-tête,
pastille, ligne de version, filet — passent aux jetons.

### Ajouté

**Entrée « Omeka S » dans le menu (`index.html`, `main.css`)**
L'instance figure désormais au bas du menu, détachée par un filet : elle n'est
pas un outil de la suite, elle est ce que la suite interroge. Son adresse est
déduite de celle de l'API plutôt qu'écrite dans le balisage, car elle change
d'un déploiement à l'autre. Elle part dans un nouvel onglet, là où les outils
voisins remplacent la page — on va y vérifier une fiche, pas quitter l'outil.
Même comportement et même présentation que dans ValoVizu.
L'entrée est réservée aux opérateurs. La classe `app-dropdown-operator` ne
suffisait pas ici : le masquage par `fromUrl` s'exécute bien avant l'insertion,
la classe `hidden` est donc posée à la création.


## [0.3.5] — 2026-10-20

### Modifié

**Bandeau aligné sur ceux de ValoVizu et de cleanKeywords (`index.html`, `main.css`)**
Le bandeau passe du Maroon au blanc, avec un filet clair en bas. Celui de
ValoVizu est clair, pas coloré : `--barre-fond` y habille la barre de légende,
en dessous. La barre sombre était ce qui empêchait les trois outils de se lire
comme une même suite.

La signature de l'établissement surmonte désormais le titre, dont la taille
passe de 1,1 rem à 25 px — la valeur des deux autres outils. Le contenu du
bandeau est centré sur 960 px pour s'aligner sur le `.container` en dessous,
obtenu par le rembourrage plutôt que par un élément d'habillage. Le bloc de
droite regroupe « Expert : … » et le bouton Paramètres, si bien que les deux
en-têtes ont maintenant la même structure à trois blocs.

**Le bouton des outils est détaché du titre et ancré au bord gauche**, à 24 px
du bord et 18 px du haut. Mesuré : il tombe au même pixel dans les trois
applications, de 390 à 1920 px de large. Chacune centrant son en-tête sur la
largeur de son propre contenu — 960 px ici, 1440 chez ValoVizu — un bouton
solidaire du bloc centré se déplaçait de près de deux cents pixels d'un outil à
l'autre. L'ancrage se fait par le haut et non par le centre, les bandeaux
n'ayant pas la même hauteur.

**Conséquence à connaître : le titre n'ouvre plus le menu.** Seul le bouton le
fait, comme chez ValoVizu. Le script est inchangé, l'identifiant `appSwitcher`
ayant suivi le bouton.

**Note technique** — deux choses ne sont volontairement pas alignées sur
ValoVizu. Le bandeau reste collant, utile quand un EC fait défiler une longue
liste de mots-clefs, et il garde une ombre, un filet seul ne détachant pas assez
un en-tête sous lequel le contenu défile. Le filigrane « & » n'est pas repris :
il demande `overflow: hidden`, ce qui écrêterait le panneau du menu.

### Corrigé

**Débordement du bandeau sur écran étroit (`main.css`)**
Le titre agrandi et la signature ajoutée mettaient le bandeau à l'étroit. Mêmes
seuils que dans cleanKeywords : retour à la ligne autorisé sous 760 px,
signature effacée sous 520 px. Plus aucun débordement de 1920 à 390 px.

**Note technique** — `.btn-ghost` reste blanc globalement : il habille aussi
« Retour à la liste », posé sur le bandeau EC qui demeure en Maroon. Seul celui
du bandeau est redéfini. Une bascule globale aurait rendu le bouton de retour
illisible sur son fond sombre.

## [0.3.4] — 2026-10-20

### Modifié

**Palette alignée sur la charte graphique de l'Université Paris 8 (`main.css`)**
Les douze jetons de couleur prennent les valeurs de la charte, accordées à
celles de ValoVizu. Les noms de jetons sont conservés : les 175 appels à `var()`
de la feuille restent intacts, et aucune règle de mise en page n'est touchée.

Raspberry `#AE164E` porte l'interaction, Poppy `#E30F1B` l'alerte, Maroon
`#75151F` le bandeau. Le vert `#1B6B45` et l'ambre `#92400E` ne viennent pas de
la charte, qui n'en contient aucun : ils portent l'opposition « fait » / « à
faire » et sont repris de cleanKeywords, où ils étaient déjà écrits en dur.

**Correction d'accessibilité, non recherchée au départ.** Quatre des cinq
couleurs portant du texte blanc étaient sous le seuil AA : `--accent` à 3.15,
`--danger` à 3.82, `--success` à 2.10, `--warn` à 2.19. Les nouvelles valeurs
sont à 6.94, 4.83, 6.49 et 7.09. Le texte secondaire, jeton le plus employé de
la feuille avec 41 usages, passe de 3.22 à 7.03.

Huit jetons de ValoVizu encore inemployés sont posés d'avance — ils ne changent
rien à l'affichage et feront d'un éventuel thème sombre un bloc à ajouter plutôt
qu'un chantier.

### Corrigé

**Trois couleurs de l'ancienne palette survivaient hors des jetons (`main.css`)**
Elles auraient produit un reskin manifestement inachevé :

- le bandeau d'un·e enseignant·e-chercheur·se mêlait son jeton à un bleu acier
  écrit en dur, ce qui donnait un dégradé rouge vers bleu ;
- l'entrée du menu correspondant à l'application courante portait l'ancien
  marine — sur le menu, c'est-à-dire précisément là où les trois outils doivent
  se ressembler ;
- le lien Wikidata des cartes de mots-clefs restait bleu, sur toutes les cartes
  portant un identifiant.

Les trois suivent désormais les jetons.

**La feuille de style est appelée en `?v=0.3.4`**, puisqu'elle change.

## [0.3.3] — 2026-10-20

### Ajouté

**ValoVizu dans le menu des outils (`index.html`)**
Le tableau de bord ValoVizu rejoint le menu de navigation, qui présente
désormais les trois outils de la suite. Le lien est relatif (`../valoVizu/`) et
suppose les trois applications voisines sous `outils/`, comme les liens
existants. Le nom du dossier respecte la casse — `valoVizu`, et non `valovizu` :
sous Apache un lien mal capitalisé renvoie un 404 sans autre signe.

L'entrée est masquée en session EC et en session DU : `fromUrl` est vrai dans
les deux cas, un lien de directeur·rice d'unité portant lui aussi `idCreator`
et `t`. Elle porte la classe `app-dropdown-operator`, qu'il suffira d'ajouter
à un outil ultérieur pour qu'il suive la même règle.

**Attention — ceci relève de l'hygiène d'interface, non du contrôle d'accès.**
Les données de l'instance étant publiques en lecture, ValoVizu reste atteignable
par son adresse quand bien même aucun menu ne la montre. Une restriction réelle
se règle côté serveur, par authentification ou filtrage sur le dossier `valo/`.

La feuille de style n'ayant pas changé, son paramètre de version reste à
`?v=0.3.2` — il ne suit que les modifications de `main.css`.

## [0.3.2] — 2026-09-30

### Nouvelles fonctionnalités

**Copie du message dans le presse-papiers (`index.html`, `main.css`)**
Un bouton 📋 accompagne désormais chaque enveloppe, dans la liste des ECs comme
dans l'encadré des directeur·rice·s d'unité. Il copie le message complet —
destinataire, objet, corps — prêt à coller dans n'importe quel webmail.

C'est la seule voie qui ne dépende d'aucune configuration de poste. L'ouverture
d'un `mailto:` passe par le gestionnaire de protocole du système d'exploitation
puis par la session du webmail : deux maillons hors de portée de l'application,
et à l'origine des dysfonctionnements constatés sur le terrain (ouverture forcée
d'un navigateur qui n'est pas celui en cours, erreur 502 de Zimbra lorsque la
session n'y est pas ouverte). Le message reste accessible même quand cette chaîne
échoue.

Les deux formes — lien `mailto:` et texte copié — sont produites à partir d'une
source unique. Modifier le texte d'un message le modifie partout.

### Correctifs

**L'envoi d'un email remplaçait l'application (`index.html`, `main.css`)**
Les enveloppes étaient des `<button>` déclenchant l'ouverture d'un `mailto:` par
un lien fabriqué à la volée, cliqué puis immédiatement retiré du DOM. Ce
contournement ne garantissait pas l'ouverture dans un nouvel onglet : selon le
navigateur et le gestionnaire de protocole, la fiche en cours était toujours
remplacée par le webmail.

Les enveloppes sont désormais de véritables liens. Le navigateur ouvre lui-même
un nouvel onglet, et le menu contextuel du clic droit propose « ouvrir dans un
nouvel onglet », « ouvrir dans une nouvelle fenêtre », comme pour n'importe quel
lien — de même que le Ctrl+clic et le clic molette.

L'adresse est posée sur la propriété `href` après le rendu plutôt qu'écrite dans
le gabarit HTML : aucune question d'échappement ne se pose sur une URL de plus
d'un kilo-octet dont une partie provient de la base. Le `rel` passe de
`noopener noreferrer` à `noopener` seul — supprimer l'en-tête `Referer` était une
variable inutile dans une chaîne d'authentification déjà instable.

**Message nommant le mauvais laboratoire (`index.html`)**
Le laboratoire mentionné dans les messages était celui sélectionné dans le menu,
et non celui de la personne destinataire. Les deux coïncident en navigation par
laboratoire, mais en recherche globale le menu conserve le dernier laboratoire
choisi : un enseignant-chercheur pouvait donc recevoir un message mentionnant le
laboratoire de quelqu'un d'autre, ou la formule générique « votre laboratoire ».

Le laboratoire est désormais lu sur la fiche de la personne. La sélection
courante reste privilégiée lorsqu'elle figure parmi ses laboratoires, afin de ne
rien changer au cas habituel. L'encadré des directeur·rice·s d'unité transmet de
la même façon le laboratoire qu'il affiche, plutôt que de dépendre de l'état du
menu.

**Échec de copie silencieux (`index.html`)**
Les trois boutons de copie — lien EC, tous les liens, message — appelaient
`navigator.clipboard` sans vérifier sa présence. Hors contexte sécurisé, cet
objet n'existe pas : le clic levait une erreur et il ne se passait rien à
l'écran, sans la moindre indication. Les trois passent désormais par un point
unique qui signale le cas et invite à sélectionner le texte à la main.

**La feuille de style n'était pas rechargée après mise à jour (`index.html`)**
Le `.htaccess` désactive le cache pour `index.html` seulement. Les utilisateurs
recevaient donc le nouveau HTML avec l'ancien `main.css` — sans conséquence
jusqu'ici, la feuille n'ayant pas changé depuis, mais cette version l'aurait fait
apparaître : liens d'envoi soulignés et mal calés jusqu'à un rechargement forcé.
La feuille est désormais appelée avec un paramètre de version
(`main.css?v=0.3.2`), à incrémenter à chaque release où elle change.

**Note technique** — `disabled` n'existe pas sur une balise `<a>` : l'état
inactif d'une enveloppe sans adresse repose sur l'absence de `href` et sur une
classe CSS, sans `pointer-events: none`, qui aurait laissé le clic atteindre la
ligne et ouvrir la fiche de l'EC.

### Limite connue

Deux comportements signalés ne relèvent pas de l'application et ne peuvent pas y
être corrigés : le système d'exploitation confie les liens `mailto:` à son
application de messagerie par défaut, quel que soit le navigateur d'où part le
clic ; et Zimbra renvoie une erreur 502 au lieu de la page de connexion lorsque
la session n'est pas ouverte. Le bouton de copie permet de contourner les deux.

## [0.3.1] — 2026-09-23

### Nouvelles fonctionnalités

**Retrait d'un mot-clef ajouté à la main (`index.html`, `main.css`)**
Une croix discrète, révélée au survol de la carte, permet de retirer de la fiche
un mot-clef qui y avait été ajouté par erreur — faute de frappe, doublon,
mauvaise sélection. Elle n'apparaît que sur les mots-clefs ajoutés manuellement :
ceux issus de l'extraction ScanR portent une annotation `curation:rank` et
restent en place. Le retrait est également refusé dès qu'un autre intervenant a
noté le mot-clef, pour ne pas effacer son travail à son insu.

Le retrait détache le concept de toutes les propriétés configurées et supprime
les expertises associées — sans cela elles resteraient en base en pointant vers
un mot-clef absent de la fiche, donc invisibles. Le `skos:Concept` lui-même est
conservé : les concepts devenus orphelins relèvent d'un nettoyage distinct.
L'item est relu juste avant l'écriture et la réponse du serveur est vérifiée.

Disponible pour les enseignant·e·s-chercheur·se·s sur leur fiche et pour les
opérateurs. Volontairement indisponible en vue DU, dont le rôle se limite au suivi.

**Note technique** — garde-fous ajoutés au fil des relectures : un verrou global
pendant le retrait (chaque retrait relit la fiche entière avant de la réécrire ;
deux retraits concurrents auraient lu la même version et le second aurait annulé
le premier), la remise à zéro du quota en début de chargement (une sortie
anticipée laissait sinon une valeur périmée), la réservation de la place du
bouton dans le titre de la carte, la pose du verrou après la confirmation et non
avant — sinon une annulation sortait sans passer par le `finally` et laissait le
verrou fermé pour le reste de la session — et l'extension de ce verrou à l'ajout
de mot-clef ainsi qu'à la suggestion, un ajout concurrent étant sinon écrasé par
la réécriture de la fiche. L'identifiant de la fiche est figé au début de
l'opération : l'utilisateur pouvant revenir à la liste entre deux écritures, les
étapes suivantes échouaient sinon sur une fiche pourtant correctement traitée.

La fiche n'est plus masquée pendant le retrait : `setLoading(true)` cachait le
bandeau, la grille et la barre d'action, et `setLoading(false)` ne les réaffiche
pas. Un retrait en échec laissait donc une page vide, à recharger manuellement.
Le retour visuel passe désormais par le curseur et le grisage du bouton, comme
pour l'enregistrement d'une note.

### Correctifs

**Page vide après une erreur d'ajout ou de suggestion (`index.html`)**
Même cause que ci-dessus, mais sur des chemins antérieurs à cette version :
l'ajout d'un mot-clef et la création d'une suggestion masquaient la fiche le
temps de l'écriture et ne la rétablissaient pas en cas d'erreur de l'API.
L'utilisateur se retrouvait devant une page vide et devait recharger. Les deux
chemins reconstruisent désormais l'affichage à partir des données déjà en
mémoire, sans relire l'API — l'erreur venant justement de là.

**Suggestion annoncée comme réussie alors qu'elle avait échoué (`index.html`)**
`addKeyword` signalait ses erreurs mais ne les remontait pas à son appelant. La
création d'une suggestion enchaînait donc sur « suggéré et ajouté » même lorsque
le rattachement à la fiche avait échoué, laissant un `skos:Concept` orphelin et
une suggestion décomptée du quota. `addKeyword` renvoie maintenant l'issue du
rattachement, et la suggestion n'est annoncée — ni décomptée — qu'en cas de
succès réel.

**Fiche EC sans issue en vue DU (`index.html`)**
Un directeur·rice d'unité qui ouvrait la fiche d'un EC de son laboratoire ne
pouvait plus revenir à la liste. Deux causes cumulées : le bouton « Retour à la
liste » n'était rendu que si `fromUrl` était faux, or un lien DU porte aussi
`idCreator` et `t` ; et `lockDlMode` neutralisait `showBrowse` par une fonction
vide. Le bouton est désormais affiché en vue DU et le retour ramène à la liste du
laboratoire. Le périmètre reste verrouillé : le laboratoire est refixé sur celui
du lien à chaque retour et le sélecteur reste masqué.

**Envoi d'email écrasant l'application (`index.html`)**
Les liens `mailto:` étaient ouverts via `window.location.href`. Avec un client
natif le navigateur intercepte sans quitter la page, mais lorsqu'un webmail
(Zimbra, Gmail…) est enregistré comme gestionnaire du protocole, le navigateur
naviguait réellement et la fiche en cours était remplacée. Les deux envois — vers
un EC et vers un DU — passent désormais par un lien `target="_blank"`, qui laisse
l'application intacte. `window.open` n'est pas utilisé : il est bloqué comme
popup par défaut.

**Suppressions annoncées à tort comme réussies (`index.html`)**
La couche d'accès à l'API ne contrôle pas le statut HTTP : un refus du serveur
(droits insuffisants, item déjà supprimé…) revient sous forme de réponse JSON
d'erreur, sans lever d'exception. La suppression d'une note affichait donc
« ✓ Note supprimée » et la retirait de l'écran alors qu'elle restait en base,
pour réapparaître au rechargement suivant. Les réponses de suppression sont
désormais vérifiées, dans le retrait d'un mot-clef comme dans la suppression
d'une note — au même titre que les créations et les mises à jour depuis la 0.2.12.

**Quota de suggestions consommé par un mot-clef retiré (`index.html`)**
Le quota était calculé en comptant les concepts créés par l'EC, indépendamment de
leur rattachement. Un EC qui se trompait de libellé, retirait son mot-clef et le
recréait aurait donc brûlé deux de ses trois suggestions. Le décompte porte
désormais sur les concepts effectivement attachés à sa fiche.

## [0.3.0] — 2026-09-08

### Améliorations ergonomiques

**Confirmation visible de l'enregistrement (`index.html`)**
Après une notation, le seul retour était le basculement discret du bouton
« Ajouter » en « Modifier ». Un message « ✓ Enregistré » s'affiche désormais à
côté du mot-clef concerné pendant quelques secondes, et « ✗ Non enregistré » en
cas d'échec. L'emplacement `.save-status` et ses styles existaient déjà dans le
code mais n'avaient jamais été utilisés.

**Boutons verrouillés pendant l'écriture (`index.html`)**
Les boutons Ajouter, Modifier et Supprimer d'un mot-clef sont désactivés le temps
de la requête. Un bouton qui ne réagit pas visiblement invite à recliquer, et
chaque clic déclenchait une écriture concurrente avant que le premier
enregistrement n'ait basculé le bouton en « Modifier ». Le déverrouillage est
placé dans un `finally`, donc garanti même en cas d'erreur.

**Indicateur de progression (`index.html`, `main.css`)**
L'en-tête de la fiche affiche le nombre de mots-clefs déjà évalués par
l'utilisateur courant sur le total (« 12 / 30 évalués »), et signale la complétion
(« ✓ 30 mots-clefs évalués ») en vert. Rien n'indiquait jusqu'ici où en était
l'enseignant·e-chercheur·se dans sa validation, ni s'il ou elle l'avait terminée.

**Guide EC enrichi et présenté à la première ouverture (`index.html`, `main.css`)**
Le bandeau « Comment noter vos mots-clefs ? » précise désormais qu'il faut cliquer
sur « Ajouter » pour enregistrer une note, et que chaque mot-clef est enregistré
séparément — on peut donc interrompre et reprendre plus tard avec le même lien.
Ce guide est en outre présenté en modale à la première ouverture du lien dans un
navigateur donné, avec un bouton « J'ai compris » ; il reste ensuite consultable
via le bandeau. Le contenu de la modale est recopié depuis le bandeau, ce qui
évite toute divergence entre les deux formulations. La clé de mémorisation est
versionnée, afin de pouvoir re-présenter le guide si son contenu évolue.
Réservé aux ECs. Si le `localStorage` est indisponible, la modale ne s'affiche pas
plutôt que de réapparaître à chaque ouverture.

### Note

Un mot-clef ajouté par l'EC reçoit automatiquement une expertise à 0
(« Sans avis ») et compte donc immédiatement comme évalué dans cet indicateur.
Cette valeur par défaut mériterait d'être reconsidérée : ajouter un mot-clef
signifie précisément que le domaine concerne l'EC.

## [0.2.13] — 2026-09-08

### Correctifs

**Niveaux d'expertise incohérents entre deux ouvertures (`index.html`)**
Lorsque plusieurs expertises du même auteur coexistaient sur un mot-clef —
séquelles du bug de pagination corrigé en 0.2.12 — trois parties du code
désignaient des enregistrements différents : le niveau surligné dans le sélecteur
provenait du premier de la liste, les pastilles et la couleur de la carte du
dernier, et la modification portait sur le premier. L'utilisateur pouvait donc
voir une note, en enregistrer une autre, et retrouver la première au rechargement.
L'ordre de la liste dépendant de celui renvoyé par l'API, la note affichée
pouvait varier d'une ouverture à l'autre sans qu'aucune modification n'ait eu lieu.

Une fonction unique `latestCreatorExpertise` sélectionne désormais l'expertise de
référence — la plus récente par `o:created`, départagée par identifiant à date
égale — et l'affichage comme l'édition s'appuient sur elle. La note montrée est
donc toujours celle qui sera modifiée, quel que soit le nombre de doublons et
quel que soit l'ordre de réponse de l'API.

**Signalement des expertises en double (`index.html`)**
Les mots-clefs portant plusieurs expertises du même auteur sont désormais
signalés à l'opérateur, avec leur nombre, afin d'identifier ce qui reste à
nettoyer en base. Le message n'est pas affiché aux ECs ni aux DU.

### Améliorations

**Suggestion de mots-clefs retirée en vue DU (`index.html`)**
La barre « Ajouter un mot-clef… » restait accessible aux directeur·rice·s
d'unité sur les fiches EC. Le rôle du DU se limitant au suivi de son
laboratoire, elle est désormais masquée dans ce mode. Les ECs et les opérateurs
conservent la fonctionnalité.

### Note

Ce correctif rend l'affichage cohérent malgré les doublons présents en base, mais
ne les supprime pas : leur nettoyage reste une opération distincte.

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


 