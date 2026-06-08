# 05 — Jointures ERP, Web et Liaison

## Objectif

Construire une base consolidée en reliant :

- l'ERP, qui contient les informations produit internes ;
- la table de liaison, qui associe `product_id` et `id_web` ;
- le fichier Web, qui contient les ventes et informations du site.

## Pourquoi utiliser une table de liaison ?

Les fichiers n'utilisent pas toujours la même clé :

- ERP : `product_id` ;
- Web : `sku` ;
- Liaison : `product_id` + `id_web`.

La table de liaison sert de pont.

## Étape 1 — Préparer les clés

```python
df_erp_clean["product_id_key"] = df_erp_clean["product_id"].apply(nettoyer_cle)
df_liaison_clean["product_id_key"] = df_liaison_clean["product_id"].apply(nettoyer_cle)
df_liaison_clean["id_web_key"] = df_liaison_clean["id_web"].apply(nettoyer_cle)
df_web_clean["sku_key"] = df_web_clean["sku"].apply(nettoyer_cle)
```

## Étape 2 — Filtrer les lignes web utiles

Dans un export WordPress/WooCommerce, certaines lignes peuvent être des images ou pièces jointes. Il faut conserver les vrais produits.

```python
df_web_clean = df_web[df_web["post_type"] == "product"].copy()
```

## Étape 3 — Fusion ERP + Liaison

```python
df_merge_erp_liaison = pd.merge(
    df_erp_clean,
    df_liaison_clean[["product_id_key", "id_web", "id_web_key"]],
    on="product_id_key",
    how="left",
    indicator="merge_erp_liaison"
)
```

Pourquoi `how="left"` ?

On veut garder tous les produits ERP, même ceux sans correspondance web.

Pourquoi `indicator=True` ou un nom d'indicateur ?

Cela ajoute une colonne permettant d'identifier :

- les lignes trouvées dans les deux tables ;
- les lignes seulement présentes à gauche ;
- les lignes seulement présentes à droite.

## Étape 4 — Contrôler les pertes

```python
anomalies_erp_sans_liaison = df_merge_erp_liaison[
    df_merge_erp_liaison["merge_erp_liaison"] == "left_only"
]
```

Si ce tableau n'est pas vide, certains produits ERP n'ont pas de correspondance dans la table de liaison.

## Étape 5 — Fusion avec Web

```python
df_merge = pd.merge(
    df_merge_erp_liaison,
    df_web_clean,
    left_on="id_web_key",
    right_on="sku_key",
    how="left",
    indicator="merge_web"
)
```

## Étape 6 — Contrôler la base consolidée

```python
print(df_merge.shape)
print(df_merge["merge_web"].value_counts(dropna=False))
```

Contrôles importants :

- nombre de lignes avant/après ;
- nombre de produits sans correspondance web ;
- doublons après jointure ;
- clés manquantes ;
- cohérence des ventes.

## Erreurs fréquentes

### Faire une jointure sur des types différents

Exemple : `product_id` numérique dans un fichier et texte dans un autre.

Solution : créer des clés nettoyées en texte.

### Faire une jointure interne trop tôt

Une jointure `inner` peut supprimer des lignes sans que tu t'en rendes compte.

### Ne pas vérifier les doublons

Si la clé n'est pas unique, une jointure peut multiplier les lignes.

## Méthode pro

Avant chaque jointure :

- vérifier l'unicité de la clé gauche ;
- vérifier l'unicité de la clé droite ;
- vérifier les valeurs manquantes ;
- faire la jointure avec indicateur ;
- analyser les lignes non appariées.
