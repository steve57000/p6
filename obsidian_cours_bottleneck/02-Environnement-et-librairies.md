# 02 — Environnement et librairies

## Librairies principales

### `pathlib`

Permet de gérer les chemins de fichiers proprement.

```python
from pathlib import Path

DATA_DIR = Path("./Data+Bottleneck")
fichier_erp = DATA_DIR / "erp.xlsx"
```

Pourquoi c'est pro :

- fonctionne mieux que les chaînes de caractères ;
- rend le code plus lisible ;
- évite les erreurs de `/` ou `\\` selon le système.

### `pandas`

La librairie centrale pour manipuler des tableaux de données.

```python
import pandas as pd

df_erp = pd.read_excel(DATA_DIR / "erp.xlsx")
```

Fonctions indispensables :

| Fonction | Utilité |
|---|---|
| `read_excel()` | Lire un fichier Excel |
| `head()` | Afficher les premières lignes |
| `info()` | Voir types et valeurs non nulles |
| `describe()` | Statistiques descriptives |
| `isna()` | Détecter les valeurs manquantes |
| `duplicated()` | Détecter les doublons |
| `merge()` | Fusionner des tables |
| `groupby()` | Agréger des données |
| `sort_values()` | Trier |
| `to_excel()` | Exporter vers Excel |

### `numpy`

Utile pour les calculs numériques et les valeurs spéciales.

```python
import numpy as np

df["rotation_stock"] = df["stock_quantity"] / df["total_sales"].replace(0, np.nan)
```

Cas fréquents :

- remplacer les divisions par zéro ;
- manipuler `np.nan` ;
- calculer des conditions vectorisées avec `np.where()`.

### `matplotlib`

Base classique pour créer des graphiques.

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
plt.bar(df["post_title"], df["ca"])
plt.xticks(rotation=45, ha="right")
plt.show()
```

### `seaborn`

Surcouche de Matplotlib, plus lisible pour les graphiques statistiques.

```python
import seaborn as sns

sns.boxplot(data=df, x="price")
```

Utile pour :

- boxplots ;
- histogrammes ;
- heatmaps de corrélation ;
- nuages de points.

### `IPython.display`

Permet d'afficher du HTML, du Markdown ou des DataFrames dans un notebook.

```python
from IPython.display import display, HTML, Markdown

display(HTML("<h2>Résultat</h2>"))
```

## Librairies optionnelles

### `plotly.express`

Très utile pour des graphiques interactifs.

```python
import plotly.express as px

fig = px.bar(df_top, x="post_title", y="ca")
fig.show()
```

### `warnings`

Permet de masquer certains avertissements non bloquants.

```python
import warnings
warnings.filterwarnings("ignore")
```

À utiliser avec prudence : ne masque pas les erreurs importantes.

## Configuration recommandée

```python
pd.set_option("display.max_columns", None)
pd.set_option("display.max_rows", 100)
pd.set_option("display.float_format", lambda x: f"{x:,.2f}")

plt.rcParams["figure.figsize"] = (12, 6)
sns.set_theme(style="whitegrid")
```

## Bonnes pratiques

- Imports regroupés au début.
- Pas de `try/except` autour des imports.
- Options d'affichage définies une fois.
- Chemins centralisés dans des variables.
- Commentaires courts mais utiles.
