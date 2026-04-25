# 🛡️ Prévision des Coûts Ultimes de Sinistres – Assurance

Projet réalisé dans le cadre du cours **Apprentissage Statistique** à l'**ISFA** 
(Institut de Science Financière et d'Assurances), sur un panel de données 
d'assurance couvrant la période 1988–2006.

## 🎯 Objectif
Prédire les valeurs manquantes du *Coût Ultime* des sinistres à partir de 
variables structurelles et textuelles, en privilégiant la robustesse aux 
valeurs extrêmes — problématique centrale en actuariat non-vie.

## ⚙️ Méthodologie
- **EDA approfondie** et ingénierie de variables (ratios financiers, 
  variables temporelles)
- **Benchmark de modèles** : OLS, Random Forest, XGBoost, CatBoost
- **Optimisation des hyperparamètres** via **Optuna** avec **Huber Loss** 
  pour la robustesse aux outliers
- **NLP** : extraction de *Topic Scores* à partir des descriptions textuelles 
  des sinistres

## 🏆 Résultats
| Modèle | R² | RMSE |
|---|---|---|
| OLS | baseline | — |
| XGBoost | — | — |
| **CatBoost** | **84%** | **0.61** |

Modèle retenu : **CatBoost** – meilleur compromis précision/robustesse.

## 🔑 Variables clés
1. Log(*Coût Initial*)
2. Topic Scores (NLP)
3. Salaire horaire effectif
4. Ratios délai de déclaration / coût-salaire

## 🛠️ Stack technique
`Python` · `CatBoost` · `XGBoost` · `Optuna` · `scikit-learn` · 
`pandas` · `NLP`
