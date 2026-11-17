# Script de soutenance orale (3 minutes) + questions possibles

**Durée cible présentation :** 3 minutes  
**Langue :** français simple et naturel  
**Ton :** clair, confiant, pédagogique  

---

## Script oral (environ 450–500 mots, ~3 min)

**[0:00 – 0:25] Identité et problématique**  
Bonjour. Je suis Jawahir Abdelkebir, étudiant en Finance et Contrôle Interne à l’ENCG Settat.  
Mon sujet est le F14 : Taux hypothécaire et taux à 10 ans.  

La question que je me pose est simple : dans quelle mesure le rendement des obligations américaines à 10 ans permet-il d’expliquer et de prévoir le taux des prêts hypothécaires à 30 ans aux États-Unis ?  
Mon objectif est de construire un modèle de régression linéaire simple et de le comparer à une référence naïve.

**[0:25 – 0:50] Données**  
J’utilise deux séries de la base FRED de la Réserve fédérale de Saint-Louis.  
La première, MORTGAGE30US, donne le taux moyen des crédits immobiliers à 30 ans, en fréquence hebdomadaire depuis 1971.  
La seconde, DGS10, donne le rendement quotidien des obligations du Trésor à 10 ans.  
J’ai aligné les fréquences en calculant, pour chaque semaine du taux hypothécaire, la moyenne du taux à 10 ans uniquement avec les données déjà disponibles à cette date, afin d’éviter toute fuite d’information.

**[0:50 – 1:30] Algorithme et protocole**  
L’algorithme principal est la régression linéaire simple :  
Mortgage_30Y = β₀ + β₁ × Treasury_10Y + ε.  
β₀ est la constante, β₁ le coefficient qui mesure l’impact d’une variation d’un point de pourcentage du taux à 10 ans sur le taux hypothécaire.  
J’ai découpé les données de façon chronologique : 70 % pour l’entraînement, 15 % pour la validation et 15 % pour le test final.  
La référence naïve consiste à prédire systématiquement la moyenne du taux hypothécaire observée sur la période d’entraînement.

**[1:30 – 2:20] Résultats**  
Sur la période de test (2018–2026), le modèle linéaire obtient un RMSE d’environ 0,80 point de pourcentage, contre plus de 4 points pour la référence naïve.  
Le R² est d’environ 0,75.  
Le coefficient β₁ est proche de 1,05 : une hausse de 1 point du taux à 10 ans se traduit, en moyenne, par une hausse d’un peu plus d’un point du taux hypothécaire.  
Cela confirme le lien économique fort entre les deux séries, tout en laissant une place à d’autres facteurs (prime de risque, marges bancaires, conditions de crédit).

**[2:20 – 3:00] Conclusion et limites**  
En conclusion, le rendement à 10 ans explique une part importante de l’évolution du taux hypothécaire, et le modèle linéaire simple surpasse nettement la référence naïve.  
Cependant, le modèle reste simplifié : il n’intègre pas la politique monétaire, les anticipations d’inflation ni les conditions de liquidité.  
Une piste d’amélioration serait d’ajouter des variables retardées ou des facteurs macroéconomiques.  
Je vous remercie. Je suis prêt à répondre à vos questions.

---

## 15 questions possibles du professeur et réponses préparées

1. **Pourquoi choisir la régression linéaire simple ?**  
   C’est l’algorithme principal demandé pour le sujet F14. Il est interprétable, adapté à une relation linéaire attendue entre deux taux d’intérêt, et constitue un excellent point de départ avant d’envisager des modèles plus complexes.

2. **Pourquoi utiliser le taux à 10 ans plutôt qu’un autre taux ?**  
   Le taux à 10 ans est le benchmark de long terme le plus suivi. Les crédits hypothécaires à 30 ans sont influencés par le coût de refinancement à long terme des banques, lui-même lié aux rendements des obligations d’État de maturité intermédiaire-longue.

3. **Pourquoi le taux hypothécaire est-il systématiquement plus élevé que le taux obligataire ?**  
   Parce qu’il incorpore une prime de risque de crédit (risque de défaut de l’emprunteur), des coûts de gestion, des marges bancaires et une compensation pour le risque de liquidité et de remboursement anticipé.

4. **Pourquoi un découpage chronologique et non aléatoire ?**  
   Les séries financières sont ordonnées dans le temps. Un découpage aléatoire créerait une fuite d’information (utiliser le futur pour prédire le passé) et ne respecterait pas la réalité opérationnelle d’une prévision.

5. **Pourquoi la métrique principale est-elle le RMSE ?**  
   Le RMSE pénalise davantage les grandes erreurs et s’exprime dans la même unité que la variable cible (points de pourcentage). Il a été fixé avant de voir les résultats.

6. **Qu’est-ce qu’une fuite de données (data leakage) ?**  
   C’est l’utilisation, lors de l’entraînement ou du prétraitement, d’informations qui ne seraient pas disponibles au moment de la prédiction réelle (par exemple des valeurs futures ou des statistiques calculées sur tout le jeu de données).

7. **Que signifie un R² de 0,75 ?**  
   Environ 75 % de la variance du taux hypothécaire sur la période de test est expliquée par le taux à 10 ans dans le modèle linéaire. Le reste correspond à d’autres facteurs non capturés.

8. **Comment interpréter β₁ ≈ 1,05 ?**  
   Toutes choses égales par ailleurs, une hausse de 1 point de pourcentage du rendement à 10 ans est associée en moyenne à une hausse d’environ 1,05 point du taux hypothécaire à 30 ans.

9. **Qu’est-ce qu’une référence naïve et pourquoi l’utiliser ?**  
   C’est un modèle extrêmement simple (ici la moyenne historique d’entraînement) qui sert de point de comparaison. Un modèle plus sophistiqué n’a d’intérêt que s’il bat cette référence.

10. **Quelles sont les principales limites du modèle ?**  
    Relation supposée linéaire et constante dans le temps, une seule variable explicative, absence de prise en compte des régimes de politique monétaire, des crises et des changements structurels (ex. 2022–2023).

11. **Comment avez-vous synchronisé les fréquences ?**  
    Pour chaque date hebdomadaire du mortgage, j’ai calculé la moyenne du DGS10 en n’utilisant que les observations quotidiennes déjà disponibles à cette date (pas de regard vers le futur).

12. **Y a-t-il des signes de surapprentissage ?**  
    Le R² d’entraînement est très élevé (~0,96) tandis que le R² de test est plus modéré (~0,75). Il y a une dégradation, mais le modèle reste largement supérieur à la naïve. La relation a un peu changé après 2010–2018.

13. **Pourquoi ne pas standardiser les variables ?**  
    Pour une régression linéaire simple avec une seule variable explicative déjà exprimée en pourcentage, la standardisation n’est pas nécessaire et complique l’interprétation directe des coefficients.

14. **Que représente le spread hypothécaire ?**  
    La différence Mortgage_30Y − Treasury_10Y. Il mesure la prime que les emprunteurs paient au-dessus du taux sans risque de long terme. Il a fortement varié selon les conditions de crédit et les périodes de stress.

15. **Quelle amélioration prioritaire proposeriez-vous ?**  
    Ajouter 1 ou 2 variables retardées du taux à 10 ans et éventuellement un indicateur de volatilité ou de conditions de crédit, tout en conservant un découpage chronologique strict et une évaluation honnête sur le test.
