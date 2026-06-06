# Architecture du Modèle de Données — Cas d'Usage 2 (Examen)
## Centre d'Appels 2018–2021

**Auteur :** Emmanuel TSAGUE  
**Spécificité :** Modèle enrichi avec 4 années de données et intelligence temporelle avancée

---

## Schéma du modèle en étoile — Version Examen

```
                    ┌──────────────────┐
                    │     D_Date       │
                    │  Calendrier      │
                    │  2018 – 2021     │
                    │  (1 461 lignes)  │
                    │                  │
                    │ PK: Date         │
                    │ Année            │
                    │ Trimestre        │
                    │ Mois_Num         │
                    │ Mois_Libellé     │
                    │ Jour_Semaine     │
                    └────────┬─────────┘
                             │ 1
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
    1     │            N     │            1     │
┌─────────┴──┐   ┌──────────┴──────────┐  ┌────┴──────────┐
│ D_Agents   │───│    F_Appels         │──│ D_TypeAppel   │
│            │ N │  (Table de faits)   │N │               │
│ PK: EmpID  │   │  132 000 lignes     │  │ PK: CallType  │
│ Nom_Agent  │   │                     │  │ Libellé       │
│ Équipe     │   │ FK: CallTimestamp   │  └───────┬───────┘
└────────────┘   │ FK: EmployeeID      │          │ 1
                 │ FK: Call Type       │          │
                 │ CallDuration        │          │ N
                 │ CallDuration_Min    │  ┌───────┴───────┐
                 │ WaitTime            │──│  D_Charges    │
                 │ CallAbandoned       │N │               │
                 │ Année               │  │ PK: CallType  │
                 │ Mois_Num            │  │ Tarif_/Min    │
                 │ Heure               │  └───────────────┘
                 │ Source_Fichier      │
                 └─────────────────────┘
```

---

## Flux de données — Version Examen (multi-sources)

```
Sources hétérogènes
        │
        ▼
[Power Query — Transformation]
        │
        ├── CSV 2018 ──────┐
        ├── CSV 2019 ──────┤
        ├── CSV 2020 ──────┼─► Normalisation ──► Append ──► F_Appels
        └── XLSX 2021 ─────┘              (132 000 lignes)
                                                    │
        ├── Lookup_Tables.xlsx ──────────────► D_Agents + D_TypeAppel
        └── Call_Charges.xlsx  ──────────────► D_Charges
        │
        ▼
[Modèle Power BI — Relations + DAX]
        │
        ├── D_Date (créée en DAX — étendue à 2021)
        ├── 4 relations N→1
        └── 18 mesures DAX (dont intelligence temporelle)
        │
        ▼
[Dashboard 6 pages]
        │
        ├── Page 1 : Synthèse Exécutive
        ├── Page 2 : Temporel Avancé
        ├── Page 3 : Performance Agents
        ├── Page 4 : Qualité de Service
        ├── Page 5 : Coûts Opérationnels
        └── Page 6 : Plan d'Action 2022
```

---

## Décisions architecturales spécifiques à l'examen

| Décision | Justification | Impact |
|---------|--------------|--------|
| Colonne `Source_Fichier` ajoutée | Traçabilité de l'origine CSV vs Excel | Débogage facilité |
| D_Date étendue à 2021 | Prérequis pour `SAMEPERIODLASTYEAR` sur 2021 | Intelligence temporelle fonctionnelle |
| Table de faits unique (pas 4 tables) | Simplicité DAX, filtrage unifié | Performance améliorée |
| `BLANK()` dans les mesures YoY | Évite d'afficher "0%" pour 2018 (pas de N-1) | Lisibilité du dashboard |
| Score_Agent composite | Évite les classements mono-critère trompeurs | Évaluation plus juste |

---

## Performance du modèle

| Aspect | Valeur | Commentaire |
|--------|--------|-------------|
| Mode de stockage | Import | Optimal pour 132 000 lignes |
| Mémoire estimée | < 50 MB | Bien dans les limites Power BI Desktop |
| Temps de rafraîchissement | < 30 secondes | Acceptable en production |
| Nombre de relations | 4 | Modèle simple et lisible |
| Mesures DAX | 18 | Toutes documentées |

---

*Emmanuel TSAGUE — Data Scientist / Data Analyst — Examen DataScientest 2024*
