# BottleNeck — Projet OpenClassrooms

Ce repository regroupe le travail de préparation, contrôle, consolidation et analyse des données de BottleNeck. L’objectif est de rapprocher les données ERP, Web WordPress et table de liaison afin de produire une base fiable pour le CODIR : chiffre d’affaires, ventes, stocks, marges, prix atypiques, anomalies et recommandations.

## Livrables officiels

- Notebook final : `livrables/Bell_Steve_1_notebook_062026.ipynb`
- Présentation finale PDF : `livrables/Bell_Steve_2_presentation_Bottleneck_062026.pdf`

## Arborescence principale

```text
livrables/
├── Bell_Steve_1_notebook_062026.ipynb
├── Bell_Steve_2_presentation_Bottleneck_062026.pdf
├── Data+Bottleneck/                 # fichiers ERP, Web et liaison attendus par le notebook
└── exports/
    ├── bottleneck_base_consolidee.xlsx
    └── tableaux/                    # dashboard et exports HTML
my_doc/                              # notes et vérifications projet
suivi_etapes/                        # documentation de suivi et guide de fonctionnement
mission/                             # documents de contexte fournis
```

## Ouvrir et exécuter le notebook

1. Ouvrir `livrables/Bell_Steve_1_notebook_062026.ipynb` dans Jupyter Notebook, JupyterLab, VS Code ou Google Colab.
2. Vérifier que le dossier de données attendu est accessible. Le notebook recherche automatiquement le dossier contenant les fichiers `erp.xlsx`, `web.xlsx` et `liaison.xlsx`.
3. Exécuter les cellules dans l’ordre pour charger les données, nettoyer les clés, consolider la base, produire les contrôles qualité et générer les exports.
4. Consulter le dashboard HTML : `livrables/exports/tableaux/dashboard_livrable.html`.
5. Consulter l’index des exports clés : `livrables/exports/tableaux/index.html`.

## Résultats clés

- 714 produits consolidés dans la base d’analyse.
- Chiffre d’affaires total estimé : 143 680,10 €.
- 13 contrôles/anomalies documentés dans le journal qualité.
- 1 alerte marge prioritaire.
- Marge totale estimée liée aux ventes : 44 660,65 €.

## Points de soutenance à retenir

- La clé Web exploitable est `sku`; la table de liaison fait le pont entre `product_id` ERP et la référence Web.
- La base d’analyse finale contient les produits rapprochés ERP/Web exploitables, et non l’intégralité brute des lignes sources.
- Les lignes Web non produits et les clés absentes sont contrôlées avant les analyses métier.
- Les indicateurs financiers doivent être interprétés sur les ventes : la marge totale correspond à `marge_unitaire × total_sales`.
- Les exports HTML servent de supports de vérification et de navigation, mais le notebook reste la source de vérité des calculs.
