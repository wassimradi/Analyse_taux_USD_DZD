# Analyse des déterminants du taux de change DZD

Analyse exploratoire des facteurs susceptibles d'expliquer les variations des taux de change **EUR/DZD** et **USD/DZD**, à partir de données macroéconomiques algériennes.

---

## Objectif

Le dinar algérien (DZD) est une monnaie administrée : son taux est fixé par la Banque d'Algérie, pas par un marché libre. Ce notebook cherche à identifier quelles variables macroéconomiques sont statistiquement associées aux mouvements du DZD, et si un modèle de régression linéaire peut les expliquer.

---

## Structure du notebook

| Section | Contenu |
|---|---|
| **1. Chargement des données** | Import depuis `data.xlsx`, mise en forme de l'index mensuel |
| **2. Matrices de corrélation sur niveaux** | Illustration du piège des tendances communes (régression fallacieuse) |
| **3. Matrices sur variations mensuelles** | Correction — seules les co-variations réelles sont retenues |
| **4. Tentative de régression OLS** | Modèle linéaire sur données mensuelles (2017–2023) |
| **5. Analyse et rejet du modèle** | Diagnostic des résultats : R², p-values, multicolinéarité |
| **6. Corrélations quotidiennes** | Test des retards du Brent (J−1, J−7, J−15, J−30) sur USD/DZD |

---

## Résultats clés

| Étape | Résultat |
|---|---|
| Corrélations sur niveaux | Trompeuses — corrélations artificiellement fortes dues aux tendances communes |
| Corrélations sur variations mensuelles | Seules les réserves de change (RSV) affichent un signal réel (−0,35) |
| Modèle OLS mensuel | R² ajusté = 9,4 % — 3 variables sur 4 non significatives — modèle rejeté |
| Retards quotidiens du Brent (J−1 à J−30) | Toutes les corrélations entre −0,04 et +0,05 — aucun signal détectable |

**Conclusion principale :** le Brent n'a pas d'effet détectable à court terme sur le fixing quotidien de l'USD/DZD. Son impact transite par les réserves de change avec un décalage de plusieurs mois, ce que la fréquence mensuelle avec les données RSV permettrait de modéliser — sous réserve de disposer de données RSV au-delà de 2023.

---

## Données utilisées

| Variable | Source | Fréquence | Couverture |
|---|---|---|---|
| EUR/DZD | Banque d'Algérie | Mensuelle | 01-2000 → 08-2026 |
| USD/DZD | Banque d'Algérie | Mensuelle & Quotidienne | 01-2000 → 08-2026 |
| IPC Algérie | Banque d'Algérie | Mensuelle | 01-2002 → 02-2026 |
| IPC États-Unis | U.S. Bureau of Labor Statistics | Mensuelle | 01-2002 → 02-2026 |
| Brent (spot mensuel) | U.S. Energy Information Administration | Mensuelle & Quotidienne | 01-2000 → 08-2026 |
| Taux directeur BA | Banque d'Algérie | Mensuelle | 03-2017 → 08-2026 |
| Réserves de change (RSV) | Banque d'Algérie | Mensuelle | 12-2015 → 12-2023 |

---

## Structure du dépôt

```
├── Correlation.ipynb                    # Notebook principal
├── Data/
│   └── Data.xlsx                        # Données mensuelles (feuilles : Data, Source)
│   └── Data quotidienne.xlsx            # Données quotidiennes (feuille : Data quotidienne)
└── README.md
```

---

## Dépendances

```bash
pip install pandas numpy matplotlib seaborn statsmodels openpyxl
```

---

## Contexte

Ce projet constitue la base analytique d'un outil d'aide à la décision en trésorerie, destiné à anticiper les mouvements du dinar dans le cadre d'opérations d'import payées en USD ou en EUR.

---

## Prochaines étapes

- Obtenir les données RSV au-delà de décembre 2023
- Tester la cointégration entre USD/DZD et les réserves de change
- Construire un modèle à correction d'erreur (MCE) si la cointégration est confirmée
- Intégrer l'analyse dans un tableau de bord Flask de suivi des taux

---

*Auteur : Wassim — Finance & Analyse de données | Algérie*
