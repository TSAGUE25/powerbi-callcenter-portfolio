# Mesures DAX — Cas d'Usage 1 (Cours)
## Centre d'Appels 2018–2020

**Auteur :** Emmanuel TSAGUE  
**Langage :** DAX (Data Analysis Expressions — Expressions d'Analyse de Données)  
**Contexte :** Ces mesures sont créées dans Power BI Desktop, dans l'onglet "Modélisation > Nouvelle mesure"

---

> **Rappel :** DAX est le langage de calcul de Power BI. Une **mesure** (measure) est un calcul qui se recalcule dynamiquement selon les filtres actifs dans le tableau de bord. Elle ne crée pas de colonne dans la table — elle produit un résultat contextuel.

---

## Table des mesures

| # | Nom de la mesure | Catégorie | Complexité |
|---|-----------------|-----------|-----------|
| 1 | Nb_Appels_Total | Volume | ⭐ |
| 2 | Nb_Appels_Abandonnés | Volume | ⭐ |
| 3 | Taux_Abandon | Qualité | ⭐⭐ |
| 4 | Taux_Service | Qualité | ⭐ |
| 5 | Durée_Moy_Min | Performance | ⭐⭐ |
| 6 | WaitTime_Moy_Sec | Performance | ⭐ |
| 7 | Durée_Totale_Heures | Coût | ⭐⭐ |
| 8 | Coût_Total | Coût | ⭐⭐⭐ |
| 9 | Coût_Moy_Par_Appel | Coût | ⭐⭐ |
| 10 | Nb_Appels_SPLY | Temporel | ⭐⭐⭐ |
| 11 | Évolution_YoY | Temporel | ⭐⭐⭐ |
| 12 | Nb_Appels_Par_Agent | Productivité | ⭐⭐ |

---

## Mesures détaillées

### M01 — Nombre total d'appels

```dax
Nb_Appels_Total =
COUNTROWS(F_Appels)
```

| Champ | Valeur |
|-------|--------|
| **Table cible** | F_Appels |
| **Résultat type** | 98 975 (total) / ~33 000 (par année) |
| **Contexte** | Se recalcule selon les filtres actifs |
| **Usage** | Carte KPI principale, courbe temporelle |

---

### M02 — Nombre d'appels abandonnés

```dax
Nb_Appels_Abandonnés =
CALCULATE(
    COUNTROWS(F_Appels),
    F_Appels[CallAbandoned] = 1
)
```

| Champ | Valeur |
|-------|--------|
| **Logique** | `CALCULATE` modifie le contexte de filtre pour ne compter que les lignes avec `CallAbandoned = 1` |
| **Usage** | Carte KPI, indicateur d'alerte |

---

### M03 — Taux d'abandon

```dax
Taux_Abandon =
DIVIDE(
    [Nb_Appels_Abandonnés],
    [Nb_Appels_Total],
    0
)
```

| Champ | Valeur |
|-------|--------|
| **Format** | Pourcentage (ex : 4,2%) |
| **Seuil d'alerte** | > 5% → problème de qualité de service |
| **Note** | `DIVIDE(a, b, 0)` retourne 0 si b = 0, évitant une erreur |
| **Usage** | Jauge d'objectif, carte KPI avec flèche de tendance |

---

### M04 — Taux de service

```dax
Taux_Service =
1 - [Taux_Abandon]
```

| Champ | Valeur |
|-------|--------|
| **Format** | Pourcentage |
| **Objectif standard** | ≥ 95% |
| **Usage** | Jauge verte/rouge selon seuil |

---

### M05 — Durée moyenne d'un appel (en minutes)

```dax
Durée_Moy_Min =
AVERAGEX(
    F_Appels,
    F_Appels[CallDuration] / 60
)
```

| Champ | Valeur |
|-------|--------|
| **Logique** | `AVERAGEX` itère sur chaque ligne, divise par 60, puis fait la moyenne |
| **Alternative** | `AVERAGE(F_Appels[CallDuration_Min])` si la colonne est déjà calculée |
| **Format** | Décimal, 1 décimale (ex : 8,5 min) |
| **Usage** | Carte KPI, comparaison par type d'appel |

---

### M06 — Temps d'attente moyen (en secondes)

```dax
WaitTime_Moy_Sec =
AVERAGE(F_Appels[WaitTime])
```

| Champ | Valeur |
|-------|--------|
| **Format** | Entier (secondes) |
| **Seuil** | > 60 sec → risque d'abandon élevé |
| **Usage** | Carte KPI, courbe temporelle |

---

### M07 — Durée totale des appels (en heures)

```dax
Durée_Totale_Heures =
SUMX(
    F_Appels,
    F_Appels[CallDuration]
) / 3600
```

| Champ | Valeur |
|-------|--------|
| **Logique** | Somme toutes les durées en secondes, divise par 3 600 pour obtenir des heures |
| **Usage** | Estimation de la charge totale des agents, calcul du coût |

---

### M08 — Coût total estimé (€)

```dax
Coût_Total =
SUMX(
    F_Appels,
    F_Appels[CallDuration] / 60
        * RELATED(D_Charges[Tarif_Par_Minute])
)
```

| Champ | Valeur |
|-------|--------|
| **Logique** | Pour chaque appel : durée en minutes × tarif au minute selon le type → somme totale |
| **`RELATED`** | Permet de récupérer le tarif depuis la table D_Charges (liée par `Call Type`) |
| **Format** | Monétaire (€) |
| **Note** | Les tarifs sont des hypothèses simulées (0,80 / 1,20 / 1,50 €/min) |

---

### M09 — Coût moyen par appel (€)

```dax
Coût_Moy_Par_Appel =
DIVIDE(
    [Coût_Total],
    [Nb_Appels_Total],
    0
)
```

| Champ | Valeur |
|-------|--------|
| **Usage** | Benchmark coût par type, comparaison annuelle |

---

### M10 — Volume d'appels l'année précédente (SPLY)

```dax
Nb_Appels_SPLY =
CALCULATE(
    [Nb_Appels_Total],
    SAMEPERIODLASTYEAR(D_Date[Date])
)
```

| Champ | Valeur |
|-------|--------|
| **`SAMEPERIODLASTYEAR`** | Fonction d'intelligence temporelle : décale le contexte d'un an en arrière |
| **Prérequis** | La table `D_Date` doit exister et être marquée comme "table de date" |
| **Usage** | Comparaison 2019 vs 2018, 2020 vs 2019 |

---

### M11 — Évolution YoY (Year-over-Year)

```dax
Évolution_YoY =
DIVIDE(
    [Nb_Appels_Total] - [Nb_Appels_SPLY],
    [Nb_Appels_SPLY],
    BLANK()
)
```

| Champ | Valeur |
|-------|--------|
| **Format** | Pourcentage signé (ex : +5,2% ou -3,1%) |
| **`BLANK()`** | Retourne vide (pas 0%) pour les années sans comparaison (2018) |
| **Usage** | Carte KPI avec flèche haut/bas, courbe de tendance |

---

### M12 — Nombre d'appels par agent (productivité)

```dax
Nb_Appels_Par_Agent =
DIVIDE(
    [Nb_Appels_Total],
    DISTINCTCOUNT(F_Appels[EmployeeID]),
    0
)
```

| Champ | Valeur |
|-------|--------|
| **`DISTINCTCOUNT`** | Compte le nombre d'agents uniques ayant traité des appels dans le contexte |
| **Usage** | Comparaison de productivité, tableau de classement agents |

---

## Bonnes pratiques DAX appliquées

| Pratique | Exemple dans ce projet |
|---------|----------------------|
| Toujours utiliser `DIVIDE` | M03, M09, M11, M12 |
| Préférer `BLANK()` à `0` pour les N/A temporels | M11 |
| Documenter chaque mesure avec un commentaire | Cette fiche |
| Organiser les mesures dans un dossier DAX dédié | Dossier `_Mesures` dans la vue Données |
| Tester chaque mesure sur un filtre connu | Validation vs. Excel source |
| Ne pas dupliquer la logique | M04 réutilise M03 |

---

## Erreurs fréquentes à éviter

> **Erreur 1 :** Écrire `F_Appels[CallDuration] / 60` dans une mesure sans `AVERAGEX` → Power BI ne saura pas s'il doit faire la somme, la moyenne, ou autre chose.

> **Erreur 2 :** Oublier de marquer `D_Date` comme "table de date" → les fonctions `SAMEPERIODLASTYEAR` et `TOTALYTD` ne fonctionneront pas.

> **Erreur 3 :** Créer des colonnes calculées là où des mesures suffiraient → les colonnes calculées consomment de la mémoire, les mesures sont dynamiques et économes.

---

*Emmanuel TSAGUE — Data Scientist / Data Analyst — Formation DataScientest 2024*
