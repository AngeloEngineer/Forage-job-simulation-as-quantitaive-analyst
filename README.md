# Forage-job-simulation-as-quantitaive-analyst
# Analyse des prix du gaz naturel – Tâche 1 (Quantitative Research, JP Morgan)

Ce dépôt contient une solution pour la tâche 1 du parcours *Quantitative Research* chez JP Morgan. L’objectif est d’analyser un jeu de données mensuelles des prix du gaz naturel, d’estimer le prix à n’importe quelle date passée et de l’extrapoler sur une année dans le futur.

## Contexte

Les données représentent un instantané mensuel des prix d’achat du gaz naturel à la fin de chaque mois, du **31 octobre 2020 au 30 septembre 2024**. Elles sont fournies dans un fichier CSV (non inclus dans ce dépôt pour des raisons de confidentialité, mais le format est supposé être similaire à celui décrit dans l’énoncé).

L’exercice consiste à :
- Visualiser les données pour identifier des tendances, saisonnalités ou autres facteurs influençant les prix.
- Implémenter une fonction Python qui, pour une date donnée en entrée, renvoie une estimation du prix du gaz naturel.
- Pour les dates passées, l’estimation doit être basée sur une interpolation des points mensuels disponibles.
- Pour les dates futures (jusqu’à un an après la dernière observation), l’estimation doit être une extrapolation qui tient compte des éventuels motifs saisonniers observés.

Les jours fériés, week‑ends et jours de marché fermé ne sont pas pris en compte : on travaille à une échelle mensuelle.

## Méthodologie

La solution proposée s’articule autour des étapes suivantes :

1. **Chargement et exploration des données**  
   Lecture du fichier CSV, conversion des colonnes de dates, vérification des valeurs manquantes et statistiques descriptives.

2. **Visualisation**  
   Tracé de la série temporelle complète (prix en fonction du temps). Recherche visuelle de motifs récurrents (saisonnalité annuelle, tendance, etc.).

3. **Interpolation pour les dates passées**  
   Pour une date située entre deux points mensuels, on utilise une interpolation linéaire (ou spline) afin d’obtenir une estimation continue. Cette approche respecte la dynamique locale des prix.

4. **Extrapolation pour l’année à venir**  
   Plusieurs méthodes peuvent être envisagées :  
   - Décomposition saisonnière (par exemple avec `statsmodels`) et prolongement de la tendance et de la composante saisonnière.  
   - Modèle ARIMA saisonnier (SARIMA) ajusté sur les données historiques.  
   - Lissage exponentiel avec prise en compte de la saisonnalité (Holt‑Winters).  
   Dans ce notebook, nous avons retenu une approche simple mais robuste :  
     * Estimation de la tendance linéaire sur les dernières années.  
     * Calcul des coefficients saisonniers moyens par mois.  
     * Combinaison des deux pour projeter les 12 mois suivants.

5. **Fonction finale**  
   La fonction `estimer_prix(date)` renvoie le prix estimé pour la date fournie (au format `datetime` ou chaîne de caractères). Elle utilise l’interpolation si la date est dans l’intervalle historique, et l’extrapolation si elle est postérieure au 30 septembre 2024 (jusqu’à un an après).

## Contenu du dépôt

- `README.md` : ce fichier.
- `Task1_solution_interpolation.ipynb` : notebook Jupyter contenant le code, les visualisations et les explications détaillées (en français).
- (Optionnel) `data/` : dossier pouvant contenir le fichier CSV des données (non inclus par défaut).
- (Optionnel) `requirements.txt` : liste des dépendances Python nécessaires.

## Prérequis et installation

Le code est écrit en Python 3. Les principales bibliothèques utilisées sont :

- `pandas` – manipulation des données
- `numpy` – calculs numériques
- `matplotlib` / `seaborn` – visualisations
- `statsmodels` – décomposition saisonnière et modèles statistiques
- `scipy` – interpolation

Pour installer les dépendances, exécutez :

```bash
pip install pandas numpy matplotlib seaborn statsmodels scipy
```

Ou, si vous utilisez `conda` :

```bash
conda install pandas numpy matplotlib seaborn statsmodels scipy
```

## Utilisation

1. Placez le fichier CSV contenant les données mensuelles dans le même répertoire que le notebook (ou ajustez le chemin dans la cellule de chargement).
2. Ouvrez le notebook `Task1_solution_interpolation.ipynb` avec Jupyter Notebook, JupyterLab, ou Visual Studio Code.
3. Exécutez toutes les cellules pour :
   - Charger et visualiser les données.
   - Voir les estimations d’exemple.
   - Tester la fonction `estimer_prix()` sur différentes dates.
4. Vous pouvez également importer les fonctions définies dans le notebook vers un autre script Python.

## Résultats attendus

Le notebook produit :
- Un graphique de la série historique avec les points mensuels.
- Un graphique superposant les valeurs observées et les valeurs interpolées/extrapolées.
- Des exemples d’appels de la fonction d’estimation, par exemple :
  ```python
  estimer_prix("2023-06-15")   # date passée (interpolation)
  estimer_prix("2025-03-01")   # date future (extrapolation)
  ```
- Une discussion sur les tendances observées (saisonnalité, chocs éventuels) et la performance de l’extrapolation.

## Remarques

- L’extrapolation sur un an est indicative et ne tient pas compte d’événements géopolitiques ou économiques imprévus. Elle reflète uniquement les motifs historiques.
- Les jours fériés et week‑ends sont ignorés car l’échelle de travail est mensuelle.
- Pour une utilisation en production, il conviendrait de valider la robustesse du modèle d’extrapolation (backtesting, intervalles de confiance).

## Auteur

Ce travail a été réalisé dans le cadre d’un exercice de simulation du rôle d’analyste quantitatif chez JP Morgan.
