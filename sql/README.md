# SQL — Requêtes Athena & KPIs

Ce dossier contient les requêtes SQL exécutées sur Amazon Athena.

---

## Objectif

Interroger les données du bucket S3 Curated via **Amazon Athena**
pour calculer des KPIs sur la santé mentale et alimenter les
dashboards QuickSight.

---

## Étapes à suivre

### Pré-requis
- La sandbox AWS Academy doit être active
- Le Glue Crawler Curated doit avoir été exécuté
- Les tables doivent être visibles dans le Glue Data Catalog

---

### Étape 1 — Configurer Athena

### Étape 2 — Sélectionner la base de données

### Étape 3 — Tester avec une requête simple

Avant de lancer les KPIs, vérifier que les données sont bien accessibles :

Prendre un **screenshot** du résultat → `screenshots/04-analytics/`

### Étape 4 — Lancer les requêtes KPIs

Les requêtes sont disponibles dans le fichier `kpis.sql`.
Les copier-coller dans l'éditeur Athena et les exécuter une par une.

---

### Étape 5 — Connecter QuickSight à Athena

---

## Screenshots à prendre

| Fichier | Contenu |
|---|---|
| `001-athena-settings.png` | Configuration du bucket de résultats |
| `002-athena-tables.png` | Tables visibles dans le Data Catalog |
| `003-athena-query-result.png` | Résultat d'une requête KPI |
| `004-quicksight-dataset.png` | Dataset connecté à Athena |
| `005-quicksight-dashboard.png` | Dashboard final avec visualisations |

---

*Projet académique — ESGI — Introduction au Cloud AWS — Mars 2026*
