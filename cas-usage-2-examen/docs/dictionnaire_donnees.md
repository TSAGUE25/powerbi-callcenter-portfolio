# Dictionnaire des Données — Cas d'Usage 2 (Examen)
## Centre d'Appels 2018–2021

**Auteur :** Emmanuel TSAGUE  
**Spécificité examen :** Intégration d'une source Excel (2021) en plus des CSV historiques

---

## Évolution par rapport au Cours (Cas 1)

| Élément | Cours (2018-2020) | Examen (2018-2021) |
|---------|------------------|-------------------|
| Sources | 3 fichiers CSV | 3 CSV + 1 Excel |
| Volumétrie | ≈ 99 000 lignes | ≈ 132 000 lignes |
| Période | 3 ans | 4 ans |
| Colonnes | 6 colonnes source | 6 colonnes source (identiques) |
| Colonnes calculées | 8 | 9 (+ `Source_Fichier`) |
| Mesures DAX | 12 | 18 |
| Pages dashboard | 5 | 6 |

---

## Table F_Appels (Table de Faits — Version Examen)

**Description :** Table unifiée de 132 000 appels couvrant 4 années. Fusion de 3 CSV (2018-2020) et 1 Excel (2021).

| Colonne | Type | Description | Particularité 2021 |
|---------|------|-------------|-------------------|
| `CallTimestamp` | Date/Heure | Horodatage de l'appel | Même format que les CSV |
| `Call Type` | Entier | Type de demande (1, 2, 3) | Identique |
| `EmployeeID` | Texte | Identifiant agent | Identique |
| `CallDuration` | Entier (sec.) | Durée de l'appel | Identique |
| `WaitTime` | Entier (sec.) | Temps d'attente | Identique |
| `CallAbandoned` | Binaire (0/1) | Appel abandonné | Identique |
| **`Source_Fichier`** | **Texte** | **Origine de la ligne** | **Nouveau — traçabilité** |

### La colonne `Source_Fichier` (nouveau dans l'examen)

| Valeur | Signification |
|--------|--------------|
| `CSV_2018` | Ligne issue du fichier CSV 2018 |
| `CSV_2019` | Ligne issue du fichier CSV 2019 |
| `CSV_2020` | Ligne issue du fichier CSV 2020 |
| `Excel_2021` | Ligne issue du fichier Excel 2021 |

**Utilité :** Permet de vérifier que la fusion s'est bien passée (pas de lignes dupliquées, pas de lignes perdues) et de filtrer sur une source spécifique pour débogage.

---

## Table D_Date (Dimension Calendrier — Version Examen)

**Étendue à 2021 :** La table couvre maintenant 2018 à 2021 (1 461 jours au lieu de 1 096).

```dax
D_Date =
VAR debut = DATE(2018, 1, 1)
VAR fin = DATE(2021, 12, 31)  -- Étendu à 2021
VAR calendrier = CALENDAR(debut, fin)
RETURN
    ADDCOLUMNS(
        calendrier,
        "Année", YEAR([Date]),
        "Mois_Num", MONTH([Date]),
        "Mois_Libellé", FORMAT([Date], "MMMM"),
        "Trimestre_Num", QUARTER([Date]),
        "Trimestre_Libellé", "T" & QUARTER([Date]) & " " & YEAR([Date]),
        "Jour_Semaine_Libellé", FORMAT([Date], "dddd"),
        "Semaine_Num", WEEKNUM([Date], 2)
    )
```

---

## Gestion de la source Excel 2021 dans Power Query

### Différences entre CSV et Excel

| Aspect | CSV 2018-2020 | Excel 2021 |
|--------|--------------|-----------|
| Format | `.csv` (texte séparé par virgule) | `.xlsx` (classeur Excel) |
| Import Power Query | `Csv.Document()` | `Excel.Workbook()` |
| En-têtes | Première ligne du fichier | Première ligne de l'onglet |
| Encodage | UTF-8 (spécifier) | Automatique |
| Onglets multiples | N/A | Navigation vers l'onglet de données |

### Code Power Query pour Excel 2021

```m
let
    Source = Excel.Workbook(
        File.Contents("C:\...\Call_Center_Data_2021.xlsx"),
        null, true
    ),
    Onglet_Data = Source{[Item="Call Center Data",Kind="Sheet"]}[Data],
    Entetes_promus = Table.PromoteHeaders(Onglet_Data, [PromoteAllScalars=true]),
    Types_définis = Table.TransformColumnTypes(Entetes_promus, {
        {"CallTimestamp", type datetime},
        {"Call Type", Int64.Type},
        {"EmployeeID", type text},
        {"CallDuration", Int64.Type},
        {"WaitTime", Int64.Type},
        {"CallAbandoned", Int64.Type}
    }),
    Source_ajoutée = Table.AddColumn(Types_définis, "Source_Fichier",
        each "Excel_2021", type text)
in
    Source_ajoutée
```

---

## Règles de gestion — Version Examen (mises à jour)

| Terme | Définition (identique au cours, confirmée sur 4 ans) |
|-------|-----------------------------------------------------|
| **Appel abandonné** | `CallAbandoned = 1` |
| **Taux d'abandon** | Abandonnés / Total, en % |
| **Taux de service** | 1 - Taux_Abandon, objectif ≥ 95% |
| **SPLY** | Same Period Last Year — même période de l'année précédente |
| **YoY** | Year over Year — variation annuelle en % |
| **YTD** | Year to Date — cumul depuis le 1er janvier |
| **Heure de pointe** | Créneaux 10h–17h (hypothèse basée sur l'analyse) |

---

*Emmanuel TSAGUE — Data Scientist / Data Analyst — Examen DataScientest 2024*
