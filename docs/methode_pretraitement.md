# Méthode de prétraitement et synchronisation des fréquences

## Problème de fréquences

- **MORTGAGE30US** : fréquence hebdomadaire (généralement date du jeudi de la semaine de publication).
- **DGS10** : fréquence quotidienne (jours ouvrés uniquement).

Une jointure naïve (même date) laisserait de nombreuses valeurs manquantes et risque d’utiliser des informations futures.

## Principe anti-fuite

Pour chaque date de référence du taux hypothécaire (semaine t), on calcule la moyenne du rendement à 10 ans en utilisant **uniquement les observations de DGS10 disponibles jusqu’à cette date inclusive**.  
Aucune observation de DGS10 postérieure à la date de référence du mortgage n’est utilisée.

## Étapes concrètes

1. Charger les deux CSV avec `parse_dates` et conversion des « . » en NaN.
2. Trier strictement par date croissante.
3. Pour chaque date `d` de MORTGAGE30US :
   - Sélectionner les observations DGS10 telles que `date_DGS10 ≤ d`.
   - Calculer la moyenne de ces valeurs sur la fenêtre de la semaine précédente (ou moyenne glissante des 5–7 derniers jours ouvrés disponibles jusqu’à d).
   - Dans l’implémentation retenue : moyenne des observations DGS10 de la semaine civile se terminant à `d` (jours ≤ d et ≥ d−6 jours), en ne retenant que les valeurs déjà connues.
4. Construire le dataset aligné hebdomadaire :
   - `date` : date de référence du mortgage
   - `Mortgage_30Y` : valeur MORTGAGE30US
   - `Treasury_10Y` : moyenne hebdomadaire calculée comme ci-dessus
   - `Mortgage_Spread` = Mortgage_30Y − Treasury_10Y
   - `Treasury_10Y_Lag1` : valeur de Treasury_10Y de la semaine précédente (past only)
5. Supprimer les lignes où Treasury_10Y est manquant (début de période si nécessaire).
6. Vérifier l’absence de doublons de dates.
7. Aucune observation n’est supprimée pour cause de valeur extrême sans justification ; les taux élevés des années 1980 sont conservés car historiquement pertinents.

## Découpage train / validation / test

- Tri chronologique strict (déjà effectué).
- 70 % premières observations → entraînement
- 15 % suivantes → validation
- 15 % dernières → test (période 2018–2026 environ)
- Aucun mélange aléatoire.
- La moyenne naïve est calculée **uniquement** sur le jeu d’entraînement.
- La standardisation n’est pas appliquée pour la régression linéaire simple à une variable (échelle déjà comparable en %).

## Variables construites

| Variable            | Définition                                      | Usage                  |
|---------------------|-------------------------------------------------|------------------------|
| Treasury_10Y        | Moyenne hebdomadaire du DGS10 jusqu’à la date  | Explicative principale |
| Mortgage_30Y        | Taux hypothécaire 30 ans                       | Cible                  |
| Mortgage_Spread     | Mortgage_30Y − Treasury_10Y                    | Analyse exploratoire   |
| Treasury_10Y_Lag1   | Treasury_10Y de la période précédente          | Variante optionnelle   |

## Reproductibilité

- Graine fixée à 42 lorsque nécessaire (Ridge, etc.).
- Chemins relatifs.
- Code source dans `notebooks/projet_F14.ipynb`.
