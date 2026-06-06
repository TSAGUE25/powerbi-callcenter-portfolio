# Architecture du Modèle de Données — Cas d'Usage 1 (Cours)
## Centre d'Appels 2018–2020

**Auteur :** Emmanuel TSAGUE  
**Outil :** Power BI Desktop — Vue Modèle

---

## Schéma du modèle en étoile

```
                        ┌─────────────┐
                        │   D_Date    │
                        │  (Calendrier│
                        │  2018-2020) │
                        │             │
                        │ PK: Date    │
                        └──────┬──────┘
                               │ 1
                               │
           ┌───────────────────┼───────────────────┐
           │                   │                   │
     1     │             N     │             1     │
┌──────────┴──┐    ┌───────────┴──────┐    ┌───────┴──────┐
│  D_Agents   │────│    F_Appels      │────│ D_TypeAppel  │
│  (Employés) │N   │  (Table de faits │  N │  (Types 1,2,3│
│             │    │   98 975 lignes) │    │              │
│ PK: EmpID   │    │                  │    │ PK: CallType │
└─────────────┘    │ FK: CallTimestamp│    └──────────────┘
                   │ FK: EmployeeID   │           │ 1
                   │ FK: Call Type    │           │
                   │ CallDuration     │           │ N
                   │ WaitTime         │    ┌──────┴──────┐
                   │ CallAbandoned    │────│ D_Charges   │
                   └──────────────────┘  N │ (Tarifs)    │
                                           │             │
                                           │ PK: CallType│
                                           └─────────────┘
```

---

## Détail des relations

| Relation | Table Gauche | Clé Gauche | Cardinalité | Table Droite | Clé Droite |
|---------|-------------|-----------|------------|-------------|-----------|
| R1 | F_Appels | `CallTimestamp` (date) | N → 1 | D_Date | `Date` |
| R2 | F_Appels | `EmployeeID` | N → 1 | D_Agents | `EmployeeID` |
| R3 | F_Appels | `Call Type` | N → 1 | D_TypeAppel | `Call Type` |
| R4 | F_Appels | `Call Type` | N → 1 | D_Charges | `Call Type` |

> **Cardinalité N → 1 :** Un appel (N) est associé à un seul agent (1). Un agent peut être associé à plusieurs appels. C'est la cardinalité standard dans un modèle en étoile.

---

## Flux des données

```
Sources brutes
     │
     ▼
[Power Query — Nettoyage & Transformation]
     │
     ├── CSV 2018 ─┐
     ├── CSV 2019 ─┼─► Fusion (Append) ─► F_Appels (nettoyée)
     ├── CSV 2020 ─┘
     │
     ├── Lookup_Tables.xlsx ──────────────► D_Agents + D_TypeAppel
     └── Call_Charges.xlsx  ──────────────► D_Charges
     │
     ▼
[Modèle Power BI — Relations & Calculs DAX]
     │
     ├── D_Date (créée en DAX)
     ├── Relations entre tables
     └── Mesures DAX (12 mesures)
     │
     ▼
[Tableau de bord — 5 Pages]
     │
     ├── Page 1 : Vue Exécutive
     ├── Page 2 : Analyse Temporelle
     ├── Page 3 : Performance Agents
     ├── Page 4 : Qualité de Service
     └── Page 5 : Analyse Coûts
```

---

## Décisions d'architecture et justifications

| Décision | Alternative envisagée | Raison du choix |
|---------|----------------------|----------------|
| Modèle en étoile | Tout dans une table plate | Performance, lisibilité DAX, extensibilité |
| Table D_Date dédiée | Extraire les dates de F_Appels | Obligatoire pour les fonctions d'intelligence temporelle DAX |
| Fusion des 3 CSV dans une table | 3 tables séparées | Simplification des mesures DAX, filtrage unifié |
| DIVIDE() pour tous les taux | Division simple `/` | Gestion automatique des divisions par zéro |
| Types de données explicites | Laisser Power BI détecter | Évite les erreurs silencieuses de format |

---

## Paramètres du modèle Power BI

| Paramètre | Valeur | Raison |
|-----------|--------|--------|
| Mode de stockage | Import | Performances optimales pour 99 000 lignes |
| Langue du modèle | Français | Cohérence avec les libellés métier |
| Format des dates | JJ/MM/AAAA | Standard français |
| Format des monétaires | #,##0.00 € | Lisibilité euro |
| D_Date marquée comme "Table de dates" | Oui | Obligatoire pour SAMEPERIODLASTYEAR |

---

*Emmanuel TSAGUE — Data Scientist / Data Analyst — Formation DataScientest 2024*
