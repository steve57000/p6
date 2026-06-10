# Suivi projet BottleNeck — Étape 1
## Cadrage du projet, inventaire des fichiers et vérification de concordance

Date de suivi initial : 27/05/2026  
Date de mise à jour : 10/06/2026  
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
- vérifier que les documents de suivi, les résumés de phase et le notebook racontent la même histoire ;
- préparer la suite du travail dans le notebook et dans la présentation.

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

Le travail doit aussi permettre d'identifier les erreurs de données et de proposer des recommandations pour améliorer l'ERP, WordPress et les processus de gestion.

---

## 3. Livrables attendus

Livrables obligatoires de la mission :

| N° | Livrable | Format | Rôle | Statut de conformité au 10/06/2026 |
|---:|---|---|---|---|
| 1 | Notebook | `.ipynb` Python ou R | Contenir les imports, l'exploration, le nettoyage, les jointures, les analyses, les graphiques et les conclusions. | Conforme : `doc_fourni/Bell_Steve_1_notebook_062026.ipynb` existe, contient 137 cellules, des contrôles qualité, des analyses métier, des exports et une conclusion. |
| 2 | Présentation | PowerPoint / PDF, 20 slides maximum | Présenter la méthodologie, les analyses et les recommandations au CODIR. | Modèle disponible : `doc_fourni/ModeĖle_preėsentation-Bottleneck.pptx`. Le fichier final de présentation devra respecter le nommage attendu. |

Nommage attendu :
- dossier ZIP : `Titre_du_projet_nom_prénom` ;
- notebook : `Nom_Prenom_1_notebook_mmaaaa` ;
- présentation : `Nom_Prenom_2_presentation_mmaaaa`.

Nommage observé et retenu pour le notebook final : `Bell_Steve_1_notebook_062026.ipynb`.

---

## 4. Inventaire des fichiers disponibles

| Fichier | Type | Contenu / rôle | Statut vérifié |
|---|---:|---|---|
| `mission/livrables_et_soutenance.pdf` | PDF | Règles des livrables, soutenance, durée et questions possibles du jury. | À utiliser pour vérifier la conformité finale. |
| `mission/quallez_vous_apprendre.pdf` | PDF | Objectifs pédagogiques du projet : librairies, notebook, jointures, analyses exploratoires et présentation. | Document de cadrage pédagogique. |
| `mission/scenario.pdf` | PDF | Scénario métier BottleNeck, consignes de Nicolas, phases 1 et 2, analyses demandées. | Source principale de la mission. |
| `my_doc/analyse_mission.pdf` | PDF | Synthèse des attentes du projet, des livrables et de la structure recommandée. | Support d'organisation. |
| `my_doc/analyse_fichiers.pdf` | PDF | Analyse initiale des fichiers sources. | Support d'inventaire. |
| `doc_fourni/Data+Bottleneck/erp.xlsx` | Excel | Extraction ERP : `product_id`, vente web, prix, stock, statut de stock, prix d'achat. | Source de données métier. |
| `doc_fourni/Data+Bottleneck/web.xlsx` | Excel | Extraction WordPress : SKU, ventes, dates, titres produits, descriptions, statuts, URLs, type de contenu. | Source de données web. |
| `doc_fourni/Data+Bottleneck/liaison.xlsx` | Excel | Table de liaison entre `id_web` et `product_id`. | Fichier clé pour les jointures. |
| `doc_fourni/Bell_Steve_1_notebook_062026.ipynb` | Notebook | Notebook final de travail : chargement, contrôles, nettoyage, jointures, analyses, anomalies, recommandations et exports. | Livrable notebook conforme au périmètre demandé. |
| `doc_fourni/ModeĖle_preėsentation-Bottleneck.pptx` | PowerPoint | Modèle de présentation. | Base de présentation à enrichir. |
| `doc_fourni/exports/bottleneck_base_consolidee.xlsx` | Excel | Export de la base consolidée. | Sortie cohérente avec le notebook. |
| `doc_fourni/exports/tableaux/index.html` | HTML | Index consolidé des tableaux exportés. | Support de navigation des exports. |
| `doc_fourni/exports/tableaux/dashboard_livrable.html` | HTML | Dashboard de synthèse du livrable. | Support complémentaire de restitution. |
| `suivi_etapes/Bell_Steve_resume_phase_1_Bottleneck.docx` | Word | Résumé de la phase 1 : préparation, contrôles, anomalies et recommandations système. | À vérifier/corriger manuellement ensuite pour pointer vers le notebook final. |
| `suivi_etapes/Bell_Steve_resume_phase_2_Bottleneck.docx` | Word | Résumé de la phase 2 : analyses métier, KPI CODIR, prix atypiques, stocks, marges et corrélations. | À vérifier/corriger manuellement ensuite pour pointer vers le notebook final. |
| `suivi_etapes/Bell_Steve_guide_fonctionnement_code_Bottleneck.docx` | Word | Guide à créer manuellement pour expliquer le fonctionnement du code, des fonctions, des exports HTML, des calculs Python et des librairies utilisées. | À créer manuellement, puis à compléter ensuite. |

