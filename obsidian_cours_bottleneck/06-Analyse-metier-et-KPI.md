# 06 — Analyse métier et KPI

## Objectif

Transformer la base consolidée en indicateurs utiles pour décider.

## KPI principaux

### Chiffre d'affaires par produit

```python
df_merge["ca"] = df_merge["price"] * df_merge["total_sales"]
```

Interprétation :

- un produit cher mais peu vendu peut générer beaucoup de CA ;
- un produit peu cher mais très vendu peut aussi être stratégique.

### Top 20 des produits en CA

```python
top_ca = df_merge.sort_values("ca", ascending=False).head(20)
```

À afficher avec :

```python
display(top_ca[["product_id", "post_title", "price", "total_sales", "ca"]])
```

### Top 20 des quantités vendues

```python
top_quantite = df_merge.sort_values("total_sales", ascending=False).head(20)
```

### Valeur du stock

```python
df_merge["valeur_stock"] = df_merge["price"] * df_merge["stock_quantity"]
```

Cette mesure aide à identifier :

- l'argent immobilisé ;
- les stocks importants ;
- les produits à surveiller.

### Mois de stock

```python
df_merge["mois_stock"] = df_merge["stock_quantity"] / df_merge["total_sales"].replace(0, np.nan)
df_merge["mois_stock"] = df_merge["mois_stock"].replace([np.inf, -np.inf], np.nan)
```

Interprétation :

- valeur élevée : stock potentiellement dormant ;
- valeur basse : risque de rupture ;
- vente nulle : à traiter séparément.

### Marge et taux de marge

```python
df_merge["marge_unitaire"] = df_merge["price"] - df_merge["purchase_price"]
df_merge["marge_totale"] = df_merge["marge_unitaire"] * df_merge["total_sales"]
df_merge["taux_marge"] = df_merge["marge_unitaire"] / df_merge["purchase_price"]
```

Points de vigilance :

- prix d'achat manquant ;
- prix d'achat nul ;
- marge négative ;
- marge anormalement élevée.

## Analyse des ventes

Questions utiles :

- Quels produits génèrent le plus de CA ?
- Quels produits se vendent le plus en quantité ?
- Quels produits ont un CA nul ?
- Quels produits sont en stock mais ne se vendent pas ?

## Analyse des stocks

Questions utiles :

- Quels produits ont le plus de stock ?
- Quels produits immobilisent le plus de valeur ?
- Quels produits sont en rupture ?
- Quels produits ont un stock incohérent avec leur statut ?

## Analyse des marges

Questions utiles :

- Quels produits sont les plus rentables ?
- Quels produits ont une marge négative ?
- Quels produits ont un fort CA mais une faible marge ?
- Quels produits ont une marge élevée mais peu de ventes ?

## Tableau de synthèse recommandé

```python
colonnes_synthese = [
    "product_id",
    "post_title",
    "price",
    "purchase_price",
    "total_sales",
    "stock_quantity",
    "ca",
    "marge_totale",
    "taux_marge",
    "mois_stock"
]

display(df_merge[colonnes_synthese].head())
```

## Conclusion métier type

Une bonne conclusion ne se contente pas de répéter les chiffres. Elle propose des actions :

- contrôler les prix atypiques ;
- corriger les produits sans liaison web ;
- surveiller les produits à forte valeur de stock ;
- prioriser les produits à fort CA et forte marge ;
- déstocker les produits peu vendus.
