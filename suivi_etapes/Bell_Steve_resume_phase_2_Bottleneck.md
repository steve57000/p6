# Projet BottleNeck

Résumé de la phase 2 — Analyses métier pour le CODIR

Objectif : consolider ce qui était demandé, ce qui a été fait dans le notebook, les résultats clés et les messages à retenir pour la présentation.

| Résumé en une phrase<br>La phase 2 transforme la base consolidée en indicateurs de pilotage : chiffre d’affaires, top références, analyse 20/80, prix atypiques, stocks, marges et corrélations, avec une lecture synthétique adaptée au comité de direction. |
| --- |

| Élément | Détail |
| --- | --- |
| Livrable concerné | Bell_Steve_1_notebook_062026.ipynb |
| Base de départ | Bell_Steve_1_notebook_062026.ipynb |
| Base utilisée | df_analyse : 714 produits consolidés |
| Périmètre de ce document | Phase 2 : analyses, interprétations CODIR et points à retenir |

## 1. Ce que la phase 2 demandait

Dans le scénario, Nicolas demande de produire des analyses utiles pour une présentation au comité de direction. L’objectif n’est pas seulement de calculer des indicateurs, mais de les présenter de manière claire, synthétique et alignée avec les préoccupations de l’entreprise : chiffre d’affaires, stock, ventes, marges et qualité des données.

| N° | Analyse demandée | Ce qu’il fallait produire |
| --- | --- | --- |
| 1 | Chiffre d’affaires | Calculer le CA par produit et le CA total général. |
| 2 | Top références | Identifier les produits qui contribuent le plus au CA et aux quantités vendues. |
| 3 | Analyse 20/80 | Vérifier si une petite partie des produits génère l’essentiel du CA ou des ventes. |
| 4 | Valeurs aberrantes | Détecter les prix atypiques avec Z-score et/ou écart interquartile, puis les visualiser avec un boxplot. |
| 5 | Stocks | Analyser l’état du stock, la valorisation, les stocks dormants et les mois de stock. |
| 6 | Marges | Calculer le prix HT, la marge unitaire, le taux de marge et repérer les marges négatives. |
| 7 | Corrélations | Mesurer les liens statistiques entre prix, prix d’achat, stock, ventes, CA, prix HT et taux de marge. |

| Point clé à retenir<br>La phase 2 doit rester CODIR-compatible : peu de technique dans le discours, mais des résultats fiables, interprétés et reliés à des décisions concrètes. |
| --- |

## 2. Base d’analyse utilisée

| Contrôle | Résultat | Interprétation |
| --- | --- | --- |
| Produits consolidés | 714 | Produits présents à la fois dans l’ERP, la liaison et les lignes Web de type product. |
| Prix valides | 714 | Tous les produits de la base consolidée ont un prix exploitable pour les analyses. |
| Ventes valides | 714 | Les quantités vendues sont exploitables sur le périmètre analysé. |
| Stocks valides | 713 | Un produit présente un stock non exploitable ou incohérent pour certains calculs. |
| Lignes exploitables pour le CA | 714 | Aucune ligne exclue du calcul du chiffre d’affaires pour prix ou ventes invalides. |
| Période des ventes | Octobre | Les ventes correspondent au mois d’octobre ; l’extraction est datée au 31 octobre. |

| Décision méthodologique<br>Les analyses de phase 2 reposent uniquement sur la base consolidée et contrôlée. Cela évite de calculer des indicateurs sur des lignes Web non produit, des clés manquantes ou des jointures incertaines. |
| --- |

## 3. Chiffre d’affaires et top références

| Indicateur | Résultat | Lecture CODIR |
| --- | --- | --- |
| CA total Web | 143 680,10 EUR | Montant total calculé avec CA = prix × quantité vendue. |
| Produits avec CA positif | 689 | Produits ayant au moins une vente sur la période. |
| Produits sans CA positif | 25 | Produits sans vente ou sans contribution au CA sur octobre. |
| Lignes exclues du CA | 0 | Aucune exclusion liée à un prix ou une vente invalide dans la base consolidée. |

| Top CA | Prix | Ventes | CA |
| --- | --- | --- | --- |
| Champagne Egly-Ouriet Grand Cru Millésimé 2008 | 225,00 EUR | 11 | 2 475,00 EUR |
| Coteaux Champenois Egly-Ouriet Ambonnay Rouge 2016 | 191,30 EUR | 6 | 1 147,80 EUR |
| Champagne Egly-Ouriet Grand Cru Brut Rosé | 79,50 EUR | 14 | 1 113,00 EUR |
| Agnès Levet Côte Rôtie Améthyste 2017 | 41,20 EUR | 20 | 824,00 EUR |
| Domaine des Comtes Lafon Volnay 1er Cru Santenots du Milieu 2015 | 115,00 EUR | 7 | 805,00 EUR |

| Interprétation<br>Les meilleures références en chiffre d’affaires combinent soit un prix élevé avec peu de ventes, soit un prix moyen avec un volume plus important. Le CA ne doit donc pas être lu uniquement comme un indicateur de quantité vendue. |
| --- |

