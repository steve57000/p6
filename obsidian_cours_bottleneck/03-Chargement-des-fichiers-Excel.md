# 03 — Chargement des fichiers Excel

## Objectif

Transformer les fichiers Excel du projet en DataFrames Pandas exploitables.

Dans le projet BottleNeck, les trois sources principales sont :

- `erp.xlsx` : données internes, prix, stock, ventes web ou non ;
- `web.xlsx` : données issues du site web ;
- `liaison.xlsx` : table de correspondance entre ERP et Web.

## Code de base

```python
from pathlib import Path
import pandas as pd

DATA_DIR = Path("./Data+Bottleneck")

df_erp = pd.read_excel(DATA_DIR / "erp.xlsx")
df_web = pd.read_excel(DATA_DIR / "web.xlsx")
df_liaison = pd.read_excel(DATA_DIR / "liaison.xlsx")
```

## Contrôle immédiat après chargement

Après chaque import, vérifie :

```python
print(df_erp.shape)
print(df_web.shape)
print(df_liaison.shape)
```

Puis affiche les premières lignes :

```python
display(df_erp.head())
display(df_web.head())
display(df_liaison.head())
```

## Fonction pro : résumé des dimensions

```python
def resume_dimensions(df, nom_fichier):
    return pd.DataFrame({
        "Fichier": [nom_fichier],
        "Nombre de lignes": [df.shape[0]],
        "Nombre de colonnes": [df.shape[1]]
    })
```

Utilisation :

```python
display(resume_dimensions(df_erp, "erp.xlsx"))
```

## Fonction pro : synthèse d'un DataFrame

```python
def synthese_dataframe(df):
    return pd.DataFrame({
        "Colonne": df.columns,
        "Type": df.dtypes.astype(str).values,
        "Valeurs non nulles": df.notna().sum().values,
        "Valeurs manquantes": df.isna().sum().values,
        "Valeurs uniques": df.nunique(dropna=True).values
    })
```

Cette fonction donne une vision rapide de la qualité du fichier.

## Checklist de chargement

- [ ] Le dossier de données existe.
- [ ] Les trois fichiers sont bien présents.
- [ ] Les dimensions sont cohérentes.
- [ ] Les colonnes attendues existent.
- [ ] Les types de colonnes sont compris.
- [ ] Les premières lignes semblent plausibles.

## Erreurs fréquentes

### Mauvais chemin

```python
FileNotFoundError: No such file or directory
```

Solution : vérifier `DATA_DIR` et le nom exact du fichier.

### Colonnes mal nommées

Excel peut contenir des espaces invisibles ou des accents. Pour nettoyer :

```python
df.columns = df.columns.str.strip()
```

### Mauvais type de données

Un identifiant peut être lu comme nombre flottant. Pour une clé de jointure, il faut normaliser avant de fusionner.
