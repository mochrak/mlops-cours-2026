# Journal de bord du binôme

<!--
Mode d'emploi (à garder en tête du fichier) :
- Une entrée par séance (et par session de travail importante hors séance), la plus récente en bas.
- 3 à 5 lignes : qui a piloté, ce qui a été fait, ce qui a bloqué ou ce qui a été appris,
  la prochaine étape. Pas de roman : c'est un outil de pilotage, pas un rapport.
- Titre d'entrée au format exact « ## S<n> — AAAA-MM-JJ — <sujet> » (date réelle) :
  le pré-contrôle de rendu (make check-m10) compte les entrées datées.
- Liez les PR, issues et ADR (#12, docs/adr/0003-...). Les problèmes non résolus restent
  visibles : c'est précisément ce qui aide l'enseignant à vous débloquer.
- L'entrée d'exemple ci-dessous (date AAAA-MM-JJ) n'est pas comptée ; supprimez-la
  quand vous aurez écrit la vôtre.
-->

**Binôme :** [À compléter : @pseudo-github-1 (Prénom Nom), @pseudo-github-2 (Prénom Nom)]
**Repo :** [À compléter : https://github.com/VOTRE_USER/mlops-cours-2026]
**Dataset :** [À compléter : german-credit (fil rouge) ou nom du dataset validé + lien de l'issue]
**Extensions :** [À compléter à la S8 : @pseudo-1 → ext/<nom> · @pseudo-2 → ext/<nom>]

## S0 — AAAA-MM-JJ — Exemple d'entrée (à supprimer)

- **Pilote / copilote :** @alice / @bob
- **Fait :** Lab 1 étapes 1 à 5 ; `dvc push` OK ; premier run MLflow dans l'expérience `scoring` (#3).
- **Bloqué / appris :** `dvc push` refusé (identifiants absents de `.dvc/config.local`, corrigé).
- **Prochaine étape :** Bob pilote le Lab 1 étapes 6-8 ; ouvrir l'ADR 0001 (métrique de promotion).

<!-- Vos entrées commencent ici. Modèle :

## S1 — AAAA-MM-JJ — Lab 0 : setup

- **Pilote / copilote :**
- **Fait :**
- **Bloqué / appris :**
- **Prochaine étape :**

-->

## Lab 1 — Versioning (pilote : Malek, relecteur : mochrak)
- Contrat dataset.yaml (German Credit), données versionnées par une stage DVC `download` (md5 24b8e38b…), remote s3://dvc-store/scoring.
- 24 runs MLflow (dummy, régression logistique, random forest, gradient boosting) ; champion = régression logistique (ROC AUC 0,809, coût métier 108 au seuil 0,318).
- Pipeline DVC download → prepare → train → evaluate ; feature store Feast (request_features, scoring_v1).
- Incident : téléchargement du modèle impossible depuis le poste (URL présignée vers `s3.storage:8333`, nom interne au cluster). Correctif : `127.0.0.1 s3.storage` dans /etc/hosts.
- Incident : `data/processed/.gitignore` non généré par DVC après un `dvc repro` interrompu ; recréé à la main.
