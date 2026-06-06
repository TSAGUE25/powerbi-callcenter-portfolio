# Schéma des données d'exemple — Cas d'Usage 1 (Cours)

## Fichiers présents dans ce dossier

| Fichier | Lignes | Période couverte | Source |
|---------|--------|-----------------|--------|
| `Call_Center_Data_2018_sample.csv` | 100 lignes | Échantillon 2018 | CSV original (100 premières lignes) |
| `Call_Center_Data_2019_sample.csv` | 100 lignes | Échantillon 2019 | CSV original (100 premières lignes) |
| `Call_Center_Data_2020_sample.csv` | 100 lignes | Échantillon 2020 | CSV original (100 premières lignes) |

> **Note :** Ces fichiers sont des échantillons (100 lignes) des données originales (≈ 33 000 lignes par année). Ils sont fournis à titre illustratif pour comprendre la structure des données. Les données originales complètes sont disponibles dans le fichier `.pbix` Power BI.

## Structure des colonnes

```
CallTimestamp,Call Type,EmployeeID,CallDuration,WaitTime,CallAbandoned
5/4/2018 16:33,3,U559430,486,2,0
```

| # | Colonne | Type | Exemple | Description |
|---|---------|------|---------|-------------|
| 1 | `CallTimestamp` | Texte (converti en DateTime) | `5/4/2018 16:33` | Date et heure de l'appel (format US : M/D/YYYY H:MM) |
| 2 | `Call Type` | Entier | `3` | Type de la demande : 1, 2 ou 3 |
| 3 | `EmployeeID` | Texte | `U559430` | Identifiant unique de l'agent |
| 4 | `CallDuration` | Entier | `486` | Durée de l'appel en secondes (486 sec = 8 min 6 sec) |
| 5 | `WaitTime` | Entier | `2` | Temps d'attente avant décroché, en secondes |
| 6 | `CallAbandoned` | Entier (0/1) | `0` | 0 = appel traité, 1 = appel abandonné avant réponse |

## Points d'attention sur la qualité des données

1. **Format de date ambigu :** `5/4/2018` peut signifier le 5 avril (format européen D/M/Y) ou le 4 mai (format américain M/D/Y). Dans Power Query, la conversion automatique doit être vérifiée.

2. **WaitTime = 0 :** Peut signifier une réponse immédiate (acceptable) ou une donnée manquante (à investiguer). Représente environ 15% des lignes.

3. **CallDuration élevée :** Quelques lignes avec `CallDuration > 3600` secondes (> 1 heure). À filtrer avant le calcul des moyennes.

4. **CallAbandoned = 1 avec CallDuration > 0 :** Certains appels abandonnés ont une durée non nulle — cela correspond probablement au temps écoulé avant le raccroché.

## Comment reproduire l'analyse complète

Pour travailler avec les 98 975 lignes complètes :
1. Obtenir les fichiers CSV complets (disponibles sur demande)
2. Les placer dans un dossier local
3. Ouvrir le fichier `.pbix` et mettre à jour les chemins sources dans Power Query

---

*Données à des fins pédagogiques — aucune donnée client réelle*  
*Emmanuel TSAGUE — Formation DataScientest 2024*
