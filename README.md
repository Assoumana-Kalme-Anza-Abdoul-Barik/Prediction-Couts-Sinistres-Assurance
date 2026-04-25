# 🛡️ Prévision des Coûts Ultimes de Sinistres – Machine Learning Actuariel

> **Cours :** Apprentissage Statistique – Master 1 Économétrie & Statistiques  
> **Institution :** ISFA – Institut de Science Financière et d'Assurances, Lyon 1  
> **Auteurs :** Abdoul Barik ASSOUMANA KALME ANZA · Mathias ADJEDJOGO · Manuel CEREZO DE LA ROCA  
> **Année :** 2024–2025

---

## 🎯 Problématique

Dans l'industrie de l'assurance, le **Coût Ultime** d'un sinistre — montant total des indemnisations à terme — est rarement connu au moment de la déclaration. Ce projet vise à **prédire et imputer les valeurs manquantes** du Coût Ultime à partir d'un panel de données réelles couvrant la période **1988–2006**, en s'appuyant sur des techniques de Machine Learning avancées.

La difficulté centrale : la distribution du Coût Ultime présente des **valeurs extrêmes significatives** (outliers), ce qui rend les modèles linéaires classiques insuffisants et nécessite des approches robustes.

---

## 📁 Structure du projet

```
├── notebook/
│   └── ISFA_ES_Project_ML_Prevision_Couts_Sinistres.ipynb
├── README.md
```

---

## ⚙️ Méthodologie

### 1. Exploration et pré-traitement des données (EDA)
- Analyse univariée et bivariée des variables
- Détection et traitement des outliers (`pyod`)
- Visualisation des données manquantes (`missingno`)
- Traitement de la variable `patrimoine` (80% manquant → exclue après analyse d'importance : contribution < 3.5%)

### 2. Feature Engineering
- **Transformation logarithmique** du Coût Initial et Coût Ultime
- **Création de ratios** : `time_to_communication`, `claim_salary_ratio`
- **Variables temporelles** : `time_regime` pour capturer la tendance temporelle identifiée dans l'EDA
- **Extraction NLP** : Topic Scores (4 topics) extraits des descriptions textuelles des sinistres via **Biterm Topic Model** (`bitermplus`, `gensim`)
- Calcul du **salaire horaire effectif** (`effective_hourly_wage`)

### 3. Modélisation et benchmark

Quatre modèles comparés sur les mêmes features :

| Modèle | R² | RMSE | MAE | Temps d'inférence |
|---|---|---|---|---|
| Régression Linéaire (OLS) | 0.79 | 0.71 | 0.47 | 0.10s |
| XGBoost | 0.83 | 0.64 | 0.41 | **0.005s** |
| **CatBoost** | **0.84** | **0.62** | **0.40** | 0.12s |
| CatBoost + Optuna (Huber Loss) | 0.83 | 0.63 | — | 0.04s |

### 4. Optimisation des hyperparamètres
- Framework : **Optuna** (optimisation bayésienne)
- Fonction de perte : **Huber Loss** pour la robustesse aux outliers
- Meilleurs hyperparamètres retenus :
  - `iterations` : 863
  - `depth` : 8
  - `learning_rate` : 0.034
  - `l2_leaf_reg` : 0.0013

### 5. Validation out-of-sample
- Application du modèle final sur données hors-échantillon
- **RMSE final : 0.63 | R² : 0.83**
- La distribution imputée du Coût Ultime reflète fidèlement les propriétés statistiques du Coût Initial (ajusté à la taille), validant la cohérence actuarielle du modèle

---

## 🔑 Variables les plus prédictives

1. **Log(Coût Initial)** — corrélation directe avec le Coût Ultime
2. **Topic Scores NLP** (topics 0, 1, 2, 3) — nature du sinistre extraite du texte
3. **Salaire horaire effectif** — proxy du profil socio-économique de l'assuré
4. **Time-to-Communication** — délai entre sinistre et déclaration
5. **Time Regime** — tendance temporelle 1988–2006

---

## 💡 Choix du modèle final : CatBoost

**CatBoost** a été retenu face à XGBoost pour les raisons suivantes :

- **Meilleur R²** (0.84 vs 0.83) et **meilleur RMSE** (0.62 vs 0.64)
- **Gestion native des variables catégorielles** sans encodage manuel
- **Robustesse supérieure** aux distributions asymétriques caractéristiques des coûts de sinistres
- En actuariat, la **précision prime sur le temps d'inférence** — le léger surcoût de CatBoost (0.12s vs 0.005s) est négligeable en production

---

## 🛠️ Stack technique

| Catégorie | Outils |
|---|---|
| **ML & Modélisation** | `CatBoost`, `XGBoost`, `scikit-learn` |
| **Optimisation** | `Optuna` (Huber Loss) |
| **NLP** | `bitermplus`, `gensim` |
| **Data** | `pandas`, `numpy` |
| **Visualisation** | `matplotlib`, `seaborn`, `missingno` |
| **Outlier Detection** | `pyod` |

---

## 📌 Contexte académique

Ce projet s'inscrit dans le cours **Apprentissage Statistique** du Master 1 Économétrie & Statistiques de l'ISFA, et illustre l'application concrète du Machine Learning à une problématique actuarielle réelle : l'estimation du coût ultime des sinistres corporels, enjeu central en **provisionnement et tarification non-vie**.
