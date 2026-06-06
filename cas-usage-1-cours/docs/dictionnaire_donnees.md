# Dictionnaire des Données — Cas d'Usage 1 (Cours)
## Centre d'Appels 2018–2020

**Auteur :** Emmanuel TSAGUE  
**Version :** 1.0  
**Date :** 2024

> Un **dictionnaire des données** est la "carte d'identité" de chaque table et colonne d'un projet data. Il est indispensable pour garantir que tous les utilisateurs du dashboard partagent la même compréhension des données.

---

## Table F_Appels (Table de Faits)

**Description :** Table principale contenant l'enregistrement de chaque appel reçu par le centre d'appels entre 2018 et 2020. Chaque ligne = un appel.

**Source :** Fichiers CSV annuels (`Call_Center_Data_2018.csv`, `2019`, `2020`)  
**Volumétrie :** ≈ 98 975 lignes  
**Fréquence de mise à jour :** Annuelle (à chaque clôture d'exercice)

| Colonne | Type original | Type Power BI | Description | Valeurs possibles | Valeur nulle ? |
|---------|--------------|--------------|-------------|-------------------|---------------|
| `CallTimestamp` | Texte (MM/DD/YYYY HH:MM) | Date/Heure | Horodatage précis de l'appel | Du 01/01/2018 au 31/12/2020 | Non |
| `Call Type` | Entier | Entier | Code du type de demande | 1, 2 ou 3 | Non |
| `EmployeeID` | Texte | Texte | Identifiant unique de l'agent | Alphanumérique, ex : `U559430` | Non |
| `CallDuration` | Entier | Entier | Durée totale de l'appel en secondes | 1 à ~7 200 | Non |
| `WaitTime` | Entier | Entier | Temps d'attente avant décroché, en secondes | 0 à ~600 | Non |
| `CallAbandoned` | Entier | Entier | Indicateur binaire : appel raccroché avant réponse | 0 (non abandonné) ou 1 (abandonné) | Non |

### Colonnes calculées ajoutées par Power Query

| Colonne créée | Logique | Type | Exemple |
|--------------|---------|------|---------|
| `Année` | `Date.Year([CallTimestamp])` | Entier | 2018 |
| `Trimestre` | `Date.QuarterOfYear([CallTimestamp])` | Entier | 1 (= T1) |
| `Mois_Num` | `Date.Month([CallTimestamp])` | Entier | 3 (= mars) |
| `Mois_Libellé` | Correspondance Mois_Num → Texte | Texte | "Mars" |
| `Jour_Semaine` | `Date.DayOfWeekName([CallTimestamp])` | Texte | "Lundi" |
| `Heure` | `Time.Hour(DateTime.Time([CallTimestamp]))` | Entier | 16 (= 16h) |
| `CallDuration_Min` | `[CallDuration] / 60` | Décimal | 8,1 |
| `WaitTime_Sec` | `[WaitTime]` (renommé pour clarté) | Entier | 2 |

---

## Table D_Date (Dimension Calendrier)

**Description :** Table de calendrier complète couvrant toute la période d'analyse. Créée avec la fonction DAX `CALENDAR()`. Obligatoire pour les fonctions d'intelligence temporelle.

**Source :** Générée en DAX dans Power BI Desktop  
**Volumétrie :** 1 096 lignes (3 années × 365/366 jours)

| Colonne | Type | Description | Exemple |
|---------|------|-------------|---------|
| `Date` | Date | Clé primaire — un jour par ligne | 15/03/2019 |
| `Année` | Entier | Année calendaire | 2019 |
| `Trimestre_Num` | Entier | Numéro du trimestre | 1 |
| `Trimestre_Libellé` | Texte | Libellé | "T1 2019" |
| `Mois_Num` | Entier | Numéro du mois | 3 |
| `Mois_Libellé` | Texte | Nom du mois | "Mars" |
| `Semaine_Num` | Entier | Numéro de semaine ISO | 11 |
| `Jour_Num` | Entier | Jour du mois | 15 |
| `Jour_Semaine_Libellé` | Texte | Nom du jour | "Vendredi" |

**Code DAX de création :**
```dax
D_Date =
VAR debut = DATE(2018, 1, 1)
VAR fin = DATE(2020, 12, 31)
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

## Table D_Agents (Dimension Agents)

**Description :** Table de référence des agents du centre d'appels.

**Source :** `Lookup_Tables.xlsx`  
**Volumétrie :** Variable selon le nombre d'agents

| Colonne | Type | Description | Exemple |
|---------|------|-------------|---------|
| `EmployeeID` | Texte | Clé primaire unique | `U559430` |
| `Nom_Agent` | Texte | Nom complet (si disponible) | `Martin Dupont` |
| `Équipe` | Texte | Groupe d'appartenance | `Équipe A` |
| `Ancienneté_Ans` | Décimal | Années d'expérience | 3,5 |

---

## Table D_TypeAppel (Dimension Types d'Appels)

**Description :** Table de référence des types d'appels (codes 1, 2, 3).

**Source :** `Lookup_Tables.xlsx`

| Code | Libellé | Description |
|------|---------|-------------|
| 1 | Renseignement | Demande d'information tarifaire ou contractuelle |
| 2 | Modification | Changement d'offre, modification de contrat |
| 3 | Réclamation | Signalement de problème, contestation |

> **Note :** Ces libellés sont des hypothèses pédagogiques. Dans un contexte réel, ils seraient définis par le métier et documentés dans une charte de gouvernance des données.

---

## Table D_Charges (Dimension Tarification)

**Description :** Table de tarification du coût opérationnel par type d'appel.

**Source :** `Call_Charges.xlsx`

| Call Type | Tarif_Par_Minute (€) | Source |
|-----------|---------------------|--------|
| 1 | 0,80 € | Hypothèse simulée |
| 2 | 1,20 € | Hypothèse simulée |
| 3 | 1,50 € | Hypothèse simulée |

> **Avertissement :** Ces tarifs sont fictifs et créés à des fins pédagogiques uniquement. Dans un projet réel, les coûts réels (salaires, infrastructure, etc.) doivent être fournis par le contrôle de gestion.

---

## Règles de gestion et définitions métier

| Terme | Définition retenue dans ce projet |
|-------|----------------------------------|
| **Appel abandonné** | `CallAbandoned = 1` — le client a raccroché avant qu'un agent décroche |
| **Appel traité** | `CallAbandoned = 0` — un agent a répondu à l'appel |
| **Durée d'appel** | `CallDuration` inclut le temps de traitement après décroché, mais pas le temps d'attente |
| **Taux d'abandon** | `Abandonnés / Total appels`, exprimé en % |
| **Taux de service** | `1 - Taux_Abandon` — standard industrie : objectif ≥ 95% |
| **Heure de pointe** | Heure de la journée avec le plus fort volume d'appels |
| **Agent actif** | Agent ayant traité au moins 1 appel sur la période filtrée |

---

## Problèmes de qualité identifiés

| Problème | Colonne | Fréquence estimée | Traitement appliqué |
|---------|---------|------------------|---------------------|
| `WaitTime = 0` sur appels non abandonnés | WaitTime | ~15% des lignes | Conservé — peut signifier réponse immédiate |
| `CallDuration` très élevée (> 3 600 sec) | CallDuration | < 0,1% | Filtré (exclu de la moyenne) |
| Dates au format US (M/D/Y vs D/M/Y) | CallTimestamp | 100% | Converti en type Date/Heure Power BI |
| `EmployeeID` sans correspondance dans Lookup | EmployeeID | < 0,5% | Conservé avec libellé "Agent inconnu" |

---

*Emmanuel TSAGUE — Data Scientist / Data Analyst — Formation DataScientest 2024*
