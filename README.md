# Projet F14 – Taux hypothécaire et taux à 10 ans

**Établissement :** ENCG Settat  
**Filière :** Finance et Contrôle Interne – S7  
**Module :** Bases de Données et Data Science  
**Année :** 2026–2027  
**Étudiant :** Jawahir Abdelkebir  
**Sujet :** F14 – Taux hypothécaire et taux à 10 ans  

## Objectif financier et problématique

**Problématique :**  
Dans quelle mesure le rendement des obligations américaines à 10 ans (DGS10) permet-il d’expliquer et de prévoir l’évolution du taux hypothécaire américain à 30 ans (MORTGAGE30US) ?

**Objectif mesurable :**  
Construire un modèle de régression linéaire simple estimant le taux hypothécaire à partir du taux à 10 ans, et le comparer à une référence naïve (moyenne d’entraînement), en respectant strictement la chronologie des données.

## Données

| Série          | Code FRED     | Fréquence   | Unité      | Source principale          |
|----------------|---------------|-------------|------------|----------------------------|
| Taux hypothécaire 30 ans | MORTGAGE30US | Hebdomadaire | %         | Freddie Mac via FRED      |
| Rendement Trésor 10 ans  | DGS10        | Quotidienne | %         | Federal Reserve (H.15) via FRED |

- Période utilisée après alignement : **1971-04-02 → 2026-11-12**
- Nombre d’observations alignées : **2 903**
- Fichiers bruts : `data/raw/MORTGAGE30US.csv` et `data/raw/DGS10.csv`
- Documentation des sources et licences : `docs/sources_donnees.md`

## Algorithme principal

**Régression linéaire simple**

\[
\text{Mortgage\_30Y} = \beta_0 + \beta_1 \times \text{Treasury\_10Y} + \varepsilon
\]

- Comparaisons : régression polynomiale (degré 2) et Ridge.

## Méthodologie (résumé)

1. Alignement des fréquences sans fuite d’information (moyenne du DGS10 calculée uniquement avec les observations ≤ date de référence du mortgage).
2. Construction des variables : Treasury_10Y, Mortgage_30Y, Mortgage_Spread, Treasury_10Y_Lag1.
3. Découpage chronologique : 70 % train / 15 % validation / 15 % test.
4. Référence naïve : moyenne du Mortgage_30Y sur le train uniquement.
5. Évaluation sur le test : RMSE (principale), MAE, R².

Détails : `docs/methode_pretraitement.md` et notebook.

## Résultats principaux (période de test 2018-07 → 2026-11)

| Modèle                  | RMSE   | MAE    | R²     |
|-------------------------|--------|--------|--------|
| Référence naïve         | 4,12   | 3,79   | −5,60  |
| Régression linéaire     | **0,80** | **0,69** | **0,75** |
| Régression polynomiale  | 0,66   | 0,59   | 0,83   |
| Ridge                   | 0,80   | 0,68   | 0,75   |

- Coefficients du modèle linéaire : β₀ ≈ 1,34 ; β₁ ≈ 1,05  
- Corrélation globale Mortgage / Treasury ≈ 0,987  
- Interprétation : une hausse de 1 point du taux à 10 ans est associée, en moyenne, à une hausse d’environ 1,05 point du taux hypothécaire.

Les graphiques sont dans `figures/`.

## Structure du dépôt

```
CodeApogee-Abdelkebir-F14/
├── README.md
├── requirements.txt
├── .gitignore
├── DECLARATION_IA.md
├── data/
│   ├── raw/
│   │   ├── MORTGAGE30US.csv
│   │   └── DGS10.csv
│   ├── processed_dataset.csv
│   ├── test_predictions.csv
│   └── metrics.json
├── docs/
│   ├── fiche_sujet_F14.md
│   ├── sources_donnees.md
│   ├── methode_pretraitement.md
│   └── script_soutenance.md
├── notebooks/
│   └── projet_F14.ipynb
├── figures/
│   ├── 01_evolution_historique.png
│   ├── 02_nuage_points_tendance.png
│   ├── 03_spread_hypothecaire.png
│   ├── 04_predictions_test.png
│   ├── 05_residus_test.png
│   └── 06_distributions.png
├── rapport/
│   └── rapport.pdf
├── presentation/
│   ├── presentation.pptx
│   └── presentation.pdf
└── video/
    └── script_video.md
```

## Installation et exécution

```bash
# Cloner le dépôt
git clone <url-du-depot>
cd CodeApogee-Abdelkebir-F14

# Créer un environnement (recommandé)
python -m venv venv
source venv/bin/activate   # Linux/Mac
# ou venv\Scripts\activate  # Windows

# Installer les dépendances
pip install -r requirements.txt

# Lancer le notebook
jupyter notebook notebooks/projet_F14.ipynb
# ou
jupyter lab notebooks/projet_F14.ipynb
```

Le notebook est conçu pour être exécuté de bout en bout avec des chemins relatifs.

## Vidéo explicative

Un script détaillé est fourni dans `video/script_video.md` (60–90 s).  
L’étudiant doit enregistrer lui-même la capture d’écran commentée et déposer `video/demonstration.mp4` ou indiquer un lien public ici :

> **Lien vidéo :** [à compléter par l’étudiant]

## Références

- FRED – Federal Reserve Bank of St. Louis : https://fred.stlouisfed.org/
- Freddie Mac Primary Mortgage Market Survey
- Documentation scikit-learn : Linear Regression
- Support de cours « Bases de Données et Data Science » – ENCG Settat 2026–2027

## Déclaration IA

Voir `DECLARATION_IA.md`. L’IA a servi d’assistance à la structuration, au code et à la rédaction ; les résultats et interprétations ont été vérifiés par l’étudiant.
