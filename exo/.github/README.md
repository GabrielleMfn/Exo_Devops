# Exo 02 — GitHub Actions

## Ce que fait l'exercice

Mettre en place une CI/CD avec tests, build Docker, scan sécurité, et déploiement simulé, plus un workflow réutilisable.

## Commandes d'exécution / test

Vérification locale du YAML (lecture humaine) :

```bash
cat workflows/ci.yml
cat workflows/reusable-docker.yml
```

Déclenchement réel : push sur `main` ou `develop`.

## Problèmes techniques rencontrés

J'ai rencontré des incohérences de chemins entre `app/...` et `exo/app/...`, surtout après réorganisation des dossiers. J'ai aussi eu un risque de décalage entre le workflow principal et le workflow réutilisable sur les paramètres `context` et `dockerfile`. Enfin, le comportement de `set -o pipefail` dépend du shell utilisé et peut casser un job si l'exécution n'est pas en `bash`.

## Correctifs appliqués

J'ai aligné tous les chemins du workflow CI vers `exo/app` pour éviter les erreurs de contexte. J'ai vérifié le couplage entre le workflow principal et le workflow réutilisable (`uses` + paramètres `with`) pour garantir une construction cohérente. J'ai également conservé les artefacts de tests afin de faciliter l'analyse en cas d'échec d'un job.
