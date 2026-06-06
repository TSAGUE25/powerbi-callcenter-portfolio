# Fichier Power BI — Cas d'Usage 2 (Examen)

## Fichier d'examen

| Fichier | Description |
|---------|-------------|
| `Exam_PBI_Call_Center TSAGUE EMMANUEL.pbix` | Tableau de bord complet — soumis comme examen certifiant |

Ce fichier représente la réalisation autonome d'un tableau de bord Power BI professionnel dans le cadre de l'examen final de la formation DataScientest.

> **Note :** Le fichier `.pbix` n'est pas versionné dans ce dépôt (taille binaire). Il est disponible sur demande directe.

## Ce que contient ce fichier

### Power Query (Transformation des données)
- Importation des CSV 2018, 2019, 2020
- Importation du fichier Excel 2021 (format différent)
- Fusion en une table unique de 132 000 lignes
- Nettoyage, typage et enrichissement des colonnes
- Colonne `Source_Fichier` pour la traçabilité

### Modèle de données
- `F_Appels` — table de faits (132 000 lignes)
- `D_Date` — table de calendrier 2018–2021 (créée en DAX)
- `D_Agents` — dimension agents
- `D_TypeAppel` — dimension types d'appels
- `D_Charges` — dimension tarification

### Mesures DAX (18 mesures)
Voir le fichier détaillé : [`dax/mesures_dax.md`](../dax/mesures_dax.md)

### Pages du tableau de bord (6 pages)
1. Synthèse Exécutive 4 ans
2. Analyse Temporelle Avancée
3. Performance des Agents (multi-années)
4. Qualité de Service et Risques
5. Analyse des Coûts Opérationnels
6. Plan d'Action 2022

## Comment ouvrir le fichier

### Prérequis

- **Power BI Desktop** installé (gratuit)
- Les fichiers de données dans le même répertoire relatif que le `.pbix`

### Si les données ne se chargent pas

1. `Accueil → Transformer les données`
2. Dans Power Query, repérer chaque source et mettre à jour le chemin
3. Les 3 sources CSV (2018-2020) et 1 source Excel (2021) doivent être accessibles

### Structure des données attendue

```
data/
├── Call_Center_Data_2018.csv
├── Call_Center_Data_2019.csv
├── Call_Center_Data_2020.csv
├── Call_Center_Data_2021.xlsx
├── Lookup_Tables.xlsx
└── Call_Charges.xlsx
```

---

*Emmanuel TSAGUE — Data Scientist / Data Analyst — Examen DataScientest 2024*
