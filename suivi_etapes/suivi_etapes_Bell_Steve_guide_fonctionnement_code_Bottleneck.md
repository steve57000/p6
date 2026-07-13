# Projet BottleNeck

## Guide de fonctionnement du code — Notebook Bell_Steve_1_notebook_062026.ipynb

Mise à jour : 10/06/2026

## Objectif du document

- Ce guide explique le fonctionnement du notebook final : librairies, fonctions, fichiers HTML, traitements Python, calculs métier et raisons des choix techniques.

## 1. Flux de travail du notebook

- Importer l’environnement Python et appliquer le thème visuel BottleNeck.

- Charger erp.xlsx, web.xlsx et liaison.xlsx.

- Explorer les dimensions, types, doublons, valeurs manquantes et clés.

- Nettoyer les clés puis rapprocher ERP, liaison et Web.

- Calculer les indicateurs métier : CA, ventes, stocks, mois de stock, marges, 20/80 et corrélations.

- Exporter la base consolidée, les tableaux HTML, le dashboard et l’index de navigation.

## 2. Librairies utilisées

- pandas : lecture Excel, DataFrame, jointures, contrôles qualité, calculs et exports.

- numpy : calculs numériques, masques et valeurs manquantes.

- matplotlib.pyplot : graphiques statiques compatibles Jupyter, PyCharm et Colab.

- seaborn : thème graphique et heatmap de corrélation.

- IPython.display : affichage enrichi HTML / Markdown dans le notebook.

- pathlib, os, socket, http.server, socketserver : chemins, dossiers d’exports et serveur local optionnel.

- re, html, base64, datetime, warnings : nettoyage de texte, sécurisation HTML, horodatage et confort d’exécution.

## 3. Fonctions d’affichage et exports HTML

- THEME centralise les couleurs utilisées dans les tableaux, messages et cartes KPI.

- _formatter_colonne adapte les formats : euros, pourcentages, identifiants, stocks et ventes.

- _slugifier transforme un titre en nom de fichier compatible pour les exports.

- _nom_export_tableau et _titre_export_tableau donnent des noms lisibles aux exports.

- _preparer_html_tableau convertit un DataFrame en tableau HTML propre.

- exporter_tableau_html écrit les fichiers dans livrables/exports/tableaux.

- afficher_tableau affiche le tableau dans le notebook et crée son export HTML.

- afficher_kpis crée des cartes d’indicateurs pour les résultats clés.

- afficher_message affiche les messages de statut, alerte ou succès.

## 4. Chargement des fichiers

- contient_fichiers_attendus vérifie la présence de erp.xlsx, web.xlsx et liaison.xlsx.

- trouver_dossier_data recherche automatiquement le dossier Data+Bottleneck pour rendre le notebook portable.

- pd.read_excel charge les trois fichiers dans df_erp, df_web et df_liaison.

## 5. Contrôles qualité et préparation

- resume_dimensions contrôle le nombre de lignes et colonnes.

- synthese_dataframe résume types, valeurs manquantes, unicité et exemples.

- nettoyer_cle homogénéise product_id, id_web et sku en texte propre.

- rapport_cles_manquantes mesure les références impossibles à rapprocher.

- afficher_statut rend les contrôles plus lisibles.

## 6. Jointures ERP / liaison / Web

- Création de product_id_key côté ERP et liaison.

- Création de id_web_key côté liaison et sku_key côté Web.

- Filtrage du Web sur post_type = product pour exclure les attachments.

- Jointure ERP + liaison, puis jointure avec les produits Web.

- Les clés manquantes sont isolées plutôt que fusionnées de force.

## 7. Calculs métier

- CA par article : ca_par_article = price × total_sales.

- CA total : somme du CA sur les lignes valides.

- Top CA et top quantités pour repérer les références prioritaires.

- 20/80 : cumul du CA et des volumes pour mesurer la concentration.

- Stock : quantités, valorisation achat, valorisation vente et stocks dormants.

- Mois de stock : stock_quantity / total_sales quand les ventes sont positives.

- Marge : prix HT, marge unitaire, marge totale, taux de marge et marges négatives.

## 8. Analyses statistiques et graphiques

- Boxplot des prix pour visualiser les valeurs atypiques.

- Z-score pour repérer les prix très éloignés de la moyenne.

- IQR pour isoler les prix au-delà du seuil Q3 + 1,5 × IQR.

- Heatmap de corrélation pour analyser les liens entre prix, stock, ventes, CA et marges.

- Matplotlib est privilégié pour assurer un rendu stable pendant la correction.

## 9. Journal des anomalies

- ajouter_anomalie centralise la source, le type, la description, le nombre de lignes et l’action recommandée.

- Le journal couvre prix, stocks, SKU, liaisons, produits non rapprochés et marges.

- Chaque anomalie est reliée à une recommandation concrète pour l’ERP, WordPress ou la table de liaison.

## 10. Exports livrables

- bottleneck_base_consolidee.xlsx : base consolidée exploitable.

- livrables/exports/tableaux/*.html : tableaux de contrôle et d’analyse.

- dashboard_livrable.html : synthèse des principaux KPI.

- index.html : navigateur des exports par catégories.

- ReusableTCPServer, trouver_port_libre et lancer_serveur_exports facilitent l’ouverture locale des exports HTML.

## 11. Mode d’emploi

- Conserver l’arborescence Data+Bottleneck avec les trois fichiers Excel.

- Ouvrir Bell_Steve_1_notebook_062026.ipynb.

- Exécuter les cellules dans l’ordre.

- Vérifier les tableaux de contrôle, le journal des anomalies, la base consolidée, le dashboard et l’index HTML.
