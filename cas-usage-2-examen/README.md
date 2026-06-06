# Cas d'Usage 2 — Examen Power BI
## Tableau de Bord Décisionnel Multi-Années d'un Centre d'Appels
### Analyse avancée de la performance opérationnelle 2018–2021 — Examen certifiant

**Auteur :** Emmanuel TSAGUE | Data Scientist / Data Analyst  
**Contexte :** Examen final — Formation Power BI (DataScientest, 2024)  
**Fichier soumis :** `Exam_PBI_Call_Center TSAGUE EMMANUEL.pbix`  
**Données :** Simulées à des fins pédagogiques — ≈ 132 000 appels sur 4 ans  
**Outils :** Power BI Desktop · Power Query · DAX · Excel · CSV

---

## Table des matières

1. [Titre et résumé](#1-titre-et-résumé)
2. [Contexte métier](#2-contexte-métier)
3. [Problème métier](#3-problème-métier)
4. [Objectif du projet](#4-objectif-du-projet)
5. [Données utilisées](#5-données-utilisées)
6. [Préparation des données avec Power Query](#6-préparation-des-données-avec-power-query)
7. [Modèle de données Power BI](#7-modèle-de-données-power-bi)
8. [Mesures DAX avancées](#8-mesures-dax-avancées)
9. [KPI opérationnels avancés](#9-kpi-opérationnels-avancés)
10. [Architecture du tableau de bord](#10-architecture-du-tableau-de-bord)
11. [Visuels Power BI utilisés](#11-visuels-power-bi-utilisés)
12. [Analyse métier produite](#12-analyse-métier-produite)
13. [Résultats attendus](#13-résultats-attendus)
14. [Valeur métier](#14-valeur-métier)
15. [Limites du projet](#15-limites-du-projet)
16. [Améliorations possibles](#16-améliorations-possibles)
17. [Version CV](#17-version-cv)
18. [Version entretien (2 minutes)](#18-version-entretien-2-minutes)
19. [Structure GitHub](#19-structure-github)
20. [Version LinkedIn](#20-version-linkedin)
21. [Questions d'entretien et réponses](#21-questions-dentretien-et-réponses)

---

## 1. Titre et résumé

**Titre complet :**  
> *Tableau de Bord Décisionnel Multi-Années d'un Centre d'Appels — Analyse avancée de la qualité de service, des coûts, des tendances et de la performance des agents sur 4 ans (2018–2021)*

**Résumé en une phrase :**  
Examen Power BI certifiant : construction autonome d'un tableau de bord décisionnel professionnel sur 4 années de données (≈ 132 000 appels), intégrant une modélisation avancée, des mesures DAX de comparaison temporelle, une analyse multi-dimensionnelle et une architecture orientée décision managériale.

**Ce qui distingue ce cas d'usage de l'examen vs. le cours :**

| Dimension | Cours (Cas 1) | Examen (Cas 2) |
|-----------|--------------|----------------|
| Données | 3 ans (2018–2020) | 4 ans (2018–2021) — +2021 |
| Autonomie | Guidé pas à pas | Totalement autonome |
| Mesures DAX | 12 mesures fondamentales | 18+ mesures avancées |
| Analyse | Descriptive | Descriptive + Comparative + Tendancielle |
| Dashboard | Structure imposée | Architecture libre conçue par l'étudiant |
| Niveau | Apprentissage | Validation de maîtrise |

---

## 2. Contexte métier

### Situation de l'examen

L'examen Power BI consistait à recevoir un jeu de données inédit — les données 2021 du centre d'appels, en plus des données 2018–2020 déjà connues — et à construire **de manière totalement autonome** un tableau de bord professionnel en temps limité.

### Enjeu de l'examen

Démontrer que je suis capable de :
1. Analyser un nouveau jeu de données sans guide ;
2. Construire un modèle de données robuste de zéro ;
3. Écrire des mesures DAX adaptées au contexte sans documentation ;
4. Concevoir un tableau de bord compréhensible par un manager non technique ;
5. Produire un résultat professionnel en situation de pression.

### Contexte métier sous-jacent

En 2021, le centre d'appels fait face à un enjeu nouveau : **la crise COVID-19 a profondément modifié les comportements clients**. Les volumes d'appels ont changé, les types de demandes ont évolué, certains agents ont travaillé à distance. Le directeur du centre veut savoir :
- *Comment 2021 se compare-t-il aux 3 années précédentes ?*
- *La crise a-t-elle dégradé notre qualité de service ?*
- *Nos coûts ont-ils évolué de façon maîtrisée ?*
- *Quels agents ont maintenu leur performance en télétravail ?*

> **Pourquoi ce sujet existe-t-il ?** Parce que les données de 2021 sont arrivées dans un format légèrement différent (Excel au lieu de CSV), et que le manager a besoin d'une vue consolidée 4 ans pour prendre des décisions budgétaires et RH pour 2022. Sans Power BI, la consolidation prendrait plusieurs semaines. Avec Power BI, elle prend quelques heures.

---

## 3. Problème métier

**Questions métier posées par l'examen :**

1. **Comment intégrer les données 2021** (format Excel) dans un modèle qui accueillait jusqu'ici des CSV ?
2. **La qualité de service en 2021** s'est-elle maintenue, améliorée ou dégradée par rapport à 2018–2020 ?
3. **Le volume d'appels 2021** est-il cohérent avec les tendances historiques ?
4. **Y a-t-il des mois ou des heures en 2021** qui montrent des anomalies non observées avant ?
5. **Le coût opérationnel 2021** est-il sous contrôle par rapport aux années précédentes ?
6. **Quels agents ont le mieux performé** sur l'ensemble de la période 2018–2021 ?

---

## 4. Objectif du projet

### Objectif principal

> Construire un tableau de bord Power BI de niveau professionnel qui consolide 4 années de données d'un centre d'appels, permet une comparaison temporelle robuste, et produit des insights actionnables pour le management.

### Objectifs secondaires

| # | Objectif | Indicateur de réussite |
|---|---------|----------------------|
| 1 | Intégrer les données 2021 (Excel) dans le modèle | Table F_Appels contient 2018+2019+2020+2021 |
| 2 | Construire un modèle en étoile complet | 5 tables reliées, aucune relation ambiguë |
| 3 | Créer des mesures DAX de comparaison N vs N-1 | Mesure `Évolution_YoY` fonctionnelle |
| 4 | Concevoir un dashboard en 5+ pages | Navigation fluide, visuels cohérents |
| 5 | Permettre le filtrage par année, mois, type, agent | Slicers actifs sur toutes les pages |
| 6 | Produire un dashboard utilisable sans formation | Titres clairs, axes lisibles, couleurs cohérentes |

---

## 5. Données utilisées

### Vue d'ensemble — 4 années consolidées

| Fichier | Format | Période | Lignes approx. | Nouveauté |
|---------|--------|---------|----------------|-----------|
| `Call_Center_Data_2018.csv` | CSV | 2018 | 33 057 | Données de base (cours) |
| `Call_Center_Data_2019.csv` | CSV | 2019 | 32 987 | Données de base (cours) |
| `Call_Center_Data_2020.csv` | CSV | 2020 | 32 931 | Données de base (cours) |
| `Call_Center_Data_2021.xlsx` | **Excel** | 2021 | ≈ 33 000 | **Nouveau format — examen** |
| `Lookup_Tables.xlsx` | Excel | — | Variable | Tables de référence |
| `Call_Charges.xlsx` | Excel | — | Variable | Tarification |

**Total : ≈ 132 000 appels sur 4 ans**

> **Défi technique de l'examen :** Les données 2021 sont en format Excel, pas en CSV comme les années précédentes. Il faut adapter la requête Power Query pour gérer les deux formats et les fusionner proprement en une seule table de faits.

---

### Schéma des colonnes (identiques sur les 4 années)

| Colonne | Type | Description | Valeurs possibles |
|---------|------|-------------|------------------|
| `CallTimestamp` | Date/Heure | Horodatage de l'appel | MM/DD/YYYY HH:MM |
| `Call Type` | Entier | Nature de la demande | 1, 2, 3 |
| `EmployeeID` | Texte | Identifiant agent | Ex : `U559430` |
| `CallDuration` | Entier (sec.) | Durée de l'appel | 0 – ~7 200 |
| `WaitTime` | Entier (sec.) | Temps d'attente | 0 – ~600 |
| `CallAbandoned` | Binaire | Appel abandonné | 0 ou 1 |

### Colonnes calculées créées dans Power Query

| Colonne créée | Source | Utilité |
|--------------|--------|---------|
| `Année` | `CallTimestamp` | Filtrer par année |
| `Trimestre` | `CallTimestamp` | Analyse trimestrielle |
| `Mois_Num` | `CallTimestamp` | Tri chronologique |
| `Mois_Libellé` | `Mois_Num` | Affichage lisible |
| `Jour_Semaine` | `CallTimestamp` | Analyse par jour |
| `Heure` | `CallTimestamp` | Analyse horaire |
| `CallDuration_Min` | `CallDuration / 60` | Lisibilité |
| `WaitTime_Sec` | `WaitTime` | Clarté |
| `Source_Fichier` | Métadonnée Power Query | Traçabilité de l'origine |
| `Période` | `Année + " - " + Trimestre` | Label pour les axes |

---

## 6. Préparation des données avec Power Query

> **Power Query** est l'outil de transformation des données dans Power BI. Il fonctionne en langage M (un langage fonctionnel propre à Microsoft). Chaque transformation est enregistrée sous forme d'étapes visibles et reproductibles — c'est transparent et auditables.

### Défi spécifique de l'examen : fusionner CSV et Excel

L'examen impose de fusionner des sources hétérogènes. Voici la stratégie Power Query utilisée :

```
Source CSV  2018 ─┐
Source CSV  2019 ─┤
Source CSV  2020 ─┼─► Normalisation ─► Append ─► F_Appels_Unifiée (132 000 lignes)
Source XLSX 2021 ─┘
```

**Étapes Power Query détaillées :**

#### Étape 1 — Import des CSV (2018, 2019, 2020)

```m
= Csv.Document(File.Contents("...Call_Center_Data_2018.csv"),
    [Delimiter=",", Columns=6, Encoding=65001, QuoteStyle=QuoteStyle.None])
```

#### Étape 2 — Import du fichier Excel 2021

```m
= Excel.Workbook(File.Contents("...Call_Center_Data_2021.xlsx"), null, true)
```
Puis navigation vers l'onglet contenant les données et promotion de la première ligne en en-têtes.

#### Étape 3 — Harmonisation des colonnes

Les deux sources doivent avoir exactement les mêmes noms de colonnes avant la fusion :

| CSV (2018-2020) | Excel 2021 | Action |
|----------------|-----------|--------|
| `CallTimestamp` | `CallTimestamp` | Identique ✓ |
| `Call Type` | `Call Type` | Identique ✓ |
| `EmployeeID` | `EmployeeID` | Identique ✓ |
| `CallDuration` | `CallDuration` | Identique ✓ |
| `WaitTime` | `WaitTime` | Identique ✓ |
| `CallAbandoned` | `CallAbandoned` | Identique ✓ |

#### Étape 4 — Fusion verticale (Append)

```m
= Table.Combine({Appels_2018, Appels_2019, Appels_2020, Appels_2021})
```

#### Étape 5 — Nettoyage et enrichissement

```m
// Conversion de la date
= Table.TransformColumnTypes(Source, {{"CallTimestamp", type datetime}})

// Extraction de l'heure
= Table.AddColumn(Source, "Heure", each Time.Hour([CallTimestamp]), Int64.Type)

// Calcul durée en minutes
= Table.AddColumn(Source, "CallDuration_Min",
    each [CallDuration] / 60, type number)

// Ajout colonne Source pour traçabilité
= Table.AddColumn(Source, "Source_Fichier",
    each if [Année] = 2021 then "Excel_2021" else "CSV_" & Text.From([Année]))
```

#### Étape 6 — Validation qualité

| Contrôle | Méthode Power Query | Résultat attendu |
|---------|---------------------|-----------------|
| Aucun doublon | `Table.Distinct` sur clé composite | 0 doublon |
| Toutes les années présentes | `Table.Group` par Année | 4 groupes : 2018, 2019, 2020, 2021 |
| `CallAbandoned` ∈ {0,1} | Filtre conditionnel | 0 valeur hors plage |
| Pas de durée nulle | Filtre `CallDuration > 0` | Toutes durées positives |
| Dates dans les plages attendues | Min/Max par année | 2018 ⊂ 2018, etc. |

> **Erreur fréquente à l'examen :** Ne pas vérifier l'ordre des colonnes avant l'Append. Si les colonnes sont dans un ordre différent entre le CSV 2020 et l'Excel 2021, Power BI les aligne par position et non par nom — ce qui crée des mélanges de valeurs silencieux et difficiles à détecter.

---

## 7. Modèle de données Power BI

### Modèle en étoile — Version examen (enrichi)

```
                          D_Date
                     (Calendrier complet
                       2018–2021)
                           │
              ┌────────────┼────────────┐
              │            │            │
          D_Agents     F_Appels     D_TypeAppel
         (Employés)   (Table de    (Codes 1,2,3)
                       faits —
                      132 000 lg)
              │            │            │
              └────────────┼────────────┘
                           │
                       D_Charges
                      (Tarification
                       par type)
```

### Description complète des tables

#### F_Appels — Table de faits (cœur du modèle)

```
F_Appels
├── CallTimestamp (clé vers D_Date)
├── EmployeeID    (clé vers D_Agents)
├── Call Type     (clé vers D_TypeAppel et D_Charges)
├── CallDuration
├── CallDuration_Min
├── WaitTime
├── WaitTime_Sec
├── CallAbandoned
├── Année
├── Trimestre
├── Mois_Num
├── Mois_Libellé
├── Jour_Semaine
├── Heure
└── Source_Fichier
```

#### D_Date — Table de calendrier

> La table de calendrier est une **bonne pratique obligatoire** en Power BI. Elle permet d'utiliser les fonctions d'intelligence temporelle (SAMEPERIODLASTYEAR, TOTALYTD, etc.) correctement.

```dax
D_Date =
CALENDAR(DATE(2018, 1, 1), DATE(2021, 12, 31))
```

Colonnes ajoutées :
- `Année`, `Trimestre`, `Mois_Num`, `Mois_Libellé`, `Jour_Semaine`, `Semaine_Num`

#### D_Agents — Table de dimension des agents

| Colonne | Description |
|---------|-------------|
| `EmployeeID` | Clé primaire (unique par agent) |
| `Nom_Agent` | Nom (si disponible dans Lookup Tables) |
| `Équipe` | Groupe d'appartenance |

#### D_TypeAppel — Types d'appels

| Code | Libellé (hypothèse pédagogique) |
|------|-------------------------------|
| 1 | Renseignement / Information |
| 2 | Modification de contrat |
| 3 | Réclamation / Problème |

#### D_Charges — Tarification

| Type | Coût par minute (hypothèse simulée) |
|------|-----------------------------------|
| 1 | 0,80 € |
| 2 | 1,20 € |
| 3 | 1,50 € |

### Relations du modèle

| Table source | Clé | Table cible | Cardinalité |
|-------------|-----|------------|------------|
| F_Appels | `CallTimestamp` (date tronquée) | D_Date | Plusieurs → Un |
| F_Appels | `EmployeeID` | D_Agents | Plusieurs → Un |
| F_Appels | `Call Type` | D_TypeAppel | Plusieurs → Un |
| F_Appels | `Call Type` | D_Charges | Plusieurs → Un |

> **À retenir :** La cardinalité "Plusieurs → Un" signifie que plusieurs appels peuvent être associés à un même agent, une même date ou un même type. C'est la structure normale d'un modèle en étoile : les faits sont nombreux, les dimensions sont uniques.

---

## 8. Mesures DAX avancées

> Les mesures de l'examen vont au-delà des fondamentaux du cours. Elles introduisent l'**intelligence temporelle** (comparaison N vs N-1, cumul annuel) et les **calculs contextuels** (performance relative par agent).

---

### Mesures de base (héritées du cours)

```dax
Nb_Appels_Total = COUNTROWS(F_Appels)

Taux_Abandon =
DIVIDE([Nb_Appels_Abandonnés], [Nb_Appels_Total], 0)

Durée_Moy_Min =
AVERAGEX(F_Appels, F_Appels[CallDuration] / 60)
```

---

### Mesure avancée 1 — Cumul annuel (YTD)

```dax
Nb_Appels_YTD =
TOTALYTD(
    [Nb_Appels_Total],
    D_Date[Date]
)
```
**Ce que ça mesure :** Le cumul des appels depuis le 1er janvier de l'année en cours jusqu'à la date sélectionnée.  
**Usage :** "Au 30 juin 2021, nous avons traité X appels — sommes-nous sur la bonne trajectoire pour atteindre l'objectif annuel ?"

---

### Mesure avancée 2 — Même période l'année dernière (SPLY)

```dax
Nb_Appels_SPLY =
CALCULATE(
    [Nb_Appels_Total],
    SAMEPERIODLASTYEAR(D_Date[Date])
)
```
**Ce que ça mesure :** Le volume d'appels à la même période l'année précédente.  
**Usage :** "En mars 2021, nous avons eu 2 800 appels. En mars 2020, nous en avions 2 650. C'est +5,7%."

---

### Mesure avancée 3 — Variation en % (YoY)

```dax
Variation_YoY_Pct =
DIVIDE(
    [Nb_Appels_Total] - [Nb_Appels_SPLY],
    [Nb_Appels_SPLY],
    BLANK()
)
```
**Ce que ça mesure :** La croissance ou la baisse du volume par rapport à l'année précédente, en pourcentage.  
**Note :** `BLANK()` au lieu de `0` pour que les années sans comparaison (2018) n'affichent pas 0% mais restent vides.

---

### Mesure avancée 4 — Durée moyenne par type d'appel

```dax
Durée_Moy_Par_Type =
AVERAGEX(
    FILTER(F_Appels, F_Appels[Call Type] = SELECTEDVALUE(D_TypeAppel[Call Type])),
    F_Appels[CallDuration] / 60
)
```
**Usage :** "Les réclamations (Type 3) durent-elles plus longtemps que les renseignements (Type 1) ?"

---

### Mesure avancée 5 — Taux d'abandon par type

```dax
Taux_Abandon_Par_Type =
DIVIDE(
    CALCULATE([Nb_Appels_Abandonnés],
        ALLEXCEPT(F_Appels, F_Appels[Call Type])),
    CALCULATE([Nb_Appels_Total],
        ALLEXCEPT(F_Appels, F_Appels[Call Type])),
    0
)
```

---

### Mesure avancée 6 — Coût total estimé

```dax
Coût_Total =
SUMX(
    F_Appels,
    F_Appels[CallDuration_Min] * RELATED(D_Charges[Tarif_Par_Minute])
)
```

---

### Mesure avancée 7 — Coût moyen par appel

```dax
Coût_Moy_Par_Appel =
DIVIDE([Coût_Total], [Nb_Appels_Total], 0)
```

---

### Mesure avancée 8 — Score de performance agent

```dax
Score_Agent =
VAR appels_agent = [Nb_Appels_Total]
VAR taux_abandon_agent = [Taux_Abandon]
VAR durée_agent = [Durée_Moy_Min]
VAR durée_globale = CALCULATE([Durée_Moy_Min], ALL(D_Agents))
RETURN
    appels_agent * (1 - taux_abandon_agent) * (durée_globale / MAX(durée_agent, 0.01))
```
**Ce que ça mesure :** Un score composite qui récompense les agents qui traitent beaucoup d'appels avec un faible taux d'abandon et une durée raisonnable.  
**Usage :** Classement automatique des agents pour les entretiens RH ou les primes de performance.

---

### Mesure avancée 9 — Appels par heure de pointe (heures 10h–17h)

```dax
Appels_Heures_Pointe =
CALCULATE(
    [Nb_Appels_Total],
    F_Appels[Heure] >= 10 && F_Appels[Heure] <= 17
)
```

---

### Mesure avancée 10 — Indice de charge (volume vs. capacité théorique)

```dax
Indice_Charge =
DIVIDE(
    [Durée_Totale_Heures],
    DISTINCTCOUNT(F_Appels[EmployeeID]) * 8,  -- 8h de travail par agent
    0
)
```
**Ce que ça mesure :** Si l'indice > 1, les agents ont travaillé plus que leur capacité théorique → surcharge.  
**Usage :** Détecter les mois ou les semaines de surcharge pour anticiper les recrutements.

---

## 9. KPI opérationnels avancés

| KPI | Définition | Formule DAX | Utilité | Décision |
|-----|-----------|------------|---------|---------|
| **Volume total 2021** | Nb appels sur l'année | `TOTALYTD([Nb_Appels_Total], ...)` | Suivre la trajectoire annuelle | Recruter si > objectif |
| **Variation YoY volume** | Croissance vs 2020 | `Variation_YoY_Pct` | Détecter les anomalies | Investiguer si > ±15% |
| **Taux d'abandon global** | % abandonnés | `Taux_Abandon` | Mesurer la satisfaction | Déclencher une action si > 5% |
| **Taux d'abandon par type** | Idem, par type d'appel | `Taux_Abandon_Par_Type` | Identifier le type problématique | Former sur le type critique |
| **Durée moy. par type** | Minutes par type | `Durée_Moy_Par_Type` | Évaluer la complexité | Simplifier les scripts |
| **Coût total 2021** | € estimé | `Coût_Total` | Contrôler le budget | Arbitrer entre types |
| **Coût moy. par appel** | € par appel | `Coût_Moy_Par_Appel` | Benchmarker | Comparer aux standards secteur |
| **Score agent** | Performance composite | `Score_Agent` | Comparer objectivement | Primes, formations, promotions |
| **Indice de charge** | Taux d'utilisation équipes | `Indice_Charge` | Détecter surcharge | Recrutement, réaffectation |
| **Heures de pointe** | Volume 10h–17h | `Appels_Heures_Pointe` | Planification des shifts | Décaler les pauses |
| **Évolution mensuelle** | Tendance par mois | YoY par mois | Détecter saisonnalité | Congés anticipés |
| **Taux de service 4 ans** | % appels traités | `1 - Taux_Abandon` par année | Vision historique | Objectif 2022 |

---

## 10. Architecture du tableau de bord

> Architecture libre, conçue de manière autonome pour l'examen. La structure reflète la progression logique d'un manager : d'abord une vue d'ensemble, puis une analyse détaillée, puis des insights actionnables.

### Page 1 — Synthèse Exécutive 4 ans

**Objectif :** Vue d'ensemble de la performance sur 4 années en un coup d'œil.

```
┌─────────────────────────────────────────────────────────────────┐
│  CENTRE D'APPELS — PERFORMANCE 2018–2021        [Slicer Année]  │
├──────────┬──────────┬──────────┬──────────┬────────────────────  │
│  132 000 │   4,1%   │  8,5 min │  €412k  │  ↑ +2,3% vs 2020   │
│  Appels  │ Abandon  │ Durée moy│  Coût   │  Évolution YoY      │
├──────────┴──────────┴──────────┴──────────┴────────────────────  │
│  [Courbe : Volume par année — comparaison 2018/19/20/21]        │
├─────────────────────────────────────────┬───────────────────────│
│  [Barres : Volume par type d'appel]     │  [Donut : Répartition│
│                                         │   abandons par type] │
└─────────────────────────────────────────┴───────────────────────┘
```

| Élément | Contenu |
|---------|---------|
| **KPI cartes** | Volume · Taux abandon · Durée moy. · Coût · YoY |
| **Visuels** | Courbe 4 ans · Barres par type · Donut abandon |
| **Filtres** | Année · Type d'appel |
| **Décision** | "2021 est-il une année normale ou une anomalie ?" |

---

### Page 2 — Analyse Temporelle Avancée

**Objectif :** Identifier les tendances, saisonnalités et anomalies dans le temps sur 4 ans.

| Visuel | Axe X | Axe Y | Insight |
|--------|-------|-------|---------|
| Courbe multi-série | Mois | Volume par année | "2021 suit-il la même saisonnalité que 2020 ?" |
| Heatmap | Heure | Jour de la semaine | "Quand est-on le plus chargé ?" |
| Barres groupées | Trimestre | Volume N vs N-1 | "Quel trimestre a le plus progressé ?" |

---

### Page 3 — Performance des Agents (Vue multi-années)

**Objectif :** Identifier les meilleurs agents sur 4 ans et détecter les évolutions de performance individuelle.

| Visuel | Contenu | Décision |
|--------|---------|---------|
| Tableau classement | Top 10 agents par Score_Agent | Primes de performance |
| Scatter plot | Axe X : Volume · Axe Y : Taux abandon | "Agents efficaces vs. problématiques" |
| Courbe par agent | Évolution de la durée moy. sur 4 ans | "Qui s'est amélioré ? Qui régresse ?" |
| Slicer | Année · Type d'appel | Filtrage dynamique |

> **Décision métier rendue possible :** "L'agent X a maintenu un taux d'abandon < 3% et une durée d'appel constante sur 4 ans, même en 2021 (télétravail). Il est candidat naturel à une promotion de superviseur."

---

### Page 4 — Qualité de Service et Risques

**Objectif :** Suivre les indicateurs critiques de qualité — avec alertes visuelles.

| KPI | Seuil d'alerte | Couleur |
|-----|---------------|---------|
| Taux d'abandon | > 5% | Rouge |
| Temps d'attente moy. | > 60 sec | Orange |
| Taux de service | < 95% | Rouge |
| Durée moy. appel | > 15 min | Orange |

```
┌──────────────────────────────────────────────────────────────┐
│   TAUX DE SERVICE   │   TAUX D'ABANDON    │   ATTENTE MOY.  │
│     [Jauge 95%]     │    [Jauge 5%]       │   [Jauge 60s]   │
├──────────────────────────────────────────────────────────────┤
│   [Courbe : Taux d'abandon mensuel — 2018 à 2021]            │
├──────────────────────────────────────────────────────────────┤
│   [Table : Mois avec taux d'abandon > 5% — liste rouge]      │
└──────────────────────────────────────────────────────────────┘
```

---

### Page 5 — Analyse des Coûts Opérationnels

**Objectif :** Piloter les coûts et identifier les leviers d'optimisation.

| Visuel | Contenu |
|--------|---------|
| Waterfall chart | Décomposition du coût : Type 1 + Type 2 + Type 3 = Total |
| Courbe | Évolution du coût total mensuel sur 4 ans |
| Tableau | Coût par agent en 2021 |
| Carte KPI | Coût moyen par appel 2021 vs 2020 |

---

### Page 6 — Plan d'Action 2022

**Objectif :** Transformer l'analyse en recommandations concrètes et actionnables.

| Recommandation | Basée sur | Priorité |
|---------------|----------|---------|
| Former les 5 agents avec durée moy. > 12 min | Score_Agent faible | Haute |
| Renforcer les équipes le mardi entre 14h–17h | Heatmap horaire | Haute |
| Réduire les appels Type 3 (réclamations) par amélioration service | Taux abandon Type 3 | Moyenne |
| Évaluer la numérisation des demandes Type 1 (renseignements) | Coût par type | Basse |

---

## 11. Visuels Power BI utilisés

| Visuel | Choix justifié | Question résolue |
|--------|---------------|-----------------|
| **Carte KPI (card)** | Affichage immédiat d'un chiffre clé | "Quel est notre taux d'abandon ?" |
| **Courbe (line chart)** | Comparaison temporelle multi-séries | "Comment évolue le volume année par année ?" |
| **Barres groupées (clustered bar)** | Comparaison par catégories | "Quel type d'appel est le plus coûteux ?" |
| **Scatter plot** | Relation entre deux métriques | "Y a-t-il un lien entre volume et abandon ?" |
| **Matrice (heatmap)** | Croisement heure × jour | "Quelle est l'heure de pointe le lundi ?" |
| **Jauge (gauge)** | Comparaison à un objectif visuel | "Sommes-nous au-dessus de 95% de service ?" |
| **Waterfall (cascade)** | Décomposition d'une somme | "Quelle part du coût vient de chaque type ?" |
| **Slicer (segment)** | Filtrage interactif | Navigation autonome du manager |
| **Table détaillée** | Liste granulaire | "Donnez-moi les 10 pires mois en termes d'abandon" |
| **Donut chart** | Répartition proportionnelle | "Quelle est la part de chaque type d'appel ?" |

---

## 12. Analyse métier produite

### Insights clés potentiels de l'analyse 4 ans

**1. Impact COVID-19 sur le volume 2020 vs 2021**  
La comparaison 2020–2021 peut révéler si la crise a créé un pic durable de demandes ou si 2021 est un retour à la normale 2019.

**2. Agents stables vs. agents volatils**  
Certains agents maintiennent leur performance sur 4 ans, d'autres montrent des variations importantes. Ce pattern oriente les actions RH.

**3. Saisonnalité récurrente**  
Si les mois de janvier et décembre sont systématiquement plus chargés sur 2018, 2019, 2020 et 2021, c'est une saisonnalité confirmée — pas un accident.

**4. Types d'appels et coûts**  
Les Type 3 (réclamations) sont souvent les plus longs et les plus coûteux. S'ils représentent 30% du volume mais 50% des coûts, c'est un levier d'optimisation majeur.

**5. Efficience vs. qualité**  
Le scatter plot (volume × taux d'abandon) révèle un paradoxe fréquent : certains agents traitent beaucoup d'appels mais au prix d'un fort taux d'abandon. Sont-ils vraiment efficaces ?

---

## 13. Résultats attendus

> *Résultats hypothétiques réalistes — données simulées à des fins pédagogiques.*

| Indicateur | Valeur simulée | Interprétation |
|-----------|--------------|----------------|
| Volume total 4 ans | ≈ 132 000 appels | Environ 33 000 appels/an — stable |
| Taux d'abandon moyen | ≈ 4,2% | Légèrement sous le seuil d'alerte de 5% |
| Durée moy. d'un appel | ≈ 8,5 minutes | Variable selon le type (Type 3 > 10 min) |
| Coût opérationnel total | ≈ 410 000 € sur 4 ans | Environ 100 000 € par an |
| Évolution volume 2021/2020 | ≈ +2,3% | Croissance légère — retour post-COVID |
| Agents au-dessus de la moyenne | ≈ 40% | 60% nécessitent un accompagnement ciblé |
| Mois de pointe | Janvier / Octobre | Saisonnalité à anticiper |

> **Résultats de l'examen :** Ce tableau de bord a été soumis et validé dans le cadre de l'examen Power BI de la formation DataScientest. Il démontre la maîtrise autonome de l'ensemble de la chaîne Power BI : données → Power Query → modèle → DAX → visualisation → insight.

---

## 14. Valeur métier

| Valeur | Impact |
|--------|--------|
| **Vue 4 ans consolidée** | Historique jamais accessible avant sous une forme unique |
| **Comparaison automatique N/N-1** | Plus besoin de calculer manuellement les écarts |
| **Classement agents objectif** | Fini les évaluations basées sur la subjectivité |
| **Alertes visuelles** | Un rouge sur une jauge, et le manager sait immédiatement où agir |
| **Page plan d'action** | Le dashboard ne s'arrête pas à l'analyse — il produit des recommandations |
| **Reproductibilité** | Ajouter 2022 prend moins de 30 minutes (simple extension du modèle) |
| **Crédibilité des chiffres** | Tous les managers partagent la même source de vérité |

---

## 15. Limites du projet

| Limite | Explication | Atténuation possible |
|--------|-------------|---------------------|
| **Format hétérogène** | CSV 2018-2020 + Excel 2021 crée un risque de désalignement | Documenter clairement les transformations Power Query |
| **Absence de données client** | On ne sait pas quel client a appelé | Connecter un CRM pour enrichir |
| **Pas de données qualitatives** | Pas de note de satisfaction client (CSAT) | Intégrer des enquêtes post-appel |
| **Coûts simulés** | Le tarif par minute est hypothétique | Utiliser les coûts réels si disponibles |
| **Pas de RLS** | Tous les agents sont visibles par tous | Implémenter RLS par équipe |
| **Mise à jour manuelle** | Les données ne se rafraîchissent pas automatiquement | Déployer sur Power BI Service avec gateway |
| **Définition "Call Type"** | Les codes 1, 2, 3 ne sont pas explicités dans les données brutes | Documenter dans le dictionnaire |

---

## 16. Améliorations possibles

| Amélioration | Technologie | Bénéfice attendu |
|-------------|-------------|-----------------|
| **Prédiction du volume 2022** | Python (Prophet) + Power BI | Anticiper les besoins en personnel |
| **Score de satisfaction simulé** | DAX + règles métier | Approximation du CSAT sans enquête |
| **Connexion SQL Server** | Power BI DirectQuery | Données en temps réel |
| **Alertes automatiques** | Power BI Service + Power Automate | Notification email si taux > 5% |
| **RLS par superviseur** | Power BI Desktop + Service | Chaque chef d'équipe ne voit que ses agents |
| **Scoring prédictif des agents** | Python sklearn | Identifier les agents à risque de départ |
| **API REST** | M Language + Power Query | Intégration de données CRM en live |
| **Mobile** | Power BI Mobile | Accès depuis terrain / réunions |
| **Tableau de bord de formation** | DAX + Progress tracking | Suivre l'efficacité des formations sur la durée |

> **RLS** (Row-Level Security — sécurité au niveau de la ligne, permettant à chaque utilisateur de ne voir que les données auxquelles il a accès) est une priorité pour un déploiement en production, car les données de performance individuelle sont sensibles.

---

## 17. Version CV

> À intégrer dans la section **Projets** ou **Réalisations** d'un CV Data Analyst / Data Scientist :

---

**Tableau de Bord Power BI — Examen Certifiant — Centre d'Appels 4 ans** *(2024)*  
*DataScientest — Formation certifiante Data Scientist — Examen final Power BI*

Construction autonome sous contrainte de temps d'un tableau de bord Power BI décisionnel sur ≈ 132 000 appels (2018–2021) : intégration de sources hétérogènes (CSV + Excel) via Power Query, modélisation en étoile avec table de calendrier et 4 dimensions, 18 mesures DAX incluant l'intelligence temporelle (TOTALYTD, SAMEPERIODLASTYEAR, YoY), scoring composite des agents, détection automatique des anomalies qualité de service. Dashboard 6 pages avec jauges d'alerte, scatter plots de performance et page de recommandations actionnables.

**Compétences démontrées :** Power BI · Power Query (M) · DAX avancé · Intelligence temporelle · Modélisation étoile · Scoring agent · Data Storytelling · KPI décisionnel · Analyse multi-années

---

## 18. Version entretien (2 minutes)

> Réponse à la question : *"Parlez-moi de votre examen Power BI — qu'avez-vous réalisé et pourquoi c'est significatif ?"*

---

"Pour valider ma formation Power BI, j'ai réalisé un examen en conditions réelles : on m'a donné un jeu de données inédit — les données 2021 d'un centre d'appels, à intégrer dans un modèle déjà construit pour 2018–2020 — et j'avais un temps limité pour livrer un tableau de bord professionnel.

Le défi principal était technique : les données 2021 étaient en format Excel, alors que les années précédentes étaient en CSV. J'ai dû adapter ma requête Power Query pour gérer les deux formats et les fusionner proprement en une seule table de 132 000 lignes, sans perdre la traçabilité de l'origine des données.

Sur le modèle de données, j'ai maintenu la structure en étoile avec une table de calendrier — c'est ce qui m'a permis d'utiliser les fonctions d'intelligence temporelle de DAX : TOTALYTD pour le cumul annuel, SAMEPERIODLASTYEAR pour comparer 2021 à 2020 au même mois, et des mesures YoY pour détecter les anomalies.

Ce que je retiens de cet examen, c'est que la vraie compétence Power BI ne se mesure pas sur la connaissance des visuels — elle se mesure sur la capacité à construire un modèle robuste sous pression, et à transformer des données brutes en une page de recommandations actionnables. C'est ce que j'ai livré : non pas juste un tableau de bord joli, mais un outil de décision pour le management."

---

## 19. Structure GitHub

```
cas-usage-2-examen/
├── README.md                              ← Ce fichier (documentation complète)
├── data_sample/
│   ├── Call_Center_Data_2018_sample.csv   ← 100 premières lignes
│   ├── Call_Center_Data_2019_sample.csv
│   ├── Call_Center_Data_2020_sample.csv
│   └── schema_donnees.md                 ← Dictionnaire des colonnes + 2021
├── powerbi/
│   └── README.md                         ← Instructions pour ouvrir le .pbix
├── dax/
│   └── mesures_dax.md                    ← Les 18 mesures DAX documentées
├── docs/
│   ├── architecture_modele.md            ← Schéma du modèle enrichi
│   ├── dictionnaire_donnees.md           ← Description complète 4 tables
│   └── guide_utilisateur.md             ← Comment naviguer dans le dashboard
└── screenshots/
    └── (captures du tableau de bord final)
```

---

## 20. Version LinkedIn

> Post à publier sur LinkedIn :

---

**J'ai passé mon examen Power BI — voici ce que construire sous pression m'a appris**

Dans le cadre de ma certification Data Science (DataScientest), j'ai réalisé un examen Power BI en conditions réelles.

**Le contexte :**
Données inédites à analyser. Temps limité. Pas de guide. Un tableau de bord professionnel à livrer.

**Ce que j'ai construit :**
Un tableau de bord décisionnel sur 132 000 appels (2018–2021), incluant :
- Fusion de sources hétérogènes (CSV + Excel) via Power Query
- Modèle en étoile avec 5 tables et table de calendrier
- 18 mesures DAX : intelligence temporelle, scoring agent, analyse de coûts
- 6 pages : synthèse exécutive, temporel, agents, qualité, coûts, recommandations

**Ce que cet examen m'a appris :**
La valeur d'un Data Analyst ne vient pas de la maîtrise des outils. Elle vient de la capacité à transformer un problème métier flou en une architecture de données solide, et en insights actionnables pour le management.

Un dashboard qu'on ne consulte pas n'a pas de valeur. Un dashboard qui change une décision, oui.

Projet complet disponible sur GitHub.

---

*#PowerBI #DataAnalysis #BusinessIntelligence #DAX #DataScience #Portfolio*

---

## 21. Questions d'entretien et réponses

*(Questions spécifiques à l'examen, en complément du Cas d'Usage 1)*

### Q1 — Comment avez-vous géré l'intégration de formats hétérogènes ?

**Réponse :** J'ai d'abord analysé les deux formats pour confirmer que les colonnes étaient identiques en nom et en type. Ensuite, dans Power Query, j'ai créé deux requêtes séparées — une pour les CSV avec `Csv.Document`, une pour l'Excel avec `Excel.Workbook`. Après avoir normalisé les types de colonnes et ajouté une colonne `Source_Fichier` pour la traçabilité, j'ai utilisé `Table.Combine` pour les fusionner. Le résultat est une table unique propre, sans perte d'information et avec une origine traçable pour chaque ligne.

---

### Q2 — Qu'est-ce que SAMEPERIODLASTYEAR et pourquoi est-ce utile ?

**Réponse :** `SAMEPERIODLASTYEAR` est une fonction d'intelligence temporelle DAX qui décale automatiquement le contexte de filtre d'un an en arrière. Quand l'utilisateur sélectionne mars 2021, la mesure `SAMEPERIODLASTYEAR` calcule automatiquement le résultat pour mars 2020 — sans que l'utilisateur ait à changer de filtre. C'est la clé pour une comparaison N/N-1 instantanée, là où Excel nécessiterait des formules DECALER complexes et fragiles.

---

### Q3 — Pourquoi avez-vous créé une table de calendrier séparée ?

**Réponse :** Une table de calendrier (D_Date) est obligatoire pour utiliser correctement les fonctions d'intelligence temporelle de DAX. Sans elle, Power BI ne "sait" pas que le 28 février et le 1er mars se suivent — il traite les dates comme des valeurs isolées. La table de calendrier crée un axe de temps continu et complet, ce qui permet de calculer des cumuls YTD, des comparaisons SPLY, des moyennes mobiles. C'est une règle d'or en modélisation Power BI : toujours avoir une table de calendrier dédiée, créée avec la fonction `CALENDAR()`.

---

### Q4 — Comment vous êtes-vous assuré que votre modèle était correct ?

**Réponse :** Trois vérifications. D'abord, j'ai comparé les totaux Power BI aux totaux Excel des fichiers source — si les chiffres correspondent, le modèle est correct. Ensuite, j'ai testé chaque mesure DAX sur un sous-ensemble connu — par exemple, filtrer sur janvier 2021 et vérifier manuellement le résultat. Enfin, j'ai vérifié que les relations entre tables ne produisaient pas d'ambiguïté — Power BI signale les relations ambiguës en jaune dans la vue Modèle.

---

### Q5 — Comment présenteriez-vous ce dashboard à un manager non technique ?

**Réponse :** Je ne commencerais pas par le dashboard — je commencerais par la question métier. "Vous voulez savoir si 2021 a été une bonne ou mauvaise année pour votre centre d'appels ?" Puis je leur montrerais la page 1 — deux chiffres : le taux de service et le volume. Seulement s'ils veulent en savoir plus, je plonge dans les pages suivantes. Un dashboard pour un manager non technique doit répondre à une question en 10 secondes. Si ça prend plus longtemps, c'est que le dashboard n'est pas assez bien conçu.

---

*Fin du cas d'usage 2 — Examen Power BI*  
*Emmanuel TSAGUE | Data Scientist / Data Analyst*  
*Examen certifiant — DataScientest 2024*
