# 08 — Visualisations et exports

## Objectif

Rendre les résultats lisibles et partageables.

Un notebook professionnel ne se limite pas à afficher des tableaux bruts. Il doit proposer :

- des tableaux formatés ;
- des graphiques clairs ;
- des exports HTML ;
- un export Excel final ;
- une page d'index pour naviguer dans les résultats.

## Graphique simple avec Matplotlib

```python
top_ca = df_merge.sort_values("ca", ascending=False).head(20)

plt.figure(figsize=(12, 6))
plt.bar(top_ca["post_title"], top_ca["ca"])
plt.xticks(rotation=45, ha="right")
plt.title("Top 20 des produits par chiffre d'affaires")
plt.ylabel("Chiffre d'affaires")
plt.tight_layout()
plt.show()
```

## Graphique statistique avec Seaborn

```python
sns.scatterplot(data=df_merge, x="price", y="total_sales")
plt.title("Relation entre prix et quantités vendues")
plt.show()
```

## Heatmap de corrélation

```python
colonnes_corr = ["price", "total_sales", "stock_quantity", "ca", "taux_marge"]
correlation = df_merge[colonnes_corr].corr(numeric_only=True)

sns.heatmap(correlation, annot=True, cmap="coolwarm", center=0)
plt.title("Corrélations entre variables")
plt.show()
```

## Export Excel final

```python
EXPORTS_DIR = Path("exports")
EXPORTS_DIR.mkdir(exist_ok=True)

df_merge.to_excel(EXPORTS_DIR / "bottleneck_base_consolidee.xlsx", index=False)
```

## Export HTML d'un tableau

Principe : convertir un DataFrame en HTML.

```python
html_table = df_merge.head(20).to_html(index=False)
Path("exports/tableaux").mkdir(parents=True, exist_ok=True)
Path("exports/tableaux/top_20.html").write_text(html_table, encoding="utf-8")
```

## Fonction pro d'export

```python
def exporter_tableau_html(df, nom_fichier, titre="Tableau exporté"):
    dossier = Path("exports/tableaux")
    dossier.mkdir(parents=True, exist_ok=True)

    contenu = f"""
    <!doctype html>
    <html lang="fr">
    <head>
        <meta charset="utf-8">
        <title>{titre}</title>
        <style>
            body {{ font-family: Arial, sans-serif; margin: 24px; }}
            table {{ border-collapse: collapse; width: 100%; }}
            th, td {{ border: 1px solid #ddd; padding: 8px; }}
            th {{ background: #33A5B6; color: white; }}
        </style>
    </head>
    <body>
        <h1>{titre}</h1>
        {df.to_html(index=False)}
    </body>
    </html>
    """

    chemin = dossier / nom_fichier
    chemin.write_text(contenu, encoding="utf-8")
    return chemin
```

## Index HTML consolidé

L'idée : créer une page `index.html` qui liste tous les tableaux exportés.

```python
html_files = sorted(Path("exports/tableaux").glob("*.html"))

liens = "\n".join(
    f'<li><a href="{p.name}">{p.stem}</a></li>'
    for p in html_files
    if p.name != "index.html"
)

index_html = f"""
<!doctype html>
<html lang="fr">
<head><meta charset="utf-8"><title>Exports BottleNeck</title></head>
<body>
<h1>Exports BottleNeck</h1>
<ul>{liens}</ul>
</body>
</html>
"""

Path("exports/tableaux/index.html").write_text(index_html, encoding="utf-8")
```

## Bonne pratique contre les doublons

Si un tableau est déjà affiché avec une fonction comme `afficher_tableau()`, évite de refaire immédiatement `display(df)` juste après. Sinon, le notebook donne l'impression d'avoir des doublons.

## Checklist visuelle

- [ ] Chaque graphique a un titre.
- [ ] Les axes sont nommés.
- [ ] Les libellés longs sont inclinés.
- [ ] Les tableaux importants sont exportés.
- [ ] Les exports ont des noms explicites.
- [ ] L'index HTML ne liste pas deux fois le même contenu.
