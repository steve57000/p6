# Suivi projet BottleNeck — Étape 1
## Cadrage du projet et inventaire des fichiers

Date de suivi : 27/05/2026
Projet : Optimisez la gestion & nettoyez les données du stock d'une boutique
Contexte : mission d'analyse de données pour BottleNeck, marchand de vin.

---

## 1. Objectif de l'étape

Cette première étape sert à cadrer clairement le projet avant de commencer le nettoyage et l'analyse des données.

Objectifs :
- comprendre la mission demandée par Nicolas ;
- identifier les livrables obligatoires ;
- inventorier tous les fichiers disponibles ;
- repérer le rôle de chaque fichier ;
- préparer la suite du travail dans le notebook.

---

## 2. Résumé du projet

La mission consiste à consolider plusieurs sources de données de BottleNeck :
- une extraction ERP ;
- une extraction du site web WordPress ;
- une table de liaison entre les deux systèmes.

Le but est de produire une base exploitable pour analyser :
- le chiffre d'affaires ;
- les ventes ;
- les stocks ;
- les marges ;
- les valeurs aberrantes ;
- les corrélations entre variables quantitatives.

Le travail doit aussi permettre d'identifier les erreurs de données et de proposer des recommandations pour améliorer l'ERP et les processus de gestion.

---

## 3. Livrables attendus

Livrables obligatoires :

| N° | Livrable | Format | Rôle |
|---:|---|---|---|
| 1 | Notebook | `.ipynb` Python ou R | Contenir les imports, l'exploration, le nettoyage, les jointures, les analyses, les graphiques et les conclusions. |
| 2 | Présentation | PowerPoint / PDF, 20 slides maximum | Présenter la méthodologie, les analyses et les recommandations au CODIR. |

Nommage attendu :
- dossier ZIP : `Titre_du_projet_nom_prénom` ;
- notebook : `Nom_Prenom_1_notebook_mmaaaa` ;
- présentation : `Nom_Prenom_2_presentation_mmaaaa`.

---

## 4. Inventaire des fichiers disponibles

| Fichier | Type | Taille approximative | Contenu / rôle | Statut |
|---|---:|---:|---|---|
| `livrables_et_soutenance.pdf` | PDF | 131 Ko | Règles des livrables, soutenance, durée et questions possibles du jury. | À utiliser pour vérifier la conformité finale. |
| `quallez_vous_apprendre.pdf` | PDF | 43 Ko | Objectifs pédagogiques du projet : librairies, notebook, jointures, analyses exploratoires et présentation. | Document de cadrage pédagogique. |
| `scenario.pdf` | PDF | 163 Ko | Scénario métier BottleNeck, consignes de Nicolas, phases 1 et 2, analyses demandées. | Source principale de la mission. |
| `analyse_mission.pdf` | PDF | 107 Ko | Synthèse déjà préparée des attentes du projet, des livrables et de la structure recommandée. | Support d'organisation. |
| `erp.xlsx` | Excel | 38 Ko | Extraction ERP : `product_id`, vente web, prix, stock, statut de stock, prix d'achat. | Source de données métier. |
| `web.xlsx` | Excel | 331 Ko | Extraction WordPress : SKU, ventes, dates, titres produits, descriptions, statuts, URLs, type de contenu. | Source de données web. |
| `liaison.xlsx` | Excel | 22 Ko | Table de liaison entre `id_web` et `product_id`. | Fichier clé pour les jointures. |
| `Template-Notebook-Bottleneck.ipynb` | Notebook | 31 Ko | Notebook de départ commencé par Nicolas, 92 cellules. | Base de travail à compléter. |
| `ModeĖle_preėsentation-Bottleneck.pptx` | PowerPoint | 266 Ko | Modèle de présentation, 7 slides. | Base de présentation à enrichir. |

---

## 5. Inventaire rapide des fichiers de données

