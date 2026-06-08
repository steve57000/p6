# 07 — Statistiques et outliers

## Objectif

Repérer les valeurs atypiques sans les supprimer automatiquement.

Une valeur atypique peut être :

- une erreur de saisie ;
- un produit premium ;
- une donnée rare mais correcte ;
- un cas métier à investiguer.

## Méthode 1 — Z-score

Le Z-score mesure l'écart d'une valeur par rapport à la moyenne en nombre d'écarts-types.

Formule :

```text
z = (valeur - moyenne) / écart-type
```

Code :

```python
moyenne_prix = df_merge["price"].mean()
ecart_type_prix = df_merge["price"].std()

df_merge["z_score_prix"] = (df_merge["price"] - moyenne_prix) / ecart_type_prix

outliers_z = df_merge[df_merge["z_score_prix"].abs() > 3]
```

Interprétation :

- `abs(z) > 2` : valeur à surveiller ;
- `abs(z) > 3` : valeur fortement atypique.

Limite : le Z-score est sensible aux valeurs extrêmes et suppose une distribution plutôt normale.

## Méthode 2 — Intervalle interquartile

Cette méthode utilise les quartiles.

```python
q1 = df_merge["price"].quantile(0.25)
q3 = df_merge["price"].quantile(0.75)
iqr = q3 - q1

borne_basse = q1 - 1.5 * iqr
borne_haute = q3 + 1.5 * iqr

outliers_iqr = df_merge[
    (df_merge["price"] < borne_basse)
    | (df_merge["price"] > borne_haute)
]
```

Interprétation :

- robuste pour les distributions asymétriques ;
- souvent plus adaptée aux prix ;
- ne dit pas si la valeur est fausse, seulement atypique.

## Visualiser les outliers

### Boxplot

```python
sns.boxplot(data=df_merge, x="price")
plt.title("Distribution des prix")
plt.show()
```

### Histogramme

```python
sns.histplot(data=df_merge, x="price", bins=30, kde=True)
plt.title("Histogramme des prix")
plt.show()
```

## Comparer les méthodes

```python
comparaison_outliers = pd.DataFrame({
    "Méthode": ["Z-score", "IQR"],
    "Nombre d'outliers": [len(outliers_z), len(outliers_iqr)]
})
```

## Ce qu'il faut écrire dans le notebook

Pour chaque méthode, ajoute une interprétation :

```markdown
La méthode IQR identifie plusieurs prix atypiques. Ces valeurs ne doivent pas être supprimées automatiquement, car elles peuvent correspondre à des bouteilles premium. Elles doivent être vérifiées avec le métier.
```

## Erreur à éviter

Ne jamais écrire :

> Ces valeurs sont fausses car elles sont atypiques.

Écris plutôt :

> Ces valeurs sont atypiques statistiquement et nécessitent une vérification métier.
