# Guide Utilisateur — Tableau de Bord Power BI Examen
## Centre d'Appels 2018–2021

**Auteur :** Emmanuel TSAGUE  
**Niveau :** Dashboard avancé — 6 pages, intelligence temporelle, scoring agent

---

## Navigation rapide

| Si vous voulez... | Aller à la page... |
|-------------------|-------------------|
| Vision globale 4 ans | Page 1 — Synthèse Exécutive |
| Comparer mois par mois avec l'année dernière | Page 2 — Temporel Avancé |
| Voir qui sont les meilleurs agents | Page 3 — Performance Agents |
| Vérifier le taux d'abandon | Page 4 — Qualité de Service |
| Contrôler les coûts | Page 5 — Coûts Opérationnels |
| Savoir quoi faire en 2022 | Page 6 — Plan d'Action |

---

## Les indicateurs clés à surveiller

### Feux tricolores (interprétation rapide)

| Indicateur | Vert ✅ | Orange ⚠️ | Rouge 🔴 |
|-----------|--------|----------|---------|
| Taux de service | ≥ 95% | 90–95% | < 90% |
| Taux d'abandon | < 3% | 3–5% | > 5% |
| Temps d'attente moy. | < 30 sec | 30–60 sec | > 60 sec |
| Variation YoY volume | -5% à +10% | +10% à +20% | > +20% (risque surcharge) |
| Durée moy. appel | < 10 min | 10–15 min | > 15 min |

---

## Interpréter la Page 3 — Scoring Agent

Le **Score Agent** est un indicateur composite (un indicateur qui combine plusieurs métriques en un seul chiffre). Un score élevé signifie que l'agent traite beaucoup d'appels avec peu d'abandons et une durée raisonnable.

**Comment l'utiliser :**
1. Trier le tableau par Score Agent (colonne Score) en ordre décroissant
2. Les agents dans le top 20% → candidats pour mentoring ou promotion
3. Les agents dans le bottom 20% → analyse individuelle, formation ciblée

> **Attention :** Un score ne remplace pas un entretien humain. Il oriente l'attention, il ne décide pas à la place du manager.

---

## Interpréter la Page 6 — Plan d'Action

Cette page transforme les données en recommandations. Elle liste les 4 leviers d'action prioritaires pour 2022, classés par impact potentiel.

**Comment l'utiliser :**
1. Valider les recommandations avec votre connaissance terrain
2. Prioriser celles qui correspondent à vos contraintes budgétaires
3. Définir un responsable et une date pour chaque action
4. Revenir au dashboard dans 3 mois pour mesurer l'effet

---

## Utiliser les comparaisons N/N-1

Sur la Page 2, vous verrez des graphiques avec deux courbes superposées (par exemple, 2021 en bleu et 2020 en gris). Voici comment les lire :

- Si la courbe 2021 est **au-dessus** de 2020 → volume plus élevé cette année
- Si la courbe 2021 est **en dessous** → moins d'appels que l'an dernier
- Les **écarts saisonniers récurrents** (même forme de courbe chaque année) indiquent une saisonnalité réelle

---

## Mise à jour des données

Pour ajouter les données 2022 lorsqu'elles seront disponibles :

1. Déposer le fichier `Call_Center_Data_2022.csv` (ou `.xlsx`) dans le même dossier
2. Ouvrir Power Query (Accueil → Transformer les données)
3. Dans la requête `F_Appels`, ajouter une nouvelle source et l'ajouter à la liste Append
4. Étendre `D_Date` : changer `DATE(2021, 12, 31)` en `DATE(2022, 12, 31)`
5. Fermer et appliquer → Actualiser

**Durée estimée :** 20–30 minutes

---

*Emmanuel TSAGUE — Data Scientist / Data Analyst — Examen DataScientest 2024*