## 4. Analyse 20/80 et ventes en quantité

| Analyse | Résultat | Lecture CODIR |
| --- | --- | --- |
| 20/80 du CA | 435 produits | 63,13 % des produits avec CA positif génèrent environ 80,09 % du CA. |
| 20/80 des quantités | 434 produits | 62,99 % des produits avec ventes positives génèrent environ 80,11 % des quantités vendues. |
| Conclusion | Portefeuille diffus | On n’observe pas un vrai effet 20/80 strict : l’activité est répartie sur une large partie du catalogue. |

| Top quantités vendues | Ventes | CA |
| --- | --- | --- |
| Château De La Selve IGP Coteaux de l’Ardèche Maguelonne Rosé 2019 | 36 | 356,40 EUR |
| Mas Laval IGP Pays d’Hérault Les Pampres Blancs 2018 | 27 | 267,30 EUR |
| I Fabbri Chianti Classico Lamole 2017 | 24 | 357,60 EUR |
| Bernard Baudry Chinon Rouge La Croix Boissée 2017 | 22 | 627,00 EUR |
| François Baur Pinot Noir Schlittweg 2017 | 22 | 279,40 EUR |

| Message à retenir<br>Les références les plus vendues ne sont pas forcément celles qui génèrent le plus de chiffre d’affaires. Pour piloter l’activité, il faut suivre deux vues : les produits qui créent du volume et ceux qui créent de la valeur. |
| --- |

## 5. Prix atypiques, Z-score, IQR et boxplot

| Contrôle prix | Résultat | Interprétation |
| --- | --- | --- |
| Prix moyen | 32,33 EUR | Prix moyen du catalogue consolidé. |
| Prix médian | 23,45 EUR | La moitié des produits coûte moins de 23,45 EUR. |
| Prix minimum / maximum | 5,20 EUR / 225,00 EUR | Amplitude importante, cohérente avec un catalogue de vins et spiritueux. |
| Seuil haut IQR | 84,09 EUR | Au-dessus de ce seuil, les produits sont considérés comme atypiques selon l’IQR. |
| Outliers IQR | 31 produits | Environ 4,34 % du catalogue consolidé. |
| Outliers Z-score | 13 produits | Produits très éloignés de la moyenne, à contrôler en priorité. |

| Exemples de prix atypiques | Prix | Contrôle |
| --- | --- | --- |
| Champagne Egly-Ouriet Grand Cru Millésimé 2008 | 225,00 EUR | Z-score 6,98 |
| David Duband Charmes-Chambertin Grand Cru 2014 | 217,50 EUR | Z-score 6,71 |
| Coteaux Champenois Egly-Ouriet Ambonnay Rouge 2016 | 191,30 EUR | Z-score 5,76 |
| Cognac Frapin VIP XO | 176,00 EUR | Z-score 5,21 |

| Conclusion sur les prix<br>Un prix atypique n’est pas automatiquement une erreur. Plusieurs outliers semblent correspondre à des produits premium. En revanche, les prix atypiques doivent être contrôlés, surtout lorsqu’ils créent une marge négative ou une incohérence avec le prix d’achat. |
| --- |

## 6. Stocks, valorisation et mois de stock

| Indicateur stock | Résultat | Lecture CODIR |
| --- | --- | --- |
| Nombre total d’unités en stock | 16 739 | Volume physique disponible sur la base consolidée. |
| Valorisation au prix d’achat | 277 305,77 EUR | Valeur estimée du stock au coût d’acquisition. |
| Valorisation au prix de vente TTC | 494 593,40 EUR | Valeur commerciale théorique si tout le stock était vendu. |
| Mois de stock calculables | 688 produits | Calcul fait seulement quand les ventes du mois sont positives. |
| Produits exclus du calcul des mois de stock | 26 | Produits sans vente positive ou avec données non exploitables. |
| Stock dormant potentiel | 3 produits | Produits sans vente mais avec stock disponible. |

| Produits avec beaucoup de mois de stock | Stock | Ventes | Mois |
| --- | --- | --- | --- |
| Champagne Gosset Grand Millésime 2006 | 125 | 4 | 31,25 mois |
| Champagne Gosset Célébris Vintage 2007 | 138 | 5 | 27,60 mois |
| Champagne Egly-Ouriet Premier Cru Les Vignes de Vrigny | 81 | 3 | 27,00 mois |
| Champagne Egly-Ouriet Grand Cru Brut Tradition | 125 | 5 | 25,00 mois |

| Interprétation stock<br>Les mois de stock permettent d’identifier les références qui immobilisent du capital. Les stocks élevés ne sont pas forcément problématiques, mais ils doivent être comparés aux ventes, au prix d’achat et à la stratégie commerciale. |
| --- |

## 7. Marges et taux de marge