Point de concordance à corriger dans les documents Word : les résumés de phase ne doivent plus faire croire qu'un fichier `Template-Notebook-Bottleneck.ipynb` est le livrable de départ présent dans le dépôt. La référence documentaire cohérente est le notebook final `Bell_Steve_1_notebook_062026.ipynb`.

---

## 5. Inventaire rapide des fichiers de données

| Fichier | Feuille | Dimensions | Colonnes principales |
|---|---|---:|---|
| `liaison.xlsx` | `Sheet1` | 825 lignes de données + en-tête, 2 colonnes | `id_web`, `product_id` |
| `erp.xlsx` | `Sheet1` | 825 lignes de données + en-tête, 6 colonnes | `product_id`, `onsale_web`, `price`, `stock_quantity`, `stock_status`, `purchase_price` |
| `web.xlsx` | `Sheet1` | 1513 lignes de données + en-tête, 29 colonnes | `sku`, `total_sales`, `post_title`, `post_status`, `post_type`, `guid`, etc. |

Premières observations confirmées :
- `erp.xlsx` est structuré autour de la clé `product_id` ;
- `web.xlsx` est structuré autour de la clé `sku` ;
- `liaison.xlsx` sert à relier `id_web` / `sku` avec `product_id` ;
- `web.xlsx` contient à la fois des lignes de type `product` et des lignes de type `attachment`, donc le notebook filtre les produits avant les analyses ;
- la table de liaison reste critique : toute erreur ou absence de clé peut créer des produits non rapprochés.

---

## 6. Périmètre de la mission et correspondance avec le notebook

### Phase 1 — Préparation et fiabilisation des données

| Attendu | Vérification dans le livrable notebook |
|---|---|
| Charger les trois fichiers de données | Présent : chargement automatisé de `erp.xlsx`, `web.xlsx` et `liaison.xlsx`. |
| Explorer les dimensions, types et valeurs manquantes | Présent : fonctions de synthèse et tableaux de contrôle. |
| Contrôler les doublons | Présent : contrôles sur `product_id`, `sku`, lignes dupliquées et lignes Web. |
| Contrôler les clés de jointure | Présent : nettoyage des clés, contrôles des clés manquantes et rapprochement ERP / liaison / Web. |
| Rapprocher ERP + liaison + web | Présent : jointure ERP + liaison puis jointure avec les lignes Web de type `product`. |
| Identifier au moins 8 erreurs ou anomalies | Présent : journal d'anomalies structuré couvrant ERP, Web, liaison, jointures, prix et marges. |
| Proposer des solutions pour améliorer les données | Présent : recommandations opérationnelles pour l'ERP, la liaison, les prix, les stocks et la qualité des données. |

### Phase 2 — Analyses pour le CODIR

| Analyse attendue | Vérification dans le livrable notebook |
|---|---|
| Chiffre d'affaires par produit | Présent. |
| Chiffre d'affaires total | Présent. |
| Tops références | Présent : top CA et top quantités. |
| Analyse 20/80 | Présent : CA et quantités. |
| Valeurs aberrantes sur les prix | Présent : Z-score et méthode IQR. |
| Boxplot des prix | Présent. |
| Analyse des stocks | Présent : stock, valorisation, mois de stock et stocks dormants. |
| Rotation ou nombre de mois de stock | Présent : indicateur de mois de stock. |
| Marges et taux de marge | Présent : prix HT, marge unitaire, taux de marge et marges négatives. |
| Corrélations entre variables quantitatives | Présent : matrice de corrélation et interprétation. |
| Recommandations pour l'ERP | Présent : recommandations de fiabilisation des données et des contrôles. |

