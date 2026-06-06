# Fichiers Power BI — Cas d'Usage 1 (Cours)

## Fichiers .pbix disponibles

Le cours Power BI est organisé en plusieurs fichiers `.pbix` qui représentent la progression pédagogique étape par étape :

| Fichier | Étape | Contenu |
|---------|-------|---------|
| `Demo_datascientest.pbix` | Étape 0 | Données brutes importées, aucune transformation |
| `Demo_datascientest_model.pbix` | Étape 1 | Modèle de données en étoile construit |
| `Demo_datascientest_model_dax.pbix` | Étape 2 | Mesures DAX ajoutées |
| `Demo_datascientest_model_dax_viz.pbix` | Étape 3 | Visualisations créées |
| `demo1 PowerBI.pbix` | Étape finale | Dashboard complet du cours |

> **Note :** Les fichiers `.pbix` ne sont pas versionnés dans ce dépôt GitHub car leur taille dépasse les recommandations Git standard. Ils sont disponibles sur demande directe ou via Git LFS (Large File Storage).

## Comment ouvrir un fichier .pbix

### Prérequis

- **Power BI Desktop** installé (gratuit) — [Télécharger ici](https://powerbi.microsoft.com/fr-fr/desktop/)
- Version recommandée : juillet 2024 ou plus récente

### Étapes

1. Lancer Power BI Desktop
2. Fichier → Ouvrir un rapport → Naviguer vers le fichier `.pbix`
3. Si une erreur de source de données apparaît :
   - Aller dans `Transformer les données` (menu Accueil)
   - Dans Power Query, cliquer sur `Source` dans les étapes appliquées
   - Mettre à jour le chemin vers les fichiers CSV locaux

### Mise à jour des sources de données (si nécessaire)

Les chemins des fichiers CSV sont stockés dans Power Query. Pour les mettre à jour :

```
Accueil → Transformer les données → Power Query Editor
→ Requête F_Appels → Étapes appliquées → Source
→ Modifier le chemin vers vos fichiers locaux
```

## Progression pédagogique recommandée

Si vous souhaitez apprendre Power BI à travers ce projet :

1. **Commencer** par `Demo_datascientest.pbix` — observer les données brutes
2. **Construire le modèle** en s'inspirant du fichier `_model`
3. **Ajouter les mesures DAX** en consultant `dax/mesures_dax.md`
4. **Créer les visualisations** page par page en suivant `docs/architecture_modele.md`
5. **Valider** en comparant votre résultat avec `demo1 PowerBI.pbix`

---

*Emmanuel TSAGUE — Data Scientist / Data Analyst — Formation DataScientest 2024*
