# Schéma des données d'exemple — Cas d'Usage 2 (Examen)

## Fichiers présents dans ce dossier

| Fichier | Lignes | Période | Remarque |
|---------|--------|---------|----------|
| `Call_Center_Data_2018_sample.csv` | 100 lignes | Échantillon 2018 | Format CSV |
| `Call_Center_Data_2019_sample.csv` | 100 lignes | Échantillon 2019 | Format CSV |
| `Call_Center_Data_2020_sample.csv` | 100 lignes | Échantillon 2020 | Format CSV |

> **Note :** Les données 2021 sont au format Excel (`.xlsx`) et ne sont pas incluses dans les échantillons. Le fichier `Call_Center_Data_2021.xlsx` est disponible dans le fichier `.pbix` de l'examen.

## Structure commune aux 4 années

```
CallTimestamp,Call Type,EmployeeID,CallDuration,WaitTime,CallAbandoned
5/4/2018 16:33,3,U559430,486,2,0
```

Voir le [schéma du Cas d'Usage 1](../../cas-usage-1-cours/data_sample/schema_donnees.md) pour la description complète des colonnes — structure identique sur les 4 années.

## Spécificité de la source Excel 2021

Le fichier `Call_Center_Data_2021.xlsx` contient les mêmes colonnes que les CSV, mais dans un format Excel :
- Onglet nommé (à identifier dans Power Query)
- Première ligne = en-têtes de colonnes
- Possible formatage des dates au format Excel (numérique) → conversion nécessaire

## Volumétrie complète pour l'examen

| Année | Lignes (estimé) | Format |
|-------|----------------|--------|
| 2018 | 33 057 | CSV |
| 2019 | 32 987 | CSV |
| 2020 | 32 931 | CSV |
| 2021 | ≈ 33 000 | Excel |
| **Total** | **≈ 131 975** | **Multi-source** |

---

*Données pédagogiques — Emmanuel TSAGUE — Examen DataScientest 2024*
