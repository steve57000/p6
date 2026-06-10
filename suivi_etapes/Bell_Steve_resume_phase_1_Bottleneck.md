# Projet BottleNeck

Résumé de la phase 1 — Agrégation, contrôle et fiabilisation des données

Objectif : savoir ce qui était demandé, ce qui a été réalisé dans le notebook, et les points importants à retenir pour la soutenance.

| Résumé en une phrase<br>La phase 1 transforme trois fichiers séparés et imparfaits — ERP, Web WordPress et table de liaison — en une base consolidée exploitable, tout en documentant les erreurs détectées et les améliorations à mettre en place dans les systèmes. |
| --- |

| Élément | Détail |
| --- | --- |
| Livrable concerné | Bell_Steve_1_notebook_062026.ipynb |
| Base de départ | Bell_Steve_1_notebook_062026.ipynb |
| Fichiers de données | erp.xlsx, web.xlsx, liaison.xlsx |
| Périmètre de ce document | Phase 1 : agrégation, contrôles qualité, anomalies et recommandations système |

## 1. Ce que la phase 1 demandait

Dans le scénario, Nicolas demande de commencer par une phase de préparation des données. Cette phase ne consiste pas encore à présenter les analyses de chiffre d’affaires : elle sert surtout à rendre les fichiers exploitables et fiables.

| N° | Attendu | Explication |
| --- | --- | --- |
| 1 | Rapprocher les fichiers | Relier l’extraction de la base Web avec la base ERP en passant par la table de liaison. |
| 2 | Identifier les erreurs | Repérer au moins 8 anomalies : erreurs de saisie, de type, de calcul, de jointure, de valeurs manquantes ou d’incohérences métier. |
| 3 | Proposer des solutions | Formuler des recommandations concrètes pour améliorer les données dans les systèmes de l’entreprise. |

| Point clé à retenir<br>La phase 1 ne doit pas seulement produire une jointure technique. Elle doit aussi expliquer la qualité des données : ce qui est fiable, ce qui est douteux, ce qui a été exclu des jointures, et ce qui doit être corrigé dans les outils source. |
| --- |

## 2. Données utilisées et rôle de chaque fichier

| Fichier | Dimensions contrôlées | Rôle dans la consolidation |
| --- | --- | --- |
| erp.xlsx | 825 lignes / 6 colonnes | Référentiel ERP : product_id, vente web, prix, stock, statut de stock, prix d’achat. |
| web.xlsx | 1513 lignes / 29 colonnes | Extraction WordPress : SKU, ventes, titre produit, statut, type de ligne, URL et autres informations web. |
| liaison.xlsx | 825 lignes / 2 colonnes | Table de correspondance entre product_id ERP et id_web / SKU côté Web. |

| Pourquoi la table de liaison est critique<br>Les références ERP et Web ne correspondent pas directement. La table de liaison permet de passer de product_id à id_web/SKU. Une erreur dans cette table peut donc exclure un produit de l’analyse ou créer un mauvais rapprochement. |
| --- |

## 3. Ce qui a été fait dans le notebook

| Étape | Travail réalisé | Détail |
| --- | --- | --- |
| 1 | Chargement des fichiers | Les trois fichiers Excel ont été chargés dans Pandas : ERP, Web et Liaison. |
| 2 | Exploration initiale | Contrôle des dimensions, des types, des valeurs manquantes, des doublons et des aperçus de données. |
| 3 | Nettoyage des clés | Création de clés homogènes : product_id côté ERP/Liaison et sku_key/id_web_key côté Web/Liaison. |
| 4 | Préparation du Web | Conservation des lignes post_type = product ; exclusion des attachments et des lignes produit sans SKU pour éviter les fausses jointures. |
| 5 | Jointure ERP + Liaison | Fusion en conservant l’ERP comme base principale afin de vérifier la couverture de la table de liaison. |
| 6 | Jointure avec Web | Fusion avec les produits Web sur id_web_key = sku_key ; création de la base consolidée df_analyse. |
| 7 | Contrôles qualité | Détection des incohérences de stock, prix, SKU, liaison, jointure et marge. |
| 8 | Journal d’anomalies | Centralisation des anomalies dans un tableau avec source, type, description, nombre de lignes et action recommandée. |

