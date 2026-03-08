# Architecture — Data Lake AWS

Ce dossier contient l'ensemble des fichiers d'infrastructure du projet Data Lake AWS, déployés via **AWS CloudFormation**.

---

## Contenu du dossier
-  `infrastructure.yaml` -> Déploiement des buckets S3 (Raw, Processed, Curated, Athena Results) 
-  `iam-roles.yaml` -> Définition des rôles IAM 

---

## Infrastructure déployée — `infrastructure.yaml`

Ce fichier crée automatiquement les 4 buckets S3 qui constituent les couches du data lake :

| Bucket | Rôle | Versioning |
|---|---|---|
| `datalake-raw-dev-groupe1` | Données brutes ingérées, jamais modifiées | Activé |
| `datalake-processed-dev-groupe1` | Données nettoyées, format Parquet | Activé |
| `datalake-curated-dev-groupe1` | Données partitionnées, prêtes pour Athena | Activé |
| `datalake-athena-results-dev-groupe1` | Résultats des requêtes SQL Athena | Désactivé |

## Déploiement

#### Étapes de déploiement

**1. Ouvrir CloudFormation**
- Se connecter à la console AWS
- Rechercher et sélectionner **CloudFormation** dans la barre de recherche en haut
  
**2. Créer la stack**
- Cliquer **"Create stack"** → **"With new resources (standard)"**
- Dans **"Specify template"** sélectionner **"Upload a template file"**
- Cliquer **"Choose file"** et sélectionner `infrastructure.yaml`
- Cliquer **Next**

**3. Configurer la stack**
- **Stack name** : `datalake-infrastructure`
- Vérifier les paramètres pré-remplis :
  - `ProjectName` : `datalake`
  - `Environment` : `dev`
  - `GroupName` : `groupe1`
- Cliquer **Next**

**4. Options avancées**
- Laisser toutes les options par défaut
- Cliquer **Next**

**5. Validation et lancement**
- Vérifier le récapitulatif des ressources à créer
- ⚠️ Cocher obligatoirement la case :
  *"I acknowledge that AWS CloudFormation might create IAM resources"*
- Cliquer **"Submit"**

**6. Suivi du déploiement**
- L'onglet **"Events"** affiche la création en temps réel
- Attendre le statut **`CREATE_COMPLETE`** (2-3 minutes)
- En cas d'erreur, le statut sera **`ROLLBACK_COMPLETE`** — consulter l'événement `CREATE_FAILED` pour le détail

#### Vérification post-déploiement
- Aller dans **S3** → vérifier que les 4 buckets sont présents
- Aller dans **CloudFormation → Stacks → datalake-infrastructure → Outputs**
  pour voir les noms exacts des buckets créés

#### Réinitialisation en cas de nouvelle session sandbox
Les sandboxes AWS Academy se réinitialisent à chaque session.
Pour recréer l'infrastructure rapidement :
1. Aller dans CloudFormation → supprimer la stack existante si présente
2. Relancer les étapes ci-dessus depuis le fichier `infrastructure.yaml`

## Paramètres configurables

| Paramètre | Valeur par défaut | Description |
|---|---|---|
| `ProjectName` | `datalake` | Préfixe de toutes les ressources |
| `Environment` | `dev` | Environnement de déploiement |
| `GroupName` | `groupe1` | Suffixe pour l'unicité des noms de buckets |

---

## Rôles IAM — `iam-roles.yaml`

> ⚠️ **Note importante — Environnement sandbox AWS Academy**
>
> Ce fichier **ne peut pas être déployé** dans un environnement sandbox AWS Academy car les sandboxes ne permettent pas la création de rôles IAM via CloudFormation (`iam:CreateRole` non autorisé).
>
> En sandbox, le rôle **`LabRole`** fourni automatiquement par AWS Academy est utilisé à la place pour tous les services (Glue, Athena, S3).

### En environnement de production, ces rôles auraient été créés

| Rôle | Utilisé par | Permissions |
|---|---|---|
| `GlueServiceRole` | AWS Glue Crawler + Glue Job | Lecture Raw, écriture Processed/Curated |
| `DataLakeAdminRole` | Architecte (P1) | Accès complet à toute l'infrastructure |
| `DataEngineerRole` | Data Engineers (P2, P3) | S3 Raw/Processed/Curated + Glue complet |
| `DataAnalystRole` | Data Analyst (P4) | Lecture Curated + Athena + QuickSight |

### Principe appliqué — Moindre privilège

Chaque rôle ne dispose que des permissions strictement nécessaires à son périmètre. Par exemple :
- Le **Data Analyst** ne peut pas modifier les données — lecture seule sur Curated
- Le **Data Engineer** n'a pas accès à Athena ou QuickSight
- Le **Glue Service Role** ne peut accéder qu'aux buckets du projet, pas à l'ensemble du compte AWS

Ce découpage reflète les bonnes pratiques de sécurité appliquées en entreprise sur les architectures data lake.

---

*Projet académique — ESGI — Introduction au Cloud AWS*
