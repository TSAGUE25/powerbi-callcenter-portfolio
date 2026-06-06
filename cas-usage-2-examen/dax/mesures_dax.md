# Mesures DAX Avancées — Cas d'Usage 2 (Examen)
## Centre d'Appels 2018–2021

**Auteur :** Emmanuel TSAGUE  
**Contexte :** Examen certifiant Power BI — DataScientest 2024  
**Niveau :** Avancé — Intelligence temporelle, calculs contextuels, scoring composite

---

> Ce fichier documente les 18 mesures DAX créées de manière autonome lors de l'examen Power BI certifiant. Ces mesures vont au-delà des fondamentaux du cours en introduisant l'intelligence temporelle (YTD, SPLY, YoY) et le scoring composite des agents.

---

## Table des mesures

| # | Nom | Catégorie | Complexité | Nouveauté vs. Cours |
|---|-----|-----------|-----------|---------------------|
| 1 | Nb_Appels_Total | Volume | ⭐ | Base |
| 2 | Nb_Appels_Abandonnés | Volume | ⭐ | Base |
| 3 | Taux_Abandon | Qualité | ⭐⭐ | Base |
| 4 | Taux_Service | Qualité | ⭐ | Base |
| 5 | Durée_Moy_Min | Performance | ⭐⭐ | Base |
| 6 | WaitTime_Moy_Sec | Performance | ⭐ | Base |
| 7 | Coût_Total | Coût | ⭐⭐⭐ | Base |
| 8 | **Nb_Appels_YTD** | Temporel | ⭐⭐⭐ | **NOUVEAU** |
| 9 | **Nb_Appels_SPLY** | Temporel | ⭐⭐⭐ | **NOUVEAU** |
| 10 | **Variation_YoY_Pct** | Temporel | ⭐⭐⭐ | **NOUVEAU** |
| 11 | **Taux_Abandon_SPLY** | Qualité temporelle | ⭐⭐⭐⭐ | **NOUVEAU** |
| 12 | **Variation_Taux_Abandon** | Qualité temporelle | ⭐⭐⭐⭐ | **NOUVEAU** |
| 13 | **Durée_Moy_Par_Type** | Analyse type | ⭐⭐⭐ | **NOUVEAU** |
| 14 | **Taux_Abandon_Par_Type** | Analyse type | ⭐⭐⭐ | **NOUVEAU** |
| 15 | **Coût_Moy_Par_Appel** | Coût | ⭐⭐ | **NOUVEAU** |
| 16 | **Indice_Charge** | Productivité | ⭐⭐⭐⭐ | **NOUVEAU** |
| 17 | **Score_Agent** | Scoring | ⭐⭐⭐⭐⭐ | **NOUVEAU** |
| 18 | **Appels_Heures_Pointe** | Planification | ⭐⭐⭐ | **NOUVEAU** |

---

## Mesures héritées du cours (M01–M07)

```dax
-- M01
Nb_Appels_Total = COUNTROWS(F_Appels)

-- M02
Nb_Appels_Abandonnés =
CALCULATE(COUNTROWS(F_Appels), F_Appels[CallAbandoned] = 1)

-- M03
Taux_Abandon = DIVIDE([Nb_Appels_Abandonnés], [Nb_Appels_Total], 0)

-- M04
Taux_Service = 1 - [Taux_Abandon]

-- M05
Durée_Moy_Min = AVERAGEX(F_Appels, F_Appels[CallDuration] / 60)

-- M06
WaitTime_Moy_Sec = AVERAGE(F_Appels[WaitTime])

-- M07
Coût_Total =
SUMX(
    F_Appels,
    F_Appels[CallDuration] / 60 * RELATED(D_Charges[Tarif_Par_Minute])
)
```

---

## Nouvelles mesures — Intelligence temporelle

### M08 — Cumul annuel (Year-to-Date)

```dax
Nb_Appels_YTD =
TOTALYTD(
    [Nb_Appels_Total],
    D_Date[Date]
)
```

**Ce que fait `TOTALYTD` :** Calcule la somme de la mesure depuis le 1er janvier de l'année en cours jusqu'à la date sélectionnée. C'est l'équivalent d'un "cumul depuis le début de l'année".

