# Exo 06 — Docker avancé

## Ce que fait l'exercice

Construire une image optimisée, comparer avec une image anti-pattern, et exécuter une stack multi-services via Docker Compose.

## Commandes d'exécution / test

Depuis `exo` :

```bash
docker build -f app/Dockerfile.bad -t app:bad app
docker build -t app:optimized app
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
docker compose -f 06-docker/docker-compose.yml up -d
docker compose -f 06-docker/docker-compose.yml ps
curl http://localhost/health
curl http://localhost/
docker compose -f 06-docker/docker-compose.yml down -v
```

## Problèmes techniques rencontrés

Pendant l'exercice, j'ai identifié un problème dans le flux multi-stage: une image dite optimisée pouvait réinjecter des dépendances qui ne devaient pas rester en production. Après le renommage des dossiers, j'ai aussi eu une incohérence de chemin dans `docker-compose.yml`, qui pointait encore vers `./app` au lieu de `../app`. Enfin, le premier pull des images a été très long, ce qui ressemblait à un blocage alors que le téléchargement était simplement en cours.

## Correctifs appliqués

J'ai conservé un Dockerfile de production propre, avec utilisateur non-root et dépendances limitées au runtime. J'ai corrigé le `build.context` vers `../app` pour que la stack continue de fonctionner après réorganisation des dossiers. J'ai ensuite validé le résultat par des tests HTTP (`/` et `/health`) et par la comparaison de taille entre l'image non optimisée et l'image optimisée.
