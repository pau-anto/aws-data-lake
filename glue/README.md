# Configurer le Glue Crawler

## ÉTAPE 1 — Créer le Data Catalog (Glue)

Une fois les fichiers uploadés, configurer le Glue Crawler sur le bucket Raw :

1. Aller dans **AWS Glue** → **go to the Data Catalog** → **Tables** → **Add table**
2. appuyer sur **Create database**  → **Nom** : `datalake_raw_db` → **Create**
3. choisie `datalake_raw_db`
4. cliquer sur **Add tables using crawler** 
5. **Nom** : `crawler-raw-datalake`
6. **Data source** : **add data source** Source → S3
7. **Path S3** → `s3://datalake-raw-dev-groupe1/`
8. **IAM Role** : sélectionner `LabRole`
9. **Target Database** : `datalake_raw_db`
10. **Schedule** → On demand
11. **Create**
12. Lancer le crawler **Run crawler** → attendre le statut **`Ready`**
13. Vérifier les tables créées dans **Glue Data Catalog**

---

### Étape 2 — Vérifier le schéma (Data Dictionary)

Après l'exécution du crawler, aller dans **Glue → Data Catalog → Tables** et vérifier :

| Table | Colonnes attendues | Type détecté |
|---|---|---|
| `survey` | age, gender, country, treatment... | string, int |
| `mental_health_lifestyle_dataset` | year, stress_level, sleep_hours... | string, int, double |
| `mental_health_diagnosis_treatment_` | diagnosis, treatment, age... | string, int |

---
### Étape 3 — test

1. Aller dans **Athena** 
2. Choisie **Query your data in Athena console**  → **Lunch query editor** → **Edit settings** → **Manage query settings**
3. **Path** → `s3://datalake-athena-results-dev-groupe1/`
4. Dans **Editor** tester la requête suivante:
```sql
    SELECT *
    FROM survey
    LIMIT 10;

---
