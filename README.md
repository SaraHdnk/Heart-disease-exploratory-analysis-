# Heart Disease UCI – Analyse exploratoire

Analyse exploratoire (EDA) du jeu de données **Heart Disease UCI**. L'objectif est de comprendre les données et de préparer une première stratégie de modélisation pour prédire la présence d'une maladie coronarienne.

## Objectifs

- Comprendre du mieux possible les données (forme, qualité, distributions).
- Identifier les variables les plus liées au diagnostic.
- Développer une première stratégie de modélisation.

## Données

- **Source** : [Heart Disease UCI sur Kaggle](https://www.kaggle.com/code/zeeshanyounas001/heart-disease-uci?select=heart_disease_uci.csv) (fichier `heart_disease_uci.csv`), issu du UCI Machine Learning Repository.
- **Taille** : 920 observations, 16 variables.
- **Provenance** : 4 centres (colonne `dataset`) : Cleveland, Hungary, Switzerland et VA Long Beach.

### Variable cible : `num`

Diagnostic de maladie coronarienne par angiographie :

| Valeur | Signification | Part des observations |
|---|---|---|
| 0 | Pas de maladie (rétrécissement des artères < 50 %) | 44,7 % |
| 1 | Maladie présente, sévérité croissante de 1 à 4 | 28,8 % |
| 2 | | 11,8 % |
| 3 | | 11,6 % |
| 4 | | 3,0 % |

Les classes 1 à 4 étant peu représentées, elles sont regroupées dans une cible binaire `num_bin` (0 = pas malade, 1 = malade) : **509 malades** et **411 non malades**.

### Variables explicatives

| Variable | Description | Type |
|---|---|---|
| `id` | Identifiant du patient | numérique |
| `age` | Âge (années) | numérique |
| `sex` | Sexe | catégorielle |
| `dataset` | Centre d'origine | catégorielle |
| `cp` | Type de douleur thoracique | catégorielle |
| `trestbps` | Pression artérielle systolique au repos (mm Hg) | numérique |
| `chol` | Cholestérol sérique total (mg/dL) | numérique |
| `fbs` | Glycémie à jeun > 120 mg/dL | catégorielle |
| `restecg` | Électrocardiogramme au repos | catégorielle |
| `thalch` | Fréquence cardiaque maximale atteinte (bpm) | numérique |
| `exang` | Angine déclenchée par l'effort | catégorielle |
| `oldpeak` | Sous-décalage du segment ST à l'effort par rapport au repos (mm) | numérique |
| `slope` | Pente du segment ST au pic de l'effort | catégorielle |
| `ca` | Nombre de gros vaisseaux coronaires (0 à 3) | numérique |
| `thal` | Résultat de la scintigraphie myocardique au thallium | catégorielle |

## Démarche

1. **Analyse de forme** : dimensions, types de variables, valeurs manquantes.
2. **Prétraitement** :
   - typage des variables catégorielles ;
   - encodage one-hot;

3. **Analyse de fond** :
   - distribution de la cible ;
   - variables numériques : boxplots et histogrammes avec densité ;
   - variables catégorielles : diagrammes circulaires.
4. **Relation variables / cible** :
   - histogrammes superposés malades / non malades pour les variables numériques ;
   - heatmaps pour les variables catégorielles.

## Principaux résultats

### Valeurs manquantes

| Variable | % manquant |
|---|---|
| `ca` | 66,4 % |
| `thal` | 52,8 % |
| `slope` | 33,6 % |
| `chol`  | 22,0 % |
| `fbs` | 9,8 % |
| `oldpeak` | 6,7 % |
| `trestbps` | 6,5 % |
| `thalch` | 6,0 % |
| `exang` | 6,0 % |
| `restecg` | 0,2 % |

### Variables les plus discriminantes

- **`thalch`** : la fréquence cardiaque maximale est plus basse chez les malades (pic vers 120 bpm, contre 160 bpm chez les non malades).
- **`oldpeak`** : un sous-décalage du segment ST entre 1 et 2,5 mm est beaucoup plus fréquent chez les malades.
- **`cp`** : 78 % des patients asymptomatiques sont malades, alors que 86 % des patients avec une angine atypique ne le sont pas.
- **`exang`** : 83 % des patients ayant une angine à l'effort sont malades.
- **`thal`** : 80 % des patients avec *reversable defect* et 76 % avec *fixed defect* sont malades, contre 30 % avec un résultat normal.
- **`slope`** : 77 % des patients avec une pente *flat* ou *downsloping* sont malades.
- **`sex`** : 63 % des hommes sont malades, contre 26 % des femmes. Les hommes représentent plus de 75 % de l'échantillon.
- **`dataset`** : les cohortes Switzerland (6,5 % de sains) et VA Long Beach (25,5 %) comptent très peu de patients sains.
- **`fbs`** : 68 % des patients avec une glycémie à jeun > 120 mg/dL sont malades.

## Pistes pour la modélisation 

- Traiter les valeurs manquantes : imputation, ou suppression de `ca` et `thal`, qui ont trop de valeurs manquantes.
- Prendre en compte l'effet centre (`dataset`), fortement lié à la cible.
- Commencer par une classification binaire (`num_bin`) avant d'aborder la prédiction de la sévérité (`num`).
- Gérer les outliers de `trestbps`, `chol` et `oldpeak`.

## Structure du projet

```
.
├── heart_disease.ipynb   # Notebook d'analyse exploratoire
└── README.md
```