**Exemple :**
- Filtre : Juin 2021
- Résultat : volume d'appels de janvier à juin 2021 inclus

**Usage :** "Sommes-nous en avance ou en retard sur notre objectif annuel ?"

---

### M09 — Volume même période l'année précédente

```dax
Nb_Appels_SPLY =
CALCULATE(
    [Nb_Appels_Total],
    SAMEPERIODLASTYEAR(D_Date[Date])
)
```

**`SAMEPERIODLASTYEAR`** : Décale le contexte de filtre d'exactement un an en arrière. Si le filtre actif est "Mars 2021", cette mesure calcule le volume de "Mars 2020".

**Prérequis absolus :**
1. La table `D_Date` doit exister
2. `D_Date` doit être marquée comme "table de dates" (clic droit → Marquer comme table de dates)
3. La colonne `D_Date[Date]` doit être de type `Date` (pas DateTime)

---

### M10 — Variation YoY en pourcentage

```dax
Variation_YoY_Pct =
DIVIDE(
    [Nb_Appels_Total] - [Nb_Appels_SPLY],
    [Nb_Appels_SPLY],
    BLANK()
)
```

**Format :** Pourcentage, 1 décimale, affichage signé (+/-)

**Exemple :**
- 2021 : 33 200 appels
- 2020 SPLY : 32 931 appels
- Résultat : (33 200 - 32 931) / 32 931 = +0,82%

**`BLANK()` vs `0` :** Pour 2018 (aucune année précédente), retourner BLANK() évite d'afficher "0%" qui serait trompeur.

---

### M11 — Taux d'abandon à la même période l'an dernier

```dax
Taux_Abandon_SPLY =
CALCULATE(
    [Taux_Abandon],
    SAMEPERIODLASTYEAR(D_Date[Date])
)
```

**Usage :** "Notre taux d'abandon de mars 2021 (4,8%) est-il meilleur ou moins bon que mars 2020 (5,2%) ?"

---

### M12 — Variation du taux d'abandon (points de %)

```dax
Variation_Taux_Abandon =
[Taux_Abandon] - [Taux_Abandon_SPLY]
```

**Format :** Points de pourcentage (pp) — une variation de -0,4pp signifie une amélioration

**Usage :** Flèche haut/bas sur une carte KPI. Vert si < 0 (amélioration), rouge si > 0 (dégradation).

---

## Nouvelles mesures — Analyse par type d'appel

### M13 — Durée moyenne par type (contextuelle)

```dax
Durée_Moy_Par_Type =
AVERAGEX(
    F_Appels,
    F_Appels[CallDuration] / 60
)
```

> Cette mesure est identique à M05 mais son résultat change selon le contexte de filtre sur `D_TypeAppel[Call Type]`. Utilisée dans un visuel filtré par type, elle donne la durée propre à chaque type.

**Exemple dans une matrice :**
| Type | Durée moy. |
|------|-----------|
| Type 1 | 7,2 min |
| Type 2 | 9,1 min |
| Type 3 | 11,4 min |

---

### M14 — Taux d'abandon par type (tous types confondus dans la page)

```dax
Taux_Abandon_Par_Type =
VAR appels_type = COUNTROWS(F_Appels)
VAR abandons_type = CALCULATE(COUNTROWS(F_Appels), F_Appels[CallAbandoned] = 1)
RETURN
    DIVIDE(abandons_type, appels_type, 0)
```

**Note :** Quand cette mesure est utilisée dans un visuel avec `Call Type` sur l'axe, elle se calcule automatiquement pour chaque type grâce au contexte de ligne (row context).

---

## Nouvelles mesures — Productivité et scoring

### M15 — Coût moyen par appel

```dax
Coût_Moy_Par_Appel =
DIVIDE([Coût_Total], [Nb_Appels_Total], 0)
```

**Usage :** Benchmark coût par type d'appel, comparaison 2021 vs 2020.

---

### M16 — Indice de charge des agents