Conclusion de vérification : le notebook `Bell_Steve_1_notebook_062026.ipynb` respecte le périmètre demandé pour la mission. Les documents de phase 1 et phase 2 devront être alignés manuellement avec ce notebook après correction de leur référence au notebook final.

---

## 7. Risques et points de vigilance dès le départ

| Point de vigilance | Pourquoi c'est important | Traitement retenu |
|---|---|---|
| Clés de jointure | Les références ne correspondent pas directement entre ERP et Web. | Nettoyage des clés et passage par `liaison.xlsx`. |
| Produits non liés | Ils peuvent fausser le chiffre d'affaires, le stock ou les marges. | Isolation et documentation des produits non rapprochés. |
| Types de données | Les prix, ventes et stocks doivent être numériques pour permettre les calculs. | Contrôle dans les synthèses et calculs métier. |
| Lignes `attachment` dans le web | Elles ne représentent pas des produits vendables. | Exclusion avant la jointure métier. |
| Valeurs manquantes | Elles doivent être isolées, expliquées ou corrigées avec une règle claire. | Journal d'anomalies et règles de correction. |
| Valeurs aberrantes | Certaines peuvent être de vrais produits premium, pas forcément des erreurs. | Détection statistique puis recommandation de contrôle métier. |
| Stock et statut incohérents | Exemple : stock positif avec statut `outofstock`. | Contrôle entre `stock_quantity` et `stock_status`. |
| RGPD | Les exports doivent éviter les données personnelles inutiles. | Contrôle RGPD documenté dans le notebook. |

---

## 8. Décision de travail pour la suite

Ordre de travail retenu et statut :

1. Cadrage du projet et inventaire des fichiers — terminé.
2. Préparation du notebook et import des librairies — terminé.
3. Chargement des fichiers Excel — terminé.
4. Exploration initiale des trois fichiers — terminé.
5. Contrôle des clés et préparation des jointures — terminé.
6. Consolidation ERP / liaison / web — terminé.
7. Identification et documentation des anomalies — terminé.
8. Nettoyage et règles de correction — terminé.
9. Calculs métier : CA, marges, stocks, rotations — terminé.
10. Analyses statistiques : valeurs aberrantes, 20/80, corrélations — terminé.
11. Recommandations ERP — terminé.
12. Préparation de la présentation — à finaliser à partir du modèle PowerPoint.
13. Vérification finale des livrables — réalisée pour le notebook et les documents de suivi le 10/06/2026.

---

## 9. Vérification des documents réalisée le 10/06/2026

| Élément vérifié | Résultat |
|---|---|
| `suivi_etape_1_cadrage_inventaire_bottleneck.md` | Mis à jour pour refléter les fichiers réellement présents, le notebook final et la vérification de concordance. |
| `Bell_Steve_resume_phase_1_Bottleneck.docx` | Vérifié sur le fond : le contenu correspond à la phase 1. Correction binaire à faire manuellement pour pointer vers le notebook final. |
| `Bell_Steve_resume_phase_2_Bottleneck.docx` | Vérifié sur le fond : le contenu correspond à la phase 2. Correction binaire à faire manuellement pour pointer vers le notebook final. |
| `Bell_Steve_1_notebook_062026.ipynb` | Vérifié : 137 cellules, 89 cellules de code, syntaxe Python valide, sections attendues présentes. |
| Exports HTML | Présents : index consolidé et dashboard livrable. |
| Nouveau guide technique Word | À créer manuellement : `Bell_Steve_guide_fonctionnement_code_Bottleneck.docx`, puis à compléter avec l’explication du code. |

---

## 10. Statut de l'étape 1

Statut : terminé et contrôlé.

Résultat : le projet est cadré, les fichiers sont identifiés, le rôle de chaque fichier est clair, le fichier de suivi est aligné avec le notebook final, et le livrable notebook respecte le périmètre demandé.

Prochaine étape recommandée : finaliser la présentation CODIR en s'appuyant sur le modèle PowerPoint, le résumé de phase 2 et le dashboard HTML.
