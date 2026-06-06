# Power BI — Portfolio de Compétences Data Analyst
### Emmanuel TSAGUE | Data Scientist / Data Analyst

---

> **Ce dépôt contient deux cas d'usage Power BI complets**, construits dans le cadre d'une formation certifiante en Data Science.  
> Ils illustrent ma maîtrise de Power BI, de la préparation des données jusqu'au tableau de bord décisionnel, dans un contexte opérationnel réel : **l'analyse de la performance d'un Centre d'Appels**.

---

## Profil

| Champ | Détail |
|-------|--------|
| **Nom** | Emmanuel TSAGUE |
| **Rôle visé** | Data Scientist / Data Analyst — Performance commerciale, Énergie, Risque client |
| **Compétences clés** | Power BI · DAX · Power Query · SQL · Python · Excel · Modélisation de données |
| **Domaines** | Énergie / EDF · Commerce · Pilotage par les indicateurs · Industrialisation Data |
| **Contact** | [LinkedIn](https://linkedin.com/in/) · [GitHub](https://github.com/) |

---

## Deux cas d'usage au programme

| # | Cas d'usage | Type | Données | Niveau |
|---|-------------|------|---------|--------|
| 1 | [Analyse de la Performance d'un Centre d'Appels — Cours Power BI](./cas-usage-1-cours/README.md) | Cours guidé | 2018–2020 (≈ 99 000 appels) | Fondamental → Avancé |
| 2 | [Tableau de Bord Décisionnel Multi-Années — Examen Power BI](./cas-usage-2-examen/README.md) | Examen autonome | 2018–2021 (≈ 132 000 appels) | Expert |

---

## Contexte commun aux deux cas

Un **centre d'appels** reçoit des milliers d'appels chaque mois. Chaque appel génère des données :
- Qui a appelé ? Quand ? Quel type de demande ?
- Combien de temps a duré l'appel ?
- Le client a-t-il attendu longtemps avant d'être pris en charge ?
- L'appel a-t-il été abandonné (raccroché avant réponse) ?

Ces données, si elles sont bien analysées, permettent de **piloter la qualité de service**, d'**optimiser les équipes** et de **réduire les coûts opérationnels**. C'est exactement ce que ces deux tableaux de bord Power BI accomplissent.

---

## Structure du dépôt

```
powerbi-callcenter-portfolio/
│
├── README.md                          ← Ce fichier (vue d'ensemble)
├── .gitignore
│
├── cas-usage-1-cours/                 ← Cas d'usage 1 : Cours
│   ├── README.md                      ← Documentation complète (24 sections)
│   ├── data_sample/                   ← Données d'exemple (100 lignes/année)
│   ├── powerbi/                       ← Instructions pour ouvrir le .pbix
│   ├── dax/                           ← Toutes les mesures DAX documentées
│   ├── docs/                          ← Architecture, dictionnaire, guide
│   └── screenshots/                   ← Captures du tableau de bord
│
└── cas-usage-2-examen/                ← Cas d'usage 2 : Examen
    ├── README.md                      ← Documentation complète (24 sections)
    ├── data_sample/                   ← Données d'exemple (100 lignes/année)
    ├── powerbi/                       ← Instructions pour ouvrir le .pbix
    ├── dax/                           ← Mesures DAX avancées documentées
    ├── docs/                          ← Architecture, dictionnaire, guide
    └── screenshots/                   ← Captures du tableau de bord
```

---

## Technologies utilisées

| Outil | Usage |
|-------|-------|
| **Power BI Desktop** | Construction des tableaux de bord |
| **Power Query (M)** | Nettoyage et transformation des données |
| **DAX** | Création des mesures et indicateurs dynamiques |
| **Excel / CSV** | Sources de données brutes |
| **Git / GitHub** | Versioning et partage du projet |

---

## Points forts de ce portfolio

- Modélisation en étoile (star schema) avec tables de faits et dimensions
- 15+ mesures DAX couvrant les KPI métier essentiels
- Tableau de bord multi-pages avec navigation intuitive
- Analyse temporelle sur 3 à 4 années glissantes
- Segmentation par type d'appel, agent, période
- Documentation professionnelle complète (dictionnaire, architecture, guide utilisateur)

---

## Comment utiliser ce dépôt

1. **Cloner** le dépôt : `git clone https://github.com/emmanueltsague/powerbi-callcenter-portfolio`
2. **Lire** le README de chaque cas d'usage
3. **Explorer** les données dans `data_sample/`
4. **Consulter** les mesures DAX dans `dax/`
5. **Ouvrir** le fichier `.pbix` avec Power BI Desktop (fichier disponible sur demande)

> **Note :** Les fichiers `.pbix` ne sont pas versionnés (trop lourds pour Git standard). Contactez-moi directement pour les obtenir, ou consultez les captures d'écran dans `screenshots/`.

---

*Projet réalisé dans le cadre de la formation Data Scientist — DataScientest (2024)*  
*Données simulées à des fins pédagogiques — aucune donnée client réelle.*