## 4. Résultat de la consolidation

| Contrôle | Résultat | Interprétation |
| --- | --- | --- |
| ERP + Liaison | 825 produits ERP rapprochés sur 825 | Aucun product_id ERP absent de la table de liaison. |
| Produits sans id_web | 91 lignes | Produits ERP présents dans la liaison mais sans référence Web renseignée. |
| Web produit exploitable | 714 lignes avec SKU | Les lignes Web de type product avec une clé SKU sont utilisées pour la jointure. |
| Lignes produit sans SKU | 2 lignes | Exclues de la jointure pour éviter une correspondance artificielle sur des valeurs manquantes. |
| Produits avec id_web mais absents du Web produit | 20 lignes | Produits à contrôler : suppression, archivage, mauvaise saisie ou absence côté Web. |
| Base finale d’analyse | 714 produits consolidés | Base utilisée ensuite pour le CA, les ventes, les stocks, les marges et les corrélations. |

| Décision importante<br>Les lignes dont la clé est manquante ne sont pas fusionnées. C’est volontaire : dans Pandas, fusionner des valeurs manquantes peut créer des correspondances artificielles. Le notebook les isole au lieu de les intégrer de force. |
| --- |

## 5. Anomalies détectées et actions recommandées

Le notebook dépasse le minimum demandé : il documente un journal d’anomalies structuré. Certaines lignes sont de vraies erreurs à corriger, d’autres sont des points à contrôler avant décision métier.

| Source | Anomalie / contrôle | Nb | Action recommandée |
| --- | --- | --- | --- |
| ERP | Statut de stock incohérent | 2 | Recalculer automatiquement stock_status à partir de stock_quantity. |
| ERP | Prix manquant, nul ou négatif | 3 | Bloquer la mise en vente si le prix est absent ou incohérent. |
| ERP | Stock négatif | 2 | Contrôler les mouvements de stock et empêcher les stocks négatifs non justifiés. |
| ERP | Prix d’achat supérieur au prix de vente | 7 | Contrôler la saisie des prix d’achat et de vente, car la marge peut être négative. |
| Web | Lignes sans SKU dans le fichier brut | 85 | Exclure les lignes non produit et rendre le SKU obligatoire pour les produits. |
| Web | SKU au format non numérique | 4 | Vérifier s’il s’agit de vraies références métier ou d’erreurs de saisie. |
| Web | SKU en doublon sur les produits | 0 | Contrôle validé : l’unicité du SKU produit est respectée. |
| Liaison | Articles sans id_web | 91 | Compléter la table de liaison pour les produits devant être présents sur le Web. |
| Jointure | ERP sans liaison | 0 | Contrôle validé : tous les product_id ERP sont couverts par la liaison. |
| Jointure | id_web sans correspondance Web produit | 20 | Vérifier les produits supprimés, archivés ou mal saisis côté WordPress. |
| Prix | Valeurs aberrantes IQR | 31 | Contrôler si ce sont des produits premium ou de réelles erreurs de prix. |
| Marge | Taux de marge négatif | 1 | Contrôler prix d’achat, prix de vente et hypothèse de TVA. |

## 6. Ce qu’il faut retenir pour expliquer le travail

L’ERP a été conservé comme source principale, car il contient le référentiel produit, les prix, les stocks et les prix d’achat.

La table de liaison a servi à relier product_id à id_web/SKU, car les deux systèmes ne parlent pas le même langage de référence.

Le fichier Web a été filtré pour ne garder que les lignes de type product ; les lignes attachment correspondent principalement à des médias et ne doivent pas entrer dans les calculs métier.

Les clés manquantes n’ont pas été fusionnées : elles ont été isolées pour éviter de créer de faux rapprochements.

Les anomalies n’ont pas été supprimées au hasard. Elles ont été détectées, documentées, puis traitées selon une règle : corriger si la règle métier est sûre, sinon isoler et demander validation.

