# Vérification qualité (notebook + exports web)

## Constats
- Le navigateur web des exports fonctionnait avec un **iframe unique** piloté par menu latéral.
- La demande métier est d'avoir **tous les tableaux visibles dans une seule page web**.
- Le rendu en "double tableau" peut apparaître pour deux raisons :
  1. un même DataFrame est affiché une fois via `afficher_tableau(...)` puis ré-affiché plus bas avec `display(df)` ;
  2. l'export générique `tableau_exporte.html` peut représenter un tableau déjà présent ailleurs (même contenu métier, nom différent).

## Correctifs appliqués
- `doc_fourni/exports/tableaux/index.html` converti en **vue consolidée** : tous les tableaux sont rendus dans la même page, via une grille de cartes et un iframe par tableau.
- Structure HTML/CSS simplifiée et homogénéisée (header, layout, cartes, chargement lazy).

## Recommandations "code pro" pour le notebook
- Ne pas mélanger `afficher_tableau(...)` et `display(...)` pour le même objet dans une cellule.
- Toujours passer un `nom_fichier` explicite à `afficher_tableau(...)` pour éviter l'ambiguïté autour de `tableau_exporte.html`.
- Lors de la génération d'index, filtrer les doublons de contenu (hash du HTML) afin de ne lister qu'une seule fois un tableau identique.