| Indicateur marge | Résultat | Interprétation |
| --- | --- | --- |
| Hypothèse de calcul | TVA 20 % | Prix HT = prix TTC / 1,20. |
| Lignes avec marge calculable | 714 | Tous les produits consolidés ont les données nécessaires au calcul de marge. |
| Taux de marge moyen | 61,02 % | Marge moyenne calculée sur les produits exploitables. |
| Taux de marge minimum / maximum | -86,39 % / 91,41 % | Un minimum négatif indique une anomalie à traiter. |
| Marge totale estimée | 44 660,65 EUR | Marge estimée sur les ventes de la période. |
| Produits avec marge négative | 1 | Produit à contrôler en priorité. |

| Marge négative détectée | Prix TTC | Prix HT | Prix achat | Taux marge |
| --- | --- | --- | --- | --- |
| Champagne Egly-Ouriet Grand Cru Blanc de Noirs | 12,65 EUR | 10,54 EUR | 77,48 EUR | -86,39 % |

| Conclusion sur la marge<br>Le calcul des marges apporte une alerte forte : au moins un produit semble vendu à un prix incohérent par rapport à son prix d’achat. Ce type de contrôle doit être automatisé dans l’ERP avant validation des prix. |
| --- |

## 8. Corrélations entre variables quantitatives

| Relation observée | Corrélation | Lecture |
| --- | --- | --- |
| Prix / Prix HT | 1,00 | Corrélation mécanique, car le prix HT est calculé à partir du prix TTC. |
| Prix / Prix d’achat | 0,98 | Les produits les plus chers ont globalement un prix d’achat plus élevé. |
| Prix / Marge unitaire | 0,94 | Les produits chers dégagent souvent une marge unitaire plus forte. |
| Stock / Mois de stock | 0,76 | Un stock élevé augmente mécaniquement le nombre de mois de stock si les ventes ne suivent pas. |
| Stock / Valorisation stock achat | 0,72 | Plus le stock est élevé, plus la valeur immobilisée augmente. |
| Quantités vendues / Prix | -0,52 | Les produits plus chers semblent moins vendus en volume, à interpréter avec prudence. |

| Prudence d’interprétation<br>Une corrélation indique une relation statistique, pas une causalité. Elle aide à orienter les questions métier, mais elle ne suffit pas à prouver qu’une variable explique directement une autre. |
| --- |

## 9. Analyses complémentaires et décisions possibles

| Axe | Action recommandée | Impact attendu |
| --- | --- | --- |
| Pilotage CA | Suivre séparément top CA et top quantités. | Distinguer les produits qui créent de la valeur et ceux qui créent du volume. |
| Prix atypiques | Créer une alerte sur les prix au-dessus des seuils IQR/Z-score. | Contrôler les erreurs de saisie sans bloquer les produits premium. |
| Marge | Bloquer ou valider manuellement les produits à marge négative. | Éviter de vendre à perte à cause d’une erreur de prix. |
| Stock | Suivre les mois de stock et les stocks dormants. | Réduire l’immobilisation financière et prioriser les actions commerciales. |
| ERP | Automatiser les contrôles prix, stock, marge et clé de jointure. | Fiabiliser les analyses futures et préparer un tableau de bord de datavisualisation. |

## 10. Ce qu’il faut retenir pour la soutenance

Le CA total calculé est de 143 680,10 EUR sur le périmètre Web consolidé.

Le catalogue ne présente pas un effet 20/80 strict : environ 63 % des références avec CA positif contribuent à 80 % du CA.

Les prix atypiques ne sont pas tous des erreurs : plusieurs correspondent probablement à des vins ou spiritueux premium.

La marge négative détectée est l’alerte métier la plus importante : elle doit être vérifiée dans l’ERP.

Les stocks doivent être suivis avec les mois de stock, car un stock élevé sans ventes rapides immobilise du capital.

Les corrélations aident à orienter l’analyse, mais ne doivent pas être présentées comme des relations de cause à effet.

| Phrase utile pour le CODIR<br>Après avoir fiabilisé les données, j’ai calculé les indicateurs de pilotage prioritaires : chiffre d’affaires, produits stratégiques, prix atypiques, stocks, marges et corrélations. Les résultats montrent un catalogue assez diffus, quelques prix premium à contrôler, une alerte sur une marge négative et des pistes concrètes pour améliorer le suivi ERP. |
| --- |

| Point demandé | Statut | Réponse courte |
| --- | --- | --- |
| CA par produit et CA total | Oui | CA total : 143 680,10 EUR. |
| Top références | Oui | Top CA et top quantités produits dans le notebook. |
| Analyse 20/80 | Oui | CA et quantités analysés : concentration moins forte qu’un 20/80 classique. |
| Valeurs aberrantes prix | Oui | Z-score, IQR et boxplot utilisés. |
| Stocks / mois de stock | Oui | Valorisation, stock dormant et mois de stock calculés. |
| Marges | Oui | Prix HT, marge unitaire, taux de marge et marge totale estimée. |
| Corrélations | Oui | Matrice et relations fortes extraites. |

Sources de travail : scénario BottleNeck, notebook initial Bell_Steve_1_notebook_062026.ipynb, livrable Bell_Steve_1_notebook_062026.ipynb et fichiers ERP/Web/Liaison.