```dax
Indice_Charge =
VAR heures_travaillées = [Durée_Totale_Heures]
VAR nb_agents = DISTINCTCOUNT(F_Appels[EmployeeID])
VAR capacité_théorique = nb_agents * 8  -- 8 heures par agent par jour * jours dans le contexte
RETURN
    DIVIDE(heures_travaillées, capacité_théorique, 0)
```

**Ce que ça mesure :**
- Indice = 1,0 → charge optimale (les agents sont à 100% de capacité)
- Indice > 1,0 → surcharge (les agents font des heures sup ou certains appels se chevauchent)
- Indice < 0,8 → sous-charge (capacité excédentaire)

**Limite :** La "capacité théorique" de 8h est une hypothèse. À ajuster selon le contrat de travail réel.

---

### M17 — Score de performance agent (scoring composite)

```dax
Score_Agent =
VAR volume = [Nb_Appels_Total]
VAR qualité = 1 - [Taux_Abandon]
VAR durée_agent = [Durée_Moy_Min]
VAR durée_référence = CALCULATE([Durée_Moy_Min], ALL(D_Agents))
VAR efficience = DIVIDE(durée_référence, MAX(durée_agent, 0.01), 0)
RETURN
    volume * qualité * efficience
```

**Logique du score :**
| Composante | Poids logique | Interprétation |
|-----------|--------------|----------------|
| `volume` | Proportionnel | Plus d'appels = plus de contribution |
| `qualité` | 0 à 1 | 0% d'abandon = facteur 1 ; 100% abandon = facteur 0 |
| `efficience` | Inversement prop. à la durée | Appel court = efficience élevée |

**Usage :** Classement des agents dans un tableau. Score élevé = agent performant sur tous les critères.

> **Important :** Ce score est une approximation pédagogique. Un scoring agent réel doit impliquer les RH, intégrer des critères qualitatifs, et faire l'objet d'un accord préalable avec les partenaires sociaux.

---

### M18 — Volume d'appels aux heures de pointe (10h–17h)

```dax
Appels_Heures_Pointe =
CALCULATE(
    [Nb_Appels_Total],
    F_Appels[Heure] >= 10 && F_Appels[Heure] <= 17
)
```

**Usage :** Quelle proportion du volume total est concentrée entre 10h et 17h ? Aide à la planification des pauses et des renforts.

```dax
-- Mesure complémentaire : part des heures de pointe
Part_Heures_Pointe =
DIVIDE([Appels_Heures_Pointe], [Nb_Appels_Total], 0)
```

---

## Architecture DAX recommandée dans Power BI Desktop

```
Panneau Champs (Fields)
└── _Mesures (table virtuelle créée pour organiser les mesures)
    ├── [Volume]
    │   ├── Nb_Appels_Total
    │   ├── Nb_Appels_Abandonnés
    │   ├── Nb_Appels_YTD
    │   ├── Nb_Appels_SPLY
    │   └── Appels_Heures_Pointe
    ├── [Qualité]
    │   ├── Taux_Abandon
    │   ├── Taux_Service
    │   ├── Taux_Abandon_SPLY
    │   └── Variation_Taux_Abandon
    ├── [Temporel]
    │   ├── Variation_YoY_Pct
    │   └── Nb_Appels_YTD
    ├── [Performance]
    │   ├── Durée_Moy_Min
    │   ├── WaitTime_Moy_Sec
    │   ├── Durée_Moy_Par_Type
    │   └── Score_Agent
    └── [Coût]
        ├── Coût_Total
        ├── Coût_Moy_Par_Appel
        └── Indice_Charge
```

---

## Check-list de validation des mesures DAX

Avant de publier le dashboard, vérifier chaque mesure :

- [ ] **M01** : Total = somme des 4 années séparées ?
- [ ] **M03** : Taux = Abandonnés M02 / Total M01 ?
- [ ] **M08** : YTD décembre 2021 = total annuel 2021 ?
- [ ] **M09** : SPLY 2021 = M01 calculé manuellement sur 2020 ?
- [ ] **M10** : YoY = (M01 - M09) / M09 calculé à la main sur un mois ?
- [ ] **M17** : Score varie bien selon l'agent sélectionné ?

---

*Emmanuel TSAGUE — Data Scientist / Data Analyst — Examen DataScientest 2024*
