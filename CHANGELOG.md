# Changelog

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


 