# 01 — Plan du notebook parfait

## Pourquoi structurer un notebook ?

Un notebook n'est pas seulement une suite de cellules de code. C'est un document d'analyse. Il doit être compréhensible par :

- toi dans plusieurs semaines ;
- un évaluateur ;
- un collègue data analyst ;
- une personne métier qui veut comprendre les résultats.

## Structure recommandée

### 1. Introduction

À mettre au début :

- contexte du projet ;
- objectif métier ;
- fichiers utilisés ;
- livrables attendus ;
- méthode générale.

Exemple :

```markdown
# Analyse BottleNeck

Objectif : rapprocher les données ERP, Web et Liaison afin d'analyser les ventes, les stocks, les prix et les anomalies potentielles.
```

### 2. Importation des librairies

Une seule cellule d'import au début du notebook. Évite d'importer des librairies au milieu sauf raison claire.

### 3. Configuration du projet

Définis les chemins, options d'affichage, paramètres graphiques et dossiers d'exports.

### 4. Chargement des données

Charge les fichiers dans des DataFrames séparés :

- `df_erp` ;
- `df_web` ;
- `df_liaison`.

### 5. Exploration de chaque fichier

Pour chaque source :

- dimensions ;
- colonnes ;
- types ;
- aperçu ;
- valeurs manquantes ;
- doublons ;
- règles métier.

### 6. Nettoyage

Crée des copies propres :

- `df_erp_clean` ;
- `df_web_clean` ;
- `df_liaison_clean`.

Ne détruis pas les DataFrames bruts : ils servent de référence.

### 7. Jointures

Fusionne progressivement :

1. ERP + Liaison ;
2. résultat + Web.

Utilise toujours `indicator=True` dans les jointures critiques pour savoir d'où viennent les lignes.

### 8. Analyses métier

Calcule les indicateurs :

- chiffre d'affaires ;
- quantités vendues ;
- valeur du stock ;
- taux de marge ;
- rotation ou mois de stock ;
- top/flop produits.

### 9. Analyse statistique

Détecte les valeurs atypiques avec :

- Z-score ;
- intervalle interquartile ;
- boxplots ;
- histogrammes.

### 10. Exports et conclusion

Termine par :

- exports HTML ;
- export Excel ;
- tableau de synthèse des anomalies ;
- recommandations métier ;
- limites de l'analyse.

## Règle professionnelle

Chaque grande étape doit contenir :

1. **Objectif** ;
2. **Code** ;
3. **Résultat** ;
4. **Interprétation** ;
5. **Action ou recommandation**.

## Erreurs fréquentes

- Mélanger nettoyage, analyse et visualisation dans une seule cellule.
- Afficher deux fois le même tableau.
- Ne pas expliquer les résultats.
- Écraser les données brutes.
- Faire une jointure sans contrôler les lignes perdues.
