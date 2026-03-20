# AWS Data Lake — Santé Mentale

Projet académique — ESGI — Introduction au Cloud AWS

---

## Description du projet

Mise en place d'un **data lake simple sur AWS** autour de données publiques sur la santé mentale (Kaggle / OSMI).

Le pipeline ingère des données brutes CSV, les transforme via AWS Glue, les stocke dans une architecture en 3 couches (Raw → Processed → Curated), puis les expose à Amazon Athena pour des analyses SQL et Amazon QuickSight pour la visualisation.

---

## Architecture

```
Source (CSV — Kaggle)
        │
        ▼ Upload manuel
┌────────────────────────────────────────────────────────────┐
│                        DATA LAKE                           │
│                                                            │
│  S3 Raw ──► Glue Crawler (Raw) ──► Glue Data Catalog  ◄─┐  │
│     │                                                   │  │
│     ▼                                                   │  │
│  Glue ETL Job (nettoyage + Parquet)                     │  │
│     │                                                   │  │
│     ▼                                                   │  │
│  S3 Processed ──► Glue Job (Partition)                  │  │
│                        │                                │  │
│                        ▼                                │  │
│                   S3 Curated ──► Glue Crawler (Curated) ┘  │
│                                                            │
└────────────────────────────────────────────────────────────┘
        │
        ▼
  Amazon Athena (requêtes SQL + KPIs)
        │
        ▼
  Amazon QuickSight (dashboards)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Sécurité : AWS IAM + AWS CloudTrail
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Structure du repository

```
aws-data-lake/
│
├── architecture/            # Infrastructure CloudFormation + IAM
│   ├── README.md            # Documentation de l'infrastructure
│   ├── infrastructure.yaml  # Buckets S3 (déployable en sandbox)
│   └── iam-roles.yaml       # Rôles IAM (référence production)
│
├── datasets/                # Données sources brutes
│   ├── survey/
│   │   └── survey.csv
│   ├── mental-health-lifestyle/
│   │   └── Mental_Health_Lifestyle_Dataset.csv
│   └── mental-health-diagnosis/
│       └── mental_health_diagnosis_treatment_.csv
│
├── glue/                    # Scripts ETL AWS Glue
│   ├── README.md
│
├── sql/                     # Requêtes Athena
│   └── kpis.sql             # Requêtes KPIs et analyses
|   └── README.md
│
├── screenshots/             # Captures console AWS (pour le rapport)
│
├── report/                  # Rapport écrit du projet
|   └── AWS_datalake_Report.pdf
│
└── README.md                # Ce fichier
```

---

## Arborescence S3

### Bucket Raw — `datalake-raw-dev-groupe1`
```
datalake-raw-dev-groupe1/
├── survey/
│   └── survey.csv
├── mental-health-lifestyle/
│   └── Mental_Health_Lifestyle_Dataset.csv
└── mental-health-diagnosis/
    └── mental_health_diagnosis_treatment_.csv
```

### Bucket Processed — `datalake-processed-dev-groupe1`
```
datalake-processed-dev-groupe1/
├── survey/
│   └── survey.parquet
├── mental-health-lifestyle/
│   └── mental_health_lifestyle.parquet
└── mental-health-diagnosis/
    └── mental_health_diagnosis.parquet
```

### Bucket Curated — `datalake-curated-dev-groupe1`
```
datalake-curated-dev-groupe1/
├── survey/
│   └── year=2014/
│       └── data.parquet
├── mental-health-lifestyle/
│   └── year=2019/
│       └── data.parquet
│   └── year=2020/
│       └── data.parquet
│   └── year=2021/
│       └── data.parquet
│   └── year=2022/
│       └── data.parquet
│   └── year=2023/
│       └── data.parquet
│   └── year=2024/
│       └── data.parquet
└── mental-health-diagnosis/
    └── year=2024/
        └── data.parquet
```

### Bucket Athena Results — `datalake-athena-results-dev-groupe1`
```
datalake-athena-results-dev-groupe1/
└── queries/
    └── 2026/
        └── 03/
            └── [résultats générés automatiquement par Athena]
```

---

## Datasets

| Fichier | Source | Description | Lignes estimées |
|---|---|---|---|
| `survey.csv` | Kaggle / OSMI | Enquête santé mentale en entreprise tech — traitement, pays, âge, travail remote | ~1 200 |
| `Mental_Health_Lifestyle_Dataset.csv` | Kaggle | Habitudes de vie et santé mentale 2019-2024 — stress, sommeil, activité physique | ~10 000+ |
| `mental_health_diagnosis_treatment_.csv` | Kaggle | Suivi de diagnostics et traitements santé mentale | ~10 000+ |

---

## Répartition des tâches

| Rôle | Périmètre |
|---|---|
| **P1 — Architecte / Infrastructure** | CloudFormation, S3, IAM, arborescence, documentation |
| **P2 — Ingestion / Catalogue** | Upload datasets, Glue Crawler, schéma, data dictionary |
| **P3 — ETL / Transformation** | Glue Job, nettoyage, Parquet, partitionnement |
| **P4 — Analytics / Reporting** | Requêtes Athena, KPIs, QuickSight, rapport + slides |

---

## Services AWS utilisés

| Service | Rôle |
|---|---|
| **Amazon S3** | Stockage des 3 couches du data lake |
| **AWS Glue Crawler** | Détection automatique du schéma des données |
| **AWS Glue Data Catalog** | Catalogue centralisé des métadonnées |
| **AWS Glue ETL Job** | Nettoyage, transformation et conversion Parquet |
| **Amazon Athena** | Requêtes SQL sans serveur sur S3 |
| **AWS CloudFormation** | Infrastructure as Code |

---

## Déploiement rapide

```bash
# 1. Cloner le repo
git clone https://github.com/pau-anto/aws-data-lake.git
cd aws-data-lake

# 2. Déployer l'infrastructure S3 via CloudFormation
# → Voir architecture/README.md pour les étapes détaillées

# 3. Uploader les datasets dans S3 Raw
# → Voir datasets/ pour les fichiers sources

# 4. Lancer le Glue Crawler sur Raw
# → Configurer avec le rôle LabRole (sandbox AWS Academy)

# 5. Lancer le Glue ETL Job
# → Script disponible dans glue/etl_job.py

# 6. Lancer les requêtes Athena
# → Requêtes disponibles dans sql/kpis.sql
```

---

### Notes sandbox AWS Academy

- Les sandboxes se **réinitialisent** à chaque session — relancer CloudFormation si besoin
- Les rôles IAM ne peuvent pas être créés via CloudFormation → utiliser le rôle **`LabRole`** fourni
- Rester sur la région **`us-east-1`** pour tous les services

---

*Projet académique — ESGI — Introduction au Cloud AWS — Mars 2026*
