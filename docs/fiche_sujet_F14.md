# Fiche sujet F14 – Taux hypothécaire et taux à 10 ans

**Établissement :** ENCG Settat  
**Filière :** Finance et Contrôle Interne  
**Semestre :** S7  
**Module :** Bases de Données et Data Science  
**Année universitaire :** 2026–2027  
**Type de travail :** Individuel  
**Étudiant :** Jawahir Abdelkebir  

## Titre du sujet

Taux hypothécaire et taux à 10 ans

## Problématique principale

Dans quelle mesure le rendement des obligations américaines à 10 ans permet-il d’expliquer et de prévoir l’évolution du taux hypothécaire américain à 30 ans ?

## Objectif mesurable

Construire un modèle de régression linéaire simple capable d’estimer le taux hypothécaire à 30 ans à partir du rendement des obligations du Trésor américain à 10 ans, et le comparer à une référence naïve (moyenne d’entraînement).

## Variable cible

- **Mortgage_30Y** : taux moyen des prêts hypothécaires à taux fixe de 30 ans aux États-Unis (série FRED MORTGAGE30US), en pourcentage.

## Variable explicative principale

- **Treasury_10Y** : rendement de marché des titres du Trésor américain à maturité constante de 10 ans (série FRED DGS10), en pourcentage.

## Nature de la tâche

Régression supervisée (prédiction d’une variable continue).

## Algorithme principal obligatoire

Régression linéaire simple (Linear Regression – algorithme n°1 du support de cours).

## Algorithmes complémentaires (comparaison)

- Régression polynomiale (degré 2)
- Régression Ridge

## Métrique principale (fixée a priori)

RMSE (Root Mean Squared Error)

## Métriques secondaires

MAE (Mean Absolute Error), R² (coefficient de détermination)

## Sources de données

- MORTGAGE30US : https://fred.stlouisfed.org/series/MORTGAGE30US  
- DGS10 : https://fred.stlouisfed.org/series/DGS10  

Organisme producteur principal : Federal Reserve Bank of St. Louis (FRED), données originelles Freddie Mac (PMMS) pour le taux hypothécaire et Board of Governors of the Federal Reserve System (H.15) pour DGS10.