Le résultat de la phase 1 est une base consolidée et contrôlée, prête pour les analyses de chiffre d’affaires, de stocks, de marges et de corrélations.

| Phrase utile pour la soutenance<br>J’ai d’abord sécurisé les données avant de les analyser : j’ai contrôlé les clés, exclu les lignes non exploitables, rapproché l’ERP et le Web via la table de liaison, puis documenté les anomalies pour éviter que les résultats métier soient faussés. |
| --- |

## 7. Solutions proposées pour améliorer les systèmes

| Axe | Recommandation | Impact attendu |
| --- | --- | --- |
| Référentiel produit | Centraliser les informations produit dans une source fiable et unique. | Réduction des contradictions entre ERP et Web. |
| Clés de jointure | Rendre obligatoire une clé commune ou automatiser la table de liaison. | Jointures plus fiables et moins de produits non rapprochés. |
| Prix de vente | Bloquer les prix absents, nuls ou négatifs. | CA calculé plus fiable. |
| Prix d’achat | Rendre le prix d’achat obligatoire. | Marge calculable pour tous les produits. |
| Stock | Empêcher les stocks négatifs non justifiés et suivre les mouvements. | Meilleure fiabilité de la disponibilité. |
| Statut de stock | Calculer automatiquement instock/outofstock depuis la quantité. | Moins d’écarts entre statut affiché et stock réel. |
| Ventes Web | Contrôler l’unicité du SKU et exclure les lignes non produit. | Analyses de ventes plus propres. |
| Qualité des données | Mettre en place un tableau de bord d’anomalies historisé. | Suivi continu et amélioration progressive. |
| RGPD | Limiter les exports aux données utiles et vérifier l’absence de données personnelles. | Réduction du risque réglementaire. |

## 8. Conclusion de la phase 1

La phase 1 est réalisée : les fichiers ERP, Web et Liaison ont été chargés, contrôlés, préparés et rapprochés.

La base consolidée obtenue contient 714 produits exploitables pour les analyses métier.

Les erreurs et points de vigilance sont documentés dans un journal d’anomalies, avec des actions recommandées.

Le travail permet de passer d’exports dispersés à une base contrôlée, plus fiable et exploitable pour le CODIR.

| À retenir en priorité<br>Le plus important n’est pas seulement la jointure réussie : c’est la traçabilité des choix. Le notebook montre quelles données sont intégrées, lesquelles sont exclues, pourquoi elles le sont, et quelles corrections l’entreprise doit prévoir dans ses systèmes. |
| --- |

## 9. Checklist rapide pour la soutenance

| Point de contrôle | Statut | Réponse courte |
| --- | --- | --- |
| Rapprochement des fichiers | Oui | ERP + Liaison puis ERP/Liaison + Web via les clés product_id et id_web/SKU. |
| Prévention des fausses jointures | Oui | Les clés manquantes sont exclues avant fusion afin d’éviter les correspondances artificielles. |
| Minimum de 8 anomalies | Oui | Le journal documente plus de 8 contrôles/anomalies avec source, description, volume et action. |
| Solutions système | Oui | Des recommandations ERP sont formulées : clé commune, contrôles de saisie, statut de stock automatique, suivi qualité. |

| Question possible | Réponse synthétique |
| --- | --- |
| Pourquoi ne pas tout corriger directement ? | Parce que certaines anomalies nécessitent une validation métier. On corrige seulement quand la règle est certaine ; sinon on isole et on recommande un contrôle. |
| Pourquoi filtrer les attachments ? | Parce que les attachments correspondent surtout aux médias/images WordPress, pas aux produits vendables. |
| Pourquoi utiliser l’ERP comme base ? | Parce que l’ERP contient le référentiel produit, les prix, le stock et le prix d’achat ; il sert de socle métier. |

Sources de travail : scénario BottleNeck, notebook initial Bell_Steve_1_notebook_062026.ipynb et livrable Bell_Steve_1_notebook_062026.ipynb.