| Fichier | Feuille | Dimensions | Colonnes principales |
|---|---|---:|---|
| `liaison.xlsx` | `Sheet1` | 825 lignes de données + en-tête, 2 colonnes | `id_web`, `product_id` |
| `erp.xlsx` | `Sheet1` | 825 lignes de données + en-tête, 6 colonnes | `product_id`, `onsale_web`, `price`, `stock_quantity`, `stock_status`, `purchase_price` |
| `web.xlsx` | `Sheet1` | 1513 lignes de données + en-tête, 29 colonnes | `sku`, `total_sales`, `post_title`, `post_status`, `post_type`, `guid`, etc. |

Premières observations :
- `erp.xlsx` semble structuré autour de la clé `product_id` ;
- `web.xlsx` semble structuré autour de la clé `sku` ;
- `liaison.xlsx` sert à relier `id_web` / `sku` avec `product_id` ;
- `web.xlsx` contient à la fois des lignes de type `product` et des lignes de type `attachment`, ce qui devra être contrôlé avant les analyses ;
- la table de liaison sera critique : toute erreur ou absence de clé peut créer des produits non rapprochés.

---

## 6. Périmètre de la mission

### Phase 1 — Préparation et fiabilisation des données

À faire :
- charger les trois fichiers de données ;
- explorer les dimensions, types et valeurs manquantes ;
- contrôler les doublons ;
- contrôler les clés de jointure ;
- rapprocher ERP + liaison + web ;
- identifier au moins 8 erreurs ou anomalies ;
- proposer des solutions pour améliorer les données dans les systèmes.

### Phase 2 — Analyses pour le CODIR

À produire :
- chiffre d'affaires par produit ;
- chiffre d'affaires total ;
- tops références ;
- analyse 20/80 ;
- valeurs aberrantes sur les prix ;
- boxplot des prix ;
- analyses des stocks ;
- rotation ou nombre de mois de stock ;
- marges et taux de marge ;
- corrélations entre variables quantitatives ;
- recommandations pour l'ERP.

---

## 7. Risques et points de vigilance dès le départ

| Point de vigilance | Pourquoi c'est important |
|---|---|
| Clés de jointure | Les références ne correspondent pas directement entre ERP et web. |
| Produits non liés | Ils peuvent fausser le chiffre d'affaires, le stock ou les marges. |
| Types de données | Les prix, ventes et stocks doivent être numériques pour permettre les calculs. |
| Lignes `attachment` dans le web | Elles ne représentent pas forcément des produits vendables. |
| Valeurs manquantes | Elles doivent être isolées, expliquées ou corrigées avec une règle claire. |
| Valeurs aberrantes | Certaines peuvent être de vrais produits premium, pas forcément des erreurs. |
| Stock et statut incohérents | Exemple possible : stock positif avec statut `outofstock`. |
| RGPD | À vérifier : présence ou non de données personnelles dans les fichiers. |

---

## 8. Décision de travail pour la suite

Nous allons travailler étape par étape, dans cet ordre :

1. Cadrage du projet et inventaire des fichiers — terminé.
2. Préparation du notebook et import des librairies.
3. Chargement des fichiers Excel.
4. Exploration initiale des trois fichiers.
5. Contrôle des clés et préparation des jointures.
6. Consolidation ERP / liaison / web.
7. Identification et documentation des anomalies.
8. Nettoyage et règles de correction.
9. Calculs métier : CA, marges, stocks, rotations.
10. Analyses statistiques : valeurs aberrantes, 20/80, corrélations.
11. Recommandations ERP.
12. Préparation de la présentation.
13. Vérification finale des livrables.

---

## 9. Statut de l'étape 1

Statut : terminé.

Résultat : le projet est cadré, les fichiers sont identifiés, le rôle de chaque fichier est clair, et la prochaine étape peut commencer avec la préparation du notebook.

Prochaine étape recommandée : Étape 2 — Préparation du notebook et import des librairies.
