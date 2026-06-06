# Guide Utilisateur — Tableau de Bord Power BI
## Centre d'Appels 2018–2020

**Auteur :** Emmanuel TSAGUE  
**Pour :** Managers et superviseurs du centre d'appels  
**Prérequis :** Power BI Desktop (gratuit) ou accès Power BI Service

---

## Comment ouvrir le tableau de bord

### Option A — Power BI Desktop (recommandé pour les données locales)

1. Télécharger Power BI Desktop gratuitement sur [powerbi.microsoft.com](https://powerbi.microsoft.com)
2. Ouvrir le fichier `.pbix` fourni (disponible sur demande)
3. Si les données ne se chargent pas, mettre à jour le chemin source dans Power Query (menu Transformer les données → Source)

### Option B — Power BI Service (cloud Microsoft)

1. Se connecter sur [app.powerbi.com](https://app.powerbi.com)
2. Importer le fichier `.pbix` via "Obtenir des données"
3. Le dashboard est alors accessible depuis n'importe quel navigateur

---

## Navigation dans le tableau de bord

Le tableau de bord est organisé en **5 pages**. Cliquer sur le nom d'une page en bas de l'écran pour y accéder.

| Page | Nom | Je veux savoir... |
|------|-----|-------------------|
| 1 | Vue Exécutive | Comment nous performons globalement |
| 2 | Analyse Temporelle | Quand sommes-nous les plus chargés ? |
| 3 | Performance Agents | Qui sont les meilleurs agents ? |
| 4 | Qualité de Service | Notre taux d'abandon est-il acceptable ? |
| 5 | Analyse Coûts | Combien coûte notre activité ? |

---

## Utiliser les filtres (slicers)

> Un **slicer** (ou segment) est un filtre interactif. Quand vous cliquez dessus, tous les visuels de la page se mettent à jour automatiquement.

### Filtres disponibles

| Filtre | Localisation | Valeurs possibles | Effet |
|--------|-------------|------------------|-------|
| **Année** | Toutes les pages | 2018, 2019, 2020 | Filtre toutes les données sur l'année choisie |
| **Trimestre** | Pages 1 et 2 | T1, T2, T3, T4 | Affine par trimestre |
| **Type d'appel** | Pages 2, 3, 4 | Type 1, Type 2, Type 3 | Analyse par nature de demande |
| **Agent** | Page 3 | Liste des agents | Zoom sur un agent spécifique |

### Comment utiliser un filtre

1. Cliquer sur une valeur dans le slicer (ex : "2019")
2. Tous les graphiques se mettent à jour
3. Pour sélectionner plusieurs valeurs : maintenir `Ctrl` et cliquer
4. Pour réinitialiser : cliquer sur l'icône ↺ en haut à droite du slicer

---

## Interpréter les visuels clés

### Carte KPI (les grands chiffres en haut)

```
┌─────────────┐
│   98 975    │  ← Valeur principale
│   Appels    │  ← Libellé
│   ↑ +2,3%  │  ← Variation vs. période précédente
└─────────────┘
```
- **Flèche verte ↑** : amélioration par rapport à la période précédente
- **Flèche rouge ↓** : dégradation à surveiller

### Jauge d'objectif

```
  Taux de service
      [======|==  ]
       92%    95%
              ↑ Objectif
```
- Si l'aiguille est **avant** l'objectif → situation à corriger
- Si l'aiguille est **après** l'objectif → objectif atteint ✓

### Courbe temporelle

- **Axe horizontal** : le temps (mois, trimestre)
- **Axe vertical** : la valeur mesurée (volume d'appels, durée, etc.)
- **Plusieurs courbes** : une par année — comparez facilement 2018 vs 2019 vs 2020

### Scatter plot (nuage de points) — Page 3 agents

- **Axe X (horizontal)** : Volume d'appels traités par l'agent
- **Axe Y (vertical)** : Taux d'abandon sur les appels de l'agent
- **Idéal** : points en bas à droite (beaucoup d'appels, peu d'abandons)
- **À surveiller** : points en haut (taux d'abandon élevé)

---

## Questions fréquentes des managers

**Q : Comment savoir si notre performance en 2020 est meilleure qu'en 2019 ?**  
R : Aller sur la Page 1. Le KPI "Évolution YoY" indique en % la variation du volume. Pour le taux d'abandon, regarder la Page 4 et comparer les courbes 2019 et 2020.

**Q : Quel agent a le meilleur taux de service ?**  
R : Aller sur la Page 3. Le tableau de classement agents montre les taux d'abandon par agent. Trier par cette colonne pour voir le meilleur et le moins bon.

**Q : À quelle heure doit-on renforcer les équipes ?**  
R : Aller sur la Page 2. La heatmap (grille heure × jour) montre en couleur foncée les créneaux les plus chargés.

**Q : Le Type 3 (réclamations) est-il vraiment plus long ?**  
R : Aller sur la Page 2 ou 3. Filtrer sur "Type 3" dans le slicer et noter la durée moy. Puis filtrer sur "Type 1" et comparer.

**Q : Comment exporter un graphique en image ?**  
R : Clic droit sur le visuel → "Exporter les données" ou faire une capture d'écran de la page.

---

## Contacts et support

| Question | Contact |
|---------|---------|
| Le fichier ne s'ouvre pas | Vérifier que Power BI Desktop est à jour |
| Les données ne correspondent pas | Vérifier la source des fichiers CSV dans Power Query |
| Ajouter une nouvelle année de données | Contacter Emmanuel TSAGUE |
| Ajouter un nouveau KPI | Contacter Emmanuel TSAGUE |

---

*Emmanuel TSAGUE — Data Scientist / Data Analyst — Formation DataScientest 2024*
