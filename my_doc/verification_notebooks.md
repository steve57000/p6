# Vérification de cohérence des notebooks

- Notebook origine : `doc_fourni/Template-Notebook-Bottleneck.ipynb`
- Notebook final : `doc_fourni/Template-Notebook-Bottleneck_COMPLET_DESIGN_NAVIGATEUR.ipynb`

## Résultat automatique (similarité de cellules)

- Cellules origine analysées : 95
- Cellules avec similarité >= 0.55 dans le notebook final : 8
- Taux de couverture estimé : 8.4%

> Méthode : comparaison de similarité textuelle (SequenceMatcher) de chaque cellule de l'origine contre toutes les cellules du final.
> Interprétation : un score faible peut provenir d'un enrichissement visuel important, même si le fond analytique est présent.

## Action ajoutée

- Ajout d'une section *Navigateur web des exports* dans le notebook final pour générer `exports/tableaux/index.html`, permettant de consulter tous les tableaux depuis une seule page.
