# 04 — Contrôle qualité et nettoyage

## Objectif

Avant de calculer des KPI, il faut vérifier que les données sont fiables.

Un bon contrôle qualité répond à quatre questions :

1. Y a-t-il des valeurs manquantes ?
2. Y a-t-il des doublons ?
3. Les types de données sont-ils corrects ?
4. Les règles métier sont-elles respectées ?

## Valeurs manquantes

```python
valeurs_manquantes = df_erp.isna().sum().reset_index()
valeurs_manquantes.columns = ["Colonne", "Valeurs manquantes"]
display(valeurs_manquantes)
```

Interprétation :

- une valeur manquante dans une description peut être acceptable ;
- une valeur manquante dans une clé de jointure est critique ;
- une valeur manquante dans un prix doit être investiguée.

## Doublons

### Doublons de lignes complètes

```python
nb_lignes_dupliquees = df_erp.duplicated().sum()
print(nb_lignes_dupliquees)
```

### Doublons sur une clé

```python
nb_doublons_product_id = df_erp["product_id"].duplicated().sum()
print(nb_doublons_product_id)
```

Une clé primaire doit être unique. Si `product_id` est censé identifier un produit, un doublon est une anomalie.

## Contrôle des types

```python
df_erp.dtypes
```

Conversion sécurisée :

```python
df_erp_clean = df_erp.copy()
df_erp_clean["product_id"] = pd.to_numeric(df_erp_clean["product_id"], errors="coerce")
df_erp_clean["price"] = pd.to_numeric(df_erp_clean["price"], errors="coerce")
```

`errors="coerce"` transforme les valeurs impossibles en `NaN`, ce qui permet de les repérer.

## Nettoyer une clé de jointure

Une clé doit être comparable entre fichiers. Exemple : `123`, `123.0` et `"123 "` doivent devenir la même valeur.

```python
def nettoyer_cle(valeur):
    if pd.isna(valeur):
        return pd.NA
    texte = str(valeur).strip()
    if texte.endswith(".0"):
        texte = texte[:-2]
    return texte
```

Utilisation :

```python
df_erp_clean["product_id_key"] = df_erp_clean["product_id"].apply(nettoyer_cle)
df_liaison_clean["product_id_key"] = df_liaison_clean["product_id"].apply(nettoyer_cle)
df_liaison_clean["id_web_key"] = df_liaison_clean["id_web"].apply(nettoyer_cle)
df_web_clean["sku_key"] = df_web_clean["sku"].apply(nettoyer_cle)
```

## Contrôle métier : stock

Règle :

- si `stock_quantity > 0`, alors le statut attendu est `instock` ;
- si `stock_quantity <= 0`, alors le statut attendu est `outofstock`.

```python
df_erp_clean["stock_status_attendu"] = np.where(
    df_erp_clean["stock_quantity"] > 0,
    "instock",
    "outofstock"
)

anomalies_stock = df_erp_clean[
    df_erp_clean["stock_status"] != df_erp_clean["stock_status_attendu"]
]
```

## Contrôle métier : prix

```python
anomalies_prix = df_erp_clean[
    df_erp_clean["price"].isna()
    | (df_erp_clean["price"] <= 0)
]
```

Prix à contrôler :

- prix manquant ;
- prix nul ;
- prix négatif ;
- prix très élevé ;
- prix inférieur au prix d'achat.

## Contrôle métier : `onsale_web`

```python
valeurs_attendues = {0, 1}
valeurs_observees = set(df_erp_clean["onsale_web"].dropna().unique())
valeurs_inattendues = valeurs_observees - valeurs_attendues
```

## Bonne pratique

Ne corrige pas silencieusement. Chaque anomalie doit être :

- identifiée ;
- affichée ;
- expliquée ;
- éventuellement corrigée ;
- documentée.
