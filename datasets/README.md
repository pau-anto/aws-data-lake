# Datasets — Ingestion & Arborescence S3

Ce dossier contient les données sources brutes du projet.

---

## Datasets utilisés

| Fichier | Source | Description | Lignes |
|---|---|---|---|
| `survey.csv` | [Kaggle / OSMI](https://www.kaggle.com/datasets/osmi/mental-health-in-tech-survey) | Enquête santé mentale en entreprise tech | ~1 200 |
| `Mental_Health_Lifestyle_Dataset.csv` | [Kaggle](https://www.kaggle.com/datasets/atharvasoundankar/mental-health-and-lifestyle-habits-2019-2024) | Habitudes de vie et santé mentale 2019-2024 | ~10 000+ |
| `mental_health_diagnosis_treatment_.csv` | [Kaggle](https://www.kaggle.com/datasets/uom190346a/mental-health-diagnosis-and-treatment-monitoring) | Suivi de diagnostics et traitements | ~10 000+ |

---

## Arborescence S3 Raw à créer

Avant d'uploader les fichiers, créer la structure de dossiers suivante dans le bucket **`datalake-raw-dev-groupe1`** :

```
datalake-raw-dev-groupe1/
├── survey/
│   └── csv/
│       └── survey.csv
├── mental-health-lifestyle/
│   └── csv/
│       └── Mental_Health_Lifestyle_Dataset.csv
└── mental-health-diagnosis/
    └── csv/
        └── mental_health_diagnosis_treatment_.csv
```

---

## Étapes d'ingestion

### Pré-requis
- Avoir une sandbox AWS Academy active
- Avoir les 3 fichiers CSV téléchargés en local (voir liens ci-dessus)
- Le bucket `datalake-raw-dev-groupe1` doit être déjà créé 

---

### Étape 1 — Ouvrir le bucket Raw

1. Se connecter à la console AWS
2. Rechercher **S3** dans la barre de recherche
3. Cliquer sur le bucket **`datalake-raw-dev-groupe1`**

---

### Étape 2 — Créer l'arborescence et uploader les fichiers

> ⚠️ Dans S3, les dossiers n'existent que s'ils contiennent un fichier.
> Il faut donc créer les dossiers et uploader les fichiers en même temps.

#### Dataset 1 — survey

1. Dans le bucket, cliquer **"Create folder"**
2. Nom : `survey` → **"Create folder"**
3. Entrer dans `survey/`
4. Cliquer **"Create folder"** → Nom : `csv` → **"Create folder"**
5. Entrer dans `csv/`
6. Cliquer **"Upload"** → **"Add files"**
7. Sélectionner `survey.csv`
8. Cliquer **"Upload"**

#### Dataset 2 — mental-health-lifestyle

1. Retourner à la racine du bucket
2. Cliquer **"Create folder"** → Nom : `mental-health-lifestyle` → **"Create folder"**
3. Entrer dans `mental-health-lifestyle/`
4. Cliquer **"Create folder"** → Nom : `csv` → **"Create folder"**
5. Entrer dans `csv/`
6. Cliquer **"Upload"** → **"Add files"**
7. Sélectionner `Mental_Health_Lifestyle_Dataset.csv`
8. Cliquer **"Upload"**

#### Dataset 3 — mental-health-diagnosis

1. Retourner à la racine du bucket
2. Cliquer **"Create folder"** → Nom : `mental-health-diagnosis` → **"Create folder"**
3. Entrer dans `mental-health-diagnosis/`
4. Cliquer **"Create folder"** → Nom : `csv` → **"Create folder"**
5. Entrer dans `csv/`
6. Cliquer **"Upload"** → **"Add files"**
7. Sélectionner `mental_health_diagnosis_treatment_.csv`
8. Cliquer **"Upload"**

---

### Étape 3 — Vérifier l'upload

Une fois les 3 datasets uploadés, vérifier que la structure est correcte :

1. Retourner à la racine du bucket `datalake-raw-dev-groupe1`
2. Tu dois voir **3 dossiers** : `survey/`, `mental-health-lifestyle/`, `mental-health-diagnosis/`
3. Dans chaque dossier `csv/` tu dois voir le fichier CSV correspondant
4. Prendre un **screenshot** de la structure → le déposer dans `screenshots/` sur GitHub

---




*Projet académique — ESGI — Introduction au Cloud AWS — Mars 2026*
